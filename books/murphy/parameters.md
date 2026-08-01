# Types of parameter learning

Name | Optimization
---|---
MLE | $\theta^* = \argmax_\theta p(\mathcal{D} \mid \theta)$
MAP | $\theta^* = \argmax_\theta p(\theta \mid \mathcal{D})$
Empirical Bayes (type-II MLE) | $\xi^* = \argmax_\xi p(\mathcal D \mid \xi)  = \argmax_\xi \int_{\Omega \theta} p(\mathcal D \mid \theta) p(\theta \mid \xi)$
VI | $q^* = \argmax_q D_{KL}(q, p(z\mid \mathcal{D}))$
Variational EM | $q^*, \theta^* = \argmax_{q, \theta}(q, p_\theta (z\mid \mathcal{D}))$
