# 5. Bootstrap

we are given $x_j$ - measurements
and we can compute a statistic $\hat \theta$ from them, but we also want to assess the standard error

We can either run measurements $n$ times or bootstrap

**non-parametric**

sample from the empirical distribution $n$ times
and we approximate the empirical distribution from the population of measurements by sampling with repetitions

then we compute statistic

then we compute its variance

**parametric**

we assume a distribution for the measurements and sample from this distribution

then compute statistic

then compute variance
