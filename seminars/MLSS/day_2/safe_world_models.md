# Safe World Models Require Reasoning

## Background

> All design choices cause inductive biases.

Observations [1,2]:
- lowest per-class test error values are reached at different model complexities
- different classes are affected by the regularization in different ways
- penalties are a source of bias, because they incentivize the network to prioritize differently the penalized features

Observations [3]:
- features learned by reconstruction learn high-variance features, when in reality we want the low-variance ones
- high-magnitude eigenvalues are aligned first

Correction of pretraining biases:
- Guillotine regularization
- ...

## JEPA training

**JEPA building blocks**

- $z_t = encoder(x_t)$
- $z_{t+1} = encoder(x_{t+1}$
- $z_{t+1} \approx predictor(z_t)$
- JEPA doesn't use reconstruction loss (see: [3]), therefore doesn't need decoder
- but it causes representational collapse to trivial solutions (e.g. $z_t = c$)

Collapse prevention methods:

- bad non-JEPA: decoder reconstruction term
- old way: two encoders (one is moving average), very sensitive to hyperparameters
- sigreg

**SIGReg** [4]

- regularizer
- we want a "healthy" distribution of $z$ (s.t. it minimises worst-case risk)
- the distribution is isotropic gaussian
  - intuition, collapse = becomes 1d
- Cramer-Wold
  - We want to project multivariate $p(z)$ and $\mathcal N(0, I)$ aligned
  - We might achieve that by projecting $p(z)$ to 1d and matching it to $\mathcal N(0, 1)$ (L2 between characteristic functions of $\mathcal N$ and empirical)
  - read some more

JEPA **planning** action sequence $\boldsymbol a$

- gradient based
- genetic algorithms

## JEPAs safety

**Issues**

- degradation in different environmental contexts [5]
- outcome/observation-based understanding of unexpected environment behavior - it's not what current models are designed to do (see next part for the explanation)

Observation of unexpected behavior of the environment
- hidden noise variable $\varepsilon_t$ impacting the outcome of "pushing the door" that was impossible to infer from the previous state
- reasoning = inference of $\hat \varepsilon_t = f(z_t, a, z_{t+1})$ (cannot predict only from $z_t$)
- abductive reasoning (world models name for counterfactual reasoning) e.g. what if we push the door with more strength

Solution [6]:
- make predictor invertible
- train predictor together with $\varepsilon$ i.e. $f(z_t, a_t, \varepsilon)$
- $\varepsilon_t = f^{-1}(z_t, a_t, z_{t+1})$
- do counterfactual analysis $\hat z_{t+1} = f(z_t, a_t, \hat \varepsilon_t)$

**Limitations**

- no scaling laws
- very weak impact of $\varepsilon$ - not captured by the model

Packages for training JEPA:

```
stable-pretraining
stable-worldmodels
```

---

[1] Balestriero, R., Bottou, L., & LeCun, Y. (2022). The effects of regularization and data augmentation are class dependent. Advances in Neural Information Processing Systems, 35, 37878-37891.

[2] Balestriero, R., Misra, I., & LeCun, Y. (2022). A data-augmentation is worth a thousand samples: Exact quantification from analytical augmented sample moments. arXiv preprint arXiv:2202.08325.

[3] Balestriero, R., & LeCun, Y. (2024). Learning by reconstruction produces uninformative features for perception. arXiv preprint arXiv:2402.11337.

[4] Balestriero, R., & LeCun, Y. (2025). Lejepa: Provable and scalable self-supervised learning without the heuristics. arXiv preprint arXiv:2511.08544.

[5] Bisulco, A., Wang, J., Daniilidis, K., Balestriero, R., & Chaudhari, P. (2026). OctoSense: Self-Supervised Learning for Multimodal Robot Perception. arXiv preprint arXiv:2606.27317.

[6] Unpublished