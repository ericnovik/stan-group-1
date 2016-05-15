---
layout: post-no-feature
title: "Events for May 15 - 21"
description: "We will be talking about Stan at the Seattle useR Meetup on Tuesday, May 17 and at the Open Data Science Boston conference on Friday, May 20 and Sunday May 22."
comments: true
category: articles
author: Eric Novik
---

The Seattle useR [meetup](http://www.meetup.com/Seattle-useR/events/229793521/) organized by Zach Stednick is filling up so if you are in the Seattle area next week, please stop by. Here is the abstract: 

>We will build up a simple Stan model from scratch to demonstrate various parts of the Stan program and will also present a multi-level, overdispersed Poison model that can be used for pricing products in a retail setting. We fit the latter model with the rstanarm package, which can be thought of as a fully Bayesian equivalent to lme4.

Later in the week, [Krzysztof Sakrejda](https://www.bio.umass.edu/oeb/directory/krzysztof-sakrejda) will be doing a 3.5 hour worshop on Probabilisitc Programming with Stan and R at the [ODSC Boston conference](https://www.odsc.com/boston). Here is his abstract:

>Explicit and implicit models are widely used for analysis and prediction in data science.  Stan is a multi-language estimation tool ideal for applications where models matter.  Stan provides a simple language for specifying and combining model building blocks such as random effects, hierarchical structures, differential equations and mixture models.  These building blocks are contained in a probabilistic programming language that allows users to specify (nearly) arbitrary statistical models and backed by state-of-the-art optimization and sampling algorithms efficient enough for Big Model analyses.  Stan gives data scientists the freedom to apply a variety of competing models and evaluate their performance with their favorite munging and analysis tools. In this workshop participants will be introduced to the Stan language and led through the model building and criticism cycle on a real-world dataset.

At the same conference, Eric Novik will be giving another introductory Stan talk on Sunday, May 22.

We are always eager to meet new and existing Stan users, so please come by and say hello.


## Bernoulli Model
The simplest model we can try is perhaps based on a series of independent binary events, each with an unknown probability \\(\\theta\\) say when someone "hands you" a series of zeros and ones and asks to estimate the posterior probability of \\(\\theta > 0.50\\). The joint model (likelihood) can be written as:

<div>
$$ p(\theta,y) = \prod_{n = 1}^{N}\theta^{y_{n}} * (1 - \theta)^{1- y_{n}} = \theta^{\sum_{n=1}^{N}y_{n}} * (1 - \theta)^{\sum_{n=1}^{N}(1- y_{n})} $$
</div>

We typically work on a log scale, and so:

<div>
$$ \log (p(\theta,y)) = \sum_{n=1}^{N}y_{n}*\log(\theta) + \sum_{n=1}^{N}(1-y_{n})*\log(1-\theta) $$
</div>

This model can be coded in Stan in several ways. The following is my first, problematic attempt at doing so. (It will run and produce a reasonable answer, but it is neither well coded nor consistent with the math.) So, you have been warned -- do not do it this way!


```r
data {
  int<lower=1> N; # check that we have at least one observation
  int<lower=0, upper=1> y[N]; # outcome y must be either 0 or 1
}
parameters {
  real theta;
}
model {
  theta ~ normal(0.5, 0.2); # prior on theta: 95% of the mass is between 0.1 and 0.9

  increment_log_prob(0); 
  for (n in 1:N)
    increment_log_prob(y[n] * log(theta) + (1 - y[n]) * log(1 - theta));

  # the above three lines are equivalent to the following sampling statement:
  # y ~ bernoulli(theta);  
}
```

