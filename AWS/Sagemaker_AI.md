# Amazon SageMaker — Certification Notes

SageMaker is AWS's end-to-end **ML platform** (the "build your own model" tier, distinct from pre-trained AI Services like Rekognition/Comprehend). It covers the full ML lifecycle: prepare data → build → train → tune → deploy → monitor → govern.

## Core Platform
- **SageMaker (AI)**: the umbrella service. Fully managed infrastructure for training and hosting ML models at any scale — you bring the data/algorithm, AWS manages the compute.
- **SageMaker Studio**: the unified, web-based IDE for SageMaker — launch notebooks, run experiments, debug training jobs, and monitor deployed models all from one interface, instead of jumping between separate consoles.

## Data Preparation & Features
- **SageMaker Data Wrangler**: visual, low-code tool to import, explore, clean, and transform datasets, and engineer features — shortens the "data prep" phase of the lifecycle. Exports prepared data into downstream SageMaker workflows (training jobs, Feature Store, Pipelines).
- **SageMaker Feature Store**: a central repository to store, share, and reuse engineered features across teams/models — avoids every team re-building the same feature from scratch, and keeps training-time and inference-time features consistent (avoids "training-serving skew").

## Training & Tuning
- **SageMaker Automatic Model Tuning (AMT)**: automatically searches for the best hyperparameters for your model, using Bayesian optimization (by default) to intelligently pick the next combination to try rather than brute-forcing every option. Works with built-in, marketplace, and custom algorithms.
- **Built-in algorithms**: SageMaker ships pre-implemented, optimized algorithms so you don't have to write model code from scratch — e.g., XGBoost (tabular data), Linear Learner (regression/classification), K-Means (clustering), **DeepAR** (time-series forecasting — worth knowing by name, it's the algorithm behind demand/sales-type forecasting use cases), BlazingText (text classification/word embeddings), Image Classification, Object Detection. Exam angle: recognize that using a built-in algorithm means AWS optimized it for SageMaker's distributed training — you still choose and configure it, but don't write it yourself.
- **What counts as "normal SageMaker"**: when people just say "SageMaker" without a suffix, they usually mean this core train/tune/deploy workflow — write or select an algorithm, point it at training data in S3, launch a training job on managed infrastructure, and get a model artifact out. Everything else in this file is a *named sub-feature* layered on top of that core flow.

## Deployment & Inference
Four deployment/inference options, chosen by latency and traffic pattern:
| Type | Use case | Key trait |
|---|---|---|
| **Real-time inference** | Low-latency, interactive predictions (e.g., a live API) | Persistent endpoint, always-on, supports autoscaling |
| **Batch transform** | Large offline scoring jobs, no live endpoint needed | Processes a full dataset at once, then shuts down |
| **Asynchronous inference** | Large payloads / long preprocessing, near-real-time | Queues requests, good for inputs too big/slow for real-time limits |
| **Serverless inference** | Spiky or low/intermittent traffic | No infrastructure to manage, scales to zero, avoids paying for idle capacity |

Exam pattern: "unpredictable, bursty traffic, don't want to manage instance sizing" → Serverless inference. "Score a 10M-row file overnight" → Batch transform.

## Explainability, Bias & Human Feedback
- **SageMaker Clarify**: detects bias in training data and model predictions, and explains *why* a model made a given prediction (feature attribution). Also used to compare models against each other. This is the go-to answer for any "explainability" or "detect bias" exam question.
- **SageMaker Ground Truth**: managed data-labeling service — combines automated labeling with human labelers (including your own workforce, Mechanical Turk, or vendor workforces) to create high-quality labeled training datasets. Also supports **RLHF (Reinforcement Learning from Human Feedback)** workflows, where humans rank/grade model outputs to fine-tune model behavior (relevant to generative AI model alignment, not just classic labeling).
- **SageMaker Ground Truth Plus**: a turnkey, fully-managed *version* of Ground Truth — AWS's own expert workforce handles the labeling project end-to-end (you don't build the labeling workflow or manage labelers yourself), at the cost of less direct control. Think of it as "Ground Truth, but AWS runs it for you."

## ML Governance
- **SageMaker Model Cards**: structured documentation for a model — intended use, training details, performance, risk considerations — for transparency and audit purposes.
- **SageMaker Model Dashboard**: a single-pane view of all your deployed models, their monitoring status, and alerts, across the account.
- **SageMaker Model Monitor**: continuously monitors deployed models in production for data drift, model quality drift, bias drift, and feature attribution drift, with alerting — the answer whenever a question asks about detecting *production* model degradation over time.
- **SageMaker Model Registry**: centralized catalog to register, version, and manage the lifecycle (e.g., approve for production) of trained model artifacts — the ML equivalent of an artifact/version repository.
- **SageMaker Role Manager**: simplifies creating and managing IAM roles/permissions scoped specifically to ML personas (e.g., data scientist vs. MLOps engineer) rather than hand-writing IAM policies for each.

## Pipelines (MLOps / CI-CD for ML)
- **SageMaker Pipelines**: defines and automates a repeatable ML workflow as a series of connected steps — essentially CI/CD for machine learning.
- Typical step order (know the *flow*, not just the names — exams may ask "what comes next"):
  1. **Data processing** (cleaning/feature engineering, e.g., via a Processing step using Data Wrangler-prepared logic)
  2. **Training** (a Training step runs the algorithm on prepared data)
  3. **Tuning** (optional — a Tuning step runs AMT if hyperparameter search is needed)
  4. **Evaluation** (a Processing/evaluation step checks model quality metrics)
  5. **Conditional check** (a Condition step gates progress — e.g., only proceed if accuracy exceeds a threshold)
  6. **Register model** (push the approved model into Model Registry)
  7. **Deploy** (create/update the inference endpoint)
- Pipelines are what turns the manual "train → tune → deploy" flow into something repeatable and automatic on new data.

## Pre-built Models & No-Code Tools
- **SageMaker JumpStart**: a hub of pre-built, pre-trained models and ready-made ML solutions (including some foundation models) that you can deploy or fine-tune with minimal setup — a shortcut when you don't need to build a model from zero. Overlaps conceptually with Bedrock for FM access, but JumpStart is the SageMaker-native model hub, offering a wider range including traditional ML solution templates, not just generative FMs.
- **SageMaker Canvas**: no-code, visual interface for building ML models and generating predictions — aimed at business analysts, not just data scientists. Supports **MLflow on SageMaker Canvas** — meaning Canvas can use MLflow (an open-source ML lifecycle tool) to track experiments/models under the hood, giving no-code users the same experiment-tracking rigor as code-first workflows.
- **MLflow on SageMaker**: SageMaker offers managed MLflow tracking servers, so teams can use the popular open-source MLflow tool (experiment tracking, model registry) natively on AWS infrastructure instead of self-hosting it.

## Extra / Security Features
- **Network Isolation Mode**: a security setting that prevents a training or inference container from making any outbound network calls — used to guarantee a job can't exfiltrate data or reach the internet, important in regulated/sensitive-data scenarios. Exam pattern: "ensure a training job cannot access the internet or external resources" → Network Isolation Mode.

## Quick Revision Summary (matches AWS's own phrasing style)
- SageMaker: end-to-end ML service
- Automatic Model Tuning: tune hyperparameters (Bayesian optimization)
- Deployment & Inference: real-time, serverless, batch, async
- Studio: unified interface for SageMaker
- Data Wrangler: explore and prepare datasets, create features
- Feature Store: store features/metadata in a central place
- Clarify: compare models, explain model outputs, detect bias
- Ground Truth: RLHF, humans for model grading and data labeling (Ground Truth Plus = AWS-managed turnkey version)
- Model Cards: ML model documentation
- Model Dashboard: view all your models in one place
- Model Monitor: monitoring and alerts for your model
- Model Registry: centralized repository to manage ML model versions
- Pipelines: CI/CD for machine learning
- Role Manager: access control
- JumpStart: ML model hub & pre-built ML solutions
- Canvas: no-code interface for SageMaker
- MLflow on SageMaker: use MLflow tracking servers on AWS
- Network Isolation Mode: block outbound network access for a job
- DeepAR: built-in algorithm for time-series forecasting

---

## AI Practitioner Revision — Key SageMaker Bullets
- SageMaker = **ML Services** tier (build custom models); AI Services (Rekognition, Comprehend, etc.) = pre-trained, no-ML-expertise tier. Exam loves this contrast.
- "Need a custom model on proprietary structured data" → SageMaker, not Bedrock.
- "No ML expertise on the team, need predictions fast" → a pre-built AI service, not SageMaker.
- Clarify = bias + explainability. Model Monitor = ongoing production drift detection. Don't mix these two up.
- Ground Truth = data labeling (+ RLHF); Ground Truth Plus = AWS runs the labeling project for you.
- Four inference types: real-time (low latency), batch (large offline), async (large payload/slow preprocessing), serverless (spiky/idle traffic).
- JumpStart = pre-built models/solutions hub (fastest path to "just deploy something that already works").
- Canvas = no-code, built for business users, not engineers.
- Pipelines = automate/repeat the ML workflow (the MLOps/CI-CD layer).
- Network Isolation Mode = security feature to block a job's outbound network access.
