---
title: 14_310x - Module_07 - Causality, Analyzing Randomized Experiments, & Nonparametric Regression 
author: "Antonello Lobianco"
date: "27 October 2018"
output:
  html_document: default
  pdf_document: default
---


# 14_310x - Module_07 - Causality, Analyzing Randomized Experiments, & Nonparametric Regression  #

##  0701 -  Causality ##

###  070101 - An Introduction to Causality ###

What is Causality?

Causal statements we make in everyday life:

- Her headache got better because she took a pill
- She got a good job because she went to MIT
- She cannot get a job interview because she is African American.

One question we can spend a few minutes trying to parse the statements. Like, what do we mean by these statements?
often it's not as clear as it seems obvious. In each of these sentence, there is a counterfactual world in which something else has happened:

- There is a counterfactual world in which she does not take a pill (simple and clear counterfactual)
- Instead of going to MIT, she could have done something else (what? not entirely clear from the statement. The counterfactual is a little bit broader, and the statement is not necessarily clear until we have better specified the counterfactual.)
- Not entirely clear what we mean here? change her race? change the way people think about race when they make hiring decisions?

Understanding causality requires understanding counterfactual precisely.
And that seems obvious, but in empirical research, it's often a problem.
However in the rest of this lesson we are going to ignore that and assume that we know exactly what the counterfactual is.

In general, when we think of causality (as opposed to when we're thinking about prediction problems for example) we think of the possible effect of _manipulating_ a cause, and what would happen if we had nor had not manipulating this cause.

It's very, very difficult to think about what causes something because there might be many things. It's much easier to think about, what effect does a cause produce?

For example, rather than thinking, _"What causes low achievement in schools in India ?"_, it is much easier to think of problems like _"Suppose we reduce the class size in two. Will school achievement improve?"_ .

In the second question you know what it is that you are talking about. That's an important intellectual posture to take.

The counterfactula can not be directly observed. By definition, the counterfactual did not occur, since it refers to the state of the world that we would have observed absent a specific cause or program or condition.

### 070102 - Causal and Non-Causal Questions ###

In general, causal questions try to determine whether an event, intervention, or a variable affects (i.e. a school intervention) one or more variables you are interested in (e.g. learning outcomes).

Why do we care about causal statements?

- We care about causal statements because social science is intimately involved with policy making. And policy making is about changing things and hoping that something good comes out of it.
- Many of the questions we want to answer in economics/social science are causal questions: Does immigration lower the wages of the native workers? Does trade increase inequality? Would a wall between Mexico and the US stop immigration?
- So a lot of data science in social sciences aims to answer question of causes and effects.
- We are going to spend a lot of the next few lectures trying to work on determining causality.

It doesn't mean we are always trying answering these question with data. Sometimes we are asking causal questions, trying to answer them with models and with theory instead.

Do we _only_ care about causal statements?

- But it is worth noting that there are some questions that are also important and are not causal questions.
- For example, we may be interested in identifying early warning sign of children at risk in school, so we can focus our effort on them. Google wants to predict what someone may be interested in based on their search patterns to serve them the ad that is most likely to be of interest. Society may want to make good predictions of who is likely to re-offend while on bail.
- These are not causal questions. We don't care why it happens to be this series of characteristics that predicts that. But if it does predict well and we feel confident in the protection, then we reached our objective, e.g. we can release all the people who are not very likely to commit an offense and not the opposite.
- The key in these questions is to have a good predictor, not what is the effect of something.
-  When we talk about machine learning later in the semester, we're going to talk a lot about machine learning for prediction problem, although we can also try and see whether we can use machine learning for causal estimation and the extent to which it works, or in combination with, causal questions.


### 070103 - The Rubin Causal Model ###

There are various ways that different people think about causality.

Thinking about causality: the potential outcome framework

- This model is due to the Harvard Statistician Donald Rubin (book for "data analysis for social science and biostats" entirely based on this framework);
- I find it very useful to think about randomized controlled trials (which will occupy it for a couple of lectures) and about causality more generally;
- It is not the only (or perhaps most common way) to think about causality in social science (SEM is much more common - writing a structural equation and estimating it. We're going to get to that when we talk about regressions), but it is spreading, and it is very useful to be conversant with it and to be able to toggle from one to other;
- It require a slight change of frame compared to what we have discussed until now;
- Given a (observation) unit, and a set of actions, we associate each action-unit pair with a potential outcome (the word "potential" comes from the fact that we don't always observe this outcome in reality).

#### Three examples ####

Thinking in terms of potential outcomes forces us to think about the counterfactual, and help us define well posed causal questions. We want to know what is the space over which we define the potential outcomes.

1. Her headache got better because she took a pill
2. She got a good job because she went to MIT
3. She cannot get a job interview because she is African American.

1. {Unit: persons; Outcome: headache (yes; No); Action: Taking the pill (yes, no)}. every person has a potential outcome with pill and a potential outcome without pill. Some times we may refer to the no pill as the “**control**” and pill as the “**treatment**”.
2. Our second example is a bit less clear: what was the alternative if she did not go to college?
3. And our third example even less clear: what do we mean by what would happen if she was from another race? what are various ways.  A very clear way to define this third statement is in term of discrimination. So keeping everything else constant, the perception of race is what matters.

#### Definition of causal effect ####

Once we have defined this idea that for any possible action, or any possible state of the world, there is a potential outcome, and for any possible person, we can start thinking about a causal effect of the treatment.

For any unit, the **causal effect of a treatment** is the difference between the potential outcome with and without the treatment.

- Headache example. Four possibilities
  1. Y(aspirin)=No headache; Y(no aspirin)=Headache
  2. Y(aspirin)=headache; Y(no aspirin)=headache
  3. Y(aspirin)=no headache; Y (no aspirin)= no headache
  4. Y(aspirin)=headache; Y (no aspirin)=no headache
- Treatment effect
  1. Make headache go away
  2. No effect
  3. No effect
  4. Give ou headache (perverse effect)
  
What is the treatment effect?

Imagine that you have a headache and the pain level is at 7. You take an aspirin (treatment) and the pain goes down to 5. Now imagine that we have a parallel universe where we don't give you aspirin, and your pain only goes to 6 - due to time, and all the other variables. Then we could isolate the treatment effect as 1, since its the difference between the treated and untreated unit. But there are no parallel universes, and hence the the fundamental problem of causal inference - we only ever have half the observations we need to know the treatment effect. Either we give you the drug or we didn't.

There are clever statiscally ways that we can try to estimate the reality of missing counterfactual - which is what this week seems to be dedicated too (RCT's mainly).   

#### Discussion: the problem of causal inference ####

- The definition of treatment effect depend on potential outcomes but not on what is actually observed
- The causal effect is the comparison of the same unit, at the same time (post treatment). The “fundamental problem of causal
inference” (Holland, 1986) is at that at most one of the potential outcomes can be realized, and thus observed. We do not observe the same unit at the same time under the two possible state of the world (with and without action).  We might observe people before and after they take the aspirin, but before and after they take the aspirin, they are not the same people anymore. Time has passed, and time does a lot to cure headaches, or to make them worse or whatnot. We observe one or the other, and that's not a problem that we are going to solve by getting more data. In some situation, in let's say, lab situation, you can control so much the environment in a physics lab that you can assume that you have almost created a comparable situation, in which case you might be able to experiment on one unit. And then what you are implicitly doing is the assumption that you have the same unit pre and post your treatment. But in the real world that we work with in social science, we cannot make such assumptions.
- For the estimation of treatment effect, we will need to make comparison for what we observe.
- Thus we will need many units (for this discussion, two different measurements of the same person over time is two different units. We will not be able to get the treatment effect for every single person. It's not happening. So that's why we need statistics, because we need a lot of observations to be able to say anything about treatment effect.
- It will be critical to know (or make assumption about) the way that some potential outcomes got realized, and not others: This is the discussion of assignment mechanisms, which we will have in a bit. We will need to either know or be willing to make assumptions about how those observation come to be, i.e. if each obserd unit comes either from the treated condition or from the control one (in some cases, like a medicine treatment we know, but even there we may not be sure if e.g. the persons really did follow the diet or sport regime or stopped smoking we told them to do)

### 070104 - SUTVA (Stable Unit Treatment Value Assumption) ###

The problem with many units

- When think about more than one unit, things can quickly become complicated, as we introduce many, many, many states of the world.
- Imagine I (A) am with David (B) in his office, and we are both preparing class notes
- We can may be both have a headache, and both of us has the option to take aspirin (1) or not(0)
- Now each of us as 4 potential outcomes: Y(A1, B0 - When Easter takes the aspirin but David doesn't) , Y(A0,B0), Y(A0, B1), Y(A1, B1)
- The fact that he does or doesn't take the aspirin may potentially affect my outcome.
  - His decision of taking the aspirin my affect your decision. But this culd be captured by potential outcome, me aspirin, no aspirin, so if the potential outcome is the headache and the only way in which he's affecting me is through my decision to take aspirin, then we might be able to reduce the dimensionality to me taking aspirin or not.
  - But his headacke may make my headacke more likely (e.g. he's complaining or not doing his work so I have more work to do)
- In this situation ,there are $\binom{4}{n} = 6$ different comparisons, depending on which of the potential outcomes are compared
  - All my _potential_ outcomes are: 1: `obsA&B{A0,B0}`; 2: `obsA&B{A1,B0}`; 3: `obsA&B{A0,B1}`; 4: `obsA&B{A1,B1}`
  - If I pick up any 2 of these I end up with 6 possible _comparitions_ (e.g. `obsA&B{A1,B0}` vs `obsA&B{A1,B1}`). However only one of them is at maximum observable.
- Before (one unit, only A) we was trying to do one comparison with one data point. That was not enough. But now we're trying to do six comparison with two data points. It's still not enough, and it's not improving. As we are adding more units we are adding more potential comparison: we will never get enough data to estimate what we want.

We need to solve this problem, and the way to solve it is with an assumption:

Stable Unit Treatment Value Assumption

- (perhaps) Natural assumption in the headache example: David’s headache does not influence mine.
-  Ways in which it will fail?

(fisrt) Assumption: _(SUTVA) The potential outcomes for any unit do not vary with the treatments assigned to other units; and, for each unit, there are no different forms or versions of each treatment unit leading to different outcomes._

- The treatment and the counterfactual are well defined
- No interference:
- What this means ?
- Examples where it will fail
  - Immunization
  - Help with job applications for unemployed.
  - Extyernalities or spill-overs (the status of other persons directly affect me)
- Ways to solve these problem? When these assumptions might be violated, then we try and think about which way it might be violated and what comparisons we can make that does not violate it.
  - ex. in immunisation, I compare villages that are sufficiently far apart that they do not interact
  - in some cases, we're positively interested in cases in the externalities on their own. So we might be interested in defining one potential state of the world, one potential condition, as I am not treated but my neighbor is treated. So, I could have one treatment that I am treated, one treatment is I am not treated and I don't know anybody who is treated, and one treatment where I am treated but I am close to people who got treated as well. Properly defining treatments in this way, the SUTVA would still hold.

### 070105 - The Selection Problem ###

The importance of the Assignment Mechanism

From now on assume SUTVA holds(we'll look at the end of the semester, speaking about experimental design, what can we do when SUTVA doesn't hold).

Then the aspirin example, where each of Myself and David can either take (1) or not take (0) aspirin, and what the other does is NOT relevant.
This naturally extends to more than two units.

Notation: Let’s assume we have a population of size $N$, indexed by $i$ taking values $1,...,N$

Define :

- $W$ is the treatment decision, e.g. $W_i=0$ you didn't got the p-ill, $W_i=1$ you got it
- $Y$ is the potential outcome in terms of headacke, where $Y_i(0)$ is the potential outcome of a untreated individual. You can have untreated potential outcomes of guys that has been really been untreated (W=0), and in this case it is observed, or the untreated potential outcomes of a guys that instead has been treated (i.e. what it would have been his headacke in the case if it would have been instead not been gived the pill), and in this case it represents the "missing", not observable outome. The same, conversily, goes for $Y_i(1)$.
- $Y_i^{obs} = Y_i(W_i) = Y_i(0)$ if $W_i = 0$ and $Y_i(1)$ if $W_i = 1$ The observed is the outcome treated if you was effectivelly treated and the non-treated outcome if you were not been treated
- $Y_i^{miss} = Y_i(1 − W_i) = Y_i(1)$ if $W_i = 0$ and $Y_i(0)$ if $W_i = 1$ The missed potential outcome is those of being treated if you were not treated and those of not being treated if you were instead being treated
- Causal effect for person $i$: $Y_i(1) − Y_i(0)$


Missing data problem (The "Holland problem"): we only see $Y_i^{obs}$ so we cannot calculate the treatment effect for each person.

We will try to infer something about $Y_i^{miss} (the potential outcome in the case where I don't observe it) from the data we do indeed observe it, but in doing that, knowing the _assignment mechanism_ will be essential: why did some people end up treated and other not?

**The selection problem**

Imagine we have a larger group of people who took aspirin, and a group who did not, and we decide to take the sample mean of people of headache for people who got or did not get the pill.

By property of the sample mean, we know that this is a good estimator for:

- $E[Y_i | W_i = 1] − E[Y_i | W_i = 0]$ Thedifference of the sample average of those treated vs the sample average of those not treated

However I can build my sample mean only based on the observed cases in the set of all potential oputcomes, i.e. the treated potential outcomes of those effectivelly treated and the non-treated potential outcome of those non treated. So what I really sample is:

- $E[Y_i^{obs}|W_i = 1] − E[Y_i^{obs}|W_i=0] ~=~ E[Y_i(1) |W_i=1]−E[Y_i(0)|W_i=0]$  

Let's now add and remove $E[Y_i(0)|W_i = 1]$:

- $E[Y_i^{obs}|W_i = 1] − E[Y_i^{obs}|W_i=0] ~=~ E[Y_i(1) |W_i=1] - E[Y_i(0)|W_i = 1] + E[Y_i(0)|W_i = 1] - E[Y_i(0)|W_i=0]$

So my sampled difference between the two groups is actually made of two parts:

- $E[Y_i(1) |W_i=1] - E[Y_i(0)|W_i = 1]$ is the treatment effect of the treated persons. This represents the treatment effect that we would like to be able to calculate, that is, the outcomes in the case where the treatment group received the treatment (which we observe) compared to outcomes in the case where the same treatment group would not have received the treatment (which is the counterfactual, and cannot be observed).
- $E[Y_i(0)|W_i = 1] - E[Y_i(0)|W_i=0]$ is the selection bias. It's a difference that exists anyway
in their potential outcome, in the untreated state, that has nothing to do with any treatment. It just happens to be, to go back to the headache example, that people who have very strong headache are more likely to take a pill.

Examples of selection biases

- People take headache pills because their headache is pretty bad.
  - If people kind of have a choice to take a pill, and people with a stronger headache take the pill, and then later I go and I measure the intensity of their headaches, then I might well find that all the people who got the pill on average, they tend to have stronger headaches than the people who did not.
  - But that's a combination of any effect that the aspirin may or may not have, combined with the fact that the people who take the pill are the ones who have the stronger headache to start with.
  - So maybe in this case, there is a very strong selection bias with all the people who have a very big headache take the pill, and then there is, say, some treatment effect as well. And the combination could well be that I observe that people who have taken the pill have a stronger headache.
- People who go to college have all sorts of attributes that are different than people who do not.

When does the bias disappear? The name of the game in social science is to make this bias go away.
So find situation in the real world or either create situation where it goes away, where you are reasonably confident that you are doing the right comparison, such that the potential outcomes of the treated and untreated become similar.

See also this pdf that explains it very well (the first few pages suffice): http://www.stat.columbia.edu/~cook/qr33.pdf


### 070106 - Randomization Solves the Selection Problem ###

In a completely randomized experiment, $N_t$ unit are randomly drawn to be in the treatment group, and $N_c$ units are drawn to be in the control group.

Then, the probability of assignment does not depend on potential outcomes: $E[Y_i(0)|W_i = 1] - E[Y_i(0)|W_i=0] = 0$ and

$E[Y_i^{obs}|W_i = 1] − E[Y_i^{obs}|W_i=0] ~=~ E[Y_i(1) |W_i=1] - E[Y_i(0)|W_i = 1] ~=~ E[Y _i(1) − Y_i(0)|W_i = 1] ~=~ E[Y_i(1) − Y_i(0)]$

In the case of a completely randomized experiment, where the selection bias goes to zero, the treatment effect on the treated $E[Y_i(1) |W_i=1] - E[Y_i(0)|W_i = 1]$ tells us the treatment effect for the treatment group, but becasue we are 
in a completely randomized experiment, since the treatment and control groups were randomly assigned and hence we assume there to be no underlying systematic differences between the groups, the measured treatment effect is the treatment effect on the treated, but since the treated is a random sample of the sample, it is also the average treatment effect, including the contro lgroup as well. 

Types of RCT (Randomised Control Trails):

1. Completely randomized
2. Stratified randomization : Blocks of some covariate X are created, randomization is done within each block
3. Pairwise randomization: Pairs are created, randomization is done within each pair.
4. Clustered randomization: Units are not individual, but groups of individuals (e.g. classrooms)
5. The result above hold whatever the type of randomization. why?
  - As long as the probability of assignment does not depend on your strata when you have a stratified design, if you just take the difference between treatment and control in any of those settings, you're back to a situation where this is going to be zero: the selection bias disappear, and the difference between treatment and control is the causal effect.
  - Because on how the expectations works, if the re is no bias at the level of each strata, there will no bias at the level of the population.
6. Why would we prefer a type of design rather than another?
  - Sometimes a kind of experiment arise naturally in one way
  - You might think that there are different causal effects in different groups. And if you want to be able to look at them, then you should make sure that you have a randomization within each sub-strata to look at them separately.
  - Reduce noises: there are differences between people, and even if the above statement is true in expectations, in any realization of the sample, they might have differences that might affect their outcomes and that will introduce some noise that you want to get rid of. So that would be a reason to stratify.

We'll have a whole lecture on experimental design.

**Difference between clustered, stratified and pairwise randomisation**

- _Clustered randomization_ refers to the case where groups (classrooms, villages, households, etc.) are all assigned to either treatment or control as a group or cluster. This is very common in cases where the treatment doesn’t make sense to assign at the individual level. For example, suppose the treatment is to receive an extra teacher in a classroom to provide extra help for students. It would not make sense to assign individual students to either treatment or control, instead, whole classrooms would need to be assigned to either group.
- _Stratified randomisation_ refers instead when individuals are placed into groups ("strata", "cells" or "blocks") based on a set of characteristics, and then randomly assigned to either treatment or control from within those groups. Stratification is a useful way to ensure that treatment and control groups are well-balanced according to key variables that we observe at baseline. Suppose that you have a group of 100 households where 80 are male-headed households and 20 are female-headed households. You want to measure the impact of a microfinance program where loans are given to the head of the household, and suspect that it may be relevant whether the household is headed by a male or female. In order to ensure that the number of female-headed households in treatment and control is equal (i.e., the treatment and control groups are well-balanced along gender of household head) you may want to use stratified randomization.
- _Pairwise-randomization_ refers to a special case of stratified randomized experiments, in which each stratum contains exactly two units; one treatment and one control unit. 

### 070107 - Types of RCTs ###

Back to our examples Let’s think of an RCT that would make sense for each of these
questions:
- Aspirin
- College
- Race and jobs

#### Aspirin ####

- This is the traditional medical RCT. Completely randomized experiment, individual level randomization (perhaps stratifying by age, race, et cetera.).

#### College ####

- We may not be able to randomly assign college going: some people may not want to go to college no matter what, and some people will want to go no matter what.
- What could we do?
- A scholarship program would be an encouragement, and ensure that some people are _more likely_ than other to go to college.
- The scholarship program itself can be analyzed as a simple randomized experiment.
- But this is not the impact of going to college!
- This is called an _encouragement design_. The analysis of these types of experiment is left for later in the semester.
- Send cvs to recruiters in two versions, indicating if the people got a college degree or not indicating that
 - But it's not the only thing I may be interested in, ok the "effect of the title" on recruiters, but you may increase your salary going to college as becasue you really learn something interesting
- Compare the salary of those that did the college with those that has been accepted for colelge but actually did not take the offer
  - it's not a randomised trial but where we hope the counterfactual is reasonably similar to the factual. We'll see examples of these cases later on. 

#### Race ####
- We need to start by refining the question.
- Are we meaning to say that African American are in general disadvantaged?
- Or specifically that they are discriminated against by potential employers, at given characteristics?
- If that is the case we can manipulate race perception without changing anything else
  - Audit studies : perception of race is manipulated by sending pairs of actors with similar characteristics except race
  - Resume studies: perception of race is manipulated by sending resumes with identical characteristics except the extent to which the name sounds African American: this can be analyzed as pairwise randomized experiments where each cv exists in two exactly similar versions except the characteristics you want to test (study of Marianne Bertrand and Sendhil Mullainathan. The call-back for african-liked names was half of white names, and they stratified for education, and found that this was even more true for higher educationnal level jobs)

### References ###

- Imbens and Rubin Causal Inference for Statistics Social and biomedical Sciences

## 0702 -  Analyzing Randomized Experiments ##

### 070201 - Overview: Analyzing Randomized Control Trials ###

Today what we are going to do is analyzing data that _comes_ from randomized controlled experiments (trials).

Three things we will go through:

- Analyzing RCT: The conventional approach. 99% of today's papers use this
- The Fisher exact test. Strangely enough, the way he was thinking about them a long time ago is probably most appropriate for us to think about the very large scale experiments, such as what Facebook might run or Google might run.
- Power Calculations

It's a practical application of what we discussed on confidence interval and power and all that.
So it will kind of serve as a sort of review of the material.

### 070202 - The Average Treatment Effect ###

We know that, with a RCT, $E[Y^{obs}|Wi = 1] − E[Y^{obs} |Wi = 0]$, the expectation of the outcome for the observed sample that happened to be treated, minus the expectation of the outcome of the observed sample that happens to be not treated, is the average treatment effect.

How can we:

- Find a good estimator for this quantity, given that, in fact, we are seeing just a sample of the data.
- Get an estimate of the standard error of this estimator
- Test whether it is zero against anything else than zero, which is a very conventional thing to test

Suppose we have a completely randomized experiment, with $N_t$ treatment unit, and $N_c$ control units. That is, we have an overall population and we are drawing a sample from it. And in this sample, we randomly assign treatment and control.

-  What would seems a reasonable estimator for the object of interest (the difference in expectations) ?
  - The difference in the average: So the expectation is the mean of the distribution. So the best estimator we know for the mean, is the average (sample mean). The two (treatment and control) are independent by the fact that we've randomly assigned a treatment. So the difference in the average is going to be a pretty good estimate. And in fact, it is an unbiased estimate.

**Estimating treatment effect and their standard deviation**

The difference in sample average
$\hat \tau = \frac{1}{N_t} \sum_{i:W_i=1} Y_i^{obs} −  \frac{1}{N_t} \sum_{i:W_i=0} Y_i^{obs}  ~=~ \bar Y_t^{obs} − \bar Y_c^{obs}$ is an unbiased estimate of the treatment effect.

The variance of a difference of two statistically independent variable is the sum of their variance, thus the variance of this estimator is $Var(\hat \tau) = \frac{\sigma^2_c}{N_c} + \frac{\sigma^2_t}{N_t}$ (because they are independent, there
is no co-variance to worry about.)

To estimate the variance $\hat Var(\hat \tau)$ replace $\sigma^2_c$ and $\sigma^2_t$ (the true variancesin the control and treatment groups) by their sample counterpart:

- $s^2_c = \frac{1}{N_c-1} \sum_{i:W_i=0} (Y_i(0) − \bar Y_c^{obs})^2$
- $s^2_t = \frac{1}{N_t-1} \sum_{i:W_i=1} (Y_i(1) − \bar Y_t^{obs})^2$

Essentially with that, we have everything we need to start thinking about:

- What's the average treatment effect for our experiment.
- What is a confidence interval of this average treatment effect?
- A test for whether this average treatment effect is significantly different from 0, at any level that we're interested, or significantly different from [whatever you want]

### 070203 - Confidence Intervals and Differences in Means ###

**Confidence intervals**

Recall our prior definition of a confidence interval: We want to find function of the random sample A and B such that $P(A(X_1, ..., X_N) < \Theta < B(X_1, ..., X_N)) > 1 − \alpha$

The difference in mean converges towards a normal distribution with the mean the treatment effect, and variance, V of tau.

If we subtract the two mean and we divide by the standard error, this new quantity will have a t distribution with the number of observations minus 1 degree of freedom

And then if the sample becomes large enough, then it's going to become a normal distribution.

All we have to do is apply the lessons from the lecture on confidence intervals: we know that the ratio of the difference
and the estimated standarderror will follow a $t$ distribution, so: $CI_{1−α}^\tau = (\hat \tau - t_{crit} * \sqrt{\hat Var}, \hat \tau + t_{crit} * \sqrt{\hat Var}$, where $t_{crit}$ is the critical value of the t (i.e. the inverse of the t-distribution), which depends on the sample size if we were using the t distribution, while it does not depend on the sample size if we are using the normal approximation (if the sample size is large enough).

  - with small samples take $t_{crit} from a table of t-distribution for the relevant \alpha (as we saw in the CI lecture), with $N_T + N_C − 1$ degrees of freedom.
  - with larger samples, we can use the normal approximation and take the critical value from the standard normal tables, e.g. 1.645 for $\alpha = 0.1$, and 1.96 for $\alpha = 0.05$.
  
### 070204 - Hypothesis Testing & The Oregon Example ###

**Hypothesis testing**

Let’s start with a standard hypothesis (0 versus non zero):

$$H_0: \frac{1}{N} \sum_{i=1}^N Y(1)-Y(0) = 0$$

$$H_1: \frac{1}{N} \sum_{i=1}^N Y(1)-Y(0) \neq 0$$

Natural test statistics (following our discussion last week) $t = \frac{\bar Y_t^{obs}-\bar Y_c^{obs}}{\sqrt{\hat Var}} follows a t distribution with N − 1 degrees of freedom, or with N large enough, a normal distribution.

The associated p value for the two sided test is : $2 * (1 − \Phi(t))$ [for the normal approximation]

What do we do in practice? In practice, most randomized control trials will report the difference between the treatment and the control groups' average and the standard error of this difference.
All you need to do is to take the ratio of the two, and that gives you your t statistics.
And then you can see what alpha it corresponds to.
Or if you know these very few values by heart, 1.645 [0.95 for the CDf of the Normal] and 1.96 [0.99 for the CDf of the Normal], it can give you an immediate idea of how many stars you want to put in front of your coefficient, which corresponds to the p-value for that.

**Oregon Health Insurance Experiment: An example**

President Trump: "Nobody knew health care could be so complicated”.

What are the causal effects of Affordable Care Act?

We are lucky to have a unique experiment that tells us a lot about it:

  - Oregon wanted to expand Medicaid before ACA but did not have enough money to do it for all the people that was elegible for the extention: they decided to do a lottery (they tought it was fair to do like that)
  - Amy Finkelstein led a team of researchers that conducted a study to follow outcomes of winners and losers of the lottery.

**Some Results**

Table 1.6: OHP effects on health indicators and financial health
Reference: Josh Angrist and Steve Pischke, Understanding Metrics.

It's a pretty common way to present results for a RCT.

What we have is the control mean, the average of the variable for each of the outcomes. And then we have the treatment effect, the difference in the averages between the control group and the treatment group.
And finally we have the standard error under parhentesis.

At the bottom you have the sample size, they are big enought that we can use the normal. We can build confidence interval with this information.

CI for treatment effect (for a given vaariable) of 0.39 and s.error 0.008 (with simple size big):

[0.039-1.96*0.008; 0.039+1.96*0.008]

1.96 is the critical value for the 99% confidence interval

Null hyphotesis test: If the value of mean(difference)/se(difference) is greater than 1.96 you can reject the null hypothesis the treatment and control comes from the same population (zero treatment effect) 
Equivalently, if the confidence interval includes 0, we fail to reject the null hypothesis that the treatment has no effect. 

### 070205-070212 - Examples: Hypothesis Testing Using Confidence Intervals ###
(no video)

**Let us spend some time with this table**

Let’s compute a 95% confidence interval for the effect of insurance on the "health is good" variable: $(0.039-0.008*1.96; 0.039+0.008*1.96)$

- Is the hypothesis that cholesterol levels went down in Portland rejected at the 10% level ?
  - No: 0.53/0.69 is below 1.645 (the critical value for 10% level using the Normal distribution)

What is the average physical health index in the treatment group in Portland?

**Interlude: do RCT really matter**

https://www.easy-lms.com/the-impact-of-extending-medicaid-coverage/course-4820

**Another view of uncertainty**

With Fisher we are taking a slight detour from the statistics we have seen so far.

We are now not going to assume that the uncertainty in our data comes from the fact that we have a _sample_ drawn from an population.

If we have the entire population, where is uncertainty coming from? Do we even need confidence intervals?

The uncertainty is because of the missing data: each individual is _either_ treated or control, but not both. And since everybody has a different potential outcome pairs (for treated and control), for each draw that nature gives us, we would get a slightly different answer.

With big data this is the right way to think about this: Imagine running an experiment on Facebook, or using the Swedish data: this is the relevant question.

**Fisher: Can we reject that the treatment has no effect on _anyone_**

Fisher was interested in the **sharp** Null hypothesis: $H_0: Y_i(0) = Y_i(1)$ for all $i$

Note that this sharp null hypothesis is very different from the hypothesis that the _average_ treatment effect is zero.

he sharp null allows us to determine for each unit the counterfactual under $H_0$.

The beauty of it is that we can calculate, for any test statistics we are interested in, the probability of the observed value under the sharp null

So, suppose we choose as our statistic the absolute difference in means by treatment status:

$|T^{ave}(W,Y^{obs})| = |\bar Y_t^{obs} - \bar Y_t^{obs}|$


**Fisher exact test**

We can calculate the probability, over the randomization distribution, of the statistic taking on a value as large, in absolute value, as the actual value given the actual treatment assigned.

This calculation gives us the p-value for this particular null hypothesis:

$p = Pr(|T^{ave}(W , Y^{obs})| ≥ |T^{ave}(W^{obs}, Y^{obs})|)$

Neyman was interested in drawing conclusions about the underlying population, using observations from a finite sub-sample.
Fisher was interested in drawing conclusions about a given sample.

**Example: Cough and Honey**

A randomized study where children were given honey or nothing.

Main outcome: cough severity the night after the assignment (from 1 to 6)

Imbens and Rubin (2015) use it to illustrate Fisher exact test

First, assume we have the data for the first 6 children

Table 5.4 First Six Observations on Cough Frequency from Honey Study
```
Unit  |Potential Outcomes | Observed Variables
      |Y_i(0)    Y_i(1)   | W_i X_i(cfp) Y_i^obs(cfa)
--------------------------------------------------------
1     |?         3        | 1   4        3
2     |?         5        | 1   6        5
3     |?         0        | 1   4        0
4     |4         ?        | 0   4        4
5     |0         ?        | 0   1        0
6     |1         ?        | 0   4        1
```
Reference: Imbens and Rubin ”Causal inference for statistics, social and biomedical sciences”

$T^{obs} = 8/3 − 5/3 = 1$


Table 5.5: First Six Observations from Honey Study with missing potential outcomes in brackets filled in under the null hypothesis of no effect
```
Unit  |Potential Outcomes | Observed Variables
      |Y_i(0)    Y_i(1)   | W_i X_i(cfp) Y_i^obs(cfa)
--------------------------------------------------------
1     |(3)       3        | 1   4        3
2     |(5)       5        | 1   6        5
3     |(0)       0        | 1   4        0
4     |4         (4)      | 0   4        4
5     |0         (0)      | 0   1        0
6     |1         (1)      | 0   4        1
```

All the possible assignment vector, and associated statistic

W_1 W_2 W_3 W_4 W_5 W_6 | levels
0   0   0   1   1   1   | -1.00
0   0   1   0   1   1   | -3.67
0   0   1   1   0   1   | -1.00
0   0   1   1   1   0   | -1.67
0   1   0   0   1   1   | -0.33
0   1   0   1   0   1   | 2.33
0   1   0   1   1   0   | 1.67
0   1   1   0   0   1   | -0.33
0   1   1   0   1   0   | -1.00
0   1   1   1   0   0   | 1.67
1   0   0   0   1   1   | -1.67
1   0   0   1   0   1   | 1.00
1   0   0   1   1   0   | 0.33
1   0   1   0   0   1   | -1.67
1   0   1   0   1   0   | -2.33
1   0   1   1   0   0   | 0.33
1   1   0   0   0   1   | 1.67
1   1   0   0   1   0   | 1.00
1   1   0   1   0   0   | 3.67
1   1   1   0   0   0   | 1.00

p value? 16/20 = 0.8

**Simulation based p value**

If we have more observations we may not be able to do all the 36 permutations (N choose k), where k is the number of treated subjects.

We can do it as simulation: draw an assignement. Compute the statistics. repeat K times, compute the probability that the
statistics is above the observed statistics.

Example for cough and honey study (35 honey, 37 control).

Table 5.9: P-values estimated through simulation Honey Data from Table 5.2 for null hypothesis of zero effects.
Statistic is Absolute Value of Difference in Average Rank of Treated and Control Cough Frequencies.
P-value is Proportion of Draws at Least as Large as Observed Statistic.

```
Number of Simulations   p-value   (s.e.)
----------------------------------------
100                     0.010     0.010
1,000                   0.044     0.006
10,000                  0.044     0.002
100,000                 0.042     0.001
1,000,000               0.043     0.000
```


### References ###

- Imbens and Rubin Causal Inference for Statistics Social and biomedical Sciences
- Angrist and Pishke Mastering Metrics


## 0703 -  (More) Explanatory Data Analysis: Nonparametric Comparisons and Regressions ##
TODO






