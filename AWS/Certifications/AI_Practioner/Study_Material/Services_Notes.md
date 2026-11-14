# AWS Certified AI Practitioner (AIF-C01): Study Guide

Exam and interview prep. Restructured from personal notes; nothing from the original notes was dropped.

**Legend**
- 🆕 **Added**: topic or point missing from the original notes
- ✏️ **Corrected / clarified**: original wording was incomplete, wrong, or contained leftover question text
- ⚠️ **Verify**: AWS branding/availability changes fast; double-check against the official exam guide

---

## Table of Contents
1. [Exam at a glance](#1-exam-at-a-glance)
2. [Scenario → Service cheat sheet](#2-scenario--service-cheat-sheet)
3. [AI / ML / GenAI fundamentals](#3-ai--ml--genai-fundamentals)
4. [Amazon Bedrock & GenAI on AWS](#4-amazon-bedrock--genai-on-aws)
5. [RAG & vector databases](#5-rag--vector-databases)
6. [Amazon Q family](#6-amazon-q-family)
7. [Amazon SageMaker](#7-amazon-sagemaker)
8. [AWS AI managed services](#8-aws-ai-managed-services)
9. [AI hardware (Trainium / Inferentia)](#9-ai-hardware)
10. [Responsible AI](#10-responsible-ai)
11. [Security, compliance & governance services](#11-security-compliance--governance-services)
12. [Core AWS services](#12-core-aws-services)
13. [Classic comparisons](#13-classic-comparisons)
14. [Exam traps](#14-exam-traps)
15. [Interview rapid-fire Q&A](#15-interview-rapid-fire-qa)
16. [Last-day checklist](#16-last-day-checklist)

---

## 1. Exam at a glance
🆕 ⚠️ *Confirm against the current official exam guide.*

| Item | Detail |
|---|---|
| Exam code | AIF-C01 |
| Format | ~65 questions (some unscored), ~90 minutes |
| Question types | Multiple choice, multiple response, ordering, matching, case study |
| Passing score | 700 / 1000 (scaled) |
| Level | Foundational. No coding. Know *what a service does and when to pick it* |

**Domains and approximate weights**

| Domain | Weight |
|---|---|
| 1. Fundamentals of AI and ML | ~20% |
| 2. Fundamentals of Generative AI | ~24% |
| 3. Applications of Foundation Models | ~28% |
| 4. Guidelines for Responsible AI | ~14% |
| 5. Security, Compliance, and Governance for AI Solutions | ~14% |

**Strategy:** the exam is mostly "scenario → pick the right service". Learn the *one-line purpose* of each service and the *contrast* with its look-alikes (Section 13).

---

## 2. Scenario → Service cheat sheet

| If the question says… | Answer |
|---|---|
| Consume pre-trained foundation models via API, serverless | **Bedrock** |
| Build/train/deploy a **custom** ML model on proprietary data | **SageMaker** |
| Team has **no ML expertise** | Pre-built AI service (not SageMaker) |
| Detect inappropriate content in images/video | **Rekognition** (content moderation) |
| Text → speech | **Polly** |
| Speech → text | **Transcribe** |
| Extract fields/tables/answers from scanned docs or invoices | **Textract** (Forms / Tables / Queries) |
| Sentiment, key phrases, entities, language detection | **Comprehend** |
| Extract meds/dosages/diagnoses from clinical notes | **Comprehend Medical** |
| Translate text between languages | **Translate** |
| Employees ask NL questions over internal documents | **Kendra** / **Amazon Q Business** / **Quick Index** |
| Chatbot / voice bot (intents, slots, utterances) | **Lex** |
| Personalized recommendations | **Personalize** |
| Human reviews low-confidence ML predictions | **Augmented AI (A2I)** |
| Crowd labeling of data | **Mechanical Turk** / **SageMaker Ground Truth** |
| Auto-generate clinical notes from doctor-patient talk | **HealthScribe** |
| Detect bias / explain model predictions | **SageMaker Clarify** |
| Filter harmful content / deny topics / redact PII in a GenAI app | **Bedrock Guardrails** |
| Ground an FM in private data (RAG) with minimal code | **Knowledge Bases for Bedrock** |
| Hybrid keyword + semantic vector search at scale | **OpenSearch** |
| Data already in PostgreSQL/Aurora → vectors | **pgvector** |
| Graph relationships + RAG (GraphRAG) | **Neptune Analytics** |
| Find PII in S3 | **Macie** |
| Track resource config changes / compliance against rules | **Config** |
| Find software vulnerabilities (EC2, ECR images, Lambda) | **Inspector** |
| Who made which API call? Resource deleted? | **CloudTrail** |
| Download compliance reports (SOC, PCI, ISO) / sign BAA | **Artifact** |
| Continuous audit prep, prebuilt frameworks (GDPR, HIPAA, SOC 2) | **Audit Manager** |
| Best-practice checks (cost, performance, security, fault tolerance) | **Trusted Advisor** |
| Enforce org-wide limits (e.g., only approved Bedrock models) | **SCPs** (AWS Organizations) |
| Private access to Bedrock/S3 without public internet | **VPC endpoint / PrivateLink** |
| Monitoring, metrics, alarms | **CloudWatch** |
| Orchestrate multi-step workflow with retries | **Step Functions** |
| Fan-out one event to many subscribers | **SNS → multiple SQS** |
| Handle poison messages | **SQS Dead-Letter Queue** |
| Cheapest training compute for ML | **Trainium (Trn1)** |
| Cheapest high-throughput inference | **Inferentia (Inf1/Inf2)** |
| AI coding assistant | **Q Developer** |
| Free no-code GenAI playground | **PartyRock** |

---

## 3. AI / ML / GenAI fundamentals
🆕 *This whole section is new. It is Domains 1–2 (~44% of the exam) and was missing from the original notes.*

### 3.1 The hierarchy
- **AI**: machines doing tasks that normally need human intelligence.
- **ML**: subset of AI; learns patterns from data instead of explicit rules.
- **Deep learning**: subset of ML using multi-layer neural networks.
- **Generative AI**: deep-learning models (foundation models / LLMs) that *generate* new content (text, images, audio, code).

### 3.2 ML learning types
| Type | Data | Examples |
|---|---|---|
| **Supervised** | Labeled | Classification (spam / not spam), regression (price prediction) |
| **Unsupervised** | Unlabeled | Clustering, anomaly detection, dimensionality reduction |
| **Semi-supervised** | Few labels + many unlabeled | Labeling cost reduction |
| **Self-supervised** | Labels derived from the data itself | How LLMs are pre-trained (predict next token) |
| **Reinforcement learning** | Reward/penalty from environment | Game agents, robotics; **RLHF** for aligning LLMs |

### 3.3 Data types
Labeled vs. unlabeled · structured (tables) vs. unstructured (text, image, audio) · tabular, time-series, text, image.

### 3.4 ML lifecycle
Business problem → data collection → data prep / feature engineering → train → tune hyperparameters → evaluate → deploy → monitor (drift) → retrain.
*(SageMaker covers each stage; see Section 7.)*

### 3.5 Training concepts
- **Overfitting**: great on training data, poor on new data (high variance). **Underfitting**: poor on both (high bias).
- **Bias vs. variance tradeoff**.
- **Training / validation / test** split.
- **Hyperparameters** (set before training) vs. **parameters/weights** (learned).
- **Inference types**: real-time, batch, asynchronous, serverless.
- **Model drift / data drift**: performance degrades as real-world data changes.

### 3.6 Evaluation metrics
| Task | Metrics |
|---|---|
| Classification | Accuracy, **Precision** (TP/(TP+FP)), **Recall** (TP/(TP+FN)), **F1** (balance of both), **AUC-ROC**, confusion matrix |
| Regression | MAE, RMSE, R² |
| Text generation / summarization | **ROUGE** (summarization), **BLEU** (translation), **BERTScore** (semantic similarity), perplexity |

Precision matters when false positives are costly; recall matters when false negatives are costly (e.g., disease screening).

### 3.7 Business metrics for AI
Cost per user, development cost, customer feedback, ROI, efficiency/productivity gains, conversion rate, average revenue per user.

### 3.8 Generative AI core concepts
- **Foundation model (FM)**: large model pre-trained on broad data, adaptable to many tasks. **LLM** = FM focused on language.
- **Transformer**: architecture behind modern LLMs; **attention** lets the model weigh which tokens matter.
- **Token**: unit of text the model processes (word/sub-word). Pricing and context limits are token-based.
- **Embedding**: numeric vector capturing semantic meaning; similar meaning = nearby vectors. Basis of semantic search/RAG.
- **Context window**: max tokens (input + output) the model can consider.
- **Multimodal models**: handle text + images (+ audio/video).
- **Diffusion models**: generate images by iteratively denoising.
- **Hallucination**: confident but incorrect/fabricated output. Mitigate with RAG, Guardrails, good prompts, human review, low temperature.
- **Non-determinism**: same prompt can yield different outputs.

### 3.9 Inference parameters (Bedrock/LLMs)
| Parameter | Effect |
|---|---|
| **Temperature** | Higher = more random/creative; lower = more deterministic/focused |
| **Top-p** (nucleus) | Sample from smallest set of tokens whose cumulative probability ≥ p |
| **Top-k** | Sample only from the k most likely tokens |
| **Max tokens / response length** | Caps output length (cost + latency control) |
| **Stop sequences** | Strings that make the model stop generating |

### 3.10 Prompt engineering
- **Zero-shot**: instruction only, no examples. **One-shot / few-shot**: include 1 / several examples.
- **Chain-of-thought (CoT)**: ask the model to reason step by step.
- **Prompt template**: reusable structure with placeholders.
- **Negative prompts**: say what *not* to produce.
- Best practices: be specific, give context, define format/length, use examples, set a persona/role, iterate.
- **Prompt misuse / attacks**: **prompt injection**, **jailbreaking**, **prompt hijacking**, **poisoning** (malicious data in prompts/training data), **exposure** (leaking sensitive data), **prompt leaking**. Mitigate with Guardrails, input validation, least privilege, monitoring.

### 3.11 Customizing a foundation model (cheapest → most expensive)
| Approach | What changes | Notes |
|---|---|---|
| **Prompt engineering** | Prompt only | Cheapest, fastest, no training |
| **RAG** | Prompt (adds retrieved context) | Current/private data, no retraining |
| **Fine-tuning** (instruction-based) | Model weights | Needs **labeled** prompt/response data; task/style specialization |
| **Continued pre-training** | Model weights | Uses **unlabeled** domain data (e.g., legal/medical corpus) |
| **Train from scratch** | Everything | Most costly; rarely right answer |

Also: **model distillation** (smaller "student" model learns from a larger "teacher"), **transfer learning**, **RLHF** (human feedback to align outputs).
Data prep for fine-tuning: curated, representative, high-quality, labeled, governed.

### 3.12 Foundation model lifecycle
Data selection → model selection → pre-training → fine-tuning → evaluation → deployment → feedback/monitoring.

### 3.13 Choosing a foundation model
Modality · cost · latency · context window · multilingual support · model size/performance · customization options · licensing/compliance · accuracy on your task.

### 3.14 Evaluating FM output
- **Automatic** evaluation (benchmark datasets, metrics like ROUGE/BLEU/BERTScore).
- **Human** evaluation (quality, tone, brand voice).
- **LLM-as-a-judge** (model grades another model's output).
- Bedrock has built-in **Model Evaluation** (automatic + human).

---

## 4. Amazon Bedrock & GenAI on AWS

### 4.1 What it is
- Fully managed service for accessing foundation models (Anthropic Claude, Meta Llama, Amazon Nova/Titan, etc.) via a **single API**. No infrastructure to manage.
- **Serverless**: you don't choose or manage underlying compute.
- **Bedrock vs. SageMaker**: Bedrock = *consume* pre-trained FMs via API; SageMaker = *build/train/deploy* custom ML models from scratch.

### 4.2 Key features tested
- **Knowledge Bases** (RAG: connects an FM to your own data via a vector store).
- **Agents / AgentCore** (multi-step task orchestration with tool use).
- **Guardrails** (content filtering, PII redaction, denied topics).
- 🆕 **Model customization**: fine-tuning and continued pre-training (custom models need **Provisioned Throughput** to serve).
- 🆕 **Model evaluation**, **Playgrounds** (try models in console), **Converse API**.
- 🆕 **Pricing modes**: **On-Demand** (pay per token), **Batch** (discounted, non-urgent), **Provisioned Throughput** (committed capacity; required for custom models; predictable performance).
- 🆕 **Agents internals**: *action groups* (typically Lambda functions the agent can call) + knowledge bases; the agent plans steps and calls tools.

### 4.3 AWS services used *with* Amazon Bedrock
**1. IAM with Bedrock**
- Implement **identity verification** and **resource-level access control**.
- Define **roles and permissions** to control access to Bedrock resources.
- Example: grant data scientists access to specific Bedrock resources based on their responsibilities.

**2. Guardrails for Bedrock**
- Restrict **specific topics** in generative AI applications.
- Filter **harmful or inappropriate content**.
- Help ensure compliance with **organizational safety policies** by analyzing user inputs and model outputs.
- 🆕 Also: PII redaction/masking, word filters, contextual grounding checks (reduce hallucination), `ApplyGuardrail` API works even with non-Bedrock models.

**3. CloudTrail with Bedrock**
- Monitor and analyze **API calls** made to Bedrock.
- Track **who** performed an action, **what** action was performed, and **when**.
- Use audit logs to support **security monitoring and compliance**.

**4. AWS Config with Bedrock**
- Monitor and record **configuration changes** to AWS resources.
- Evaluate resource configurations against **organizational compliance requirements**.
- Help identify configuration changes that may introduce **security or compliance risks**.

**5. AWS PrivateLink with Bedrock**
- Establish **private connectivity** to Bedrock through VPC endpoints.
- Keep API traffic between your VPC and Bedrock on the **AWS private network** rather than the public internet.
- Improve **network isolation and security**.

🆕 **6. Also relevant:** **CloudWatch** (invocation metrics/logs), **KMS** (encryption of custom models/data), **SCPs** (restrict which models accounts may invoke, see Section 11.9).

---

## 5. RAG & vector databases

### 5.1 RAG pipeline (know the order)
1. **Ingest**: load documents (S3, web, Confluence, SharePoint, etc.).
2. **Chunk**: split the text (fixed-size, hierarchical, semantic, or no chunking).
3. **Embed**: convert chunks to vectors with an embedding model (Amazon Titan Text Embeddings, Cohere Embed).
4. **Store**: save vectors and metadata in a vector database.
5. **Query**: embed the user question with the *same* embedding model.
6. **Retrieve**: similarity search (cosine, dot product, or Euclidean), return top-k chunks.
7. **Augment and generate**: add chunks to the prompt; the LLM answers using them.

**Why RAG:** grounds answers in current, private data without retraining, reduces hallucinations, lets you cite sources. Cheaper and faster than fine-tuning for knowledge updates.

**Knowledge Bases for Amazon Bedrock** is the managed version. It automates steps 1–4 and offers `Retrieve` and `RetrieveAndGenerate` APIs.

### 5.2 AWS vector store options

| Service | Key points | Pick it when… |
|---|---|---|
| **Amazon OpenSearch Service / Serverless** | k-NN vector search, hybrid search (keyword + vector), scales well. Serverless is the default "quick create" option in Bedrock Knowledge Bases. | You need a general-purpose, scalable vector store or hybrid search. |
| **Aurora PostgreSQL / RDS PostgreSQL (pgvector)** | Vectors stored in relational tables alongside SQL data. | Your data is already in PostgreSQL and you want SQL + vectors together. |
| **Amazon Neptune Analytics** | Graph database with vector search, used for GraphRAG. | Relationships between entities matter. |
| **Amazon DocumentDB** | Vector search over JSON documents, MongoDB-compatible. | Your data is already in DocumentDB. |
| **Amazon MemoryDB** | In-memory vector search with very low latency. | You need real-time, ultra-fast retrieval. |
| **Amazon S3 Vectors** | Native vector storage in S3, low cost (newer; ⚠️ verify current status). | Large vector sets and cost matters more than latency. |
| **Third-party (Pinecone, Redis Enterprise Cloud, MongoDB Atlas)** | Supported by Bedrock Knowledge Bases. | You already use these tools. |

**Related, but not a vector DB:** **Amazon Kendra** is managed intelligent enterprise search with ML-based semantic retrieval. It can serve as the retriever in a RAG app.

### 5.3 Exam tips
- "Already uses PostgreSQL / Aurora" → **pgvector**.
- "Fully managed RAG with minimal code" → **Knowledge Bases for Bedrock**.
- "Hybrid keyword + semantic search at scale" → **OpenSearch**.
- "Graph relationships and RAG" → **Neptune Analytics**.
- Use the **same embedding model** for indexing and querying.
- Vector stores are not for tabular analytics. Athena, CloudFront and Route 53 are not vector stores.
- RAG changes the **prompt**, not the model. Fine-tuning changes the model.
- 🆕 Embeddings capture *meaning*; similar text → nearby vectors. Chunk size affects retrieval quality.

---

## 6. Amazon Q family
**Branding note:** AWS has been renaming parts of this family through 2025–2026 (QuickSight → Amazon Quick Suite → Amazon Quick; Q Business closed to new customers and is folding into Amazon Quick as of mid-2026). Current exam guides may still reference the older names below since exam content updates lag product renames. Know both the concept and that the branding is in flux. ⚠️

| Product | What it is |
|---|---|
| **QuickSight (Q in QuickSight / now part of "Amazon Quick")** | AWS's BI/dashboarding tool. The "Q" generative layer lets you ask natural-language questions over dashboards and get auto-generated visuals/summaries. |
| **Q Developer** | AI coding assistant: code suggestions, chat, troubleshooting; integrated into IDEs (VS Code, JetBrains), the AWS Console, and CLI. Includes the Glue ETL-authoring assistant feature. |
| **PartyRock** | Free, no-code playground built on Bedrock for experimenting with and building simple generative AI apps. Learning/prototyping tool, **not production**. |
| **Q for EC2** | Sizing recommendations/guidance for choosing the right EC2 instance type for a new workload. |
| **Q for Glue** | Glue-specific piece of Q Developer; helps author, troubleshoot, and explain Glue ETL jobs/scripts in natural language. |
| **Q Business** | Generative AI assistant for internal enterprise use: search, summarize, and generate content across a company's own documents/data sources (being folded into Amazon Quick). |
| **Quick Index** | Fully managed system that indexes information from sources like Amazon S3 and SharePoint, so employees can ask natural-language questions and get grounded answers from that indexed data (use case: accessing internal documents). |
| **Quick Research** | Synthesizes internal data with online information to produce comprehensive reports **with citations** (use case: a marketing manager requesting a research report on competitors). |

---

## 7. Amazon SageMaker
- End-to-end ML platform: build, train, tune, deploy, and monitor **custom** models.
- Exam pattern: "need a custom model trained on proprietary structured data" → SageMaker, not Bedrock.
- Full ML lifecycle (data prep → train → tune → deploy → monitor) is native to SageMaker.
- **Use cases:** custom churn-prediction model on your own customer data; fraud model tuned to your transaction patterns; data science team needing full control over algorithm choice and hyperparameters.

### 7.1 SageMaker AI feature list
| Feature | Purpose |
|---|---|
| **SageMaker** | End-to-end ML service |
| **Automatic Model Tuning** | Tune hyperparameters |
| **Deployment & Inference** | Real-time, serverless, batch, async |
| **Studio** | Unified interface / IDE for SageMaker |
| **Data Wrangler** | Explore and prepare datasets, create features |
| **Feature Store** | Store feature metadata/features in a central place |
| **Clarify** | Compare models, explain model outputs, **detect bias** |
| **Ground Truth** | RLHF; humans for model grading and **data labeling** |
| **Model Cards** | ML model documentation (ex: model's purpose, responsible owners, and known limitations. |
| **Model Dashboard** | View all your models in one place |
| **Model Monitor** | Monitoring and alerts for your model (drift/quality) |
| **Model Registry** | Centralized repository to manage ML model versions |
| **Pipelines** | CI/CD for machine learning |
| **Role Manager** | Access control |
| **JumpStart** | ML model hub and pre-built ML solutions (includes some FM access) |
| **Canvas** | No-code interface for SageMaker |
| **MLflow on SageMaker** | Use MLflow tracking servers on AWS |
| **Autopilot** | AutoML |

---

## 8. AWS AI managed services

### 8.1 Tiers: AI Services vs. ML Services
- **AI Services** (pre-trained, no ML expertise, just call the API): Rekognition, Polly, Textract, Comprehend, Translate, Kendra, Lex, Transcribe, Personalize, plus **Bedrock** for the generative AI tier.
- **ML Services** (build/train/deploy your own custom models): **SageMaker**.
- **Exam trap:** "team has no ML expertise" → pick a pre-built AI service, not SageMaker.

### 8.2 Vision / documents / language / speech

#### Rekognition (vision)
- Pre-trained computer vision: image and video analysis: object/scene detection, facial analysis/comparison, text-in-image (OCR-lite), content moderation, celebrity recognition.
- No ML expertise required: API call in, structured labels out.
- Pattern: "detect inappropriate content in user-uploaded images" → Rekognition content moderation.
- **Use cases:** flagging NSFW/violent images on a social platform; verifying identity via facial comparison at a kiosk; counting/tracking people or objects in retail video; searching video archives for a face or object.

#### Textract (documents)
- Extracts text and structured data from scanned documents (images/PDFs). Goes beyond plain OCR.
- API modes:
  - **Raw text detection**: plain "find all the text" (DetectDocumentText).
  - **Forms**: key-value pairs (e.g., "Name: John Doe").
  - **Tables**: tabular data preserving row/column structure.
  - **Queries**: natural-language questions against a document ("What is the invoice total?") without knowing the layout in advance.
  - **Layout**: structural elements (titles, headers, paragraphs, lists) preserving reading order.
- Pattern: "automate invoice/claim-form data extraction into structured fields" → Textract (Forms/Queries), often chained with Comprehend for NLP on the extracted text.
- **Use cases:** invoice/receipt processing for accounts payable; insurance claim form fields; digitizing scanned tax docs or ID cards; line-item tables from scanned financial statements.

#### Comprehend and Comprehend Medical (NLP)
- **Comprehend**: NLP for unstructured text: sentiment analysis, key phrase extraction, language detection, and **NER (Named Entity Recognition)**: finds built-in entity types (people, places, organizations, dates).
- **Custom Entity Recognition**: train Comprehend to recognize business-specific entities (product SKUs, internal case IDs); requires labeled training data.
- **Custom Classification**: trains Comprehend to sort text into your own categories.
- **Comprehend Medical**: extracts medical info (medications, dosages, diagnoses, treatments, anatomy) from unstructured clinical text like doctor's notes. **HIPAA-eligible**.
- Pattern: "extract structured medical info from free-text clinical notes" → Comprehend Medical, not vanilla Comprehend.
- **Use cases:** review sentiment at scale; routing support tickets by topic; redacting PII from free-text logs before storage; medication/dosage from physician notes.

#### Translate
- Neural machine translation between languages, real-time or batch.
- **Active Custom Translation**: bias output using your own parallel-text examples (e.g., preserve brand terminology).
- Commonly chained **Transcribe (speech→text) → Translate → Polly (text→speech)** for multilingual voice pipelines.
- **Use cases:** localizing a website/app UI; real-time support-chat translation; batch-translating a document archive; multilingual voice translation.

#### Transcribe and Transcribe Medical (speech → text)
- **ASR (Automatic Speech Recognition)**: converts speech to text.
- Key features:
  - **Automatic PII redaction**: detects and removes/masks sensitive info (names, SSNs, etc.).
  - **Custom language model**: improves accuracy for domain-specific speech by training on your own text data.
  - **Custom vocabulary**: improves recognition of specific words/phrases (jargon, product names, acronyms). Simpler option vs. a full custom language model.
  - **Toxicity detection**: flags harmful language (harassment, hate speech) in audio; useful for moderating voice/gaming platforms.
- **Transcribe Medical**: clinical speech, accurate medical terminology, HIPAA-eligible.
- Pattern: "improve accuracy for a small set of unusual product names" → **custom vocabulary** (cheaper/faster) rather than a custom language model (broader domain-specific accuracy).
- **Use cases:** searchable transcripts/subtitles for meetings/videos; call-center transcription for QA/analytics; live webinar captioning; PII redaction in call transcripts before storage.

#### Polly (text → speech)
- Text-to-speech: lifelike speech audio.
- **Neural TTS** vs. standard voices; supports **SSML** (Speech Synthesis Markup Language, *not* "Space"; common mix-up) for pauses, emphasis, pronunciation.
- Pairs with Transcribe (the reverse direction); exams like to contrast these two.
- **Use cases:** reading news aloud in an app; IVR/phone-system voice prompts; narrating e-learning; accessibility for visually impaired users.

#### Lex (chatbots)
- Builds conversational chatbots/voice bots using the same tech behind Alexa.
- Key terms:
  - **Intent**: the goal a user wants to accomplish (e.g., "BookHotel").
  - **Slot**: a piece of information needed to fulfill the intent (city, check-in date); like a required parameter.
  - **Sample utterances**: example phrases that train the bot to recognize an intent ("I want to book a room," "Reserve a hotel for me").
- Combines **ASR + NLU** in one managed service.
- Often paired with **Lambda** (fulfillment logic) and **Amazon Connect** (contact-center voice bots).
- **Use cases:** website customer-service chatbot; voice IVR for a call center (with Connect); internal Slack bot for booking rooms/order status.

#### Kendra (enterprise search)
- Intelligent enterprise search: NLP-powered search across unstructured sources (S3, SharePoint, Confluence, RDS, etc.), returning **direct answers**, not just links.
- Pattern: "let employees ask natural-language questions across scattered internal documents" → Kendra. (This use case increasingly overlaps with Amazon Q Business in newer exam content.)
- **Use cases:** internal help-desk search over HR policies/wikis; customer-facing FAQ search; one search box across SharePoint + S3 + a database.

### 8.3 Recommendations, human-in-the-loop, healthcare

#### Personalize
- Managed recommendation engine (same tech as Amazon.com); no ML expertise needed.
- **Recipes**: pre-built algorithm templates: *User-Personalization* (general recs), *Similar-Items* (SIMS), *Personalized-Ranking* (re-ranks a list for a user), *Trending-Now*, *Next-Best-Action*.
- Needs **interaction data** (clicks, purchases, views), not just item metadata.
- **Use cases:** "recommended for you" carousels; personalized content ranking on streaming/media; personalized marketing email content.

#### Mechanical Turk
- Crowdsourcing marketplace: humans complete small tasks (**HITs**, Human Intelligence Tasks): labeling images, transcribing audio, moderating content.
- Commonly used to generate labeled training data; often paired with **SageMaker Ground Truth**, which can route low-confidence labels to MTurk workers.
- **Use cases:** bulk image labeling; human transcription/verification of short clips; large-scale data validation/surveys.

#### Augmented AI (A2I)
- Human-review workflows for ML predictions: routes **low-confidence** predictions to a human reviewer before use.
- Integrates natively with **Textract, Rekognition, and custom SageMaker models**.
- Pattern: "ensure a human checks any Textract extraction below a confidence threshold" → A2I.
- **Use cases:** human review of low-confidence document extraction before it hits a financial system; moderator double-checking borderline Rekognition flags; compliance sign-off on automated loan decisions.

#### HealthScribe
- Generates clinical documentation automatically from patient-clinician conversations (AI medical scribe).
- Combines speech recognition + medical NLP (conceptually Transcribe Medical + Comprehend Medical-style capabilities) → structured clinical note summary.
- HIPAA-eligible, healthcare-specific.
- **Use cases:** auto-generating visit summaries; cutting clinician time on manual notes/EHR entry.

### 8.4 Other AI services worth recognizing (lower weight, but appear)
⚠️ AWS has been closing several of these to new customers / retiring them. The exam may still reference them. Know the concept; verify current status.

| Service | Purpose | Use cases |
|---|---|---|
| **Amazon Fraud Detector** | Managed fraud detection ML, no ML expertise; trained on your own historical fraud data | Suspicious online payments; fake account sign-ups/promo abuse |
| **Amazon Forecast** | Time-series forecasting (demand, inventory, resources) | Retail demand forecasting; staffing/resource forecasting |
| **Lookout for Vision** | Detects visual defects in manufactured products | Defective parts on a production line via camera |
| **Lookout for Metrics** | Detects anomalies in business/operational metrics automatically | Unexpected revenue drop or error spike without manual thresholds |
| **Lookout for Equipment** | Abnormal equipment behavior from sensor data (predictive maintenance) | Predicting machine failure from vibration/temperature data |
| **Amazon Monitron** | End-to-end hardware + ML for equipment condition monitoring | Turnkey sensor + ML kit where no sensors/data pipeline exist |
| **Amazon Panorama** | Computer vision on edge devices/on-prem cameras (appliance + SDK); local/low-latency inference | Shelf/inventory monitoring with existing cameras; PPE detection on a factory floor |

---

## 9. AI hardware

| Chip | Purpose | Instances |
|---|---|---|
| **AWS Trainium** | Purpose-built silicon for **training** ML models at lower cost than GPU instances | **Trn1**: optimized for large-scale deep learning training |
| **AWS Inferentia** | Purpose-built silicon for **inference** at high throughput, low cost | **Inf1** (first gen); **Inf2** (second gen, higher perf, better for large-model/LLM inference at scale) |

- Pattern: distinguish **Trainium (training)** from **Inferentia (inference)** by task, and both from general-purpose **GPU instances (P/G families)**, which are more flexible but typically costlier.
- **Use cases:** Trainium: training a large DL/foundation model from scratch at lower cost; large distributed training. Inferentia: high-volume production inference at lowest cost per prediction; LLM inference at scale (Inf2).

---

## 10. Responsible AI
🆕 *Entire section added (Domain 4, ~14%). Your notes had Bedrock Guardrails, Clarify, A2I and Ground Truth; this ties them together.*

### 10.1 Core dimensions
| Dimension | Meaning |
|---|---|
| **Fairness** | Outputs don't systematically disadvantage groups |
| **Explainability** | Can explain *why* the model made a prediction |
| **Privacy & security** | Protect personal/sensitive data; secure the system |
| **Transparency** | Clear info on how the system works and its limits |
| **Veracity & robustness** | Accurate, reliable, handles unexpected inputs |
| **Governance** | Policies, accountability, compliance processes |
| **Safety** | Prevents harm to users/society |
| **Controllability** | Humans can steer/override the system |

### 10.2 Bias and data quality
- Sources of bias: unrepresentative/imbalanced data, sampling bias, historical bias, labeling bias.
- Good datasets: **diverse, inclusive, balanced, curated** sources, regularly audited.
- Effects of bias: unfair outcomes, loss of customer trust, legal/reputational risk.

### 10.3 Risks specific to GenAI
Hallucination · toxicity/harmful content · IP/copyright infringement · privacy leakage · bias · prompt attacks (Section 3.10) · over-reliance.

### 10.4 Interpretability vs. performance
- **Interpretable / explainable** models (linear regression, decision trees) are easier to explain but may be less accurate.
- **Black-box** models (deep neural nets) perform better but are harder to explain. Tradeoff to know.

### 10.5 AWS tools for Responsible AI
| Tool | Role |
|---|---|
| **Bedrock Guardrails** | Content filters, denied topics, PII redaction, grounding checks |
| **SageMaker Clarify** | Detect bias in data/models; explain predictions |
| **SageMaker Model Monitor** | Detect drift/quality issues post-deployment |
| **SageMaker Model Cards** | Document model purpose, data, limitations |
| **SageMaker Ground Truth** | Human-labeled data, human feedback |
| **Amazon A2I** | Human review of low-confidence predictions |
| **AWS AI Service Cards** | Transparency docs for AWS AI services (intended use, limits) |
| **Bedrock Model Evaluation** | Evaluate FM output quality/toxicity/accuracy |
| **Amazon Transcribe toxicity detection** | Flag toxic audio |
| **Rekognition content moderation** | Flag unsafe images/video |

### 10.6 Human oversight
Keep a **human in the loop** for high-stakes decisions (lending, healthcare, hiring).

---

## 11. Security, compliance & governance services

### 11.0 Quick summary (combined from both of your section summaries)
| Service | One-liner |
|---|---|
| **IAM Users** | Mapped to a physical user; has a password for the AWS Console |
| **IAM Groups** | Contain users only |
| **IAM Policies** | JSON document outlining permissions for users or groups |
| **IAM Roles** | For EC2 instances or AWS services |
| **EC2 Instance** | AMI (OS) + instance size (CPU + RAM) + storage + security groups + EC2 User Data |
| **AWS Lambda** | Serverless, Function as a Service, seamless scaling |
| **VPC Endpoint (PrivateLink)** | Private access to AWS services within a VPC |
| **S3 Gateway Endpoint** | Access Amazon S3 privately |
| **Macie** | Find sensitive data (e.g., PII) in S3 buckets |
| **Config** | Track config changes and compliance against rules |
| **Inspector** | Find software vulnerabilities in EC2, ECR images, Lambda functions |
| **CloudTrail** | Track API calls made by users within the account |
| **Artifact** | Access compliance reports such as PCI, ISO, etc. |
| **Trusted Advisor** | Get insights; Support Plan adapted to your needs |

### 11.1 IAM (Identity and Access Management)
- Controls *who* can do *what* on *which* resource. Foundational to every security question on every AWS exam.
- **Users, Groups, Roles, Policies.** **Roles** (not users) are the exam's preferred answer for service-to-service or temporary access: no long-lived credentials.
- Policies are JSON documents; **identity-based** (attached to user/role) vs. **resource-based** (attached to the resource, e.g., S3 bucket policy).
- **Principle of least privilege**: almost always the "best practice" answer for overly broad permissions.
- **MFA**; **IAM Identity Center** (successor to AWS SSO) for federated/multi-account access.
- 🆕 Don't use the root account for daily work; enable MFA on root.

**IAM policy structure**
- **Version**: policy language version, always include `"2012-10-17"`
- **Id**: identifier for the policy (optional)
- **Statement**: one or more individual statements (required). Each statement has:
  - **Sid**: identifier for the statement (optional)
  - **Effect**: `Allow` or `Deny`
  - **Principal**: account/user/role the policy applies to ✏️ *(used in **resource-based** policies such as S3 bucket policies; identity-based policies omit it because the principal is whoever it's attached to)*
  - **Action**: list of actions allowed or denied
  - **Resource**: list of resources the actions apply to
  - **Condition**: conditions for when the policy is in effect (optional)

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Sid": "AllowInvokeApprovedModel",
    "Effect": "Allow",
    "Action": "bedrock:InvokeModel",
    "Resource": "arn:aws:bedrock:us-east-1::foundation-model/<approved-model-id>"
  }]
}
```
🆕 Explicit **Deny** always overrides Allow.

### 11.2 Service Control Policies (SCPs)
- A policy type in **AWS Organizations** that sets the **maximum permissions** for IAM users and roles in member accounts.
- An SCP **does not grant** permissions. It's a **guardrail**: an action the SCP denies cannot be performed, even if an IAM policy in the account allows it.
- Example: the compliance team attaches an SCP to the org root or OUs that **denies Bedrock invocation actions (e.g., `bedrock:InvokeModel`) on every foundation model except approved model ARNs**. Because it applies across all member accounts, an account admin **cannot override** it by granting broader IAM permissions. This gives central, enforceable control over which models every account can use.

### 11.3 Macie
- Fully managed **data security and data privacy** service using ML and pattern matching to discover and protect sensitive data in AWS.
- Identifies and alerts you to sensitive data such as **PII**.
- 🆕 Works on **S3**.

### 11.4 Config
- Auditing and recording **compliance** of your AWS resources.
- Records configurations and **changes over time**.
- Can store configuration data in **S3** (analyzed by **Athena**).
- Questions it answers:
  1. Is there unrestricted SSH access to my security groups?
  2. Do my buckets have any public access?
  3. How has my ALB configuration changed over time?
- You can receive alerts (**SNS** notifications) for any changes. ✏️ *(typo fixed)*
- **Per-region** service; can be **aggregated** across regions and accounts.

### 11.5 Inspector
- **Automated security assessments**; reporting and integration with **AWS Security Hub**; sends findings to **Amazon EventBridge**.
- **EC2 instances**: uses the **AWS Systems Manager (SSM) agent**; analyzes for unintended network accessibility; analyzes the running OS against known vulnerabilities.
- **Container images pushed to Amazon ECR**: assessed as they are pushed.
- **Lambda functions**: identifies software vulnerabilities in function code and package dependencies; assessed as functions are deployed.

### 11.6 CloudTrail
- Governance, compliance and **audit** for your AWS account.
- **Enabled by default.**
- History of events / API calls made via **Console, SDK, CLI, AWS services**.
- Logs can go to **CloudWatch Logs** or **S3**.
- A trail can apply to **all Regions (default)** or a single Region.
- **If a resource is deleted in AWS, investigate CloudTrail first.**

### 11.7 CloudWatch
- Monitoring and observability: **metrics, logs, alarms, dashboards**.
- Logs (aggregation), Metrics (numeric time-series), Alarms (trigger actions/notifications on thresholds), Events/**EventBridge** (event-driven triggers; EventBridge is the newer, more feature-rich evolution).
- Pattern: "detect and alert on a pipeline failure" → CloudWatch Alarm, often paired with **SNS**.
- **CloudWatch vs. CloudTrail:** CloudWatch = performance/operational monitoring; CloudTrail = API-call auditing (who did what, when).

### 11.8 Artifact ("only provides reports and agreements")
- Portal for **on-demand access to AWS compliance documentation and agreements**.
- **Artifact Reports**: download AWS security/compliance documents from third-party auditors (ISO certifications, PCI, SOC reports).
- **Artifact Agreements**: review, accept, and track status of AWS agreements such as the **Business Associate Addendum (BAA)** or **HIPAA**-related agreements, for an individual account or across an organization.
- Supports internal audit/compliance.
- ✏️ Not to be confused with a Claude "artifact" or with build artifacts; this is the compliance portal.

### 11.9 Audit Manager (assess risk and compliance)
- Assess risk and compliance of AWS workloads; **continuously audit** AWS service usage and prepare audits.
- Prebuilt frameworks: **CIS AWS Foundations Benchmark 1.2.0 & 1.3.0**, **GDPR**, **HIPAA**, **PCI DSS v3.2.1**, **SOC 2**.
- Generates compliance reports alongside **evidence folders**.
- **Artifact vs. Audit Manager:** Artifact = download AWS's *own* compliance docs; Audit Manager = audit *your* usage.

### 11.10 Trusted Advisor
- Guidance to provision resources following **AWS best practices**.
- Optimizes **cost savings, performance, security, fault tolerance**, making it a useful governance tool for AI systems.
- 🆕 Also checks **service limits**. Depth of checks depends on the **Support Plan**.

### 11.11 Compute Optimizer
- Analyzes utilization metrics for **EC2 instances, Auto Scaling groups, EBS volumes, Lambda functions, and ECS services on Fargate**.
- Recommends optimal resource configurations for those types.
- ✏️ It does **not** analyze **Amazon Comprehend custom model endpoints**, so it can't recommend actions for idle Comprehend endpoints (original note referred to a "retailer's idle endpoints" scenario).

### 11.12 VPC (Virtual Private Cloud)
- Your own **logically isolated network** within AWS: subnets, route tables, gateways, security groups, NACLs.
- **Public subnet** (route to Internet Gateway) vs. **private subnet** (no direct internet route; often a **NAT Gateway** for outbound-only access).
- **Security Groups** (stateful, instance-level) vs. **Network ACLs** (stateless, subnet-level).
- **VPC Endpoints** let services like S3/DynamoDB be reached privately without the public internet: a common "improve security" answer.
- For data pipelines: Lambda/Glue jobs can run **inside a VPC** to reach private resources (e.g., RDS in a private subnet).

### 11.13 Governance, compliance and secure AI practices
🆕 *Domain 5 (~14%) concepts missing from the original notes.*

**AWS shared responsibility model**
- **AWS**: security *of* the cloud (hardware, facilities, managed-service infrastructure).
- **Customer**: security *in* the cloud (data, IAM, configuration, prompts, application). For Bedrock, you own your data, access control, and guardrail settings.

**Data protection for AI**
- Encrypt **at rest** (KMS) and **in transit** (TLS).
- Bedrock does **not** use your prompts/data to train the base models; custom models are private to your account.
- Data governance: data lineage, source citation, data residency, retention, quality, access control, cataloging, logging/monitoring.

**Secure data engineering:** assess data quality, use privacy-enhancing techniques (masking/anonymization), enforce access controls, protect data integrity.

**Generative AI Security Scoping Matrix** (how much you own as you move up): Scope 1 consumer app → Scope 2 enterprise app → Scope 3 pre-trained model (e.g., Bedrock) → Scope 4 fine-tuned model → Scope 5 self-trained model. More scope = more security responsibility.

**Regulatory/compliance standards to recognize:** ISO 27001 / **ISO 42001** (AI management systems), SOC 1/2/3, PCI DSS, GDPR, HIPAA, **NIST AI RMF**, **EU AI Act** (risk-tiered AI regulation). Find AWS's reports in **Artifact**.

**AWS Well-Architected:** pillars (operational excellence, security, reliability, performance efficiency, cost optimization, sustainability) with a **Machine Learning Lens** and **Generative AI Lens**.

**Documentation for governance:** **Model Cards**, **AI Service Cards**, data cards; maintain audit trails (CloudTrail), config history (Config), and regular reviews.

---

## 12. Core AWS services

### 12.1 S3 (Simple Storage Service)
- **Object storage**, not a filesystem: stores objects (data + metadata) in buckets, addressed by key.
- **Storage classes** (cost vs. retrieval speed): Standard, Intelligent-Tiering, Standard-IA, One Zone-IA, Glacier Instant Retrieval, Glacier Flexible Retrieval, Glacier Deep Archive.
- **Lifecycle policies** automate moving/expiring objects across classes. Pattern: "minimize storage cost for infrequently accessed data" → lifecycle policy to IA/Glacier.
- **11 nines durability, 99.99% availability** (Standard). Durability ≠ availability; exams test this distinction.
- **Versioning**; **encryption** (SSE-S3, SSE-KMS, SSE-C); **bucket policies** (resource-based, attached to bucket) vs. **IAM policies** (identity-based) vs. **ACLs**.
- Backbone of the **data lake** pattern, paired with **Glue Data Catalog + Athena** for serverless querying.
- 🆕 Common AI role: stores training data, model artifacts, and Knowledge Base source documents.

### 12.2 EC2 (Elastic Compute Cloud)
- Resizable virtual servers ("instances"). Exam rarely goes deep for data/AI certs; know it as the general-purpose compute layer.
- **Pricing:** On-Demand (pay as you go), Reserved (1–3yr commit, cheaper), Spot (spare capacity, cheapest, can be interrupted), Savings Plans.
- **Instance types:** compute-optimized (C), memory-optimized (R), storage-optimized (I), general purpose (M/T). 🆕 Accelerated computing: **P/G (GPU), Trn, Inf**.
- **Auto Scaling Groups + Elastic Load Balancer** = standard resilience/scaling pattern.
- For AI/DE exams: mainly "what powers this managed service under the hood" or Well-Architected cost questions.

### 12.3 Lambda
- Serverless, event-driven compute: no servers to provision, scales automatically, pay-per-invocation.
- **Triggers:** S3 events, API Gateway, DynamoDB Streams, SQS, SNS, EventBridge, Kinesis.
- **Max timeout: 15 minutes.** Exam trap: long-running batch jobs need Glue / Step Functions / Batch, not Lambda.
- Good for lightweight transforms in a data pipeline; not for heavy multi-hour ETL.
- **Cold starts** are a known tradeoff vs. always-on compute.

### 12.4 Step Functions
- Serverless **orchestration**: coordinates multiple services into a visual workflow (state machine) with built-in error handling, retries, and parallel branches.
- Two types: **Standard** (long-running, up to 1 year, exactly-once) and **Express** (short, high-volume, at-least-once, cheaper).
- Pattern: "orchestrate a multi-step pipeline across Lambda, Glue, and SNS with retry logic" → Step Functions over manually chaining Lambdas.

### 12.5 SNS (Simple Notification Service)
- **Pub/sub** messaging: one message, many subscribers (**fan-out**). **Push**-based.
- Subscribers: SQS queues, Lambda, HTTP/S endpoints, email, SMS.
- Pattern: "notify multiple downstream systems of one event" → SNS fan-out to multiple SQS queues.

### 12.6 SQS (Simple Queue Service)
- **Message queuing**: decouples producers and consumers. **Pull**-based (consumer polls).
- **Standard queue:** at-least-once delivery, best-effort ordering, nearly unlimited throughput.
- **FIFO queue:** exactly-once processing, strict ordering, lower throughput.
- **Visibility timeout:** how long a message is hidden from other consumers after being read (prevents duplicate processing while one worker handles it).
- **Dead-letter queue (DLQ):** captures messages that fail repeatedly; answer for "handle poison messages" / "prevent pipeline from stalling on bad data."
- **SNS vs. SQS:** SNS = push, fan-out, many subscribers; SQS = pull, one message processed by one consumer (per queue).

### 12.7 Athena
✏️ *The original notes had an empty heading; filled in.*
- **Serverless, interactive SQL** queries directly on data in **S3** (no loading/ETL, no servers).
- **Pay per query** (by data scanned). Cut cost/speed up queries with columnar formats (**Parquet/ORC**), compression, and partitioning.
- Uses the **Glue Data Catalog** for table metadata; core of the S3 data-lake pattern.
- Can analyze **AWS Config** data stored in S3 and CloudTrail/log data.
- Not a vector store.

---

## 13. Classic comparisons

| Pair | Distinction |
|---|---|
| **Bedrock vs. SageMaker** | Consume pre-trained FMs via API vs. build/train custom models |
| **AI Services vs. ML Services** | Pre-trained API (no ML skills) vs. SageMaker (custom) |
| **RAG vs. fine-tuning** | RAG changes the prompt (fresh/private data, cheap); fine-tuning changes the model (style/task, needs labeled data) |
| **Fine-tuning vs. continued pre-training** | Labeled prompt/response pairs vs. unlabeled domain corpus |
| **Transcribe vs. Polly** | Speech→text vs. text→speech |
| **Comprehend vs. Comprehend Medical** | General NLP vs. clinical text |
| **Custom vocabulary vs. custom language model** | Few specific terms (simple/cheap) vs. broad domain accuracy |
| **Kendra vs. Q Business** | Enterprise search service vs. generative AI assistant (converging; Q Business folding into Amazon Quick) |
| **Trainium vs. Inferentia** | Training vs. inference |
| **CloudTrail vs. CloudWatch** | API audit (who did what) vs. performance/ops monitoring |
| **CloudTrail vs. Config** | API calls/events vs. resource configuration state and compliance over time |
| **Artifact vs. Audit Manager** | AWS's compliance reports/agreements vs. auditing *your* resources |
| **Inspector vs. Macie** | Software vulnerabilities (EC2/ECR/Lambda) vs. sensitive data in S3 |
| **SCP vs. IAM policy** | Org-level maximum guardrail (grants nothing) vs. grants permissions to identities |
| **Identity-based vs. resource-based policy** | Attached to user/role vs. attached to resource (S3 bucket policy) |
| **SNS vs. SQS** | Push, fan-out vs. pull, one consumer |
| **Security Group vs. NACL** | Stateful, instance-level vs. stateless, subnet-level |
| **Durability vs. availability (S3)** | Not losing data (11 nines) vs. being accessible (99.99%) |
| **Standard vs. Express Step Functions** | Long-running, exactly-once vs. short, high-volume, at-least-once |
| **Standard vs. FIFO SQS** | At-least-once, best-effort order vs. exactly-once, strict order |
| **Mechanical Turk vs. A2I** | Crowd labeling/tasks vs. human review of ML predictions in a workflow |
| **Guardrails vs. Clarify** | Runtime GenAI content safety vs. bias detection/explainability in ML |

---

## 14. Exam traps
- **No ML expertise → pre-built AI service**, not SageMaker.
- **Lambda max 15 min.** Long jobs → Glue / Step Functions / Batch.
- **SSML** = Speech Synthesis Markup Language.
- **Durability ≠ availability.**
- **Roles > users** for services and temporary access.
- **SCPs never grant permissions**; they only cap them.
- **Resource deleted? → CloudTrail first.**
- **Config is per-region** (aggregate across regions/accounts).
- **CloudTrail is on by default**; trails cover all regions by default.
- **Comprehend ≠ Comprehend Medical**; clinical text needs the Medical one.
- **Custom vocabulary** before custom language model for a handful of terms.
- **Same embedding model** for indexing and querying in RAG.
- **Vector DB ≠ Athena/CloudFront/Route 53.**
- **RAG changes the prompt; fine-tuning changes the model.**
- **Custom (fine-tuned) Bedrock models need Provisioned Throughput.** 🆕
- **Compute Optimizer doesn't cover Comprehend custom endpoints.**
- **Artifact = compliance docs portal**, not a monitoring/auditing tool.
- **PartyRock is for learning/prototyping**, not production.
- **Q / Quick branding is shifting**; know the concepts under both names.

---

## 15. Interview rapid-fire Q&A
🆕 *Short answers to practice out loud.*

**What's the difference between AI, ML, deep learning, and GenAI?**
Nested: AI ⊃ ML ⊃ deep learning ⊃ generative AI (FMs/LLMs that create content).

**Bedrock or SageMaker?**
Bedrock to use/customize pre-trained foundation models via API, serverless. SageMaker to build, train, and deploy your own models with full control.

**How do you reduce hallucinations?**
RAG grounding, Guardrails (contextual grounding checks), clear prompts, lower temperature, human review, citing sources.

**RAG vs. fine-tuning?**
RAG injects fresh/private knowledge at query time (no retraining, cheaper, citable). Fine-tuning changes the model for a task/style using labeled data.

**What is an embedding and why does it matter?**
A vector capturing meaning; enables semantic similarity search, the core of RAG.

**What does temperature do?**
Controls randomness: low = focused/deterministic, high = creative/varied.

**How do you secure a GenAI app on AWS?**
IAM least privilege + roles, Guardrails, PrivateLink/VPC endpoints, KMS encryption, CloudTrail/CloudWatch logging, Config, SCPs for approved models, Macie for PII in S3.

**How do you prove compliance to an auditor?**
Artifact for AWS's reports; Audit Manager for continuous evidence on your workloads; Config/CloudTrail for history.

**Shared responsibility for Bedrock?**
AWS secures the infrastructure; you secure your data, access, prompts, guardrails, and usage.

**How do you ensure responsible AI?**
Diverse data, Clarify for bias/explainability, Model Cards for transparency, Guardrails for safety, A2I/human-in-the-loop, monitoring for drift, governance policies.

**Rekognition vs. Textract?**
Rekognition analyzes images/video (objects, faces, moderation); Textract extracts text, forms, tables from documents.

**Kendra vs. Bedrock Knowledge Bases?**
Kendra = managed enterprise search/retriever; Knowledge Bases = managed RAG pipeline for FMs (can use Kendra as retriever).

**Trainium vs. Inferentia?**
Trainium trains (Trn1); Inferentia serves inference (Inf1/Inf2).

**When would a human review ML output?**
Low confidence or high stakes → Amazon A2I.

**What is prompt injection?**
Malicious input that overrides instructions to leak data or change behavior. Mitigate with Guardrails, input validation, least privilege.

---

## 16. Last-day checklist
- [ ] Recite the **scenario → service table** (Section 2) from memory.
- [ ] Know the **RAG pipeline order** and vector store picks.
- [ ] Know **inference parameters** and **prompt techniques**.
- [ ] Know **fine-tuning vs. continued pre-training vs. RAG vs. prompting**.
- [ ] Know **precision/recall/F1** and **ROUGE/BLEU/BERTScore**.
- [ ] Know the **8 responsible-AI dimensions** and which AWS tool covers each.
- [ ] Know **IAM policy elements**, **SCP behavior**, **shared responsibility**.
- [ ] Know the **security service one-liners** (Macie, Config, Inspector, CloudTrail, Artifact, Audit Manager, Trusted Advisor).
- [ ] Know **Trainium vs. Inferentia**, **Standard vs. FIFO**, **SNS vs. SQS**, **Standard vs. Express workflows**.
- [ ] Re-read **Exam traps** (Section 14).
- [ ] Skim the official exam guide for any renamed services. ⚠️
