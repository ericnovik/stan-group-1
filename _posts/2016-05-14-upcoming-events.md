---
layout: post-no-feature
title: "Upcomming Events"
description: "This week we are launching Stan Group Inc. Even though Stan Group is a commercial entity we remain comitted to keeping the Stan language and all the fitting algorithms free and open source."
comments: true
category: articles
---

## Bernoulli Model
The simplest model we can try is perhaps based on a series of independent binary events, each with an unknown probability $$ \\(\\theta\\) $$ say when someone "hands you" a series of zeros and ones and asks to estimate the posterior probability of $$ \\(\\theta > 0.50\\) $$. The joint model (likelihood) can be written as:

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


![Drawing A]({{ site.url }}/images/money.jpg)