# Ch 2. RVs


## Basic Identities

**Law of total expectation**

$$
    E X = E_Y \{ E \{X|Y\}\}
$$

Proof:

$$
    E_Y \{ E \{X|Y\}\} = \sum_y [\sum_x x P(X = x | Y = y) P(Y=y)] = \\ \sum_y \sum_x xP(X=x | Y = y) P(Y=y) = \\ \sum_y \sum_x x P(X=x, Y=y) = \\ \sum_x x \sum_y P(X=x, Y=y) = \sum_x x P(X=x) = EX
$$

**Law of total variance**

$$
    VX = E_Y\{V(X|Y)\} + V_Y \{ E(X|Y) \}
$$

**Dirac's delta sifting property**

$$
\int_{-\infty}^{+\infty} f(y)\delta (x-y)dy = f(x)
$$

Proof:

$$
    E_Y\{V(X|Y)\} + V_Y \{ E(X|Y) \}  =\\ E_YE(X^2|y) - E_Y E(X|Y)^2 + E_YE(X|Y)^2 - (E_YE(X|Y))^2 = \\ EX^2 - (EX)^2 = VX
$$


**Convolution of Gaussians is a Gaussian**

Let $x_1 \sim \mathcal{N}(\mu_1, \sigma_1)^2$, $x_2 \sim \mathcal{N}(\mu_2, \sigma_2)^2$ and $y = x_1 + x_2$
$$
p(y) = \mathcal{N}(y\mid \mu_1 + \mu_2, \sigma_1^2 + \sigma_2^2)
$$

**Central Limit Theorem**

Let $X_j \sim \mathcal{A}(\mu, \sigma^2)$
where $\mathcal A$ - any distribution.

$$
S_N = X_1 + \dots + X_N \\
\rightarrow S_N \sim \mathcal N (\mu, \frac{\sigma}{\sqrt{N}})
$$

## Log-sum-exp trick

$$
    \operatorname{lse}(a) = \operatorname{log} \sum_{c=1}^C \operatorname{exp}(a_c)
$$


$$
p(c) = \frac{\operatorname {exp}(a_c)}{\sum_{c=1}^C \operatorname{exp}(a_c)} \\
\operatorname {log} p(c) = a_c - \operatorname{log} \sum_{c=1}^C \operatorname{exp}(a_c) \\
\operatorname{log} p(c) = a_c - a_{max} - \operatorname{log} \sum_{c=1}^C \operatorname{exp}(a_c - a_{max}) 
$$

Using $a_{max}$ we ensure the sum of $\operatorname{exp}$ does not overflow.


## Gaussians

PDF

$$
 \mathcal{N}(x|\mu,\sigma^2) = \frac{1}{\sqrt{2\pi \sigma^2}} \operatorname{exp}(-\frac{(x-\mu)^2}{2 \sigma^2}) = \phi(x;\mu,\sigma)
$$

CDF

$$
\Phi(x;\mu,\sigma) = \int^x_{-\infty} \mathcal{N}(z|\mu,\sigma^2)dz = \frac{1}{2}(1 + \operatorname{erf}(\frac{x-\mu}{\sigma\sqrt{2}}))
$$

Note: Murphy uses $\mathcal N(x|\mu, \sigma^2)$ meaning $Prob\{X=x\}, EX=\mu, VX=\sigma^2$

ERF

$$
\operatorname{erf}(u) = \frac{2}{\sqrt{\pi}} \int_0^u e^{-t^2}dt
$$

Let $\mathcal{N}(0,1)$.
Interval $(\Phi^{-1}(\alpha/2), \Phi^{-1}(1-\alpha/2))$:

$$
Prob \{\Phi^{-1}(\alpha/2) < X < \Phi^{-1}(1-\alpha/2)\} = 1 - \alpha
$$


Uncertainty of a heteroskedastic model. Given $p(y) = \mathcal{N}(y|\mu(x), \sigma^2(x))$ we can quantify uncertainty about our estimate of $y$

$$
Prob\{t_1 < y < t_2\} = 1 - \varepsilon \\
(t_1, t_2) = (\mu(x) - \sigma(x)\phi^{-1}(\varepsilon/2), \mu(x) + \sigma(x)\phi^{-1}(1-\varepsilon/2))
$$

Example of such model:
$\mu(x) = w^T_\mu x, \sigma(x) = \sigma_+(w^T_{\sigma}x)$
(where $\sigma_+$ - softplus, which we use for nonnegativity)

## Beta

PDF

$$
Beta(x;a,b) = \frac{1}{B(a,b)} x^{a-1}(1-x)^(b-1)
$$

where

$$
B(a,b) = \frac{\Gamma(a) \Gamma(b)}{\Gamma(a+b)} \\
\Gamma(a) = \int_0^{\infty} x^{a-1}e^{-x}dx
$$

## Gamma

$$
Ga(x \mid a, b) = \frac{b^a}{\Gamma (a)} x^{a-1} e^{-xb}
$$

Special cases:
- $Exp(x\mid \lambda) = Ga(x \mid 1, \lambda)$
- $\chi^2(x \mid \nu) = Ga(x \mid \frac{\nu}{2}, \frac{1}{2})$

## Empirical distribution

PDF

$$
\hat p_N(x) = \frac{1}{N}\sum_{n=1}^N \delta (x - x^{(n)})
$$

CDF

$$
\hat P_N(x) = \frac{1}{N}\sum_{n=1}^N \mathbb{I} (x^{(n)} \le x)
$$


## Distributions comparison

### Student t

- heavy tail
- therefore robust to outliers
- $\nu$, dof = degree of normality (large = more like Gaussian)

### Cauchy

- student t with $\nu=1$
- heavy tail
- half-Cauchy: 
  - useful for Bayesian models 
  - heavy tail
  - finite distribution at the origin


### Laplace

- double sided exponential
- also a heavy tail

## RVs transforms

Let $y = f(x)$

**Discrete**

$$
Prob\{Y=y\} = \sum_{x} \mathbb{I}(f(x) = y)Prob\{X=x\}
$$

**Continuous**


$f$ is invertible

$$
p_Y(y) = |\frac{d}{dy}f^{-1}(y)| p_X(g(y))
$$

$f$ is multivariate

$$
p_Y(y) = |\operatorname{det} J_g(y)| p_X(g(y))
$$

