## Data Engineering Learning Log

### 1. Unix Epoch Timestamps in JSON [5th Aug 2026]

**What I learned:** A JSON date field showing up as a large number (e.g., `1787011200000`) isn't a broken or unusual field — it's Unix epoch time in milliseconds: 
the count of milliseconds elapsed since January 1, 1970, 00:00:00 UTC.

**Why systems use it:**
- Timezone-unambiguous — the same number means the exact same instant everywhere, unlike a date string which is ambiguous without a timezone attached.
- Cheap to store and compare — comparing two integers is trivial; comparing date strings across formats is error-prone.
- Common in JavaScript-based systems and REST APIs — JS's native `Date.now()` returns milliseconds since epoch, so anything built on a JS-based layer tends to inherit this format.

**How to tell seconds vs. milliseconds by magnitude:**
- Epoch in **seconds** (current era): ~10 digits (e.g., `1787011200`)
- Epoch in **milliseconds** (current era): ~13 digits (e.g., `1787011200000`)

**The conversion — it's just arithmetic, not a special function:**

```sql
SELECT
    TO_DATE(
        DATEADD(
            'millisecond',
            TRY_TO_NUMBER(raw:CADIS_NEXT_RESET_DATE::STRING),
            '1970-01-01'::TIMESTAMP_NTZ
        )
    ) AS CADIS_NEXT_RESET_DATE
FROM raw_table;
```

**Reading it inside-out (the useful skill, not just this query):**
1. `raw:CADIS_NEXT_RESET_DATE::STRING` — extract the value from the JSON column and cast to string. Example value: `"1787011200000"`.
2. `TRY_TO_NUMBER(...)` — convert to a number. `TRY_TO_NUMBER` returns `NULL` on malformed input instead of erroring — safer than `TO_NUMBER` when source data quality isn't guaranteed.
3. `DATEADD('millisecond', <number>, '1970-01-01'::TIMESTAMP_NTZ)` — add that many milliseconds to the epoch start. This *is* the conversion.
4. `TO_DATE(...)` — truncate the resulting timestamp down to just the date, dropping time-of-day.

**Takeaway:** epoch conversion isn't magic — it's "epoch start + N milliseconds," and knowing that means I can reconstruct this in any SQL dialect, not just memorize one Snowflake snippet.

---

### 2. RAISE vs. RETURN in Snowflake Stored Procedures (and why Task suspension silently failed) [5th Aug 2026]

**What I learned:** a stored procedure was catching exceptions and logging them to our exception-report table correctly — but the calling Task, configured with `SUSPEND_TASK_AFTER_NUM_FAILURES = 1`, never suspended. This meant failures kept re-running every minute, generating repeated failure emails, instead of stopping until someone manually investigated and resumed the Task.

**Root cause:** the procedure used `RETURN` with an error message after catching the exception, instead of re-`RAISE`ing it.

```sql
-- WRONG — this "succeeds" from the Task's perspective
CREATE OR REPLACE PROCEDURE sp_example()
RETURNS STRING
LANGUAGE SQL
AS
$$
BEGIN
    BEGIN
        -- business logic that might fail
        INSERT INTO target_table SELECT * FROM source_table;
    EXCEPTION
        WHEN OTHER THEN
            INSERT INTO exception_report (proc_name, error_message, resolved)
            VALUES ('sp_example', :SQLERRM, FALSE);
            RETURN 'FAILED: ' || :SQLERRM;   -- <-- procedure still completes normally
    END;
    RETURN 'SUCCESS';
END;
$$;
```

**Why this breaks Task suspension:** `RETURN` ends the procedure and hands back a value — even if that value is an error string, the procedure call itself completes without error. `SUSPEND_TASK_AFTER_NUM_FAILURES` only watches for an actual unhandled error propagating out of the procedure call. A "successful" return that happens to contain the word "FAILED" in a string is invisible to that mechanism.

**The fix — re-`RAISE` after logging:**
```sql
-- CORRECT — this actually fails the procedure call, which the Task sees
CREATE OR REPLACE PROCEDURE sp_example()
RETURNS STRING
LANGUAGE SQL
AS
$$
BEGIN
    BEGIN
        INSERT INTO target_table SELECT * FROM source_table;
    EXCEPTION
        WHEN OTHER THEN
            INSERT INTO exception_report (proc_name, error_message, resolved)
            VALUES ('sp_example', :SQLERRM, FALSE);
            RAISE;   -- <-- re-raises the original exception, procedure call fails
    END;
    RETURN 'SUCCESS';
END;
$$;
```

**Task definition, for reference:**
```sql
CREATE OR REPLACE TASK task_example
  WAREHOUSE = my_wh
  SCHEDULE = '1 MINUTE'
  SUSPEND_TASK_AFTER_NUM_FAILURES = 1
AS
  CALL sp_example();
```

**Takeaway:** logging an exception and failing loudly are two separate steps — catching an error to record it doesn't automatically communicate "something went wrong" to whatever's calling you. The caller (here, the Task) only reacts to an actual propagated failure, not a value that happens to describe one.
