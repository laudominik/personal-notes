# 1. Probability


## Probability triplet

$(\Omega, \mathcal F, P)$

- $\Omega$ - set of all elementary events
- $\mathcal F$ - $\sigma$-algebra of all possible events
    - $\mathcal F  = 2^\Omega$ - discrete case
    - $\mathcal F = \mathcal B(\Omega)$ - continuous case (Borel set)
- $P: \mathcal F \rightarrow [0,1]$
    1. $P(\emptyset) = 0$
    2. $P(\bigcup_{\omega \in \Omega} \omega) = 1$
    3. $AB = \emptyset \rightarrow P(A \cup B) = P(A) + P(B)$

**probability simplex**

a space $\Delta^{n-1} \subseteq \mathbb R^n$

For $(p_1, \dots, p_n) \in \Delta^{n-1}$
- $\sum_{j=1}^n p_j = 1$
- $p_j \ge 0; \quad \forall j$


## RVs

$X: \Omega \rightarrow \mathbb R$
e.g. $\Omega = \{H, T\}$

$$
X(\omega) = \begin{cases}
0 \quad \omega = H \\
1 \quad \omega = T
\end{cases}
$$

$$
\operatorname{Im}(X) = \{X(\omega): \omega \in \Omega\}
$$
- if $\operatorname{Im}(X)$ finite, then $X$ is discrete
- if $\operatorname{Im}(X)$ is infinite, then $X$ is continuous


## CDF

$F_X: \mathbb R \rightarrow [0,1]$

$$
F_X(x) = P(X \le x)
$$

**Quantile**

$q_p: [0,1] \rightarrow \mathbb R$

$$
P(X \le q_p(x)) \ge p \\
P(X \ge q_p(x)) \ge 1-p
$$

## Moments

moment | eq
--|--
$\mu$ | $\mathbb E X$
k-th ordinary | $\mathbb E X^k$
k-th central | $\mathbb E (X-\mathbb EX)^k$
k-th standardized | $\mathbb E\left(\frac{X - \mu}{\sigma}\right)^k$
variance | 2-nd central
skewness | $\gamma_1 = \mathbb E\left(\frac{X - \mu}{\sigma}\right)^3$

- skewness measures symmetry ($\gamma_1 > 0$ -> more mass greater than mean)

## Distributions

$U(a,b) = \frac{1}{b-a}$
- $\mu = \frac{a+b}{2}$
- $\sigma^2 = \frac{(b-a)^2}{12}$
- $\gamma_1 = 0$
- represents no knowledge

$\mathcal N(x\mid \mu, \sigma^2)$: $X=\mu + \sigma \mathcal N(0, 1)$
- mean of many random iid variables
- CLT: $\sum_{j=1}^N X_j \sim \mathcal N(\mu, \sigma^2/n)$ (for $X_j$ i.i.d.)

$\operatorname{Log-normal}(x\mid \mu, \sigma^2)$: $X=\exp(\mu + \sigma \mathcal N(0,1))$
- product of many random iid variables
- $Z=\prod_{j=1}^N X_j$
- $\ln (Z) = \sum_{j=1}^N \ln(X_j) \sim \mathcal N$
- CLT for logs

$\chi^2(x\mid n)$
- $X_j \sim \mathcal N(0,1)$
- $Z=\sum_{j=1}^N X_j^2\sim \chi^2$
- $\sqrt{Z} \sim \chi$
- $n$ - degrees of freedom

$\exp(x\mid \lambda) = \lambda \exp(-\lambda x)$
- time between continuous independent events

$\operatorname{Geometric}(k \mid p) = (1-p)^{k-1}p$
- $p$ - success proba
- number of failures before the first success

$\operatorname{Gamma}(x \mid k, \lambda)$
- sum of $k$ random iid exponential variables
- $\operatorname{Gamma}(x \mid 1, \lambda) = \exp(x\mid \lambda)$

## Law of large numbers

Assumptions on $X_j$:
- iid
- square integrable

$$
\frac{1}{N} \sum_{j=1}^N X_j \rightarrow \mathbb{E} X
$$
