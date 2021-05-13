---
title: 14_310x - Module_09 - Practical Issues in Running Regressions, and Omitted Variable Bias
author: "Antonello Lobianco"
date: "7 November 2018"
output:
  html_document: default
  pdf_document: default
---


# 14_310x - Module_09 - Practical Issues in Running Regressions, and Omitted Variable Bias #

##  0901 -  Practical Issues in Running Regressions  ##

###  090101 and 090102 - Using the F-test ###

The t-test is printed out every time we run a regression. Before we talk about how to use it, let's remind us of its mathematical basis.

The t-test is a test where we don't use as a basis for creating critical value, the normal distribution, but instead the t distribution.

Recall that, if the errors have a normal distribution, then so do the $\hat \beta$s.

But their variances (and covariances) depend on the error variance, which we typically will not know. So when we substitute in $\hat \sigma^2$ for $\sigma^2$ in the formula for the $\hat \beta$ variance, the standardized version of $\hat \beta$ now has a t distribution, not a normal distribution any more.

As the sample becomes large enough, it really doesn't matter. The t distribution would become very, very similar to the normal distribution.

That's the mathematical justification for the t-test, but we often don't have or want to assume a normal distribution of the errors (we don't need it to justify the OLS model. We just need that error terms are IID, they don't need to be necessarily normally distributed). We still use the t-test in that case, essentially as a way to make the hypothesis test a little more conservative than one based on a normal distribution, at least for small samples.

So what's happening with the t-test is that the tails of the distribution are a little bit fatter. So the critical value will be a little bit more conservative than with a normal distribution.

- $H_0: \beta_i = c$:
  - c may be zero or any other constant that we know
  - $T = (\hat \beta_i - c)/SE(\hat \beta_i)$, where the standard error for the beta estimator is $SE(\hat \beta_i) = (\sigma^2(X^TX)^{-1})_{ii}^{1/2}$, i.e. the ith diagonal element of the variance-covariance matrix$ (recall that the standard error SE of a estimator is the standard deviation of its distribution or an estimate of that standard deviation)
   - we would tipically employ $\hat \sigma^2$ computed from the residuals ($\hat\ sigma^2 = \hat \epsilon^T \hat \epsilon/(n-k)$) in place of the unknown $\sigma^2$

- $H_0: R\beta = c$:
 - more generic, with hypothesis that potentially involve multiple parameters
 - $T = (R\hat \beta - c)/SE(R \hat \beta)$, where $SE(R \hat \beta) = (\sigma^2 R (X^TX)^{-1} R^T )^{1/2}$
 - Since this is a t-test, and we can only test one hypothesis (potentially involving multiple parameters), R is a `[1,(k+1)]` matrix and c is a scalar here.

Back to the question of when and how it's useful:

- for the simpler case of hypothesis $H_0: \beta_j = c$ versus $H_A: \beta_j = c$, the F-test is equivalent to the t-test (the t-test statistic and critical values are the square root of those for the F-test.)
  - so, you can use either, but it's easier to use the t-test for a single estimated coefficient if $H_0: \beta_j = 0$ since it's printed out right there for you.
- for one-sided test, like $H_0: \beta_j > 0$ versus $H_A: \beta_j < 0$
  - you _need_ a t-test
  
Tests reported by statistical software packages  :

  - the f-test returned by R in the summary of a regression is those of _all_ coefficients (but not the intercept) being 0.
  - the t-test returned by R in the summary of a regression is those of each individual coefficient being 0 (indeed it is reported for each coefficient)
    - for each coefficient we got also at what level we are rejecting the hypothesis that it's equal to 0 (p-value), and this is summarised by the little stars
  - for bivariate regressions (a regression with only the constant and one single other regressos) the t-value squared should be equal to the F-statistics (e.g. 3.911^2 = 15.3)
  - if you're interested in something else, like two coefficients are equal, or two coefficients are equal or opposite sign, you'll have to construct those tests, either as test statistics or as t-test or as f statistics (when possible).
    - in practice, it's like you're going to ask your software to do it for you
  - we don't have the critical values of the tests, but we could check that the t critical value squared is equal to the F critical value.
  - to find the Confidence Intervals for each individual parameter we plus or minus to the estimate the critical value at the alpha we want (e.g. 1.96 for a 95% confidence) times the standard error.
   - if zero is not inside the interval we can reject the null that the parameter is zero
   - R's default is to report test-statistics at the 5% significance level, but you can check at other percentages (package `linearHyphotesis`)
   
### 090103 - Regressions with Binary Dependent Variables on the RHS ###  
  
Now you have particularly have all of the machinery to do all
of the regression that you want.

Pratical issues with regression:

- Dummy Variables
- Other Functional Form issues
- On example of Putting things together : Regression discontinuity Design

We arenot going to talk much or at all about when the **dependent** variable is a dummy variable, which brings a bunch of other difficulties of interpretation that have to do with the fact that errors are certainly not normally distributed, because Y - and the errors - are one or either zero, so by definition errors cannot be normally distributed.

**Dummy Variables**

$Y_i = \alpha + \beta D_i + \epsilon_i$

$D_i$ is a *dummy variable*, or an *indicator variable*, if it takes the value 1 if the observation is in group A, and 0 if in group B.

Examples:

- RCT (randomised control trials): 1 if in treatment group, 0 otherwise
- gender: 1 if male, 0 if female
- time breacks: 1 before great depression, 0 after
- 1 before generic substitution act (for medicines) passed, 0 otherwise,
- attributes of goods: 1 if the house has a deck in the backyard, 0 otherwise,


Interpretation

- Without any other control variables, it is easy to verify that $\hat \beta = \bar Y_A − \bar Y_B$ (if D is a dummy that is 1 when the observation belongs to group A and zero otherwise).
  - So you can always estimate the difference between the treatment and control group for an RCT using an OLS regression framework instead of doing it by hand
  - The standard errors reported in the regression would be slightly different from the Neyman standard errors we computed before
    - because the Neyman standard errors adjust for sample size of EACH group, whereas the OLS standard errors adjust for the size of the overall sample
      - $SE(\hat \beta_i) = \sigma/(n-1)$
      - $SE(Test)_{Neyman} = \frac{\sigma^2_{contr}}{n_{contr}-1} + \frac{\sigma^2_{treatm}}{n_{treatm}-1}$
    - but it won’t matter that much if the samples are large enough or similar in treatment and control groups sizes


### 090104 - Regressions with Categorical Variables ###

From a categorical variable to dummy variables

- We have a categorical variable with many, let's say 50 groups (e.g. 50 states)
- It usually does not make much sense to include it directly as a regressor
- It should be transformed ("encoded" is a more specific word) into 50 dummy variables: for each state, the dummy= 1 if the observation is from that state, and 0 otherwise
  - Be careful NOT to introduce _all_ of them and the constant
    - R would complain about multi-colinearity. We typically omit ONE of the categories
    - We typically omit ONE group (if we don’t do it, R may do it for us)
  - What is the interpretation of each coefficient?
    - It is the difference between the value of this group and the value for the omitted (reference) group.
      - you want to omit one category yourself and not let your statistical package decide which to omit, as you want to choose a reference that is more intuitive, that makes more sense for it to be a reference
- We can then test if the individual coefficients (of the various dummies associated to the various states) are statistically different

You should use this transformation even for ordinal measures, like set of treatments by ranges of the doses (low doses/ intermediate/ high dose) or satisfation judgments (very satisfied/partially satisfied/very unsatisfied), as introducing the variable itself directly in the regression, then the associated coefficient would not have much sense.

What about "age" ? It's too big to be encoded in a dummy. We will see it later.

### 090105 - More on Categorical Variables ###

**Dummy variables with other variables in the regression**

With other variables in the regression

$Y_i = \alpha + \beta D_i + \gamma X_{i} + \epsilon_i$

- where:
  - $X$ is a matrix of "normal" (non-dummy) regressors, and $\gamma$ is the vector of associated coefficients
  - $D$ is a matrix of dummy variables, and $\beta$ is the vector of associated coefficients
- In that case the $\beta_j$s are the difference in intercept between reference and group j.
- This is the most frequent way that RCT are analyzed:
  - the matrix X represent “control” variables: things that did not affect the assignment (assignment is randomised) but may have been different at baseline, differences left over becasue the sample size is not very big
  - we may present the RTC experiment first without the X and then with the X introduced
  - the assumption is that treatment doesn't change the effect of X, just put you up across every value of X
  - example:  baby aspirin might reduce the chance to have a heart attack, but gender and age also has an influence, as men and people above a certain age are more likely to have a heart attack. But if we assume that the effect is going to be lower for everyone, that would justify this model.
- Also in non-RCT contexts when you want to "control" for other variables, on top of your main dummy variable of interest
  - example: is it the case that women tend to have lower wages than men, conditional that women/men have different levels of education, they major in different fields, etc.. 

  

### 090106, 090107 and 090108 - Interaction terms, their interpretation and difference-in-difference###  

**Dummy variables and Interactions**

There are now two sets of dummy variables, say, Treatment and control, and Male and Female.

$Y_i = \alpha + \beta D_i + \gamma M_i + \delta M_i * D_i + \epsilon_i$

- where:
  - $D$ dummy variable matrix for treatment
  - $M$ dummy variables for controlling groups
  - $M_i * D_i$ is a `1,size(M_i)*size(D_i)` row product of all possible combinations between treatments and control groups ("interaction" terms)
- not "just" a intercept shifter but interacted with the X's
- example1: a model for a treatment that might be randomly assigned, but where you realise that there may be groups within your control or treatment (for example male versus female) that may respond differntly to the same treatment, and you are interested in testing that difference as well.
- example2: $D$ is training program for unemployment people, $Y$ whether or not the person is reemployed after 6 months; $M$ is a dummy for gender (male: 1; female: 0); $M_i * D_i$ is the product between the two set of dummies, so it's going to be 1 if the person is a male and treated, and 0 otherwise (here, with only one dummy in both the sets, it is a single dummy)
- what you're looking at is not just an intercept shifter, but to know whether the treatment has a different effect in one group than another.
- "model with interactions", where  one variable is going to be multiplied by the other


How do we interpret these coefficients:

This is a very popular design called "difference-in-difference of an interaction variable".

```
            | Treatment
            | No:0        | Yes:1
------------|------------------------
Female (0)  | \bar Y_00   | \bar Y_01
Male   (1)  | \bar Y_10   | \bar Y_11

```

- $\hat \alpha$
  - an estimate of mean for women in the control group - $\bar Y_{00}$
- $\hat \beta$
  - an estimate of the difference between the treatment and control group means for women (women is called the "omitted group") - $\bar Y_{01}-\bar Y_{00}$
  - we call this the **treatment main effect**
- $\hat \gamma$
  - an estimate of the difference between Males and Females in the control group, when treatment is equal to 0 - $\bar Y_{10}-\bar Y_{00}$
  - we call this the **gender main effect**
- $\hat \delta$
  - an estimate of the difference between the treatment effect for males and for female - $\bar Y_{11}-\bar Y_{01} - (\bar Y_{10}-\bar Y_{00})$ - the difference between male and female in the treatment minus the same difference but in the control, or, equivalently but perhaps more interesting, $\bar Y_{11}-\bar Y_{10} - (\bar Y_{01}-\bar Y_{00})$, the treatment effect for males minus the treatment effect for females
  - we call this the **interaction effect**
  
- How do you obtain an estimate of the treatment effect for males ?
  - we are looking for $\bar Y_{11} - \bar Y _{10}$, and from the formula of $\hat \delta$ and $\hat \beta$, we obtain that $\bar Y_{11} - \bar Y _{10} = \hat \beta + \hat \delta$ 
- How do you obtain, for example, an estimate of the mean for males ?
  - for the control group, we are looking for $\bar Y_{10}$, and this is simply $\hat \alpha + \hat \gamma$
    - or, from looking at the model equation, this condition is obtained when $D_i=0$ and $M_i * D_i$ is also zero, so it remains $\hat \alpha + \gamma$ 
  - for the treatment group, we are looking for $\bar Y_{11}$ and we just add the treatment effect for males: $\bar Y_{11} = \hat \alpha + \hat \gamma + \hat \beta + \hat \delta$
    - or, from looking at the model equation, this condition is obtained when all the tems are 1, and so it is $\hat \alpha + \hat \beta+ \hat \gamma + \hat \delta$


**Difference-in-Differences**

This is the basic “difference in differences” model which is often used by empirical researchers in a situation where there was a change in the law (or an event) affecting one group but not the other, and you are willing to assume that in the absence of the law, the difference between the two group would have remained stable over time

- In this case you have $D_i = 1$ if post law, 0 otherwise, and $M_i = 1$ if group affected by the law, 0 otherwise.


### 090109 - Examples of Difference in Differences ###

Famous examples:

- Mariel Boatlift experiment (David Card, 1990);
  - Castro let for a few days people leave the island if they wished
  - Did the massive arrival of Cubans in Miami in a few days afected the labor market outcomes of the people
who lived already in Miami before their arrival?
  - He compared Miami with a set of other cities similar in size and other socio-economic characteristics and he run a difference-in-difference analysis
  - $Y$ is the incomes, one dummy set is for native of Miami/native of other cities and the other dummy is pre vs after massive cuban arrival
  - He assumes that, while there are business cycles and things change over time, but in the absence of the Cubans mass arrival, Miami and the other comparison city would have moved roughly in sync
  - The interesting result in this paper is that delta is 0, that is it doesn't seem to have been an effect on the wages of native Miami people given by the amassival arrival of the new people.
  - Questions on its assumptions:
    - is it really true that these other cities would have been on the same trend
  - http://journals.sagepub.com/doi/abs/10.1177/001979399004300205
  - http://www.globaldetroit.com/wp-content/uploads/2014/11/1332199320marielimpactstudy.pdf
    
New Jersey-Pennsylvania experiment (Card and Krueger)
  - They look at minimum wage
  - They compared fast food restaurant workers in New Jersey and Pennsylvania, looking at employment
after an increase in the minimum wage in one of the states.
    - Y is the employment levels, the dummy treatment is the time period before and after the minimum wage increase, and the groups are the two states
      - instead of comparing with just one other state the work could have compared the employment levels with a weithed average of all the other states, weighted in a way that your weighted average looks very much like the state where the treatment is done, so that your $\beta$ looks very much zero - as the state with the tratment and the weighted average of the other states looks very similar (work of Alberto Abadie, "synthetic control group method")
      - a testable implication that the cities are on the same trend is that they were on the same trend before. So it is possible at least look at what happened before.
      
    
### 090110 and 090111 - Evidence from Indonesian School Construction ###

Example : INPRES school construction program in Indonesia

Second five year plan (1974-79):

- A consequence of the oil shock: for most of the world was a negative shock, but it turns out for Indonesia it was a big positive shock because Indonesia is an oil producing country.
- After a huge civil war with milions of people dying
- A large program:
  - 61,807 primary schools constructed from to 1973/74 to 1978/79
  - number of schools multiplied by 2. 1 school for every 500 children.
- A very sharp change in policy: Before 1973, no construction, ban on recruiting for public service positions
  - So that gives us one dimension of differences, which is the cohort that are going to be exposed to this program are the younger cohort: kids who were young enough that they can go to the schools when the program starts. (i.e. those aged 2 to 6 in 1974)
- A program meant to favor low-enrollment regions
  - allocation rule: number of schools constructed in a district was proportional to the number of children (ages 7 to 12) NOT enrolled in primary school
  - they were building more schools in places that had a lower level of education to start with.

  
Data Available:
- SUPAS 95: A survey done in 1995: after the children educated in these schools have completed their schooling, and have started working(they are now adults)
- 150,000 men born 1950-1972
- Variables we have: education, year and region of birth, wages.
  - education and wages are outcome, while year and region of birth is going to be used to construct the exposure to the program.
  - the causal question we are interested in asking is what's the causal effect of building these schools on school enrollment and, also, on wages.

Sources of variation.
Two factors affect the intensity of the program.

- Year of birth
  - you were more exposed to the program if you were younger
    - you were more and more exposed as they were building more schools for the kids who were 11 started being exposed, 10 were more exposed, 9 even more, ... etc., until the most exposed kids are the youngest people in the sample who were two in 1974.
  - in particular, because kids in Indonesia go to school until 12, kids were not exposed at all if they were 12 or older in 1974
  - Cohort of people `aged 2 to 6 in 1974`: the most exposed to the program
  - Cohort of people `aged 12 to 17 in 1974`:not exposed at all to the program
- Region of birth.
  - the government was targeting low enrollment regions and therefore that created substantial variation in program intensity across districts.


Difference in difference

TABLE 3 -- MEANS OF EDUCATION AND LOG(WAGE) BY COHORT AND LEVEL OF PROGRAM CELLS

```
|----------------------|------------------------------|--------------------------------|
|                      |Years of education            |  Log(wages)                    |
|                      |------------------------------|--------------------------------|
|                      |Level of program in           |Level of program in             |
|                      |Region of birth               |Region of birth                 |
|                      |------------------------------|--------------------------------|
|                      | High   | Low    | Difference | High    | Low     | Difference |
|                      | (1)    | (2)    | (3)        | (4)     | (5)     | (6)        |
|----------------------|------------------------------|--------------------------------|
|**Panel A: Experiment of Interest**                                                   |
|Aged 2 to 6 in 1974   |8.49    |9.76    |-1.27       | 6.61    |6.73     |-0.12       |
|                      |(0.043) |(0.037) |(0.057)     |(0.0078) |(0.0064) |(0.010)     |
|Aged 12 to 17 in 1974 |8.02    |9.40    |-1.39       |6.87     |7.02     |-0.15       |
|                      |(0.053) |(0.042) |(0.067)     |(0.0085) |(0.0069) |(0.011)     |
|Difference            |0.47    |0.36    |0.12        |-0.26    |-0.29    |0.026       |
|                      |(0.070) |(0.038) |(0.089)     |(0.011)  |(0.0096) |(0.015)     |
|                                                                                      |
|**Panel B: Control Experiment**                                                       |
|Aged 12 to 17 in 1974 |8.00    |9.41    |-1.41       |6.87     |7.02     |-0.15       |
|                      |(0.054) |(0.042) |(0.078)     |(0.0085) |(0.0069) |(0.011)     |
|Aged 18 to 24 in 1974 |7.70    |9.12    |-1.42       |6.92     |7.08     |-0.16       |
|                      |(0.059) |(0.044) |(0.072)     |(0.0097) |(0.0076) |(0.012)     |
|Difference            |0.30    |0.29    |0.013       |0.056    |0.063    |0.0070      |
|                      |(0.080) |(0.061) |(0.098)     |(0.013)  |(0.010)  |(0.016)     |
|----------------------|------------------------------|--------------------------------|
Note: The sample is made of the individuals who earn a wage. Standard errors are in parentheses
```

Source: Duflo, 2001 ”Schooling and Labor market consequence of school constructions in Indonesia: Evidence from an Unusual Experiment” American economic review.

- https://dx.doi.org/10.1257/aer.91.4.795
- http://www.uh.edu/~adkugler/Duflo.pdf

- Panel A
  - You can see that if we look at the older cohort (12 to 17 in 1974) and you compare the high intensity program
region and low intensity program regions, you can see that the years of education of these old guys is higher in the low program regions (9.40 vs 8.02)
    - That reflects the fact that more schools were built in places where education levels were low.
    - Difference of -1.39 for the low-intensity program regions.
  - If we look at the younger guys (aged 2 to 6 in 1974), it is still the case that their education level is lower when they are from the high program region, but of course it's because it reflects the heterogeneity of the regions.
    - What we can see is that over time the differences become lower (-1.27 compared to -1.39)
  - So under the assumption, which is the critical difference and different assumption that in the absence of the program, the difference between the high program intensity region and low program region would have remained the same, then I can take the difference in these two differences and I can attribute that to the causal effect of the schools.
    - that gives me a difference in difference estimates of 0.12 years of education: being in a high intensity program versus a low one, which corresponds to about a couple of schools per thousand kids, had increases the years of education by 0.12.
  - We can do the same analysis for wages and see there is a 2.6% increase in wages being in a high-level of program region
  
- Panel B  
  - How do we know that this assumption of parallel trend, this assumption that the difference would have remained the same, is satisfied ?
  - We will never know, but we can at least get some idea by looking at what happened before: is it the case that, before the program started, these places were effectivelly on a parallel trend ?
  - You can see that if we compare now the old guys (12 to 17 in 1974) to some very old guys (18 to 24 in 1974), there the difference seemed to be relatively constant over time (very slight convergence between regions)



We didn't need any regression up to now. Let's now try and set it up as a regression instead.

The same set-up can be transformed in a regression as :

$Y_{i} = \alpha + \beta K_i + \gamma R_i + \delta K_i * R_i + \epsilon_i$

where:

  - $Y_i$ is the level of education for the guy $i$
  - $\alpha$ is the constant
  - $K_i$ is a dummy for kind of cohort the guy is (0: old [12-17 in 1974], 1: young [2-6 in 1974])
  - $R_i$ dummy for intensity of the program kind region (0: low intensity region, 1:high intensity region)
  - $K_i * R_i$ is the interaction term
  
  - $\alpha$ is the degree of education for the old people in the low program region -  `9.40` in the table
  - $\beta$ is the difference between young and old in region that get low program - `0.36` in the table
  - $\gamma$ is the difference between the high and low program region among the old guys - `-1.39` in the table
  - $\delta$ is the difference in difference, the number of interest - `0.12` in the table
  
  
So we can go from the regression of a  difference in difference to the table form and the other way around, so when you have the table, you can fill up the regression, and when you have the regression, you can fill up the table.

More generally: Interactions

More generally, the coefficient on the interaction between dummy variable and some variable X (that could also be a dummy variable, but not necessarily) tells us the extent to which the dummy variable changes the regression coefficient for that variable.

$Y_i = \alpha + \beta^* D_i + \gamma X_{1i} + \delta^* D_i X_{1i} + ··· + \epsilon_i$


Where:

 - $\beta$ is main effect that tells you by how much the dummy variable shifts the level
 - $X$ is an interactive variable, and if it's a continuous variable it's coefficient $\gamma$ is giving you a slope for the group from which the dummy is 0.
 - $delta^* D_i X_{1i}$ is an interaction term between the two, which is going to tell you by how much the slope is shifted for the group for which the dummy is turned on.
 
### 090112 - More on Evidence from Indonesia ###

INPRES example: use variation across cohorts

Before we arbitrary divided regions in high-program and low-program regions. Now we can instead use the number of schools bilt in each region, so that it is a continuous variable instead of a dummy.

The model in 090111 become hence

$Y_{i} = \alpha + \beta K_i + \gamma P_i + \delta K_i * P_i + \epsilon_i$

where:

  - $Y_i$ is the level of education for the guy $i$
  - $\alpha$ is the constant
  - $K_i$ is a dummy for kind of cohort the guy is (0: old [12-17 in 1974], 1: young [2-6 in 1974])
  - $P_i$ number of schools bilt the region where the guy comes from
  - $K_i * P_i$ is the interaction term
  - $\delta$ it tells the difference in the slope before the program and after, i.e. the effect of the school


In a further set-up, instead of just dividing the cohort of ex-students in young and old, we set also a dummy for each year of birth (less one dummy).
And then instead of only controlling linearly for the number of schools, putting the number of school built, we put a bunch of region of birth dummies, again omitting one.


$S_{ijk} = c_1 + \alpha_{1j} + \beta_{1k} + (P_j * T_i )\gamma_1 + \epsilon_{ijk}$

where:

- $S_{ijk}$ is the education of individual $i$ born in region $j$ in year $k$,
- $T_i$ is a dummy indicating whether the individual belongs to the “young” cohort in the subsample,
- $P_j$ denotes the intensity of the program in the region of birth (number of school built)
- $c_1$ is a constant,
- $\beta_{1k}$ is a set of cohort-of-birth fixed effects [in practice, a series of dummies=1 for each year of birth, omit 1] - a synthetic form for both the vector of coefficients and the associated dummies
  - it isn't really important which one is the dummy we omit. Traditionally, we omit the oldest guys, so everybody's compared to the oldest guys.
- $\alpha_{1j}$ is a set of district-of-birth fixed effects [in practice, a series of dummies=1 for each district of birth, omit 1] - a synthetic form for both the vector of coefficients and the associated dummies
  - it isn't really important which one is the dummy we omit
  
Although we have lots of region of birth dummy, and lots of year of birth dummy, we only have one interaction coefficient, which is the one that happens to be we are interested in: number of schools built in the region of birth, interacted with whether the guy is young or old.


A linear regression representation of a difference-in-difference model is one where there is one interaction term between two variables that are both categorical.

This is hence no longer a difference-indifference specification but rather a "fixed effect regression".
The bunch of dummies for the region of birth are called region of birth fixed effect, as are systematic differences that don't vary over time between regions (betwee the young and old cohort).

Same for the year of birth fixed effects.  Year-of-birth fixed effects are introduced to control for inherent differences in education levels across time.  Year of birth fixed effects are a set of dummy variables indicating the year-of-birth (one dummy for each year-of-birth (yob)). The coefficient on a given yob dummy is just the mean education (in years) for children born in that year. These yob dummies are included to separate the effect of school building from the effect of being young. Since the young are more educated anyway, if the old to the young are compared without controlling for their yob, the estimator would conflate the actual effect of school building on education with the trend of increasing education levels across time. 

Fixed effects can be accounted for using a set of 0/1 dummy variables for the categorical variable of interest.

For each region is hence possible to compute the education of the young guys minus the educaton of the old ones and regress it on the number of schools build in the region of birth. We obtain the following charts:

Figure 1: Regional growth in education and log wages accross cohort and program intensity (Per capita denotes per 1000 children)

- Figure A1: Experiment of interest: education
  - Y: Educ.of young cohort−Educ. of old cohort
  - X: Number of INPRES schools per capita
  --> growing trend

- Figure A2: Experiment of interest: log(wages)
  - Y: Log(wages) of young cohort−Log(wages) of old cohort
  - X: Number of INPRES schools per capita
  --> growing trend

- Figure B1: Control experiment: education
  - Y: Educ. of old cohort−Educ. of very old cohort
  - X: Number of INPRES schools per capita
  --> very slowing growing trend
  
- Figure B2: Control experiment: log(wages)
  - Y: Log(wages) of old cohort−Log(wages) of very old cohort
  - X: Number of INPRES schools per capita
  --> stable trend
 
 
TABLE 4 -- EFFECT OF THE PROGRAM ON EDUCATION AND WAGES: COEFFICIENTS OF THE INTERACTIONS BETWEEN COHORT DUMMIES AND THE NUMBER OF SCHOOLS CONSTRUCTED PER 1000 CHILDREN IN THE REGION OF BIRTH.

```
|------------------------------|-------------|-----------------------------|--------------------------------|
|                                            | Dependent variable                                           |
|                                            |-----------------------------|--------------------------------|               
|                                            |Years of education           |  Log(hourly wage)              |
|                              |-------------|-----------------------------|--------------------------------|
|                              |Observations | (1)     | (2)     | (3)     | (4)      | (5)      | (6)      |
|------------------------------|-------------|---------|---------|---------|----------|----------|----------|
|**PANEL A: Experiment of Interest: Individuals Aged 2 to 6 or 12 to 17 in 1974**                           |
|**(Youngest Cohort: Individuals Ages |2 to 6 in 1974)**                                                    |
|Whole Sample                  |78,470       |0.124    |0.15     |0.188    |          |          |          |
|                              |             |(0.0250) |(0.0260) |(0.0289) |          |          |          |
|Sample of wage earners        |31,061       |0.196    |0.199    |0.259    |0.0147    |0.0172    |0.0270    |
|                              |             |(0.0424) |(0.0429) |(0.0499) |(0.00729) |(0.00737) |(0.00850) |
|                                                                                                           |
|**PANEL B: Control Experiment : Individuals Aged 12 to 24 in 1974**                                        |
|**(Youngest Cohort: Individuals Ages 12 to 17 in 1974)**                                                   |
|Whole Sample                  |78,488       |0.0093   |0.0176   |0.0075   |          |          |          |
|                              |             |(0.0260) |(0.0271) |(0.0297) |          |          |          |
|Sample of wage earners        |30,225       |0.012    |0.024    |0.079    |0.0031    |0.00399   |0.0144    |
|                              |             |(0.0474) |(0.0481) |(0.0555) |(0.00798) |(0.00809) |(0.00915) |
| Control variables:                                                                                        |
|Year of birth* enrollment rate in 1971      | No      | Yes     | Yes     | No       | Yes      | Yes      |
|Year of birth* water and sanitation program | No      | No      | Yes     | No       | No       | Yes      |
|------------------------------|-------------|-----------------------------|--------------------------------|
 
```

Notes: All specifications include region of birth, year of birth dummies and interactions between the year of birth dummies and the number of children in the region of birth (in 1971). The numbers of observations refer to the specification in columns 1 and 4. Standard errors are in parentheses.

The coefficient $\gamma$ tells us that the difference in education between the young cohort and the old cohort is 0.124 year larger for each school built per 1000 kids.

As a further sophistication, one could also separate the cohort in the interaction term not just between young and old, but between all the cohorts, the people who are 23, the people who are 22, the people who are 21, et cetera.
And run this regression always comparing cohort by cohort.
In that case, we should see no effect until people are young enough,  so we're going to get a bunch of 0's for the older cohort, and then start positive numbers as the people are young enough to go into the schools.

The other columns in the table is because they had another program that they started roughly at the same time and that we was worried would also affect education potentially.
And that also was worried, could have been in the same regions. So column 3 controls for the water and sanitation programs.

As it turns out, the coefficient becomes a little bit higher for each control you add. So if anything, without introducing this control, we're slightly underestimate the effect of the reform.


### 090113 - Alternative Functional Forms & Fixed Effects ###

Other functional form issues

- Transforming the dependent variable
- Non linear transformations of the independent variables

Sometimes the economic theory tell you that the model of interest is not at all linear. Still, sometimes it is transformable to a linear model.


Transformations of the dependent variable

- Suppose $Y_i = A X_{1i}^{\beta_1} X_{2i}^{\beta_2} \epsilon_i$
  - then run linear regression $log(Y_i) = \beta_0 + \beta_1 log X_{1i} + \beta_2 log X_{12} + \epsilon_i$  to estimate $\beta_1$ and $\beta_2$
  - "log-log" relationship
  - note that $\beta_1$ and $\beta_2$ are elasticities: when $X_1$ changes by 1%, $Y$ changes by $\beta_1$ %.
  - example: a Cobb-Douglass production function can be transformed to a linear model just taking the log:
$Y=K^\alpha L^{1-\alpha} ~~ => ~~ log(Y) = \alpha log(K) + (1-\alpha) log(L)$
  -  the log-log formulation is very popular because of that elasticity interpretation. And it's unit free, so it's very convenient.
  - an other widely used relationship is the log-linear: the log of the variable is regressed on a linear version of a regressor
    - example: returns to education formulation $logY_i = \beta_0 + \beta_1 S_i + \epsilon_i$
    - when education increases by 1 year, wages increase by $\beta_1 %$.
    - it represents very well some things (like the effect of education)
  - There could be also mixed forms, where the outcome is in log, some regressors are in log form and some others are not
    - when the outcome is in logs and the regressor is in logs, the coefficients represent elasticities: the coefficients measure the % change in the outcome as a result of a 1% change in the regressor.
    - if the outcome is in logs, but the regressor is not, the coefficient represents the % in the outcome resulting from a unit increase in the regressor. 
    
- Suppose $Y_i = \frac{1}{\beta_0 +\beta_1 X_{1i}+\beta_2 X_{2i} + \epsilon_i}$
  - then run regression $\frac{1}{Y_i}=\beta_0 +\beta_1 X_{1i}+\beta_2 X_{2i} + \epsilon_i$
  - "Box Cox Transformation" or 
  
- Suppose $P_i = \frac{e^{\beta_0 +\beta_1 X_{1i}+\beta_2 X_{2i} + \epsilon_i}}{1 + e^{\beta_0 +\beta_1 X_{1i}+\beta_2 X_{2i} + \epsilon_i}}$
  - $P_i$ is the percentage of individuals choosing a particular option (e.g. buying a particular car over a list)
  - $X_1, X_2$ are the attributes the choice depends from 
  - then run regression: $Y_i = log \left(\frac{P_i}{1-P_i} \right) = \beta_0 +\beta_1 X_{1i}+\beta_2 X_{2i} + \epsilon_i$
  - "Discrete choice model" or "Extreme value formulation"
  - It happens to fall off quite naturally from choice models. And it also, again, seems to often correspond to reality quite nicely.
  
  
## 0902 -  Omitted Variable Bias ##

### 090201 - Non-linear transformations of Independent Variables ###
  
  