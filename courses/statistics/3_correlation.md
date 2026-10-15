# 3. Correlation

## Copulas

**Sklar theorem**

Let $H(x,y)$ be a joint distribution and $F(x)$, $G(y)$ its marginalizations.

$$
\exists_{C: \mathbb R^2 \rightarrow \mathbb [0,1]} C(F(x), G(y)) = H(x,y)
$$

**examples**: 

- $x, y$ independent -> $C(p,q) = pq$
- $x, y$ comonotonic -> $C(p,q) = \min{p, q}$ (Frechet-Hoeffding upper bound)
- $x, y$ countermonotonic -> $C(p,q) = \max{p+q-1, 0}$ (Frechet-Hoeffding lower bound)

**uses**:

- simulation - simulating various dependences
- gives full dependence info

## Correlation coefficients

**general formula**

let $a$ and $b$ be vectors created from all pairs of measurements of some values $x$ and $y$

$$
Corr(x, y) = cos(a, b)
$$

**examples**

- Pearson $r$: $a_{ij} = x_i - x_j$, $b_{ij} = y_i - y_j$
- Spearman $\rho$
- Kendall $\tau$

$\rho$ and $\tau$ are robust against outliers 

## Autocorrelation

For a timeseries $X_t$

$$
\rho(k) = \operatorname{Corr}[X_t, \mathcal L^k(X_t)]
$$ 

- $\mathcal L^k(X_t) = X_{t-k}$ - a lag operator 


