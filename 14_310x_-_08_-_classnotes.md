---
title: 14_310x - Module_08 - Single and Multivariate Linear Models 
author: "Antonello Lobianco"
date: "30 October 2018"
output:
  html_document: default
  pdf_document: default
---


# 14_310x - Module_08 - Single and Multivariate Linear Models #

##  0801 -  The Linear Model ##

###  080101 - Motivation for the linear model ###

A little bit of review (what done in first half of the semester):

- After establishing a foundation in probability, we proceeded to the estimation of unknown parameters (we talked about criteria for assessing them as well as where they might come from).
- Most, if not all, of that foundational discussion was focused on estimating parameters of a univariate distribution (like the mean or the variance or some other parameter that characterizes it).
- So much of what we care about in social science (and many other settings as well) involves _joint distributions_, though.
  - so we really have to be concerned with how to estimate parameters, characterizing either joint distributions, or conditional distributions
- Esther’s discussion of causality was the beginning of (and a special case, really) of our consideration of the joint distribution of variables of interest and how we will estimate parameters of these joint distributions. You can think of much of what she did as considering the joint distribution of two variables where one was simply a coin flip (H: treatment, T: control) and the other was the outcome of interest (e.g., infant mortality, or website effectiveness).
- And, in fact, we were mostly concerned with the conditional distribution of the outcome variable conditional on the coin flip.
- We can (and did) think of the treatment and control groups being two separate populations, and we were interested in, say, testing whether their means were equal.
- We can also think about having one population and a joint distribution of those two random variables on that population.
  - what we were really doing is estimating conditional means, conditional on this coin flip random variable
  
These are of two different  but equivalent ways to think about randomized control trials.

*Linear model and linear regression*

- What if, instead of a coin flip, the first random variable is continuous? It can take on a whole range of values.
  - How do we analyze the conditional distribution of our outcome variable conditional on something like a continuous random variable?
  - How do we estimate the parameters of that conditional distribution?
- That's what are we going to be talking in this lesson.
- The workhorse model we use is the _linear model_ and the way we estimate the parameters is _linear regression_.


Why do we care about joint distributions and estimating the parameters associated with them? Some motivations and exemples:

- prediction
  - If I am the type of person who reads xkcd comics, am I also the type of person who is likely to click on an ad for a t-shirt bearing the Russian cover design of Moby Dick?
  - Is someone who drives a Volkswagen more likely to live in neighbourhood X or neighbourhood Y ?
- determining causality
  - If I give my dog a treat every time he does not bark at another dog walking by our house, will he stop barking at other dogs?
  - Does paying students for reading books increase their scores on critical reading exams?
- just understanding the world better
  - Are people only influenced by price, quality, characteristics, and expected weather when they purchase a convertible, or are they also influenced by the weather on that particular day?
  - "The Psychological Effect of Weather on Car Purchases", QJE, 2014, https://doi.org/10.1093/qje/qju033
  - Are students’ career choices influenced by which companies give out the nicest t-shirts at career fair?
  
In each of those examples, there were two or more random variables, jointly distributed, and we would like to know characteristics of their joint distribution in order to answer the questions.

Up to this point, we have primarily dealt with univariate distributions – probability distributions of only one random variable. In general, though, multivariate distributions – probability distributions of multiple random variables -- are more useful in real life.

In the multivariate examples we’ve seen so far (the Random Control Trials), our population in some sense had two random variables: one discrete (“treatment” or “control”) and one continuous (test scores, for example). In this example, each member of the population was assigned either “treatment” or “control”, and each member of the population received some score on the end-of-treatment test. We could imagine calculating the conditional distribution of test scores based on whether a member was assigned to treatment or control. The linear (regression) model allows us to perform a similar calculation in the more general case where both variables are continuous. 

We can see the X,Y linear regression model as a method that allows us to estimate parameters associated with joint distributions, in other words to estimate parameters for the conditional distribution of an outcome variable based on continuous random variables.


### 080102 and 080103 - Outlining the Linear Model and its assumptions ###

**The linear model, bivariate style**

In each of these examples, we had two or more random variables that had a joint distribution and we wanted to know the characteristics of their joint distribution in order to answer the questions that we were interested in. When we look at the function Y = g(X), we look at it as the expected value of Y conditional to X (for a clear comparition between marginal and conditiona ldistribution see t[his Stack Overflow question](https://math.stackexchange.com/a/2667856/234027)).

The linear model is a model that establishes a relationship between two random variables on which we have repeated observations.

Linear model:

$Y_i = \beta_0 + \beta_1 X_i + \epsilon_i ,  i = 1, ..., n$

where :

- $Y_i$ and $X_i$ are two random variables (on which we have repeated observations)
- $Y_i$ is the "dependent variable" (or "explained variable" or "regressand" - these two terms less used)
- $X_i$ is the "independent variable" (or "explanatory variable" or "regressor" - these two terms more used)
- $\epsilon_i$ is an unobserved random variable, the error
- $\beta_0$ and $\beta_1$ are the parameters to be estimated, the "regression coefficients"" (there will be an other less obvious parameter we'll take about it later)

This model allows us to consider **the mean** of a random variable Y as a function of another (random) variable X.
If we obtain estimates for $\beta_0$ and $\beta_1$, we than have an estimated conditional mean function for Y.


**Assumptions** 

The above linear model in addition to the following basic assumptions is known as the _classical linear regression model_:

1. $X_i$ and $\epsilon_i$ are uncorrelated r.v.
2. identification: $(1/n)\sum_i(X_i-\bar X )^2 > 0$
  - we have some variation in the X variable
  - we rule out a case like "all the observations have the same $X_i$ but different $Y_i$"" because it doesn’t give us the variation in X that we need to identify the mean of Y conditional on X
3. zero mean: $E(\epsilon_i) = 0$
 - We rule it out, but we don’t have any information that would help us figure out whether the mean of the epsillon was non-zero and the intercept was just different
 - an other way to say this assumption is that the expectation of the error and the intercept of the regression line, are not separately identified, we don't have any way to sort them out with the data
4. homoskedasticity: $E(\epsilon_i^2) = \sigma^2$ for all i
 - $Var(\epsilon_i^2) = E(\epsilon_i^2)-E(\epsilon_i)^2 = \sigma^2-0=\sigma^2$: the variance of the error term is constant across X
 - under heteroskedasticity the error under certain regions of the X have higher variance
 - use k and not c for homo/heteroskedasticy ("skedasticy"" meaning scattered)
 - heteroskedasticity would actually not be a fatal problem, we can deal with it, but for now we are assuming homoskedasticy
5. no serial correlation: $E(\epsilon_i \epsilon_j) = 0 ~if~ i \neq j$
 - errors are not correlated if they're associated with different observations
 -  a stronger way of saying this is that the errors are independent across observations. We don't need to impose that, we're just going to say that they're uncorrelated.
 - with positive serial correlation, errors are clustered around values of Y on different regions of X, so if an error is positive (negative), surronding errors are more likely to be positive (negative) too
 - again this is not fatal, we can deal with this, but we're going to assume for now that we don't have it

Notes on assumptions:

- We sometimes impose an alternative assumption to (1.) for our convenience: $X_i$ are fixed in repeated samples, or nonstochastic
  - this makes some of the proofs easier
- Assumptions 3 to 5 could be subsumed under a single, stronger assumption: $\epsilon_i ~ i.i.d. ~ \sim N(0,\sigma^2)$
 - like having a normal surrounding each point of the regression line
 - when we'll speack about inference we'll impose this stronger assumption
 - this stronger version is not necvessary compared with the original three assumptions 3 to 5, but makes proofs easier and more elegant 
 
### 080104 - Defining the Linear Model ###

**Properties of model**

The following properties are valid under the assumptions above, and in particular the assumption, just for our convenience, that the x's are non stochastic. And so we haven't written that the expectation/variance/covariance of y conditional on x, but we can think of $Y_i$ (with the index subscript $i$) as basically the conditional distribution of y given x.

- $E(Y_i) = E(\beta_0 + \beta_1 X_i + \epsilon_i) = \beta_0 + \beta_1 X_i + E(\epsilon_i) = \beta_0 + \beta_1 X_i$
- $Var(Y_i) = E((Y_i - E(Y_i))^2) = E((\beta_0 + \beta_1 X\i + \epsilon_i - \beta_0 - \beta_1 X\i)^2) = E(\epsilon_i^2) = \sigma^2$
  - $Var(Y_i) = Var(\beta_0 + \beta_1 X_i + \epsilon_i) = Var(\epsilon_i) = \sigma^2$
  - so the variance of $Y_i$ is the variance of the error term (remember, $Y_i$ is conditional to X!. The variance of $Y$ - without the subscript $i$ - would be higher) 
- $Cov(Y_i,Y_j) = 0, i \neq j$ (can be shown using properties of $\epsilon_i$)


**Estimates for the βs**

The βs are parameters in the conditional mean formula. How do we find estimates for β0 and β1? Three different options:

- least squares: $\min_{\beta} \sum_i(Y_i - \beta_0 - \beta_1 X_i)^2$
 - we measure the sum of squares of the vertical distance between the observed points and the regression line ($Y_i - \beta_0 - \beta_1 X_i$, called "residuals")
 - also known as the OLS : Ordinary Least Square estimator
- least absolute deviations: $\min_\beta \sum_i|Y_i - \beta_0 - \beta_1 X_i|$
 - we measure the sum of absolute values of the vertical distance between the observed points and the regression line 
- reverse least squares: $\min_β \sum_i(X_i -\beta_0/\beta_1 - Y_i/\beta_1)^2$
 - we minimise the sum of squares of the _horizontal_ distance between the observed points and the regression line 

### 080105 - Properties of Least Squares Estimation ###

We’ll focus on least squares (sometimes called “**ordinary least squares**” or **OLS**). Why?
- Under the assumptions of the Classical Linear Regression Model, OLS provides the minimum variance (most efficient), unbiased estimator of $\beta_0$ and $\beta_1$
- It is the MLE (Maximm Likelihood Estimator) under the normality of errors assumption
- The estimates are consistent and asymptotically normal
- These things are not true of the other estimators (which one in particualar?)
- You may want to use the least absolute deviations estimator when you're particularly worried about the credibility of your data and you think you might have some outliers that you don't want to take out of your data set but you don't want them to have undue influence on your parameter estimates, because the last square estimator, being an estimator of the MLE class, has very nice properties but it is also the most sensible estimator to wrong data (it's more sensitive to observations that are out in the tails because it's squared)
- under heteroskedasticy the least square estimator is still unbiased, but it is no longer the most efficient and its standard error (and hence the inference you would make) would be biased, so we will need to modifying it.

Do we have to do a numerical minimization every time we want to solve for the least squares estimates?
No, we have lovely, closed-form solutions:

$\hat \beta_1 = \frac{(1/n)\sum_i(X_i - \bar X)(Y_i - \bar Y)}{(1/n)\sum_i(X_i - \bar X)^2}$
$\hat \beta_0 = Y - \beta_1 \bar X$

- If we go on to do more advanced econometrics, we would in fact encounter estimators where we have to do complicated numerical minimizations every time to get parameter estimates, but OLS is not one of them, we have for it a nice closed-form solutions
- How do we get these? Pages of tedious calculations, up [on the website](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/fa3dade4b3069a8c9bf221c44df1696f/asset-v1:MITx+14.310x+3T2018+type@asset+block/Derivation_of_OLS_Estimators.pdf) for your viewing pleasure.
- I don’t want you to get the idea that OLS estimators are horrible, complicated things. They are very elegant and intuitive, but this summation-based notation is not up to the task.
- And they could be lovelier still if we weren’t too afraid of using [matrix notation](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/eaff3f2526749d9a0788ad33a9621c42/asset-v1:MITx+14.310x+3T2018+type@asset+block/Matrix_Notation_etc.pdf) . . .
- Note that the numerator is the covariance between $Y$ and $X$, while the denumerator is the variance of $X$.
- Also, for the bivariate linear regression, the predicted value when $x= \bar x$ is $\bar y$ 

### 080106 - Understanding Least Squares Estimation ###

**Definitions**

A couple of important definitions:

- residual: $\hat \epsilon_i = y_i-\hat y_i$ - the deviation between an ordered pair of a particular $x_i$ and $y_i$ and the fitted regression line.
- fitted value: $\hat y_i = \hat \beta_o + \hat \beta_1 x_i$ -  it is, for any particular value of $x_i$, the value of $\hat y_i$ on the fitted line
associated with that value of $x_i$
- regression line, or fitted line: $\hat \beta_0 + \hat \beta_1 \vec{x}$ 

What do we always ask when we learn about a new estimator (and why do we ask it)?
  - We want to know how is it distributed (because we cannot perform inference, creating confidence intervals and running hypothesis tests, unless we know how it is distributed or we don't know at least the variance of our estimators)
  
Let's use (for convenience) $\bar x$ as $\frac{1}{n} \sum_i x_i$ and $\hat \sigma^2$ as $\frac{1}{n} \sum_i (x_i - \bar x)^2$

**Moments of the βs estimators**

|            |$\hat \beta_0$                                                     |$\hat \beta_1$                               |
|------------|-------------------------------------------------------------------|---------------------------------------------|
|Mean:       |$\beta_0$                                                          |$\beta_1$                                    |
|Variance:   |$\frac{\sigma^2 \bar X^2}{n \hat \sigma_x^2} + \frac{\sigma^2}{n}$ |$\frac{\sigma^2}{n \hat \sigma_x^2}$         |
|Covariance: |$\frac{-\sigma^2 \bar X}{n \hat \sigma_x^2}$                       |$\frac{-\sigma^2 \bar X}{n \hat \sigma_x^2}$ |

As OLS estimates are unbiased, their mean is the parameter itself.

How do we get these? Pages of tedious calculations, up [on the website](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/fa3dade4b3069a8c9bf221c44df1696f/asset-v1:MITx+14.310x+3T2018+type@asset+block/Derivation_of_OLS_Estimators.pdf) for your viewing pleasure.


### 080107 - Comparative static ###

Some comparative statics of the estimates - imagine having two different data sets, they're identical except for one of them having:

- A larger $\sigma^2$ (variance of the error) means _larger_ $Var(\hat \beta_j)$ (both $\beta_0$ and $\beta_1$)
  - points more scattered than with a low $Var(\hat \beta_j)$ (where the points would be closer to the regression line)
  - we are less sure of our estimate, hence higher variance of our estimators
- A larger $\hat \sigma^2$ means _smaller_ $Var(\hat \beta_J)$
  - having a larger variation in our explanatory variable - i.e. the explanatory variable is more spread out - (with a small $\hat \sigma^2$ all observation points are around a region of the X) improves our trust in the estimates
- A larger n means smaller $Var(\hat \beta_J)$
  - It follows from consistency of $\hat \beta_J$
- If $\bar x > 0$, $Cov(\beta_0,\beta_1) < 0$ (and the opposite)
  - A mechanical relationship between the two estimates:  if $\bar x$ is positive, if we overestimate the intercept of the line we're going
to underestimate the slope, an underestimate of the intercept is associated with an overestimate of the slope (?still unclear)
  - We know that $\hat \beta_0$ and $\hat \beta_1$ are related as follows: $\hat \beta_0 = \bar Y - $hat \beta_1 \bar X$. Therefore, if we overestimate or underestimate the intercept $\beta_0$, then the slope $\beta_1$ will have to make up for it (by doing the opposite). 

**Normal approximation**

One step further: If we use the stronger assumption that the errors are i.i.d. $\sim N(0,\sigma^2)$, we obtain the result that $\beta_0$ and $\beta_1$ will also have normal distributions: normality of errors ==> normality of estimators

Note that the distributions of  $\beta_0$ and $\beta_1$ are functions of $\sigma_2$, the error variance.

But we often don’t know $\sigma_2$, so we need to estimate it.

Let’s use $\hat \sigma^2 = \frac{1}{n-2}\sum_i \hat \epsilon_i^2$ because it's unbiased for $\sigma^2 in the linear model$
 - Why the "-2"" in the denominator? Because we’re estimating two parameters, $\beta_0$ and $\beta_1$, and it turns out that’s what we need for $\hat \sigma^2$ to be unbiased (do you remember our unbiased estimator for the variance when we were just estimating the mean had an n minus 1 in the denominator? Well, here we need the n minus 2 because we're estimating two parameters, $\beta_0$ and a $\beta_1$.)

What happened when we were doing univariate inference and we replaced an unknown variance with an estimate of the variance? The standardized sample mean had a standard normal distribution, with variance equal to standardized normal distribution variance/n, but we actually didn't know what that variance was, and we replaced an estimate for it.

The same thing is going to happen here (using the Student's t distribution!), we have that $\hat \beta_0$ hat and $\hat \beta_1$ have normal distributions (under normality of errors assumption) but we don't know what their variance is. We're going to have an estimated variance instead, and so that's where this sort of t-test in the linear regression context comes from.

### 080109 and 080110 - Analysis of Variance and Goodness of fit ###

**Analysis of the variance**

Now that we have all of the pieces (model, estimators, information about the distribution of estimators, etc.), we could proceed with inference, but we’re going to put that off for a little while.

For now, let’s take a quick detour: analysis of variance. We also going to talk about some sort of practical issues in estimating linear regressions before we get to inference.

We want some way to indicate how closely associated X and Y are, or how much of Y’s variation is “explained” by X’s variation. We perform an **analysis of variance** and that leads us to a measure of goodness-of-fit for our linear regression.


Let’s start by defining the **Sum of Squared Residuals**, **SSR**.

- $SSR = \sum_i(Y_i - \hat Y_i)^2 = \sum_i(Y_i- \hat \beta_0 -\hat \beta_1 X_i)^2 = \sum_i (\hat \epsilon_i)^2$
  - the sum of all the vertical distances from point to regression line, squared
  - it's what we are minimizing in the OLS choosing $\hat \beta_0$ and $\hat \beta_1$ 

This is, in some sense, a measure of goodness-of-fit, but it is not unit-free, which is inconvenient. If we divide by the **Sum of Squares Totals**, that gives us a unit-free measure because the units cancel:

- SST = \sum_i (Y_i-\bar Y)^2
 - The sum of all the vertical lines between each observed point and their average, squared (we don't even need the regression line)
 
Note that the ration between the SSR and the SST is going to be always between 0 and 1: $0 \leq SSR/SST \leq 1$ Because (a) both of these values are non-negative, by construction, and (b) the fact that the regression line is the “least squares” line ensures that SSR <= SST.

Note that SST isn't the maximum error we can have, as we could draw a line that gave us higher error, but SSR is the least error we can have (that's the way we choose the regression line to minimise exactly this quantity). So any other line (including $Y = \bar Y$) is going to give us higher sum of squared errors than the SSR.

**Goodness of fits (R^2)**

I guess we wanted a measure of fit that had larger values when the fit was better, or we explained more, so we defined $R^2 = 1 - SSR/SST$ (it's an arbitrary decision).

It turns out that SST can be decomposed into two terms, SSR and the **Sum of Squares of the Model**, SSM:

- $SSM = \sum_i(\hat Y_i - \bar Y)^2$
  - it is the sum of the vertical distances between the estimated points (i.e. the fitted values of the regression line) and their average $\bar Y$
  - the cross term goes away because of how $\hat \beta$ is chosen.
  - then r squared could also be written as just the model sum of squares over the total sum of squares: $SST=SSR+SSM$, $R^2 = 1-\frac{SSR}{SST}$, $R^2 = 1- \frac{SSR}{SSR+SSM} = \frac{SSM}{SST}$


- In bivariate regression, $R^2$ is equal to $r_{XY}^2$, the sample correlation coefficient for $X$ and $Y$. Why we then need $R^2$?  $R^2$ is however a more general formulation, though, and is defined for linear models with more than one explanatory variable.
- In addition to using $R^2$ as a basic measure of goodness-offit, we can also use it as the basis of a test of the hypothesis that $\beta_1 = 0$ (or \beta_1 = ... = \beta_k = 0$ if we have $k$ explanatory variables):
  - We reject the hypothesis when the test statistic $\frac{n-k}{k-1}\frac{R_2}{1-R_2}$ (i.e. $(n-2)R_2/(1-R_2)$ for k=2 - $\beta_0$ and $\beta_1$), which has an F distribution under the null (and the assumption of normally diustributed errors), is large.
    -  if $R^2$ is large, that means that the variation in x is explaining a lot of the variation in y, we have a high measure for our goodness of fit, and so we want to reject the null that the coefficient on x, this $\beta_1$, is equal to 0.

### 080111 and 080112 - Getting Familiar with Regression Output and interpreting them ###

Let’s talk about a few practical issues, introduce multiple regression (with matrix notation), and then return to inference (it’s just that this summation-based notation is so clunky, we’ll all be happier to see confidence intervals, t-tests, and F-tests in more elegant notation).

What does regression output look like? How do we interpret it?

Here’s some output from Stata on two separate bivariate regressions:

**Results about the regression specific to each explanatory variable**

|      ds|      Coef.| Std. Err.|     t| P > \|t\|| [95% Conf. Interval]|
|--------|-----------|----------|------|----------|---------------------|
| 1hd3rev|  0.0006272|  0.000455|  1.38|     0.173| -0.000281  0.0015354|
|   _cons| -0.0011316|  0.004421| -0.26|    0.799 | -0.009956  0.0076928|

- The first line is $\hat \beta_1$, the second one is $\hat \beta_2$ (stata always put the contant at the bottom)
- `Std. Err.` is the standard errors - the estimated standard deviations of the distributions of those estimators
- The 4th and 5th column are the t-test for the individual coefficients (we'll get to know them later)


**Results about the regression not specific to each explanatory variable**

|                 |         |
|-----------------|---------|
|Number of obs:   |       69|
|F(1,69):         |     1.90|
|Prob > F:        |   0.1727|
|R-squared:       |   0.0276|
|Adj. R-squared:  |   0.0131|
|Root MSE:        |  0.00746|

- Second and third rows are the results of the F-test: we would fail to reject the null that $\beta_1 = 0$ for any reasonable sized testc (we want to reject the null that $\beta_1$ is equal to 0 if we get large values of that f-test)
- If instead (for an other regression) we would have had `F(1,658) = 4.50` and `Prob>F = 0.0376` we would reject the null that $\beta_1 = 0$ for a 5% test, but not for a 1% one. 

- The p-value (reported here as `Prob > F`) is the probabilitys at which, if you were doing a size p-test, you would reject the null, i.e. it tells us the boundary of the size of the test, at which we would reject the null.
- What size test do we usually do? We do 5% tests, we do 1% tests, but here we have to do a test as large as $\alpha = 18%$ test in order to reject the null.
  - We're never going to do an 18% test, so we can't reject the null here.
c

** Results from R**
(other regression)

|              |   Estimate| Std. Error| t value| Pr(>\|t\|)|    |
|--------------|-----------|-----------|--------|-----------|----|
|   (Intercept)| -362.02694|  102.99766|  -3.515|   0.001953|**  |
| gss_data$year|    0.20204|    0.05166|   3.911|   0.000749|*** |

`Multiple R-squared: 0.4101, Adjusted R-squared: 0.3833`
`F-statistics: 15.3 on 1 and 22 DF, p-value: 0.000749`

R prints the data for the intercept at the top.

With this F-test here, we would reject the null that $\beta_1 = 0$ for any reasonably sized test (as the significance level $\alpha$, the probability of Type I error we would make - rejecting the null when it is indeed true - is so small at 0.00075).

How do we interpret our parameter estimates, $\hat \beta_1$ in particular?

  - $\hat \beta_1$ is the estimated effect on Y of a one-unit increase in X.
  - Precise nuances of the interpretation will depend on whether we think we have estimated a causal relationship or something else (more on that later).
  - E.g. in a regression concerning a baseball team, if the dependent variable is "number of attendances [thousands]" and the explanatory variable is "numberw of winned games", a coefficient is 31.17391 would means that one additional win is associated with an additional 31,000 fans in attendance over the course of the season (we might want to exercise a little caution to think that this is a causal relationship, and we'll get to that more later in the semester)


### 080113 - Dummy (or indicator) variables and pratical issues ###

What if X only takes on two values, 0 or 1? We have a special name for that type of variable, a dummy variable (sometimes we call it an indicator variable).

No problem---nothing we have done here rules out any particular distribution for X or possible values of X (well, the pictures would look different, with all the observation concentrated either on X=0 or X=1)

When we estimate a regression where X is a dummy variable, then $\hat \beta_0$ is interpreted as the mean of the group of observations that have the dummy variable equaling 0, and $\hat \beta_1$ is the increment in the mean or the effect of having the dummy variable equal to 1:

```
          Y |
            |               *
            |            *
            |         *  -
            |      *  |  hat
            |   *     |  beta_1
hat beta_0  |*---------  -
            |
            ---------------------------------> X
            0         1
```

Dummy variables serve a number of important roles in linear models. We’ve (sort of) already seen one, RCTs (Randomized Control Trials).

Suppose we have some treatment in whose effect we are interested. We randomly assign the treatment to half of the observations and leave the other half untreated.

We could just simply have done the whole analysis in the linear regression framework, assigning the treated observations X = 1 and the untreated X = 0.

If we estimate the regression $Y_i = \beta_0 + \beta_1 X_i + \epsilon_i $, $\hat \beta_1$ will be the estimated effect of the treatment.

By the way, X need not be randomly assigned half 0s and half 1s to be a dummy variable. Any characteristic that exists on some but not all observations can be represented with a dummy.

We will see other uses for dummy variables when we get to multiple regression - we can use them to multiply, to interact with other explanatory variables and shift slopes around, and trace out non-linear relationships. We can do all kinds of tricky things with dummy variables,

**Nonlinear models**

Isn’t a linear model really restrictive? What if X and Y have a relationship, but it’s not linear?

- Note that the linear model is actually super flexible and can allow for all kinds of nonlinear relationships.
  - When we get to multiple regression, we’ll see some examples.
  - we can take nonlinear functions of both of the variables, x variable and y variable, and analyze those relationships between those nonlinear functions of x and y within a linear framework (in other words, we can transform X and Y using nonlinear functions and perform linear regression on these transformed variables)
  - we can also create interaction variables, which is sort of the product of two variables, and this would allows for other types of non-linearity
- We can do a nonparametric version of a linear regression, kernel regression, but there are tradeoffs, namely efficiency.
  - the kernel regression is sort of infinitely flexible in some sense, but you pay for it, because it's not an efficient estimator at all.

### 080114 - The Multivariate Linear Model ###

See next (080201)


##  0802 -  The Multivariate Linear Model ##

### 080201, 080202 and 080203 - The Multivariate Linear Model and assumptions ###
(no video)

Multivariate linear models is a generalisation of the linear model

Why we need it?

- where we're primarily interested in the relationship between one particular x variable and our dependent or y variable, there could be other factors that might come into play that we need to account for in our analysis
- we may actually have many variables to start with, for example when we don't have any a priori notion about what x variables might be important predictors of y

Let’s consider a more general linear model:

$Y_i = \beta_0 + \beta_1 X_{1,i} + \beta_2 X_{2,1} + ... + \beta_k X_{k,1} + \epsilon_i ,  i = 1, ..., n$

This is a job for matrix notation!

An handout of [matrix notation](https://prod-edxapp.edx-cdn.org/assets/courseware/v1/eaff3f2526749d9a0788ad33a9621c42/asset-v1:MITx+14.310x+3T2018+type@asset+block/Matrix_Notation_etc.pdf) is available in the "resource"" tab of the class.


- let $\vec X_i = (X_{0,i}, ..., X_{k,i})$ being an 1x(k+1) (row) vector ($X_{0,i}==1$)
  -  implicitly, there ia a 1 multiplying $\beta_0$.
- let $\vec \beta = (\beta_0, \beta_1, ... , \beta_k)^T$ being an (k+1)x1 (column) vector

So we have:

$Y_i = $\vec X_i \vec \beta + \epsilon_i, ~~i = 1, ... , n$

This is the model for each individual observation, but we can go even further (omitting the vector symbol):

- let $Y = (Y_1, ... , Y_n)^T$ being a `[n,1]` (column) vector of the outcome variable across the different observations
- let $\epsilon= (\epsilon_1, ... , \epsilon_n)^T$ being a `[n,1]` (column) vector of the error terms across the different observations
- let $X = \begin{bmatrix}  X_{0,1} & X_{1,1} & \cdots & X_{k,1} \\  X_{0,2} & X_{1,2} & \cdots & X_{k,2} \\  \vdots  & \vdots  & \ddots & \vdots  \\ X_{0,n} & X_{1,n} & \cdots & X_{k,n} \\ \end{bmatrix}$ being an `[n,k+1]` matrix (with $X_{0,i}==1$) of our data where the rows are the differnet observations and the columns are the different explanatory variables (the first column being identically equal to 1)

So we have the multivariate regression model as:

|        |  |         |           |   |           |
|--------|--|---------|-----------|---|-----------|
|$Y$     |= |$X$      | $\beta$   | + |$\epsilon$ |
|`[n,1]` |  |`[n,k+1]`| [`k+1,1`] |   |`[n,1]`    |


We can rewrite the assumtions we made as:

1. **identification**: $n > k+1$, X has full column rank $k+1$ (i.e., regressors are linearly independent, i.e., $X^TX$ is invertible)
- we need to have more observations than regressors
- we can’t have any regressors that do not have positive sample variation
- we can’t have any regressors that are linear functions of one or more other regressors
  - For example this would not work: imagine a case where we want to estimate the effect of schooling, work experience, and age, on salary, so we estimate the following model on individual-level data: $Y_i = \beta_0 + \beta_1 X_{1,i} + \beta_2 X_{2,i} + ... + \beta_k X_{k,i} + \epsilon_i$ where:
    - $Y_i$, salary
    - $X_{1,i}$, years of schooling
    - $X_{2,i}$, years of work experience 
    - $X_{3,i}, age
    - $X_{1,i} + X_{2,i} + 6 = X_{3,i}$ for all i in the sample
  - Actually, researchers most often run afoul of this assumption when using dummy variables to indicate, say, observations falling into an exhaustive and mutually exclusive set of classes. Suppose each observation in your data set of pets is either a dog, cat, or fish. You cannot create and include a dummy variable for each type of pet because together they add up to a column of 1s, which is perfectly collinear with the first column of the X matrix. You need to omit one of them.
  - R will tell you if you make this mistake.
    
2. **error behavior**: $E(\epsilon)) = 0, E(\epsilon \epsilon^T) (= Cov(\epsilon)) = \sigma^2 I_n$ (stronger version $\epsilon \sim N(0,\sigma^2 I_n)$) - where $I_n$ is the [n,n] identity matrix
- $\epsilon = \{\epsilon_1, ..., \epsilon_n\}^T$ is a `n,1` raw vector, so $\epsilon \epsilon^T$ is a `n,n` matrix
- $E(\epsilon \epsilon^T) = \begin{bmatrix} E(\epsilon_1 \epsilon_1) & E(\epsilon_1 \epsilon_2) & \cdots & E(\epsilon_1 \epsilon_n) \\ E(\epsilon_2 \epsilon_1) & E(\epsilon_2 \epsilon_2) & \cdots & E(\epsilon_2 \epsilon_n) \\ \vdots  & \vdots  & \ddots & \vdots  \\ E(\epsilon_n \epsilon_1) & E(\epsilon_n \epsilon_2) & \cdots & E(\epsilon_n \epsilon_n) \\ \end{bmatrix} = \begin{bmatrix} Var(\epsilon_1) & Cov(\epsilon_1, \epsilon_2) & \cdots & Cov(\epsilon_1, \epsilon_n) \\ Cov(\epsilon_2, \epsilon_1) & Var(\epsilon_2) & \cdots & Cov(\epsilon_2, \epsilon_n) \\ \vdots  & \vdots  & \ddots & \vdots  \\ Cov(\epsilon_n, \epsilon_1) & Cov(\epsilon_n, \epsilon_2) & \cdots & Var(\epsilon_n) \\ \end{bmatrix} = \begin{bmatrix} \sigma^2 & 0 & \cdots & 0 \\ 0 & \sigma^2 & \cdots & 0 \\ \vdots  & \vdots  & \ddots & \vdots  \\ 0 & 0 & \cdots & \sigma^2 \\ \end{bmatrix} = \sigma^2 I_n$ (where $E(\epsilon_i \epsilon_i) = Var(\epsilon_i)$ and $E(\epsilon_i \epsilon_y) = Cov(\epsilon_i, \epsilon_j)$ is true becasue $E(\epsilon)=0$)
  - This is called the variance-covariance matrix of $\epsilon$ and we denote it with $Cov(\epsilon)$
  
### 080204 -  Deriving Estimators in MV Linear Model ###
(no video)

What is $\hat \beta$?

- It is the vector that minimizes the sum of squared errors, i.e., $\hat \epsilon^T \hat \epsilon = (Y-X \hat \beta)^T(Y-X \hat \beta)$ (`[1,n]` x `[n,1]` = `[1,1]`)
- To compute it, take the derivative of  $\hat \epsilon^T \hat \epsilon$ with respect to $\hat \beta$ and set it to zero and then solve for $\hat \beta$:
  - $\frac{\partial \hat \epsilon^T \hat \epsilon}{\partial \hat \beta} = -2X^T(Y-X \hat \beta) = 0$
  - $-X^T Y + X^T X \hat \beta = 0$
  - $\hat \beta = (X^T X)^{-1} X^T Y$ (if  $X^T X$ is invertible)
  - beautiful
  
What is the distribution of  $\hat \beta$ ?
  
  - $E(\hat \beta) = E((X^T X)^{-1} X^T Y) = (X^T X)^{-1} X^T E(X \beta + \epsilon) = (X^T X)^{-1} X^T X \beta + 0 = \beta$ (we can treate X as fixed, so they can come outside of the expectation operator)
  - Cov(\hat \beta) = \sigma^2(X^T X)^{-1}$
  - $\hat \sigma^2 = \frac{\hat \epsilon^T \hat \epsilon}{n-k}$ is the residual variance (i.e the variance of that’s not explained by the regressors.
    - Note that adding regressors with no explanatory power will just increase the degrees of freedom and leave the residual variance fixed, thereby increasing the variance estimator. However, if the regressor explains some of the variation in the model, then adding it will decrease the variance.
  
If the errors are normally distributed, the estimators of the parameters are also normal. But note that distribution of the errors only affects the distribution of $\hat \beta$, as $\hat \beta$ remains an unbiased estimator irrespective of the distribution of the errors. This assumption is useful for inference purposes (recall the discussion on hypothesis testing, we need to make an assumption about the underlying distribution so we have something to compare your estimator to!). 

### 080205 and 080206 - Inference in the linear model ###
(no video)

Now, finally, we get to inference. Typically, we will want to test hypotheses involving the βs. (The βs are the parameters in our conditional mean function of our outcome variable Y, and the questions we want to answer are usually about the nature of this conditional mean function.)
Sometimes we are only interested in one of the βs. Other times we might want to simultaneously test hypotheses about several of them.
As we saw in the output I showed you earlier, statistical packages typically perform some standard tests for us, but there may be other ones we need to do ourselves.

Let’s start with a pretty general framework for testing hypotheses about β. It’s not only quite general and flexible, it’s also super intuitive.

Let’s consider hypotheses of the following form:

- $H_0: R\beta = c$
- $H_A: R\beta = c$

R is a `r,(k+1)` matrix of "restrictions".

Each row is an individual restiction we set to the model (if $r = 1$, then we are just testing one restriction), where on each restriction we can test a single $\beta_i$ or any linear condition involving multiple $\beta$s.

Indeed, almost any hypothesis involving $\beta$ you can dream up in the context of the linear model can be captured in this framework. You can test whether individual parameters are equal to zero. You can test whether individual parameters are equal to something other than zero. You can test multiple hypotheses simultaneously. You can test hypotheses about linear combinations of parameters. The world is your oyster.

Let's see some examples:

- $H_0: \beta_1 = 0$
  - The null hyphotesis is made of a single restriction, furthermore involving a single parameter
  - This corresponds to $R = \{0,1,0,...,0\}$ (a single `1,k` row) and $c=0$ (a scalar)
- $H_0: \beta_1 = \beta_2 = ... = \beta_k = 0$
  - The null hyphotesis is made of k restrictions simultaneously, each involving a single parameter (further, it's value tested is the same)
  - $R = \begin{bmatrix} 0 & 1 & 0 & \cdots & 0 \\ 0 & 0 & 1 & \cdots & 0 \\ \vdots  & \vdots & \vdots  & \ddots & \vdots  \\ 0 & 0 & 0 & \cdots & 1 \\ \end{bmatrix}$ and $c = \begin{bmatrix} 0  \\ 0 \\ \vdots  \\ 0 \\ \end{bmatrix}$
- $H_0: \beta_1 = \beta_2, \beta_3=5, \beta_k= -2$
  - Here the null is composed of 3 restrictions simultaneously, where the first one include multiple parameters
  - $R =  \begin{bmatrix} 0 & 1 & -1 & 0 & 0 & \cdots & 0 \\ 0 & 0 & 0  & 1 & 0 & \cdots & 0 \\ 0 & 0 & 0  & 0 & 0 & \cdots & 1 \\ \end{bmatrix}$ and $c = \begin{bmatrix} 0  \\ 5 \\ -2 \\ \end{bmatrix}$

One thing you cannot do in this framework is test one-sided hypotheses (ee’ll get back to those.)

You can also test a single parameter by not being, let's say, z by using the t test:

$t = \frac{\hat \beta- \beta_{H_0}}{s.e.(\hat \beta)}$


### 080207 - Examples of Hypothesis Testing ###

We have a super intuitive and cool way to test these hypotheses (first, think of the null as describing a set of restrictions on the model):

1. We estimate the unrestricted model.
2. We impose the restrictions of the null and estimate that model.
3. We compare the goodness-of-fit of the models. If the restrictions don’t really affect the fit of the model much, then the null is probably true or close to true, so we do not want to reject it. If the restrictions really bind, then we do want to reject the null.
  - $H_0$ is the model with the restrictions
    - if the restricted model shows a decreased fitness compared with the unrestricted one:
      - the restrictions are not good
      - I reject the null and take $H_A$
    - if the restricted model shows a similar fitness than the unrestricted one:
      - the restrictions are real
      - I do not reject the null


Estimating the unrestricted model should be simple - just run the regression. But how do we estimate the restricted model?

- If the restriction is that certain βs = 0, then leave the regressors corresponding to those βs out of the restricted model
- If the restriction is that, say, two βs are equal, create a new regressor, which is the sum of the regressors corresponding to those βs and include that sum in the restricted model in place of the original regressors.
  - e.g. from $Y_i = \beta_1 X_{1,i} + \beta_2 X_{2,i} + ...$ to $Y_i = \beta_1 (X_{1,i} + X_{2,i}) + ...$
- What if the restriction is that some β = c? Then that term would not have any parameter that needs to be estimated under the null. We just subtract the constant times X $c X_i$ from the dependent variable $Y$, and rerun that regression, and that's our restricted regression.

So going back, we estimate the unrestricted model. We then impose restrictions and estimate that model.
At that time we compare the goodness of fit, and if the goodness of fit is not very different, we don't reject the null; if it's very different, we reject the null.

This is an F-test (we’ve mentioned a special case of the F test before. This is a more general formulation.
  - $T = \frac{((SSR_R - SSR_U)/r)}{(SSR_U/(n-(k+1))}$
    - where $SSR_R$ is the sum of square of the residuals for the RESTRICTED model, and $SSR_U$ is the sum of square of the residuals for the UNRESTRICTED model
  - $T \sim F_{r,n-(k+1)}$ under the null and we reject the null for large values of the test statistic.
    - Why an F distribution? The reason goes back to one of the facts I told you about special distributions a couple of weeks ago. The ratio of two independent $\chi^2$ random variables divided by their respective degrees of freedom are distributed as F

