# Language models as priors for structured inference

## 1. Bayesian ML for safety

Probabilsitic inference: 
$$
p(z / x) \propto p(x/z)p(z)
$$

Amortisation

$$
q_\theta (z/x)
$$

- $z$ - might be interpreted as an explanation

Intractable sequence-level problems:
- sequence level tempered sampling
- fixed-level sampling
- lexical constraints

Reasonining as posterior inference:
- given X->Y find explanation Z
- generalization of CoT
- $p(z/x,y) \propto p(y/x,z)p(z/x)$

Observations:

- according to speaker: LLMs more useful as priors
- LLM provably does posterior predictive modelling $P(y/x)$

## 2. Amortised inference

Example intractable inference method [1]:

1. learn $P(z/x)$ (using RL)
2. $z ~ P(z/x)$
3. $p(y/x) = \sum_z q(z/x) p(y/x,z)$ - why is this better?

[2] **EM-based** method (!!!!)

- very interesting paper
- started reading into it
- stopped listening :<

---

[1] Hu, E., Jain, M., Elmoznino, E., Kaddar, Y., Lajoie, G., Bengio, Y., & Malkin, N. (2024, May). Amortizing intractable inference in large language models. In International Conference on Learning Representations (Vol. 2024, pp. 43053-43083).

[2] Ruan, Y., Band, N., Maddison, C. J., & Hashimoto, T. (2025). Reasoning to learn from latent thoughts. arXiv preprint arXiv:2503.18866.
