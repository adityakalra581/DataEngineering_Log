# ML & AI Fundamentals — Condensed Certification Notes

## Types of Learning

| Type | Labels? | How it works | Use case |
|---|---|---|---|
| **Supervised** | Full labels on every example | Learns to map input → known correct output | Spam detection, price prediction, fraud classification |
| **Unsupervised** | None | Finds structure/patterns with no "correct answer" to check against | Customer segmentation, anomaly detection, dimensionality reduction |
| **Semi-supervised** | Small labeled set + large unlabeled set | Uses the few labels as an anchor, leverages structure in the unlabeled data to generalize further | Medical imaging (expert-labeled scans are scarce, raw scans are plentiful) |
| **Self-supervised** | None provided — model generates its own labels from the data itself | Pretext tasks: predict a masked/hidden part of the input from the rest | Pre-training LLMs (predict next word), pre-training vision models (predict masked image patch) |
| **Reinforcement** | None — uses reward signal instead | Agent learns a policy via trial-and-error interaction with an environment to maximize cumulative reward | Game-playing agents, robotics control, resource allocation optimization |

**Quick distinctions exams probe:**
- Unsupervised vs. self-supervised: unsupervised has no labels at all and no pretext task; self-supervised manufactures pseudo-labels from the data's own structure.
- Semi-supervised vs. supervised: semi-supervised is the answer when "labeling is expensive/limited" is mentioned in the question.
- Reinforcement is the odd one out — it's not about labels at all, it's about reward/penalty feedback over a sequence of actions.

## ML learning techniques with examples

## Overview

| Technique | Data | Goal |
|---|---|---|
| Supervised | Fully labeled | Predict a known output |
| Unsupervised | No labels | Find hidden structure |
| Semi-supervised | Small labeled set + large unlabeled set | Get good accuracy without labeling everything |
| Self-supervised | Unlabeled (labels generated from the data itself) | Learn representations; how LLMs are pre-trained |
| Reinforcement | No dataset; an agent interacts with an environment | Maximize cumulative reward |

---

## 1. Supervised learning

Trains on **labeled** data (input + correct output) to predict outputs for new inputs.

### Classification (predict a category)
- **Binary:** spam vs. not spam; loan default yes/no; tumor malignant/benign.
- **Multi-class:** classify a support ticket as billing, technical or sales; identify a flower species.
- **Common algorithms:** logistic regression, decision trees, random forest, XGBoost, SVM.

### Regression (predict a number)
- Predict house price from size and location.
- Forecast next month's sales or tomorrow's temperature.
- Estimate delivery time.
- **Common algorithms:** linear regression, XGBoost, random forest.

### Neural networks (a more complex supervised technique)
Stacked layers of mathematical transformations whose weights are adjusted during training.
- Predicting a digit from a handwritten image (MNIST).
- Image classification (cat vs. dog), speech recognition.
- Deep learning is neural networks with many layers. Neural networks can also be used unsupervised or self-supervised.

**AWS mapping:** SageMaker XGBoost, Linear Learner, Image Classification; Amazon Rekognition Custom Labels.

---

## 2. Unsupervised learning

Finds patterns in **unlabeled** data. There is no "right answer" to learn from.

### Clustering
Groups similar data points together.
- Customer segmentation by purchase behavior.
- Identifying types of network traffic to flag potential security incidents.
- Grouping news articles by topic.
- **Algorithms:** k-means, hierarchical clustering, DBSCAN.

### Dimensionality reduction
Reduces the number of features while keeping the important information. It is often used to preprocess data for other ML tasks and to cut cost and complexity.
- Compressing hundreds of sensor readings into a few key components.
- Blurring or cropping background features in an image recognition app.
- Visualizing high-dimensional data in 2D.
- **Algorithms:** PCA, t-SNE, autoencoders.

### Anomaly detection
Finds data points that don't fit the normal pattern.
- Detecting unusual server metrics or a sudden spike in API calls.
- Spotting faulty equipment from sensor data.
- **Algorithm:** Random Cut Forest.

### Association rule learning
Finds relationships between items.
- Market-basket analysis: "customers who buy bread often buy butter."

**AWS mapping:** SageMaker K-Means, PCA, Random Cut Forest; Amazon Lookout for Metrics (anomaly detection).

---

## 3. Semi-supervised learning

Uses a **small labeled set** plus a **large unlabeled set**. It is useful when labeling is expensive or slow.

**Examples:**
- **Fraud identification:** within a large set of transactions, only a subset has been confirmed as fraud by experts. The solution trains on the unlabeled data first, then refines with the labeled data for better accuracy.
- **Sentiment analysis:** labeling sentiment across every customer channel is not cost-effective. Train on the large unlabeled portion first, then on a labeled sample, to get more confidence in overall customer sentiment.
- **Medical imaging:** only a few scans are annotated by radiologists, but thousands more are available unlabeled.
- **Speech recognition:** a small amount of transcribed audio plus a large archive of untranscribed audio.

**AWS mapping:** SageMaker Ground Truth can label a subset of data and use active learning to auto-label the rest.

---

## 4. Self-supervised learning

The model creates its own labels from the data's structure. No human labeling is needed.
- **LLMs:** predict the next token or a masked word in text (GPT-style, BERT-style).
- **Image models:** predict missing patches of an image.
- This is the foundation of **pre-training for foundation models**, which are later adapted with fine-tuning or prompting.

---

## 5. Reinforcement learning (RL)

An **agent** takes **actions** in an **environment**, receives **rewards or penalties**, and learns a policy that maximizes long-term reward.
- A robot learning to navigate a warehouse (reward for reaching the target, penalty for collisions).
- Game-playing agents (chess, Go).
- Dynamic pricing, ad bidding, and traffic-signal optimization.
- **RLHF (Reinforcement Learning from Human Feedback):** human preferences train a reward model that aligns an LLM's responses to be more helpful and safe.

**AWS mapping:** SageMaker RL; AWS DeepRacer (a hands-on RL learning tool).

---

## Other terms to know
- **Transfer learning:** reuse a model trained on one task as the starting point for another. For example, fine-tune a pre-trained image model for your own product photos.
- **Online vs. batch learning:** online updates the model continuously as new data arrives; batch trains on a fixed dataset.

---

## Exam tips: spot the technique from the scenario

| If the scenario says… | Answer |
|---|---|
| Labeled data, predict a category | Supervised classification |
| Labeled data, predict a number | Supervised regression |
| Predict a digit from a handwritten image | Neural network |
| Group customers or traffic without labels | Clustering |
| Reduce the number of features / simplify data | Dimensionality reduction |
| Few labels + lots of unlabeled data | Semi-supervised |
| Agent, rewards, penalties, environment | Reinforcement learning |
| Model learns from the text itself (predict the next word) | Self-supervised |
| Unusual or rare events with no labeled examples | Anomaly detection (unsupervised) |

I can turn these into a 10-question scenario quiz, or add them to a combined study sheet with the earlier notes.

## Model Fit
- **Underfitting**: model too simple, misses real patterns → poor performance on both training and test data. Fix: more complex model, more/better features, train longer.
- **Overfitting**: model too complex, memorizes training data noise → great training performance, poor test/generalization performance. Fix: more training data, regularization, simplify model, early stopping, dropout.
- **Good fit**: balanced performance across training and unseen (test/validation) data.

## Bias vs. Variance
- **Bias**: error from overly simplistic assumptions → underfitting. High bias = model is "too dumb" to capture the pattern.
- **Variance**: error from oversensitivity to small fluctuations in training data → overfitting. High variance = model is "too reactive" to training-set quirks.
- **Bias-variance tradeoff**: reducing one typically increases the other; the goal is the sweet spot that minimizes total error on unseen data.

## Evaluation Metrics
**Classification:**
- Accuracy — % correct overall (misleading on imbalanced data)
- Precision — of predicted positives, how many were actually positive (minimizes false positives)
- Recall (sensitivity) — of actual positives, how many were caught (minimizes false negatives)
- F1 score — harmonic mean of precision and recall, useful when you need a single balanced number
- AUC-ROC — how well the model separates classes across all thresholds
- Confusion matrix — the raw breakdown (TP/FP/TN/FN) underlying all the above

**Regression:**
- MAE (Mean Absolute Error) — average absolute difference, easy to interpret
- MSE (Mean Squared Error) — penalizes large errors more heavily
- RMSE — square root of MSE, back in original units
- R² — proportion of variance explained by the model

**Clustering (unsupervised):** no ground truth to compare to, so metrics are internal —
- Silhouette score — how well-separated/cohesive the clusters are
- Inertia — within-cluster sum of squared distances (lower = tighter clusters)

## ML Project Lifecycle (Phases)
1. **Business problem framing** — define the goal and success criteria
2. **Data collection** — gather raw data
3. **Data preparation / EDA / feature engineering** — clean, explore, transform, engineer features
4. **Model training** — fit the model to prepared data
5. **Model evaluation** — test against metrics above on held-out data
6. **Deployment** — put the model into production (real-time, batch, etc.)
7. **Monitoring & retraining** — watch for drift/degradation, retrain as needed

(This maps directly onto the AIF-C01 "ML development lifecycle" task statement — know this order.)

## Hyperparameters vs. Parameters
- **Parameters**: learned automatically during training (e.g., neural network weights) — you don't set these directly.
- **Hyperparameters**: set *before* training by you (or a tuning process) — e.g., learning rate, number of trees, batch size, number of epochs, number of clusters (k).
- **Hyperparameter tuning**: the search for the best hyperparameter values — grid search (try every combination), random search (sample combinations randomly), Bayesian optimization (intelligently pick the next combination based on past results — this is what SageMaker Automatic Model Tuning uses by default).

-------
# Generative AI Model Architectures: AWS AI Practitioner (AIF-C01) Study Notes

Covers: Foundation Models, LLMs, Transformers, Self-Attention, Diffusion Models, VAE, GAN, Latent Space, plus Bedrock pricing modes and sample questions.

Relevant exam domains: **Domain 2 (Fundamentals of Generative AI)** and **Domain 3 (Applications of Foundation Models)**. The exam tests recognition and scenario matching, not math.

---

## 1. Foundation Models (FMs) and LLMs

A **foundation model** is a large model pre-trained on broad, mostly unlabeled data (text, images, code, audio) so it can be adapted to many tasks instead of one narrow job.

- Pre-trained at massive scale (billions of parameters)
- General purpose: summarize, translate, answer questions, write code, describe images
- Adaptable via prompting, RAG, or fine-tuning
- Can be single-modality or multimodal

A **Large Language Model (LLM)** is a foundation model specialized for language. It is trained to predict the next token in a sequence.

> **All LLMs are foundation models, but not all foundation models are LLMs** (for example, image-generation diffusion models are FMs but not LLMs).

### Key terms

| Term | Meaning |
|---|---|
| Token | Unit of text the model reads/writes (about 3/4 of an English word); Bedrock On-Demand bills per token |
| Context window | Max tokens the model can consider at once (prompt + response) |
| Parameters | Learned weights; more usually means more capability and cost |
| Prompt | Input given to the model |
| Temperature | Randomness; low = focused and consistent, high = creative |
| Top-p / Top-k | Limit the pool of candidate tokens sampled from |
| Max tokens | Cap on response length |
| Stop sequence | Text that tells the model to stop generating |
| Hallucination | Confident but incorrect or made-up output |
| Embeddings | Numeric vectors representing meaning; used for semantic search and RAG |
| RAG | Retrieval-Augmented Generation: fetch relevant documents and add them to the prompt |
| Fine-tuning | Further training on labeled data to specialize a model |
| Continued pre-training | Further training on unlabeled domain data |

### Customization ladder (cheapest to costliest)

1. Prompt engineering (zero-shot, few-shot, chain-of-thought, negative prompting)
2. RAG (Knowledge Bases for Amazon Bedrock)
3. Fine-tuning / continued pre-training
4. Pre-training from scratch (rarely needed)

---

## 2. Transformers

A **transformer** is the neural network architecture (from the 2017 paper *Attention Is All You Need*) behind nearly all modern LLMs and many image, audio, and multimodal models.

**Why it mattered:** RNNs/LSTMs read text one token at a time (slow, weak on long context). Transformers process **all tokens in parallel** and use **attention** to relate any token to any other, making large-scale training on GPUs practical.

### Building blocks

1. Token embeddings
2. Positional encoding (restores word order lost by parallel processing)
3. Self-attention
4. Feed-forward network
5. Residual connections + layer normalization

These form one layer; large models stack dozens to 100+ layers.

### Self-attention

Self-attention lets every token decide which other tokens matter for understanding it.

Example: *"The animal didn't cross the street because **it** was too tired."* Self-attention helps link "it" to "animal."

Each token is projected into three vectors:

- **Query (Q):** what am I looking for?
- **Key (K):** what do I offer?
- **Value (V):** the information passed along if selected

Steps:

1. Compare each Q with every K (dot product) to get relevance scores
2. Scale by sqrt(d_k) and apply softmax to get weights summing to 1
3. Take a weighted sum of the V vectors

```
Attention(Q, K, V) = softmax( Q . K^T / sqrt(d_k) ) . V
```

**Multi-head attention:** several attention heads run in parallel, each learning different relationships (grammar, coreference, nearby words). Outputs are concatenated.

### Encoder

Reads and understands the input.

- Bidirectional self-attention (each token sees left and right context)
- Output: context-aware vectors, one per input token

### Decoder

Generates output one token at a time (autoregressive).

1. **Masked self-attention:** can only attend to earlier tokens (causal mask)
2. **Cross-attention** (encoder-decoder models only): attends to the encoder's output
3. Feed-forward network, residuals, normalization
4. Final linear + softmax gives probabilities over the vocabulary

### Three architecture types

| Type | Direction | Examples | Good for |
|---|---|---|---|
| Encoder-only | Bidirectional | BERT, RoBERTa | Classification, search, embeddings, NER |
| Decoder-only | Left-to-right | GPT, Claude, Llama | Text generation, chat, code, reasoning |
| Encoder-decoder | Both | T5, BART, original Transformer | Translation, summarization |

Most modern LLMs (including those on Bedrock) are **decoder-only**.

### Use cases

Text generation and chat, code generation, translation, summarization, semantic search and RAG (embeddings), classification and entity extraction, vision (Vision Transformers), speech (recognition and synthesis), multimodal models.

### Limitations

- Attention cost grows quadratically with sequence length
- Heavy data and compute needs
- Hallucinations and bias from training data

---

## 3. Latent Space

**Latent space** is a compressed, lower-dimensional representation where a model stores the essential features of data rather than raw detail. "Latent" means hidden: the features are learned, not directly observed.

**Analogy:** a face photo has about 1 million pixels, but a face can be described by a few dozen traits (shape, skin tone, hair, expression). Those traits are the latent variables.

Properties:

- Much lower-dimensional than the raw data
- Similar items sit close together; smooth movement gives smooth changes in output
- Sampling a point and decoding it generates new plausible data
- Used in VAEs, GANs (the input noise vector), latent diffusion, and embeddings

---

## 4. Diffusion Models

A **diffusion model** generates data (mostly images, also video and audio) by starting from pure random noise and **removing noise step by step** until a coherent output appears.

### How it works

1. **Forward process (training only):** add a little Gaussian noise to real images over many steps until only noise remains
2. **Reverse process (learned):** train a network (usually a U-Net, increasingly a transformer) to predict the noise at each step
3. **Generation:** start from random noise and denoise repeatedly
4. **Text conditioning:** a text encoder (such as CLIP, transformer-based) turns the prompt into embeddings that guide denoising via cross-attention
5. **Latent diffusion:** run the process in a compressed latent space (using a VAE) instead of raw pixels, which is much cheaper (Stable Diffusion)

### Common controls

| Control | Meaning |
|---|---|
| Prompt | What you want |
| Negative prompt | What to avoid |
| CFG / guidance scale | How strictly to follow the prompt (higher = more literal) |
| Steps | Number of denoising steps (more = more detail, slower) |
| Seed | Same seed + settings reproduces the same image |

### Use cases

Text-to-image, image-to-image (style transfer, variations), inpainting (edit masked region), outpainting (extend borders), upscaling, background removal, text-to-video, audio generation, marketing and design assets.

### Strengths and weaknesses

- Strengths: very high quality, diverse outputs, stable training
- Weaknesses: slower (many steps), can have artifacts, struggles with text inside images, can reflect training bias

---

## 5. VAE (Variational Autoencoder)

A **VAE** compresses data into a latent space and reconstructs it, while keeping the latent space smooth enough to generate new data.

### How it works

1. **Encoder** outputs a probability distribution (mean and variance), not a single point
2. A point **z** is sampled from that distribution (reparameterization trick)
3. **Decoder** reconstructs the input from z
4. **Loss = reconstruction loss + KL divergence**
   - Reconstruction loss: output should resemble input
   - KL divergence: keeps the latent distribution close to a standard normal, so the space is continuous with no gaps

**Generation:** sample a random z from the normal distribution and run it through the decoder.

| | Autoencoder | VAE |
|---|---|---|
| Latent encoding | Single fixed point | Probability distribution |
| Good for | Compression, denoising | Compression and generation |
| Latent space | May have gaps | Smooth, continuous |

### Use cases

Image generation and variations, anomaly detection (anomalies reconstruct poorly), compression and denoising, synthetic data generation, component inside latent diffusion.

### Strengths and weaknesses

- Strengths: stable training, smooth latent space, good diversity
- Weaknesses: outputs often blurry compared with GAN or diffusion

---

## 6. GAN (Generative Adversarial Network)

A **GAN** trains two networks that compete:

- **Generator:** turns random noise (a latent vector) into fake data
- **Discriminator:** tries to tell real data from generated data

The generator tries to fool the discriminator; the discriminator tries to catch it. Both improve until fakes are hard to distinguish. After training, only the generator is used.

**Analogy:** counterfeiter vs detective.

### Variants

DCGAN (images), Conditional GAN (guided by a label), StyleGAN (realistic faces), CycleGAN (unpaired image translation), Pix2Pix (paired image translation).

### Use cases

Realistic image and face generation, image-to-image translation, super-resolution, synthetic data for augmentation, deepfakes (responsible AI concern).

### Strengths and weaknesses

- Strengths: sharp, realistic outputs; fast generation (single forward pass)
- Weaknesses: unstable training, **mode collapse** (limited variety of outputs), hard to evaluate and control

---

## 7. Comparison of Generative Model Families

| Model | Core idea | Typical output | Training | Quality | Diversity | Generation speed |
|---|---|---|---|---|---|---|
| Transformer / LLM | Self-attention, next-token prediction | Text, code | Stable | High | High | Token by token |
| VAE | Encode to latent distribution, decode | Images, compression, anomaly detection | Stable | Often blurry | Good | Fast |
| GAN | Generator vs discriminator | Images | Unstable | Sharp | Can suffer mode collapse | Fast |
| Diffusion | Iteratively denoise noise | Images, video, audio | Stable | Very high | High | Slower (many steps) |

**Memory tricks**

- VAE = Vague but smooth (blurry, stable)
- GAN = Game between two players (sharp, unstable)
- Diffusion = Denoise down from noise (best quality, slower)
- Transformer = Tokens + Text (attention)

---

## 8. Amazon Bedrock: On-Demand vs Provisioned Throughput

| | On-Demand | Provisioned Throughput |
|---|---|---|
| Pricing | Per token (images per image) | Per hour per model unit |
| Commitment | None | None (hourly), 1 month, or 6 months (cheaper per hour) |
| Capacity | Shared | Dedicated, guaranteed |
| Throttling risk | Yes, at quota limits | No, within purchased capacity |
| Idle cost | Zero | Pay even if unused |
| Custom (fine-tuned) models | Mostly not supported | Supported (generally required) |
| Best for | Dev, testing, spiky or unpredictable traffic | Steady high-volume production, custom models |

Related: **Batch inference** (offline jobs, usually discounted) and **cross-region inference** (route On-Demand requests across Regions to absorb spikes).

AWS Region basics: each Region has a minimum of 3 Availability Zones; each AZ is one or more discrete data centers with independent power, cooling, and networking. Mumbai is `ap-south-1` and Hyderabad is `ap-south-2`.

---

## 9. AWS Services Mapping

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
| Generative AI assistants for work/developers | Amazon Q (Business, Developer) |
| Filter harmful content, deny topics, redact PII | Guardrails for Amazon Bedrock |
| Ground answers in your data (RAG) | Knowledge Bases for Amazon Bedrock |
| Multi-step task automation | Agents for Amazon Bedrock |
| Vector storage for embeddings | OpenSearch Serverless, Aurora PostgreSQL (pgvector), others |

Titan Image Generator supports inpainting, outpainting, variations, background removal, and adds an **invisible watermark** to identify AI-generated images (a responsible AI point). Model lineups change often, so verify current names in the Bedrock console and the official exam guide.

### Evaluation metrics: ROUGE, BLEU, BERTScore

All three compare **generated text** against **reference text** written by humans.

| | **ROUGE** | **BLEU** | **BERTScore** |
|---|---|---|---|
| **Full name** | Recall-Oriented Understudy for Gisting Evaluation | Bilingual Evaluation Understudy | BERT-based similarity score |
| **Main use** | **Summarization** | **Machine translation** | Any text generation where meaning matters (summaries, translation, paraphrase, Q&A) |
| **How it works** | Measures n-gram or sequence overlap, focused on **recall** (how much of the reference appears in the output) | Measures n-gram overlap, focused on **precision** (how much of the output appears in the reference), with a **brevity penalty** | Compares **contextual embeddings** of tokens using cosine similarity, then reports precision, recall and F1 |
| **Captures meaning?** | No, exact word match only | No, exact word match only | **Yes**, handles synonyms and paraphrases |
| **Score range** | 0–1 | 0–1 | Roughly 0–1 (higher is better) |

### ROUGE variants
- **ROUGE-1:** unigram (single word) overlap.
- **ROUGE-2:** bigram (two-word) overlap.
- **ROUGE-L:** longest common subsequence, which reflects sentence structure and word order.

### Limitations
- **ROUGE and BLEU** penalize valid paraphrases because they only match exact words. "The car is fast" and "The automobile is quick" would score low.
- **BERTScore** is better for semantic equivalence but is computationally heavier and depends on the underlying embedding model.
- None of them check **factual correctness**, tone or safety, so human evaluation is still needed for subjective qualities such as brand voice.

### Memory tricks
- **ROUGE → Recall → summaRies**
- **BLEU → Bilingual → translation (precision)**
- **BERTScore → Bert understands meaning → semantic similarity**

### Exam tips
- Summarization quality → **ROUGE**.
- Translation quality → **BLEU**.
- Semantic similarity or paraphrase tolerance → **BERTScore**.
- Subjective qualities such as tone and helpfulness → **human evaluation**.
- General language understanding benchmarks → **GLUE / SuperGLUE**.
- **Amazon Bedrock Model Evaluation** supports automatic metrics (including ROUGE and BERTScore, plus accuracy, robustness and toxicity) and human-based evaluation.

### temperature, Top P, Top K

All three are **inference parameters**. They control how the model picks the next token from its probability distribution. They change output style and randomness only. They do **not** change the model's knowledge or weights.

Running example: the model has to complete *"The sky is ___"* and assigns these probabilities:

| Token | blue | clear | cloudy | falling | green | purple |
|---|---|---|---|---|---|---|
| Probability | 0.50 | 0.20 | 0.15 | 0.08 | 0.05 | 0.02 |

---

### 1. Temperature

**Definition:** Temperature rescales the probability distribution before sampling and controls how random the output is.
- **Low (near 0):** the distribution sharpens, so the model almost always picks the top token. Output is focused, predictable and repeatable.
- **High (near 1 or above):** the distribution flattens, so unlikely tokens get picked more often. Output is more varied and creative, with more risk of errors.

**Example:**
- Temperature 0.1: "The sky is **blue**" nearly every time.
- Temperature 1.0: you might get "The sky is **cloudy**", "**clear**", or occasionally "**falling**".

**Use cases:**

| Setting | Use cases |
|---|---|
| Low (0–0.3) | Factual Q&A, data extraction, classification, code generation, customer support answers, summarizing legal or medical text |
| Medium (0.4–0.7) | General chatbots, email drafting, balanced summaries |
| High (0.8–1.0) | Brainstorming, poetry, storytelling, marketing taglines, idea generation |

---

### 2. Top K

**Definition:** Top K limits sampling to the **K most likely tokens**. Everything else is discarded, then the model samples among those K (probabilities renormalized). K is a **fixed count**.

**Example:**
- **Top K = 1:** only "blue" (greedy, fully deterministic).
- **Top K = 3:** the model samples among **blue, clear, cloudy** only. "Falling", "green" and "purple" are impossible.
- **Top K = 50:** a very wide pool, so output is more diverse.

**Use cases:**
- **Low K (1–10):** precise, safe outputs such as code, structured data and factual answers.
- **High K (40–100+):** creative writing and open-ended generation.

**Weakness:** K is fixed regardless of the distribution. If one token has 95% probability, K=3 still lets two low-quality tokens in.

---

### 3. Top P (nucleus sampling)

**Definition:** Top P picks the **smallest set of tokens whose cumulative probability reaches P**, then samples only from that set. The pool size **adapts** to the model's confidence.

**Example (using the table above):**
- **Top P = 0.5:** blue alone reaches 0.50, so the pool is {blue}.
- **Top P = 0.9:** blue (0.50) + clear (0.70) + cloudy (0.85) + falling (0.93), so the pool is {blue, clear, cloudy, falling}.
- If the model is very confident (e.g., "blue" = 0.95), Top P = 0.9 picks just **one** token. Top K = 3 would still allow three.

**Use cases:**
- **Low P (0.1–0.5):** factual, precise, consistent responses.
- **High P (0.9–1.0):** creative or conversational content, where the model should consider a wider range.

---

### Comparison

| | Temperature | Top K | Top P |
|---|---|---|---|
| **Controls** | Shape of the distribution (how "peaked" it is) | A fixed number of candidate tokens | A cumulative probability threshold |
| **Range** | 0 to 1 in most Bedrock models (some allow higher) | Integer (1, 2, 50, …) | 0 to 1 |
| **Pool size** | Not applicable, all tokens remain possible | Fixed | Dynamic |
| **Lower value →** | More deterministic | Fewer choices, safer | Fewer choices, safer |
| **Higher value →** | More random and creative | More choices, more diverse | More choices, more diverse |

---

### Exam tips
- **Deterministic, factual output** (code, extraction, compliance answers) → **low temperature** (and/or low Top P / Top K).
- **Creative output** (brainstorming, stories) → **high temperature**.
- Top K = a **number** of tokens. Top P = a **probability** threshold.
- Temperature changes *how likely* tokens are. Top K and Top P change *which tokens are eligible*.
- They are typically **combined**, and a common practice is to tune temperature **or** Top P, not both aggressively at once.
- Other inference parameters to remember: **max tokens / response length** (caps output size) and **stop sequences** (text that ends generation).
- Changing these parameters does **not** retrain the model or reduce hallucinations by itself. For hallucinations, use RAG, Guardrails and prompt engineering.

---

## 10. Classic Exam Traps

- "Generate images from text" means a **diffusion model**, not an LLM
- "Understand context between words using attention" means a **transformer**
- "Use company documents without retraining" means **RAG / Knowledge Bases**, not fine-tuning
- "Consistent, factual answers" means **lower temperature**
- "Steady high-volume workload or custom model" means **Provisioned Throughput**; "spiky or experimental" means **On-Demand**
- "No ML expertise, just call a model" means **Bedrock**; "build, train, tune your own model" means **SageMaker**
- "Filter harmful content or block topics" means **Guardrails**
- "Two networks competing, generator vs discriminator" means **GAN**
- "Mode collapse" is always a **GAN** issue
- "Compress and generate with probabilistic latent space" means **VAE**
- "Anomaly detection via reconstruction error" means **autoencoder / VAE**
- "Generate synthetic data to augment a dataset" can mean **GAN or VAE**
- Bedrock's headline image models are **diffusion-based**
- All LLMs are FMs; not all FMs are LLMs

---

## 11. Sample Questions

**Q1.** A marketing team wants to create product images from text descriptions with minimal setup. Which combination is the best fit?
- A. A VAE trained on SageMaker
- B. A diffusion-based image model on Amazon Bedrock
- C. An encoder-only transformer
- D. A GAN trained from scratch

**Answer: B.** Bedrock offers managed diffusion-based image models (Titan Image Generator, Stable Diffusion, Nova Canvas) with no infrastructure to manage.

---

**Q2.** Which mechanism allows a transformer to determine which other words in a sentence are most relevant to a given word?
- A. Convolution
- B. Self-attention
- C. Backpropagation through time
- D. Pooling

**Answer: B.** Self-attention uses Query, Key, and Value vectors to weigh relationships between tokens.

---

**Q3.** What does the reverse process of a diffusion model learn to do?
- A. Compress images into a latent vector
- B. Classify real vs fake images
- C. Predict and remove noise at each step
- D. Translate text between languages

**Answer: C.**

---

**Q4.** A GAN keeps generating only a small set of similar images regardless of the input noise. What is this problem called?
- A. Overfitting
- B. Vanishing gradient
- C. Mode collapse
- D. Underfitting

**Answer: C.** Mode collapse means low diversity in the generator's outputs.

---

**Q5.** Which two components make up the VAE loss function?
- A. Cross-entropy and accuracy
- B. Reconstruction loss and KL divergence
- C. BLEU and ROUGE
- D. Generator loss and discriminator loss

**Answer: B.** D is the GAN loss.

---

**Q6.** Which transformer architecture type do most modern LLMs such as Claude and Llama use?
- A. Encoder-only
- B. Encoder-decoder
- C. Decoder-only
- D. Recurrent

**Answer: C.**

---

**Q7.** A company wants its chatbot to answer using internal policy documents without retraining the model. Which solution is the most cost-effective?
- A. Pre-train a new foundation model
- B. Fine-tune the model weekly
- C. Use RAG with Knowledge Bases for Amazon Bedrock
- D. Increase the temperature

**Answer: C.** RAG grounds answers in your data without changing model weights.

---

**Q8.** A team wants a more deterministic, factual response from an LLM. Which inference parameter change helps?
- A. Increase temperature
- B. Decrease temperature
- C. Increase max tokens
- D. Remove stop sequences

**Answer: B.** Lower temperature reduces randomness.

---

**Q9.** A customer-facing application has steady, heavy traffic around the clock and cannot tolerate throttling. Which Bedrock mode should they use?
- A. On-Demand
- B. Provisioned Throughput
- C. Batch inference only
- D. Playground

**Answer: B.** Provisioned Throughput gives dedicated capacity and consistent performance.

---

**Q10.** A developer fine-tunes a model in Amazon Bedrock and wants to run it. What is generally required?
- A. On-Demand mode
- B. Provisioned Throughput
- C. Cross-region inference
- D. PartyRock

**Answer: B.** Most custom models require Provisioned Throughput.

---

**Q11.** Which statement about latent space is correct?
- A. It is the raw pixel representation of an image
- B. It is a compressed, lower-dimensional representation capturing essential features
- C. It is the set of labeled training examples
- D. It is the output vocabulary of an LLM

**Answer: B.**

---

**Q12.** A company wants to detect unusual sensor readings by measuring how poorly a model can reconstruct them. Which model type fits best?
- A. Autoencoder / VAE
- B. Decoder-only LLM
- C. Diffusion model
- D. Encoder-decoder translation model

**Answer: A.** Anomalies reconstruct poorly, producing high reconstruction error.

---

**Q13.** Which statement is true?
- A. All foundation models are LLMs
- B. All LLMs are foundation models, but not all foundation models are LLMs
- C. Diffusion models are LLMs
- D. LLMs cannot be foundation models

**Answer: B.**

---

**Q14.** Which metric is commonly used to evaluate the quality of text summarization?
- A. BLEU
- B. ROUGE
- C. Mean squared error
- D. AUC

**Answer: B.** BLEU is for translation; ROUGE is for summarization.

---

**Q15.** A company needs to block certain topics and redact PII in an application that uses a Bedrock model. Which feature should it use?
- A. Provisioned Throughput
- B. Knowledge Bases
- C. Guardrails for Amazon Bedrock
- D. Agents for Amazon Bedrock

**Answer: C.**

---

**Q16.** Why is latent diffusion (as in Stable Diffusion) more efficient than diffusion in pixel space?
- A. It uses fewer training images
- B. It denoises in a compressed latent space produced by a VAE
- C. It replaces the U-Net with a GAN
- D. It skips the noise process entirely

**Answer: B.**

---

**Q17.** Which model family is known for training instability because two networks are trained against each other?
- A. Transformer
- B. VAE
- C. GAN
- D. Diffusion

**Answer: C.**

---

**Q18.** A team needs to build, train, and deploy its own custom ML model with full control over infrastructure. Which service fits best?
- A. Amazon Bedrock
- B. Amazon SageMaker
- C. PartyRock
- D. Amazon Q Business

**Answer: B.**

---

## 12. Quick Self-Check (flashcards)

| Question | Answer |
|---|---|
| What does a transformer use to relate tokens? | Self-attention (Q, K, V) |
| Which part of a transformer generates text? | Decoder (masked self-attention) |
| Which part understands input? | Encoder (bidirectional) |
| What starts a diffusion generation? | Random noise |
| What are the two GAN networks? | Generator and discriminator |
| Why is a VAE latent space useful for generation? | It is smooth and continuous because of KL divergence |
| Cheapest way to improve model output? | Prompt engineering |
| Billing unit for Bedrock On-Demand text models? | Input and output tokens |
| Billing unit for Provisioned Throughput? | Per hour per model unit |
