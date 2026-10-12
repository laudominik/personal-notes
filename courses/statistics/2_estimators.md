# Estimators

Statistic - a function of data

## Estimator

$\hat \theta: \mathbb{R}^n \rightarrow \mathbb{R}$
- a statistic
- also a random variable
- estimate is a realization of this random variable and it is a scalar

**Desired properties**
- consistency: $\lim_{n\rightarrow \infty} \hat \theta = \theta$
- unbiasedness: $\mathbb E [\hat \theta] = \theta$
- estimator is efficient when it has the lowest variance

Examples:
- unbiased but inconsistent: $\hat\theta = \mu(x_1, x_2, x_3 \dots x_n) = x_1$. 
    - $\mathbb E [\hat \theta] = \mathbb E x_1 = \mathbb EX = \mu$
    - variance does not shrink

## Information inequality

Suppose we observe data $x$. Let's assume that it is generated from some probability distribution $f(x\mid \theta)$. ($\theta$ is some parameter). So log-likelihood of the data is

$$
\log f(x\mid \theta)
$$

Score function: $\mathcal S(x;\theta) = \frac{\partial}{\partial \theta} \log f(x\mid \theta)$
- tells us whether data fits the assumed $\theta$
- e.g. $\mathbb E[\mathcal S(x; \theta)] = 0$ for true $\theta$

Fisher information

$$
\mathcal{I}(x;\theta) = \mathbb V{\mathcal S(x;\theta)} = \mathbb E[\mathcal S(x;\theta)^2] + \underbrace{(\mathbb E[\mathcal S(x;\theta)])^2}_{0} = \mathbb E[\frac{\partial^2}{\partial \theta^2} \log f(x\mid \theta)] 
$$
- it measures the sharpness of the peak
- if sharply picked ($\mathcal{I}$ big) then we know $\theta$ precisely
- if flatter ($\mathcal{I}$ small) then we do not know $\theta$

**Cramer-Rao**

- Let $\hat \theta$ be an unbiased estimator

$$
Cov[\hat\theta, \mathcal S] = \mathbb E[\hat\theta \mathcal S] - \mathbb E[\hat\theta] \underbrace{\mathbb E[ \mathcal S]}_0 = \int \hat\theta \mathcal S f(x\mid \theta)dx = ... = 1
$$

- from Cauchy-Schwarz

$$
Cov(T,s)^2 \le \mathbb{V}(\hat \theta) \mathbb{V}(\mathcal S)
$$

$$
1 \le \mathbb{V}(\hat \theta) \mathbb{V}(\mathcal S) \rightarrow \mathbb{V}(\hat \theta) \ge \frac{1}{\mathcal I(x;\theta)}
$$


## Central measures

Central tendency measure function $m: \mathbb R \rightarrow \mathbb R$
- mean property
- invariance under exchange
- homogenity

## Spread measures

$$
\mathbb VX = \mathbb E[X^2] - (\mathbb EX)^2
$$


$$\text{IQR} = q_{.75} - q_{.25}$$

- mean absolute distance, more robust to outliers
- median absolute distance

**Boxplots**

- the whiskers: [$Q1 - 1.5\text{IQR}$, $Q3 + 1.5\text{IRQ}$]
- the box: [$Q1$, $Q3$]
