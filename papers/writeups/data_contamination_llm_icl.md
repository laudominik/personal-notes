# Detecting Data Contamination in LLM via In-Context Learning (Write-up)

CoDeC (Contamination Detection via Context)

## Idea

- Context sampled from dataset seen during training provides no new info to the model, unseen data improves prediction
- Count of data in training dataset that reduces confidence is a measure of contamination

### Contamination definition (in this article)

How much of the targeted dataset $\mathcal{D}$ or very similar data was in the training set.

### Threat model

- Gray-box model: token probabilities (logit vector)

### Addressed gaps

- Existing methods assume access to training data
- Existing methods are volatile to hyperparameters
- -||- have low interpretability

## Method

1. For each datapoint $x \in \mathcal{D}$, obtain average of log-likelihood for each token
2. Do the same for $x_1|x_2|\dots|x_n|x$, where $x_i$ are additional samples from $\mathcal{D}$
3. Score: $\Delta(x) = logprob_{\text{in-context}}(x) - logprob_{\text{baseline}}(x)$
4. Contamination $= \frac{1}{N} \sum_{i=1}^N \mathbb{1}[\Delta(x_i) < 0]$

