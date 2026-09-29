# AWS Services — Important for AI Practitioner Certification Notes

Exam-focused notes: what each service is, when to pick it, and the comparisons exams like to test.

## S3 (Simple Storage Service)
- Object storage, not a filesystem — stores objects (data + metadata) in buckets, addressed by key.
- Storage classes (cost vs. retrieval speed tradeoff): Standard, Intelligent-Tiering, Standard-IA, One Zone-IA, Glacier Instant Retrieval, Glacier Flexible Retrieval, Glacier Deep Archive.
- Lifecycle policies automate moving/expiring objects across classes — common exam scenario: "minimize storage cost for infrequently accessed data" → lifecycle policy to IA/Glacier.
- 11 nines durability, 99.99% availability (Standard) — durability and availability are different concepts, exams test this distinction.
- Versioning, encryption (SSE-S3, SSE-KMS, SSE-C), bucket policies vs. IAM policies vs. ACLs (bucket policy = resource-based, attached to bucket; IAM policy = identity-based).
- Backbone of the data lake pattern — paired with Glue Data Catalog + Athena for serverless querying.

## EC2 (Elastic Compute Cloud)
- Resizable virtual servers ("instances"). Exam rarely goes deep here for data/AI certs — know it as the general-purpose compute layer.
- Pricing models: On-Demand (pay as you go), Reserved (1-3yr commit, cheaper), Spot (spare capacity, cheapest, can be interrupted), Savings Plans.
- Instance types matter by workload: compute-optimized (C), memory-optimized (R), storage-optimized (I), general purpose (M/T).
- Auto Scaling Groups + Elastic Load Balancer = the standard resilience/scaling pattern.
- For AI/DE exams: mainly shows up as "what powers this managed service under the hood" or in Well-Architected cost questions, not as a deep-dive topic.

## IAM (Identity and Access Management)
- Controls *who* can do *what* on *which* resource. Foundational to every security question on every AWS exam.
- Users, Groups, Roles, Policies. **Roles** (not users) are the exam's preferred answer for service-to-service or temporary access — no long-lived credentials.
- Policies are JSON documents; identity-based (attached to user/role) vs. resource-based (attached to the resource, e.g., S3 bucket policy).
- Principle of least privilege — almost always the "best practice" answer when a question asks what to do about overly broad permissions.
- MFA, IAM Identity Center (successor to AWS SSO) for federated/multi-account access.

## SNS (Simple Notification Service)
- Pub/sub messaging — one message, many subscribers (fan-out pattern). Push-based.
- Subscribers: SQS queues, Lambda, HTTP/S endpoints, email, SMS.
- Exam pattern: "notify multiple downstream systems of one event" → SNS fan-out to multiple SQS queues.

## SQS (Simple Queue Service)
- Message queuing — decouples producers and consumers. Pull-based (consumer polls).
- Standard queue: at-least-once delivery, best-effort ordering, nearly unlimited throughput.
- FIFO queue: exactly-once processing, strict ordering, lower throughput.
- Visibility timeout: how long a message is hidden from other consumers after being read (prevents duplicate processing while one worker handles it).
- Dead-letter queue (DLQ): captures messages that fail processing repeatedly — common exam answer for "handle poison messages" or "prevent pipeline from stalling on bad data."
- SNS vs. SQS is a classic exam contrast: SNS = push, fan-out, many subscribers; SQS = pull, one message processed by one consumer (per queue).

## Lambda
- Serverless, event-driven compute — runs code without provisioning servers, scales automatically, pay-per-invocation.
- Common triggers: S3 events, API Gateway, DynamoDB Streams, SQS, SNS, EventBridge, Kinesis.
- Max execution timeout: 15 minutes — exam trap: long-running batch jobs need Glue/Step Functions/Batch instead, not Lambda.
- Good for lightweight transforms in a data pipeline; not for heavy multi-hour ETL.
- Cold starts are a known tradeoff vs. always-on compute.

## Step Functions
- Serverless orchestration — coordinates multiple services into a visual workflow (state machine) with built-in error handling, retries, and parallel branches.
- Two workflow types: Standard (long-running, up to 1 year, exactly-once) and Express (short, high-volume, at-least-once, cheaper).
- Exam pattern: "orchestrate a multi-step pipeline across Lambda, Glue, and SNS with retry logic" → Step Functions is the go-to answer over manually chaining Lambdas.

## CloudWatch
- Monitoring and observability: metrics, logs, alarms, dashboards.
- CloudWatch Logs (log aggregation), CloudWatch Metrics (numeric time-series), CloudWatch Alarms (trigger actions/notifications on thresholds), CloudWatch Events/EventBridge (event-driven triggers — EventBridge is the newer, more feature-rich evolution).
- Exam pattern: "detect and alert on a pipeline failure" → CloudWatch Alarm, often paired with SNS for notification.
- Distinct from CloudTrail: CloudWatch = performance/operational monitoring; CloudTrail = API call auditing (who did what, when).

## Bedrock
- Fully managed service for accessing foundation models (Anthropic Claude, Meta Llama, Amazon Nova/Titan, etc.) via a single API — no infrastructure to manage.
- Key features tested: Knowledge Bases (RAG — connects an FM to your own data via a vector store), Agents/AgentCore (multi-step task orchestration with tool use), Guardrails (content filtering, PII redaction, denied topics).
- Contrast with SageMaker: Bedrock = consume pre-trained FMs via API; SageMaker = build/train/deploy custom ML models from scratch.
- Serverless — you don't choose or manage underlying compute.




## Amazon Q family
**Branding note:** AWS has been renaming parts of this family through 2025–2026 (QuickSight → Amazon Quick Suite → Amazon Quick; Q Business closed to new customers and is folding into Amazon Quick as of mid-2026). Current exam guides may still reference the older names below since exam content updates lag product renames — know both the concept and that the branding is in flux.

- **QuickSight (Q in QuickSight / now part of "Amazon Quick"):** AWS's BI/dashboarding tool. The "Q" generative layer lets you ask natural-language questions over your dashboards and get auto-generated visuals/summaries.
- **Q Developer:** AI coding assistant — code suggestions, chat, troubleshooting, integrated into IDEs (VS Code, JetBrains), the AWS Console, and CLI. Includes the Glue ETL-authoring assistant feature.
- **PartyRock:** A free, no-code playground built on Bedrock for experimenting with and building simple generative AI apps — mainly a learning/prototyping tool, not a production service.
- **Q for EC2:** Gives sizing recommendations/guidance for choosing the right EC2 instance type for a new workload.
- **Q for Glue:** The Glue-specific piece of Q Developer — helps author, troubleshoot, and explain Glue ETL jobs/scripts using natural language.
- **Q Business:** Generative AI assistant for internal enterprise use — search, summarize, and generate content across a company's own documents/data sources (being folded into Amazon Quick).

## VPC (Virtual Private Cloud)
- Your own logically isolated network within AWS — subnets, route tables, gateways, security groups, NACLs.
- Public subnet (has route to Internet Gateway) vs. private subnet (no direct internet route, often uses a NAT Gateway for outbound-only access).
- Security Groups (stateful, instance-level) vs. Network ACLs (stateless, subnet-level) — classic exam contrast.
- VPC Endpoints let services like S3/DynamoDB be reached privately without traversing the public internet — common "improve security" exam answer.
- For data pipelines: Lambda/Glue jobs can run inside a VPC to reach private resources (e.g., an RDS database in a private subnet).

# AWS AI Managed Services — Certification Notes

## Rekognition
- Pre-trained computer vision service — image and video analysis: object/scene detection, facial analysis/comparison, text-in-image (OCR-lite), content moderation, celebrity recognition.
- No ML expertise required — API call in, structured labels out.
- Exam pattern: "detect inappropriate content in user-uploaded images" → Rekognition (content moderation feature).

## Polly
- Text-to-speech — converts text into lifelike speech audio.
- Neural TTS vs. standard TTS voices; supports SSML (Speech Synthesis Markup Language — not "Space," that's a common mix-up) for fine control over pauses, emphasis, and pronunciation.
- Pairs conceptually with Transcribe (speech-to-text, the reverse direction) — exams like to pair/contrast these two.

## SageMaker
- End-to-end ML platform: build, train, tune, deploy, and monitor custom models.
- Key sub-features tested: SageMaker Studio (IDE), SageMaker Autopilot (AutoML), SageMaker Clarify (bias detection & explainability), SageMaker Feature Store, SageMaker Model Monitor, SageMaker JumpStart (pre-built models/solutions, including some FM access).
- Exam pattern: "need a custom model trained on proprietary structured data" → SageMaker, not Bedrock.
- Full ML lifecycle: data prep → train → tune → deploy → monitor is native to SageMaker.

## Textract
- Extracts text and structured data from scanned documents (images/PDFs) — goes beyond plain OCR.
- Key API modes:
  - **Raw text detection** — plain "find all the text" (DetectDocumentText).
  - **Forms** — extracts key-value pairs (e.g., "Name: John Doe").
  - **Tables** — extracts tabular data preserving row/column structure.
  - **Queries** — ask natural-language questions against a document ("What is the invoice total?") and get the answer directly, without needing to know the doc's layout in advance.
  - **Layout** — identifies structural elements (titles, headers, paragraphs, lists) to preserve reading order.
- Exam pattern: "automate invoice/claim-form data extraction into structured fields" → Textract (Forms/Queries), often chained with Comprehend for further NLP on the extracted text.

## Comprehend and Comprehend Medical
- **Comprehend**: NLP service for unstructured text — sentiment analysis, key phrase extraction, language detection, and **NER (Named Entity Recognition)** — automatically finds built-in entity types (people, places, organizations, dates).
- **Custom Entity Recognition**: lets you train Comprehend to recognize entity types specific to your business (e.g., product SKUs, internal case IDs) that aren't in the built-in set — requires labeled training data.
- Custom Classification: similarly, trains Comprehend to sort text into your own custom categories.
- **Comprehend Medical**: a specialized version that extracts medical information (medications, dosages, diagnoses, treatments, anatomy) from unstructured clinical text like doctor's notes — HIPAA-eligible service.
- Exam pattern: "extract structured medical info from free-text clinical notes" → Comprehend Medical, not vanilla Comprehend.

## Translate
- Neural machine translation between languages, real-time or batch.
- Supports "Active Custom Translation" to bias output using your own parallel-text examples (e.g., preserve brand terminology).
- Commonly chained after Transcribe (speech→text) and before Polly (text→speech) to build multilingual voice pipelines.

## Kendra
- Intelligent enterprise search — NLP-powered search across unstructured data sources (S3, SharePoint, Confluence, RDS, etc.), returning direct answers, not just a list of links (unlike traditional keyword search).
- Exam pattern: "let employees ask natural-language questions across scattered internal documents" → Kendra. (Note: this use case increasingly overlaps with Amazon Q Business in more recent exam content.)

## Lex
- Builds conversational chatbots/voice bots using the same tech behind Alexa.
- Key terms:
  - **Intent** — the goal a user wants to accomplish (e.g., "BookHotel").
  - **Slot** — a piece of information needed to fulfill the intent (e.g., city, check-in date) — like a required parameter.
  - **Sample utterances** — example phrases used to train the bot to recognize an intent ("I want to book a room," "Reserve a hotel for me").
- Combines ASR (speech-to-text) + NLU (natural language understanding) in one managed service.
- Often paired with Lambda (to fulfill the intent's business logic) and Connect (for contact-center voice bots).

## Transcribe and Transcribe Medical
- **ASR (Automatic Speech Recognition)** — converts speech to text.
- Key features:
  - **Automatic PII redaction** — automatically detects and removes/masks sensitive info (names, SSNs, etc.) from transcripts.
  - **Custom language model** — improves accuracy for domain-specific speech patterns by training on your own text data.
  - **Custom vocabulary** — improves recognition of specific words/phrases (jargon, product names, acronyms) not well handled by the default model — the simpler option vs. a full custom language model.
  - **Toxicity detection** — flags harmful language (harassment, hate speech) in audio content, useful for moderating voice/gaming platforms.
- **Transcribe Medical**: specialized for clinical speech — recognizes medical terminology accurately, HIPAA-eligible.
- Exam pattern: "improve transcription accuracy for a small set of unusual product names" → custom vocabulary (cheaper/faster) rather than a full custom language model (used for broader domain-specific accuracy improvements).

## Personalize
- Managed recommendation-engine service — builds personalized recommendations (products, content) without needing ML expertise, based on the same tech Amazon.com uses.
- **Recipes**: pre-built algorithm templates for specific use cases you choose from rather than build yourself — e.g., User-Personalization (general recommendations), Similar-Items (SIMS), Personalized-Ranking (re-ranks a given list for a user), Trending-Now, Next-Best-Action.
- Needs interaction data (clicks, purchases, views) as input, not just item metadata.

## Mechanical Turk
- Crowdsourcing marketplace — humans complete small tasks ("HITs" — Human Intelligence Tasks) such as labeling images, transcribing audio, or moderating content.
- Commonly used to generate labeled training data for ML models — often paired with SageMaker Ground Truth, which can route low-confidence labels to Mechanical Turk workers.

## Augmented AI (A2I)
- Builds human-review workflows for ML predictions — routes low-confidence predictions to a human reviewer before the result is used.
- Integrates natively with Textract, Rekognition, and custom SageMaker models.
- Exam pattern: "ensure a human checks any Textract extraction below a confidence threshold before it's used downstream" → A2I.

## HealthScribe
- Generates clinical documentation automatically from patient-clinician conversations — effectively an AI medical scribe.
- Combines speech recognition + medical NLP (conceptually built on Transcribe Medical + Comprehend Medical-style capabilities) to produce a structured clinical note summary.
- HIPAA-eligible, healthcare-specific.

## Amazon Hardware for AI (purpose-built ML chips)
### AWS Trainium
- Custom silicon purpose-built for **training** ML models at lower cost than GPU-based instances.
- **Trn1 instances**: the EC2 instance family powered by Trainium chips, optimized for large-scale deep learning training workloads.

### AWS Inferentia
- Custom silicon purpose-built for **inference** (running predictions from an already-trained model) at high throughput and low cost.
- **Inf1 instances**: first-generation Inferentia-powered instances.
- **Inf2 instances**: second-generation, higher performance, better suited for large-model/LLM inference at scale.
- Exam pattern: distinguish Trainium (training) from Inferentia (inference) by task, and both from general-purpose GPU instances (P/G families) which are more flexible but typically costlier.

## Note: AI Services vs. ML Services tier
AWS splits its AI/ML portfolio into two tiers, and exams test this distinction directly:
- **AI Services** (pre-trained, no ML expertise needed — just call the API): everything above except SageMaker — Rekognition, Polly, Textract, Comprehend, Translate, Kendra, Lex, Transcribe, Personalize, plus Bedrock for the generative AI tier specifically.
- **ML Services** (build/train/deploy your own custom models): SageMaker.
Exam trap: "team has no ML expertise" → pick a pre-built AI service, not SageMaker.

## Other AI Services Worth Recognizing (lower exam weight, but appear)
- **Amazon Fraud Detector** — managed fraud-detection ML, no ML expertise needed; trained on your own historical fraud data.
- **Amazon Forecast** — time-series forecasting (demand, inventory, resource planning) as a managed service.
- **Amazon Lookout for Vision** — detects visual defects in manufactured products via computer vision.
- **Amazon Lookout for Metrics** — detects anomalies in business/operational metrics automatically.
- **Amazon Lookout for Equipment** — detects abnormal equipment behavior from sensor data for predictive maintenance.
- **Amazon Monitron** — end-to-end hardware + ML solution for equipment condition monitoring and predictive maintenance.
- **Amazon Panorama** — brings computer vision models to edge devices/on-prem cameras (appliance + SDK), for scenarios needing local/low-latency inference rather than cloud round-trips.
