---
layout: post
title:  "Ordinary Least Squares Linear Regression in R and Python (statsmodels)"
date:   2022-01-03 00:00:00 -0500
comments: true
categories: blog
usemathjax: true
excerpt_separator: <!--more-->
---

*A simple walkthrough on how to implement Multiple Linear Regression in R and Python.*

<!--more-->

## Overview

*Ah, linear regression using ordinary least squares (OLS).*

It feels comfortable and familiar - like your favorite pair of sweatpants or the recording of the song you've heard so many times you can anticipate every nuance in the instrumentation. It feels strange calling it machine learning and it seems almost more appropriate to call it statistical learning due to its simplicity and elegance. The line between machine learning and statistical learning/applied statistics is blurry to say the least. Call it what you want, linear regression is absolutely foundational for understanding more powerful machine learning models (logistic regression, regularization/shrinkage, nonparametric methods), and can also be very powerful in its own right for its interpretive properties and predictive capabilities. Understanding the ordinary least squares approach that is used to fit the model is also important as it illustrates how models are fit using a closed-form-formula vs. numerically (i.e. using gradient descent).

But enough praise and semantics:

## **What is Linear Regression?**

Straight from Wikipedia: "In statistics, linear regression is a linear approach for modelling the relationship between a scalar response and one or more explanatory variables." 

The linked dataset contains *sales* (in thousands) for a particular product and the product's associated advertising budgets (in thousands) for *TV*, *radio*, and *newspaper* medias. In this case, we can attempt to model the relationship between *sales* (scalar response) and one or many of the associated advertising budgets (explanatory variables) as a linear function. The former case (one explanatory variable) is referred to as simple linear regression and is generally of the form:

$$ Y = \beta_0 + \beta_1x_1 $$
