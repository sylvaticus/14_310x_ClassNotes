---
title: 14_310x - Module_05 - Special Distributions, the Sample Mean, the Central Limit
  Theorem
author: "Antonello Lobianco"
date: "10 October 2018"
output:
  html_document: default
  pdf_document: default
---


# 14_310x - Module_05 - Special Distributions, the Sample Mean, the Central Limit Theorem #

##  Lecture 0501 - Special Distributions ##

What is so special about "special" distributions ?

- Some distributions are special because they are connected to others in useful ways
- Some distributions are special because they can be used to model a wide variety of random phenomena.
- This may be the case because of a fundamental underlying principle
- Or it may be because the family has a rich collection of pdfs with a small number of parameters which can be estimated from the data (estimating one parameter, we can map, potentially, a lot of phenomena. We like that to be explaining, to be trying to fit in distribution with as few parameters as possible.)

Like network statistics, there are always new candidate special distributions! But to be really special a distribution must be mathematically elegant, and should arise in interesting and diverse applications

Many special distributions have standard members, corresponding to specified values of the parameters.

Today’s class is going to end up being more of a reference class than a conceptual one.
For each distribution we give the mean and variance and what they looks like.

"Special" distribution that we deal with:

- Bernoulli
- Binomial
- Uniform
- Negative binomial
- Geometric
- Normal
- Log-normal
- Pareto
- Poisson  (to model the arrival of events)

### 050101 - The Bernoulli Distribution ###

- Type: Discrete
- Interpretation: It models a single success when the probability is p (One of the two outcomes you're going to name "success". One of the two outcomes you're going to name "failure")
- Example of experiment: Flipping a coin
- Explicit parameters:
  - p -> probability of success
- Implicit parameters:
  - q -> probability of failure q = 1-p
- Support: $X \in {0,1}$
- PMF: $f(x;p) = p^x(1-p)^{1-x}$
- E[X]: p
- E[X^2]: p
- Var[X]: p(1-p)

The Bernoulli distribution that have the largest variance are the one that are with p as a half.

Prof. Duflo works on randomized controlled trial: People are assigned to treatment and control groups with probability p

Note on the distribution notation convention:

f(x,y;a,b,c) means that X and Y are the two r.v. of this bivariate (in this case) distribution, and a,b,c are its parameters

### 050102 - The Binomial Distribution ###

- Type: Discrete
- Interpretation: Number of successes in independent binary trials with the same probability (i.e. i.i.d. Bernulli trials)
- Example of experiment: Flipping a coin multiple times, sampling with replacement 
- Explicit parameters:
  - p -> probability of success
  - n -> number of trials 
- Implicit parameters:
  - q -> probability of failure q = 1-p
- Support: $X \in {0,1,2,...,n}$
- PMF: $f(x;n,p) = {n \choose x}  p^x (1-p)^{n-x}$ where ${n \choose x} = \frac{n!}{!x(n-x)!}$
- E[X]: np
- Var[X]: np(1-p)

It is the sum of i.i.d X vr.v. distributed according to the Bernulli distribution.

It's a symmetric distribution and almost bell shaped

With "estimation" it is possible to test if some observed data are actually distributed like a binomial.

### 050103 - The Hypergeometric Distribution ###

- Type: Discrete
- Interpretation: Number of successes in binary trials with varying probability
- Example of experiment: Sampling without replacement
- Explicit parameters:
  - A -> number of successes available
  - B -> number of failures available
  - n -> number of trials 
- Implicit parameters:
  - p -> initial probability of success p = A/(A+B)
  - q -> initial probability of failure q = B/(A+B)
  - N -> number of outcomes (successes and faliures) available
- Support: $$x \in \{max(0,n+A-N),...,min(A,n) \}$
- PMF: $f(x;A,B,n) = \frac{{A \choose x} {B \choose n-x}}{  {A+B \choose n} }$
- E[X]: nA/(A+B)
- Var[X]: $n\frac{A}{A+B}\frac{B}{A+B}\frac{A+B-n}{A+B-1}$

Compared with the variance in the binomial, the last term in the variance of the hypergeometric is going to make the variance smaller, as it accounts that every time we remove a drawn element there is a smaller sample, so there are fewer options.

If N is much larger than n, the replacement doesn't matter anymore, and the binomial becomes a good approximation to the hypergeometric distribution.

#### Negative binomial distribution ####

**Type A**

- Type: Discrete
- Interpretation: Number of independent binary trials with the same probability (i.e. i.i.d. Bernulli trials) necessary to achieve r successes 
- Example of experiment: Flipping a coin multiple times, counting the trials necessary to reach r heads
- Explicit parameters:
  - p -> probability of success
  - r -> number of successes desired
- Implicit parameters:
  - q -> probability of failure q = 1-p
- Support: $X \in {r,r+1,r+2,...,inf}$
- PMF: $f(x;r,p) = {x-1 \choose r-1}  p^r (1-p)^{x-r}$
- E[X]: r(1-p)/(1-p)
- Var[X]: r*(1-p)/p^2

$p^(r-1) (1-p)^{x-r}$ is the probability of any sequence with $r-1$ successes and $x-r$ failures and then $p$ is the probability of success after $r-1$ failures.
${x-1 \choose r-1}$ is the number of possibille sequences where $r-1$ are successes

**Type B**

Number of failures to achieve r successes

The sum of negative binomial r.v. is still a negative binomial r.v. with parameter $r = \sum_{r_i}$

#### Geometric distribution ####

**Type A**

- Type: Discrete
- Interpretation: Number of trials to reach the first success in a serie of independent binary trials with the same probability (i.e. i.i.d. Bernulli trials)
- Example of experiment: Flipping a coin multiple times, counting the trials necessary to reach a head
- Explicit parameters:
  - p -> probability of success
- Implicit parameters:
  - q -> probability of failure q = 1-p
- Support: $X \in {1,2,...,inf}$
- PMF: $f(x;p) = p (1-p)^{x-1}$
- E[X]: 1/(p
- Var[X]: (1-p)/p^2

**Type B**

- Type: Discrete
- Interpretation: Number of failures before the first success in a serie of independent binary trials with the same probability (i.e. i.i.d. Bernulli trials)
- Example of experiment: Flipping a coin multiple times, counting the number of tails before reaching a heads
- Explicit parameters:
  - p -> probability of success
- Implicit parameters:
  - q -> probability of failure q = 1-p
- Support: $X \in {0,1,2,...,inf}$
- PMF: $f(x;p) = p (1-p)^{x}$
- E[X]: (1-p)/p
- Var[X]: (1-p)/p^2c

The geometric distribution can be seen as a specialisation of the negative binomial distribution with r = 1

The sum of r independent gemetric r.v. is a negative binomial r.v. with parameter r

The geometric ditribution shows the memorylessness property: since all trials are indepedent, the distribution of additional failures before first success is still geometric with the same propetries (i.e. the histroy or past draws doesn't matter for future draws)


### 050104,050105 and 050106 - The Poisson Distribution ###

- Type: Discrete
- Interpretation: Number of costant rate independent events occuring in a fixed interval of time
- Example of experiment: Number of customers entering a shop in a minute, number of words starting with letter "c" in a book
- Explicit parameters:
  - $\gamma$ -> istantaneous arrival rate (on limit t -> 0)
  - t -> units of time
- Implicit parameters:
  - $\lambda$ -> propensity to arrive in some amount of time $\lambda = \gamma * t$
- Support: $X \in {0,1,2,...,inf}$
- PMF: $f(k;\gamma,t) = e^{-\gamma t} \frac{(\gamma t)^k}{k!}$ or $f(k;\lambda) = e^{-\lambda} \frac{(\lambda)^k}{k!}$
- E[X]: $\gamma * t$
- Var[X]: $\gamma * t$

Attenction that the arrivals must be independent (not the people in a caffetteria as they tend to arrive together), the frequency have to remain constant (not the people in a ceffetteria as depending on the time frequence change), and there could not be two or multiple arrivals at the exact same time.

The poisson distribution is asymetrical (i.e. "skewed"), as it is constrained to be non-negative, but as $\lambda$ increases the non-negative contraint become less likely to bind and the distribution become closer and closer to being symmetric and looking as a binomial distribution.

For small values of p, the Poisson distribution can simulate the Binomial distribution and it is easier to compute

When do we use a Poisson distribution?

- Poisson distributions are useful with count data: Number of goals in a soccer match; Number of ideas that a researcher has in a month; number of accidents
- Some count data won’t work well with Poisson: e.g. number of students who arrive at the coop (students arrive together; the events are not independent).

The parameter λ governs both the mean and the variance, so some times that it not what you want (you cannot increase the mean without increasing the variance).
The negative binomial can be thought of as a generalization that does not have this property

The poisson distribution can be seen as a binomial distribution when you reduce the time slots to infinitesimally small slots and lambda is big enought (if lambda is small the distribution is very skewed toward zero)

Advantage: it is much easier to work with than the binomial distribution.

#### A summary of Random processes ##### 

**Bernoulli processes (t is discrete):**

 - Number of arrivals: Binomial $p_N(n;t,p)$ - discrete
 - Interarrival time: Geometric $p_T(t;p)$ - discrete
 - Time to the Kth arrival: Pascal $p_T(t;k,p)$ - discrete

**Poisson processes (t is continuous):**

 - Number of arrivals: Poisson $p_N(n;t,\lambda)$ - discrete
 - Interarrival time: Exponential $f_T(t;\lambda)$ - continuous
 - Time to the Kth arrival: Erlang $f_T(t;k,p)$ - continuous

### 050107 - Exponential and Uniform Distributions ###

#### Exponential ####

- Type: Continuous
- Interpretation: Time to arrival of independent events
- Example of experiment: Waiting time before a customer arrive in a shop; time before an hard disk breaks
- Explicit parameters:
  - $\lambda$ -> arrival rate
- Support: $X \geq 0$
- PDF: $f(t;\lambda) = \lambda e^{-\lambda t}$
- E[X]: $1/\lambda$
- Var[X]: $1/\lambda^2$
- $P(T \geq \tau)$: $e^{-\lambda \tau}$
- CDF: ($P(T \leq \tau)$): $1-e^{-\lambda \tau}$

The eponential is memoryless: $P(X \geq t+h| X\geq t) = P(X \geq h) = e^{-\lambda h}$. It doesn't matter what happened before.
It is a special case of the Gamma distribution that model the "waiting time" before a number (non necessary an integer number) of occurrences.


#### Uniform continuous ####

- Type: Continuous
- Interpretation: Complete ignorance
- Example of experiment: Probability that an arrow land on a sector of a target
- Explicit parameters:
  - $a$ -> beginning of the interval
  - $b$ -> ending of the interval (b >= a)
- Support: $a \leq X \leq b$
- PDF: $f(x;a,b) = \frac{1}{b-a}$
- $P(X \geq x)$: $\frac{b-x}{b-a}$
- CDF: ($P(X \leq x)$): $\frac{x-a}{b-a}$ 
- E[X]: $1/2 (a+b)$
- E[X^2]: $\frac{1}{3} \frac{b^3-a^3}{b-a}$
- Var[X]: $\frac{1}{12} (b-a)^2$

The probability that X is in a certain sub-interval [a2;b2] depends only on the length of that interval.

"Standard uniform": X~U(a=0,b=1)

If U is a standard uniform, also 1-X it is a standard uniform.

Many many applications:

- very useful in hypothesis testing
- quasi-random number generators. Computers don’t really know random numbers. Many programming languages have the ability to generate pseudo-random numbers, which are really draw from a standard uniform distribution
  - from a uniform distribution, you can use the inverse CDF method to get a sample for many (not all) distributions you are interested in (or you can just directly sample from the relevant distribution in R - but note that R does not always use the inverse
transform method)

For any distribution in R, the Q version gives you the inverse quantile function (the inverse of the CDF).

R useful commands for probability/samples:

- `runif(n,a,b)` randow draw n samples from a uniform a-b distribution
- `qexp(x,rate)` returns the probability that X is below x using the exponential function
- `rexp(n, rate)` returns n samples from the exponential
- `sample(vector,n,replace)` samples n elements from the vector with (replace=TRUE) or without (replace=FALSE) replacement.

### 050108 - The Normal Distribution ###

- Type: Continuous
- Interpretation: 
- Example of experiment: Many natural phenomena
- Explicit parameters:
  - $\mu$ -> average
  - $\sigma^2$ -> variance (Attention!!: some software - R included - parametrice the Normal to the standard deviation instead!)
- Support: $-\inf \leq X \leq +\inf$
- PMF: $f(x;\mu,\sigma) = \frac{1}{\sqrt{2 \pi \sigma^2 }} e^{- \frac{(x-\mu)^2}{2 \sigma^2 } }$
- E[X]: $\mu$
- Var[X]: $\sigma^2$

"Standard normal":

- $Z \sim Z(\mu=0,\sigma^2=1)$
- The PDF for the standard normal is normally indicated with $\phi(z)$ 
- The CDF for the standard normal is normally indicated with $\Phi(z)$ 

It's look symmetric, bell shaped and with a thin tail.
As the variance becomes smaller and smaller, the normal becomes higher and higher at the mode.

#### The Normal distribution as a limit case of the Binomial distribution ####

Let X~B(n,p). For any number c and d it can be proved that:

$\lim_{n -> \inf} P(c \leq \frac{X-np}{\sqrt{np(1-p)} } \leq d) = \int_c^d \frac{1}{\sqrt{2 \pi}} e^{\frac{x^2}{2}} dx$

where we notice that $np$ is the mean of the binomial, $\sqrt{np(1-p)}$ is it's standard deviation and $\frac{1}{\sqrt{2 \pi}} e^{\frac{x^2}{2}}$ is the PDF of the Normal distribution.

When p's are very little we would approximate the binomial with a Poisson,and not with a normal distribution.

#### From the Stantard Normal to any Normal distribution ####

If Z is a standard normal, any r.v. $X = \mu + \sigma Z$ with $\sigma \neq 0$ is also a normal distributed r.v. with:

$f(x;\mu,\sigma^2) = \frac{1}{\sigma} \phi\left( \frac{x-\mu}{\sigma} \right)  = \frac{1}{\sqrt{2 \pi \sigma^2 }} e^{- \frac{(x-\mu)^2}{2 \sigma^2 } } => X \sim N(\mu,\sigma^2)$

#### Properties of the Normal ####

- If $X_1$ is normal then also $X_2 = a + bX_1$ is also normal with $E[X_2] = a+bE[X_1]$ and $Var[X_2] = b^2 Var[X_1]$
- If $X_1, X_2, ... , X_n$ are independent r.v. with $X_i \sim N(\mu_i, \sigma_i^2)$ their sum remains a normally distributed r.v. with $Y = \sum_i X_i \sim N(\sum_i \mu_i, \sum_i \sigma_i^2)$ (we already knew the mean and variance, but we know now that the sum also remains normal)
- Any linear combination of normally distributed random variables, will also follow a normal distribution

### 050109 - The Normal Distribution: Finding the Area Under the Curve ###

The integral of $\phi(x)$ over regions of $\mathbb{R}$ cannot be expressed in closed-form

Therefore we use tables (or software...) to figure out the answer we are looking for.

Tables of the standard normal distribution return P(Z < a). Being the standard normal distribution symmetrical, printed tables normally show only one side of the distribution (e.g. a>0).

Still it is possible to find any values with these rules:

- P(Z<-a) = 1 - P(Z<a)
- P(a < Z < b) = P(Z<b) - P(Z<a)
- P(Z>a) = 1- P(Z<a)
- to lookup P(X<a) where X is a NON-standard normal, it is necessary to first normalize X, and then lookup in the standard normal table

```
|                *
|               * *  
|              *   *
|             *     *
|            *       *
|          *           *
|        *               *
|      *##              ####*
|  *######              #########*
|----------------------------------->Z
|       -a       0      a
                
```
        
68.2% of the area under the CDF of the standard normal is between 1 and -1 standard deviations, 95% between -2 and +2 standard deviations and 99.7% between -3 and 3

### 50110 - Finding the Area Under the Curve Using R ###

Useful R command about the normal distribution:

- `rnorm(n,mean,sd)` - Generates n random numbers from the normal distribution
- `dnorm(x,mean,sd)` - PDF, returning the density at a given point x
- `pnorm(x,mean,sd, lower.tail=TRUE)` - CDF, it gives the area under the left side of the point x (or the area on the right side - i.e. P(X>x) - if lower.tail=FALSE)
- `qnorm(p,mean,sd)` - Inverse of the pnorm ("quantile function"), it returns the x where the P(X<x) is p

##  Lecture 0502 - The Sample Mean, Central Limit Theorem, and Estimation ##

### 050201 and 050202 - The Sample Mean ###


The sample mean (notation: x with a bar on top and sometimes a subscript to indicate the size of the sample it comes from) is the arithmetic average of the n random variables from a random sample of size n, that is the arithmetic average of the *realisations* of those n random variables:

$\bar X_n = (1/n) (X_1 + X_2 + ... + X_n) = \frac{1}{n} \sum_{i=1}^n X_i$

We treat it as a function of a random variable as we saw them earlier, and we are interested in the distribution of this new random variable.

A nice link on "realisation of a r.v.": https://www.statlect.com/glossary/realization-of-a-random-variable

We'll focus for now on random samples from the same distribution, so that each sample, seen as a random variable, is i.i.d, altought it is possible to define random samples from different distributions (like e.g. sampling without replacement).

If we were known the distribution of the Xs we could use the convolution formula to find the distribution of $\bar X_n$.

But even without knowing the origina ldistribution, we can still apply the properties of the moments to get some information about the moments of  $\bar X_n$:

**Expectation of $\bar X_n$ (the sample mean)**

$E(\bar X_n) = E(\frac{1}{n}\sum X_i) = \frac{1}{n}E(\sum X_i) = \frac{1}{n}\sum \mu = \mu$ - without any independence hypothesis (only $\mu$ is the same across all the sampled $X_i$)!

Where $\mu$ is the mean of the distribution from which X is sampled from.

**Variance of $\bar X_n$ (the sample mean)**

$Var(\bar X_n) = Var(\frac{1}{n} \sum X_i)) = \frac{1}{n^2} Var(\sum X_i)) = (indep.) = \frac{1}{n^2} n Var(X) = \frac{\sigma^2}{n}$

Where $\sigma^2$ is the variance of the distribution from which X is sampled from.

Note that the standard deviation of the sample mean is hence $\bar \sigma_n = \sqrt{Var(\bar X_n) } = \frac{\sigma}{\sqrt{n}}$


==> The distribution of the sample mean is centered around the original mean, more "concentrated"" than the original distribution and becoming more and more concentrated as n becomes larger.

Why we are interested in sample mean?

We have an IID random sample from some distribution but we don't know everything about the distribution. Maybe we don't know, for instance, what the mean of that distribution is. But if we have a random sample from that distribution and we calculate the sample mean of that random sample, then that sample mean can tell us something about the underlying distribution itself.

### 050203 - The Central Limit Theorem (CLT) ###

It deals with the distribution of the sample mean: let $X1, X2, ..., X_n$ form a i.i.d. random sample of size n from a distribution with finite mean and variance.

Then for any fixed number $\chi$,

$lim_{n \to \infty} P \left( \frac{\sqrt{n} (\bar X - \mu)}{\sigma} \leq \chi \right) = \Phi(\chi)$

$\frac{\sqrt{n} (\bar x - \mu)}{\sigma}$ is a standardized version of the sample mean, with its mean subtracted off and divided by its standard deviation, so to have mean equal to zero and unit standard deviation.

Standardization of a r.v.: to subtract the mean from it and divide by the standard deviation, in order to obtain a new r.v. with mean equal to zero and s.d. equal to 1

So, the CLT means:

- The CDF of the standardized sample mean of a random sample tend to the standard normal CDF when n tend to infinity;
- (Equivalently) The CDF of a sample mean tends to the normal $N \sim (\mu,\sigma^2 /n)$ CDF.

Some notes on the CLT:

- Whatever the original distribution of $X_i$ is, as long as the random sample taken fro mthat distribution is reasonably large, the sample mean will be approximaly normal;
- We can know a lot about the behaviour of the sample mean without knowing the undrlying distribution of the $X_i$s ;
- It serves as the basics for statistics;
- The CLT only holds if the X's are independent
- Central limit theorem tells us that if our sample size is large enough it's smaple mean is going to be approximately normal.
- the CLT gives some notion on why the normal distribution is so important
- there are difference versions of the CLT when we relax the assumptions of a i.i.d. random sample

### 050204 and 050205 - Estimation ###

What is statistics? It’s the study of **estimation** and **inference**.

We’ll get to inference a little bit later (couple of weeks). For this lecture, we’ll focus on estimation. This lecture should be seen as providing a bridge between probability and statistics.

We need to think of random variables in two ways, as a function from the sample space to the real numbers, and as a stochastic object that can “take on” different realizations with different probabilities.

We use the notation X to stand for the random variable, and use x to stand for its realization (or possible realizations).
Before, we defined a random sample as an i.i.d. collection of random variables. We can also call the realizations of those random variables a random sample. Or just data.
In a manner similar of "random variable"", the term *random sample* can refer to the set of random variables that constitutes the sample, but can also refer to the realizations of these random variables.

An **estimator** is a function of the random variables in a random sample (for example the sample mean is an estimator). The specific function is chosen to have properties useful for giving us information about the distribution that the random sample come from (the "underlying distribution").

We will often use $\hat \theta$ as general notation for an estimator.

A **parameter** is a constant indexing a family of distributions.
Examples of parameters are μ and σ 2 from the normal distribution, λ from the exponential distribution, a and b from the uniform distribution, n and p from the binomial distribution, etc. We will often use $\theta$ (without the hat) as a general notation for a parameter.

In an *estimation*, we typically want to determine the values of parameters that govern an observed stochastic process or phenomenon - i.e. we want to estimate unknown parameters. We then look for estimators of our sample data that gives us useful information of these parameters.
In other words, we know (or assume) that a set of random variables, a random sample, is distributed i.i.d. normal, or i.i.d. uniform, or i.i.d. exponential. Estimation is trying to determine the specific μ and $\sigma^2$, or a and b, or λ...

For instance, to "estimate" the mean of a distribution, we might choose a function whose result, when applied to the random sample, is a random variable tightly distributed around the mean of the distribution of those random variables.
Then we plug the realizations of the random sample, or data, into the function to obtain a number, a realization of the
function, and this number is going to give us important information about where the mean is.

The function of the random sample is the *estimator*. The number, or realization of the function of the random sample, is *the estimate* (we use $\hat \theta$ to stand for both, but in many books $\hat \Theta$ is the estimator and $\hat \theta$ is the estimate).

The way that we quantify the uncertainty of our estimate is through the variance of the distribution of our estimator.

### 050206 and 050207 - Estimation: An Example and procedures ###

Let's assume $X \sim U[0,\theta]$, i.e. $f_X(x) = 1/\theta$ for $0 \leq x \leq \theta$, 0 otherwise.

We want to estimate $\theta$.

Possible accounts:

- $\hat \theta_1$ Compute the max (nth order statistic) of the random sample and use that as $\theta$;
- $\hat \theta_2$ Compute the sample mean and multiply it by two. Use then that as $\theta$;
- $\hat \theta_3$ Compute the sample median (the number above and below which half of the sample falls) and multiply it by two. Use then that as $\theta$;
- $\hat \theta_3$ Just get a random number and use it as $\theta$;

Note that for the first 3 cases, our estimator becomes better and better as n (the sample size) gets larger.

Also for the first case we are guaranteeed that $\hat \theta \leq \theta$, while this is not true for the second or third case. 

How did we come out with these functions ? Is there a more systematic way that we can generate these estimators? How do we know they are reasonable ? How do we choose among them ?


An estimator is a function of random variables. It's a sort of mathematical construct that we can talk about the distribution of.
In the real world we typically have one random sample, which has multiple draws from some distribution.
And that random sample that we have is sort of a series of realizations from that distribution, a series of numbers.
So what we do is we choose a function that has properties that are going to be useful for us and then we plug those realizations into the function.
And basically, an estimate is one realization from this distribution of the estimator. And that doesn't mean it's going to equal theta.
We could have a realization here, a realization here. If our estimate is from a distribution that's centered around theta and is pretty concentrated around theta, then we have some confidence that our estimate is close to theta.

For the rest of this lecture and some of next, we will talk about two topics, (1) frameworks for choosing estimators (next lecture)  and (2) criteria for assessing estimators (next segments) and these topics will answer those questions.


### 050208 and 050209 - Criteria for Assessing Estimators ###

An estimator is a random variable, so it has a distribution. Everything that you need to know about a random variable
is embodied in its distribution.
Our criteria for assessing estimators will be based on characteristics of their distributions.

**Bias (or "biasedness")**

An estimator $\hat \Theta$ is unbiased for $\theta$ if $E(\hat \Theta) = \theta$ for all $\theta$ in $\Theta$ (the space for all possible values of $\theta$ - the parameter, not the estimator. In other words whatever the real parameter is, the estimator is unbiased if its expected value is equal to the parameter).

You do not need to know the real value of $\theta$ in order to prove that the estimator is unbiased. Is is enought to prove that its distribution - given the sampled data - has the mean equal to $\theta$, whatever $\theta$ actually is.








