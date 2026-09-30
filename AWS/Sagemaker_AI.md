# AWS SageMaker AI (AIF-C01 / AI Practitioner) - Revision Notes

## 1. What is SageMaker?

**Amazon SageMaker = AWS's end-to-end Machine Learning platform**

Use SageMaker when you need to:

- Build ML models
- Train ML models
- Tune hyperparameters
- Deploy models
- Monitor models
- Manage ML lifecycle

### Exam Tip

✅ **Custom model on proprietary/company data → SageMaker**

✅ **Need to train/fine-tune your own ML model → SageMaker**

❌ Not Bedrock if the question is primarily about building traditional/custom ML models.

### SageMaker Lifecycle

```text
Prepare Data
    ↓
Feature Engineering
    ↓
Train Model
    ↓
Tune Model
    ↓
Evaluate Model
    ↓
Deploy Model
    ↓
Monitor Model
```

---

# 2. SageMaker Studio

## What is it?

**Unified IDE for Machine Learning**

Provides:

- Notebooks
- Data preparation
- Model training
- Deployment
- Monitoring

### Exam Keyword

> "Single interface for entire ML workflow"

Answer → **SageMaker Studio**

---

# 3. SageMaker Autopilot

## What is it?

AWS AutoML service.

Automatically:

- Cleans data
- Selects algorithms
- Trains multiple models
- Tunes parameters
- Selects best model

### Use Case

Business user wants ML model but lacks ML expertise.

### Exam Clue

> "Automatically build best model"

Answer → **SageMaker Autopilot**

---

# 4. Automatic Model Tuning (AMT)

## What is it?

Hyperparameter optimization.

Automatically searches for:

- Learning rate
- Batch size
- Epochs
- Tree depth

to find best performing model.

### Exam Clue

> "Improve model accuracy by testing different hyperparameter combinations"

Answer → **Automatic Model Tuning (AMT)**

---

# 5. SageMaker Built-In Algorithms

Pre-built algorithms provided by AWS.

Examples:

### Classification

- XGBoost
- Linear Learner

### Clustering

- K-Means

### Forecasting

- DeepAR

### Recommendation

- Factorization Machines

### NLP

- BlazingText

### Exam Clue

> Want quick training without custom coding

Answer → Built-in algorithms.

---

# 6. SageMaker Data Wrangler

## Purpose

Data preparation and feature engineering.

### Capabilities

- Import data
- Clean data
- Transform data
- Visualize data
- Create ML features

### Exam Clue

> "Prepare data before training"

Answer → **Data Wrangler**

---

# 7. SageMaker Feature Store

## What is a Feature?

Feature = Model input variable.

Examples:

- Age
- Salary
- Click rate
- Purchase frequency

## Purpose

Central repository for storing and reusing ML features.

Benefits:

- Consistency
- Reusability
- Governance
- Versioning

### Exam Clue

> "Store engineered features centrally for multiple teams/models"

Answer → **Feature Store**

---

# 8. SageMaker Clarify

## Purpose

Responsible AI

### Capabilities

#### Bias Detection

Checks for:

- Gender bias
- Race bias
- Sampling bias

#### Explainability

Explains why model made prediction.

Example:

```text
Loan Rejected
70% due to Income
20% due to Credit Score
10% due to Debt
```

### Exam Clue

> Detect bias

OR

> Explain model decisions

Answer → **SageMaker Clarify**

---

# 9. SageMaker Ground Truth

## Purpose

Human labeling service.

Used for:

- Image labeling
- Text labeling
- Video labeling

Training data creation.

### RLHF Connection

Humans provide feedback on model outputs.

RLHF = Reinforcement Learning from Human Feedback.

### Ground Truth Plus

AWS-managed data-labeling workforce.

AWS handles:

- Labelers
- Workflow
- Quality control

### Exam Clue

> Human labeling of training data

Answer → Ground Truth

---

# 10. SageMaker Model Monitor

## Purpose

Monitor deployed models.

Detects:

- Data drift
- Concept drift
- Quality issues

### Example

Model trained on 2024 customer data.

2026 customer behavior changes.

Model Monitor detects degradation.

### Exam Clue

> Monitor prediction quality after deployment

Answer → Model Monitor

---

# 11. SageMaker Governance Services

---

## Model Cards

Model documentation.

Includes:

- Purpose
- Limitations
- Metrics
- Risk assessment

### Think

ML Report Card.

---

## Model Dashboard

Single pane to view:

- Models
- Monitoring status
- Compliance status

### Think

ML Control Center.

---

## Model Registry

Central repository for model versions.

Stores:

```text
v1
v2
v3
```

Supports approvals.

### Exam Clue

> Manage versions of ML models

Answer → Model Registry

---

## Role Manager

Simplifies IAM permissions.

### Exam Clue

> Manage ML access permissions

Answer → Role Manager

---

# 12. SageMaker Pipelines

## Purpose

CI/CD for Machine Learning.

Automates ML workflows.

### Key Steps (Very Important)

```text
Data Preparation
    ↓
Feature Engineering
    ↓
Training
    ↓
Evaluation
    ↓
Approval
    ↓
Registration
    ↓
Deployment
    ↓
Monitoring
```

### Exam Clue

> Automate entire ML lifecycle

Answer → SageMaker Pipelines

---

# 13. SageMaker Deployment / Inference Types

This is heavily tested.

---

## Real-Time Inference

Low latency predictions.

Examples:

- Fraud detection
- Recommendation engines

### Use When

Need immediate response.

---

## Serverless Inference

AWS manages infrastructure.

### Use When

- Sporadic traffic
- Cost optimization

---

## Batch Inference

Process large dataset offline.

### Examples

- Monthly predictions
- Risk scoring

---

## Asynchronous Inference

Large requests.

Long-running predictions.

### Examples

- Huge documents
- Large image processing

---

## Multi-Model Endpoints

Multiple models on single endpoint.

### Use When

Large number of smaller models.

---

### Quick Exam Table

| Requirement | Choice |
|---|---|
| Immediate response | Real-Time |
| No infrastructure management | Serverless |
| Millions of predictions offline | Batch |
| Long-running requests | Async |
| Multiple models on one endpoint | Multi-model |

---

# 14. SageMaker JumpStart

## Purpose

Model Hub

Provides:

- Foundation models
- Pre-trained models
- Solution templates

### Think

AWS Marketplace for ML.

### Use Cases

- Start quickly
- Deploy prebuilt models
- Explore FMs

### Exam Clue

> Pretrained model catalog

Answer → JumpStart

---

# 15. SageMaker Canvas

## Purpose

No-code Machine Learning.

Target Users:

- Business Analysts
- Product Managers
- Non-technical users

Capabilities:

- Upload CSV
- Build model
- Generate predictions

No Python required.

### Exam Clue

> Create ML model without coding

Answer → Canvas

---

# 16. MLflow on SageMaker

Used for:

- Experiment tracking
- Run tracking
- Model tracking

Popular for MLOps teams.

### Exam Clue

> Need experiment management and tracking

Answer → MLflow on SageMaker

---

# 17. Network Isolation Mode

## Purpose

Extra security.

Training containers:

- No internet access
- No external communication

### Use Case

Sensitive workloads.

### Exam Clue

> Highly regulated environment

Answer → Network Isolation

---

# 18. DeepAR

## Purpose

Time-series forecasting algorithm.

Examples:

- Sales forecasting
- Demand forecasting
- Inventory predictions

### Exam Clue

> Forecast future values using historical time series

Answer → DeepAR

---

# 19. SageMaker vs Bedrock

Most Important Exam Comparison

| Feature | SageMaker | Bedrock |
|---|---|---|
| Custom ML models | ✅ | ❌ |
| Train models from scratch | ✅ | ❌ |
| Full ML lifecycle | ✅ | ❌ |
| Feature engineering | ✅ | ❌ |
| Traditional ML | ✅ | ❌ |
| Foundation Models | Limited | ✅ |
| GenAI APIs | ❌ | ✅ |
| Claude/Llama access | ❌ | ✅ |

### Easy Memory Trick

**Build ML → SageMaker**

**Use Foundation Models → Bedrock**

---

# 20. High-Probability Exam Questions

### Q1

Need a no-code ML tool?

✅ SageMaker Canvas

---

### Q2

Need automated ML model creation?

✅ SageMaker Autopilot

---

### Q3

Need feature reuse across teams?

✅ Feature Store

---

### Q4

Need detect bias?

✅ Clarify

---

### Q5

Need human data labeling?

✅ Ground Truth

---

### Q6

Need model version control?

✅ Model Registry

---

### Q7

Need ML CI/CD?

✅ Pipelines

---

### Q8

Need pretrained solution/model catalogue?

✅ JumpStart

---

### Q9

Need hyperparameter tuning?

✅ Automatic Model Tuning (AMT)

---

### Q10

Need monitoring after deployment?

✅ Model Monitor

---

# 1-Minute Pre-Exam Memory Dump

```text
Studio = ML IDE

Autopilot = AutoML

AMT = Hyperparameter Tuning

Data Wrangler = Data Preparation

Feature Store = Reusable Features

Clarify = Bias + Explainability

Ground Truth = Human Labeling / RLHF

Model Monitor = Drift Detection

Model Registry = Versioning

Model Cards = Documentation

Model Dashboard = Model Overview

Role Manager = Permissions

Pipelines = ML CI/CD

JumpStart = Model Hub

Canvas = No-Code ML

DeepAR = Forecasting

Network Isolation = Secure Training

Real-time = Instant Prediction
Batch = Offline Prediction
Serverless = No Servers
Async = Long-running Requests

Custom ML → SageMaker
Foundation Models → Bedrock
```
