# AWS Lambda and Step Functions: Revision Notes

Updated: 2026-11-03

## 1. Lambda core concepts

Exam hook: Lambda = serverless, event-driven, short-lived compute; you pay per request and per GB-second.

**Basics**

- Runs your code in response to events; no servers to manage. Scales automatically.
- **Handler** = entry point. **Event** = JSON input. **Context** = runtime info (request ID, remaining time).
- Runtimes: Python, Node.js, Java, .NET, Ruby, custom runtime (provided.al2023), or **container image** (up to 10 GB).
- Architectures: x86_64 or **arm64 (Graviton)**, arm64 is cheaper and often faster.
- **Pricing**: requests + duration (GB-seconds, billed per 1 ms). Free tier exists. Memory also sets CPU share.

**Key limits (memorise)**

| Limit | Value |
| --- | --- |
| Max timeout | 15 minutes (default 3 s) |
| Memory | 128 MB to 10,240 MB (CPU scales with memory) |
| Ephemeral storage `/tmp` | 512 MB default, up to 10,240 MB |
| Deployment package (zip) | 50 MB zipped direct upload, 250 MB unzipped (code + layers) |
| Container image | 10 GB |
| Env variables | 4 KB total |
| Sync payload (request/response) | 6 MB each |
| Async payload | 1 MB (older exam material says 256 KB) |
| Layers per function | 5 |
| Account concurrency (default) | 1,000 per Region (can be raised) |

**Concurrency**

- **Concurrency** = number of requests being served at the same time.
- **Reserved concurrency**: guarantees AND caps a function's share of account concurrency. Free to set. Use to protect downstream DBs or to guarantee capacity.
- **Provisioned concurrency**: pre-initialised environments, removes cold starts. Costs extra. Can be scheduled with Application Auto Scaling.
- **Throttling**: over the limit gives HTTP 429 (`TooManyRequestsException`); sync callers must retry, async/stream sources retry automatically.

**Cold start vs warm start**

- Cold start = new execution environment created (download code, start runtime, run init code). Warm = reuse existing environment.
- Reduce: provisioned concurrency, **SnapStart** (Java, also Python/.NET), smaller packages, lighter dependencies, init outside the handler, arm64.
- Put SDK clients, DB connections and config loading **outside the handler** so warm invokes reuse them.

**Versions and aliases**

- **`$LATEST`** = mutable working copy. **Published version** = immutable snapshot with its own ARN.
- **Alias** = named pointer to a version (`prod`, `dev`). Supports **weighted routing** between two versions for canary / blue-green, with CodeDeploy for gradual shifts and auto rollback.
- Point triggers at the alias ARN so you can switch versions without changing the trigger.

## 2. Lambda triggers and invocation models

Exam hook: know WHO calls Lambda (push vs poll) because that decides retry behaviour and error handling.

| Model | Who invokes | Examples | Retries / errors |
| --- | --- | --- | --- |
| **Synchronous** | Caller waits for the response | API Gateway, ALB, CloudFront (Lambda@Edge), Cognito, SDK `RequestResponse` | No automatic retry; caller handles errors |
| **Asynchronous** | Lambda queues the event, returns 202 | S3, SNS, EventBridge, SES, CloudWatch Logs, SDK `Event` | 2 automatic retries (configurable 0 to 2), then **DLQ** or **on-failure destination** |
| **Poll-based (event source mapping)** | Lambda service polls the source and invokes you | SQS, Kinesis, DynamoDB Streams, MSK, self-managed Kafka, MQ | Depends on source (below) |

**What is an event source mapping (ESM)?**

- A Lambda-managed resource that **polls a source for you and invokes your function with batches of records**. No polling code needed; the source never calls Lambda itself.
- Sources: SQS, Kinesis, DynamoDB Streams, MSK / self-managed Kafka, Amazon MQ, DocumentDB.
- Lambda pollers read records, form a **batch**, invoke the function **synchronously**, and handle checkpointing and retries.
- Execution role needs read permission on the source (e.g. `sqs:ReceiveMessage`, `kinesis:GetRecords`).
- **Push vs poll**: S3, SNS, EventBridge push to Lambda (async, no ESM). SQS and streams are polled by Lambda (ESM).
- Key settings: batch size, batching window, filtering, partial batch response, max concurrency (SQS), parallelization factor, bisect on error, max retry attempts, max record age (streams), on-failure destination.
- Exam one-liner: ESM = Lambda polls a queue or stream and is configured on the Lambda side.

**Event source mapping details**

- **SQS (standard/FIFO)**: batch size up to 10,000 (standard), batching window, failed batch returns to queue after visibility timeout. Use **partial batch response** (`ReportBatchItemFailures`) so only failed messages retry. Set queue **visibility timeout >= 6x function timeout**. Use a **DLQ on the queue** (not on Lambda) for poison messages.
- **Kinesis / DynamoDB Streams**: ordered per shard; a failing batch **blocks the shard** until it succeeds or the record expires. Controls: max retry attempts, max record age, **bisect batch on error**, on-failure destination, **parallelization factor** (up to 10 per shard), tumbling windows, partial batch response.
- **Filtering**: event filtering at the mapping drops unwanted events before invoking (saves cost).
- **S3**: use S3 Event Notifications or EventBridge; object-level events (create/delete/restore). Avoid writing output to the same bucket/prefix that triggers the function (infinite loop).
- **EventBridge / EventBridge Scheduler**: cron and rate schedules, event pattern rules.
- **API Gateway / Function URL**: HTTP endpoint (Function URL = built-in HTTPS endpoint, auth NONE or AWS_IAM).

**Async error handling**

- **DLQ** (SQS or SNS) = failed event only. **Destinations** (on success / on failure) = richer, includes request/response context, targets SQS, SNS, Lambda, EventBridge. **Destinations are preferred over DLQ.**
- Settings: maximum event age (up to 6 hours) and retry attempts.
- **Idempotency is mandatory**: events can be delivered more than once (at-least-once). Use idempotency keys, Powertools for AWS Lambda idempotency utility, DynamoDB conditional writes.

**Exam shortcuts**

- File lands in S3 -> process -> Lambda via S3 notification.
- Need ordering + replay of streaming data -> Kinesis with Lambda.
- Decouple and buffer spikes -> SQS in front of Lambda (control with reserved concurrency / max concurrency on the mapping).
- Schedule a job -> EventBridge Scheduler.
- Lambda 15-min limit hit -> move to Glue/ECS/Fargate/EMR or split with Step Functions.

## 3. Layers, packaging, networking, security, performance

Exam hook: layers share dependencies; VPC Lambda needs NAT or endpoints for internet; IAM execution role is the identity.

**Lambda layers**

- A layer is a **zip of libraries, custom runtimes or data** shared across functions (e.g. pandas, shared utility code, boto3 versions).
- Max **5 layers** per function; total unzipped size (function + layers) <= **250 MB**.
- Layers are **versioned and immutable**; functions pin a specific layer version. Update = publish a new version, then update the function.
- Extracted to `/opt` at runtime (Python: `/opt/python`, Node: `/opt/nodejs/node_modules`). Order matters: later layers overwrite earlier files.
- Can be shared across accounts / publicly. Not supported with container-image functions (put dependencies in the image).
- Benefits: smaller deployment packages, reuse, faster deploys. **AWS-managed layers**: AWS SDK for pandas (awswrangler), Powertools, Lambda Insights extension, Parameters and Secrets extension.
- **Lambda extensions** = processes running alongside your function (monitoring, secrets caching); often shipped as layers.

**Packaging**

- Zip (direct <= 50 MB, or via S3) vs container image (<= 10 GB, pushed to ECR). Use images for large ML or custom dependencies.
- IaC: SAM, CDK, CloudFormation, Terraform.

**Configuration**

- **Environment variables**: encrypted at rest with KMS; for secrets use **Secrets Manager / SSM Parameter Store** instead (cache them outside the handler).
- **Timeout** >= expected runtime; for SQS keep it below the queue visibility timeout.
- **Memory tuning**: more memory = more CPU; use **AWS Lambda Power Tuning** to find the cheapest setting.

**Networking**

- Default: Lambda runs in an AWS-managed VPC with internet access, no access to private VPC resources.
- **VPC-enabled Lambda**: attach to subnets + security groups (ENIs via Hyperplane) to reach RDS, ElastiCache, etc. Then it **loses internet access** unless in a private subnet with a **NAT gateway**, or uses **VPC endpoints** (S3 and DynamoDB gateway endpoints, interface endpoints for others).
- Use **RDS Proxy** to pool DB connections from many concurrent Lambdas.

**Security**

- **Execution role** (IAM): what the function can do (least privilege; e.g. `AWSLambdaBasicExecutionRole` for CloudWatch Logs, `AWSLambdaVPCAccessExecutionRole` for VPC).
- **Resource-based policy**: who can invoke the function (S3, SNS, other accounts).
- Code signing, KMS for env vars, Secrets Manager for credentials.

**Storage and state**

- Lambda is stateless; use S3, DynamoDB, EFS (mount for shared/large files, needs VPC), or `/tmp` for scratch.

**Cost and performance tips**

- Right-size memory, use arm64, trim packages, reuse connections, filter events at the source, batch records, avoid long idle waiting (use Step Functions Wait / callbacks instead of sleeping in Lambda).
- Compute Savings Plans cover Lambda.

## 4. Lambda monitoring and troubleshooting

Exam hook: metrics = CloudWatch, logs = CloudWatch Logs, traces = X-Ray, API activity = CloudTrail.

**CloudWatch metrics (automatic)**

| Metric | Meaning |
| --- | --- |
| Invocations | Number of invokes |
| Errors | Invokes that failed (function error) |
| Throttles | Invokes rejected due to concurrency limit |
| Duration | Run time (watch p95/p99, compare to timeout) |
| ConcurrentExecutions | Active environments now |
| IteratorAge | Age of last record in stream (Kinesis/DynamoDB Streams). Rising = consumer falling behind |
| DeadLetterErrors | Failed to send to DLQ |
| AsyncEventsReceived / AsyncEventAge / AsyncEventsDropped | Async queue health |
| ProvisionedConcurrencyUtilization | How much provisioned capacity is used |
| OffsetLag | Lag for Kafka/MSK sources |

**Logs**

- `print`/`console.log` output goes to **CloudWatch Logs** group `/aws/lambda/<function>` (needs execution role permission).
- Every invoke emits `START`, `END`, `REPORT` (duration, billed duration, memory used, init duration = cold start).
- Use **structured JSON logs** + **CloudWatch Logs Insights** to query. **Metric filters** turn log patterns into metrics. **Subscription filters** stream logs to Firehose/Kinesis/Lambda/OpenSearch.
- Set **log retention** (default never expires, costs money). Lambda advanced logging controls: JSON format, log level, custom log group.

**Tracing and deeper insight**

- **AWS X-Ray**: enable **Active tracing** to see end-to-end traces, segments/subsegments, downstream calls (DynamoDB, S3, HTTP) and latency. Needs `xray:PutTraceSegments` permission. Also available in Step Functions.
- **CloudWatch Lambda Insights**: extension (layer) giving system metrics (CPU, memory, disk, network, cold starts).
- **CloudWatch Application Signals** / ServiceLens: service-level view.
- **Powertools for AWS Lambda**: structured logging, tracing, metrics (EMF), idempotency.
- **Embedded Metric Format (EMF)**: emit custom metrics from log lines without API calls.
- **CloudTrail**: who changed / invoked what (management events; data events for Lambda invokes if enabled).
- **AWS Config**: configuration compliance. **Health Dashboard**: service issues.

**Alarms and alerting**

- **CloudWatch alarms** on `Errors`, `Throttles`, `Duration` p99, `IteratorAge`, `ConcurrentExecutions` -> **SNS** (email/Slack/PagerDuty) or EventBridge -> remediation.
- Use **error rate** (metric math: Errors / Invocations) rather than raw counts.
- **CodeDeploy** alarms trigger automatic rollback on alias traffic shifting.

**Troubleshooting map**

| Symptom | Likely cause / fix |
| --- | --- |
| Timeouts | Timeout too low, slow downstream, VPC without NAT. Raise timeout/memory, fix network |
| Throttles (429) | Concurrency limit hit. Raise limit, reserved/provisioned concurrency, add SQS buffer |
| Out of memory | Raise memory, check Lambda Insights |
| Access denied | Execution role or resource policy missing permission |
| Rising IteratorAge | Slow function, hot shard. Raise parallelization factor, more shards, optimise code |
| Duplicate processing | At-least-once delivery. Make handler idempotent |
| Cold start latency | Provisioned concurrency, SnapStart, smaller package |

## 5. Step Functions

Exam hook: serverless workflow orchestrator using state machines written in Amazon States Language (ASL, JSON); visual, with built-in retries, error handling and execution history.

**Core concepts**

- **State machine** = workflow definition. **Execution** = one run. **State** = a step. Input/output passed as JSON between states.
- Authoring: Workflow Studio (drag and drop), ASL JSON, CDK/SAM/CloudFormation.
- Triggers: EventBridge, API Gateway, SDK/CLI, Lambda, S3 (via EventBridge), console, schedules.

**State types**

| State | Purpose |
| --- | --- |
| Task | Do work: call Lambda or an AWS service |
| Choice | If/else branching on input values |
| Parallel | Run branches at the same time |
| Map | Iterate over an array; **Distributed Map** processes millions of S3 objects at high concurrency |
| Wait | Delay for time or until a timestamp |
| Pass | Pass/transform data, no work |
| Succeed / Fail | End the execution with success / error |

**Standard vs Express workflows**

| | Standard | Express |
| --- | --- | --- |
| Max duration | Up to 1 year | 5 minutes |
| Execution semantics | **Exactly-once** | At-least-once (async) / at-most-once (sync) |
| Rate | ~2,000 starts per second | 100,000+ per second |
| History | Full execution history in console + API, 90 days | Only in **CloudWatch Logs** |
| Pricing | Per state transition | Per execution count and duration |
| Best for | Long-running, auditable, human approval, ETL | High-volume, short, event processing, IoT, streaming |

**Service integration patterns (for Task states)**

- **Request Response** (default): call the service, move on immediately after the HTTP response.
- **Run a Job (`.sync`)**: wait until the job completes (Glue, EMR, Batch, ECS, SageMaker, Athena, nested Step Functions).
- **Wait for Callback (`.waitForTaskToken`)**: pause until an external system returns the task token. Used for **human approval** and long async work. Standard only.
- **Optimized integrations** (200+ AWS services direct, e.g. DynamoDB, SNS, SQS, Glue, Athena) mean **no Lambda glue code needed**. Also **HTTP Task** for external APIs.

**Error handling**

- **Retry**: `ErrorEquals`, `IntervalSeconds`, `MaxAttempts`, `BackoffRate`, `JitterStrategy`, `MaxDelaySeconds`.
- **Catch**: on error, route to a fallback state (cleanup, notify, compensate). `ResultPath` keeps original input + adds error info.
- Built-in error names: `States.ALL`, `States.Timeout`, `States.TaskFailed`, `States.Runtime`, `Lambda.ServiceException`, `Lambda.TooManyRequestsException`.
- **TimeoutSeconds** and **HeartbeatSeconds** on tasks detect stuck work.
- **Saga pattern**: Catch -> compensating transactions to undo earlier steps.

**Data flow**

- `InputPath` -> `Parameters` -> `ResultSelector` -> `ResultPath` -> `OutputPath` (JSONPath), or newer **JSONata** (`Arguments`, `Output`, `Assign` variables).
- Payload limit **256 KB** per state input/output. For bigger data pass an **S3 pointer** (Distributed Map reads S3 directly).

**Why Step Functions over chaining Lambdas**

- No Lambda calling Lambda (you pay while it waits, hit 15 min limit, hard to debug).
- Declarative retries, parallelism, branching, waits up to a year, visual debugging, audit trail.

## 6. Tracking and monitoring Lambda pipelines with Step Functions

Exam hook: Step Functions gives per-step visibility that a chain of Lambdas does not. Each Lambda does one job; the state machine owns flow, retries, state and the audit trail.

**Typical data pipeline pattern**

1. **EventBridge / S3 event** starts the state machine (e.g. new file lands).
2. **Lambda (validate)** checks file format and schema.
3. **Choice** state routes: valid -> continue, invalid -> quarantine.
4. **Task `.sync`** runs a Glue job (or EMR/Athena) and waits for completion.
5. **Parallel / Map** fans out work (per partition or per file).
6. **Lambda (load / publish)** writes results, updates a status table (DynamoDB).
7. **Catch** on any state -> **SNS / SQS DLQ** notification and cleanup Lambda. **Succeed** or **Fail** ends the run.

**What Step Functions tracks for you**

- **Execution history** (Standard): every state entered/exited, input, output, duration, retries, error cause. Visible in the console graph (green/red/blue per state) and via `GetExecutionHistory`.
- **Execution status**: `RUNNING`, `SUCCEEDED`, `FAILED`, `TIMED_OUT`, `ABORTED`. Query with `ListExecutions` / `DescribeExecution`.
- **Redrive**: rerun a failed execution **from the point of failure** without redoing earlier steps.
- **Task tokens / heartbeats** track long external work.
- **Execution name and input** can carry a business ID (order ID, file name) for correlation.

**Monitoring options**

| Need | Use |
| --- | --- |
| Metrics (executions started/succeeded/failed/timed out/aborted, throttled, duration) | **CloudWatch metrics** for Step Functions, per state machine |
| Alarm on failures | CloudWatch alarm on `ExecutionsFailed`, `ExecutionsTimedOut`, `ExecutionThrottled` -> SNS |
| React to status change | **EventBridge** event `Step Functions Execution Status Change` -> Lambda / SNS / SQS / Slack |
| Detailed logs (required for Express) | **CloudWatch Logs** logging level: ALL, ERROR, FATAL, OFF; optionally include execution data |
| End-to-end trace across Step Functions + Lambda + other services | **X-Ray tracing** enabled on the state machine and on each Lambda (trace ID propagates automatically) |
| Audit of API calls | **CloudTrail** |
| Per-function health | Lambda metrics, logs, Insights (section 4) |
| Business-level tracking | Write status (`STARTED`, `VALIDATED`, `LOADED`, `FAILED`) per run to a **DynamoDB** table or emit custom metrics via EMF |

**Passing context and correlation**

- The **context object** (`$$.Execution.Id`, `$$.Execution.Name`, `$$.State.Name`, `$$.Execution.StartTime`) can be passed into each Lambda so logs from every step share one **execution ID**. Then query CloudWatch Logs Insights by that ID to see the whole run.
- Log the execution ID and a business key in every Lambda (Powertools structured logging) for easy filtering.

**Design best practices**

- One responsibility per Lambda; keep orchestration logic **out** of Lambda code.
- Let Step Functions do retries/backoff; Lambda should throw clear errors (custom error names) so Catch/Retry can match on them.
- Make Lambdas **idempotent** (Standard retries and redrive can re-run a task).
- Prefer **direct service integrations** (DynamoDB, SNS, Glue, Athena) over Lambda wrappers: less code, less cost.
- Do not pass big payloads; store in S3 and pass the key (256 KB limit).
- Set **timeouts and heartbeats** on every Task so a stuck step cannot hang forever.
- Use **Express** for high-volume short flows (sync from API Gateway), **Standard** for long, auditable ETL.

**Exam cheat sheet**

| Scenario | Answer |
| --- | --- |
| Orchestrate multi-step serverless ETL with retries and audit | Step Functions Standard |
| Run a Glue job and wait for finish | Task with `.sync` integration |
| Human approval step | `.waitForTaskToken` |
| Process millions of S3 objects in parallel | Distributed Map |
| High-volume, <5 min, cheap per run | Express workflow |
| Rerun only from failed step | Redrive execution |
| Alert when a workflow fails | EventBridge status-change rule or CloudWatch alarm -> SNS |
| Trace requests across Lambda + other services | X-Ray active tracing |
| Lambda stream consumer falling behind | `IteratorAge` alarm, parallelization factor, more shards |
| Shared dependencies across many functions | Lambda layer |
| Remove cold starts | Provisioned concurrency / SnapStart |
| Limit Lambda from overwhelming a database | Reserved concurrency, RDS Proxy, SQS buffer |
| Failed async invoke context | On-failure destination (preferred) or DLQ |
| Workflow longer than 15 min | Step Functions (not a single Lambda) |
