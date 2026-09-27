# AWS Certified AI Practitioner (AIF-C01) — Full Prep Plan

**Timeline:** Voucher by Oct 12, 2026 — you have 17 days from today (Sept 25) to prep, plus buffer after. Your Generative AI Essentials training on the 17th already covered Bedrock, Nova 2, AgentCore, and Guardrails hands-on, so Domains 2–3 have a head start. **Exam facts (official AWS):** 65 questions, 90 minutes, $100 USD, pass mark 700/1000 (scaled), Pearson VUE (test center or online proctored).

## The 5 Domains (know the weights — study time should roughly mirror them)

| Domain | Weight |
| --- | --- |
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

- **AWS Skill Builder** (free tier) — official "AWS Certified AI Practitioner Learning Plan" and free digital courses: [https://skillbuilder.aws](https://skillbuilder.aws)
- **Official exam guide (PDF)** — download from the AWS Certified AI Practitioner page: [https://aws.amazon.com/certification/certified-ai-practitioner/](https://aws.amazon.com/certification/certified-ai-practitioner/)
- **AWS Bedrock documentation** (free, no account needed to read): [https://docs.aws.amazon.com/bedrock/](https://docs.aws.amazon.com/bedrock/)
- **freeCodeCamp's full AIF-C01 course on YouTube** — several free multi-hour walkthroughs exist; search "AWS Certified AI Practitioner freeCodeCamp"
- **Tutorials Dojo blog** — free AIF-C01 study guide and domain breakdowns: tutorialsdojo.com
- **AWS Whitepapers** — "Overview of Amazon Web Services," "Machine Learning Best Practices," "Responsible Use of Machine Learning" — all free PDFs on AWS's site
- **AWS Free Tier account** — spin up Bedrock in the console and actually run a prompt, try Guardrails, try Knowledge Bases hands-on; this cements Domain 3 far better than reading alone

## 17-Day Plan (Sept 25 → Oct 12, then buffer to exam day)

| Dates | Focus |
| --- | --- |
| Sept 25–27 | Domain 1 fully. Read AWS Skill Builder's free intro modules. Note every AWS service name and one-line purpose (Rekognition, Comprehend, Transcribe, Polly, Translate, Textract, Personalize, Forecast, Lex). |
| Sept 28–30 | Domain 2. You've already seen Bedrock, Nova 2, and Guardrails live in training — use that as recall practice rather than starting cold. Nail down tokens, embeddings, chunking, transformer basics, and gen AI limitations (hallucination, non-determinism). |
| Oct 1–4 | Domain 3 (heaviest weight, 28%) — prompt engineering techniques, fine-tuning vs. RAG vs. prompting decision tree, agents/AgentCore, evaluating FM performance. If you still have free-tier AWS access, redo a quick Bedrock + Knowledge Bases run-through. |
| Oct 5–7 | Domain 4 + Domain 5. Memorize the 8 responsible-AI dimensions cold. Shared responsibility model, governance vs. compliance, securing AI pipelines. |
| Oct 8–11 | Full-length practice exams (aim for 2–3). Review every wrong answer's *reasoning*, not just the correct option. Re-drill whichever domain is weakest. |
| Oct 12 | Get the voucher. Light review only — no new material. |
| Buffer | Post Oct 12 |

## Practice Questions (all 5 domains) — with Answers

1. **What's the difference between AI, ML, and deep learning?** AI is the broad field of building systems that perform tasks requiring human-like intelligence. ML is a subset of AI where systems learn patterns from data rather than following explicit rules. Deep learning is a subset of ML using multi-layer neural networks, well-suited to unstructured data like images and text.
2. **Give an example of supervised vs. unsupervised learning.** Supervised: predicting loan default using labeled historical data (default/no-default). Unsupervised: clustering customers into segments with no predefined labels.
3. **What does "inference" mean as distinct from "training"?** Training is the process of fitting a model's parameters to data. Inference is using the already-trained model to generate a prediction or output on new, unseen input.
4. **Name three AWS services used for computer vision, NLP, and speech, respectively.** Computer vision: Amazon Rekognition. NLP: Amazon Comprehend. Speech: Amazon Transcribe (speech-to-text) or Amazon Polly (text-to-speech).
5. **What is a foundation model, and how does it differ from a traditional ML model?** A foundation model is a large model pre-trained on broad, diverse data that can be adapted to many downstream tasks. A traditional ML model is typically trained narrowly for one specific task from scratch.
6. **What is an embedding, and why does it matter for RAG?** An embedding is a numerical vector representation of text (or other data) that captures semantic meaning. RAG uses embeddings to find and retrieve the most semantically relevant documents from a vector store before generating a response.
7. **What's the difference between fine-tuning and RAG — when would you pick each?** Fine-tuning retrains a model on your own labeled data so it internalizes new behavior/knowledge — pick it when you need consistent style/format changes or domain-specific reasoning baked into the model. RAG retrieves relevant external data at query time without changing the model — pick it when the underlying knowledge changes often or is too large to bake in (e.g., a constantly updated knowledge base).
8. **What is prompt engineering, and name two techniques (e.g., zero-shot, few-shot).** Prompt engineering is designing input prompts to get better, more reliable model outputs without changing the model itself. Techniques: zero-shot (no examples given), few-shot (a handful of examples included in the prompt), and chain-of-thought (asking the model to reason step by step).
9. **What is hallucination in an LLM context, and why does it happen?** Hallucination is when a model generates plausible-sounding but factually incorrect or fabricated content. It happens because LLMs generate the statistically likely next token rather than verifying facts against a ground truth source.
10. **What's the tradeoff between a large, general-purpose FM and a small, task-specific one (cost/latency/accuracy)?** Large general-purpose FMs tend to be more capable and flexible across tasks but cost more per call and have higher latency. Smaller, task-specific models are cheaper and faster but may underperform on tasks outside their narrow training focus.
11. **What does Amazon Bedrock do, and how is it different from SageMaker?** Bedrock provides API access to pre-trained foundation models from AWS and third parties (Anthropic, Meta, etc.) without managing infrastructure. SageMaker is a broader ML platform for building, training, and deploying custom models from scratch.
12. **What is a vector database used for in a gen AI application?** It stores embeddings and enables fast similarity search, so an application can retrieve the most semantically relevant chunks of data to feed into a prompt (the retrieval step in RAG).
13. **Name the 8 dimensions of responsible AI.** Fairness, explainability, privacy & security, safety, controllability, veracity & robustness, governance, transparency.
14. **What does explainability mean, and what AWS tool helps with it?** Explainability is the ability to understand and articulate why a model produced a given output. Amazon SageMaker Clarify helps detect bias and explain model predictions.
15. **What is a "guardrail" in the context of an LLM application, and what can it filter or block?** A guardrail is a configurable safety layer applied to model inputs/outputs. Bedrock Guardrails can filter denied topics, block harmful content categories, redact PII, and enforce word/phrase restrictions.
16. **What is prompt injection, and how would you defend against it?** Prompt injection is when a malicious input tries to override or manipulate a model's intended instructions (e.g., embedded in retrieved content). Defenses include input validation, guardrails, strict system-prompt boundaries, and treating retrieved/external content as untrusted data rather than instructions.
17. **What's the difference between AI governance and AI compliance?** Governance is the internal policies, oversight, and accountability structures an organization sets for how it builds and uses AI. Compliance is meeting external regulatory or legal requirements (e.g., GDPR, industry-specific regulation).
18. **What does the shared responsibility model mean for an AI workload on AWS?** AWS secures the underlying infrastructure ("security of the cloud"), while the customer is responsible for securing their data, access configuration, and how they use the AI service ("security in the cloud") — this applies to AI services just as it does to any other AWS service.
19. **Name two ways to secure sensitive data used in a gen AI pipeline.** Encrypt data at rest and in transit; apply IAM least-privilege access controls. (Also valid: use Macie to detect/classify PII, or use Guardrails to redact PII in model input/output.)
20. **What business metric would you use to decide if a gen AI feature succeeded (accuracy alone isn't always it — cost, latency, user satisfaction also matter)?** There's no single right answer — the point is picking a metric tied to business value, such as task completion rate, cost per inference, response latency, or user satisfaction/adoption, rather than only model accuracy.
21. **When would a business choose an agent-based architecture over a single prompt-response call?** When the task requires multiple steps, tool use, decision-making, or orchestration across systems (e.g., looking up data, taking an action, then responding) rather than a single self-contained answer.
22. **What's the ML development lifecycle, in order, from data to deployed model?** Data collection → exploratory data analysis (EDA) → feature engineering → model training → evaluation → deployment → monitoring (with feedback looping back to retraining as needed).
