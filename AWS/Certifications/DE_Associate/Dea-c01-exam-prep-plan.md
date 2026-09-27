# AWS Certified Data Engineer - Associate (DEA-C01) — Prep Plan

**Target:** December 2026 (assuming you start right after your AI Practitioner exam wraps up in mid-Oct — adjust the calendar below once you have an exact December date).

## Exam Facts (official AWS)
- 65 questions, 130 minutes, $150 USD
- Pearson VUE (test center or online proctored)
- Pass score: 720/1000 (scaled)
- Target candidate: 2–3 years of data engineering experience + 1–2 years hands-on AWS — you're well within this bar already

## The 4 Domains (official weights)

| Domain | Weight |
|---|---|
| 1. Data Ingestion and Transformation | 34% |
| 2. Data Store Management | 26% |
| 3. Data Operations and Support | 22% |
| 4. Data Security and Governance | 18% |

## Syllabus by Domain

**Domain 1 — Data Ingestion and Transformation (34%, largest — prioritize)**
- Batch vs. stream ingestion patterns
- Services: AWS Glue (jobs, crawlers, DataBrew), Amazon Kinesis (Data Streams, Firehose, Data Analytics), Amazon MSK (Managed Kafka), AWS DMS (Database Migration Service), AWS Lambda for lightweight transforms, Step Functions for pipeline orchestration
- Transformation approaches: ETL vs. ELT, schema-on-read vs. schema-on-write
- Programming concepts: idempotency, replayability, error handling in pipelines (SQL/Python level knowledge — no language syntax tested directly)

**Domain 2 — Data Store Management (26%)**
- Choosing the right store: S3 (data lake), Redshift (data warehouse), DynamoDB (NoSQL/key-value), RDS/Aurora (relational OLTP), ElastiCache
- Data modeling: partitioning, indexing strategies, file formats (Parquet, ORC, Avro, JSON) and compression
- Schema design and evolution, AWS Glue Data Catalog, Lake Formation for governed data lakes
- Data lifecycle management: S3 lifecycle policies, storage classes, Redshift WLM/RA3 nodes

**Domain 3 — Data Operations and Support (22%)**
- Orchestration: Step Functions, Amazon MWAA (Managed Airflow), EventBridge
- Monitoring and troubleshooting: CloudWatch (metrics, logs, alarms), CloudTrail
- Data quality: AWS Glue Data Quality, validation approaches
- Performance/cost optimization: Redshift query tuning, Glue job bookmarks, right-sizing compute

**Domain 4 — Data Security and Governance (18%, smallest but "trickiest" per most guides — don't skip it)**
- IAM: roles, policies, least privilege for data pipelines
- Encryption: KMS (at rest), TLS (in transit)
- PII/data privacy: Amazon Macie, column-level/row-level security in Lake Formation and Redshift
- Logging and auditability: CloudTrail, Glue job audit logs
- Data governance frameworks and compliance basics (shared with the AI Practitioner content you just covered — good overlap)

**Explicitly out of scope:** ML training/inference, language-specific programming syntax, drawing business conclusions from data.

## Free Resources
- **Official DEA-C01 Exam Guide (PDF)** — download from https://aws.amazon.com/certification/certified-data-engineer-associate/
- **AWS Skill Builder free official practice question set (20 Qs)** — search "AWS Certified Data Engineer Associate Official Practice Question Set DEA-C01" on skillbuilder.aws
- **AWS Skill Builder** — free "Exam Prep" and service-specific digital courses (Glue, Kinesis, Redshift, Lake Formation)
- **AWS documentation** (free) — Glue Developer Guide, Kinesis Developer Guide, Redshift Database Developer Guide, Lake Formation Developer Guide
- **AWS Whitepapers** — "Data Analytics Lens" (Well-Architected), "Building a Modern Data Architecture on AWS" — free PDFs
- **AWS Free Tier account** — you already have hands-on Glue/Kinesis/Redshift-adjacent experience from work; use free tier to specifically touch Glue crawlers, Kinesis Firehose, and Lake Formation permissions since those are less likely to overlap with your current Snowflake-heavy stack
- **r/AWSCertifications DEA megathread** — community-maintained free resource list (search "AWSCertifications DEA resources")

## 8-Week Plan (adjust start date once your Dec exam date is fixed)

| Week | Focus |
|---|---|
| 1–2 | Domain 1 (34%) — Glue, Kinesis, MSK, DMS, Step Functions, batch vs. stream, ETL vs. ELT. This is your biggest scoring domain, so go deep. |
| 3–4 | Domain 2 (26%) — S3/Redshift/DynamoDB/RDS selection criteria, file formats, partitioning, Glue Data Catalog, Lake Formation. Map each AWS store back to a Snowflake equivalent you already know — it'll speed recall. |
| 5 | Domain 3 (22%) — MWAA, EventBridge, CloudWatch, Glue Data Quality, performance tuning. |
| 6 | Domain 4 (18%) — IAM, KMS, Macie, Lake Formation permissions, CloudTrail. Don't shortchange this domain — it's rated as one of the trickier ones despite the low weight. |
| 7 | Full-length practice exams (2–3 attempts). For every miss, go read the AWS doc explaining why the tempting wrong answer was wrong — this exam is scenario-based with close distractors. |
| 8 | Final review of weak domains, book the exam if not already scheduled. |

## Practice Questions

1. When would you choose Kinesis Data Streams over Kinesis Firehose?
2. What's the difference between AWS Glue jobs and Glue crawlers?
3. When would you pick Redshift over Athena for querying data in S3?
4. What's the benefit of using Parquet over JSON for a data lake, and why?
5. What's a Glue job bookmark, and what problem does it solve?
6. When would you choose DynamoDB over RDS for a workload?
7. What does AWS Lake Formation add on top of plain S3 + Glue Data Catalog permissions?
8. What's the difference between ETL and ELT, and when might you prefer each on AWS?
9. How would you orchestrate a multi-step pipeline with retries and error handling — name two AWS services and when you'd pick one over the other.
10. What's the purpose of Amazon MSK, and when would you choose it over Kinesis?
11. How does S3 lifecycle policy help control storage cost over a data's lifetime?
12. What's the difference between row-level and column-level security in a Redshift/Lake Formation context?
13. How would you detect and redact PII in a data pipeline — name the relevant AWS service.
14. What's the role of CloudTrail vs. CloudWatch in a data pipeline's operations?
15. Why might you choose AWS DMS for a migration task instead of writing custom extraction code?
