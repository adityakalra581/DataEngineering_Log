### 1. Data Skew: Repartition vs. Broadcast Join vs. Salting [Aug 5th 2026]

**What I learned:** asked how to handle data skew in an interview, I answered "repartition" — a reasonable instinct, but not the targeted fix. Here's the actual breakdown of when each technique applies.

**The problem — data skew:** in a distributed Spark job, data is split across executors, typically hashed by a key (join key or groupBy key). 
If one key value has disproportionately more rows than others, that partition becomes a bottleneck — most executors finish fast and idle while one executor struggles through an oversized partition.
Total job runtime is gated by the slowest partition, not the average.

**Why `repartition()` alone doesn't fix it:**
```python
# This does NOT fix skew if the skew is in the key itself —
# rows for the heavy key still all hash to the same partition
df_repartitioned = df.repartition(200, "region")
```
`repartition()` redistributes based on the existing key — if `region = "NA"` has 10x the data of every other region, repartitioning still routes all NA rows together, since partitioning is hash-based on the key value itself.

**Fix 1 — Broadcast Join** (for small-table joins, not skew specifically):
```python
from pyspark.sql.functions import broadcast

result = large_df.join(broadcast(small_df), on="key")
```
Sends a full copy of the small table to every executor, avoiding a shuffle entirely. Only works when the small side genuinely fits in executor memory
(Spark's default auto-broadcast threshold is 10MB, configurable).

**Fix 2 — Salting** (for skew specifically, regardless of table size):
```python
from pyspark.sql import functions as F

num_salt_buckets = 10

# Large, skewed table: add a random salt to spread the heavy key
large_salted = large_df.withColumn(
    "salt", (F.rand() * num_salt_buckets).cast("int")
).withColumn(
    "salted_key", F.concat(F.col("region"), F.lit("_"), F.col("salt"))
)

# Small table: explode into one row per salt value so matches still work
small_exploded = small_df.withColumn(
    "salt", F.explode(F.array([F.lit(i) for i in range(num_salt_buckets)]))
).withColumn(
    "salted_key", F.concat(F.col("region"), F.lit("_"), F.col("salt"))
)

result = large_salted.join(small_exploded, on="salted_key")
```
Splits a skewed key into multiple sub-keys, spreading its rows across several partitions instead of one. Both sides of the join need matching salt logic to still find each other.

**The distinction that matters:**
| Technique | Solves | Table size requirement |
|---|---|---|
| Broadcast join | Small-table joins (avoids shuffle) | One side must fit in memory |
| Salting | Skewed key distribution | Works regardless of size — it's about distribution, not size |
| Repartition | General redistribution | Doesn't fix skew if the key itself is unevenly distributed |

**Takeaway:** "handle skew" and "join is slow because one table is small" are two different problems with two different fixes — I was answering the second problem's fix for the first problem's question.

---
