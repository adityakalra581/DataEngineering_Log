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
## What is a transformer?

A **transformer** is a neural network architecture introduced in the 2017 paper *"Attention Is All You Need"*. It is the foundation of nearly all modern LLMs and many image, audio, and multimodal models.

Before transformers, language models used RNNs/LSTMs, which read text **one token at a time**, so they were slow to train and struggled with long-range context. Transformers process **all tokens in parallel** and use **attention** to relate any token to any other, which made training at massive scale on GPUs practical.

## Core building blocks

1. **Token embeddings:** each token becomes a vector.
2. **Positional encoding:** added to the embeddings, because processing in parallel loses word order otherwise.
3. **Self-attention:** lets each token look at the other tokens.
4. **Feed-forward network:** processes each token's representation further.
5. **Residual connections + layer normalization:** keep deep stacks stable to train.

These are bundled into a **layer** (block), and the model stacks many of them (dozens to 100+ in large LLMs).

## Self-attention mechanism

Self-attention lets every token decide **which other tokens matter for understanding it**.

Take: *"The animal didn't cross the street because **it** was too tired."* To interpret "it," the model needs to link it to "animal," not "street." Self-attention learns to do this.

**How it works:** each token's vector is projected into three vectors:

- **Query (Q):** "what am I looking for?"
- **Key (K):** "what do I contain / offer?"
- **Value (V):** "the information I pass along if selected"

Then:

1. Compare each token's **Q** with every token's **K** (dot product) to get **relevance scores**.
2. Scale the scores (divide by √d_k) and apply **softmax** to turn them into weights that sum to 1.
3. Take a **weighted sum of the V vectors** using those weights.

```
Attention(Q, K, V) = softmax( Q·Kᵀ / √d_k ) · V
```

The result is a new vector for each token that blends in information from the tokens most relevant to it, so "it" ends up carrying information about "animal."

**Multi-head attention:** the model runs several attention "heads" in parallel, each with its own Q/K/V projections. One head might track grammar, another coreference, another nearby words. The outputs are concatenated and mixed.

## Encoder

The encoder **reads and understands** the input.

```
Input tokens → Embedding + Position
   → [ Multi-head self-attention → Add & Norm → Feed-forward → Add & Norm ] × N
   → Contextual representations
```

- Self-attention is **bidirectional**: every token sees tokens both to its left and right.
- The output is a set of **context-aware vectors**, one per input token, capturing meaning.

## Decoder

The decoder **generates** output one token at a time. Each layer has three parts:

1. **Masked self-attention:** each position can only attend to **earlier** tokens, not future ones. This "causal mask" prevents cheating during training and matches how generation works.
2. **Cross-attention** (only in encoder-decoder models): the decoder's queries attend to the **encoder's output** (keys and values), so it can consult the input while generating.
3. **Feed-forward network**, plus residuals and normalization.

At the end, a linear layer + softmax produce probabilities over the vocabulary. One token is chosen, appended, and the process repeats (**autoregressive generation**).

**Translation example (English → Hindi):** the encoder reads the English sentence; the decoder produces Hindi words one by one, using cross-attention to look back at the relevant English words.

## Three architecture types

| Type | Uses | Direction | Examples | Good for |
|---|---|---|---|---|
| **Encoder-only** | Encoder | Bidirectional | BERT, RoBERTa | Classification, search, embeddings, NER |
| **Decoder-only** | Decoder (no cross-attention) | Left-to-right | GPT, Claude, Llama | Text generation, chat, code, reasoning |
| **Encoder-decoder** | Both | Both | T5, BART, original Transformer | Translation, summarization |

Most modern LLMs, including those you'd use on Bedrock, are **decoder-only**.

## Use cases

- **Text generation and chat:** assistants, content writing, Q&A
- **Code:** generation, completion, review, debugging
- **Translation:** machine translation across languages, including Indian languages
- **Summarization:** documents, meetings, support tickets
- **Search and RAG:** embedding models turn text into vectors for semantic search
- **Classification and extraction:** sentiment, intent detection, entity extraction, document parsing
- **Vision:** Vision Transformers (ViT) for image classification and detection
- **Image generation:** diffusion models often use transformer components
- **Speech:** speech recognition (Whisper) and text-to-speech
- **Multimodal:** models that combine text, image, audio, and video
- **Science:** protein structure prediction and other domains

## Limitations

- Attention cost grows **quadratically** with sequence length, which is why long context windows are expensive.
- They need large data and compute to train.
- They can hallucinate and inherit biases from training data.

Want me to walk through a small numeric example of attention scores, explain positional encoding in more detail, or cover how the context window and KV cache affect Bedrock cost and latency?
