# AWS DEA-C01 Revision Notes: Data Engineering Fundamentals

## 1. Types and properties of data

Exam hook: match the data type to the AWS store and the V to the problem it creates.

| Type | Shape | Examples | Typical AWS store |
| --- | --- | --- | --- |
| Structured | Fixed schema, rows/columns | RDBMS tables, CSV | RDS, Aurora, Redshift |
| Semi-structured | Self-describing tags/keys, flexible schema | JSON, XML, logs | DynamoDB, S3 + Athena, OpenSearch |
| Unstructured | No schema | Images, video, audio, PDFs, free text | S3 (+ Rekognition, Textract, Comprehend) |

**The 3 Vs (5 Vs in some questions)**

- **Volume**: amount of data (GB to PB). Fix: S3, Redshift, Glue/EMR for scale.
- **Velocity**: speed of arrival and processing (batch vs near-real-time vs real-time). Fix: Kinesis Data Streams, Firehose, MSK.
- **Variety**: many formats and sources. Fix: data lake + Glue Catalog + schema-on-read.
- **Veracity**: trustworthiness and quality of data. Fix: validation, Glue Data Quality, profiling.
- **Value**: business usefulness of the data.

**Schema-on-write vs schema-on-read**

- Schema-on-write: structure enforced at load (warehouse, RDBMS). Fast queries, rigid.
- Schema-on-read: structure applied at query time (lake, Athena). Flexible, cheap ingest.

## 2. Warehouse vs lake vs lakehouse, and data mesh

Exam hook: warehouse = curated structured analytics; lake = raw anything; lakehouse = both on one copy of data.

|  | Data warehouse | Data lake | Lakehouse |
| --- | --- | --- | --- |
| Data | Structured, processed | Raw: structured, semi, unstructured | All types, with table layer |
| Schema | On write | On read | On read + enforced table formats |
| Users | BI analysts | Data scientists, engineers | Both |
| Cost | Higher (compute + storage coupled, less so on RA3) | Low (S3) | Low storage |
| AWS | Redshift | S3 + Glue Catalog + Athena | S3 + Iceberg/Hudi/Delta, Redshift Spectrum, Lake Formation |

- **ETL vs ELT**: warehouse classically ETL; lake/lakehouse favour ELT (load raw first, transform later).
- **Lakehouse extras**: ACID transactions, time travel, upserts on S3. On AWS: Apache Iceberg (Athena, Glue, EMR) is the main exam answer.
- **Redshift Spectrum**: query S3 data from Redshift without loading it.
- **Lake Formation**: central permissions (table/column/row level), governance on top of the lake.
- **Glue Data Catalog**: central metadata store (databases, tables, schemas), used by Athena, EMR, Redshift Spectrum.

**Data mesh**

- Decentralised, domain-oriented data ownership (not one central team).
- 4 principles: **domain ownership**, **data as a product**, **self-serve data platform**, **federated computational governance**.
- Each domain publishes its own data products; central team only provides platform and standards.
- AWS fit: Lake Formation + Glue Catalog with cross-account sharing, DataZone for discovery and governance.
- Contrast: data lake/warehouse = centralised; mesh = decentralised.

## 3. ETL pipelines and orchestration

Exam hook: pick the orchestrator by complexity and ecosystem; pick the trigger by event vs schedule.

**ETL pipeline stages**: Extract (source) → Transform (clean, join, aggregate, convert format) → Load (target). ELT swaps the last two.

**AWS Glue (serverless ETL)**

- Spark-based jobs (Python/Scala), Python shell jobs, Ray, streaming ETL.
- **Crawlers** infer schema and populate the Data Catalog.
- **Job bookmarks** track processed data so reruns skip it (incremental loads).
- **Glue Workflows / triggers** chain crawlers and jobs. **Glue Studio / DataBrew** for visual and no-code prep.
- **Glue Data Quality** (DQDL rules) validates data in the pipeline.
- Worker types: G.1X / G.2X, auto scaling, Flex execution for cheaper non-urgent jobs.

**Orchestration options**

| Service | Use when |
| --- | --- |
| Step Functions | Serverless, visual state machine, AWS-native integrations, retries and error handling built in. Default exam answer. |
| MWAA (Managed Airflow) | Already using Airflow, complex DAGs, many dependencies, open-source portability. |
| Glue Workflows | Orchestrating only Glue crawlers and jobs. |
| EventBridge | Event-driven or scheduled triggers (cron/rate); starts pipelines, does not orchestrate steps. |
| Lambda | Light glue logic, short tasks (15 min cap). |
| S3 Event Notifications | Trigger on object create/delete. |

**Pipeline design principles**

- **Idempotent**: rerunning gives same result (no duplicates). Use upserts/MERGE, partition overwrite, bookmarks.
- **Retries with backoff**, dead-letter queues (SQS/SNS) for failures.
- **Monitor**: CloudWatch metrics/logs/alarms, EventBridge for failure events, SNS notifications.
- **Batch vs streaming**: batch = scheduled, high throughput; streaming = Kinesis/MSK, low latency.
- **Decouple** stages with S3, SQS, Kinesis so one failure does not break everything.

## 4. Data sources and file formats

Exam hook: columnar + compressed + partitioned = cheap and fast Athena/Redshift Spectrum queries.

**Common sources and how they connect**

| Source | Ingestion route |
| --- | --- |
| Relational DBs (RDS, on-prem) | DMS (full load + CDC), Glue JDBC connections, zero-ETL to Redshift |
| Streaming / IoT / clickstream | Kinesis Data Streams, Firehose, MSK, IoT Core |
| Logs | CloudWatch Logs → Firehose / S3 |
| SaaS apps (Salesforce, SAP) | Amazon AppFlow |
| APIs, files, flat files | Lambda, Glue, Transfer Family (SFTP), DataSync (bulk on-prem to S3) |
| Large offline transfer | Snowball family |
| NoSQL | DynamoDB Streams, DynamoDB export to S3 |

- **CDC (change data capture)**: capture only inserts/updates/deletes from DB logs. DMS does this.
- **Kinesis Data Streams** = you manage consumers, replay up to 365 days. **Firehose** = fully managed delivery to S3/Redshift/OpenSearch, near-real-time buffering, can convert to Parquet.

**File formats**

| Format | Type | Layout | Best for |
| --- | --- | --- | --- |
| CSV | Text | Row | Simple exchange, human readable; no schema, large, slow |
| JSON | Text | Row | Semi-structured, APIs, logs; verbose |
| Avro | Binary | Row | Streaming (Kafka/MSK), write-heavy, schema evolution, schema embedded |
| Parquet | Binary | Columnar | Analytics (Athena, Redshift Spectrum, EMR); compression + column pruning + predicate pushdown |
| ORC | Binary | Columnar | Hive/Hadoop ecosystem, similar to Parquet |

- Row format = good for write-heavy / read whole record. Columnar = good for read-heavy analytics reading few columns.
- **Compression**: Snappy (fast, default), GZIP (smaller, slower, not splittable), LZO, ZSTD, bzip2 (splittable). Prefer splittable or compressed columnar so Spark can parallelise.
- **Small files problem**: many tiny S3 files slow Athena/Spark. Fix: compact to \~128 MB to 1 GB files (Glue, Firehose buffering, Iceberg compaction).
- **Partitioning**: lay out S3 as `year=2026/month=10/` so queries prune data. Do not over-partition (too many small partitions).

## 5. Data modeling, lineage and schema evolution

Exam hook: star schema for Redshift analytics; lineage answers "where did this come from"; schema changes should not break pipelines.

**Data modeling**

- **Normalised (3NF)**: reduces redundancy, good for OLTP writes (RDS/Aurora).
- **Denormalised**: fewer joins, good for OLAP reads.
- **Star schema**: central **fact table** (measures + foreign keys) surrounded by **dimension tables** (descriptive attributes). Simple, fast joins. Default for warehouses.
- **Snowflake schema**: dimensions further normalised. Less storage, more joins.
- **Fact types**: transactional, periodic snapshot, accumulating snapshot. **Grain** = what one fact row represents; define it first.
- **Slowly changing dimensions (SCD)**: Type 1 overwrite (no history), Type 2 new row with effective dates/flag (full history), Type 3 extra column for previous value.
- **Redshift keys**: DISTKEY (distribution), SORTKEY (ordering), DISTSTYLE (AUTO/EVEN/KEY/ALL); ALL for small dimensions.
- **DynamoDB modeling**: design from access patterns; partition key + sort key; single-table design; GSIs/LSIs.

**Data lineage**

- Tracks origin, transformations and movement of data end to end.
- Uses: debugging, impact analysis, auditing, compliance (GDPR), trust in data.
- AWS: Glue Data Catalog + Glue/Spark lineage, **Amazon DataZone**, **SageMaker Catalog / Unified Studio lineage (OpenLineage)**, Lake Formation for access audit, **CloudTrail** for API activity.
- **Data catalog** = metadata (schemas, owners, tags); **lineage** = flow of data between assets.

**Schema evolution**

- Handling changes in source schema over time (add/drop/rename columns, type change).
- **Backward compatible**: new schema reads old data. **Forward compatible**: old schema reads new data. **Full** = both.
- Safe changes: adding optional/nullable columns with defaults. Risky: renaming, dropping, changing types.
- **Avro** and **Parquet** support evolution (Avro best, schema travels with data); **Iceberg** supports add/drop/rename/reorder columns without rewriting data.
- **Glue Schema Registry** (with MSK/Kinesis/Avro/JSON Schema) enforces compatibility modes and versions schemas.
- **Glue crawlers** can update the Catalog on schema change (update behaviour: update in database / log).
- **Redshift Spectrum / Athena**: schema-on-read, column order and names matter for CSV, names matter for Parquet.

## 6. Performance, sampling, skew, validation and profiling

Exam hook: most performance answers are indexing, partitioning, right keys, caching, and fixing hot partitions.

**Database performance optimization**

- **Indexing**: speeds reads, slows writes, costs storage. Index columns used in WHERE/JOIN/ORDER BY.
- **Partitioning**: split large tables or S3 prefixes by a key (date) so queries scan less.
- **Query tuning**: select only needed columns, filter early, avoid SELECT \*, use EXPLAIN plans.
- **Caching**: ElastiCache (Redis/Memcached), DAX for DynamoDB, Redshift result cache.
- **Scaling reads**: read replicas (RDS/Aurora), Aurora Serverless; vertical vs horizontal scaling.
- **Redshift**: pick DISTKEY/SORTKEY well, `VACUUM` and `ANALYZE`, materialized views, Concurrency Scaling, WLM, compression encodings (AUTO).
- **DynamoDB**: high-cardinality partition keys, on-demand vs provisioned capacity, avoid hot partitions.
- **Athena**: Parquet/ORC, partition pruning, compaction, partition projection.
- **Connection pooling**: RDS Proxy.

**Data sampling techniques**

- **Simple random**: every record equal chance. Quick, can miss small groups.
- **Stratified**: split into groups (strata) and sample each. Keeps representation of rare classes.
- **Systematic**: every k-th record. Easy, risky if data has a periodic pattern.
- **Cluster**: randomly pick whole groups, use all records in them. Cheap, higher variance.
- **Reservoir sampling**: fixed-size sample from a stream of unknown length.
- Watch for **sampling bias**; sample size affects confidence. Used for profiling, testing pipelines, ML training sets.

**Data skew**

- Uneven distribution of data across partitions/nodes: one partition gets most of the work (hot key), causing slow stragglers and hot shards.
- Causes: popular keys, nulls in the key, poorly chosen partition key, time-based keys (all writes to "today").
- **Fixes**:
  - **Salting**: add a random suffix to hot keys to spread them, then aggregate again.
  - Choose a high-cardinality, evenly distributed partition key.
  - Repartition / adaptive query execution in Spark; broadcast join for small tables.
  - Redshift: DISTSTYLE EVEN/ALL or better DISTKEY. DynamoDB: write sharding, adaptive capacity.
  - Kinesis: use more shards or better partition keys; resharding for hot shards.
  - Filter or handle null keys separately.

**Data validation and profiling**

- **Validation** = checking data meets rules: completeness (no missing), consistency, accuracy, uniqueness (no duplicates), validity (type/format/range), timeliness, referential integrity.
- **Profiling** = analysing data to learn its structure, stats and quality: row counts, nulls, distinct values, min/max, distributions, patterns, outliers.
- Profile first to discover issues, then write validation rules.
- **AWS tools**: **Glue Data Quality** (DQDL rules, built on Deequ), **Glue DataBrew** (visual profiling + recipes), Deequ on EMR, Athena queries, Lambda checks. Redshift primary/foreign key constraints are informational only (not enforced); NOT NULL is enforced.
- Handle bad records: quarantine to a separate S3 prefix / DLQ, alert via SNS/CloudWatch, do not silently drop.

## 7. Git review and last-minute cheat sheet

**Git architecture**

- Distributed version control: every clone holds the full history.
- Three areas: **working directory** → (`git add`) **staging area/index** → (`git commit`) **local repository** → (`git push`) **remote repository**.
- **HEAD** = pointer to current commit/branch. **Branch** = movable pointer to a commit.

| Command | What it does |
| --- | --- |
| `git init` / `git clone <url>` | Create repo / copy remote repo |
| `git status` / `git diff` | See changes |
| `git add <file>` | Stage changes |
| `git commit -m "msg"` | Save staged changes locally |
| `git push` / `git pull` | Send to / fetch + merge from remote |
| `git fetch` | Download remote changes without merging |
| `git branch` / `git checkout -b` (or `git switch -c`) | List / create branch |
| `git merge <branch>` | Combine branch into current (can create merge commit) |
| `git rebase <branch>` | Replay commits on top of another branch (linear history) |
| `git log` | View history |
| `git revert <commit>` | New commit that undoes a commit (safe on shared branches) |
| `git reset` | Move HEAD / unstage; `--hard` discards changes (dangerous) |
| `git stash` | Shelve uncommitted work |
| `git cherry-pick <commit>` | Apply one commit onto current branch |

- **Merge conflict**: same lines changed in both branches; fix manually, `git add`, then commit.
- **Pull request**: review step before merging into main.
- **AWS CodeCommit** is closed to new customers (since July 2024); expect GitHub/GitLab/Bitbucket with CodePipeline/CodeBuild. Pipelines as code: **CloudFormation / CDK / SAM / Terraform**.

**Cheat sheet: pick the service**

| Need | Answer |
| --- | --- |
| Serverless SQL on S3 | Athena |
| Petabyte warehouse | Redshift |
| Serverless ETL + catalog | Glue |
| Big-data Spark/Hadoop clusters | EMR |
| Real-time streams, custom consumers | Kinesis Data Streams |
| Stream to S3/Redshift with no code | Firehose |
| Kafka-compatible | MSK |
| DB migration + CDC | DMS |
| SaaS to AWS | AppFlow |
| Lake permissions, column/row security | Lake Formation |
| Orchestrate serverless workflow | Step Functions |
| Orchestrate Airflow DAGs | MWAA |
| Visual no-code data prep | Glue DataBrew |
| Data quality rules | Glue Data Quality |
| Schema compatibility for streams | Glue Schema Registry |
| Best analytics format | Parquet (columnar, compressed, partitioned) |
