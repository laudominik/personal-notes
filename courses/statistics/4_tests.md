# 4. Hypothesis testing

## p-value

Let $X$ - statistic, $\bar x$ - observation


p value: Probability of obtaining a value as extreme as $x$ assuming $H_0$
- right side $p = P(X > x )$
- left side $p = P(X < x)$
- both side $p = P(|X| > |x|)$

p-value < 0.05 -> reject $H0$

**Error types**

- $H_0$ is true -> rejection causes type 1 error (false positive)
- $H_0$ is false -> non-rejection causes type 2 error (false negative)


## Z-test

- $X_j \sim f(\mu, \sigma^2)$ 
- known variance $\sigma^2$
- unknown mean $\mu$
- hypothesized mean $\mu_0$

$H_0$: $\mu=\mu_0$

Let's say we use for the mean estimation an estimator
$$
\hat\mu = \frac{X_1 + \dots + X_n}{n}
$$

and we observe $\bar\mu$, under $H_0$ $p(\hat\mu)=\mathcal N(\hat\mu \mid \mu_0, \sigma^2/n)$ (from CLT)

$$
p=\mathbb{P}(|\hat\mu - \mu| \ge |\bar\mu - \mu| \mid H_0) = \mathbb{P}(|\frac{\hat\mu - \mu_0}{\sigma/\sqrt{n}}| \ge |\frac{\bar\mu - \mu_0}{\sigma/\sqrt{n}}|) = \mathbb{P}(|\hat z| \ge |\bar z|)
$$

then 

$$
p = \mathbb{P}(|\hat z| \ge |\bar z|) = \mathbb{P}(\hat z \ge |\bar z|) + \mathbb{P}(\hat z \le -|\bar z|) = 2[1 - \Phi(|\bar z|)]
$$


## t-test

- $X_j\sim \mathcal N(\mu, \sigma^2)$ iid
- $\sigma$ - unknown, $\mu$ - unknown
- $\mu_0$ - hypothesized

$H_0$: $\mu = \mu_0$

$\hat \mu$, $\hat s$ 
- unbiased mean and stdev estimators
- $\hat\mu$, $\hat\sigma$ are also **independent** (true for normal distribution)

Step 1: plug in to the z-test the now-random $\hat s$ and $\hat \mu$

$$
\hat t = \frac{\hat \mu - \mu_0}{\hat s/\sqrt{n}} = \frac{\hat z}{\hat s/\sigma} \sim \operatorname{t-Student}(n-1)
$$

Step 2: compute p-value

$$
p = \mathbb{P}(|\hat t| \ge |\bar t| \mid H_0) = 2F_{n-1}(-|\bar t|)
$$

where $F_\nu$ - CDF of t-Student with $\nu$ DOF


## Kolmogorov-Smirnov

$H_0$: $X_j$ has a distribution with CDF $F_0$

$\hat F(x) = \frac{1}{n}\sum_{j=1}^n \mathbb{1}[X_j \le x]$

$D_n = \sup_x |\hat F(x) - F_0(x)|$

compare $\sqrt{n}D_n$ against Kolmogorov distribution 

## Other tests

- chi-squared
    - $X_j$, $Y_j$ - variables
    - $H_0$: variables are independent
- Wilcoxon
    - t-student alternative for non-normal data
- F-test
    - $H_0$: variances of two populations are equal
- Shapiro-Wilk
    - $H_0$: data distribution is normal

## FWER

$$
FWER = 1 - (1-\alpha)^m
$$
- Family-wise error rate
- carrying out $m$ indepentent tests
- error rate grows with more tests

**Bonferroni correction**: $\alpha' = \alpha/m$

## QQ-plots

- X axis: first dataset quantiles
- Y axis: second dataset quantiles

## Power analysis

$$
Power = 1-\beta
$$ 

probability of rejecting $H_0$ when in fact it is not true

The question is: what would the test do if $H_1$ was true
