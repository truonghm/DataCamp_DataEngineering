# Foundations of Probability in R

## The binomial distribution

1. Note that:

```r
pbinom(q, size, prob) = 1 - pbinom(q, size,  prob, lower.tail=FALSE)
```

2. Variance:
$$\sigma^2=Var(X)=size\cdot p \cdot (1-p)$$

## Laws of probability

1. Multiplying a random variable:

Given $X \sim Binomial(10, 5)$:

```r
x <- rbinom(100000, 10, 5)
mean(x)
var(x)
y <- 3* x
mean(y)
var(y)
```
- When multiplying a random variable by $k$ (a constant), all of the values of that random variable are also multiplied by $k$.
- When multiplying a random variable by $k$ (a constant), we also multiply the expected value by $k$.
- When multiplying a random variable by $k$, we multiply the variable by $k^2$.

So:

$$E[k \cdot X] = k \cdot E[X]$$
$$var(k \cdot X) = k^2 \cdot var(X)$$

2. Adding 2 random variables

$$E[X+Y]=E[X] + E[Y]$$
(Even if $X$ and $Y$ are not independent)
$$Var[X+Y]=Var[X] + Var[Y]$$
(Only if $X$ and $Y$ are independent)

## Conditional Probability (Bayesian Statistics)