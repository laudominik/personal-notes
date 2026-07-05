# Variational Inference (VI)

## Basics

- $p_\theta (z)$ - prior (assumed)
- $p_\theta (x\mid z)$ - likelihood (assumed)
- $p_\theta (x,z)$ - joint (assumed)
- $p_\theta (z \mid x) =  \frac{p_\theta(x,z)}{p_\theta(x)} \propto p_\theta (x,z)$ - posterior (unknown, intractable)


*Finding* $p_\theta(x)$:

$$
p_\theta(x) = \int_{\Omega z} p(x, z)dz
$$

When it is not intractable?

- $p(z)$ is a conjugate prior (i.e. posterior is of the same family as the prior)
- the integral has a closed form

Usually it is intractable


*Why do we care about finding* $p_\theta(x)$:

- for generative models we wanna find $\theta$ to maximize $p_\theta(x)$
- for Bayesian Inference we want to know full posterior  $p_\theta(z/x)$

*Variational Method*: optimize over function space to find $q$ s.t.

$$
q(z) \approx p_\theta(z\mid x)
$$

s.t.

$$
q(z) = \arg \min_{q' \in \mathcal{Q}} D_{KL} (q(z) \parallel p(z\mid x))
$$

*How does that help?*

- finding posterior is now an optimization problem
- we can fully use the found distribution for various downstream actions

**General VI methodology**

1. Define $p(z)$ and $p(x\mid z)$
2. Propose $q\phi(z) \approx p(z \mid x)$
3. Select loss function 

$$ 
\text{ELBO} = \mathbb{E}_{z \sim q(z)}[\log p(x, z) - \log q(z)] = -\mathcal L
$$

4. Find $\nabla \mathcal{L}$
