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

Proof:

$$
    E_Y\{V(X|Y)\} + V_Y \{ E(X|Y) \}  =\\ E_YE(X^2|y) - E_Y E(X|Y)^2 + E_YE(X|Y)^2 - (E_YE(X|Y))^2 = \\ EX^2 - (EX)^2 = VX
$$


## Log-sum-exp trick

$$
    lse(a) = log \sum_{c=1}^C exp(a_c)
$$

