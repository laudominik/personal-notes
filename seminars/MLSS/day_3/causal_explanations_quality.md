# Towards quality standards for causal explanations

## Background

**Tools**

Causal Bayesian Network
$$
P(X_1,...,X_d) = \Pi P(X_j | Parents(X_j))
$$

Structural Causal Model
$$
X_j = f_j(Parents(X_j), \varepsilon_j)
$$

Bayesian network properties - well known.

- Hard intervention: set variable $X_j$ to $x_j'$.
  - Bayes networks: Modeled as replacing the $P(X_j\mid Parents(X_j))$ with point (Kronecker delta)
  - SCM: replace single structural equation
- Soft intervention: replace prob. with some other prob.

**Causal attribution problems**

Explain:
- distribution change - which parameter changes distribution mainly
- statistical fluctuation - explain the variance of $X_n$
- outliers

**Sparse mechanisms shift**

- we assume the change in a target distribution is caused by the least amount of changing factors 
- however it might not be the case which means we might've missed a common cause in the model for these i.e. we think $X_1$, $X_2$, $X_3$ caused the change, but in reality they depend on a common variable $X_0$ which is the true source of change

**Quantitative contribution**

1. Decompose
2. Replace one by one with different distributions
3. The resulting differences $\Delta$ correspond to contribution of each component's change

**Packages for causal explanations**

```
dowhy
```

## RCA of outliers

1. Compute quantitative contribution of all of the variables
2. Unroll target function $X_d=F(N_1,...,N_d)$ as a function of the noise by inserting the functions of its parents (and so on)
3. Compute noise values $n_1, .., n_d$ from $n_1, ...,n_d = F^{-1}(X_d)$ by sampling (its not always possible, for fixed parents it is invertible)
4. Set values one by one to some normal values $n_j'$ and see if it fixes the outlier - we get quantitative explanation why outlier happens.
    - $S(x) = -logC(g(x))$ - outlier score, we see where in tail $x$ lies (one sided)
    - contribution = log of ratio of some things (incl)
    - didn't really get that part, but it's just an equation
  
Frequent events cannot explain rare events.
- Example: roll d4 and d100 and see $Prob(X_1, X_2 = 1, 1)$, it is explained more by d100


## RCA in unknown graphs [1]

Assumptions:
- $d$ variables
- causal graph can have cycles
- linked with linear equations $x = Ax + n$

Root cause:
- $\Sigma_X^{-1} x$ has extreme values only for root cause and parents

## Causal pathways [2]

Does the pathway explain some event?

- $\{B_1,\dots, B_N\}=B$ - binary variables
- we observe $B_t=1$ where the probability is low
- we look for RCA $B_R\subset B$

$$
P(B | B_R=1) \ge P(B_t=1)
$$

---
[1] Schkoda, D., & Janzing, D. (2025). Root Cause Analysis of Outliers in Unknown Cyclic Graphs. arXiv preprint arXiv:2510.06995.

[2] Haghighat, A., & Janzing, D. (2026). Formalizing and falsifying causal pathways of rare events. arXiv preprint arXiv:2605.31254.