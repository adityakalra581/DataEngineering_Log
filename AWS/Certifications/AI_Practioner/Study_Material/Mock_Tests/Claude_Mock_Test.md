# AWS Certified AI Practitioner (AIF-C01) – Mock Test

**Questions:** 65 | **Suggested time:** 90 minutes | **Passing target:** ~70% (46+ correct)

**Domain coverage (mirrors the official exam weighting):**

| Domain | Topic | Weight | Questions |
|---|---|---|---|
| 1 | Fundamentals of AI and ML | 20% | Q1–Q13 |
| 2 | Fundamentals of Generative AI | 24% | Q14–Q29 |
| 3 | Applications of Foundation Models | 28% | Q30–Q47 |
| 4 | Guidelines for Responsible AI | 14% | Q48–Q56 |
| 5 | Security, Compliance, and Governance for AI | 14% | Q57–Q65 |

Choose **one** answer unless the question says "Choose TWO". Don't peek at the answers section until you finish.

---

## Domain 1: Fundamentals of AI and ML

**Q1.** Which option correctly orders these fields from broadest to narrowest?

A. Machine learning > Artificial intelligence > Deep learning > Generative AI
B. Artificial intelligence > Machine learning > Deep learning > Generative AI
C. Deep learning > Machine learning > Artificial intelligence > Generative AI
D. Generative AI > Deep learning > Machine learning > Artificial intelligence

**Q2.** A real-estate company wants to predict the selling price of a house based on its size, location, and number of bedrooms. What type of ML problem is this?

A. Binary classification
B. Clustering
C. Supervised learning (regression)
D. Reinforcement learning

**Q3.** A retailer has customer purchase data with no labels and wants to discover natural customer segments. Which approach fits best?

A. Unsupervised learning (clustering)
B. Supervised learning (classification)
C. Supervised learning (regression)
D. Reinforcement learning

**Q4.** A robot learns to navigate a warehouse by receiving rewards for reaching targets and penalties for collisions. Which learning paradigm is this?

A. Supervised learning
B. Unsupervised learning
C. Semi-supervised learning
D. Reinforcement learning

**Q5.** A model achieves 99% accuracy on training data but only 70% on validation data. What is the most likely issue?

A. Underfitting
B. Overfitting
C. Data leakage from the test set
D. Low learning rate

**Q6.** A bank builds a fraud-detection model. Missing a fraudulent transaction (false negative) is far more costly than flagging a legitimate one. Which metric should be prioritized?

A. Precision
B. Specificity
C. Recall
D. Accuracy

**Q7.** *(Choose TWO)* Which metrics are appropriate for evaluating a **regression** model?

A. Root Mean Squared Error (RMSE)
B. F1 score
C. AUC-ROC
D. Mean Absolute Error (MAE)
E. Confusion matrix

**Q8.** A business analyst with no coding experience wants to build ML models and generate predictions using a visual interface. Which AWS service should they use?

A. Amazon SageMaker Ground Truth
B. Amazon SageMaker Studio Lab
C. Amazon SageMaker Model Monitor
D. Amazon SageMaker Canvas

**Q9.** A company needs to extract text, tables, and form fields from scanned invoices. Which service is the best fit?

A. Amazon Textract
B. Amazon Rekognition
C. Amazon Comprehend
D. Amazon Translate

**Q10.** A company wants to analyze customer reviews to detect sentiment, key phrases, and named entities. Which service should it use?

A. Amazon Lex
B. Amazon Kendra
C. Amazon Comprehend
D. Amazon Polly

**Q11.** A call center wants to convert recorded customer calls into text for analysis. Which service should it use?

A. Amazon Polly
B. Amazon Transcribe
C. Amazon Lex
D. Amazon Textract

**Q12.** An e-commerce site wants to provide real-time personalized product recommendations. Which AWS service is purpose-built for this?

A. Amazon Kendra
B. Amazon Comprehend
C. Amazon Rekognition
D. Amazon Personalize

**Q13.** A data science team needs to score a very large dataset once a week, with no need for a persistent endpoint or low latency. Which SageMaker inference option is most cost-effective?

A. Batch transform
B. Real-time inference endpoint
C. Serverless inference
D. Multi-model endpoint

---

## Domain 2: Fundamentals of Generative AI

**Q14.** What is a foundation model (FM)?

A. A small model trained for exactly one narrow task
B. A rule-based expert system
C. A large model pre-trained on broad data that can be adapted to many downstream tasks
D. A database of labeled training examples

**Q15.** In the context of LLMs, what is a token?

A. A security credential for calling the model API
B. A basic unit of text (word, sub-word, or character) the model processes
C. A numerical weight in the neural network
D. A single training example

**Q16.** What are embeddings?

A. Compressed backups of model weights
B. Encrypted prompts
C. Rules for formatting model output
D. Numerical vector representations that capture the semantic meaning of data

**Q17.** Which innovation is the core of the transformer architecture used by most modern LLMs?

A. The self-attention mechanism
B. Convolutional filters
C. Recurrent memory cells only
D. Decision-tree ensembles

**Q18.** What is the effect of increasing the **temperature** inference parameter?

A. The model runs faster
B. The response becomes shorter
C. The output becomes more random and creative
D. The output becomes more deterministic

**Q19.** Which inference parameter restricts token sampling to the smallest set of tokens whose cumulative probability exceeds a threshold?

A. Top K
B. Top P
C. Max tokens
D. Stop sequences

**Q20.** An LLM confidently states a fact about a company that is completely fabricated. What is this called?

A. Overfitting
B. Data drift
C. Prompt injection
D. Hallucination

**Q21.** Which type of model generates images by starting from random noise and iteratively removing noise?

A. Diffusion model
B. Recurrent neural network
C. Linear regression
D. K-means

**Q22.** What does the **context window** of an LLM define?

A. The number of users who can call the model at once
B. The geographic Region where the model runs
C. The maximum amount of input and output text (tokens) the model can consider at once
D. The amount of training data used

**Q23.** A model that can accept both text and images as input and reason across them is best described as:

A. Unimodal
B. Multimodal
C. Rule-based
D. Federated

**Q24.** Which task is **least** suited for a generative AI solution?

A. Drafting marketing copy
B. Summarizing meeting transcripts
C. Generating product images
D. Computing exact payroll deductions that must be 100% deterministic and auditable

**Q25.** Which statement best describes Amazon Bedrock?

A. A fully managed service that offers foundation models from multiple providers through a single API
B. A service for labeling training data
C. A service for detecting objects in images
D. A data warehouse service

**Q26.** Which AWS offering is a generative AI assistant that helps developers write, explain, and debug code inside their IDE?

A. Amazon Lex
B. Amazon Q Business
C. Amazon Q Developer
D. Amazon Textract

**Q27.** What is Amazon PartyRock?

A. A tool for labeling audio
B. A no-code playground for building and experimenting with generative AI apps
C. A GPU instance family
D. A vector database

**Q28.** Which service provides a hub of pre-trained models and built-in solutions that can be deployed with a few clicks in SageMaker?

A. Amazon SageMaker Clarify
B. Amazon SageMaker Pipelines
C. Amazon SageMaker Data Wrangler
D. Amazon SageMaker JumpStart

**Q29.** A company deploys a GenAI customer-service chatbot. Which metric best demonstrates **business value**?

A. Reduction in average handling time and improvement in customer satisfaction score
B. Number of model parameters
C. Size of the training dataset
D. GPU utilization

---

## Domain 3: Applications of Foundation Models

**Q30.** A developer includes three example input/output pairs in a prompt to guide the model's format. What technique is this?

A. Zero-shot prompting
B. Fine-tuning
C. Few-shot prompting
D. Continued pre-training

**Q31.** Asking a model to "think step by step" before answering a math word problem is an example of:

A. Chain-of-thought prompting
B. Retrieval-augmented generation
C. Negative prompting
D. Model distillation

**Q32.** Which prompting technique explicitly tells the model what **not** to include in its output?

A. Few-shot prompting
B. Negative prompting
C. Role prompting
D. Zero-shot prompting

**Q33.** What is the primary purpose of Retrieval Augmented Generation (RAG)?

A. To make the model train faster
B. To reduce the model's parameter count
C. To translate languages
D. To ground model responses in external, up-to-date data without retraining the model

**Q34.** Which feature of Amazon Bedrock provides a fully managed RAG workflow (ingestion, embedding, retrieval, and prompt augmentation)?

A. Knowledge Bases for Amazon Bedrock
B. Provisioned Throughput
C. Model invocation logging
D. Amazon Bedrock Guardrails

**Q35.** *(Choose TWO)* Which AWS services can be used as a vector store for embeddings in a RAG application?

A. Amazon OpenSearch Service
B. Amazon Athena
C. Amazon Aurora PostgreSQL with pgvector
D. Amazon CloudFront
E. Amazon Route 53

**Q36.** A company wants a foundation model to consistently adopt its specific tone and domain terminology, and it has a labeled dataset of prompt/response pairs. Which approach is most appropriate?

A. Zero-shot prompting
B. Increasing temperature
C. RAG only
D. Fine-tuning (instruction tuning)

**Q37.** A company has a large corpus of **unlabeled** domain-specific text (e.g., legal documents) and wants the model to better understand that domain. Which customization approach fits?

A. Continued pre-training
B. Few-shot prompting
C. Prompt templates
D. Guardrails

**Q38.** Which Amazon Bedrock capability lets a model break down a user request, call company APIs, and complete multi-step tasks such as booking a trip or processing an order?

A. Bedrock Guardrails
B. Bedrock Model Evaluation
C. Agents for Amazon Bedrock
D. Bedrock Batch inference

**Q39.** When must you purchase **Provisioned Throughput** in Amazon Bedrock?

A. Whenever you use any base model
B. When you want to use a custom (fine-tuned or continued pre-trained) model, or need guaranteed throughput
C. Only when using Amazon Titan models
D. Only for embeddings

**Q40.** Which approach is typically the **lowest cost** to try first when improving FM output quality?

A. Pre-training a model from scratch
B. Continued pre-training
C. Prompt engineering
D. Fine-tuning

**Q41.** A legal team needs to summarize 300-page contracts in a single request. Which model characteristic is most important?

A. Image generation capability
B. A large context window
C. Low training cost
D. Support for speech output

**Q42.** Which metric is commonly used to evaluate the quality of **text summarization** by comparing overlap with reference summaries?

A. ROUGE
B. BLEU
C. RMSE
D. AUC

**Q43.** Which metric is commonly used to evaluate **machine translation** quality?

A. ROUGE
B. F1 score
C. BLEU
D. MAE

**Q44.** Which metric uses contextual embeddings to evaluate the **semantic similarity** of generated text to a reference?

A. BERTScore
B. BLEU
C. Perplexity only
D. Accuracy

**Q45.** A team wants to assess subjective qualities like brand voice, tone, and helpfulness of a chatbot's responses. Which evaluation approach is best?

A. ROUGE scoring
B. Latency measurement
C. Token counting
D. Human evaluation

**Q46.** Which of the following is a benchmark dataset commonly used to evaluate general language understanding of models?

A. ImageNet
B. GLUE
C. MNIST
D. CIFAR-10

**Q47.** Which Amazon model family is designed specifically to convert text into embeddings for semantic search in RAG applications?

A. Amazon Titan Text Embeddings
B. Amazon Polly
C. Amazon Rekognition
D. Amazon Translate

---

## Domain 4: Guidelines for Responsible AI

**Q48.** A hiring model trained mostly on resumes from one demographic performs poorly for other groups. What is the best mitigation?

A. Increase the model's temperature
B. Use more diverse and representative training data and test for bias
C. Remove all logging
D. Switch to a larger context window

**Q49.** Which AWS service helps detect bias in datasets and models and explains model predictions?

A. Amazon SageMaker Clarify
B. Amazon Macie
C. AWS Config
D. Amazon Inspector

**Q50.** Which feature of Amazon Bedrock can block denied topics, filter harmful content, redact PII, and check responses for grounding?

A. Knowledge Bases
B. Guardrails for Amazon Bedrock
C. Agents
D. Provisioned Throughput

**Q51.** A medical imaging company wants human reviewers to validate low-confidence ML predictions before they are acted on. Which service supports this workflow?

A. Amazon SageMaker Canvas
B. Amazon Augmented AI (Amazon A2I)
C. Amazon SageMaker Feature Store
D. AWS Glue

**Q52.** Which SageMaker capability helps document a model's intended use, training data, evaluation results, and risk ratings in one place for governance and transparency?

A. SageMaker Pipelines
B. SageMaker Debugger
C. SageMaker Model Cards
D. SageMaker Neo

**Q53.** What is a typical trade-off between model interpretability and performance?

A. Highly interpretable models (e.g., linear regression, decision trees) may achieve lower performance than complex black-box models
B. Interpretable models are always more accurate
C. There is never any trade-off
D. Black-box models are always easier to explain

**Q54.** Which risk is specific to generative AI models trained on large public datasets?

A. Hardware failure
B. Infringement of intellectual property or copyrighted content in outputs
C. Network latency
D. Disk encryption failure

**Q55.** A public chatbot starts producing offensive and hateful content. What is this problem called, and which is a suitable mitigation?

A. Drift; retrain monthly
B. Overfitting; reduce epochs
C. Underfitting; add layers
D. Toxicity; apply content filters (Guardrails) and curate training data

**Q56.** After deployment, a company wants to continuously detect data drift and bias drift in a production model. Which SageMaker feature should it use?

A. SageMaker Model Monitor
B. SageMaker Ground Truth
C. SageMaker Studio Lab
D. SageMaker JumpStart

---

## Domain 5: Security, Compliance, and Governance for AI

**Q57.** Under the AWS shared responsibility model for Amazon Bedrock, which is the **customer's** responsibility?

A. Physical security of data centers
B. Patching the underlying hypervisor
C. Managing IAM permissions, data classification, and application-level security
D. Maintaining the managed service's network infrastructure

**Q58.** Which principle should be applied when granting IAM permissions to users and applications calling AI services?

A. Least privilege
B. Share a single admin role
C. Use root credentials for automation
D. Grant full access to speed up development

**Q59.** Which AWS service automatically discovers and protects sensitive data such as PII in Amazon S3 buckets, helping before data is used for training?

A. AWS Shield
B. Amazon Macie
C. AWS WAF
D. Amazon GuardDuty

**Q60.** Which combination best protects AI training data at rest and in transit?

A. Plaintext storage with HTTP
B. Encryption with AWS KMS keys at rest and TLS in transit
C. Only security groups
D. Public S3 buckets with obfuscated names

**Q61.** A company wants to call Amazon Bedrock from a VPC **without** traversing the public internet. What should it use?

A. Internet gateway
B. Public NAT gateway only
C. AWS Direct Connect for the public endpoint
D. AWS PrivateLink (VPC interface endpoint)

**Q62.** Which service records API calls made to Amazon Bedrock for auditing and governance?

A. AWS CloudTrail
B. Amazon Route 53
C. AWS Artifact
D. Amazon SES

**Q63.** Where can a customer download AWS compliance reports (such as SOC and ISO) on demand?

A. AWS Trusted Advisor
B. AWS Artifact
C. AWS Cost Explorer
D. Amazon Inspector

**Q64.** An attacker crafts input that tricks an LLM into ignoring its system instructions and revealing confidential data. What is this attack, and which is a good defense?

A. SQL injection; use parameterized queries only
B. DDoS; use AWS Shield only
C. Prompt injection; use input validation, Guardrails, and least-privilege access for tools and data
D. Phishing; use MFA only

**Q65.** A company must demonstrate continuous compliance of its AI workloads to auditors with minimal manual evidence gathering. Which service helps?

A. AWS Audit Manager
B. Amazon Polly
C. Amazon SageMaker Canvas
D. AWS Snowball

---
---

# Answers and Explanations

## Answer key

| Q | Ans | Q | Ans | Q | Ans | Q | Ans | Q | Ans |
|---|---|---|---|---|---|---|---|---|---|
| 1 | B | 14 | C | 27 | B | 40 | C | 53 | A |
| 2 | C | 15 | B | 28 | D | 41 | B | 54 | B |
| 3 | A | 16 | D | 29 | A | 42 | A | 55 | D |
| 4 | D | 17 | A | 30 | C | 43 | C | 56 | A |
| 5 | B | 18 | C | 31 | A | 44 | A | 57 | C |
| 6 | C | 19 | B | 32 | B | 45 | D | 58 | A |
| 7 | A, D | 20 | D | 33 | D | 46 | B | 59 | B |
| 8 | D | 21 | A | 34 | A | 47 | A | 60 | B |
| 9 | A | 22 | C | 35 | A, C | 48 | B | 61 | D |
| 10 | C | 23 | B | 36 | D | 49 | A | 62 | A |
| 11 | B | 24 | D | 37 | A | 50 | B | 63 | B |
| 12 | D | 25 | A | 38 | C | 51 | B | 64 | C |
| 13 | A | 26 | C | 39 | B | 52 | C | 65 | A |

## Explanations

### Domain 1: Fundamentals of AI and ML

1. **B.** AI is the broadest field; ML is a subset of AI, deep learning (neural networks) is a subset of ML, and generative AI is built largely on deep learning.
2. **C.** Predicting a continuous numeric value from labeled historical data is supervised regression.
3. **A.** Unlabeled data and a goal of finding natural groupings points to unsupervised learning, specifically clustering (e.g., k-means).
4. **D.** An agent learning through rewards and penalties from interacting with an environment is reinforcement learning.
5. **B.** Very high training accuracy with much lower validation accuracy means the model memorized the training data (overfitting). Fixes: more data, regularization, simpler model, early stopping.
6. **C.** Recall = TP / (TP + FN). When false negatives are costly (fraud, disease), maximize recall. Precision matters when false positives are costly.
7. **A, D.** RMSE and MAE measure error for numeric predictions. F1, AUC-ROC, and confusion matrix are classification metrics.
8. **D.** SageMaker Canvas is a no-code visual ML tool for business analysts. Ground Truth is for data labeling; Model Monitor is for production monitoring.
9. **A.** Textract extracts text, tables, and forms from scanned documents. Rekognition analyzes images/video; Comprehend does NLP on text.
10. **C.** Comprehend does sentiment analysis, entity recognition, key phrase extraction, and language detection.
11. **B.** Transcribe is speech-to-text. Polly is the reverse (text-to-speech).
12. **D.** Personalize is a managed recommendation service. Kendra is intelligent enterprise search.
13. **A.** Batch transform is for offline, large-scale inference without a persistent endpoint. Real-time endpoints stay running and cost more for this use case.

### Domain 2: Fundamentals of Generative AI

14. **C.** Foundation models are pre-trained on large, broad datasets and adaptable to many tasks via prompting, fine-tuning, or RAG.
15. **B.** Tokens are the units (words, sub-words, characters) that LLMs read and generate. Pricing and context limits are typically measured in tokens.
16. **D.** Embeddings are vectors where semantically similar items are close together. They power semantic search and RAG.
17. **A.** Transformers rely on self-attention to weigh the relevance of different tokens to each other.
18. **C.** Higher temperature flattens the probability distribution, producing more varied/creative output. Lower temperature is more deterministic and focused.
19. **B.** Top P (nucleus sampling) samples from the smallest token set whose cumulative probability exceeds P. Top K limits to the K most likely tokens.
20. **D.** Hallucination is when a model produces plausible-sounding but false or fabricated content. Mitigate with RAG, Guardrails (contextual grounding), and human review.
21. **A.** Diffusion models (e.g., Stable Diffusion) generate images by iterative denoising.
22. **C.** The context window is the total tokens (prompt + response) a model can handle at once.
23. **B.** Multimodal models handle multiple data types (text, images, audio, video).
24. **D.** GenAI is probabilistic, so tasks needing exact, deterministic, auditable computation are better handled by conventional software.
25. **A.** Bedrock is a fully managed service offering FMs from Amazon and third-party providers via one API, with tools for RAG, agents, guardrails, and customization.
26. **C.** Amazon Q Developer (formerly CodeWhisperer features) is the AI coding assistant. Q Business is for enterprise data Q&A.
27. **B.** PartyRock is a Bedrock-powered, no-code playground to build and share GenAI apps.
28. **D.** SageMaker JumpStart is the model hub with pre-trained models, notebooks, and solution templates.
29. **A.** Business metrics tie to outcomes such as efficiency, customer satisfaction, cost savings, and conversion. Parameter count and GPU utilization are technical metrics.

### Domain 3: Applications of Foundation Models

30. **C.** Giving a few examples in the prompt is few-shot prompting. Zero-shot gives no examples.
31. **A.** Chain-of-thought prompting elicits step-by-step reasoning, improving accuracy on complex tasks.
32. **B.** Negative prompting specifies what the model should avoid.
33. **D.** RAG retrieves relevant external documents at query time and adds them to the prompt, reducing hallucination and keeping answers current without retraining.
34. **A.** Knowledge Bases for Amazon Bedrock automates the RAG pipeline. Guardrails handle safety; Provisioned Throughput handles capacity.
35. **A, C.** OpenSearch Service and Aurora PostgreSQL (pgvector) support vector storage and similarity search. (RDS PostgreSQL, DocumentDB, and Neptune Analytics are other options.) Athena, CloudFront, and Route 53 are not vector stores.
36. **D.** Fine-tuning with labeled prompt/response pairs teaches style, format, and domain behavior. RAG supplies knowledge but doesn't change behavior as strongly.
37. **A.** Continued pre-training uses large amounts of unlabeled domain text. Fine-tuning uses labeled data.
38. **C.** Agents orchestrate multi-step tasks and invoke APIs/Lambda functions through action groups.
39. **B.** Custom models in Bedrock require Provisioned Throughput; it also provides guaranteed capacity for production. Base models can be used on-demand (pay per token).
40. **C.** Cost/effort ladder (low → high): prompt engineering → RAG → fine-tuning → continued pre-training → training from scratch.
41. **B.** Long documents require a model with a large enough context window to fit the input.
42. **A.** ROUGE measures n-gram/sequence overlap and is the standard for summarization.
43. **C.** BLEU compares generated translations to reference translations.
44. **A.** BERTScore uses contextual embeddings to measure semantic similarity rather than exact word matches.
45. **D.** Subjective qualities require human evaluation (Bedrock Model Evaluation supports human-based workflows alongside automatic evaluation).
46. **B.** GLUE (and SuperGLUE) evaluate language understanding. ImageNet, MNIST, and CIFAR-10 are image datasets.
47. **A.** Titan Text Embeddings produces vector embeddings for semantic search and RAG. (Cohere Embed models are also available in Bedrock.)

### Domain 4: Responsible AI

48. **B.** Unrepresentative data causes bias. Use diverse, balanced datasets and test fairness across groups.
49. **A.** SageMaker Clarify detects pre- and post-training bias and provides feature-attribution explainability.
50. **B.** Guardrails offer content filters, denied topics, word filters, sensitive-information (PII) filters, and contextual grounding checks.
51. **B.** Amazon A2I adds human review workflows for ML predictions.
52. **C.** Model Cards centralize documentation for transparency and governance.
53. **A.** Simpler models are easier to interpret but may be less accurate on complex tasks; complex models perform better but are harder to explain.
54. **B.** GenAI outputs can reproduce copyrighted material or raise IP concerns. Other GenAI risks: hallucination, toxicity, bias, privacy leakage.
55. **D.** Toxicity (hateful/offensive output) is mitigated with content filters, Guardrails, curated training data, and red-teaming.
56. **A.** Model Monitor tracks data quality, model quality, bias drift, and feature attribution drift in production. Ground Truth is for labeling.

### Domain 5: Security, Compliance, and Governance

57. **C.** AWS secures the infrastructure ("security **of** the cloud"); customers manage access, data, and application configuration ("security **in** the cloud"). Bedrock does not use customer prompts/data to train base models.
58. **A.** Least privilege: grant only the permissions needed, using IAM roles and policies.
59. **B.** Macie uses ML to discover and classify sensitive data (PII) in S3.
60. **B.** Use KMS-managed encryption at rest and TLS in transit.
61. **D.** AWS PrivateLink creates VPC interface endpoints to reach Bedrock privately.
62. **A.** CloudTrail logs API activity. Bedrock model invocation logging (to CloudWatch Logs or S3) captures prompts and responses.
63. **B.** AWS Artifact provides on-demand access to compliance reports and agreements.
64. **C.** This is prompt injection (a top OWASP risk for LLM apps). Defenses: input validation/sanitization, Guardrails, limiting tool and data permissions, and output filtering.
65. **A.** AWS Audit Manager continuously collects evidence and maps it to compliance frameworks. (AWS Config tracks resource configuration compliance.)

---

## Score yourself

| Score | Readiness |
|---|---|
| 55–65 | Exam ready |
| 46–54 | Close; review weak domains |
| Below 46 | Revisit study material by domain |

Tip: note which **domain** your wrong answers cluster in and focus your revision there.
