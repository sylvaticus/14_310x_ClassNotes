---
title: 14_310x - Module_10 - Into to Machine Learning and Data Visualisation
author: "Antonello Lobianco"
date: ""
output:
  html_document: default
  pdf_document: default
---


# 14_310x - Module_10 - Into to Machine Learning and Data Visualisation #

##  1001 -  Machine Learning I  ##

##  1002 -  Machine Learning II  ##

### 01 - understanding ordinary least squares ###

c. Assuming a specific data generating process, if we engage in the OLS procedure, then on average across many draws, beta hat will be equal to the true beta in the population.

When we say $\hat beta$ is unbiased, we are making a mathematical statement equivalent to the statement in C. Note that, unbiasedness is not the same as efficiency. An estimator could have 0 variance, and still be very biased. So B and D are incorrect. The statement in A is in fact true, we know that the OLS estimator minimizes the mean squared error, which is a function of both the bias and the variance. However, it is not what we mean when we say beta hat is unbiased. 

One might be willing to sacrifice unbiasedness for less variance in a predictive model. This bias-variance trade-off is also a common tradeoff when dealing with estimation, however prediction models typically are more lax in terms of valuing unbiasedness and interpretability.

### 02 - decision trees ###
you can always construct a decision tree to fit your data perfectly, because you can always just create more and more partitions until you have one observation in each node. In this case, your model will completely overfit, and will perform very poorly out of sample. So although you want to fit the data, you need to be careful not to over fit. Intuitively, if you perfectly fit the data, you are just repeating the data, so you will be able to make virtually no predictions. 

### 03 - Pruning decision trees ###
In the OLS example, it says, if the problem
we had was we had 2000 data points and 20,000 variables,
then the problem is we're allowing OLS
to use too many coefficients.

But instead of being allowed to use 20,000,
you're allowed to use 20.
Find the 20 best variables in this dataset that
allows you to fit this data.

So that's the first part of machine learning.

Increasing the lower bound of points per leaf prevents you from putting one or two points in every leaf and exactly overfitting. Restricting the number of leafs in at tree as well helps in creating too many splits and creating a model that only works in your sample. Restricting the upper bound of points per leaf if anything would encourage overfitting since it would lead to more splits in a decision tree instead of allowing for generality in other datasets. Increasing the number of data points for which you are training with will not regularize you tree since your tree will then over fit again in the new data.

Limiting the number of variables allowed in your regression is a way of restricting the complexity of your model. As Prof. Mullainathan explained this will impose a constraint on your maximizer, and therefore help avoid over fitting. Note, however, that restricting the complexity of your model will increases the bias of your estimator (recall the bias-variance tradeoff). Changing the number of data points in your regression will not affect the overfitting problem in your functions because they will just keep overfitting any new data you give them. As a side note, the second method is exactly what LASSO (least absolute shrinkage and selection operators) regressions do in order to increase the predictive ability of your coefficients.

the complexity of your model (C), is decided by your algorithm. In practice, you run your model with different levels of complexity, and you use a separate dataset to choose the level of complexity that maximizes the accuracy of your predictions. 


### 04 - Overfitting and the Tuning Problem ###

Human data mining has the following feature--
once I've seen a data set, I can't forget that I've seen it.
An algorithm can perfectly be trained here, tuned here,
and then retrained again and completely forget
everything it has seen.

Traditional cross validation criterion compare your predicted values against the observed data, in order to assess models’ ability to fit your data. So they are precisely a measure of in-sample fit. In prediction problems, you are concerned with out-of-sample fit. If we used the cross-validation criterion on the full dataset to choose the complexity of your model, it will always favor higher complexity, since that will improve your in-sample fit.

Machine learning works by splitting the sample into a “tuning set” and a “training set”. Once you fit your model on the training set, cross validation criteria are used to assess the fit of your model on the set of data which were not used to fit it, i.e the “tuning set”. 


The higher the complexity of your model, the stronger the signal it will provide you. However, the probability of overfitting also increases as your model’s complexity increases. So A and C are incorrect. D and B are equivalent: the more your model overfits in-sample, the worse it will be at performing out-of-sample predictions. Recall what happens as you increase the number of nodes in your tree to perfectly fit the data: you can fit the data perfectly, but that will be useless for making out-of-sample predictions. 

### 05 - Observability of Prediction ###

More data has a huge first order effect
because it allows you to fit a more complex model.
And if you're fitting a more complex model, what's
gonna happen?
You're gonna predict better.

I come from a world of estimation
where standard errors go as one over the square root of n.
And for most coefficients, for most things,
I think there's diminishing returns in getting data.
So I would never have said, hm, we're not
finding an effect with 100,000.
What I really need is another 100,000 data points.

Increasing the sample size implies that it will take more splits to fit the data perfectly. So intuitively, you can “split” the tree more times to improve both your in-sample and out-of-sample fits, before you start overfitting the data. (Note: it takes more splits to overfit the data if you have more data).

The important point to note here is that sample size matters a lot in machine learning. As you can see, increasing the sample size allows you to fit more complex models without sacrificing your prediction error both in your training and tuning datasets. As Prof. Mullainathan mentioned, assuming you have a reasonably complex model, the quality of your predictions can really only be improved by adding more observations. Because then you can fit a more complex model, without the overfitting prediction error dominates.

So the quality of your algorithm can only take you so far, your ability to predict is bounded by your number of observations. 

### 06 - Econometric and machine learning ###
how to split your data between test and training data is usually decided arbitrarily. However, one could apply the “power calcs” framework to take a more structured approach in making this decision.

Cross validation (or this type of tuning) is used to find the complexity that does best out of sample. It is not used to find the model that does best out of sample. This is precisely how machine learning solves “the curse of dimensionality”. We reduce the problem of selecting the best model (which usually is very high dimensional, if we have a large number of features) to a very low-dimensional problem to find the complexity that does best out of sample.

Here’s an example to illustrate these concepts:

Suppose you have data on amazon book sales, and 100 variables that contain different information about the book, and all the corresponding details of the book’s page on amazon over time. You are interested in predicting the demand for books, but don’t have any theory of the determinants.

You want to fit a regression tree model to this data. To this goal, you load your data into R. Now, you don’t know how deep that tree should be (how many splits it should have). Following the tuning procedure, you tell R to fit the regression tree of depth d that best fits your training data for each possible number of splits x ranging over some sufficiently wide range.

R automatically computes measures of fit (R squared is the default) for each of these models. Now, this graph would be obtained by plotting your measure of fit resulting from fitting a regression tree model with x splits. So each of these models might weight different features differently, and may or may not included the same feature set. But you have now reduced the problem of selecting the best model among all possible models ($2^d$ possible models), to just finding the optimal complexity (the optimal number of splits), which is just 1 dimensional. At this point, we use cross-validation to choose the level of complexity that does best out of sample, not to select the model. 

### 07 - Estimation, Prediction, and Things You Can See ###

Both prediction and estimation are observable, in the sense that if we acquire new data we can literally see (and measure) how well our model predicts. However, the difference is that the machine learning framework tuning relies on the observability of the model’s out-of-sample ability to predict in order to estimate the model that corresponds to the complexity level that generates the best prediction. On the other hand, usually in estimation, we don’t have an out-of-sample measure of the model’s prediction (although we could), so we don’t “observe” it in that sense. However, we could, i.e. there’s no reason we can’t see how well a model that resulted from an estimation predicts a certain outcome if we had data. (Note: the reason this is not usually done is because the type of data we usually need to answer meaningful causal questions tends to be hard to get or expensive to collect.) 

### 08 - Public policy and machine learning ###

In the early 90s, an enormous number of papers were published that try to explain differences in GDP across countries. They mostly vary in terms of the variables they include, each with the claim that they found the set of variables that determine GDP.

One could imagine that the authors of these papers had 2 goals:

        They want to understand why some countries grow while others don’t.
        They want to know which countries will stay poor.

Which would be a more useful tool for these researchers? Both

The researchers would want to use an estimation framework to understand why some countries grow and others don’t (A), because they want to interpret their estimates. However, it would probably make more sense to use machine learning to predict which countries are likely to stay poor (B). Since the goal is more about predicting GDP rather than understanding it’s determinants, it doesn’t really matter to them what goes into the “black-box”.

Like in the example of prices and consumer purchase decisions example Prof. Mullainathan used in class, comparing the out-of-sample prediction error of both models doesn’t tell you anything about what the coefficient on human capital measures should be. But, if you find that including measures of human capital in your feature set leads to much better predictions relative to if you exclude them, then that tells you that human capital is pretty important for explaining why countries grow.

If your model does just as well whether or not you include human capital from your feature set, then that tells you it doesn’t really matter. 

### 09 - Improving Hypothesis Testing ###

How do I convert greenness or yellowness
into what I really want-- agricultural yield?
For that I would relate the visual features I see to yield.
So that means I would take my satellite data,
I would have some real data on yield,
and then I would turn that into a prediction problem.
What features of the satellite images, after I process them,
relate to yield?
All of this allows me to take this huge data and chunk it
through and turn it into something meaningful.
And then that meaningful thing could go into an experiment.

The pre-processing of data with machine learning, in this case, involves being able to extract the features that you want from data, or in other words, getting sentiment from tweets. The processing of data as described by Prof. Mullainathan would be actually performing the learning in the economically meaningful units you want to analyze, in this case number or intensity of protests. For measuring the effects of protests on political instability, machine learning would not have that much to say. Web crawling is the gathering of data and although this could be pre-processing, web crawling itself does not include machine learning. 

### 10 - New Data and Machine Learning ###

For any type of data, there is always the risk that you are not measuring what you think you are measuring. You could have a great algorithm that does very well at predicting a variable that is intended to be a proxy for something else, and that could turn out to be a better measure than survey data or other forms. There are also things you can’t measure directly through survey data or other more traditional data collection methods. For example, if you are interested in summarizing features of language or speech, or features of satellite data, you would not be able to do this through traditional data collect methods. So prediction can be used to generate measures by turning the measurement problem into a prediction problem of predicting a particular meaningful feature based on a lot of other features.

### 11 - Prediction in Policy ###

there are decisions
that we have in life that are dependent on predictions, not
on causal unknowns.

Sometimes the decisions that we need to make are dependent on prediction, not on causal unknowns. Another example is the decisions of admissions committee to admit a given student, which depends on whether the student would do well or not. If there were an algorithm to predict students’ success (measured in whatever way, possibly using machine learning to pre-process available data), that could help reduce the number of students who end up struggling. 






##  1003 -  Data Visualisation  ##

