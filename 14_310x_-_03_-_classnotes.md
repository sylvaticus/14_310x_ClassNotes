---
title: "14_310x - Module_03 - Describing Data, Joint and Conditional Distributions of Random Variables "
author: "Antonello Lobianco"
date: "25 September 2018"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```


# 14_310x - Module_03 - Describing Data, Joint and Conditional Distributions of Random Variables #

## Lecture 0301 -  Summarizing and Describing Data  ##

### 030101 - An Overview: Exploratory Data Analysis ###

Data visualisation:

1) Visualising for yourself: "exploratory data analysis"
2) visualising it for others: convey a story

This lesson the focus is in (1), exploratory data anlysis, so today lesson doesn't cover principles about how to represent data for others.

Index:

- plotting histograms
- plotting kernel density
- comparing two or more distributions
- plotting estimates of CDFs
- bivariate distributions

Not yet this lesson:

- relation between variables (find patterns)

### 030102 - The Histogram ###
- A histogram is a rough estimate of the probability distribution function of a continuous variable.
- It's a function that count the number of observations that fits into each "bin".
- Let n be the total number of obs. and k the number of bins, the histogram then meets the definition:
  - $n = \sum_{i=1}^{k}m_i$
  - You can divide by the total number of obs. to obtain the density: the proportion of cases that fit within each bin.
- Free book: Hadley Wickam, "R for data science"
- ggplot2:
  - the data you want to represent
  - the geometry
  - an aesthetic - what is has been represent

### 030103 - Plotting Histograms in R: Part One ###
- Use set working directory to your own before starting working.
- Make one directory for data ("data") and one for the charts ("graph").
- Right format ("tidy data"): one observation per person, with multiple variables on the same line

### 030104 - Plotting Histograms in R: Part Two ###
```
ggplot(mydata,aes(height_cm)
 +geom_histogram(fill="blue", colour="darkblue")
 +xlab("Height in cm.")
 )
 ```
- To remove outliers, you can either filter your dataset or specify a range in the graph.
- Subsets of your original data do not duplicate space in memory, they are just references to the original data.

### 030105 - Plotting Histograms in R: Part Three ###

- cowplot: you can put graphs in a grid
- which size of the bins ? if the bins are too large, the graph would not be informative (limit: just one class), with bins too narrowed, there may be no statistical significance
  - No real science for determining the size of a bin in a histogram (while there is for plotting a kernel)
  - The choice between the size of the bin is always a choice between how much information I want to try to convey from my underlying data versus how much noise I'm willing to tolerate.
  - So the trade-off in the size of the bin is always between bias and variance.
  - The number of bins that the data will tolerate is going to depend on the number of observations that you have.
- Doing an operation in R (or other software) as compared with doing it in Excel is that you have a track record of what yo uhave done.
- Also second advantage is that you are less likely to make a mistake.

### 030106 - Intro to Kernel Density Estimation ###

The kernel density estimation is, in a sense, a smoothed histogram.

- In order to do the histogram, we take in a particular interval, we basically stack up little vertical bins for each observation, proportional to the number of observations that we have.
- For the kernel, we do the same thing, except that, instead of stacking little rectangular bars, we sum up the result from a kernel function.
- Any function that is symmetric, integrate to 1 and centered to the observed point (e.g. a normal, Epanechnikov, uniform, triangle..) can be used as a "kernel".

Epanechnikov is kind of more of a rounder bell.

3.53

bandwidth: the width of the band, like the length of the bin for an histogram

So,

1) I arrange my observations on the x axis
2) I draw around each of my obs a kernel function (they will overlap across most of the x axis)
3) I discretise the x axis in a number of bins of a choosen width
4) For each bins I sum up the height of the kernels for each points (the x values, not the observations) that fall within the bin
5) You draw the height value that you got at the middle of your bin


- kernel density estimation is a non-parametric way to estimate the probability density function of a r.v.
- It is a straightforward extension of the histogram: at each point we take a weighted average of the frequency of the observations.
- Formally:
  - let (x1,x2,...,xn) be an i.i.d. sample drawn fro msome distribution with an unknown PDF f.
  - We are interested in estimating the shape of this function f
  - its kernel density function is given by $\hat f_h(x) = \frac{1}{n} sum_{i=1}^{n} K_h(x-x_i) = \frac{1}{nh}\sum_{i=1}^n K \left( \frac{x-x_i}{h} \right)$
  - where K() is the _kernel_, a non-negative function that integrates to one and has mean zero, and $h > 0$ is the bandwidth
  - there are many choises for K(), but they are typically bell-shaped (a normal, Epanechnikov, cosine, triangle, even uniform).

See also here: https://www.youtube.com/watch?v=gPWsDh59zdo or the advanced pdf in the folder.

### 030107 - Kernel Density Estimation in R ###

```
ggplot(mydata, aes(myVarIAmInterestedIn)) +
      geom_histogram(data=myData, aes(myVarIAmInterestedIn, ..density..), fill="white", color="darkred")  +       geom_density(kernel="gaussian", aes(myVarIAmInterestedIn))
```
      
(The plus sign has to be in the same row)

- By default the histogram displays count data, we have to specify we want show a density (frequence) also for the histogram
- The most important things are the choice of the kernel function and the bandwith

### 030108 - Bandwidth in Kernel Functions ###

The paramemter bandwith is `bw`, e.g. `geom_density(kernel="gaussian", aes(height_cm), bw=2)`

- At the boundary, there is bias in the kernel estimation because you are going-- you have only half of the data. Because when you are trying to do your bandwidth for something that is the boundary, it doesn't see any data to the left, so it's only summing up data to the right, so it will tend to go too high.
- That is something that is true with Kernels.
- It's not a big deal because in the case of density estimation because we know to be careful, but that that's become very apparent when we have a very large bandwidth.
- The bigger the bandwidth is the flatter and smoother the kernel distribution is.
- Too small bandwith: under smoothing effect: the kernel distribution it's really trying to go to every nook and cranny of the distribution.
  - And intuitively, we feel that represents the particularity of my one sample, as opposed to the underlying distribution that I'm trying to estimate from the data.
- Too large bandwitch: over smoothing effect. There seems to be missing something about the distribution that is real, that is not noise.
- What this means is that there is a trade-off between bias and variance: 
  - the *bias* is the difference between what you're estimating and a true underlying shape of the distribution, i.e. the difference between the model’s expected predictions and the correct values. 
Error due to bias is caused by false, simplified assumptions in the model that do not understand the dataset’s complexity, which leads to underfitting. This dilemma is referred to as the bias-variance tradeoff.
  - the *variance* is the difference between the true underlying shape of the distribution of the data generating process and what happens in reality, because some noise got thrown, or got added by life (beacause I don't have the whole population but just a partial sample).
- Error due to variance is caused from being too sensitive to fluctuations in a dataset; it can cause additional noise and may fail to model accurate future observations (i.e., overfitting).

- The kernel and bandwitch choice can be obtained trying to minimize the mean square errors (the difference between your estimated function minus the true squared), which is a combination of bias and variance.

- Mean square error is itself an estimated object. So we're trying to minimize an estimated object.
- R computes automatically an optimal bandwidth.

### 030109 - Comparing Distributions: Part One ###

- With large enough n, the binomal distribution started to have the shape of a Normal distribution.
- Indeed the binomial distribution B(n, p) is approximately normal with mean np and variance np(1 − p) for large n and for p not too close to zero or one.
- Many random variables can be conveniently assumed to have a normal distribution.
- Height (in a particular population) is a canonical example in textbooks, but why should it be normally distributed? or not?
- When we define the normal distribution more formally, and discuss where it comes from, we will discuss why heights should or should not be normally distributed.
- We need first to look at the Central Limit Theorem

### 030110 - Comparing Distributions: Part Two ###

- Densities are sometimes preferable to counts - counts depends on the smaple size, difficult to make comparation between samples of different size.
- If we're going to use data that comes from two different data sets from-- in one single plot, we need to tell it every time what is the data we're using.
- So instead of putting the data in the arguments of the plot call, we're going to put it inside the geometry call.
- We need to put it for each of the geom density.

### 030111 - Plotting Cumulative Distribution Functions ###

CDF are often preferred when comparing distributions to the densities, but depends what you want to stady: the shape is much more visible with a PDf than with a CDF.

```
ggplot(myData, aes(myVarIAmInterestedIn)) +
     stat_ecdf(data=myData, aes(myVarIAmInterestedIn), color="darkblue")+
     stat_ecdf(data=myData, aes(anOtherVarIAmInterestedIn), color="darkred")+ 
```

First order stochastic dominance of distribution X over distribution Y: for any possible value k, P(X<k) is equal or lower than P(Y<k).

If I need to compare how many items are below k in the two distribution, it is less intuitive to compare areas as in the PDF than compare heights as in the CDF (also for computing rations between these two values).

##  Lecture 0302 -  Joint, Marginal, and Conditional Distributions  ##

### 030201 and 030202  - Joint PDFs ###

- "Support" of a distribution -> the multidimensional region (e.g. the xy-plane in 2d) over which there is a positive probability. 
- To integrate over a joint distribution one has first to find its "boundaries".
  - ex. f_XY(x,y) = cx^2y for x^2 <= y <= 1, 0 oth.  ("a smile" if drawn on the xy plane)
    - The integration boundaries in this case are:
    - [-1,1] for the x dimension
    - [x^2,1] for the y dimension
- The integral over these boundaries must be set equal to 1:
  - $\int_{-1}^{1}\int_{x^2}^{1} c x^ 2 y dy dx = 1$
- You first integrate the first "inner" integral and then the outer one:
  -  $\int_{-1}^{1}\int_{x^2}^{1} c x^ 2 y dy dx = c \int_{-1}^{1} x^2 \left[ \frac{y^2}{2} \right]_{x^2}^1 dx = c \int_{-1}^{1} \frac{x^2}{2} dx - c \int_{-1}^{1} \frac{x^6}{2} dx = \frac{c}{2} \left[ \frac{x^3}{3} - \frac{x^7}{7} \right]_{-1}^1 = \frac{4c}{21}$
- From which you find c, the "normalisation constant" must be equal to 1.


### 030203 and 030204 - Marginal Distributions ###

- The marginal distribution is flatting of the joint distribution for one particular dimension, i.e. for any particular value (for a discrete case) is the sum of all the joints distributions for that value:
  - Discrete: $f_X(x) = \sum_{Y} f_{XY}(x,y)$
  - Continuous: $f_X(x) = \int_{Y} f_{XY}(x,y) dy$
- If the joint PF is symmetric, f_X(x) = f_Y(y)
- An "Indicator function" is nothing else than a function defined on a set X that indicates membership of an element in a subset A of X, having the value 1 for all elements of A and the value 0 for all elements of X not in A ([source](https://en.wikipedia.org/wiki/Indicator_function)).
  - So if our sample space S is for example {1,2,3,4} and we define an event A as the odd integers, the indicator function for A is $I_A : \{1=>1; 2=>0; 3=>1; 4=>0\}$


### 030205 - Independence of Random Variables ###

- While you can always goes from the joint distribution to the manrginal ones, in general the reverse is not true: yo ucan't go from the marginal distributions to the joint one: you miss the information on how the two variables relate together.
the joint distribution contains the informations on how the variables are distribuited on their own but also how do they relate to each other.
- We already see that for two individual events A and B, the two events are independent iff P(AB) = P(A)*P(B).
- Two r.v. X and Y are independent if for all pairs of (x,y) the event {X=x} and {Y=y} are independent:
  - P(X=x and Y=y ) = P(X=x) * P(Y=y)
- or equivalently:
  - f_XY(x,y) = f_X(x) * f_Y(y)  (the joint PDF factors as the two PDF's - same with PMF)
- This implies that also the cumulative is F_XY(x,y) = F_X(x) * F_Y(y)
- If the joint can be factored out as two functions of the x and y alone (any, not necessarily the PDF's) X and Y are independent:
  - f_XY(x,y) = g(x) * h(y)
- But attenction to the support!!! If the support of e.g. y depends on x even if you can factor out the joint the two r.v. are not independent !

### 030206 - Examples of Independent Random Variables ###

- Taking two tablets whose effectiveness period is a joint is an exponential in the form  e^(x+y) is actually a Poisson process where x and y are independent.
- Independance for discrete r.v.: for 2 discrete r.v. we have independance iff the joint table is linearly dependant (rows -or columns- are proportional to one other: every row (column) has to be a multiple of the other row (column) )
- So, for example, for column 1 and column 2 the total column probabilities may be different, but the way that "mass" of probability is disctibuited within the column (across the rows) is the same for column 1 and 2 (and all the other columns).


### 030207 - Conditional distribution ###

@see also: 020108 - Conditional Probability, 020110 - Bayes' Theorem

- The conditional distribution allows us to update the distribution of a random variable, if necessary, given relevant information.
- Conditional PDF of Y given X is:
  - $f_{Y|X}(y|x) = \frac{f_{XY}(x,y)}{f_X(x)}$
- Conditional PDFs are a function of both X and Y, but for a particular value of the conditioning variable, though, they behave just like a marginal PDF.
- The formula implies that it can be computed from the joint PDF, taking the relevant slice of the joint PDF and blow it up so that it is a PDF itself (i.e. it must integrate to 1). We do it dividing the joint for the PDf of the specific X.

### 030208 - Examples of Conditional Distributions ###

- A PDF conditional to X is defined only for non-zero values of f(x)
- f_Y{y} = f_{Y|X}(y|x) iff X,Y are independent
- If two r.v. are independent, knowing something about the realisation of one doesn't tell yo uanything about the distribution of the other.