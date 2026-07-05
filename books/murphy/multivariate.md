# Ch 3. Multivariate distributions


**Simpson paradox**

> a statistical trend or relationship that appears in several different groups of data can disappear or reverse sign when these groups are combined

**Covariance matrix**

- full $\Sigma$
- diagonal $\Sigma = diag(\sigma_1, \dots \sigma_n)$
- spherical/isotropic $\Sigma = \sigma^2 I$

**Precision matrix**

$$
\Lambda = \Sigma^{-1}
$$

## Multivariate Normal

$$
\mathcal{N}(y \mid \mu, \Sigma) = \frac{1}{(2\pi)^{D/2} |\Sigma|^{1/2}}exp(-\frac{1}{2} (y-\mu)^T \Sigma^{-1} (y-\mu))
$$

**Marginals**

Given $[y_1, y_2] \sim \mathcal N ([\mu_1, \mu_2]^T, \Sigma)$:

$$
p(y_1) = \mathcal N (y_1 \mid \mu_1, \sigma_1^2)
$$

**Conditionals**

$$
p(y_1\mid y_2) = \mathcal N(y_1 \mid \mu_{1\mid 2}, \Sigma_{1\mid 2}) \\
\mu_{1\mid 2} = \mu_1 + \sigma_{12} \sigma_{2}^{-1} (y_2 - \mu_2) \\
\Sigma_{1\mid 2} = \sigma_1 - \sigma_{12} \sigma_2^{-1} \sigma_{21}
$$

**Bayes rule for Gaussians** (Linear Systems)

Let $y$ be a noisy observation of $z$
$$
z \sim \mathcal{N}(z \mid \mu_z, \Sigma_z) \\
p(y\mid z) = \mathcal N (y \mid Wz + b, \Sigma_{y\mid z})
$$

$$
p(z \mid y) = \mathcal{N} (z \mid \mu_{z\mid y}, \Sigma_{z\mid y}) \\
\Lambda_{z\mid y} = \Lambda_z + W^T \Lambda_y W
$$


