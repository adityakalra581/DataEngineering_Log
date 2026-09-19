# AWS Gen AI Essentials Training — Day 1 Notes
**Date:** September 17, 2026  
**Training:** Gen AI Essentials on AWS  
**Certification Target:** AWS Certified AI Practitioner (AIF-C01)

---

## 1. Evolution of AI

| Stage | What it is |
|---|---|
| Artificial Intelligence | Broad field — machines simulating human intelligence |
| Machine Learning | Subset of AI — machines learn from data without explicit programming |
| Deep Learning | Subset of ML — neural networks with many layers, learns complex patterns |
| Generative AI | Subset of Deep Learning — models that generate new content (text, image, audio, code) |
| Agentic AI | Next evolution — AI that takes autonomous multi-step actions to achieve goals |

---

## 2. AWS Regions — How to Choose

### Factors for Choosing a Region
- **Data residency / compliance** — GDPR requires EU data to stay in EU, HIPAA for healthcare data in US
- **Latency** — choose region closest to your end users
- **Service availability** — not all AWS services available in all regions (e.g. Bedrock not in all regions)
- **Cost** — same service costs differently across regions due to local infrastructure, taxes, energy costs
- **Disaster recovery** — multi-region setup for high availability

### Why Same Services Cost Differently Across Regions
Local operating costs, data center infrastructure costs, energy costs, and local taxes all vary by geography. US East (N. Virginia) is typically cheapest — most mature infrastructure.

---

## 3. Core AWS Services — Quick Reference

| Service | What it does |
|---|---|
| **Bedrock** | Managed API access to foundation models — no infrastructure to manage |
| **S3** | Object storage — stores files, images, documents, training data |
| **EC2** | Virtual machines — compute on demand |
| **IAM** | Identity and Access Management — controls who can access what |
| **Lambda** | Serverless functions — run code without managing servers |
| **VPC** | Virtual Private Cloud — isolated private network within AWS |
| **RDS** | Managed relational databases — PostgreSQL, MySQL, Oracle |
| **ECS** | Elastic Container Service — run Docker containers |
| **EKS** | Elastic Kubernetes Service — managed Kubernetes |
| **CloudWatch** | Monitoring, logging, alerting for all AWS services |

---

## 4. Generative AI Fundamentals

### Embeddings
- Numerical vector representation of text, images, or audio
- Similar meaning = similar vectors = close in vector space
- Used to convert unstructured data into something a model can compare mathematically

### RAG (Retrieval Augmented Generation)
- Retrieve relevant documents from a knowledge base → pass as context to LLM → generate grounded response
- Solves hallucination problem — model answers from your documents, not just training memory
- Pipeline: Query → Embed query → Search vector DB → Retrieve top K docs → Pass to LLM → Response

### Vector Database
- Stores embeddings (vectors) and enables fast similarity search
- AWS options: OpenSearch Serverless with vector engine, Aurora pgvector

### Knowledge Base (Amazon Bedrock)
- Managed RAG on AWS — connect your S3 documents to a foundation model
- Handles chunking, embedding, vector storage, retrieval automatically
- No need to build RAG infrastructure manually

### PDF Chunking
- Large documents split into smaller chunks before embedding
- Strategies: fixed size, sentence boundary, semantic chunking
- Chunk overlap prevents context loss at boundaries

---

## 5. Foundation Models vs LLMs

| | Foundation Model | LLM |
|---|---|---|
| Definition | Large pretrained model on broad data — can be adapted for many tasks | Specifically a foundation model trained on text |
| Modality | Can be text, image, audio, video, code (multimodal) | Text only |
| Examples | Amazon Titan, Anthropic Claude, Meta Llama | GPT-4, Claude, Llama |
| Relationship | LLM is a type of Foundation Model | Subset of Foundation Models |

### Modality
- **Single modality** — handles one data type only (text in, text out)
- **Multimodal** — handles multiple data types (text + image + audio in/out)

---

## 6. Models Available in Amazon Bedrock

| Provider | Models available |
|---|---|
| **Amazon** | Titan Text, Titan Embeddings, Nova Micro, Nova Lite, Nova Pro |
| **Anthropic** | Claude 3 Haiku, Claude 3 Sonnet, Claude 3 Opus, Claude 3.5 series |
| **Meta** | Llama 3, Llama 3.1, Llama 3.2 |
| **Mistral** | Mistral 7B, Mixtral 8x7B |
| **Cohere** | Command R, Command R+ |
| **Stability AI** | Stable Diffusion (image generation) |

### Amazon Nova Family
- **Nova Micro** — text only, fastest and cheapest, lowest latency
- **Nova Lite** — multimodal (text + image + video), low cost
- **Nova Pro** — multimodal, highest capability in Nova family, balanced cost/performance

### When to Use Bedrock vs Specific AWS AI Services
- **Use specific services** (Rekognition, Polly, Transcribe, Comprehend) when your use case maps exactly to what they do
- **Use Bedrock** when you have specific needs not covered by managed services — custom prompting, fine-tuning, complex RAG, agents

---

## 7. Customizing Foundation Models

| Method | What it is | Cost | When to use |
|---|---|---|---|
| Prompt engineering | Change input to get better output — no retraining | Free | First thing to try always |
| RAG | Retrieve external docs and pass as context | Low | When model needs your org's specific knowledge |
| Fine-tuning | Retrain model on your labelled dataset | High | When prompt engineering and RAG are insufficient |
| Continued pre-training | Further train on unlabelled domain data | Very high | Specialist domains — medical, legal |

---

## 8. Key Model Parameters

| Parameter | What it controls |
|---|---|
| **Temperature** | Randomness of output. 0 = deterministic, 1 = creative. Lower for factual tasks, higher for creative |
| **Top P (nucleus sampling)** | Model samples from smallest set of tokens whose cumulative probability ≥ P. Lower = more focused |
| **Top K** | Model considers only top K most likely next tokens. Lower K = less random output |

Rule of thumb: for factual/precise outputs lower all three. For creative outputs raise temperature.

---

## 9. System Prompt vs User Prompt

| | System Prompt | User Prompt |
|---|---|---|
| Who sets it | Developer / operator | End user |
| Purpose | Sets behaviour, persona, constraints, tone for the entire session | The actual question or instruction per turn |
| Example | "You are a helpful financial advisor. Never give specific investment advice." | "What is a good diversified portfolio strategy?" |
| Persistence | Stays constant across the conversation | Changes every turn |

---

## 10. Responsible AI

### Key Principles
- **Fairness** — model should not discriminate by race, gender, age, religion
- **Explainability** — ability to explain why a model made a decision
- **Transparency** — disclose when AI is being used
- **Privacy** — protect personal data, comply with regulations
- **Robustness** — model should perform reliably across different inputs
- **Governance** — audit trails, version control, human oversight

### Removing Bias from Models
- Diverse and representative training data
- Bias detection during training — SageMaker Clarify
- Regular auditing of model outputs
- Human review for high-stakes decisions

### AI System Lifecycle — 8 Stages
1. Problem definition
2. Data collection
3. Data preparation and cleaning
4. Model selection
5. Model training
6. Model evaluation
7. Model deployment
8. Monitoring and maintenance

---

## 11. Guardrails in Amazon Bedrock

### Types
| Guardrail Type | What it blocks |
|---|---|
| **Deny topics** | Specific topics you define — e.g. competitor mentions, political content |
| **PII filter** | Detects and redacts personal identifiable information — names, emails, phone numbers, SSN |
| **Word filter** | Blocks specific words or phrases — profanity, brand names |
| **Grounding** | Checks response is grounded in the retrieved context — reduces hallucination |
| **Relevance** | Checks retrieved context is relevant to the query before passing to model |
| **Contextual grounding** | Combines grounding + relevance — most comprehensive hallucination prevention |

### Classic vs Standard Guardrails
- **Classic** — simpler rule-based filters, faster, lower cost
- **Standard** — more sophisticated, semantic understanding, better at nuanced cases

### Applying Guardrails via API
- Pass `guardrailIdentifier` and `guardrailVersion` in your Bedrock API call
- Can apply at inference time — does not require retraining the model
- Works with any model available in Bedrock

---

## 12. Amazon AI Services — Quick Reference

| Service | What it does | Use case |
|---|---|---|
| **Amazon Q Business** | Enterprise AI assistant on your company data | Employee productivity, internal knowledge search |
| **Amazon Q Developer** | AI coding assistant | Code generation, debugging, documentation |
| **Amazon Polly** | Text to speech | Voice applications, accessibility |
| **Amazon Rekognition** | Image and video analysis | Face detection, object recognition, content moderation |
| **Amazon Transcribe** | Speech to text | Meeting transcription, call centre analytics |
| **Amazon Comprehend** | NLP — sentiment, entities, language detection | Text analysis without building a custom model |
| **Amazon Kendra** | Intelligent enterprise document search | Different from Knowledge Bases — keyword + semantic search on docs |

---

## 13. Amazon Bedrock Agents

- LLM that can take multi-step autonomous actions
- Can call APIs, query databases, run Lambda functions, search knowledge bases
- Uses ReAct pattern — Reason → Act → Observe → Repeat until goal achieved
- Bedrock Agent Core — foundational layer for building production-grade agents on AWS

---

## 14. Security, Compliance and Governance

### Key Regulations
| Regulation | What it covers |
|---|---|
| **GDPR** | EU data privacy — right to erasure, data portability, consent required |
| **HIPAA** | US healthcare data — strict controls on patient health information |
| **SOC 2** | Security and availability controls — common for SaaS |

### AWS Tools for Compliance
| Tool | Purpose |
|---|---|
| **AWS Artifact** | Self-service portal to access AWS compliance reports and agreements — SOC, ISO, PCI reports |
| **AWS Audit Manager** | Continuously audit AWS usage against compliance frameworks |
| **Amazon Macie** | Detects PII and sensitive data in S3 |
| **AWS CloudTrail** | Audit log of all API calls — who did what and when |
| **IAM** | Least privilege access — controls who can call Bedrock or any service |
| **KMS** | Encryption key management — encrypt data at rest |
| **VPC Endpoints** | Keep Bedrock API calls within private network — no public internet |

---

## Certification Exam Relevance — Domain Mapping

| Training Topic | Exam Domain | Weight |
|---|---|---|
| AI evolution, ML fundamentals | Domain 1 — AI/ML Fundamentals | 20% |
| Foundation models, LLMs, modality, embeddings | Domain 2 — Gen AI Fundamentals | 24% |
| Bedrock, Knowledge Bases, Agents, Guardrails, model selection | Domain 3 — Applications of Foundation Models | 28% |
| Responsible AI, bias, AI lifecycle | Domain 4 — Responsible AI | 14% |
| Security, GDPR, HIPAA, IAM, Artifact | Domain 5 — Security and Governance | 14% |

---

*Day 2 notes to follow*
