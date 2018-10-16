---
title: "14_310x - Module_04 - Functions and Moments of a Random Variables & Intro to Regressions"
author: "Antonello Lobianco"
date: "3 October 2018"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```


# 14_310x - Module_04 - Functions and Moments of a Random Variables & Intro to Regressions #

##  Lecture 0401 -  Functions of Random Variables  ##

### 040101 - Functions of Random Variables, Part I ###

Knowledge stages:
Random variables -> Functions of Random variables -> Statistics

Statisctics (like sampel mean or t-test) are function of random variables

We are interested in how a new variable (itself a r.v.), function of an other r.v. - or possibly of a vector of r.v. - for which we know the PDF is in turn distributed.

Graphical modification:

- Multiplying a r.v. by a positive contant -> it "stetch up" its distribution: the density ditributes itself over a wider (double) range and ssquish down (as the area must be the same, it has to integrate to 1!), but keeps the same shape.
- Taking the absoule value -> it flips the negative values of the support of the r.v. to the positive side (adding up - folding over - to the density already present there)
- Adding a constant -> shift the density right (positive contant) or left (negative one) of its original support

In all cases the PDF has to keep integrating to 1

#### Transformation of a uniform to a binomial ####

Question: Which function g can trasform a Uniform in [0,1] to a Binomial(n=2, p=0.5) ?

We are now dealing with the trasformation from a continuous r.v. to a discrete one.

For the binomial $B(2,0.5)$, $P_Y(0) = 0.25$, $P_Y(1) = 0.5$ and $P_Y(2) = 0.25$

A function that map the interval of X from 0 to 0.25 to 0, 0.25-0.75 to 1 and 0.75-1 to 2 would be a suitable one (but it's not the only one, one could go in the other way across the support for example)

Any function that "chop" the support of the X (the unit interval) and map appropriate sized sub-intervals to each point mass of the binomial would be appropriate.

Sometimes there will be multiple functions of a random variable that will map into the distribution of a new random variable.

Which specific method to apply to retrieve the distribution of a function of a r.v. X depends on:

- X is continuous or discrete
- X is a scalar (a sinlge r.v.) or a vector (multiple r.v.)
- the function Y = h(X) is invertible or not

On this course we focus one single method + examples instead of general treatment.

### 040102 - Functions of Random Variables, Part II ###

1) We find the CDF of Y:

$F_Y(y) = P(g(X) \leq y) = \int_{\{x|g(x) \leq y\}} f_X(x) dx$


  - How do we find the CDF of y? -> We integrate the PDF of x over the region x, such that h of x is less than or equal to y.

  - How to find what the appropriate region is ? -> $F_Y(y) = P(Y<=y) = P(h(X) <= y)$ and from here we solve for X to find the interval(s) where to integrate


2) We differentiate to take the PDF of Y (if Y is continuous):

$f_Y(y) = \frac{dF_Y}{dy}(y)$

For discrete r.v. we have directly that, for Y=h(X), $f_Y(y) = P(Y = y) = P(h(x) = y) = P(x = h^{-1}(y)) = f_X(h^{-1}(y))$

e.g. :

$X = a^x, Y = \frac{X}{X+1} => f_Y(y) = P(Y = y) = P(\frac{X}{X+1} = y) = P(x = \frac{y}{1-y}) = a^{\frac{y}{1-y}}$


### 040103 - Functions of Random Variabes: An Example ###

X ~ U(-1,1) (i.e. f_X(x) = 1/2 in [-1.1]), Y = X^2  ==> f_Y(y) ??

Support of X = [-1,1] ==> induced support of Y = [0,1]

$F_Y(y) = P(Y \leq y) = P(X^2 \leq y) = P(-\sqrt{y} \leq \sqrt{y}) = \int_{-\sqrt{y}}^{\sqrt{y}} f_X(x) dx$

= $\int_{-\sqrt{y}}^{\sqrt{y}} 1/2(x) dx = \sqrt{y}$

$F_Y(y) = \sqrt{y} => f_Y(y) = \frac{d \sqrt{y}}{dy} = \frac{1}{2 \sqrt{y}}$  in 0 <= y <= 1


### 040104 - Linear Transformations of Random Variables ###

4 Examples of functions or random variables in this lesson:

- Linear trasformation of a single r.v.
- Probability integral transformation
- Convolution
- Order statistics

Now linear trasformation:

It comes out all the time. Linear trasformation of a r.v. is important because e.g. (1) the r.v. is expressed in a unit that need to be changed, (2) some formula dictates a linear relationship between variables, and we know how one is distributed, (3) some theory predicts a linear relationship between variables.

$f_Y(y) = \frac{1}{\lvert a \rvert} f_X(x)(\frac{y-b}{a})$

Proved using the method described in 030202:

*a=0 :*

$F_Y(y) = P(Y \leq y) = P(aX+b \leq y) = P(X \leq \frac{y-b}{a}) = \int_{-\infty}^{\frac{y-b}{a}} =F_X(\frac{y-b}{a})$

$\frac{d F_Y(y)}{dy} = \frac{F_X(\frac{y-b}{a})}{dy} => f_Y(y) = f_X(\frac{y-b}{a}) * \frac{1}{a}$ (chain rule)

*a<0 :*

$F_Y(y) = P(Y \leq y) = P(aX+b \leq y) = P(X \geq \frac{y-b}{a}) = 1- \int_{-\infty}^{\frac{y-b}{a}} = 1 - F_X(\frac{y-b}{a})$

$\frac{d F_Y(y)}{dy} = \frac{1 - F_X(\frac{y-b}{a})}{dy} => f_Y(y) = -f_X(\frac{y-b}{a}) * \frac{1}{a}$ (chain rule)

*a=0 :*

We don't really have a r.v., y is degenerated in a single point in 0 ("point mass").

Hence, in both cases:

$f_Y(y) = \frac{1}{\lvert a \rvert} f_X(\frac{y-b}{a})$

Note that the support of f_Y is still determined by the support of the f_X: if the expression (y-b)/a results in area outside the pupport of f_X, also f_Y will be zero.

### 040105 and  040106 - Probability Integral Transformation and its applications ###

Process used to go from sampling from a U(0,1) distribution to samples from an other know distribution, e.g. if we use a computer software that is capable to give us only random numbers from a U(0,1) but we need to sample from a Normal.

For a r.v. X, we define Y to be equal to its CDF, i.e. Y = F_X. Which is the PDF of Y (f_Y(y)) ?

1) The induced support of Y is [0,1], as the CDF of X is defined in this interval
2) F_X is invertible, as it is non-decreasing
3) $F_Y(y) = P(Y \leq y) = P(F_X(x) \leq y) = P(X \leq F_X^{-1}(y)) = F_X(F_X^{-1}(y)) = y$

For $F_X^{-1}$ is meant the inverse function. E.g. if we set F_X(x) to be "2x+2" we have:

$2x+2=y => x=\frac{y-2}{2} => 2*\left[ \frac{y-2}{2}  \right] +2 = y$

4) The r.v. with F_Y(y) = y is the uniform in [0,1], whatever X is distributed
5) We can go the other way around starting from the uniform and going to any other distribution: you start from the f_X(x), you compute the F_X(x), you sample from U(0,1) and trought F_X^(-1)(x) you find the equivalent sample on x:

```
   y =F_X
   |
 1 |-----------------------*
   |               *
 k |------->-- *
   |         * |
   |      *    |
   |   *       |
   |*          |
   ----------------------------> X
```
   
   e.g. f_X = 1/2 in [-1,1] -> F_X(x) = (x+1)/2
   For a sample k in U(0,1) the equivalent value over f_X is (x+1)/2 = k => x=2k-1
   
### 040107 -  Convolution in the Context of Probability ###
   
Concolution in the context of probability refers to the sum of independent r.v. and more in general linear functions of independent r.v.
In this lesson we accounts only for sum of 2 independent r.v.

Question:   $X$ and $Y$ are continuous independent r.v., and $Z=X+Y$. Which is $f_Z(z)$ ?

1) We know $f_{XY}(x,y) = f_X(x)f_Y(y)$ as X,Y are independent.

2) To find the CDF of Z we integrate in the region of the joint $f_{X,Y}(x,y)$ where the outcomes in terms of x and y would results in a value of z below Z:

$F_Z(z) = \int_{-\infty}^{\infty} \int_{-\infty}^{z-y} f_X(x)f_Y(y) ~dx~ dy$

(across the whole Y axis, we take then only the values of X that are between -inf and x=z-y)


```
             Y
      *      |
        *    |
          *  |               
             *
             | * 
             |---*
             |   | * z
             |   |   *
-----------------|-----*--------------> X
             |  x= z-y   *
             |             *
```

3) To get the $f_Z(z)$ we take the derivative over $dz$ on both sides, recognising that:

$\frac{d(\cdot)}{dz} = \frac{d(\cdot)}{dx}*\frac{dx}{dz}$ (chain rule) and $\frac{dx}{dz} = 1$

$f_Z(z) = \int_{-\infty}^{\infty} f_X(z-y) f_Y(y) ~dy$


### 040108 -  Order Statistics ###   

Taken a group of independent, identically distribued (i.e. "i.i.d.") r.v. (also called as a "random sample") we define:

- $Y_1$ = 1st order statistic = smallest value
- $Y_2$ = 2nd order statistic, the second smallest value
- ....
- $Y_n$ = nth order statistic, the nth smallest value, i.e. the highest value within the random sample

We are interested in finding $f_{Y_n}(y_n)$ (in the following text referred to  $f_{n}(y)$ for convenience).

Usage:

- in economics applications (auctions?)
- the base for important estimations


#### nth order statistics ###

For the nth order statistics we have, by definition of $Y_n$ and the hypothesis of independende of the "X_n"s in the random sample , that:

$F_n(y) = P(Yn \leq y) = P(X_1 \leq y, X_2 <= y, ..., X_n \leq y) = P(X_1 \leq y) P(X_2 <= y) ... P(X_n \leq y)$

As the X_n are salso identically distributed we have:

$F_n(y) = F_X(y)^n$

$\frac{F_n(y)}{dy} = \frac{F_n(y)}{dx} * \frac{dx}{dy}$

$f_n(y) = \frac{F_X(y)^n}{dx} * 1 = n(F_X(y))^{n-1} f_X(y)$

#### 1st order statistics ###

$F_n(y) = 1- (1-F_X(y))^n$

$f_n(y) = -n * (1-F_X(y))^{n-1}*-1*f_X(y) =  n * (1-F_X(y))^{n-1}f_X(y)$

#### Other order statistics ###
We can apply a similar reasoning to second order statistic, third order statistic, etc, but those become a little bit more complicated, because we don't have a whole bunch of events of "everything being less than".

We have instead to consider all the different permutations of ways that we can get sort of one of the order statistics, less than a value and two of them greater than a value and so forth.

### 040109 -  Distributions of Order Statistics ###

How these order statistics evolve when we change n ?
As n increases (at the limit tending to +inf) the density of the first order statistics accumulate toward the lowest value of the support of X, the density of the nth-order statistics accumulate toward the highest value of the X support (the support of the order statistics remain equal to those of the original X).


## Lecture 0402 - Moments of a Distribution ##

### 040201 - Probability Integral Transformation ###

Question: $X ~ U[0,1]$ and $Y = -\ln(X)/\lambda$ with $\lambda > 0$. What is $f_Y(y)$ ?

The induced support is y > 0

We apply "040102 - functions of random variables, Part II": we find the CDF of Y, we replace and solve for X and finally we differenciate for $dy$ t ofind the PDF of Y:

$F_Y(y) = P(Y \leq y) = P(-\ln(X)/\lambda \leq y) = P(X >= e^{-\lambda y}) = 1- e^{-\lambda y} $

where the last step is by definition of the uniform distribution (we don't need to make the integral calculus: for a U distribution the CDF is just the ratio between the lenght of the segment a->x and those of the segment a->b)

$F_Y(y) = 1- e^{-\lambda y} => f_Y(y) = \lambda e^{-\lambda y}$

f_Y(y) is an exponential distribution. Let's check if F_Y(y) is the inverse of the function that we used to transform the uniform in Y, the logarithmic:

$F_Y(y) = 1- e^{-\lambda Y} => X = 1- e^{-\lambda Y} => Y = -ln(1-X)/\lambda$

It is the same distribution but mirrored (if x is uniform 0-1, 1 minus x is as well) !

When we saw that we can take the inverse of the cdf and use that to transform a uniform 0-1 random variable to get the pdf of the random variable whose cdf we're using, we didn't say that was unique.
So in particular, there might be lots of different functions that are going to transform a uniform 0-1 random variable into an exponential random variable.
And these happen to be two of the functions.

### 040202 - Moments of a Distribution ###

PDF has the complete information across a r.v. Sometimes we don't care precisely what the shape of the distribution of our random variable is in every region of the support, but we need only the most salient features:

- where it is centered
- where it reach its peak
- how spread out it is
- wheter it is symmetric
- how tick are the tails
- etc.

The "moments" (or "serie of moments") of a distribution help us summarize some of the most salient figures of a distribution

Mean, median and mode all describe in different ways where the distribution is located or centered.

#### Mode ####

The point where the PDF reach its peak

#### Median #####

The point above and below which the integral of the pdf is equal to 1/2, i.e. the value for which 50% of the values are greater, and 50% are smaller.

#### Mean (or "expectation") ####

"Mean"", "expected value"" ("E") or "$\miu$"

The integral (or sum, for discreate r.v.)  over the support of the distribution of X times the PDF of X.

$E(X) = \int_X f_X(x) dx$

$E(X) = \sum_X f_X(x) dx$

Geometric representation: the balancing point of the density

"Sample mean": it's a related but different concept

es. $f_X(x) = \lambda e^{-\lambda x}$ with support $x \geq 0$

$E(x) = \int x f_X(x) dx = \int x \lambda e^{-\lambda x} dx = 1/\lambda$ (last equation using integration by parts)


## Lecture 0403 - Expectation, Variance, and an Introduction to Regression ##

### 040301 and 040302 - The St. Petersburg Paradox ###

Expected return of a function of a r.v.

We could find the distribution of the new function of a r.v. with one of the methods of 0401 and then find its expected values, but if the expected value is all what we need we can take the shortcut:

$E[Y] = E[g(X)] = \int y f_Y(y) dy = \int g(x) f_X(x) dx$

$E[Y] = E[g(X)] = \sum_{y} y f_Y(y) dy = \sum_{x} g(x) f_X(x) dx$

Bernulli distribution is binomial distribution where n=1 (just one trial).

*The game*
I flip a fair coin until it comes up head.
If the number of flips necessary is X, I pay you 2^X euros. How much would you be willing to pay me to play this game ?
X is a geometric distribution with prob = 0.5: number of trials until success
E(X) = 2
Y = winning amount = 2^X

$E[Y] = \sum_{X} 2^X (1/2)^{X-1} (1/2) = \sum_{X} 2^X (1/2)^X = \sum_{X} 1 = \infty$ 

*The paradox*

The paradox is that the correct value should be infinity, but noone would pay infinity or even a much smaller sum to play this game.

*The solution*

The solution is that we are risk averse. We are risk adverce, we have diminishing marginal utility of money, i.e. the utility function in relation to money is increasing but concave.

So we should put the game in term of our expected evaluation of the game.

Arbitrary function: Z = valuation of the winning = ln(Y) = ln(2^X)

$E[Z] = \sum_{X} ln(2^X) (1/2)^X = \sum_{X} X ln(2) (1/2)^X = ln(2) \sum_{X} X (1/2)^X = ln(2) * 2$

Well below infinity!

### 040303 - Properties of Expectations ###

- a is constant => E(a) = a 
- Y = aX+b (linear trasformation) => E(Y) = a E(X) +b
- Y = X1 + X2 +...+Xn => E(Y) = E(X1) + E(X2) + ... + E(Xn)  
- Y = a1X1 + a2X2 + ... + anXn + b (linear combination) => E(Y) = a1E(X1) + a2E(X2) +...+anE(Xn) + b (no need of independence)
- X,Y indep. => E[XY] = E[X] E[Y]


### 040304 and 040305 - Variance and its Properties ###

Expectation is the most important moment of a distribution but not the only one we care of.

While expectation describe the location, or center, of a distribution, the variance, or $\sigma^2$, describes how spread out it is.

$Var(X) = E[(X-\mu)^2]$

I.e. we're creating a new random variable, which is just equal to the squared deviation between x and its expectation, and then we're taking the expectation of that new random variable, and that's the variance.

Variance is an expectation and hence many of its properties wil lfollow from that.

Properties of Variance:

- Var(X) >= 0 whatever X is
- a is constant => Var(a) = 0
- Y = aX+b => var(Y) = a^2 Var(X)  (Variance is not affected by the addictive constant b, but shrink or spread out a distribution and its variance changes by the square of the multiplicative factor)
- Y = X1 + X2 +...+Xn *and X are independent* => Var(Y) = Var(X1) + Var(X2) + ... + Var(Xn)
- Y = a1X1 + a2X2 + ... + anXn + b *and X are independent* => Var(Y) = a1^2 E(X1) + a2^2  E(X2) +...+ an^2 E(Xn)
- Var(X) = E(X^2) - E(X)^2

Variance of a function:

$Y = r(X) => Var(Y) = E(Y^2) - E(Y)^2 = E(r(X)^2) - E(r(x))^2 = \int r(x)^2 f_X(x) dx - \left[ \int r(x)^2 f_X(x) dx  \right]^2$ 


#### Standard deviation ####

A measure of dispersion that has the same unit as the random variable

"Standard deviation", "SD" or $\sigma$

$SD(x) = \sigma = \sqrt{Var(X)} = \sqrt{\sigma^2}$


### 040306 - Conditional Expectation ###


The conditional expectation is the expectation of a conditional distribution.

$E(Y|X) = \int y f_{Y|X}(x|y) dy$

E(Y|X) is a function of X and hence a r.v. itself, while E(Y|X=x) is a specific number, or a particular realisation of that r.v.

*Law of iterated expectations*

$E(E(Y|X)) = E(Y)$

*Law of total variance*

$Var(E(Y|X)) + E(Var(Y|X)) = Var(Y)$

For example, you can view the law of total variance of Y as if the Xs are different groups and the total variance of Y is made of the variance within each group (the expected value of the variance conditional of being in each group - $E(Var(Y|X))$) plus the variance between each group (the variance between each group's expected value - $Var(E(Y|X))$).

### 040307 - An Example: Conditional Expectation ###

Example: 

Random variables:

- N -> "Number of patents provided each year"
- S -> "Number of patents representing a commercial success"

Parameters:

- p -> probability that each patent being a commercial success
- n -> number of paptents this year (realisation of N)

Data:

- E(N) = 2
- Var(N) = 2
- n = 5
- p = 0.2

Question: P(S = 3|N=n) ?  E(S|N=n) = ? 

$f_{S|N}(s|N=n) = B(5,0.2) => P(S = 3|N=n) = {5\choose 3} 0.2^3 0.8^2 = 0.05$

$f_{S|N}(s|N=n) = B(5,0.2) => E(S|N=n) = 5*0.2 = 1$

### 040308 - An Example: Unconditional Expectation and Variance ###

We can use the law of iterated expectations / law of total variance to retrieve the unconditional expectation/variance

Example (same context as 040307 except there is no realised n)

Question: E(S) = ? Var(S) = ?

$E(S) = E(E(S|N)) = E(Np) = p E(N) = 0.2*2 = 0.4$

$Var(S) = Var(E(S|N)) + E(Var(S|N))$

$Var(S) = Var(Np) + E(Np(1-p)) = = p^2 Var(N) + p(1-p)E(N) = 0.04*2+0.16*2=0.4$


### 040309 - Covariance and Correlation ###

Up to now we saw moments of univariate distributions.

Covariance is a moment of a joint distribution that describes one aspect of the relationship between random variables.

It describes how closely associated are two r.v.: if two random variables are independent, their covariance is equal to 0; if two random variables are very closely related, then they're going to have a high magnitudo covariance (either positive or negative).

$Cov(X,Y)  = \sigma_{XY} = E[(X-\mu_X)(Y-\mu_Y)]$ 

Note that we denote it with a single sigma, even if it would make more sense to denote covariance with sigma squared X,Y.

*Correlation*

A standardised version of the covariance. A unit free moment.

$Correlation(X,Y) = \rho(X,Y) = \rho_{X,Y} = \frac{E[(X-\mu_X)(Y-\mu_Y)]}{\sqrt{Var(X)}\sqrt{Var(Y)}} = \frac{Cov(X,Y)}{\sigma_X \sigma_Y} = \frac{\sigma_{XY}}{\sigma_X \sigma_Y}$

Correlation interpretation: 

- $\rho > 0$ => X and Y are "positively correlated"
- $\rho < 0$ => X and Y are "negatively correlated"
- $\rho = 0$ => X and Y are "uncorrelated"

### 040310 - Properties of Covariance ###

1. Cov(X,X) = Var(X)
2. Cov(X,Y) = Cov(Y,X)
3. Cov(X,Y) = E(XY) - E(X)E(Y)
4. X,Y indep => Cov(X,Y) = 0 *But the opposite is not necessarily true!* You can have cov=0 and still have dependent variables
5. Cov(aX+b, cY+d) = a * c * Cov(X,Y)   Linear transformations. The addictive constant don't change the covariance
6. Var(X+Y) = Var(X) + Var(Y) + 2Cov(X,Y)  The variance of the sum when X,Y are not independent
7. $|\rho_{X,Y}| \leq 1|$
8. $|\rho_{X,Y}| = 1 ~~iff~~ Y=aX+b, ~~ a \neq 0|$ Y is a linear transformation of X

### 040311 - A Preview of Regression ###

Linear regression in the context of probability - we are not yet estimating the parameters of the regression fro mactual smaple data.

If the magniture of rho is equal to one we can write X and Y as linear combinations:
$\rho_{XY} = 1  => Y=a+bX, ~~ b>0$
$\rho_{XY} = -1 => Y=a+bX, ~~ b<0$

If the magnitude of rho is below one we can instead write Y as linear functions of X and an opther r.v. U:

$|\rho_{XY}| < 1 => Y = \alpha + \beta X + U$ where U is a new random variable.

If we define:

$\Beta = \frac{\rho_{XY}\sigma_Y}{\sgma_X} $ 
$\alpha = \mu_Y - \beta_{\mu_X} $ 

Then:

$U = Y - \alpha - \beta X$ has the following properties:

- E(U) = 0
- Cov(X,U) = 0

We call $\alpha$ and $\beta$ "regression coefficients" in a bi-variate regression and think of $\alpha + \beta X$ as the part of Y that is "explained by X and U as the "unexplained" part" (we are decomposing the variance we observe in Y).
We typically think of u in a regression context as being the error term.

### 040312 - Markov Inequality and Chebyshev Inequality ###

#### Markov Inequality ####

Given X is a random variable that is always non-negative,
for any t > 0, P(X>=t) <= E(X)/t

How much probability is out in the right tail of this random variable x is bounded by some function of the expectation of x.

The probability of P(X>t) is small both in case E(X) is small or if t is large


#### Chebyshev Inequality ####

For any t > 0, $P(|X-E(X)| \geq t) \leq Var(X)/t^2$

If the variance is small, then x is unlikely to be too far away from the mean

Chebyshev inequality returns "better" bounds than the Markov Inequality, in the sense that the bounds are closer to the real probability values, as it uses more information compared to the Markov Inequality (the mean and the variance, vs the mean only)




