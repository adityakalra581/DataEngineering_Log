# ML & Generative AI Fundamentals: AIF-C01 Study Guide
Companion to the AWS services guide. Covers **Domain 1 (AI/ML fundamentals)**, **Domain 2 (GenAI fundamentals)**, **Domain 3 (Applications of FMs, incl. fine-tuning)** and the responsible-AI dimensions. All original notes are kept.

**Legend**
- 🆕 **Added**: not in your original notes
- ✏️ **Corrected / clarified**
- ⚠️ **Verify** against the official exam guide / AWS console (names and availability change)

## Table of Contents
1. [The big picture: three axes (fixes the "which category?" confusion)](#1-the-big-picture-three-axes)
2. [The 5 learning types](#2-the-5-learning-types)
3. [Algorithm cheat sheet: CNN, RNN, SVM, K-means & friends](#3-algorithm-cheat-sheet)
4. [Neural network families in simple terms](#4-neural-network-families-in-simple-terms)
5. [Other ML terms](#5-other-ml-terms)
6. [Model fit and bias vs. variance](#6-model-fit-and-bias-vs-variance)
7. [Evaluation metrics](#7-evaluation-metrics)
8. [ML lifecycle, parameters, hyperparameters](#8-ml-lifecycle-parameters-hyperparameters)
9. [Foundation models and LLMs](#9-foundation-models-and-llms)
10. [Fine-tuning: what, why, when, how](#10-fine-tuning-what-why-when-how)
11. [Transformers and self-attention](#11-transformers-and-self-attention)
12. [Latent space](#12-latent-space)
13. [Diffusion models](#13-diffusion-models)
14. [VAE](#14-vae-variational-autoencoder)
15. [GAN](#15-gan-generative-adversarial-network)
16. [Generative model families compared](#16-generative-model-families-compared)
17. [Inference parameters: temperature, Top K, Top P](#17-inference-parameters)
18. [Evaluating generated text: ROUGE, BLEU, BERTScore](#18-evaluating-generated-text)
19. [Amazon Bedrock pricing modes and Regions](#19-amazon-bedrock-pricing-modes-and-regions)
20. [AWS services mapping](#20-aws-services-mapping)
21. [Responsible AI dimensions](#21-responsible-ai-dimensions)
22. [Scenario → answer tables and exam traps](#22-scenario--answer-tables-and-exam-traps)
23. [Sample questions](#23-sample-questions)
24. [Flashcards](#24-flashcards)

---

## 1. The big picture: three axes
🆕 *This is the key to never mixing categories again. Most confusion comes from treating three different things as one list.*

| Axis | Question it answers | Examples |
|---|---|---|
| **A. Learning type** | *How does the model get feedback / what data does it learn from?* | Supervised, unsupervised, semi-supervised, self-supervised, reinforcement |
| **B. Task** | *What problem is being solved?* | Classification, regression, clustering, anomaly detection, dimensionality reduction, generation |
| **C. Algorithm / architecture** | *What is the model itself?* | Linear regression, SVM, K-means, CNN, RNN, Transformer, GAN |

**How they combine**
- An **algorithm** usually belongs to one or two **learning types** and solves certain **tasks**.
- **CNN and RNN are architectures, not learning types.** They are *neural networks* that are trained using a learning type. Typically **supervised** (labeled images/sequences), but a CNN/RNN/Transformer can also be self-supervised or unsupervised (e.g., autoencoders).
- **Names can mislead:** *Logistic regression* does **classification**, not regression. *K-nearest neighbors (KNN)* is **supervised**; *K-means* is **unsupervised**. Same "K", different families.

**30-second decision rule**
1. Are there **labels** (correct answers)? → **Supervised** (category = classification, number = regression).
2. **No labels**, finding structure? → **Unsupervised** (groups = clustering, simplify = dimensionality reduction, oddities = anomaly detection).
3. **Few labels + lots of unlabeled**? → **Semi-supervised**.
4. **Model makes its own labels from the data** (predict the next word / masked part)? → **Self-supervised**.
5. **Agent + environment + rewards**? → **Reinforcement learning**.

---

## 2. The 5 learning types

| Type | Labels? | How it works | Use case |
|---|---|---|---|
| **Supervised** | Full labels on every example | Learns to map input → known correct output | Spam detection, price prediction, fraud classification |
| **Unsupervised** | None | Finds structure/patterns with no "correct answer" to check against | Customer segmentation, anomaly detection, dimensionality reduction |
| **Semi-supervised** | Small labeled set + large unlabeled set | Uses the few labels as an anchor; leverages structure in unlabeled data to generalize further | Medical imaging (expert-labeled scans are scarce, raw scans are plentiful) |
| **Self-supervised** | None provided; model generates its own labels from the data | Pretext tasks: predict a masked/hidden part of the input from the rest | Pre-training LLMs (predict next word), pre-training vision models (predict masked image patch) |
| **Reinforcement** | None; uses a reward signal | Agent learns a policy via trial-and-error with an environment to maximize cumulative reward | Game-playing agents, robotics control, resource allocation optimization |

**Overview by goal**

| Technique | Data | Goal |
|---|---|---|
| Supervised | Fully labeled | Predict a known output |
| Unsupervised | No labels | Find hidden structure |
| Semi-supervised | Small labeled + large unlabeled | Good accuracy without labeling everything |
| Self-supervised | Unlabeled (labels generated from the data itself) | Learn representations; how LLMs are pre-trained |
| Reinforcement | No dataset; an agent interacts with an environment | Maximize cumulative reward |

**Quick distinctions exams probe**
- **Unsupervised vs. self-supervised:** unsupervised has no labels at all and no pretext task; self-supervised manufactures pseudo-labels from the data's own structure.
- **Semi-supervised vs. supervised:** semi-supervised is the answer when the question says "labeling is expensive/limited."
- **Reinforcement** is the odd one out: it's not about labels, it's about reward/penalty feedback over a sequence of actions.

### 2.1 Supervised learning
Trains on **labeled** data (input + correct output) to predict outputs for new inputs.

**Classification (predict a category)**
- **Binary:** spam vs. not spam; loan default yes/no; tumor malignant/benign.
- **Multi-class:** classify a support ticket as billing, technical or sales; identify a flower species.
- **Common algorithms:** logistic regression, decision trees, random forest, XGBoost, SVM.

**Regression (predict a number)**
- House price from size and location; next month's sales or tomorrow's temperature; delivery time.
- **Common algorithms:** linear regression, XGBoost, random forest.

**Neural networks (a more complex supervised technique)**
Stacked layers of mathematical transformations whose weights are adjusted during training.
- Predicting a digit from a handwritten image (MNIST); image classification (cat vs. dog); speech recognition.
- **Deep learning** = neural networks with many layers. Neural networks can also be used unsupervised or self-supervised.

**AWS mapping:** SageMaker XGBoost, Linear Learner, Image Classification; Amazon Rekognition Custom Labels.

### 2.2 Unsupervised learning
Finds patterns in **unlabeled** data. There is no "right answer" to learn from.

**Clustering:** groups similar data points.
- Customer segmentation by purchase behavior; identifying types of network traffic to flag potential security incidents; grouping news articles by topic.
- **Algorithms:** k-means, hierarchical clustering, DBSCAN.

**Dimensionality reduction:** reduces the number of features while keeping important information. Often used to preprocess data for other ML tasks and to cut cost and complexity.
- Compressing hundreds of sensor readings into a few key components; visualizing high-dimensional data in 2D.
- ✏️ Original note said "blurring or cropping background features in an image recognition app": that is closer to *feature selection / preprocessing* than dimensionality reduction. Kept for completeness; for the exam think "fewer features, same information."
- **Algorithms:** PCA, t-SNE, autoencoders.

**Anomaly detection:** finds data points that don't fit the normal pattern.
- Unusual server metrics or a sudden spike in API calls; faulty equipment from sensor data.
- **Algorithm:** Random Cut Forest.

**Association rule learning:** finds relationships between items.
- Market-basket analysis: "customers who buy bread often buy butter."

**AWS mapping:** SageMaker K-Means, PCA, Random Cut Forest; Amazon Lookout for Metrics (anomaly detection; ⚠️ AWS has been retiring Lookout services, so verify status).

### 2.3 Semi-supervised learning
**Small labeled set + large unlabeled set.** Useful when labeling is expensive or slow.
- **Fraud identification:** only a subset of transactions is confirmed as fraud by experts. The solution trains on the unlabeled data first, then refines with the labeled data for better accuracy.
- **Sentiment analysis:** labeling sentiment across every customer channel isn't cost-effective. Train on the large unlabeled portion first, then on a labeled sample, to gain confidence in overall sentiment.
- **Medical imaging:** few scans annotated by radiologists, thousands unlabeled.
- **Speech recognition:** small amount of transcribed audio + large archive of untranscribed audio.
- 🆕 Common technique: **pseudo-labeling** (model labels the unlabeled data with its own confident predictions, then retrains).

**AWS mapping:** SageMaker Ground Truth can label a subset and use **active learning** to auto-label the rest.

### 2.4 Self-supervised learning
The model creates its own labels from the data's structure. No human labeling needed.
- **LLMs:** predict the next token or a masked word (GPT-style, BERT-style).
- **Image models:** predict missing patches of an image.
- The foundation of **pre-training for foundation models**, later adapted with fine-tuning or prompting.

### 2.5 Reinforcement learning (RL)
An **agent** takes **actions** in an **environment**, receives **rewards or penalties**, and learns a **policy** that maximizes long-term reward.
- Robot navigating a warehouse (reward for reaching the target, penalty for collisions); chess/Go agents; dynamic pricing, ad bidding, traffic-signal optimization.
- **RLHF (Reinforcement Learning from Human Feedback):** human preferences train a **reward model** that aligns an LLM's responses to be more helpful and safe.

**AWS mapping:** SageMaker RL; **AWS DeepRacer** (hands-on RL learning tool).

---

## 3. Algorithm cheat sheet
🆕 *The section you asked for. Each algorithm: plain-English idea → learning type → task → use case.*

### 3.1 Master table

| Algorithm | Simple idea | Learning type | Task | Typical use case |
|---|---|---|---|---|
| **Linear regression** | Fit a straight line through data to predict a number | Supervised | Regression | House price, sales forecast |
| **Logistic regression** | Fit a curve that outputs a probability (yes/no). *Name says regression, but it classifies* | Supervised | Classification | Spam, churn, loan default |
| **Decision tree** | A flowchart of yes/no questions leading to an answer | Supervised | Classification & regression | Loan approval rules, easy-to-explain decisions |
| **Random forest** | Many decision trees vote; averaging reduces overfitting (**bagging**) | Supervised | Classification & regression | Fraud, general tabular data |
| **XGBoost (gradient boosting)** | Trees built one after another, each fixing the previous one's mistakes (**boosting**) | Supervised | Classification & regression | Structured/tabular data winner; SageMaker built-in |
| **SVM (Support Vector Machine)** | Draw the best boundary line/plane that separates classes with the **widest margin**; kernels handle curved boundaries | Supervised | Classification (also regression) | Text/email classification, image & bio-data classification, smaller datasets |
| **KNN (K-Nearest Neighbors)** | "You are like your closest neighbors": label a point by the majority of its K nearest labeled points | Supervised | Classification & regression | Recommendations, simple pattern matching |
| **Naive Bayes** | Uses probabilities, assuming features are independent | Supervised | Classification | Spam filtering, sentiment, document classification |
| **K-means** | Pick K groups; assign each point to the nearest center; move centers; repeat | **Unsupervised** | Clustering | Customer segmentation |
| **Hierarchical clustering** | Build a tree of clusters by merging/splitting the closest groups | Unsupervised | Clustering | Taxonomy, gene grouping |
| **DBSCAN** | Groups dense regions; sparse points become noise/outliers | Unsupervised | Clustering (+ outliers) | Irregular-shape clusters, geo data |
| **PCA** | Squash many features into a few "principal components" that keep most variance | Unsupervised | Dimensionality reduction | Compress sensor data, preprocessing, visualization |
| **t-SNE** | Project high-dimensional data to 2D/3D for viewing | Unsupervised | Visualization / dim. reduction | Visualizing embeddings |
| **Random Cut Forest** | Randomly splits data; points isolated quickly are anomalies | Unsupervised | Anomaly detection | Server metrics, API spikes (SageMaker built-in) |
| **Apriori / association rules** | Find items that frequently occur together | Unsupervised | Association | Market-basket analysis |
| **Autoencoder** | Compress input to a small code, then rebuild it; bad rebuilds = anomalies | Unsupervised / self-supervised | Compression, denoising, anomaly detection | Sensor anomaly detection |
| **Neural network / MLP** | Layers of weighted nodes learn complex patterns | Mostly supervised (can be any) | Classification & regression | Digit recognition, general prediction |
| **CNN** | Neural net with **filters that slide over an image** to spot edges → shapes → objects | Mostly **supervised** (also self-supervised pre-training) | Image classification, object detection, segmentation | Medical imaging, face/object recognition, defect inspection |
| **RNN / LSTM / GRU** | Neural net with **memory** that reads a sequence step by step | Mostly **supervised** (also self-supervised for language) | Sequence tasks: forecasting, text, speech | Time-series forecasting, speech recognition, older translation models |
| **Transformer** | Reads all tokens at once using **self-attention** | **Self-supervised** pre-training, then supervised/RL fine-tuning | Language, vision, multimodal, generation | LLMs (Claude, Llama, GPT), BERT |
| **GAN** | Generator vs. discriminator "forger vs. detective" | Unsupervised (generative; adversarial training) | Generation | Realistic images, synthetic data |
| **VAE** | Encode to a smooth probabilistic latent space and decode | Unsupervised (generative) | Generation, anomaly detection | Variations, synthetic data |
| **Diffusion model** | Learn to remove noise step by step | Self-supervised-style training (generative) | Image/video/audio generation | Stable Diffusion, Titan Image, Nova Canvas |
| **Q-learning / DQN / PPO** | Agent learns which action earns the most reward | **Reinforcement** | Sequential decision-making | Robotics, games, pricing |

### 3.2 Grouped by learning type (memorize this layout)

**Supervised** → linear regression, logistic regression, decision tree, random forest, XGBoost, **SVM**, **KNN**, Naive Bayes, neural networks (**CNN**, **RNN** usually trained this way)

**Unsupervised** → **K-means**, hierarchical clustering, DBSCAN, PCA, t-SNE, Random Cut Forest, Apriori, autoencoder, (GAN/VAE as generative)

**Semi-supervised** → not an algorithm family: it's a *setup* (pseudo-labeling, label propagation; Ground Truth active learning)

**Self-supervised** → Transformer pre-training (next-token / masked-token), masked image models, contrastive learning

**Reinforcement** → Q-learning, DQN, policy gradients/PPO, RLHF

### 3.3 Classical ML vs. deep learning 🆕
| | Classical ML | Deep learning |
|---|---|---|
| Examples | Linear/logistic regression, trees, SVM, KNN, K-means, XGBoost | CNN, RNN, Transformer, GAN, diffusion |
| Best data | Structured/tabular, smaller datasets | Unstructured (images, text, audio), very large datasets |
| Feature engineering | Often manual | Learned automatically |
| Compute | Modest (CPU often fine) | Heavy (GPU / Trainium) |
| Explainability | Easier (trees, linear) | Harder (black box) |

### 3.4 Ensembles 🆕
- **Bagging** (e.g., random forest): many independent models trained in parallel on random samples, then vote → **reduces variance** (overfitting).
- **Boosting** (e.g., XGBoost): models trained sequentially, each correcting the last → **reduces bias**.

### 3.5 Algorithms ↔ AWS SageMaker built-ins 🆕 ⚠️
| SageMaker built-in | Category |
|---|---|
| Linear Learner, XGBoost, KNN, Factorization Machines | Supervised (classification/regression/recommendation) |
| K-Means, PCA, Random Cut Forest, IP Insights | Unsupervised |
| Image Classification, Object Detection, Semantic Segmentation | Supervised (CNN-based vision) |
| DeepAR (time-series forecasting), Seq2Seq, BlazingText | Supervised (RNN/sequence & text) |
| LDA, Neural Topic Model | Unsupervised topic modeling |
| SageMaker RL | Reinforcement |

---

## 4. Neural network families in simple terms
🆕

- **Neural network (MLP):** layers of "neurons"; each multiplies inputs by learned **weights**, adds them, passes through an activation. Training adjusts weights via **backpropagation** + **gradient descent** to reduce a **loss**. **Deep learning** = many layers.
- **CNN (Convolutional Neural Network):** built for **grid-like data (images)**. Small **filters** slide across the picture detecting edges, then textures, then shapes, then objects. Also **pooling** to shrink the image. Use cases: image classification, object detection, face recognition, medical scans, defect detection (what's behind services like Rekognition). *Learning type: usually supervised.*
- **RNN (Recurrent Neural Network):** built for **sequences**. Keeps a **hidden state (memory)** of what it has read so far, processing one step at a time. Use cases: time series, speech, text. **Problem:** forgets long-range context (**vanishing gradient**), slow (sequential). **LSTM / GRU** are improved RNNs with "gates" to keep long-term memory. **Largely replaced by transformers** for language. *Learning type: usually supervised.*
- **Transformer:** like an RNN's goal (handle sequences) but reads **everything in parallel** and uses **attention** (Section 11). Basis of LLMs.
- **Autoencoder / VAE:** compress then reconstruct (Sections 12, 14).
- **GAN:** two competing networks (Section 15).
- **Diffusion:** denoising generator (Section 13).

**One-liners to memorize**
- CNN → **C**ameras / images
- RNN → **R**ecurring sequences / time
- Transformer → **T**okens + attention
- SVM → widest-**margin** separator
- K-means → **unlabeled** groups
- KNN → **labeled** neighbors vote

---

## 5. Other ML terms
- **Transfer learning:** reuse a model trained on one task as the starting point for another. Example: fine-tune a pre-trained image model on your own product photos. (Fine-tuning is a form of transfer learning; see Section 10.)
- **Online vs. batch learning:** online updates the model continuously as new data arrives; batch trains on a fixed dataset.
- 🆕 **Data splits:** training / validation (tune hyperparameters) / test (final unbiased check).
- 🆕 **Class imbalance:** rare positives (fraud) make accuracy misleading; use precision/recall/F1 or resampling.
- 🆕 **Feature engineering:** creating/transforming inputs (scaling, encoding categories, combining columns).

---

## 6. Model fit and bias vs. variance

### Model fit
- **Underfitting:** model too simple, misses real patterns → poor on **both** training and test data. *Fix:* more complex model, more/better features, train longer.
- **Overfitting:** model too complex, memorizes training noise → great on training, poor on test/generalization. *Fix:* more training data, **regularization**, simplify model, **early stopping**, **dropout**.
- **Good fit:** balanced performance across training and unseen (test/validation) data.

### Bias vs. variance
- **Bias:** error from overly simplistic assumptions → underfitting. High bias = "too dumb" to capture the pattern.
- **Variance:** error from oversensitivity to small fluctuations in training data → overfitting. High variance = "too reactive" to training quirks.
- **Tradeoff:** reducing one typically increases the other; aim for the sweet spot that minimizes total error on unseen data.

| Symptom | Train error | Test error | Diagnosis |
|---|---|---|---|
| Both bad | High | High | Underfitting / high bias |
| Train great, test bad | Low | High | Overfitting / high variance |
| Both good | Low | Low | Good fit |

---

## 7. Evaluation metrics

**Classification**
- **Accuracy:** % correct overall (misleading on imbalanced data).
- **Precision:** of predicted positives, how many were actually positive (minimizes **false positives**).
- **Recall (sensitivity):** of actual positives, how many were caught (minimizes **false negatives**).
- **F1 score:** harmonic mean of precision and recall; one balanced number.
- **AUC-ROC:** how well the model separates classes across all thresholds.
- **Confusion matrix:** the raw TP/FP/TN/FN breakdown behind all of the above.
- 🆕 Formulas: Precision = TP/(TP+FP); Recall = TP/(TP+FN). Prefer **recall** when missing a positive is costly (disease screening); prefer **precision** when false alarms are costly (spam filtering a legit email).

**Regression**
- **MAE:** average absolute difference, easy to interpret.
- **MSE:** penalizes large errors more heavily.
- **RMSE:** square root of MSE, back in original units.
- **R²:** proportion of variance explained by the model.

**Clustering (unsupervised):** no ground truth, so metrics are internal.
- **Silhouette score:** how well-separated/cohesive the clusters are.
- **Inertia:** within-cluster sum of squared distances (lower = tighter clusters).

**Generative text** → Section 18 (ROUGE/BLEU/BERTScore).

---

## 8. ML lifecycle, parameters, hyperparameters

### ML project lifecycle (know this order; maps to AIF-C01 "ML development lifecycle")
1. **Business problem framing:** define the goal and success criteria
2. **Data collection:** gather raw data
3. **Data preparation / EDA / feature engineering:** clean, explore, transform
4. **Model training:** fit the model to prepared data
5. **Model evaluation:** test against metrics on held-out data
6. **Deployment:** production (real-time, batch, etc.)
7. **Monitoring & retraining:** watch for drift/degradation, retrain as needed

### Parameters vs. hyperparameters
- **Parameters:** learned automatically during training (e.g., neural network weights).
- **Hyperparameters:** set *before* training by you or a tuning process: learning rate, number of trees, batch size, number of epochs, number of clusters (k).
- **Hyperparameter tuning:** the search for the best values:
  - **Grid search:** try every combination.
  - **Random search:** sample combinations randomly.
  - **Bayesian optimization:** pick the next combination intelligently from past results. SageMaker **Automatic Model Tuning** uses this by default. ✏️ *(it also supports random, grid and Hyperband)*

---

## 9. Foundation models and LLMs
A **foundation model (FM)** is a large model pre-trained on broad, mostly unlabeled data (text, images, code, audio) so it can be adapted to many tasks instead of one narrow job.
- Pre-trained at massive scale (billions of parameters); general purpose (summarize, translate, answer questions, write code, describe images).
- Adaptable via prompting, RAG, or fine-tuning. Can be single-modality or **multimodal**.

A **Large Language Model (LLM)** is a foundation model specialized for language, trained to predict the next token.

> **All LLMs are foundation models, but not all foundation models are LLMs** (e.g., image-generation diffusion models are FMs but not LLMs).

### Key terms
| Term | Meaning |
|---|---|
| Token | Unit of text the model reads/writes (~¾ of an English word); Bedrock On-Demand bills per token |
| Context window | Max tokens the model can consider at once (prompt + response) |
| Parameters | Learned weights; more usually means more capability and cost |
| Prompt | Input given to the model |
| Temperature | Randomness; low = focused/consistent, high = creative |
| Top-p / Top-k | Limit the pool of candidate tokens sampled from |
| Max tokens | Cap on response length |
| Stop sequence | Text that tells the model to stop generating |
| Hallucination | Confident but incorrect or made-up output |
| Embeddings | Numeric vectors representing meaning; used for semantic search and RAG |
| RAG | Retrieval-Augmented Generation: fetch relevant documents and add them to the prompt |
| Fine-tuning | Further training on labeled data to specialize a model |
| Continued pre-training | Further training on unlabeled domain data |

### Customization ladder (cheapest → costliest)
1. **Prompt engineering** (zero-shot, few-shot, chain-of-thought, negative prompting)
2. **RAG** (Knowledge Bases for Amazon Bedrock)
3. **Fine-tuning / continued pre-training**
4. **Pre-training from scratch** (rarely needed)

---

## 10. Fine-tuning: what, why, when, how
🆕 *Expanded section. Your notes only had one-line definitions.*

### 10.1 What is it? (simple terms)
A pre-trained model already "knows language." **Fine-tuning** = giving it extra *training* on your own examples so its **weights change** and it gets better at *your* task, style, or domain. Analogy: a general doctor (the FM) doing a residency in cardiology (fine-tuning).
- It is a form of **transfer learning**.
- It **changes the model**; prompting and RAG only change the *input*.
- Learning type used: usually **supervised** (labeled prompt → ideal response pairs). Continued pre-training is **self-supervised** (unlabeled text).

### 10.2 Types of customization (what changes, what data)
| Method | What it does | Data needed | Typical use |
|---|---|---|---|
| **Instruction fine-tuning** (supervised fine-tuning, SFT) | Teaches the model to follow your task format with prompt/response pairs | **Labeled** prompt-completion pairs (hundreds to thousands+) | Classify tickets in your taxonomy, answer in brand tone, structured JSON output, domain Q&A style |
| **Continued pre-training** (domain adaptation) | Keeps training on raw domain text so the model learns the vocabulary/knowledge | **Unlabeled** domain corpus (large) | Legal, medical, financial jargon; internal terminology |
| **Full fine-tuning** | Updates *all* weights | Labeled | Max quality, highest cost/risk |
| **PEFT / LoRA / adapters** (parameter-efficient) | Updates only a **small number of added weights**, freezing the base model | Labeled | Much cheaper/faster; commonly used; less forgetting |
| **RLHF / preference tuning** | Human rankings train a reward model that steers outputs | Human preference data | Make responses helpful/safe (how chat models are aligned) |
| **Distillation** | Smaller "student" learns to imitate a large "teacher" | Teacher outputs | Cheaper, faster inference with similar quality |
| **Transfer learning** (general term) | Reuse a pre-trained model for a new task | Varies | Fine-tune a vision model on your product photos |

### 10.3 When to fine-tune (decision guide)
Start at the cheapest rung and move up only if needed: **Prompt engineering → RAG → Fine-tuning → (rarely) pre-train from scratch.**

| Situation | Best choice |
|---|---|
| Need quick improvement, no training | Prompt engineering (few-shot, clear instructions) |
| Need **current / private / frequently changing facts**, citations | **RAG** (not fine-tuning) |
| Need a consistent **style, tone, format**, or niche **task behavior** that prompts can't achieve | **Fine-tuning** |
| Need to learn a large body of **domain language** (unlabeled text) | **Continued pre-training** |
| Need high-volume cheap inference | **Distillation** / smaller model |
| Both facts *and* style | **RAG + fine-tuning combined** |

**Fine-tuning does NOT:** reliably add fresh facts (they go stale; use RAG), fix every hallucination, or work with little/poor data.

### 10.4 Pros and cons
| Pros | Cons |
|---|---|
| Better task/domain accuracy | Costs compute + labeled data + time |
| Consistent tone/format | Needs high-quality, representative data (garbage in → garbage out) |
| Shorter prompts (lower per-call tokens/latency) | **Catastrophic forgetting** (loses general abilities) |
| Can use a smaller/cheaper model | **Overfitting** on small data |
| | Knowledge becomes stale → retrain |
| | Privacy: training data may leak into outputs if sensitive data is included |
| | Custom model must be hosted (Bedrock: **Provisioned Throughput**) |

### 10.5 How it works on AWS
**Amazon Bedrock (managed, no infrastructure)**
1. Pick a **base model that supports customization** (not all do; ⚠️ check the console).
2. **Prepare data** as **JSON Lines (.jsonl)** in **S3** (format varies by model; e.g., `{"prompt": "...", "completion": "..."}`). Optional **validation dataset**.
3. Create a **model customization job** (fine-tuning or continued pre-training): set **hyperparameters** (epochs, batch size, learning rate), IAM role, S3 input/output, optional KMS encryption.
4. Job produces a **custom model** (private to your account).
5. **Evaluate** it (Bedrock Model Evaluation / your test set).
6. **Buy Provisioned Throughput** to run the custom model (generally required).
7. Monitor, and retrain as data changes. Use **Guardrails** on top.

**Amazon SageMaker**
- **JumpStart:** pick a pre-trained model and fine-tune it with a few clicks/API calls.
- Full control: your own training scripts on GPU/Trainium instances, **Automatic Model Tuning** for hyperparameters, **Ground Truth** to create labeled data, **Model Registry/Pipelines** to manage versions.
- Choose SageMaker when you need full control, open-weight models, or custom algorithms.

### 10.6 Data best practices
- **Quality over quantity**; representative, diverse, de-duplicated, correctly labeled.
- Remove PII/sensitive data (governance, privacy).
- Split into **training / validation / test** sets.
- Watch for **bias** in the data (Section 21).
- Watch the training vs. validation loss: training ↓ while validation ↑ = **overfitting**.

### 10.7 Fine-tuning vs. RAG vs. prompting at a glance
| | Prompt engineering | RAG | Fine-tuning |
|---|---|---|---|
| Changes model weights? | No | No | **Yes** |
| Cost / effort | Lowest | Low-medium | High |
| Fresh/private data | No | **Yes** | Stale after training |
| Style/format/task skill | Limited | Limited | **Strong** |
| Citations/grounding | No | **Yes** | No |
| Needs labeled data | No | No | **Yes** (unlabeled for continued pre-training) |

---

## 11. Transformers and self-attention
A **transformer** is the neural network architecture (2017 paper *Attention Is All You Need*) behind nearly all modern LLMs and many image, audio and multimodal models.

**Why it mattered:** RNNs/LSTMs read one token at a time (slow, weak on long context). Transformers process **all tokens in parallel** and use **attention** to relate any token to any other, making large-scale GPU training practical.

### Building blocks
1. Token embeddings
2. Positional encoding (restores word order lost by parallel processing)
3. Self-attention
4. Feed-forward network
5. Residual connections + layer normalization

These form one layer; large models stack dozens to 100+ layers.

### Self-attention
Every token decides which other tokens matter for understanding it.
Example: *"The animal didn't cross the street because **it** was too tired."* Self-attention links "it" to "animal."

Each token is projected into three vectors:
- **Query (Q):** what am I looking for?
- **Key (K):** what do I offer?
- **Value (V):** the information passed along if selected

Steps: (1) compare each Q with every K (dot product) → relevance scores; (2) scale by sqrt(d_k) and apply softmax → weights summing to 1; (3) take a weighted sum of the V vectors.

```
Attention(Q, K, V) = softmax( Q . K^T / sqrt(d_k) ) . V
```

**Multi-head attention:** several attention heads run in parallel, each learning different relationships (grammar, coreference, nearby words); outputs are concatenated.

### Encoder
Reads and understands the input. Bidirectional self-attention (each token sees left and right context). Output: context-aware vectors, one per input token.

### Decoder
Generates output one token at a time (**autoregressive**).
1. **Masked self-attention:** can only attend to earlier tokens (causal mask)
2. **Cross-attention** (encoder-decoder models only): attends to the encoder's output
3. Feed-forward network, residuals, normalization
4. Final linear + softmax → probabilities over the vocabulary

### Three architecture types
| Type | Direction | Examples | Good for |
|---|---|---|---|
| Encoder-only | Bidirectional | BERT, RoBERTa | Classification, search, embeddings, NER |
| Decoder-only | Left-to-right | GPT, Claude, Llama | Text generation, chat, code, reasoning |
| Encoder-decoder | Both | T5, BART, original Transformer | Translation, summarization |

Most modern LLMs (including those on Bedrock) are **decoder-only**.

**Use cases:** text generation and chat, code generation, translation, summarization, semantic search and RAG (embeddings), classification and entity extraction, vision (Vision Transformers), speech, multimodal models.

**Limitations:** attention cost grows **quadratically** with sequence length; heavy data/compute needs; hallucinations and bias from training data.

---

## 12. Latent space
A **latent space** is a compressed, lower-dimensional representation where a model stores the essential features of data rather than raw detail. "Latent" = hidden: features are learned, not directly observed.

**Analogy:** a face photo has ~1 million pixels, but a face can be described by a few dozen traits (shape, skin tone, hair, expression). Those traits are the latent variables.

Properties:
- Much lower-dimensional than raw data
- Similar items sit close together; smooth movement gives smooth changes in output
- Sampling a point and decoding it generates new plausible data
- Used in VAEs, GANs (the input noise vector), latent diffusion, and embeddings

---

## 13. Diffusion models
A **diffusion model** generates data (mostly images; also video and audio) by starting from pure random noise and **removing noise step by step** until a coherent output appears.

### How it works
1. **Forward process (training only):** add a little Gaussian noise to real images over many steps until only noise remains
2. **Reverse process (learned):** train a network (usually a **U-Net**, increasingly a transformer) to predict the noise at each step
3. **Generation:** start from random noise and denoise repeatedly
4. **Text conditioning:** a text encoder (e.g., **CLIP**, transformer-based) turns the prompt into embeddings that guide denoising via cross-attention
5. **Latent diffusion:** run the process in a compressed latent space (using a VAE) instead of raw pixels: much cheaper (Stable Diffusion)

### Common controls
| Control | Meaning |
|---|---|
| Prompt | What you want |
| Negative prompt | What to avoid |
| CFG / guidance scale | How strictly to follow the prompt (higher = more literal) |
| Steps | Number of denoising steps (more = more detail, slower) |
| Seed | Same seed + settings reproduces the same image |

**Use cases:** text-to-image, image-to-image (style transfer, variations), inpainting (edit masked region), outpainting (extend borders), upscaling, background removal, text-to-video, audio generation, marketing/design assets.

**Strengths:** very high quality, diverse outputs, stable training. **Weaknesses:** slower (many steps), artifacts, struggles with text inside images, can reflect training bias.

---

## 14. VAE (Variational Autoencoder)
A **VAE** compresses data into a latent space and reconstructs it, while keeping the latent space smooth enough to generate new data.

### How it works
1. **Encoder** outputs a probability distribution (mean and variance), not a single point
2. A point **z** is sampled from that distribution (reparameterization trick)
3. **Decoder** reconstructs the input from z
4. **Loss = reconstruction loss + KL divergence**
   - Reconstruction loss: output should resemble input
   - KL divergence: keeps the latent distribution close to a standard normal → continuous space, no gaps

**Generation:** sample a random z from the normal distribution and run it through the decoder.

| | Autoencoder | VAE |
|---|---|---|
| Latent encoding | Single fixed point | Probability distribution |
| Good for | Compression, denoising | Compression and generation |
| Latent space | May have gaps | Smooth, continuous |

**Use cases:** image generation and variations; **anomaly detection** (anomalies reconstruct poorly); compression and denoising; synthetic data generation; component inside latent diffusion.
**Strengths:** stable training, smooth latent space, good diversity. **Weakness:** outputs often blurry vs. GAN/diffusion.

---

## 15. GAN (Generative Adversarial Network)
Two networks compete:
- **Generator:** turns random noise (a latent vector) into fake data
- **Discriminator:** tries to tell real data from generated data

The generator tries to fool the discriminator; the discriminator tries to catch it. Both improve until fakes are hard to distinguish. After training, only the generator is used. **Analogy:** counterfeiter vs. detective.

**Variants:** DCGAN (images), Conditional GAN (guided by a label), StyleGAN (realistic faces), CycleGAN (unpaired image translation), Pix2Pix (paired image translation).
**Use cases:** realistic image/face generation, image-to-image translation, super-resolution, synthetic data for augmentation, deepfakes (responsible AI concern).
**Strengths:** sharp, realistic outputs; fast generation (single forward pass). **Weaknesses:** unstable training, **mode collapse** (limited variety), hard to evaluate and control.

---

## 16. Generative model families compared

| Model | Core idea | Typical output | Training | Quality | Diversity | Generation speed |
|---|---|---|---|---|---|---|
| Transformer / LLM | Self-attention, next-token prediction | Text, code | Stable | High | High | Token by token |
| VAE | Encode to latent distribution, decode | Images, compression, anomaly detection | Stable | Often blurry | Good | Fast |
| GAN | Generator vs. discriminator | Images | Unstable | Sharp | Can suffer mode collapse | Fast |
| Diffusion | Iteratively denoise noise | Images, video, audio | Stable | Very high | High | Slower (many steps) |

**Memory tricks**
- VAE = **V**ague but smooth (blurry, stable)
- GAN = **G**ame between two players (sharp, unstable)
- Diffusion = **D**enoise down from noise (best quality, slower)
- Transformer = **T**okens + **T**ext (attention)

---

## 17. Inference parameters
**Temperature, Top P and Top K** are inference parameters controlling how the model picks the next token. They change output style/randomness only. They do **not** change the model's knowledge or weights.

**Running example:** completing *"The sky is ___"*

| Token | blue | clear | cloudy | falling | green | purple |
|---|---|---|---|---|---|---|
| Probability | 0.50 | 0.20 | 0.15 | 0.08 | 0.05 | 0.02 |

### Temperature
Rescales the probability distribution before sampling and controls randomness.
- **Low (near 0):** distribution sharpens → almost always the top token; focused, predictable, repeatable.
- **High (near 1 or above):** distribution flattens → unlikely tokens picked more often; varied/creative, more risk of errors.
- Example: temp 0.1 → "blue" nearly every time; temp 1.0 → "cloudy", "clear", occasionally "falling".

| Setting | Use cases |
|---|---|
| Low (0–0.3) | Factual Q&A, data extraction, classification, code generation, customer support answers, summarizing legal/medical text |
| Medium (0.4–0.7) | General chatbots, email drafting, balanced summaries |
| High (0.8–1.0) | Brainstorming, poetry, storytelling, marketing taglines, idea generation |

### Top K
Limits sampling to the **K most likely tokens**; the rest are discarded, then probabilities are renormalized. K is a **fixed count**.
- K=1: only "blue" (greedy, fully deterministic). K=3: sample among blue, clear, cloudy only. K=50: very wide pool, more diverse.
- Low K (1–10): precise, safe outputs (code, structured data, factual). High K (40–100+): creative, open-ended.
- **Weakness:** K is fixed regardless of the distribution. If one token has 95% probability, K=3 still lets two low-quality tokens in.

### Top P (nucleus sampling)
Picks the **smallest set of tokens whose cumulative probability reaches P**, then samples only from that set. Pool size **adapts** to model confidence.
- P=0.5 → {blue}. P=0.9 → blue (0.50) + clear (0.70) + cloudy (0.85) + falling (0.93) → {blue, clear, cloudy, falling}.
- If the model is very confident ("blue" = 0.95), P=0.9 picks just **one** token; Top K=3 would still allow three.
- Low P (0.1–0.5): factual, precise, consistent. High P (0.9–1.0): creative/conversational.

### Comparison
| | Temperature | Top K | Top P |
|---|---|---|---|
| Controls | Shape of the distribution (how "peaked") | A fixed number of candidate tokens | A cumulative probability threshold |
| Range | 0 to 1 in most Bedrock models (some allow higher) | Integer (1, 2, 50, …) | 0 to 1 |
| Pool size | N/A: all tokens remain possible | Fixed | Dynamic |
| Lower → | More deterministic | Fewer choices, safer | Fewer choices, safer |
| Higher → | More random/creative | More choices, more diverse | More choices, more diverse |

### Exam tips
- **Deterministic, factual output** (code, extraction, compliance answers) → **low temperature** (and/or low Top P / Top K).
- **Creative output** → **high temperature**.
- Top K = a **number** of tokens. Top P = a **probability** threshold.
- Temperature changes *how likely* tokens are; Top K/Top P change *which tokens are eligible*.
- Typically combined; common practice is to tune temperature **or** Top P, not both aggressively.
- Other inference parameters: **max tokens / response length** (caps output size) and **stop sequences** (text that ends generation).
- These do **not** retrain the model or reduce hallucinations by themselves. For hallucinations: RAG, Guardrails, prompt engineering.

---

## 18. Evaluating generated text
ROUGE, BLEU and BERTScore compare **generated text** against **human-written reference text**.

| | **ROUGE** | **BLEU** | **BERTScore** |
|---|---|---|---|
| Full name | Recall-Oriented Understudy for Gisting Evaluation | Bilingual Evaluation Understudy | BERT-based similarity score |
| Main use | **Summarization** | **Machine translation** | Any text generation where meaning matters (summaries, translation, paraphrase, Q&A) |
| How it works | N-gram/sequence overlap, focused on **recall** (how much of the reference appears in the output) | N-gram overlap, focused on **precision** (how much of the output appears in the reference), with a **brevity penalty** | Compares **contextual embeddings** with cosine similarity; reports precision, recall, F1 |
| Captures meaning? | No (exact word match) | No (exact word match) | **Yes** (synonyms/paraphrases) |
| Score range | 0–1 | 0–1 | Roughly 0–1 (higher = better) |

**ROUGE variants:** **ROUGE-1** (unigram overlap), **ROUGE-2** (bigram overlap), **ROUGE-L** (longest common subsequence; reflects sentence structure/word order).

**Limitations**
- ROUGE/BLEU penalize valid paraphrases ("The car is fast" vs. "The automobile is quick" scores low).
- BERTScore handles semantics but is computationally heavier and depends on the embedding model.
- None check **factual correctness**, tone or safety → human evaluation still needed (e.g., brand voice).

**Memory tricks:** ROUGE → Recall → summa**R**ies · BLEU → **B**ilingual → translation (precision) · BERTScore → Bert understands meaning → semantic similarity.

**Exam tips**
- Summarization → **ROUGE** · Translation → **BLEU** · Semantic similarity/paraphrase tolerance → **BERTScore**
- Subjective qualities (tone, helpfulness) → **human evaluation**
- General language-understanding benchmarks → **GLUE / SuperGLUE**
- **Amazon Bedrock Model Evaluation** supports automatic metrics (incl. ROUGE and BERTScore, plus accuracy, robustness, toxicity) and human-based evaluation.

---

## 19. Amazon Bedrock pricing modes and Regions

| | On-Demand | Provisioned Throughput |
|---|---|---|
| Pricing | Per token (images per image) | Per hour per model unit |
| Commitment | None | None (hourly), 1 month, or 6 months (cheaper per hour) |
| Capacity | Shared | Dedicated, guaranteed |
| Throttling risk | Yes, at quota limits | No, within purchased capacity |
| Idle cost | Zero | Pay even if unused |
| Custom (fine-tuned) models | Mostly not supported | Supported (generally required) |
| Best for | Dev, testing, spiky/unpredictable traffic | Steady high-volume production, custom models |

**Related:** **Batch inference** (offline jobs, usually discounted) and **cross-region inference** (route On-Demand requests across Regions to absorb spikes).

**AWS Region basics:** each Region has a minimum of 3 Availability Zones; each AZ is one or more discrete data centers with independent power, cooling and networking. Mumbai = `ap-south-1`; Hyderabad = `ap-south-2`.

---

## 20. AWS services mapping

| Need | Service / model |
|---|---|
| Access FMs via API, no infrastructure | Amazon Bedrock |
| Text generation, chat, summarization | Claude (Anthropic), Llama (Meta), Mistral, Cohere, Amazon Titan Text, Amazon Nova |
| Image generation (diffusion-based) | Stability AI (Stable Diffusion / Stable Image), Amazon Titan Image Generator, Amazon Nova Canvas |
| Video generation | Amazon Nova Reel |
| Embeddings | Amazon Titan Embeddings, Cohere Embed |
| Build, train, deploy custom models | Amazon SageMaker |
| Pre-trained models to deploy quickly | SageMaker JumpStart |
| No-code prompt experimentation | Bedrock playgrounds, PartyRock |
| GenAI assistants for work/developers | Amazon Q (Business, Developer) |
| Filter harmful content, deny topics, redact PII | Guardrails for Amazon Bedrock |
| Ground answers in your data (RAG) | Knowledge Bases for Amazon Bedrock |
| Multi-step task automation | Agents for Amazon Bedrock |
| Vector storage for embeddings | OpenSearch Serverless, Aurora PostgreSQL (pgvector), others |

Titan Image Generator supports inpainting, outpainting, variations, background removal, and adds an **invisible watermark** to identify AI-generated images (a responsible-AI point). ⚠️ Model lineups change often; verify names in the Bedrock console and the official exam guide.

---

## 21. Responsible AI dimensions
Explained through a **hiring / resume-screening** example (company wants impartial decisions across demographic groups and reviewers who understand each decision).

### Explainability ✅ (addresses reviewers understanding decisions)
- The ability to understand and evaluate **why** an AI system produced a specific output. In hiring, explanations show reviewers which factors (relevant experience, required certifications) influenced the screening decision.
- AWS lists it as a core dimension of responsible AI. Explanations help reviewers check decisions before acting, **detect inappropriate factors**, and explain outcomes to applicants.
- Complements fairness by making each decision open to human review.

### Fairness ✅ (addresses impartiality across groups)
- Considers the impact of an AI system on **different groups of stakeholders** (gender, age, ethnicity). A fair screening system does not systematically favor or disadvantage particular demographic groups.
- To support it: check training data for **representation and historical bias**, evaluate **selection rates and error rates across groups**, adjust data or model when disparities appear.

### Controllability
- Mechanisms to **monitor and steer** the AI system (override or stop it). Valuable for consequential decisions, but it does **not** specifically address impartial treatment of groups or help reviewers understand *why* a decision was made.

### Veracity and robustness
- Producing **correct outputs even when inputs are unexpected or adversarial**. Robust systems work reliably on unusual inputs, but this does not ensure impartial outcomes nor explain individual decisions. Necessary, but doesn't address either goal.

### Privacy and security
- Appropriately **obtaining, using and protecting data and models**. Essential in recruitment, but strong privacy controls don't ensure impartial outcomes or reviewer understanding.

### Quick matcher 🆕
| Goal in the question | Dimension |
|---|---|
| Equal treatment across demographic groups | **Fairness** |
| Understand *why* the model decided | **Explainability** |
| Human can stop/override | **Controllability** |
| Reliable on odd/adversarial input, accurate | **Veracity & robustness** |
| Protect personal data and models | **Privacy & security** |
| Clear documentation of how it works/limits | **Transparency** |
| Accountability, policy, compliance | **Governance** |
| Avoid harm | **Safety** |

---

## 22. Scenario → answer tables and exam traps

### 22.1 Spot the technique
| If the scenario says… | Answer |
|---|---|
| Labeled data, predict a category | Supervised classification |
| Labeled data, predict a number | Supervised regression |
| Predict a digit from a handwritten image | Neural network (CNN) |
| Group customers or traffic without labels | Clustering (K-means) |
| Reduce the number of features / simplify data | Dimensionality reduction (PCA) |
| Few labels + lots of unlabeled data | Semi-supervised |
| Agent, rewards, penalties, environment | Reinforcement learning |
| Model learns from the text itself (predict the next word) | Self-supervised |
| Unusual/rare events, no labeled examples | Anomaly detection (unsupervised, Random Cut Forest) |
| 🆕 Images / object recognition | CNN |
| 🆕 Sequences/time series with memory | RNN / LSTM |
| 🆕 Widest-margin boundary between classes | SVM |
| 🆕 Label by majority of nearest labeled neighbors | KNN (supervised) |
| 🆕 Add style/tone/format skill using labeled examples | Fine-tuning |
| 🆕 Teach domain jargon with lots of unlabeled text | Continued pre-training |

### 22.2 Classic exam traps
- "Generate images from text" → **diffusion model**, not an LLM
- "Understand context between words using attention" → **transformer**
- "Use company documents without retraining" → **RAG / Knowledge Bases**, not fine-tuning
- "Consistent, factual answers" → **lower temperature**
- "Steady high-volume workload or custom model" → **Provisioned Throughput**; "spiky or experimental" → **On-Demand**
- "No ML expertise, just call a model" → **Bedrock**; "build, train, tune your own model" → **SageMaker**
- "Filter harmful content or block topics" → **Guardrails**
- "Two networks competing, generator vs. discriminator" → **GAN**
- "Mode collapse" → always a **GAN** issue
- "Compress and generate with probabilistic latent space" → **VAE**
- "Anomaly detection via reconstruction error" → **autoencoder / VAE**
- "Generate synthetic data to augment a dataset" → **GAN or VAE**
- Bedrock's headline image models are **diffusion-based**
- All LLMs are FMs; not all FMs are LLMs
- 🆕 **K-means = unsupervised; KNN = supervised**
- 🆕 **Logistic regression = classification**
- 🆕 **CNN/RNN are architectures, not learning types**
- 🆕 **Fine-tuning changes weights; RAG changes the prompt**
- 🆕 **Fine-tuning needs labeled data; continued pre-training uses unlabeled data**
- 🆕 **Fine-tuned Bedrock models need Provisioned Throughput**
- 🆕 **Overfitting = great train, poor test; underfitting = poor on both**

---

## 23. Sample questions

**Q1.** A marketing team wants to create product images from text descriptions with minimal setup. Which is the best fit?
A. A VAE trained on SageMaker  B. A diffusion-based image model on Amazon Bedrock  C. An encoder-only transformer  D. A GAN trained from scratch
**Answer: B.** Bedrock offers managed diffusion-based image models (Titan Image Generator, Stable Diffusion, Nova Canvas) with no infrastructure.

**Q2.** Which mechanism lets a transformer determine which other words in a sentence are most relevant to a given word?
A. Convolution  B. Self-attention  C. Backpropagation through time  D. Pooling
**Answer: B.** Self-attention uses Query, Key, and Value vectors.

**Q3.** What does the reverse process of a diffusion model learn to do?
A. Compress images into a latent vector  B. Classify real vs. fake images  C. Predict and remove noise at each step  D. Translate text
**Answer: C.**

**Q4.** A GAN keeps generating only a small set of similar images regardless of input noise. What is this called?
A. Overfitting  B. Vanishing gradient  C. Mode collapse  D. Underfitting
**Answer: C.** Low diversity in generator outputs.

**Q5.** Which two components make up the VAE loss?
A. Cross-entropy and accuracy  B. Reconstruction loss and KL divergence  C. BLEU and ROUGE  D. Generator loss and discriminator loss
**Answer: B.** D is the GAN loss.

**Q6.** Which transformer architecture do most modern LLMs such as Claude and Llama use?
A. Encoder-only  B. Encoder-decoder  C. Decoder-only  D. Recurrent
**Answer: C.**

**Q7.** A company wants its chatbot to answer from internal policy documents without retraining. Most cost-effective?
A. Pre-train a new FM  B. Fine-tune weekly  C. Use RAG with Knowledge Bases for Amazon Bedrock  D. Increase temperature
**Answer: C.** RAG grounds answers in your data without changing weights.

**Q8.** A team wants a more deterministic, factual LLM response. Which change helps?
A. Increase temperature  B. Decrease temperature  C. Increase max tokens  D. Remove stop sequences
**Answer: B.**

**Q9.** A customer-facing app has steady, heavy 24/7 traffic and cannot tolerate throttling. Which Bedrock mode?
A. On-Demand  B. Provisioned Throughput  C. Batch inference only  D. Playground
**Answer: B.** Dedicated capacity, consistent performance.

**Q10.** A developer fine-tunes a model in Amazon Bedrock and wants to run it. What is generally required?
A. On-Demand mode  B. Provisioned Throughput  C. Cross-region inference  D. PartyRock
**Answer: B.**

**Q11.** Which statement about latent space is correct?
A. Raw pixel representation  B. Compressed, lower-dimensional representation capturing essential features  C. Set of labeled training examples  D. Output vocabulary of an LLM
**Answer: B.**

**Q12.** A company detects unusual sensor readings by measuring how poorly a model reconstructs them. Which model type?
A. Autoencoder / VAE  B. Decoder-only LLM  C. Diffusion model  D. Encoder-decoder translation model
**Answer: A.** Anomalies reconstruct poorly → high reconstruction error.

**Q13.** Which statement is true?
A. All FMs are LLMs  B. All LLMs are FMs, but not all FMs are LLMs  C. Diffusion models are LLMs  D. LLMs cannot be FMs
**Answer: B.**

**Q14.** Which metric is commonly used for text summarization quality?
A. BLEU  B. ROUGE  C. Mean squared error  D. AUC
**Answer: B.** BLEU = translation; ROUGE = summarization.

**Q15.** A company must block certain topics and redact PII in an app using a Bedrock model. Which feature?
A. Provisioned Throughput  B. Knowledge Bases  C. Guardrails for Amazon Bedrock  D. Agents
**Answer: C.**

**Q16.** Why is latent diffusion (Stable Diffusion) more efficient than pixel-space diffusion?
A. Fewer training images  B. It denoises in a compressed latent space produced by a VAE  C. Replaces the U-Net with a GAN  D. Skips the noise process
**Answer: B.**

**Q17.** Which model family is known for training instability because two networks train against each other?
A. Transformer  B. VAE  C. GAN  D. Diffusion
**Answer: C.**

**Q18.** A team must build, train, and deploy its own custom ML model with full infrastructure control. Which service?
A. Amazon Bedrock  B. Amazon SageMaker  C. PartyRock  D. Amazon Q Business
**Answer: B.**

**🆕 Q19.** A retailer groups customers by purchasing behavior with no predefined labels. Which approach?
A. Logistic regression  B. K-means  C. KNN  D. Random forest
**Answer: B.** K-means is unsupervised clustering. The others are supervised.

**🆕 Q20.** Which is a *learning type*, not an algorithm or architecture?
A. CNN  B. SVM  C. Reinforcement learning  D. K-means
**Answer: C.**

**🆕 Q21.** A hospital has a few labeled X-rays and thousands of unlabeled ones. Which approach fits?
A. Supervised  B. Semi-supervised  C. Reinforcement  D. Unsupervised clustering only
**Answer: B.**

**🆕 Q22.** A company wants its assistant to always reply in a specific brand voice and JSON format, and has 3,000 labeled examples. Best approach?
A. RAG only  B. Fine-tuning  C. Higher temperature  D. Train a model from scratch
**Answer: B.** Style/format consistency from labeled pairs = (instruction) fine-tuning.

**🆕 Q23.** A model scores 99% on training data but 70% on test data. What is the problem and a fix?
A. Underfitting; add layers  B. Overfitting; add data/regularization/early stopping  C. High bias; train longer  D. Data drift; deploy
**Answer: B.**

**🆕 Q24.** Which learning technique trains LLMs during pre-training?
A. Supervised on human labels  B. Self-supervised (predict next token)  C. Reinforcement only  D. Association rules
**Answer: B.**

---

## 24. Flashcards

| Question | Answer |
|---|---|
| What does a transformer use to relate tokens? | Self-attention (Q, K, V) |
| Which part of a transformer generates text? | Decoder (masked self-attention) |
| Which part understands input? | Encoder (bidirectional) |
| What starts a diffusion generation? | Random noise |
| What are the two GAN networks? | Generator and discriminator |
| Why is a VAE latent space useful for generation? | Smooth and continuous because of KL divergence |
| Cheapest way to improve model output? | Prompt engineering |
| Billing unit for Bedrock On-Demand text models? | Input and output tokens |
| Billing unit for Provisioned Throughput? | Per hour per model unit |
| 🆕 CNN is best for? | Images (filters detect edges → shapes → objects) |
| 🆕 RNN is best for? | Sequences/time series (has memory); LSTM/GRU fix forgetting |
| 🆕 SVM idea? | Widest-margin separator between classes (supervised) |
| 🆕 K-means vs. KNN? | K-means = unsupervised clustering; KNN = supervised neighbor vote |
| 🆕 Is logistic regression regression? | No: classification |
| 🆕 Bagging vs. boosting? | Parallel trees reduce variance (random forest) vs. sequential trees reduce bias (XGBoost) |
| 🆕 Fine-tuning vs. RAG? | Changes weights (style/task) vs. changes prompt (fresh/private facts) |
| 🆕 Fine-tuning data? | Labeled prompt-response pairs (JSONL in S3 for Bedrock) |
| 🆕 Continued pre-training data? | Unlabeled domain text |
| 🆕 Risk of fine-tuning? | Catastrophic forgetting, overfitting, stale knowledge, cost |
| 🆕 What does LoRA/PEFT do? | Trains a small set of extra weights instead of all weights |
| 🆕 Running a fine-tuned Bedrock model needs? | Provisioned Throughput |
| 🆕 Precision vs. recall? | Precision = avoid false positives; recall = avoid false negatives |
