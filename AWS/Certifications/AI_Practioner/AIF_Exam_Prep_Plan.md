# AWS Certified AI Practitioner (AIF-C01) — Full Prep Plan

**Exam facts (official AWS):** 65 questions, 90 minutes, $100 USD, pass mark 700/1000 (scaled), Pearson VUE (test center or online proctored).

## The 5 Domains (know the weights — study time should roughly mirror them)

| Domain | Weight |
|---|---|
| 1. Fundamentals of AI and ML | 20% |
| 2. Fundamentals of Generative AI | 24% |
| 3. Applications of Foundation Models | 28% |
| 4. Guidelines for Responsible AI | 14% |
| 5. Security, Compliance, and Governance for AI Solutions | 14% |

## Full Topic List by Domain

**Domain 1 — Fundamentals of AI and ML (20%)**
- Core terms: AI vs. ML vs. deep learning vs. neural networks, computer vision, NLP, model, algorithm, training vs. inference, bias, fairness, overfitting/underfitting
- Types of ML: supervised, unsupervised, reinforcement learning
- Practical AI/ML use cases (fraud detection, recommendation, forecasting, personalization)
- The ML development lifecycle: data collection → EDA → feature engineering → model training → evaluation → deployment → monitoring
- Core AWS ML services: Amazon SageMaker (and sub-features), Rekognition, Comprehend, Transcribe, Polly, Translate, Textract, Personalize, Forecast, Lex

**Domain 2 — Fundamentals of Generative AI (24%)**
- Core concepts: tokens, embeddings, chunking, vectors, prompt, transformer architecture, foundation model, LLM, multimodal, diffusion models
- Capabilities and limitations: hallucination, non-determinism, "knowledge cutoff," interpretability, cost/latency tradeoffs
- Benefits and drawbacks of gen AI adoption for a business
- AWS gen AI stack: Amazon Bedrock (and its FM providers), Amazon Titan/Nova models, SageMaker JumpStart, PartyRock, Amazon Q

**Domain 3 — Applications of Foundation Models (28% — highest weight, prioritize)**
- Model selection criteria: cost, modality, latency, context window, accuracy needs
- Prompt engineering techniques: zero-shot, few-shot, chain-of-thought, prompt templates
- Fine-tuning vs. RAG vs. prompt engineering — when to use which, and why (cost/data/latency tradeoffs)
- RAG mechanics: embeddings, vector databases, retrieval, Bedrock Knowledge Bases
- Agents and orchestration: what an agent adds over a single LLM call, Bedrock Agents/AgentCore
- Evaluating FM performance: human evaluation vs. automated benchmarks, business metrics vs. model metrics (BLEU/ROUGE aren't core-tested but the concept of "evaluate against a metric" is)

**Domain 4 — Guidelines for Responsible AI (14%)**
- The 8 dimensions of responsible AI: fairness, explainability, privacy & security, safety, controllability, veracity & robustness, governance, transparency
- Bias sources: training data bias, and how it surfaces in outputs
- Transparent/explainable models: SageMaker Clarify, model cards
- Human-in-the-loop, guardrails as an implementation of responsible AI (Bedrock Guardrails)

**Domain 5 — Security, Compliance, and Governance for AI Solutions (14%)**
- AWS shared responsibility model as applied to AI workloads
- Securing AI systems: data encryption (at rest/in transit), IAM least-privilege, PII handling, prompt injection defenses
- Governance frameworks vs. compliance requirements (distinct concepts — governance = internal policy/oversight, compliance = meeting external regulation/standard)
- Relevant AWS security services: IAM, Macie (PII detection), Bedrock Guardrails, CloudTrail (audit logging)

## Free Resources
- **AWS Skill Builder** (free tier) — official "AWS Certified AI Practitioner Learning Plan" and free digital courses: https://skillbuilder.aws
- **Official exam guide (PDF)** — download from the AWS Certified AI Practitioner page: https://aws.amazon.com/certification/certified-ai-practitioner/
- **AWS Bedrock documentation** (free, no account needed to read): https://docs.aws.amazon.com/bedrock/
- **freeCodeCamp's full AIF-C01 course on YouTube** — several free multi-hour walkthroughs exist; search "AWS Certified AI Practitioner freeCodeCamp"
- **Tutorials Dojo blog** — free AIF-C01 study guide and domain breakdowns: tutorialsdojo.com
- **AWS Whitepapers** — "Overview of Amazon Web Services," "Machine Learning Best Practices," "Responsible Use of Machine Learning" — all free PDFs on AWS's site
- **AWS Free Tier account** — spin up Bedrock in the console and actually run a prompt, try Guardrails, try Knowledge Bases hands-on; this cements Domain 3 far better than reading alone

## 17-Day Plan (Sept 25 → Oct 12, then buffer to exam day)

| Dates | Focus |
|---|---|
| Sept 25–27 | Domain 1 fully. Read AWS Skill Builder's free intro modules. Note every AWS service name and one-line purpose (Rekognition, Comprehend, Transcribe, Polly, Translate, Textract, Personalize, Forecast, Lex). |
| Sept 28–30 | Domain 2. You've already seen Bedrock, Nova 2, and Guardrails live in training — use that as recall practice rather than starting cold. Nail down tokens, embeddings, chunking, transformer basics, and gen AI limitations (hallucination, non-determinism). |
| Oct 1–4 | Domain 3 (heaviest weight, 28%) — prompt engineering techniques, fine-tuning vs. RAG vs. prompting decision tree, agents/AgentCore, evaluating FM performance. If you still have free-tier AWS access, redo a quick Bedrock + Knowledge Bases run-through. |
| Oct 5–7 | Domain 4 + Domain 5. Memorize the 8 responsible-AI dimensions cold. Shared responsibility model, governance vs. compliance, securing AI pipelines. |
| Oct 8–11 | Full-length practice exams (aim for 2–3). Review every wrong answer's *reasoning*, not just the correct option. Re-drill whichever domain is weakest. |
| Oct 12 | Get the voucher. Light review only — no new material. |
| Buffer | Post Oct 12 | Schedule and sit the exam within a few days while it's fresh. |

## Practice Questions (all 5 domains)

1. What's the difference between AI, ML, and deep learning?
2. Give an example of supervised vs. unsupervised learning.
3. What does "inference" mean as distinct from "training"?
4. Name three AWS services used for computer vision, NLP, and speech, respectively.
5. What is a foundation model, and how does it differ from a traditional ML model?
6. What is an embedding, and why does it matter for RAG?
7. What's the difference between fine-tuning and RAG — when would you pick each?
8. What is prompt engineering, and name two techniques (e.g., zero-shot, few-shot).
9. What is hallucination in an LLM context, and why does it happen?
10. What's the tradeoff between a large, general-purpose FM and a small, task-specific one (cost/latency/accuracy)?
11. What does Amazon Bedrock do, and how is it different from SageMaker?
12. What is a vector database used for in a gen AI application?
13. Name the 8 dimensions of responsible AI.
14. What does explainability mean, and what AWS tool helps with it?
15. What is a "guardrail" in the context of an LLM application, and what can it filter or block?
16. What is prompt injection, and how would you defend against it?
17. What's the difference between AI governance and AI compliance?
18. What does the shared responsibility model mean for an AI workload on AWS?
19. Name two ways to secure sensitive data used in a gen AI pipeline.
20. What business metric would you use to decide if a gen AI feature succeeded (accuracy alone isn't always it — cost, latency, user satisfaction also matter)?
21. When would a business choose an agent-based architecture over a single prompt-response call?
22. What's the ML development lifecycle, in order, from data to deployed model?
