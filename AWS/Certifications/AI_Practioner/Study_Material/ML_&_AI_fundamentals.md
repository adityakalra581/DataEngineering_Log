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
