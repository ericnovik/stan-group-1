---
layout: post-no-feature
permalink: /about/index.html
title: Why Stan
description:
tags: [about, Jekyll, theme, responsive]
---
### Stan vs Traditional Data Mining / Machine Learning
There are many differences between Stan and traditional data mining tools like Neural Networks and Random Forests. The main difference is that the model is directly expressed in the Stan program and the result of the fitting process is a conditional distribution of the parameter given data, not a point estimate. This has many consequences some of which are summarized below.

![Stan vs Data Mining]({{ site.url }}/images/stan-dm.png)

### Why You Should Use Stan
* You want to be able to explain to yourself, your colleagues, your management, or your regulators what the model is doing. Your model is front and center in Stan, which is contrary to the black-box algorithms.
* You are not satisfied with the set of models that come with traditional software like Excel, SAS, and Stata.
* You want to account for unknowns at the individual and group levels. Models of this type are called hierarchical, and you should use these models to inform your decisions when your data have hierarchical structure. For example, sales by product type, geographical region, store, and individual customer.
* You want an accurate estimate of the uncertainty in your predictions in order to manage the risk entailed by your decisions. Decision Theory is a natural consequence of Bayesian Analysis.
* You model has thousands of unknowns, which violates the assumptions of traditional statistical inference. Big data requires complex models, and Stan was built to estimate models that are too complex for other software.

