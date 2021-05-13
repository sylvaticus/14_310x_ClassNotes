---
title: "14_310x - Module 02 - Fundamentals of Probability, Random Variables, Joint Distributions + Collecting Data "
author: "Antonello Lobianco"
date: "24 September 2018"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```


# 14_310x - Module 02 - Fundamentals of Probability, Random Variables, Joint Distributions + Collecting Data #

##  Lecture 0201 - Fundamentals of Probability ##

### 020101 - Set Theory and Probability ###
Definitions:

- Sample space: collection of all possible outcomes of an experiment
- Event (capital letters): any collection of outcomes (including null, individual ones, all outcomes)
- "Occurred" event: when the realised outcome is part of the event
- Event B is contained in A if every outcome of B is also an outcome of A

Set theory results:

- If A in B than A U B = B
- If A in B and B in A then A = B
- If A in B then AB = A (Intersection in set theory: the cupsized U, Intersection in Probability theory: just AB)
- A U Ac (complement) = The whole space S

We are going to rely to results from the set theory

Specific probability theory terminology:

- Mutually exclusive (disjoint) events: no outcomes in common ("mutually exclusive"" is the probabilistic sense - their joint probability is zero; disjoint is the set sense (they do not share any outcomes. Events can be mutually exclusive but not disjoint if the outcomes they share have zero probability.)
- Exhaustive ("complementary"" in set theory) events: their union is the whole space S
- Partition: group of events that are together mutually exclusive and exhaustive (they "form a partition" of the sample space)

### 020102 - Defining Probability ###

We assign a number P to every event A --> probability is a function going from mthe sample space to the real line P:S-->R

- Assioms:
  1. P(A) >= 0 for all events A in S 
  2. P(S) = 1 (normalisation)
  3. For any sequence of disjoint events P(A1 U A2 U ... Ai) = sum(P(A1) + P(A2) + .... + P(Ai))
- A probability on a sample space is any collection of numbers P(A) that satisfy these three axioms.
- If we have a sample space and if we have some function that satisfies these three properties, then we're going to call it a probability.
- And it's just simply any function defined on the sample space that satisfies these three properties.
- "Probability distribution" or "probability function" depending on the textbook.
- Results in probability theory proven from set theory:
  - P(Ac) = 1-P(A)    P(Ac sometimes difficult to find. Use this rule then)
  - P(NULL) = 0
  - If A in B then P(A) < PB)
  - For all A, 0 <= P(A) <= 1
  - P(A U B) = P(A) + P(B) - P(AB)
  - P(ABc) = P(A) - P(AB)

### 020103 - Probability: An Example ###
Uniform distribution (or "Simple sample space"): P(A) = (n outcomes in A / n outcomes in S)

- All we do is counting to compute probabilities.
- Each outcome is equally likely.

### 020104 - Probability: Another Example ###

- Sampling with replacement: p(i times A) = p(A)^i
- Sampling without replacement: p(i times A) = [n(A)!/(n(A)-i)!] /  [n(S)! / (n(S)-i)!]
- `n!` (read as "n factorial") means `n * (n-1) * (n-2) * ... * (n-n)`

### 020105 - Ordered and Unordered Arrangements ###

- "combinatorics" = establish the counting rules

*1. Experiment with two indepenent parts, the first with m possible outcomes and the scond part with n possible outcomes  *

- The exeriment has a total of m*n possible outcomes

*2. Permutation: ordered arrangment of objects *

- The total number of permutations of af a set of N objects is N!
- The total number of permutations on n objects taken from a set of N objects is N!/(N-n)!

*3. Combination: unordered arrangment of objects *

- The number of different combinations of n objects taken from a a set of N objects is N! / [(N-n)! n!] or "N choose n" (N n)
- We "take out" all the different ordering to avoid double counting by dividing, compared by permutations, by n!


### 020106 - Office Arrangements and Pizza Toppings ###

- Way to arrange 40 items in a continuous 40 slots space in a way that items 1 and 2 are on adjacent slots:
  - 1 and 2 are tighted together so there are now only 39! possibilities, multiplied by 2 because it doesn't matter if 1 or 2 are before/after, all over 40! possibilities.
  - Alternativly,  for any room there is 1/40 prob it is 1 multiplied 1/39 prob that on the right there is 2. Then multiply by two for accounting the opposite cases and finally multiply by 39 to account to all the possible twin rooms: p = 1/40 * 1/39 * 2 * 39 = 2/40
- With a list of n item of cat A and m item of cat B (e.g. vegetarian and not vegetarian toppings in pizza) which is the probability that choosing two items without replacement we end up with one item in cat A and one item in cat B ?
  - The total number of possibilities are (n+m choose 2). The possibilities that match the conditions are m*n, so the solution is:
  - `p = m*n/(m+n choose 2)`
- A bit more generally, if I want n1 items from cat A and m1 items from cat B, the possibilities for n1 items from cat A are (n choose n1) and the possibilities of m1 items from cat B are (m1 choose m).
  - The probability p is hence p = [(n choose n1) * (m choose m1)] / (m+n choose n1+m1)
  - This example will be the base for a special distribution named "hypergeometric"

### 020107 - Independence and a Basketball Example ###

- Events A and B are independent if P(AB) = P(A)*P(B) (the probability of their intersection is equal to the product of their probabilities. Valid also for more than two events.)
- This implies P(A|B) = P(A) and P(B|A) = P(B)
- If I know that one occour it doesn't tell me anything about the probabilities that the other also occours.
- All disjoint events ARE dependent events.
- If A and B are independent, also their complements are independent.
- Probability of one particular sequence of k successes in n independent trials with P(success) = p: p^k * (1-p)^(n-k)
- Number of occurrences: the occurrences can happen in any k slots out of the n available ones, so we can use combination to retrieve the number of times we can get them: (n choose k)
- So the total probability is a sum of the probabilities of any single possible equally probable occurrence:
(n choose k) * p*k * (1-p)^(n-k) ("binomial ditribution")
- Probabilieis of at least 1 success in n trials: 1-P(all failings) = 1-(1-p)^n
- This example is the base for the binomial distribution.

### 020108 - Conditional Probability ###

@see also: 030207 - Conditional distribution

- P(A|B) "Probability of A conditonal to ( or "given") B" 
- P(A|B) = P(AB)/P(B) (with P(B) not 0) "the probability of the intersection of A and B divided by the probability of B"
- I prefer to see it as P(AB) = P(A)*P(B|A)
- Both the event and the sample space are redefined:
  - the numerator of the conditional probability is obtained redefined the event as only the part of the event that's now relevant, given that we know B occurred
  - the denominator is obtained redefining the sample space, as only considering the part of the sample space that's now relevant, given that we know B occurred


### 020109 - Conditional Probability in American Presidential Politics ###


### 020110 - Bayes' Theorem ###

- P(AB) = P(A)*P(B|A)
- P(AB) = P(B)*P(A|B)
- ==> P(B)*P(A|B) = P(A)*P(B|A)
- ==> P(A|B)  = P(A)*P(B|A) / P(B)

Total probability theorem:

- P(B) = P(A1)*P(B|A1)+P(A2)*P(B|A2)+..+P(Ai)*P(B|Ai) Where {A1, A2,...,Ai} forms a partition of the sample space

Bayes rule rewritten:

- P(A1|B)  = P(A1)*P(B|A1) / [P(A1)*P(B|A1)+P(A2)*P(B|A2)+...+P(Ai)*P(B|Ai)] where {A1, A2,...,Ai} forms a partition of the sample space

- "Objective probability" is the "prior"", "ex-ante"" or "unconditional" probability - before the treatment refines it
- The conditional probability, after updating based on observation, is the "posterior"


##  Lecture 0202 -  Random Variables, Distributions, and Joint Distributions  ##


### 020201 - Introduction to Random Variables ###

- "Random variable": a real-valued function whose domain is the sample space
- It's just some numerical characteristic of the sample space we are interested in
- Number of all possible subsets of a n-items set: 2^n
- Probability: from the set of all subsets of the sample space to [0,1]
- Random variable (X): From S to R
- The probability induces the distribution of X
- Discrete random variable (r.v.): can take only a finite or countably infinite number of values
- Continuous random variable: one that can take any value in the real line
  - Note that this is a distinction on how many values the rv can takes, not the fact that the values themselves are integers or float.
    - An experiment where the r.v. can take values only 1.5, 2.5 or 3.5 is a discrete r.v.


### 0202xx - Somewhere ###

#### Probability function (discrete r.v.) ####

- "Probability function PF": the probability associated to each possible value of the r.v. (the PMF, probability mass function)
- The probability function (PF) of X, where X is a discrete r.v., is the function fx such that for any real number x, fx(x) = P(X=x)


#### Hypergeometric distribution ####

Probability that out of m+n samples *without replacement* n1 comes from a given category damned as "success" (and m from the other category - for more than 2 categories see the Multivariate hypergeometric distribution):

- [(n choose n1) * (m choose m1)] / (m+n choose n1+m1)

The formula is normally expressed in terms of n1 being the random variable:

- X ~ H(N,K,n)
- p_X(x) = [(K choose x) * (N-K choose n-x)] / (N choose n) 

- The hypergeometric distribution describes the number of "successes" in n trials where you're sampling without replacement from a sample of size N whose initial probability of success was K/N

#### Binomial distribution ####

The binomial distribution describes the number of "successes" in n independent trials where the probability of success in each is p.

- X ~B(n,p)
- p_X(x) = (n choose x) p^x (1-p)^(n-x)

Here we deal with sampling WITH replacement


#### Density function (continuous r.v.) ####

- The density, or probability density function PDF, is the continuous analogue to the discrete PF.

- A random variable X is continuous if there exists a non-negative function fx such that for any interval A in R:
  - P(X in A) = Integral_across A {fx(x) dx}

- fx(x) always higher or equal to zero
- $\int f_X(x) =1$  - integral to one instead of summing up to 1
- P(A) = P(a <= X <= b) = $\int_A f_X(x) df$
- P(X=x) = 0 for any x

#### Uniform (continuous) distribution ####
The probability of X belonging to any subinterval of S is proportional to the lenght of the subinterval

- fx(x) = 1/(b-a) if a <= x <= b, o otherwise
- X ~ U(a,b)
- P(c<= x <= d) = (d-c) / (a-b)   (with a <= c <= d <= b)

#### Cumulative distribution function ####

The cumulative distribution function (CDF) FX of a r.v. X is defined for each x as:

- Fx(x) = P(X <= x)

The same for discrete and continuous r.v.

Properties:

- 0 <= Fx(x) <= 1
- FX(x) is non decreasing in x
- lim x->-inf Fx(x) = 0
- lim x->+inf Fx(x) = 1
- Fx(x) is right continuous
- continuous everywhere for continuous r.v., with jumps for discrete r.v.

Relaction between density and cumulative functions:
 - Fx(x) = P(X < x) = $\int_{- \inf}^{x} f_X(x) dx$
 - F'X(x) = dF(x)/dx = f_x(x)


#### Joint distributions ####

- "Bivariate distribution" -> a joint distribution with two r.v.
- The joint probability density function f_XY(x,y) is the surface such that for any region A of the xy-plane:
  - P((X,Y) in A) = $\int \int_A f_{XY}(x,y) dx dy$
- The joint must integrate to 1 over the xy plane
- Any individual point or one dimensional rurve has zero probability
- For discrete r.v. the joint distribution is defined as:
  - f_XY(x,y) = P(X = x and Y=y)


##  Lecture 0203 -Gathering and Collecting Data  ##

### 020301 - An Overview: Where Can We Find Data ###

Three possible sources (also index of the lecture):

1) Existing data libraries
2) Collecting your own data
3) Extracting data from internet (somewhere in the middle)

### 020302 - Existing Data Libraries: Part One ###

### 020303 - Existing Data Libraries: Part Two ###

- Interaction with the news from yahoo
- Uber transport data available
- Wayback machine to retrieve the histoy of used book prices when new sellers enter the market

### 020303 - Existing Data Libraries: Part Three ###


### 020305 - Data That Is Not Readily Available ###

In same cases you need to go physically to data provider in a non-wired internet to make your elaborations

### 020306 - Harvesting Data ###
Two ways:

- use an API
- webscrap pages

### 020307 - Example: Google Map's API ###
- Google map example query: the time it takes to go from 54 Bremer Street to 50 Memorial Drive on Tuesday the 21st of February at 8:00 in the morning.
- Paper on the effect on route times of an odd-even licence place restriction in Delphi
- Data can be different than the general mood: this restriction has been implemented two times for 15 days each. The first tie the consensus has been that it did work, the second time that it didn't, but results show in both implementations a similar and significative reduction of travel time across Delphi.

### 020308 - Web Scraping with Python ###
- When API is not available
- The web page is dispaying the data
- Internet changed the price of used books
- Python libraries: `BeutifulSoup` and `request`

### 020309 - Web Scraping with R: Part One ###
- R package: rvest (by Hadley Wickham)
- Works best by a Google plugin named selectorgadget.com
- Car Poly

### 020310 - Web Scraping with R: Part Two ###




