---
title: 14_310x - Module_11 - Endogeneity, Instrumental Variables, and Experimental Design
author: "Antonello Lobianco"
date: "21/11/2018"
output:
  html_document: default
  pdf_document: default
---


# 14_310x - Module_11 - Endogeneity, Instrumental Variables, and Experimental Design #

## 1101 - Endogeneity and Instrumental Variables  ##

When regression control will just not doit...

- Back to our effort to establish causality, after ML where not only we don't look at causality but even at the coefficients, as there we were doing predictions
- Let's assume we are working on a model of this type: $Y_i = β_0 + β_1 X_{i1} + β_2 X_{i2} + \epsilon_i$, where we have some x1 variable that we actually observe and for which we are interested in their causal effect. And then we have some x2 variables that we may or may not observe.
-  We have seen some possible control-variables strategy to get at it: simply controlling for the right set of X_{i2} variable, difference in difference stategies, Regression discontinuity designs....
- But some times none of this will work:
  - the necessary control variables may not be available.
  - there is no regression discontinuity we can use
  - or a plausible case can be made that the variable Xi2 can affect $Y_i$
  - the variable x1 affects y but y affects also the variable x1 (tv watching make you unhappy but also being unhappy cause you watching more TV): we have an endogeneity problem!

Yahoo! News  "**Unhappy people watch more TV: study**"

https://www.reuters.com/article/us-misery/unhappy-people-watch-more-tv-study-idUSTRE4AH2WA20081118

An extensive new research study has found that unhappy people watch more TV while those consider themselves happy spend more time reading and socializing. 
“The pattern for daily TV use is particularly dramatic, with ‘not happy’ people estimating over 30 percent more TV hours per day than ‘very happy’ people,” the study says. “Television viewing is a pleasurable enough activity with no lasting benefit, and it pushes aside time spent in other activities — ones that might be less immediately pleasurable, but that would provide long-term benefits in one’s condition. _In other words, TV does cause people to be less happy._” 

“TV is not judgmental nor difficult, so people with few social skills or resources for other activities can engage in it,” says the study. “Furthermore, chronic unhappiness can be socially and personally debilitating and can interfere with work and most social and personal activities, but even the unhappiest people can click a remote and be passively entertained by a TV. _In other words, the causal order is reversed for people who watch television; unhappiness leads to television viewing._” 


Even methods like difference-in-differences and regression discontinuity designs DO NOT always provide us with causal estimates: The interpretation of your estimates depends on the validity of your assumptions in whatever design you use. If you cannot credibly claim that the parallel trends assumption is satisfied, then estimates obtained from a differences-in-differences design cannot be interpreted causally. Similarly, for regression discontinuity design, if there are reasons to believe that people can manipulate the threshold to decide what side of the cutoff they are on, then that will undermine your estimates.

Ultimately, the causal interpretation of your coefficients hinges on the credibility of the set of assumptions you are making irrespective of your design. 

If it is plausible that the outcome variable affects the regressor of interest, then There is an endogeneity problem, because the coefficient will measure the effect of the regressor on the outcome variable AND the effect of the outcome variable on the regressor. 

### 110102 - Endogeneity vs. OVB ###

**Endogeneity**

- We talk about endogeneity, when there is a mutual relationship between a regressor and the dependent variable, i.e when reasonable case can be made either way...
- Examples:
  - Democracy and Growth
  - Health and Exercise
  - Crosswords and Cognitive decline
  - Prices and quantities
  - Economic crisis and government collapse
- Everytime there is a feedback

So these are examples of endogeneity, and whenever we have endogeneity, we have a problem.
At least, we're going to be difficult to come up with a control using the strategy of adding a lot of extra rules or even a difference-in-difference once there is endogeneity.

And there were so many cases where there is no, strictly speaking, endogeneity, but there is just a very thorny, omitted variable bias, but where we know that there is no data set in which that variable is there. We could even name and pinpoint the variable if we could, but we just don't have it.

And I'll give you one example of that that's going to be our running example for today, which is the benefits of education.

**The Benefits of Education**

- There is a correlation between education and many outcomes. e.g. knowledge, earnings, fertility (in development countries), health etc. ..
- If we run a regression of any of this outcome on education without controlling for a number of stuff, they might be biased
  - What is the possible bias if we interpret the relationship between education and earnings causally?
    - For example, the family background might affect those outcomes later in life. Kids who come from poorer backgrounds may have trouble finding a job for reasons that have nothing to do with education, but say because of networks and for this reason, they have lower earnings.
      - So that's going to create an omitted variable bias. And it might be very difficult to observe everything we need to know about the kids.
    - Applying to college is an other example. People who are more energetic, smarter, will find it easier to get an education. But they might also do much better in life anyway, even without the education.
- One solutionto any such problem could be to run an RCT, but in this case, assigning education to people is not going to be possible, because an education is really a choice: you can't really force kids to stay in school past a point, and, on the other hand, you can't force parents not to send kids to school.
  - Randomly assigning “education” to people is hence not possible: one’s education is closely linked to other aspects of one’s person.

** Examples of Omitted Variables Bias (OVB) **

In all the cases above, it is highly likely that there are variables that are correlated with both the outcome and the regressor that are missing from this simple model. So your estimates are likely to be biased because there is an omitted variable problem.

For example, consider the following possible stories in each of these scenarios:

- For "Incarceration rates and police presence", places with higher incarceration rates might have more crime, leading to more arrests, and law enforcement might be more present in places where there is more crime.
- For "Grades and class attendance", better (smarter) students might be more likely to attend class because they are smart, and it is easier and more enjoyable for them. And smarter students are probably more likely to get better grades irrespective of whether they attend class.
- For "Cancer and smoking", smokers in the US are more like to be low-educated and live below the poverty line. Belonging to this demographic may in turn expose them to other carcinogens (cancer-causing substances), either through their occupation or their environments. Alternatively, other behaviors, such as increased alcohol consumption along with smoking further increase the risk of cancer.

** Examples of endogeneity problem **

Estimating the relationships in A (Income and democracy) and C (Sleep and coffee consumption) using a simple OLS model might conflate the effect of the regressor on the outcome with the effect of the outcome on the regressor. In A, democracy might increase GDP, but richer countries might be more likely to democratize. In C, drinking coffee might lead to sleeplessness and a poor night’s sleep. In turn, a poor night’s sleep makes people more tired, which might lead to higher coffee consumption. 

### 110103 - Education Instrumental Variable (IV) Example ###

**Assigning an “instrument”**

As we can't randomly assign “education” to people, what else can we do? 

- Well, instead of trying to assign education randomly, we could try and assign students to a program, which may lead them to get more education. What are examples of interventions like that (that could be randomised)?
  - a schoolarship grant
- Then we can exploit the fact that the intervention affects education: if it has no direct impact on earnings (or any other outcomes you want to look at), but you see that it affects the earnings, you can infer that it affected earnings through education, and hence that education affects earning.
- Today, we are going to use this insight to look formally at a tool to use a randomized experiment to estimate a relationship of interest: the method of instrumental variables.

[scholarship] ---affect----> [education] ---affect---> [final outcomes] (cognitive scores, earing, attitude...)

As you are willing to assume that there is no direct effect between scholarship and final outocmes, the effect MUST be due to the education channel.

The scholarship is said to be an "instrument" for education. It is something that makes education move and that we can exploit in order to look at the causal effect of education.


A good (valid) instrument has to have 3 features:

1. It has to have a significant effect on your variable (regressor) of interest: because otherwise it is not useful, since it would not explain any of the variation in your regressor.
2. It has to be randomly assigned (or as good as randomly assigned): because otherwise it might be correlated with other variables that are omitted from your model but also have an effect on your outcome.
3. It does not affect your outcome directly: because otherwise you would misinterpret the effect of the instrument as the effect of your regressor on the outcome variable, when it is in fact a combination of the effect of the regressor and the direct effect of your instrument. This would bias your coefficients. This is often referred to as “the exclusion restriction”.

II and III together imply “independence” of the instrument with respect to the potential outcomes. 

Do not confond instruments with omitted variables: An _omitted variable_ is precisely a variable that is still correlated with your outcome after controlling for your regressors, whereas an _instrument_ should be uncorrelated with your outcome after controlling for your regressors. So controlling for the random scholarship award would be useless, because, in theory, if it is a good instrument it should be uncorrelated with your outcome once you account for the effect of your regressor.

Note that whether your instrument is correlated or uncorrelated with the outcome depends on whether your regressor actually affects the outcome. If assigned scholarships have an impact on education and education affects the outcome, assigned scholarships will be correlated with both education and the outcome, since education and the outcome are correlated with each other. On the other hand, if assigned scholarships have an impact on education, and education doesn’t affect the outcome variable, then scholarships and education will be correlated, whereas scholarships and the outcome won’t be correlated (since education and the outcome are uncorrelated with each other).

We are now going to formalise this concept.

### 110104 - Education IV Example, cont. ###

**Random Assignment as an Instrumental Variable**

- The question: How much does education improve cognitive scores, and wages.
- Notation:
  - $Y_i = α + βA_i + \epsilon_i$
  - where $A_i$ is whether individual i goes to secondary school (the symbol "A" for education is for historical reasons), and $Y_i$ is earnings (but we'll see also cognitive scores)
- We are however NOT willing to make the assumption that $\epsilon_i$ is independent of $A_i$.
  - That's the big difference between the setting we have now and what we looked at before.
  - Because here there is an omitted variable problems, and we don't have the (omitted) variable (which is the "ability", or "spunk" or "network connections" or whatever goes with the background). 
  - So in the absence of that omitted variable, I know that if I estimated this equation, it would be biased. Beta would be biased.
  
- Note that this formulation still assumes that the effect of education is the same for all people. We will discuss how to interpret IV when this is not true in a bit.
  - We may not like this assumption because education might be more effective for some people than some others.
  - We'll discuss how to interpret the Instrumental Variable (IV) when this assumption is not correct later on in this lecture.
- We are interested in the causal estimation of beta


**Randomized Scholarship**

So what did we do?

- We went to Ghana where primary and junior high school are free.
- Then there is a big gateway exam after junior high school, and you can only go to secondary school if you qualify. But, in addition, it's very expensive.
- Pascaline Dupas, Michael Kremer and I have conducted a randomized experiment in Ghana which makes it more likely to that students who qualify for high school actually attend: a scholarship program. Scholarship were randomly assigned to students who qualified for secondary school but had not yet enrolled by September. They had qualified for secondary school but didn't go (it's pretty common in Ghana to show up late in secondary school, to show up in January instead of September, because you manage to gather the money)
  -  we took a sample of about 3,000 kids, and we randomly selected about 1,000 of them, offering them a scholarship to cover the 4 years of secondary school.
- Let $Z_i$ be a (randomly assigned) dummy variable equal to 1 if one is assigned to the treatment group (and were therefore offered the scholarship), 0 otherwise.
- They did that in 2008 and followed them (both the losers of the scholarship, the people who didn't get it, and the winners) since then:
  - So first, they had to wait until they left school, which they did in 2012.
  - And then since 2013 they've been interviewing them regularly
- Receiving a scholarship increases the probability to ever enroll in high school by 33% for females, 36% for males, but it does not translate into a one to one relationship between going to school because most of people who got the scholarship did go to secondary school, but not everyone.
- On the other side, a bunch of the kids who didn't get the scholarship managed to find some other way to pay for secondary school and attended it.

**Scholarship and participation in Senior High School**

- They look at boys and girls separately.
- They found back in 2013 97% of the kids that they had identified in 2008 (remarcable!).
  - when you don't track people, especially not equally, the people who remain are not necessarily representative of the initial population.
  - there are two concerns with low tracking: first, the sample you end up with might be different from the sample you started with. Therefore, you might be worried that you are not estimating what you thought you were or on the relevant population of interest. The second reason one might worry about low tracking rates is that the set of people who couldn’t be tracked might be different in both groups. This would imply that the remaining samples in each group are no longer comparable, and your estimates would be biased since they fail to account for that.

```
Table 2: Survey Rate and Educational Outcomes at 5-yr Follow-up
Note: SHS stands for Secondary School

|------------------------------------------------------------------------------------------------------------------|
|                                             | Female                           | Male                            |
|                                             | Treatment | Control | Difference | Treatment | Control | Difference|
|                                             | Mean      | Mean    | (SE)       | Mean      | Mean    | (SE)      |
|                                             | (1)       | (2)     | (3)        | (4)       | (5)     | (6)       |
|---------------------------------------------|-----------|---------|------------|-----------|---------|-----------|
| **Panel A. Survey Rate**                    |           |         |            |           |         |           |
| Surveyed in person in 2013                  | 0.97      | 0.966   | 0.004      | 0.957     | 0.969   | -0.012    |
|                                             |           |         | (0.012)    |           |         | (0.012)   |
| Observations                                | 333       | 701     |            | 345       | 671     |           |
|                                             |           |         |            |           |         |           | 
| **Panel B. Educational Outcomes**           |           |         |            |           |         |           |                   
| Ever enrolled in SHS                        | 0.78      | 0.446   | 0.334      | 0.927     | 0.569   |  0.358    | 
|                                             |           |         | (0.032)*** |           |         | (0.029)***|
| Completed SHS                               | 0.576     | 0.244   | 0.332      | 0.706     | 0.323   |  0.383    | 
|                                             |           |         | (0.031)*** |           |         | (0.031)***|
| Started and Stopped SHS                     | 0.146     | 0.072   | 0.073      | 0.161     | 0.089   |  0.071    | 
|                                             |           |         | (0.02)***  |           |         | (0.021)***|
| Enrolled in SHS other than admission SHS(a) | 0.015     | 0.004   | 0.011      | 0.012     | 0.008   |  0.004    | 
|                                             |           |         | (0.006)*   |           |         | (0.006)   |
| Still enrolled in SHS                       | 0.043     | 0.124   | -0.081     | 0.048     | 0.149   |  -0.101   | 
|                                             |           |         | (0.02)***  |           |         | (0.021)***|
| Even enrolled in SHS track...                                                                                    |
|---------------------------------------------|-----------|---------|------------|-----------|---------|-----------|
Source: Duflo et al. (2014) “Estimating the Impact and Cost-Effectiveness of Expanding Access to Secondary Education in Ghana"
```

This is really the causal effect of getting the scholarship on going to secondary school because that was randomly assigned.

### 110105 - Constructing Meaningful Estimates ###

**Combining the two: an instrumental variables estimate of the effect of going to school on cognitive scores**

We may be interested in just the results from table A: the effects of schoolaships on {earnings,cognitive scores}, but there could be other ways to improve them other than scholarships, like building new schools, making them chapper, etc.
What we really want here is the effect of education on {earnings,cognitive scores}, not the specific effect of scholarship.

We can combine the fact that we have an effect of scholarship on going to school and an effect of scholarship on test scores to look at whether we can get a good estimate of the causal effect of going to schools on test scores.

We'll now combine the above equation ($Y_i = α + βA_i + \epsilon_i$), which is our causal effect model, with the data of the table 2 above, which is the fact that the scholarship did cause some people to be more likely to go to secondary school.

So let's combine the two with the expectation notation that we introduced when we talked about causality.

- Effect of treatment on participation can be measured by: $E[A_i|Z_i = 1] − E[A_i|Z_i = 0]$ (1)
  - this is for example 0.334 for girls from the table 2 above
- Effect of treatment on cognitive test scores can be measured by: $E[Y_i|Z_i = 1] − E[Y_i|Z_i = 0]$ (2)
  - for example, if we refer to Y as the "Total Standardised Score" in Table 3 below, this is 0.18 for girls and 0.125 for boys

Using our expression for $Y_i$ , we have:

$E[Y_i|Z_i = 1] = α + βE [A_i|Z_i = 1] + E [\epsilon_i|Z_i = 1]$

and:

$E[Y_i|Z_i = 0] = α + β E[A_i|Z_i = 0] + E [\epsilon_i |Z_i = 0]$


Therefore:

$E[Y_i |Z_i = 1] − E[Y_i |Z_i = 0] = β(E[A_i |Z_i = 1] − E[A_i |Z_i = 0]) + E [\epsilon_i |Z_i = 1] − E [\epsilon_i |Z_i = 0]$

Now assume about $E [\epsilon_i |Z_i = 1] − E [\epsilon_i |Z_i = 0] = 0$ [we will comment this assumption in a minute]

Putting everything together:

$\hat \beta = \frac{E [Y_i |Z_i = 1] − E [Y_i |Z_i = 0]}{E [A_i |Z_i = 1] − E [A_i |Z_i = 0]} $

Beta is hence simply the ratio between the causal effect of the treatment (scholarship) on the outcome of interest (cognitive score or incomes) and the causal effect of the scholarship on the desired regressos (going to school).

In other words, what we're doing is to scalw up the effect of the scholarship by the fact that not everybody is induced to go to school because of the scholarship. Some would have gone anyway and some didn't go even despite that.

For a girl, $\hat \beta$ is 0.18/0.334=0.54, so it's telling us that girls who have gone to school have cognitive test scores that is about 0.54
standard deviation higher than girls who haven't gone to school.

Te IV estimate for the effect of education on cognitive scores is going to be always larger than, or equal to, the effect of scholarships on cognitive scores.
If everyone who got the scholarship went to secondary school, the denominator in the formula of $\hat \beta$ would be equal to 1, and therefore, beta hat would just the same as the effect as the scholarship. But if not everybody goes to school, you are dividing the numerator by a number smaller than one, so you will get a larger number: intuitively, it “scales up” the reduced form coefficient to take into account that the difference is accounted for only a fraction of the people who actually went to school. 


### 110106 - Assessing the Instrument ###

** Three conditions make Z a good instrument **

1. Be correlated with your regressor of interest, i.e. affecting $A_i$ : $E [A_i |Z_i = 1] − E [A_i |Z_i = 0]$ should not be zero, otherwise beta and its standard errors go to infinity!
  - That's easy to check. If it's not there, you just try for another instrument.
  - If the instrument is not correlated with the regressor, then it will not produce variation in the regressor.
2. It is randomly assigned, or as good as randomly assigned, so that  $E [\epsilon_i |Z_i = 1] − E [\epsilon_i |Z_i = 0] = 0$ and $E [Y_i |Z_i = 1] − E [Y_i |Z_i = 0]$ can be interpreted as the causal effect of Z on Y
  - when we use RCT as an instrument this is guaranteed
  - When we use instruments that are not randomly assigned that are given to us by nature, or by natural experiment, it might have to be argued that it is in fact true (it needs to be checked)
  - If the instrument is not randomly assigned, then the instrument may not produce only exogenous variation in the regressor.
  - If the instrument directly affected the outcome variable, then we could not distinguish between the effect from the variation in the regressor and the direct effect.
3. It has no direct effect on $Y$ (exclusion restriction). This may or may not be true, it has to be argued on a case by case basis, and cannot be tested.
  - otherwise part of the effect of the iv (scholarship) on the outcome of interest (test scores) comes not from the regressor of interest (education), but from another channel.
  - otherwise we would have an omitted variable bias and $E [\epsilon_i |Z_i = 1] − E [\epsilon_i |Z_i = 0] would not be zero (the error term would be correlated with the instrument) 
  - you could prove yourself wrong (e.g. schoolarship per se make people more confident), but you're never going to be able to prove yourself right
  - we cannot test that our choosen instrument has no direct effect on Y, because the direct effect on Y and the indirect effect (through the instrumented for regressor) cannot be disentangled. The key assumption underlying any IV strategy is that the only impact of the instrument on the outcome (test scores) is through your instruments’ effect on the regressor (attendance). This is commonly known as the “exclusion restriction.” This is not an assumption we can test or prove empirically, but we can try and think of what other variables might be affected by our instrument and affect our outcome of interest and try to find evidence in support of our claim.
  
### 110107 - Threats to the Validity of Your Estimates ###
  
What could be another reason why a scholarship would have a direct impact on test scores?

- if the family has to scrape up money, but now they have a scholarship, they can use the money for better nutrition or books
- we only have so much bandwidth to think about things in our life and therefore, if I take some boundaries away, because you're thinking will I ever be able to manage their money, you have less bandwidth to think about making good grades
- motivation:  the kids who got a scholarship might feel, (in this case, wrongly, as that would not be true for this particular scholarship, but it's true for a lot ofsholarships) that, oh, if I don't do well in class, they are going to take the scholarship away from me.

All these would positively bias the estimate, because the people who get the scholarship, their increasing test scores would be, to some extent, due to the fact that they got a scholarship, not just due to the fact that they went to school.

This experiment (where treatment and control groups are all students that did already qualify for secondary school) could give us a good idea of keeping the exact system in place, but making it free or cheaper.
That's basically going to cover it pretty much. What it would not necessarily cover is improving education keeping this amount of money, but relaxing or removing the admission requirement. Then you're going to get new kids coming in. But they're going to be different kids compared to this one.
So that's a question of external validities:  if we don't assume that the returns to education are the same for everyone, that might not tell us the effect of another manipulation in another sample.

How do you synthesizes from the many question you put on a survey to those few points you analyse and report ?

- you are running the experiment, because you have a model in mind. You can write it up and therefore say in advance what you want to look at (and the rest is going to substantiate my case). So people call that pre-analysis plan or registration.
- the other way is to do the same exercise, but ex post, which is write down the model ex post to justify what you're looking at and then try to organize all of the things that you look at to make that case.

With a large survey with many variables, you are likely to find some significant results just by chance if you report everything or whatever is significant. By making a pre-analysis plan or an ex-post model, you can promise to focus on a few variables, which means that the chances that you’ll find significant results by chance are reduced.


**RCT as IV**

$\hat \beta = \frac{E [Y_i |Z_i = 1] − E [Y_i |Z_i = 0]}{E [A_i |Z_i = 1] − E [A_i |Z_i = 0]} $

- Careful: never forget to check both conditions when thinking about using an instrument. The third condition is often not
verified even when the first is.
- For example, in this example, could the scholarships per se be having an effect on cognitive scores?
- If the assumptions are valid: We obtain the effect of health on knowledge by dividing the effect of the program on cognitive scores by the effect of the program on education.

### 110108 - The Wald Estimate ###

$\hat \beta = \frac{E [Y_i |Z_i = 1] − E [Y_i |Z_i = 0]}{E [A_i |Z_i = 1] − E [A_i |Z_i = 0]} $

The Wald estimate is simply the ratio between the reduced form and the first stage.
 - the reduced form is the difference between the outcome for people who get the instrument equal one, and the people who don't get the instrument.
 - the first stage is the same thing for the intermediating variable (the regressor of interest)
 

- Equation 1 is the first stage relationship (the denominator).
- Equation 2 is the reduced form relationship (the numerator).
- Therefore, the Wald Estimate is the reduced form divided by the first stage. $\hat \beta$, given by the equation above, is the _Wald estimate_ of the effect of SHS participation on $Y_i$ . It is the simplest form of the instrumental variable estimator ($Z_i$c is our instrument) when you have a set up like that, with a binary instrument.

(back to Table 2)

- there are various ways to analyse the effect of the scholarship on school participation (completed, started,..)
- in the first row of panel B, among women, almost 45% of people who did not get a scholarship went to a secondary school anyway.


**Scholarship and cognitive test scores**

Now, let's look at the reduced form relationship on cognitive test scores.

- In Panel A we have some test scores (IQ measures) that we would hope are not affected  by going to school.
  - it is reassuring that there is no statistically significant difference in control/treatment
- in panel B, we see the results on a test that we computed, which is a test of both your reading ability and your mathematical ability in a sort of practical context. We are trying to give them problems that are similar to something they could encounter in a business situation or in a daily situation.
  - here there is a difference between students that received the schoolarship and those that didn't
- these are the effect of the schoolarships, but we want those of going to school

```
Table 3: Impact on General Intelligence and Cognitive Skills

|------------------------------------------------------------------------------------------------------------------|
|                                  | Female                         | Male                            | All        |
|                                  | Treatm.  | Control | T-C Diff. | Treatm.  | Control | T-C Diff. | T-C Diff.   |
|                                  | Mean(SD) | Mean(SD)| (SE)      | Mean(SD) | Mean(SD)| (SE)      | (SE)        |
|                                  | (1)      | (2)     | (3)       | (4)      | (5)     | (6)       | (7)         |
|----------------------------------|---------------------------------|---------------------------------|-----------|
| **Panel A. Scores on General Intelligence Tests**                                                                |
| Memory for Digit Span (Forward)  | 7.350    | 7.38    | -0.03     | 7.764    | 7.714   | 0.049     | 0.015       |
|                                  | (2.587)  | (2.597) | (0.175)   | (2.606)  | (2.513) | (0.172)   | (0.123)     |
| Memory for Digit Span (Backward) | 4.402    | 4.374   | 0.028     | 4.9      | 4.714   | 0.186     | 0.113       |
|                                  | (1.835)  | (1.676) | (0.117)   | (1.918)  | (1.874) | (0.128)   | (0.087)     |
| Raven's Progressive Matrices     | 6.52     | 6.558   | -0.037    | 7.403    | 7.368   | 0.035     | 0.012       |
|                                  | (2.512)  | (2.588) | (0.173)   | (2.427)  | (2.573) | (0.171)   | (0.123)     |
| **Panel B. Performance on Readin and Math Skills Test**                                                          |                   
| Standardized score, Reading Test | 0.042    | -0.096  | 0.138     | 0.242    | 0.1     | 0.142      | 0.143      |
| (6 questions)                    | (1.019)  | (1.047) | (0.07)**  | (0.852)  | (0.939) | (0.062)**  | (0.047)*** |
| Standardized score, Math Test    | -0.026   | -0.189  | 0.163     | 0.272    | 0.197   | 0.074      | 0.124      |
| (10 questions)                   | (0.998)  | (0.990) | (0.067)** | (0.949)  | (0.972) | (0.065)    | (0.048)*** |
| Total Standardized Score         | 0.006    | -0.174  | 0.18      | 0.306    | 0.181   | 0.125      | 0.158      |
|                                  | (1.061)  | (1.022) | (0.07)**  | (0.912)  | (0.944) | (0.063)**  | (0.048)*** |
| Full effort on Test (a)          | 0.598    | 0.558   | 0.04      | 0.703    | 0.663   | 0.04       | 0.042      |
|                                  | (0.491)  | (0.497) | (0.033)   | (0.458)  | (0.473) |(0.032)     | (0.023)*   |
|                                                                                                                  |
| Observations                     | 323      | 679     | 1002      | 330      | 651     | 981       | 1983        |
|----------------------------------|--------------------------------|--------------------------------|-------------|

(a): as assessed by surveyor

Notes: Data from 2013 in-person follow-up survey. Columns 1, 2, 4, and 5: means. Columns 3, 6 and 7: coefficient estimates for the treatment assignment dummy in linear probability models excluding any controls; standard errors presented in parentheses, with ***, **, * indicating significance at 1, 5 and 10%.

Source: Duflo et al. (2014) ”Estimating the Impact and Cost-Effectiveness of Expanding Access to Secondary Education in Ghana”
```

**Scholarship and earnings - at 24**

```
|------------------------------------------------------------------------------------------------------------------|
| **Panel B. Outcomes at 7-yr follow-up (2015)**                                                                   |
| Enrolled in formal study / training | 0.098   | 0.075   | 0.023     | 0.081   | 0.079   | 0.002      |  0.012    |
|                                     | (0.298) | (0.264) | (0.019)   | (0.273) | (0.269) | (0.018)    | (0.013)   |
| Apprentice                          | 0.058   | 0.087   | -0.029    | 0.039   | 0.085   | -0.046     |-0.037     |
|                                     | (0.235) | (0.282) | (0.018)   | (0.194) | (0.278) | (0.017)*** | (0.012)***|
| Wage worker                         | 0.202   | 0.19    | 0.012     | 0.359   | 0.337   | 0.022      | 0.02      |
|                                     | (0.402) | (0.393) | (0.027)   | (0.481) | (0.473) | (0.032)    | (0.021)   |
| Day or Seasonal Laborer             | 0.031   | 0.016   | 0.015     | 0.084   | 0.054   | 0.029      | 0.023     |
|                                     | (0.173) | (0.125) | (0.010)   | (0.278) | (0.227) | (0.016)*   | (0.01)**  |
| Farming                             | 0.055   | 0.084   | -0.029    | 0.093   | 0.089   | 0.004      | -0.012    |
|                                     | (0.229) | (0.278) | (0.018)   | (0.291) | (0.285) | (0.019)    | (0.013)   |
| Working for own or family business  | 0.193   | 0.225   | -0.032    | 0.087   | 0.113   | -0.026     | -0.031    |
|                                     | (0.395) | (0.418) | (0.028)   | (0.282) | (0.317) | (0.021)    | (0.017)*  |
| No occupation                       | 0.337   | 0.318   | 0.02      | 0.246   | 0.239   | 0.007      | 0.012     |
|                                     | (0.474) | (0.466) | (0.031)   | (0.431) | (0.427) | (0.029)    | (0.021)   |
| Actively searching for a job        | 0.336   | 0.237   | 0.1       | 0.31    | 0.34    | -0.03      | 0.036     |
|                                     | (0.473) | (0.425) | (0.029)***| (0.463) | (0.474) | (0.031)    | (0.021)*  |
|   - if no occupation                | 0.618   | 0.452   | 0.166     | 0.573   | 0.677   | -0.104     | 0.053     |
|                                     | (0.488) | (0.499) | (0.058)***| (0.498) | (0.469) | (0.065)    | (0.044)   |
| Total earnings last month (GHC)     | 72.7    | 58.7    | 14.0      | 161.6   | 154.2   |  7.5       | 12.2      |
|                                     | (138.7) | (116.0) | (8.318)*  | (227.1) | (229.2) | (15.3)     | (9.0)     |
| Earned no money last month          | 0.613   | 0.612   | 0.001     | 0.407   | 0.431   | -0.023     | -0.014    |
|                                     | (0.488) | (0.488) | (0.033)   | (0.492) | (0.496) | (0.033)    | (0.024)   |
|                                     |         |         |           |         |         |            |           |
| Observations                        | 326     | 689     | 1015      | 334     | 662     | 996        | 2011      |
|------------------------------------------------------------------------------------------------------------------|

```  
Notes: Data from 2013 in-person follow-up survey (Panel A) and 2015 callback survey (Panel B). Columns 1, 2, 4, and 5: means. Columns 3, 6 and 7: coefficient estimates for the treatment assignment dummy in linear probability models excluding any controls; standard errors presented in parentheses, with ***, **, * indicating significance at 1, 5 and 10%.


**The Wald Estimate**

Let us calculate the Wald estimator ourselves, for cognitive scores.

- Effect of schoolarship on cognitive scores? WOMEN 0.185 Standard deviation of tests MEN 0.125 standard deviation.
- Effect of schoolarship on years of schooling (variable not on the table)? WOMEN 1.227 years MEN 1.115 years
- Effect of years of schooling on cognitive scores? WOMEN: 0.185/1.227 = 0.15; MEN 0.125/1.115 = 0.11 (every extra year of schooling caused by the scholarship increases your test scores by 15% of a standard deviation if you are a women, 11% of a standard deviation if you're a man)
- Effect of graduating from a secondary school: WOMAN: 0.18/0.332 = 0.54 MEN: 0.125/0.383=0.33
- How about earnings?

### 110109 - The Wald Estimate - Part Two ###

For computing the Wald Estimate we are basically dividing the reduced form by the first stage.
So any bias in the reduced form is going to be blown up by the first stage.
Even "small" bias would be blown up, expecially if the first stage is very small


**The importance of the exclusion restriction**

- You can see that even a “small” violation of either of the conditions for the validity of the instrument can result in very large bias. Any bias in the reduced form will be “blown up” when I divide by the first stage difference.
-  Let’s consider some examples. Valid, no valid (violation of the exclusion restriction)? [hint: there is both here!]
 - Doctors are randomly selected to receive advice to remind their patients that it is flu season and they should take a flu shot, can we use it as an instrument for taking the flu shot, so we can estimate the impact of taking the flu shot on sick days?
   - "encouragement side" studies
   - the letter may inentivate doctors to communicate more general "ggod practice" with flu season.. wash your hands, drink orange juice.. we might find this patient being healthier, not just because they are more likely to get the flu shot, but because of this range of other behavior.
 - Kids who apply to charter schools are picked randomly if the school is oversubscribed. Would it be a valid instrument to look at your lottery number for looking at the admission to a particular charter school, to look at the effect of the charter school ?
 - Kids are in school that are randomly selected to receive deworming pills, can we use being in treatment school as an instrument for actually being de-wormed (hint: worms are highly contagious) ?. But when the deworing pill not everyone is there, or some kids can not be giving the deworming pill. Normally only half kids receive the pill.
 
 
In general, if some of the IV assumptions are violated, your reduced form estimates will be less biased relative to your Wald Estimate. 

Explanation

Continuing with the scholarship example, suppose that scholarships affect test scores through both attendance and through their effect on psychological outcomes (ex. Self-esteem). This would imply that your estimate of the effect of scholarships on test scores would be a combination of the effect of attendance, and the psychological effect. This is your reduced form estimate, which is the numerator of your Wald estimate. So if self-esteem is omitted from your model, your reduced form will be biased. But your Wald estimate is just your reduced form divided by your first stage estimate, which is less than or equal to 1. This implies that the bias in your reduced form estimates will be “blown up” in your Wald estimate because of the fact that first stage is less than 1. So your Wald estimate will be more biased than your reduced form estimates.

### 110110 - What Can We Say About the Exclusion Restriction? ###

Going back to the doctor's letter on flu vaccine of previous segment.

- it's  from a real paper by Guido Imbens
- interested in flu shot as regressor on sick days (outcome)
- "encouragement design": the random treatment is the letter sent to doctors, and this is an instrument for the variable "I have received the flu shot"

Possible flawns:
- we still got a lot of "reminders". Getting one more may not make any effect, i.e. we may not have a first stage effect. If it is not very large you can still use it, but be sure that there is really no other violation (or they would be blowen up by a very small first stage!)
- the letter may remember the doctor to give other advises other than the flu shot: wash your hand, take orange juice,.. making thinks less ill, irrispectively of the flu vaccine.


### 110111 - Instrument Validity: Charter Schools in the US ###

Going back to the charter's school example of segment 110109.

Due to too much demand, charter schools randomly select who take in.

Interested on the impact of going to charter school, and we use winning this lottery as instrument variable.

We look at all the kids who applied to charter school

Possible flawns:

- We may obtain a causal effect for the pool of applicant to a charting school, which may or may not be representative of the larger pool of students.
- Winning the lottery might make you happy and losing the lottery might make you depressed.

It seems to be a pretty good instrument, and actually, it's been used frequently (Josh Angrist and Parag Pathak)

### 110112 - Instrument Validity: Deworming and Other Examples ###

Going back to the deworming example of segment 110109.

kids are either in a school that is (randomly) assigned to receive the pill or not.

But not every kid actually gets the pill, because they were absent on this day or something like that.

I have an individual-level data set where I know whether the kid was actually de-wormed and whether they are in a de-worming school, so I can calculate for each school the fraction of kids who actually were de-wormed.

Even the kids who did not get the pill actually get much better if they are in a school with other kids who get de-wormed simply because worms are so contagious that if you have worms, you'd give them to the neighbors.

Being in a treatment school has an effect irrespective of whether I got treated myself, because worms are contagious.

Since the children who are not dewormed benefit, simply being in a treatment school (the instrument) directly affects cognitive scores (the outcome) which violates the exclusion restriction.

Any example with externalities are examples that are going to violate the no direct effect between iv and outcome, because there is a direct effect of being
in a treatment school and the outcome.

Other examples:

- One Laptop Per Child: Providing laptop, would it be a good instrument? No, it may have a lot of different effects. For example, if laptops improved cognitive abilities independent of their effect on years of education, then this would violate the exclusion restriction. 
- Free meals is an other example that would be problematic, as it may not only send more people to school but also improve their learning by better nutrition
- You can still analyse the effect of the laptop (or meal) to test score, but you can not use them as IV to look at the effect of education.


### 110113 - Local Average Treatment Effect (LATE) ###

 
**The interpretation of IV when the treatment effect is not constant**

- In reality the effect of going to school on test scores is likely to be different for different children. [remember that when we first introduced causality we did not assume constant treatment effect]
  - ideally, we would like a little i next to the beta. But we are not going to be able to estimate the causal effect for each person!
  - if we simply look at the reduced form effect, we know the interpretation of it: it's still the average treatment effect.
- In that case the simple calculation we just did does not apply
- Yet, under a fairly mild assumption (that we wil lsee soon), the Wald estimate still has a causal interpretation, which is in fact quite intuitive: it captures the effect of the treatment on those who are compelled by the instrument to get treated: this is the _Local Average Treatment Effect_, or LATE.

**Hmm... what?**

- Back to our school example, and consider the binary decision of going to high school or not.
- There could in principle be 4 groups of kids:
  1. Those who would go to high school anyways: _Always Takers_
  2. Those who would not go to high school even when offered a scholarship: _Never Takers_
  3. Those who would not go if not offered, but go if offered: _Compliers_
  4. Those who would go if not offered, but not go if offered: _Defiers_
- Now that last group is a bit weird, no? So let’s assume it does not exist
- In that case, just a few easy lines of Algebra that I will spare you are enough to prove that the Wald estimate is the effect for the compliers. The trick to the proof is that the first stage for the other two groups is zero: only the compliers contribute to the first stage.

- I'm not even getting the causal effect for all of them. I'm getting the causal effect for the type of kids who is moved by me (the "Compliers").
- So the question is, is the group that is in general of interest or not particularly? And that has to be argued.
  - In a lot of cases, it happens to be the group we are interested in with an instrument, because the instrument are often of the form that you have facilitated some group to get the thing. So for example, in the case of scholarship, there are the people who are moved by my instrument by having free school to actually go to school. So consider a government. What could be their policy to improve secondary school? Well, it could be to make it cheaper. So the type of people who would go to school because of that policy would be very much like the type of people who go to school because of my instrument. So that would be nice.
  - But when you're using very exotic instrument, which happens more with using natural variation sorts of instruments, sometimes we're like well, do we care about these people? OK, I know I have an excellent estimate of the causal effect for them. But that might not be representative of the causal effect for the rest of the world. And I'm not so interested in them.
  
  
## 1102 - Experimental Design ##

The next three segments in the course videos are under the topic "Experimental design", but they actually belong to the first lesson "Endogeneity and Instrumental Variables".

It is only from segment 110204 that Experimental design is introduced and discussed.

### 110201 - Two Stage Least Squares ###

**From the Wald Estimate to two state Least Squares**

- Instead of computing differences in means and taking the  ratio, we could have couched this in a regression framework.
  - when we run a regression on a dummy variable, it's coefficient it's simply going to be numerically the difference in means of the two groups (the records with dummy equal to 0 and those whose dummy equal to 1)
- First stage $\hat \pi_1$ in the equation: $A_i = \pi_0 + \pi_1 Z_i + υ_i$
- Reduced form : $\hat \gamma_1$ in the equation $Y_i = γ_0 + γ_1 Z_i + ω_i$
  - we could have take the ratio between $\hat \gamma_1$ and $\hat \pi_1$ and that would have been the Wald estimate
  - an alternative approach (usefull expecially if the instrument is NOT a simple dummy variable) is the following:
    - Two stage least square: Run the first stage, and take the fitted values $\hat A_i$,
    - Then, in the second stage, run: $Y_i = α + β \hat A_i + \epsilon_i$ (i.e. run a regression of y_i on the predicted values of the first stage)
    - $\hat \beta$ would be the Wild estimate
    - let's build on this second approach and prove it in the next slides

**The two stage least squares and the Wald estimates are identical**

- $\hat \beta = \frac{Cov (Y_i, \hat A_i )}{Var(\hat A_i)}$
  - by definition of what OLS is (?isn't this true only for simplest bivariate OLS models?)
- $\hat \beta  = \frac{Cov (Y_i, \pi_0 + \pi_1 Z_i )}{Var(\pi_0 + \pi_1 Z_i)}$
  - by replacing $\hat A_i$ with it's model (by definition of $\hat A_i$)
- $\hat \beta = \frac{\pi_1 Cov(Y_i, Z_i )}{\pi_1^2 Var(Z_i)} = \frac{\gamma_1}{\pi_1}$
  - by the properties of expected variance and covariance we can take some terms out
    - the covariance between $Y_i$ and $\pi_0$ is 0, because $\pi_0$ is a constant
    - the covariance between $Y_i$ and $\pi_1 Z_i$ is $\pi_1$ times the covariance between $Y_i$ and  $Z_i$
    - by properties of the variance $Var(\pi_0 + \pi_1 Z_i) = \pi_1^2 Var(Z_i)$
  - $\frac{Cov(Y_i, Z_i )}{Var(Z_i)}$ is simply the OLS coefficient of $Z_i$ in the $Y_i$ regression, i.e. $\gamma_1$.

So basically, instead of running the Wald estimate, we could have done what is literally two-stage least squares.
We can follow this approach, called **two-stage least squares (2SLS) **, even when Z is not a dummy variable.


**More generally: two stage least squares**

Consider back the model we saw at the beginning of 1101:

$Y_i = β_0 + β_1 X_{i1} + β_2 X_{i2} + \epsilon_i$

- where $X_{i1}$ is a **vector** of variables you are interested in and that you are concerned about being endogenous with $Y$, and $X_{2i}$ are some variables that you assume are not endogenous and hence you don't need to instrument (control variables.. gender, regional effects,...) In a certain way we can say that $X_{2i}$ are variables that "instrument for themselves".
  - sometimes those control variables are needed because the instrument is only valid when these control variables are included
- Look for an instrument. You need at least as many instruments as the number of $X_1$ varables (this is referred to as the "rank condition"), but you could have more.
  - Remember, our instruments, they need to be good:
    - They need to be, of course, correlated with x1;
    - They need to be on randomly assigned, or as good as randomly assigned so that we can have a causal effect of the instrument on the y;
    - And they also need to not have a direct effect on the y, so that all their effect on the y can be confidently attributed to the x.
  - Denote Z the matrix $(Z_1 , ...Z_k , X_2)$ [in other words the control variables, which do not need to be instrumented, are part of the matrix of instruments.

Intuitive steps:

- First stage: $X_{1i} = π_o + π_1 Z_1 + π_2 Z_2 + ··· + π_k Z_k + X_{2i} + ω_i$
- Second stage: $Y = β_0 + β_1 \hat X_{1i} + X_{i2} + \epsilon_i$

In practice if you do that point estimates will be correct, but the standard errors and all the tests will be wrong (because you have estimated your first stage, rather than knowing it, and the standard errors must reflect this uncertainty).


**Two stage least square in reality**

- In reality, you run two stage least square in one stage,
- Specify your Y , your X , your Z
- If Z and X have the same number of variables (e.g. if you have chosen one instrument for one endogenous variables, and included the control variable(s) in the matrix of instruments), then the 2SLS formula (which is actually done in one step) is: $\hat \beta = (Z'X)^{−1} Z'Y$ and the variance is $Var(IV ) = σ^2 (Z'X)^{−1} Z'Z (Z'X)^{−1}$
  - we replace the `X'X` formula with `Z'X`: the formula takes the X's, it projects it onto the vectors of Z's and it takes only the part of the X's that's explained by the Z to instrument with the Y.
  - note that the Z matrix contains the instrument for the endogenous variables (that we called $X_1$) plus the control variables (that we called $X_2$), but also X includes both the $X_1$'s and the $X_2$'s, this is why if you have chosen one instrument for one endogenous variables, Z and X have the same number of variables
  - for an OLS estimator, we have $Var(\hat \beta ) = σ^2 (X'X)^{−1} X'X (X'X)^{−1}$ that can reduce to $σ^2 (X'X)^{−1}$
    - the larger these matrices are in the middle, the bigger the standard error.
    - So basically, the fact that the terms in the formula for the variance of the Instrumental Variables refuse to collapse is basically accounting for the fact that the first stage is estimated, and our standard error is going to be a bit larger than if we pretended that the $\hat X$ that we're putting in the regression is known.
- If there are more instrument than endogenous variable, the formula is a big longer, but the idea remains just the same: it will project the X onto the Z and take the projected value.

### 110203 - Two Stage Least Squares in R ###

**IV in R: Test scores on SHS completion, females**

`ivreg(formula= total_score ~ shs_complete + region.f | treatment + region.f, data = data_female)`


**IV in R: Test scores on SHS completion, females with controls**

`ivreg(formula= total_score ~ . - treatment | . - shs_complete, data = data_female_controls)`

We could add more control if we wanted.
And here we would expect the control to make no difference because we have a good instrument, we have a randomly assigned instrument.

Results:

- [...]
- shs_complete: estimate: 0.63134, p-value: 0.00701

And, in fact, they don't seem to make any much difference at all: the coefficient for shs_complete is very similar to the 2SLS regression without controls)

A very significant, and quite large impact of going to secondary school on scores on the cognitive exam,

**IV in R: Test scores on SHS completion, males**

`ivreg(formula= total_score ~  shs_complete + region.f | treatment + region.f, data = data_male)`

** IV in R: Test scores on SHS completion, males with controls**

`ivreg(formula= total_score ~ . - treatment | . - shs_complete, data = data_male_controls)`

** IV in R: Test scores on SHS completion, R code**

## Load packages, load in the data
library("AER")
setwd("/whatever/your/path/is")
data<-read.csv("Ghana_data.csv")

## Prep region fixed effects
data$region.f <- factor(data$region)

## Subset male and female
data_female<-subset(data,gender=0)
data_male<-subset(data,gender=1)

## Subset data to include controls
controls <- c("total_Score", "shs_coplete", "treatment", "region.f", "age", "base_bece_score", "hhh_highest_edu")
data_female_controls<-data_female[controls]
data_male_controls<-data_male[controls]

## IV Regression: Standardised test scores on secondary schooling completion
## Female (without and with controls)
summary(ivreg(total_score ~ shs_complete + region.f | treatment + region.f, data = data_female))
# - region.f is the regional fixed effects
# - `shs_complete + region.f` is the X matrix
# - `treatment + region.f` is the Z matrix
# - the vertical bar `|` is used to separate the X and the Z matrix
summary(ivreg(total_score ~ .-treatment | .-shs_complete, data = data_female_controls))
# `.-treatment` means all variables except treatment

## Males (without and with controls)
summary(ivreg(total_score ~  shs_complete +region.f | treatment + region.f, data = data_male))
summary(ivreg(total_score ~ .-treatment | .-shs_complete, data = data_male_controls))


### The following parts of 1102 are VERY partial ###

### 110205 - Stratification ###

What does it mean to use stratification in randomization?

- Randomizing among groups that are similar ex-ante (for pre-specified characteristics) 

Stratification means you randomize among groups that are similar ex-ante. For example, you could split your sample by gender and then randomize only among females and only among males to insure that the treatment and control group had the same number of females and males.

Why would you use stratification?

- Help power by reducing variance in the outcome variable

Since stratification makes the treatment and control group similar ex-ante, there should be less random variation in the outcome variable (to the extent the variable we used to stratify to predict the outcome). With less random variation, the regressions will have more power to identify the variation caused by the intervention. So stratifying will increase your power by reducing variance for any given sample size.

- Randomization is a tool which you can use to assign a sample of individuals to different groups (although you could then decide to also change the probability of treatment within each strata).
- If there are spillovers between your units of randomization, whether or not you randomized people within strata will not change whether or not there are spillovers. You always can randomize without strata, but you often should include strata because of the power benefits. Another benefit is that it signals ex-ante that these are potentially group of interest. In medicine stratifying ex-ante is a condition for reporting sub-group analysis. 

### 110206 - Clustering ###

Clustering means to randomize across groups instead of individuals. For example, one might randomize the treatment across entire schools rather than to individuals within the school.

If the unit of observation for your outcomes is the same unit across which you randomize, that is just a simple RCT where you have a different unit of interest. A clustered randomized experiment is one in which treatment is assigned at a more aggregate level than the observation unit for your outcomes. 

Why would you want to randomize clusters?

-	When there are significant externalities within the cluster from treating individuals in the cluster
- When it is not practical to randomize at the individual level 
- When the cluster is the natural unit of randomization

Clustering does not help power and in fact, it reduces power. However, if there are significant externalities (“spillovers”), then clustering may be necessary to ensure that treated individuals do not affect your control individuals. For example if you deworm at the individual level, the externality across kids within the same classroom will bias your estimates downwards.

Sometimes, it will be impractical to give an intervention to individuals within a cluster without giving the intervention to the whole cluster (for example if you give tablets to some kids within a class you may need to give them to all of them). The intervention you are interested in randomizing, might be at the cluster level (for example, if your intervention in a teacher training program, all the children in the classes taught by this teacher will be affected), in which case your cluster might be the natural unit of randomization. 

### 110207 - Phase-in design ###

When we can not treate only a part of your sample, but all must be treated.
Jet, we can introduce a temporal delay in treatment for some, and then those not yet treated will act as a comparison group.


If an NGO says they cannot contact individuals without ever providing them the intervention, then a Phase-in approach could allow for randomization:

The phase-in approach creates random variation in the timing of the intervention, while ensuring that all individuals will receive the intervention eventually. So it would enable you to promise that everyone will get treated, whilst still allowing you to use randomization to estimate the causal impact of your treatment. 

What are the problems with phase-in randomization relative to simple randomization?

- Long-run effects are more difficult to measure
- Anticipation of receiving treatment may change the behavior of the control group (during the implementation of the experiment)

Long-run effects are more difficult to measure, because the control group receives the treatment in the long-run. For anticipation, the control group often knows it will receive the treatment soon, which may change their behavior. Spillover effects and attrition bias should not be any worse than in simple randomization.


### 110208 - “The Bubble” and Encouragement Designs ###

Randomization in a “bubble” means you randomize just above and below a cutoff range, which can be useful with organizations that do not want to significantly vary their inclusion criteria.
- You accept ("treate") everyone above the cutoff
- You randomise those in the middle (beteen a minimal and a maximal score in a given variable). You do not need to assign the same probability of being treated to people above and below the cutoff as long as you are randomizing within the just below and the just above the cutoff groups.
- You do not include anyone below the cutoff

Which of the following is true about encouragement design?

- The intervention is used as an instrument for the effect of the program
- The Ghana scholarship intervention is an example
- The intervention should not directly affect the outcomes of interest

Since encouragement designs only change the probability that one is treated, participation offer is often used as an instrument for the effect of the program. In this case, your intervention is just to offer individuals to participate in a given program (not the program itself). The scholarship in the Ghana example encouraged individuals enroll in secondary high school. To be an effective instrument, the encouragement only needs to create some variation in take-up between the treatment and the control, so it does not need all treated individuals to take up the program. Since the intervention is used as an instrument, it should not directly affect the outcome because of the exclusion restriction.

### 110209 - Two-Step Randomization ###

What does the evidence that workers assigned to active labor market interventions do better than a control group within the same town suggest about the effects of active labor market policies?

- They help the treated, compared to others in the same market

This evidence can only tell us that active labor market policies help those who are treated, but cannot tell us what the general equilibrium effects are. It is possible that the intervention reduces unemployment by reducing search costs for firms, but it is also possible that the intervention is simply giving jobs to the treated by displacing the control group from those jobs, creating no net benefit.

What approach could estimate the general equilibrium effects?

- Two-step randomized control trial correct 

In the two-step RCT, you can randomly assign the proportion of treated to areas and then randomly assigns treatment status to individuals in these areas. By varying the proportion of treated in each area, one can test the general equilibrium effects of the program.

### 110210 - An example: ALMPs in France ###

Comparing the group assigned to control to the group assigned to super control (0% assignment areas) estimates the:

- Displacement effect

The difference between these groups is that the control must compete for jobs with treated individuals. The effect of this competition will be the displacement effect of treated individuals taking jobs that control individuals would have gotten.

Comparing the group assigned to treatment to the group assigned to super control (0% assignment areas) estimates the:

- Effect on the treated 

The difference between these groups is that one of them received the treatment, which means that comparing them will estimate the effect on the treated. Comparing treatment to control is not sufficient, because the control suffers from displacement effects as well as the effect of not being treated.

### 110211 - Evaluating Multiple Interventions ###

### 110212 - Sub-treatments and Sample Size ###

What should your sample size ideally be when you have many sub-treatments? 

- Large enough sample size to test treatment vs. control
- Large enough sample size to test the effect of each sub-treatment separately with respect to the control 
- Large enough sample size to test interactions 

One needs more power to test the sub-treatments than one would need to simply do a treatment vs. control comparison, so ensuring that your sample size is large enough to test each separately compared to the control. Ideally, you can also power the experiment to compare all treatment groups to each other (interactions).

Testing interactions requires more power than testing treatment vs. control alone or even to test the effect of each sub-treatment.

### 110213 - Indonesia Rice Results ###

