# AWS Gen AI Essentials — Lab Day 1
**Date:** September 17, 2026  
**Training:** Gen AI Essentials on AWS (Company Sponsored — Publicis Sapient)  
**Platform:** Amazon Bedrock (Builder / Temporary AWS Account)  
**Model used:** Amazon Nova Lite (primary demo model)

---

## Lab 1 — Prompt Engineering on Amazon Bedrock

### What we did
- Opened Amazon Bedrock console and browsed the model catalogue
- Selected Amazon Nova Lite for all demonstrations (lightweight, fast, cost-effective)
- Sent prompts directly via the Bedrock playground UI — no code written

### Concepts practiced

| Technique | What we did | What we observed |
|---|---|---|
| Zero-shot prompting | Asked the model a question with no examples | Model answered from its training knowledge alone |
| Single-shot prompting | Gave one example before asking the question | Model picked up the pattern from the example |
| Few-shot prompting | Gave 2-3 examples before asking | More consistent and structured responses |
| System prompt | Added instructions defining model persona and constraints | Model stayed within defined behaviour across all turns |
| User prompt | Regular question sent per turn | Model responded within the context set by system prompt |

### Key difference observed — System vs User Prompt
- **System prompt:** Set once, applies to the entire conversation. Defines who the model is, what it can/cannot do, what tone to use
- **User prompt:** Per-turn input from the end user. The model treats the system prompt as its operating instructions and the user prompt as the task
- Example: System prompt said "You are a helpful HR assistant. Only answer questions about company policies." User prompt asking about cooking was refused — guardrails in effect even without explicit Bedrock Guardrails feature

---

## Lab 2 — Responsible AI with Bedrock Guardrails

### What we did
- Created a Guardrail in the Amazon Bedrock console
- Configured deny topics — blocked the model from giving:
  - Medical advice
  - Investment advice / financial recommendations
- Tested the guardrail by sending prompts that directly asked for blocked content

### What we observed
- Without guardrail: model would attempt to answer medical/investment questions
- With guardrail applied: model returned a safe, neutral refusal message instead of the blocked content
- The guardrail acted as a layer on top of the model — no retraining needed, applied at inference time
- Even indirect attempts to get blocked content were caught by semantic understanding (not just keyword matching)

### Guardrail configuration we used
| Setting | Value |
|---|---|
| Deny topics | Medical advice, investment advice |
| Filter type | Standard (semantic — not just keyword) |
| Applied to | Both input (user prompt) and output (model response) |
| Model | Amazon Nova Lite |

### Why this matters
Guardrails are the primary mechanism for making foundation models safe for enterprise deployment. Instead of fine-tuning or replacing the model, you add guardrails to enforce business rules, compliance requirements, and content policies at runtime.

---

## Lab 3 — HR Assistant with Bedrock Knowledge Bases (RAG Pipeline)

### What we built
An internal HR assistant that answers employee questions based on actual company policy documents — not the model's general training knowledge.

### Architecture

```
Employee documents (S3)
        ↓
Bedrock Knowledge Base
(chunking → embedding → vector storage)
        ↓
Bedrock Agent
(retrieves relevant chunks, passes to model as context)
        ↓
Amazon Nova Lite (LLM)
(generates answer grounded in retrieved documents)
        ↓
Web UI
(employee types question, gets policy-grounded answer)
```

### Step by step what we configured

| Step | What we did |
|---|---|
| 1 | Uploaded HR documents to S3 — leave policies, employee handbook, company guidelines |
| 2 | Created a Bedrock Knowledge Base pointing to that S3 bucket |
| 3 | Bedrock automatically chunked the documents, generated embeddings, stored in a vector index |
| 4 | Created a Bedrock Agent with a system prompt defining it as an HR assistant |
| 5 | Connected the Knowledge Base to the Agent as a retrieval tool |
| 6 | Tested via the Bedrock console UI — asked HR questions, verified answers cited company docs |

### What the agent does at query time
1. Employee asks: "How many casual leaves do I get per year?"
2. Agent embeds the question into a vector
3. Searches Knowledge Base for most similar document chunks
4. Retrieves top K relevant chunks (leave policy section)
5. Passes retrieved chunks + question to Nova Lite
6. Nova Lite generates answer grounded in the retrieved policy
7. Employee sees accurate, document-based answer — not hallucinated

### System prompt used (approximate)
*"You are an HR assistant for [Company]. Answer employee questions only using the provided company documents. If the answer is not in the documents, say you do not have that information. Do not provide advice beyond what is stated in the policies."*

### Key observations
- Model refused to answer questions not covered in the uploaded documents
- Answers were traceable back to specific policy documents (citations)
- No hallucination on policy-specific questions — RAG grounding worked as expected
- The entire setup was done via console UI — no code written

---

## No Code — Why That's Fine

Everything in these labs was done through the AWS console UI. This is by design — Bedrock's console is built for exploration and prototyping before you write any API code. The underlying architecture (S3 → Knowledge Base → Agent → LLM) is identical to what you would build programmatically using the Bedrock SDK. The lab demonstrates the concepts; production implementation uses the API.

---

## Key Terms to Remember for Exam

| Term | One line |
|---|---|
| Zero-shot | No examples given — model uses training knowledge only |
| Few-shot | 2-3 examples given — model picks up pattern |
| System prompt | Persistent instructions defining model behaviour for the session |
| Guardrail | Runtime safety filter — blocks/redacts content at inference time without retraining |
| Knowledge Base | Bedrock's managed RAG — S3 docs → chunked → embedded → vector indexed → retrievable |
| Bedrock Agent | LLM that uses tools (Knowledge Base, APIs) to answer questions with retrieved context |
| RAG | Retrieval Augmented Generation — ground model responses in external documents |
| Chunking | Splitting documents into smaller pieces before embedding — needed for context window limits |
| Embedding | Converting text chunks into vectors for similarity search |

---
