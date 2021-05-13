---
title: 14_310x - Module_06 - Assessing and Deriving Estimators - Confidence Intervals, and Hypothesis Testing
author: "Antonello Lobianco"
date: "19 October 2018"
output:
  html_document: default
  pdf_document: default
---


# 14_310x - Module_06 - Assessing and Deriving Estimators - Confidence Intervals, and Hypothesis Testing #

##  0601 -  Assessing and Deriving Estimators ##

###  060101 and 060102 -  Unbiased Estimators ###

The estimator $\hat \theta_2 = average(x_1,x_2,...,x_n)*2$ for the upper parameter of a uniform distribution (segments 050206 and 050207) is unbiased:

$\hat \theta_2 = 2 \frac{1}{n}\sum_{i=1}^{n} X_i => E(\hat \Theta_2) = E(2 \frac{1}{n}\sum_{i=1}^{n} X_i) = 2 \frac{1}{n}\sum_{i=1}^{n} E(X_i) =  2 \frac{1}{n} (n \frac{\theta}{2}) = \theta$

The estimator $\hat \theta_1 = max(x_1,x_2,...,x_n)$ is biased:

To prove it we first need to find the PDF of this estimator, and then comute its expected value: 

- $f_{\hat \Theta_1}(x) = n f_x(x) [F_x(x)]^{n-1}$ (nth-order statistics pdf formula)
- $f_{\hat \Theta_1}(x) = n \frac{1}{\theta}\left( \frac{x}{\theta} \right)^{n-1} = n \frac{x^{n-1}}{\theta^n}$(after plugging in specific uniform(0,theta) PDF and CDF)
- $E({\hat \Theta_1}) = \int_0^\theta x n \frac{x^{n-1}}{\theta^n} dx =\frac{n}{\theta^n} \int_0^\theta x^n dx = \frac{n}{n+1} \theta$ 

In this case we know the Bias and we can swipe it off by multiplying the estimator by $\frac{n+1}{n}$, but in other cases of biased estimators we can't compute the amount of the bias and hence we can't adjust the estimator. 

###  060103 -  Efficient Estimators ###

Now two general important theorems:

1. The sample mean for an i.i.d. sample is unbiased for the population mean (we already proved this case)

2. The sample variance for an i.i.d. sample is unbiased for the population variance, if it is computed as $\bar S^2 = \frac{1}{n-1} \sum (x_i - \bar x)^2$ (note the $n-1$ in the denominator)

**Efficiency**

Given two unbiased estimators, $\hat \theta_1$ is more efficient than $\hat \theta_2$ if, for any given sample size:

$var(\hat \theta_1) \leq var(\hat \theta_2)$

Note that we have defined efficiency here just for unbiased estimators. The notion of efficiency can exist for broader classes of estimators as well, but we won’t give a formal definition. If an estimator has a larger variance, it's less efficient than an estimator with a smaller variance.

Sometimes we are iunterested in trade-off bias and variance (efficiency). In other wods, we may be willing to accept a little bit of bias in our estimator if we can have one that has a much lower variance:

**Mean squared error**

$MSE(\hat \theta) = E[(\hat \theta - \theta)^2] = Var(\hat \theta) + [E(\hat \theta) - \theta]^2$ where the last term would be zero for a perfectly unbiased estimator.

Choosing a minimum mean squared error estimator is an explicit way to trade off bias and variance in an estimator (not the only way, but a decent one: there's no right answer when it comes to choosing among estimators that are not dominated in all ways).

For the two estimator of the upper limit of a uniform distribution we saw in segments 050206 and 050207, $\hat \theta_1 = max(x_1,x_2,...,x_n)$ and $\hat \theta_2 = 2 \frac{1}{n}\sum_{i=1}^{n} X_i$, the first one is biased but much more efficient than the unbiased second one, and so in that case, you might want to choose the nth order statistic, just because for a reasonably sized random sample its distribution is going to be much more tightly distributed near theta than two times the sample mean.

**Consistency**

In complement to biasiness and efficiency, a further criterion for an estimator is consistency: $\hat \Theta$ is a consistent estimator for $\theta$ if $lim_{n \to \infty} P(|\theta-\hat \theta_n| < \delta) = 1$, i.e. if its distribution collapses to a single point at the true parameter as $n \to \infty$

Note that you can still have consistent estimators that are biased. For example if you take as estimator of the mean $\frac{1}{n} \sum X_i + \frac{1}{n}$, it is biased but the bias swipes off as we increase n.


### 060104 - Robust Estimators ###

Other important criteria to choose an estimator are:

A) Computational simplicity: how easy is the estimator to compute
B) Robustness: how robust it is to assumptions we made, i.e. wheter the estimator will still do a decent job if some of the assumtions we maid are incorrect, e.g. we've assumed a wrong distribution, or this is shifted away from what we tought.

The 2-times-the-sample-median estimator we used for estimating the upper bound of a uniform is more robust of the 2-times-the-sample-mean, in the sense that it's bias will be lower if we misspecified the kind of the underlying distribution (e.g. instead of a uniform(0,theta), the real distribution was a triangular one with most of the density around 0).

### 060105 - Method of Moments ###

Up to now we figured out how to decide if an estimator is good or not after we have one, but how do we find them in the first place?

There are thre main frameworks for deriving estimators:

1. Method of Moments
2. Maximum Likelihood Estimator
3. Manually thinking to somethng "clever" 

We have seen example of both (1) and (2) (without knowing there where "Method of Moments" or "Maximum Likelihood Estimator"): the 2 times the sample mean, in the $U \sim [0,\theta]$ example, is a method of moments estimator, while the nth order statistic is a maximum likelihood estimator.

The "method of moments" has been developed in 1894 by Karl Pearson

What are moments?

In mathematics the n-th moment is defined as:

$\mu_n = \int_{- \infty}^{+ \infty} (x-c)^2 f(x) dx$

The moment of a function, without further explanation, usually refers to the above expression with c = 0 ("moments about the origin").

For the second and higher moments, the central moment ("moments about the mean", with c being the mean) are usually used rather than the moments about zero, because they provide clearer information about the distribution's shape.

The n-th moment about zero of a probability density function f(x) is the expected value of $X^n$ and is called a _raw moment_ or _crude moment_ or _moment about the origin_. The moments about its mean μ are called central moments, and these describe the shape of the function.

So, there are two different type of moments in a distribution, those defined about the origin and those defined about the mean.

Moments about the origin can be converted to moments about the mean trough an equation.

- Population moments (about the origin): $E(X)$, $E(X^2)$, $E(X^3)$, ...
- Sample moments (about the origin): $\frac{1}{n} \sum X_i$, $\frac{1}{n} \sum X_i^2$, $\frac{1}{n} \sum X_i^3$, ...

For example, the first population moment of a Normal is $\mu$, of a Poisson is $\lambda$, of a Uniform [0,theta] is $\theta/2$,...

To estimate a parameter, we can equate the first population moment (a function of the parameter) to the first sample moment, consider the parameter as our estimator, and solve for the estimator (once we equate the sample moment and the population moment, now we 're solving for an estimator, not the parameter).

For example, in case of a uniform where the unknown parameter ($\theta$) is the upper bound, we have:

- first population moment: $E(X) = \frac{\theta}{2}$
- first sample moment: $\frac{1}{n} \sum X_i$

We can now equate them and solve: $\frac{\hat \theta}{2} = \frac{1}{n} \sum X_i => \hat \theta = \frac{2}{n} \sum X_i$

If we have more than one parameter to estimate we can just use as many sample and population moments as necessary. Each one is called a "moment condition". If we have k parameters to estimate, we will have k moment conditions. In other words, we will have k equations in k unknown to solve.

### 060106 and 060107 - Maximum Likelihood Estimation (MLE) ###

Idea usually attributed to Lagrange, circa 1770, and analytics to R.A. Fisher, circa 1930).

The maximum likelihood estimator of a parameter $\theta$ is the value $\hat \theta$ which most likely would have generated the observed sample.

So, we have the data and we assume a specific "family"" of distributions, i.e. distributions that vary by one or more parameter.
Of all the possible PDFs of these distribution, the parameter(s) of the one that is most likely to have generated the observed data is (are) our maximum likelihood estimate(s).

How do we find the one of a bunch of PDFs that is most likely to have produced the data ?

We have to sort of reinterpret the joint PDF of the data, or random sample. We have to think of it as a function of its parameters and maximize it over all the possible values of those parameters.

We know, if we have a random sample (i.i.d.!), how to get the joint PDF of that random sample: it's just the product of the individual PDFs of all of the random variables in our random sample.
We think of that joint PDF as a function of those parameters, and we're going to maximize that joint PDF over the possible values of the parameters.

In other words, we define a function $L(\theta|x)$, the likelihood function, as the joint PDF of the data, i.e. $L(\theta|x) = \prod_i f(x| \theta )$ for an i.i.d. random sample, and we just maximize $L$ over $\theta$ in $\Theta$.
We can use any monotonic transformation of $L$ and it will still be maximized by the same $\theta$. Computationally, it is often easier to take the log of $L$ and maximize that because then the product becomes a sum, which is easier to deal with.
Whatever maximizes the likelihood function is going to maximize any monotonic transformation and the likelihood function, so it's fine to take a log.

What we do is we take the joint PDF of our random sample.
Instead of calling it a "joint PDF", we're now going to call it a "likelihood function". That's just suggestive to us that we have to think of it in a different way. It's exactly the same thing. The likelihood function is the joint PDF of a random sample.

It’s good practice to write down joint PDFs, maybe take the logs, take the derivatives with respect to $\theta$, set the derivatives equal to zero, and solve for the maximum likelihood estimators. We don't do it in this course.

### 060108 - Examples of Maximum Likelihood Estimation, Part II ###

Given a random sample X_i assumed from a $U \sim [0,\theta]$ distribution, find the MLE.

The individual PDF is $f_X(x) = 1/\theta$ for $x \in [0,\theta]$, 0 oth.

The joint PDf, expressed as a function of theta (i.e. the likelihood function) is $L(\theta) =  \prod_i f(x| \theta ) = (1/\theta)^n$.

Note that values of $\theta$ below $X_n$ have a zero probability to have generated the given random sample, and hence the lickelihood function for them is zero. So the likelihood function has non-zero probabilities only for $X_n \leq \theta$.

The value of theta that maximise this function is exactly the n-th order statistic:

```
L(theta)
 |
 |       *
 |       *
 |       *
 |        *
 |         *
 |           *
 |               *
 |                       *
-------------------------------> theta
 |       |
 0      X_n

```

Note that we solved graphically but we coudn't have take the normal first order condition to find the maximum of this function, as it is not differentiable on its maximum value.

### 060109 - Another Example of Maximum Likelihood Estimation ###

Given a random sample X_i assumed from a $U \sim [\theta-\frac{1}{2}, \theta+\frac{1}{2}]$ distribution, find the MLE.

The individual PDF is $f_X(x) = 1$ for $x \in [\theta-\frac{1}{2}, \theta+\frac{1}{2}]$, 0 oth.

The joint PDf, expressed as a function of theta (i.e. the likelihood function) is $L(\theta) =  1$.

What it matter now is to find the interval over the likelihood function is defined.

From our definition of the problem, $\theta$ can be at most 1/2 above the 1st order statistic, but also al most 1/2 below the n-th order statistic.
The likelihood function is hence a constant, "maximised" for any value of $\hat \theta$ over the range $[x_n - \frac{1}{2}, x_1 + \frac{1}{2}]$.

The left edge of the interval is given by $\theta - \frac{1}{2}$, by our assumption. The first order statistic must be bigger than this, so $X_1\geq \theta -\frac{1}{2}$ or $X_1 +\frac{1}{2} \geq \theta$. On the other side, the right edge of the interval $\theta +\frac{1}{2}$ must be bigger than $X_n$, i.e. $X_n\leq \theta + \frac{1}{2}$. Putting everything together,

$$X_n - \frac{1}{2} \leq \theta \leq X_1 + \frac{1}{2}. $$

### 060110 - Properties of Maximum Likelihood Estimators ###

Maximum likelihood estimators it's a very popular framework for deriving estimators

Properties of Maximum likelihood estimators:

**Favorable**

1. If there is an efficient estimator in a class of consistent estimators, MLE will produce it (checks for the efficiency of ML estimators are not needed)
2. Under certain regularity conditions, MLEs will have asymptotically (as sample space increases) normal distributions (like a CLT for MLEs). Very useful for inference.
3. They're kind of always available. If you're willing to take a stand, like make an assumption about what the underlying distribution is, then you can write down a maximum likelihood estimator

**Unfavorable**

1. They can be biased (we saw an example).

Note that in the particular example we saw (MLE of $U \sim [0,\theta]$) we could fix the bias because the bias is a function of n.
And as we know how big our sample size is, so we can actually undo that bias.
But there are lots of other examples where you don't know the size of the bias, and so you can't always undo it.

2. They might be difficult to compute.
3. They can be sensitive to incorrect assumptions about the underlying distribution, more so than other estimators.

So the method of moments estimators tend to be more robust to sort of the underlying assumptions, because they rely less. While the maximum likelihood estimators really rely on the entire shape of the distribution of the random sample, the method of moments estimators don't. They just rely on the moments. And so it is kind of intuitively that something that relies on the entire shape of a distribution, if you're wrong about that shape of a distribution, it might be sensitive to something that can be wrong. And that's in general true.


### 060111 - Summary of Semester So Far ###

Summary to date

**Probability basics**

- Introduced concept and talked about simple sample spaces, independent events, conditional probabilities, Bayes Rule
- Kind of foundation for going on and studying probability
- Do note that we didn't study Bayes' rule in the context of random variables, just a fundation of this important concept

**Random variables**

- Defined a random variable, discussed ways to represent distributions (PF, PDF, CDF), covered random variable versions of concepts we covered in probability basics (independent events -> independent r.v., conditional probability -> conditional PDF, ...)
- Differently from PMF/PDF, the definition of CDF is the same for discrete and continuous r.v.

**Empirical stuff**

- Parts covered by Esther
- Counterpart of what we have been talking theoretically in class
- Histogram, kernel density function (non-parameteric method)

**Functions of random variables**

- Saw some basic strategies and several important examples (probability integral transformation, convolution, linear transformation of r.v., order statistics)

**Moments**

- Defined moments of distributions (expectation, variance..) and learned many techniques and properties to help compute moments of functions of random variables (like using properties of expectaiton, variance or covariance rather than compute the PDF of the function of r.v. and then compute the moments by definition over their PDF)

**Special distributions**

- Binomial, hypergeometric, geometric, negative binomial, Poisson, exponential, uniform, normal
- The students on-campus have the tables of moments of each of these distribution provided on exam day

**Estimation**

- CLT, had general discussion and discussion about sample mean, criteria for assessing and frameworks for deriving estimators


##  0602 -   Confidence Intervals, Hypothesis Testing and Power Calculations  ##

### 060201 - Introducing the Standard Errors (quantifying reliability) ###

We have seen various estimators, made observations about their distributions, discussed how we might derive them, (e.g. method of moments and maximum likelihood estimation) and also discussed criteria that we might use to choose among them.

That’s all well and good, but when we actually have to report estimates, people will want to have some objective measure of how good, or reliable, or precise, our estimates are.

So we often report an estimate along with its standard error. The estimate is indeed a realization from the distribution of the estimator. So we report that estimate, that one number that's a realization from that distribution.

One way we quantify this is by reporting the variance (or estimated variance) of the estimator along with the estimate

This can give a lot of information about how reliable our estimate is and how confident we are in our estimate.

The **standard error** of an estimate is the standard deviation (or estimated standard deviation) of the estimator.
For example, $\bar X_n$ has mean $\mu$ and variance $\frac{\sigma^2}{n}$, so $SE(\bar X_n) = \frac{\sigma}{\sqrt{n}$ (n is our sample size).

Often we don't know the true variance $\sigma^2$ of the population and need instead to estimate it. In that case we have $SE(\bar X_n) = \frac{\hat \sigma}{\sqrt{n}$ (so we'll just plug-in the estimate for sigma squared).

Sometimes we put the standard error in parentheses right after the estimate

The standard error certainly gives us some idea of how tightly concentrated around the unknown parameter the distribution of the estimator is. That’s useful, but sometimes it might be useful to report essentially equivalent information in a different form, an interval.

In other words, we could construct an interval using information about the distribution of the estimator. That interval will be narrow when the estimator has a tight distribution, and wide when it has a dispersed distribution.
We’ll call it a confidence interval.


### 060202 - Confidence Intervals ###

We want to find two functions of the random sample $A(X_1, ..., X_n)$ and $B(X_1, ..., X_n)$ such that
$P(A(X_1, ..., X_n) < \theta < B(X_1, ..., X_n)) = 1 - \alpha$

That is, we want to find two random functions (i.e. functions of random variables) such that the true parameter (fixed but unknown) is between them with a probability equal to $1-\alpha$ (named the "degree of confidence" or "confidence level", it is something we choose for, e.g. 0.05 or 0.99).

Note tyhat we can write a probabilistic statement not because of $\theta$. $\theta$ is unknown but constant. It is the realisations of the smaple data, and hence A and B that are random.

What we did before instead was a "point estimate" and we was looking for a single function, e.g. if we were looking for a unbiased estimation, a $\hat \theta$ such that $E(\hat \theta(X_1, ..., X_n)) = \theta$.

The interval $[A(X_1, ..., X_n), B(X_1, ..., X_n)]$ is said to be a "$1-\alpha$ confidence interval for $\theta$"
(so $\alpha$ is the probability that the estimate is outside the given range).

These two functions are not unique.
We have a distribution of an estimator, and what we're going to do is we're going to use the information of the distribution of some estimator for theta to construct these confidence intervals.
And we could imagine a sort of shifting them to the right and left and still having alpha probability outside the interval.
So how do we choose them? Typically, we choose A and B such that $\alpha/2$ of the probability falls on each side of the interval.

It is important to notice that after we have a data set, i.e. the realisation of the $X_i$,  $A(X_1, ..., X_n)$ and $B(X_1, ..., X_n)$ become just numbers, so probability statements involving those quantities and $\theta$ don’t make sense.
So for instance once we have numbers, let's say A is 1.3 and B is 7.9, it doesn't really make sense to say, $\theta$ is between 1.3 and 7.9 with probability 0.95, as there is no more any random variable involved.

Where do we find those functions? You can find them “from scratch.” But in most cases that we will encounter in everyday data analysis, though, others have found those functions and a resulting formula for a “95% confidence interval for the mean of an unknown distribution with sample size greater than 30” (or whatever).

Important to note: in order to find those functions and derive the formulae, one needs to know how an estimator for the unknown parameter is distributed (typically just knowing its mean and variance is **not** sufficient, we really have to know the whole distribution of the estimator to construct a confidence interval).


### 060203 - The Chi Squared Distribution ###

This is precisely where our friends, the $\chi^2$, $t$, and $F$ distributions, come in.
We have to take a little detour into sort of learning about these three additional special distributions.
They are used in something liek the "t-test"", the "f-test"" or the "t-confidence intervals".
We're going to learn today why those distributions are important and where they come in.

These are all distributions that, unlike other special distributions we’ve encountered, don’t really appear in nature, don’t really describe stochastic phenomena we observe. Rather, they were “invented” because estimators or functions of estimators had distributions that needed to be described and tabulated, they're kind of invented for the purpose of constructing confidence intervals and performing tests.

In segment "060103 -  Efficient Estimators" we saw that the sample variance estimator $\bar S^2 = \frac{1}{n-1} \sum (x_i - \bar x)^2$ is the a unbiased estimator for the variance of a distribution.

We define the "chi squared distribution" as $\chi^2_{n-1} \sim \frac{(n-1) S^2}{\sigma^2}$, where $n-1$ is its single parameter of the distribution, known as "degrees of freedom".

When we construct a confidence interval for the variance, we're going to base it on that particular function, and we know that that function has a chi-squared distribution, so that's going to allow us to construct the confidence interval.

If the random sample is i.i.d from a normal distribution, the sample variance follows a $\chi_{n-1}$ distribution.

### 060204 and 060205- The Student's t Distribution ###

**Definition:**

The random variable composed as $\frac{X}{ \left( \frac{Z}{n} \right)^{\frac{1}{2}} }$, where $X \sim N(0,1)$ and $Z \sim \chi_n^2$ and $X$ and $Z$ are independent, follows the t-distribution with parameter $n$.

In particular, for parameter $t-1$, we have:

$\frac{X}{ \left( \frac{Z_{n-1}}{n-1} \right)^{\frac{1}{2}} } \sim t_{n-1}$

For a generic normal distribution $N(\mu,\sigma^2)$ sampling we have that $\frac{\bar X - \mu}{(\sigma^2/n)^{1/2}} \sim N(0,1)$ (this is true at the limit for $n \to \infty$ from sampling from any distribution thanks to the CLT, but here we are sampling from a Normal, so it is just a standardized form of the Normal, we don't need the CLT).

We can hence write:

$\frac{ \frac{\bar X - \mu}{(\sigma^2/n)^{1/2}}   }{ \left( \frac{Z_{n-1}}{n-1} \right)^{\frac{1}{2}} } \sim t_{n-1}$

We know also that the random variable distributed according to the chi distribution for $n-1$ degree of freedom is $(n-1)S^2/\sigma^2$. We can hence write:

$\frac{ \frac{\bar X - \mu}{(\sigma^2/n)^{1/2}}   }{ \left( \frac{    (n-1)S^2/\sigma^2    }{n-1} \right)^{\frac{1}{2}} } \sim t_{n-1}$

Cancelling things we finally obtain: 

$\frac{(\bar X - \mu)}{S/\sqrt{n}}  \sim t_{n-1}$ 

This is exactly the function of $\bar x$ that had a $N(0,1)$ distribution, except instead of having $\sigma$ here, we have $s$.
So basically our estimator for sigma $s$ is here, instead of the true sigma $\sigma$.
So basically what this tells us is, if we form this sort of standardized version of $\bar x$, but instead of using sigma we use s, then instead of having a normal (0,1) distribution, it will have a t-distribution.

In other words, the t-distribution arises when estimating the mean of a normally distributed population in situations where the sample size is small and population standard deviation is unknown

If we can form this function of the estimator $\bar x$r and we know it has this distribution, then we can use that to build a confidence interval around.

As n gets very large, the t-distribution is going to converge to the normal distribution. And so for large n, it doesn't matter if we use the normal or the t distribution.

What happens if we're not sampling from a normal distribution? What happens if we're sampling from some crazy other distribution?
The central limit theorem tells us that our sample mean is normally distributed. But again, that's only true when n is sufficiently large.

The hardest case is when we're sampling from a crazy distribution and our number of observations is very small. Then we can neither appeal to the central limit theorem or to this distributional fact.

To determinate the appropiate number of samples n in an experiment, in order for example to chech if our assumptions of similarity between the t-distribution and the normal one holds, we need to perform a so-called "power analysis".

How large does your sample have to be before you have sort of a reasonable amount of confidence that the estimates that you're deriving from the sample are reasonable? When our sample is small (but still random with respect to the population) it just means that we don't have very much confidence in our estimates. And that's going to be exhibited in how large our confidence intervals and how big the standard error of our estimate is.

**Origin**

The t-distribution was actually formulated by William Sealy Gosset in his job as Chief Brewer in the Guinness Brewery.
He derived and tabulated this distribution to aid in his analysis of data for quality control across batches of beer.
He had a relatively small number of batches of beer, he didn't know what the variance of the distribution that he was sampling from was, and so he was the one who actually formulated and tabulated or derived and tabulated this distribution.
He then published his paper in 1908 in Biometrika under the pseudonym "Student" because, even though the Guinness Brewery was supportive of his efforts, they didn't want it known that their Chief Brewer was publishing papers in mathematical statistics (another researcher at Guinness had previously published a paper containing trade secrets of the Guinness brewery. To prevent further disclosure of confidential information, Guinness prohibited its employees from publishing any papers regardless of the contained information. However, after pleading with the brewery and explaining that his mathematical and philosophical conclusions were of no possible practical use to competing brewers, he was allowed to publish them, but under a pseudonym ("Student"), to avoid difficulties with the rest of the staff).

### 060206 - The F-distribution ###

**Definition:**

The random variable composed as $\frac{X/n}{Z/m}$, where $X \sim \chi_n^2$ and $Z \sim \chi_m^2$ and $X$ and $Z$ are independent, follows the f-distribution with parameters ("degree of freedom") $(n,m)$.

The Fisher–Snedecor f-distribution (after Ronald Fisher and George W. Snedecor) is a continuous probability distribution that arises frequently as the null distribution of a test statistic, most notably in the analysis of variance (ANOVA), e.g. the F-test.

Why is that a useful fact? Suppose we have samples from two different populations.
We might want to know whether the distributions in the two populations were, in fact, the same.
If they are, we can form the ratio of the sample variances divided by their degrees of freedom (true variances canceling because they’re the same) and the ratio then has the above distribution.

So, if the value of F we obtain after we plug the data is quite odd, we can reject the hypothesis it's the same population our data come from.


### Exercise on the applicability of the chi, t and f distributions ###
Which distribution is useful for testing the following hypotheses ? :

**Scenario A**. You think the true variance is $\sigma^2$, and you want to use your data to make inferences about $\sigma^2$

==> $\chi^2$

Since you know the that $\frac{(n-1)S^2}{\sigma^2} \sim \chi_{n-1}^2$, you can construct your test-statistic, and compare that to the $\chi^2$ with $n-1$ degrees of freedom.

**Scenario B**. You think the true mean is 0, and you want to use your data to test whether $\mu=0$. You don't know the true variance.

==> $t$

When the population variance is unknown, the standardized sample mean follows a t-distribution with $t-1$ degrees of freedom. So by comparing your test statistic to the t-distribution you can make inferences about the sample mean.

**Scenario C**: You want to compare the variances in two independent population.

==> $t$

As explained in question 1, we know that the ratio of the sample variances divided by the their degrees of freedom follows an F distribution if the variances are equal. So if you compare the test statistic you obtained with the F distribution, you can test whether the variances are in fact equal. 

### 060207 and 060208 - Constructing Confidence Intervals ###

So let’s construct some confidence intervals. We will focus initially on two cases. These are not the only cases you will ever encounter, but they are, by far, the most important:

- Case 1: We are sampling from a normal distribution with a known variance and we want a confidence interval for the mean.

- Case 2: We are sampling from a normal distribution with an unknown variance and we want a confidence interval for the mean.

**Case 1**

We have an estimator for the mean, $\bar X$ , which (the estimator!) has a normal distribution itself with mean $\mu$ and variance $\sigma^2/n$ (where $\mu$ and $\sigma^2$ refers to the mean and variance of the distribution from which X are sampled) .

Hence, $\frac{\bar X - \mu}{\sigma / \sqrt{n}}$ is a STANDARD normal distribution N(0,1). 

For any given $\alpha$ that we choose we have:

$P(\Phi^{-1}(\alpha/2) < \frac{\bar X - \mu}{\sigma / \sqrt{n}} < -\Phi{-1}(\alpha/2) ) = 1-\alpha$

where $\Phi^{-1}(\alpha/2)$ is the inverse CDF of the standard normal evaluated at $\alpha/2$, and the negative sign in front of the upper bound is the value of the other tais as of simmetry ($\Phi{-1}(1-\alpha/2) = -\Phi^{-1}(\alpha/2)$):

```
* CDF of the standard normal distribution
+ CDF of the t-distribution

  F_X(x)/|\
         |
      1  |                                         *      +
         |                                *     +
1-alpha/2| ---------------------------*----+
         |                         *  | +  |
         |                       *  + |    |
         |                     +*     |    |
         |                     *      |    |
         |               +  *         |    |
alpha/2  |-----------+---*            |    |
         |      +    *   |            |    |
     0   |+   *      |   |            |    |
         -------------------------------------------------> x
                         |      |     |
          Phi^-1(alpha/2)       0      Phi^-1(1-alpha/2)
                     |                     |
         t^-1(alpha/2)                     t^-1(1-alpha/2)
```

It's not in the form of a confidence interval yet.
Remember, we have to have $\mu$, the unknown parameter, kind of isolated in the middle.

Rearranging we have:

$P(\bar X + \Phi^{-1}(\alpha/2) \frac{\sigma}{\sqrt{n}} < \mu < \bar X - \Phi^{-1}(\alpha/2) \frac{\sigma}{\sqrt{n}} ) = 1-\alpha$

(note that $\Phi^{-1}(\alpha/2)$ is negative)

This is still a probability statement, until we actually plug in the realizations. What is random is NOT $\mu$, but out CI range!

The definition of a confidence interval is that of a range made by two functions a and b such that the probability that A (of x) is less than or equal to $\mu$, that in turn is is less than or equal to B (of x) is 1 minus $\alpha$.

The $1-\alpha$ CI is hence $[\bar X + \Phi^{-1}(\alpha/2) \frac{\sigma}{\sqrt{n}}, \bar X - \Phi^{-1}(\alpha/2) \frac{\sigma}{\sqrt{n}}]$

After we plug realisations, the Confident interval become just two numbers.

The Confidence Interval it's essentially the same information that you would be presenting more or less if you just reported a standard error. But it's in a different form. It's a little bit a matter of convention, e.g. in medicine they use more CIs, in economics the yuse more tests (e.g. we can reject with X% confidence that this is zero) and standard errors.

**Case 2**

We have an estimator for the mean, $\bar X$ , which (the estimator!) has a normal distribution with mean $\mu$ and variance $\sigma^2/n$ (where $\mu$ and $\sigma^2$ refers to the mean and variance of the distribution from which X are sampled), but here we don't know the variance of the distribution, $\sigma^2$.  And because we don't know the variance of the distribution that we're sampling from, we also don't know the variance of $\bar X$, because remember the variance of $\bar X$ is $\sigma^2/\sqrt{n}$, so we don't know its variance, we have only the sample standard deviation $S$.

We do know, thought, that $\frac{\sqrt(n)(\bar X-\mu)}{S} \sim t_{n-1}$.

Hence, in a way similar to case 1, we can write:

$P(t_{n-1}^{-1}(\alpha/2) < \frac{\sqrt(n)(\bar X-\mu)}{S} < -t_{n-1}{-1}(\alpha/2) ) = 1-\alpha$

Rearranging we have:

$P(\bar X + t_{n-1}^{-1}(S/2) \frac{\sigma}{\sqrt{n}} < \mu < \bar X - t_{n-1}{-1}(\alpha/2) \frac{S}{\sqrt{n}} ) = 1-\alpha$

And the $1-\alpha$ t confidence interval is hence $[\bar X + t_{n-1}^{-1}(\alpha/2) \frac{S}{\sqrt{n}}, \bar X - t_{n-1}^{-1}(\alpha/2) \frac{S}{\sqrt{n}}]$

**Comparision of Case 1 and Case 2**

The t distribution is similar to the standard normal (bell shaped, symmetric, centered at zero,...) but has “fatter tails.” It converges to the normal as $n \to \infty$:

$t_{n-1}^{-1}(\alpha/2) ~~\to~~ \Phi^{-1}(\alpha/2) ~~as~~ n \to \infty$

The t gives you a wider interval than the normal for finite n. In other words, for small values of n, like n equals 10 for instance, the t distribution is going to be kind of more spread out compared with the normal distribution.

Because it has sort of thicker tails, for the same $\alpha$ the inverse of the t distribution is going to be further out of the centre than for the inverse of the normal. For the confidence interval, this means that the t gives you a wider confidence interval than the normal for finite n (~below 50).

The intuition is that we are less sure of the distribution of our estimator because we don’t know Var(X) and must estimate it. The t “penalizes” our confidence interval by making it wider, reflecting our greater uncertainty. As n goes to infinity, our uncertainty becomes relatively less important.

**Variations from the theme**

We don’t always fall into case 1 or case 2. What do we do then? Using facts that we know about how functions of random variables are distributed, we can construct a confidence interval “from scratch” on our own.

In practice, we usually appeal to CLT-like results to argue that the estimator has an approximate normal distribution, and then just use the t confidence interval formula with an estimated variance (for large n, the t and normal confidence intervals are the same.)

We're kind of assuming that we're in case two even if we're not sampling from a normal distribution, as if we appeal to central limit theorem type results, we can claim that that function of x bar is going to be approximately normal. And if n is really big, using t or normal distribution doesn't matter any more.


### 060209 - Hypothesis Testing ###
(no videos)

Well, now we know what an estimator is, how to estimate unknown parameters, and a couple of different ways to express how confident we are in our estimates (standard error and Confidence Interval). That gets us a long way and gives us a very good foundation for studying all kinds of estimation going forward.

One more foundational bit is quite important: hypothesis testing.

In social science (as well as other settings for data analysis), we often encounter questions that we want to answer.
For example, _do the lifespans of popes follow a lognormal distribution? Does the income tax rate affect the number of hours employees are willing to work? Do used books cost more on the internet than they do in brick and mortar stores? Has NAFTA hurt US manufacturing workers?_

The tool that statisticians have invented to help answer such questions (and quantify how confident we are in the answers) is the hypothesis test.

Purpose: Given a random sample from a population, is there enough evidence to contradict some assertion about the population?

Let’s build the structure underlying the hypothesis test. First, we’ll need a bunch of definitions:

- An **hypothesis** is an assumption about the distribution of a random variable in a population;
- A **maintained hypothesis** is one that cannot or will not be tested;
- A **testable hypothesis** is one that can be tested using evidence from a random sample;
- The **null hypothesis**, $H_0$ , is the one that will be tested.
- The **alternative hypothesis**, $H_A$ , is a possibility (or series of possibilities) other than the null.

For instance, we might want to perform a test concerning
unknown parameter $\theta$ where $X_i \sim f(x;\theta)$.
$H_0$ : $\theta in \Theta_0$
$H_A$ : $\theta in \Theta_A$ , where $\Theta_0$ and $\Theta_A$ are disjoint.

More definitions:

- A **simple hypothesis** is one characterized by a single point, i.e., $\Theta_0 = \theta_0$.
- A **composite hypothesis** is one characterized by multiple points, i.e., $\Theta_0$ is multiple values or a range of values.

Usual set-up: simple null and composite alternative

### Example ###

$X_i$ is  i.i.d. ~ $N(\mu,\sigma^2)$, where $\sigma^2$is known. ---> the maintained hypotheses

Interested in testing whether $\mu = 0$.                        ---> the testable hypothesis

**Problem type a:**

- $H_0 : \mu = 0$ --> the null hypothesis, simple
- $H_A : \mu = 1$ --> the alternative hypothesis, simple

We're only allowing for the possibility that mu is equal to 0 or 1. We're not allowing for any other possibilities.
Easiest case to analyse.

**Problem type b:**

- $H_0 : \mu = 0$ --> the null hypothesis, simple
- $H_A : \mu \neq 0$ --> alternative hypothesis, composite, two-sided

**Problem type c:**

- $H_0 : \mu = 0$ --> the null hypothesis, simple
- $H_A : \mu > 0$ --> alternative hypothesis, composite, one-sided


We then either “accept” or “reject” the null hypothesis based on evidence from our sample.


Obviously, mistakes can be made---we can “reject” a null that is true or “accept” a null that is false. We want to set up our hypothesis test to analyze and control these errors.

We first need a taxonomy:

```
                   H_0 true      H_0 false
      accept H_0   No error      Type II Error
      reject H_0   Type I error  No error          
```

- The **significance level** of the test, $\alpha$, is the probability of type I error.
- The **operating characteristic** of the test, $\beta$, is the probability of type II error.
- We call 1-α the **confidence level**. We call 1-β the **power**.
- Finally, we define the **critical region** of the test, $C$ or $C_X$, as the region of the support of the random sample for which we reject the null hypothesis.


### 060212 and 060213 - Hypothesis Testing Example ###
(no videos)

How to find the critical region ? Let's go back to the example, case (a) and let's assume we have only two smaples.
What kind of sampled outcome would lead us to believe the null, or at the opposite doubt the null in favour of the alternative?

We can characterize the critical region in terms of the sample mean, i.e. higher or below 0.5


```
X1 |
   *
   | *
   |   *        reject (choose Ha)
   |     *
   |       *
   |accept   *
   |(choose H0)*
   --------------*--------> X2
                   *
                     * X1+X2=k
```   
There's some constant k that's going to divide where our sample lives into two regions.  We get large values of our sample,
we want to reject the null, we get small values we want to accept the null.

Instead of thinking about the x1, x2 space, we can just think about forming the sum of x1 and x2.
That's going to be exactly equivalent to what we just put up, but maybe it's easier to think about just forming this sum and rejecting for large values of the sum, and not rejecting for small values.

And also equivalent to the picture, is instead of forming the sum, form the sample mean.

All these three ways of thinking (graphical, the sum or the mean) are equivalent and would result in identical procedure, but the graphical one become hard at the sample size (i.e. the dimensions) increases.

We use the "mean" method, i.e. we’ll base our testing procedure on the **test statistic** $T = \bar X$, accepting the null for "small" values, and reject it for “large” values.

In other words, critical region C will take the form $\bar X > k$, for some k yet to be determined.
   
How do we choose k? We have to trade off two types of error.

Distribution of the test statistics under the null and the alternative:

```

* f_x under H0 
+ f_x under HA

                  *    |           +
               *    *  |         +    +
             *        *|       +        +
            *          *      +          +
           *           |*   +             +
          *            |  *                 +
        *              | +  *                 +   
    *                 +|       *                +
*                 +    |          *                 +
              +        |               *                 +
------------------------------------------------------------->
                 0beta k  alpha     1

```   

- $\alpha = \int_k^{+\infty} f_{\bar X}(\bar X | H_0) d\bar X$  - type I error probability : rejecting the null when it is indeed true
- $\beta = \int_{-\infty}^{Beta} f_{\bar X}(\bar X | H_A) d\bar X$  - type II error probability : accepting the null when it is indeed false ($H_A$ is instead true)
   
Choice of any one of α, β, or k determines the other two.
Furthermore, choosing them involves an explicit trade-off between the probability of type I and type II errors:

- increasing $k$ means $\alpha \downarrow$ and $\beta \uparrow$
- decreasing $k$ means $\alpha \uparrow$ and $\beta \downarrow$

#### Numerical example ####

We implement the example of before, case a for n=25 and $\sigma^2=4$.

The test statistic $T = \bar X$ is hence distributed according to $N(0, 4/25)$ under $H_0$ and $N(1, 4/25)$ under $H_A$.


- $X \sim N(0,\sigma^2=4) => T = \bar X \sim N(0, \sigma^2=4/25) => \alpha = P(T>k|\mu=0) = 1 - \Phi(\frac{k-0}{2/5})$
- $X \sim N(1,\sigma^2=4) => T = \bar X \sim N(1, \sigma^2=4/25) => \beta = P(T<k|\mu=1) = \Phi(\frac{k-1}{2/5})$

(see 050109 for the standardisation of the Normal distribution)


Choosing different values of k leads to the following chart in the $(\alpha,\beta)$ space:

```
beta |
     |  *
     |  
     |   *
     |   
     |    *
     |     *
     |      *
     |       *
     |         *
     |            *
     |                   *      *
     ----------------------------> alpha
```

If we then increase n we see that the function go "sticker" to the two axis, while with smaller samples it becomes more of a straight line.

Also with larger sample the two normals become more taller and slimmer (the variance of the test statistics decreasing) getting tighher around their means and the area that intersect them (the $\alpha$ and $\beta$ errors) much thinner.


#### Some notes on hyphotesis testing ####

**Choice of the null/alternative hypothesis**

How do we know which hypothesis should be the null and which should be the alternative?

- We are free to choose, but often choose so that the type I error (rejecting a true null) is the more serious of the errors. Then we will choose k so that its probability $\alpha$ is at an acceptably low level, such as .05 or .01.

(Of course, if n is really large and we keep α fixed, then β might be very small, which might not be what we want.) ??

**Composite alternative**

What if $\mu$ is not either 0 or 1 (e.g. problem types (b) and (c)) ?

- Much of the time we set up a hypothesis test so that $\Theta_0 \cup \Theta_A$ is the entire parameter space. So either the null or the alternative must be true. That means that one or both of the hypotheses are composite. When we have composite hypotheses, the test becomes more difficult to analyze. In particular, $\alpha$ and $\beta$ may no longer be values (functions of k), but could instead be functions of the unknown parameter(s) $\theta$.
- For example in problem type (b) ($H_0: \mu=0; H_A: \mu \neq 0$) we could use the same test statistic $T=\bar X$, but what should the critical region look like ?


### 060214 - Power Calculations in Experimental Design ###
(incomplete)

On how to design the experiment so that we can easily interpret its results.

Suppose we're interested in designing a simple, completely randomized experiments to test some hypothesis. How large should my sample be?
Sampling is expensive, but on the other hand sample size influence the variance of my estimators and hence the CI.

How small of a sample can I get away with and be able to say something that is going to be meaningful. This is called power calculation:  what's going to be the power of our test, i.e. the $1-\beta$ or the probability of not making type II error, the probability of not making the error of accepting the null even when the null is not actually true.


### 060215 - Estimating Tau and Sigma and 060216 - Power Calculations in Practice ###

Go to hell. Very badly designed course :-(








