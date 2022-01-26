---
layout: post
title:  "Ordinary Least Squares Linear Regression in R and Python"
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

It feels **comfortable** and **familiar** - like your favorite pair of sweatpants or a recording of a song you've heard so many times you can anticipate every nuance in the instrumentation. It feels strange calling it machine learning and it seems almost more appropriate to call it statistical learning due to its simplicity and elegance. The line between machine learning and statistical learning/applied statistics is blurry to say the least. Call it what you want, linear regression is absolutely foundational for understanding more powerful machine learning models (logistic regression, regularization/shrinkage, nonparametric methods), and can also be very powerful in its own right for its interpretive properties and predictive capabilities. Understanding the ordinary least squares approach that is used to fit the model is also important as it illustrates how models are fit using a closed-form-formula vs. numerically (i.e. using gradient descent).

But enough praise and semantics:

## **What is Linear Regression?**

Straight from Wikipedia: "In statistics, linear regression is a linear approach for modelling the relationship between a scalar response and one or more explanatory variables." 

The [linked dataset](https://raw.githubusercontent.com/hardikkamboj/An-Introduction-to-Statistical-Learning/master/data/Advertising.csv) contains *sales* (in thousands) for a particular product and the product's associated advertising budgets (in thousands) for *TV*, *radio*, and *newspaper* medias. Companies pour vast amounts of money into advertising, and need to confirm that their money is being well spent. What is the relationship between a product's advertising budgets and sales? In this case, we can attempt to model the relationship between *sales* (scalar response) and one or many of the associated advertising budgets (explanatory variables) as a linear function. The former case (one explanatory variable) is referred to as simple linear regression and is generally of the form:

$$Y \approx \beta_0 + \beta_1x_1$$

or in the example described:

$$sales \approx \beta_0 + \beta_{TV}x_{TV}$$

To model this relationship, we need to estimate the parameters $\beta_0$ and $beta_{TV}$ using the data we already have. Our goal is to obtain parameter estimates $\hat{\beta_0}$ and $\hat{\beta_{TV}}$ such that the linear model fits the data well, that is for each data point $i = 1,...,n$:

$$y_i = \hat{\beta_0} + \hat{\beta_{TV}}x_{TV_i}$$

## **Ordinary Least Squares**

The most common way to determine the parameter estimates $\hat{\beta_0}$ and $\hat{\beta_{TV}}$ is by minimizing the residual sum of squares (RSS) and is known as ordinary least squares. Let $\hat{y_i} = \hat{\beta_0} + \hat{\beta_{TV}}$ be the prediction for Y based on the ith value of X. Then $e_i = y_i - \hat{y_i}$ and $RSS = e_1^2 + e_2^2 + ... + e_n^2$. In the case of simple linear regression, these minimized parameter estimates can be directly calculated as follows:

$$ \hat{\beta_{TV}} = \frac{\sum_{i=1}^n(x_i - x)(y_i - y)}{\sum_{i=1}^n(x_i-x)^2},$$

$$ \hat{\beta_0} = y - \hat{\beta_{TV}}x_{TV}$$

These equations are derived by differentiating the the following equation in respect to $\beta_0$ and $\beta_{TV}$, setting the resultant equations to 0, and solving  for the parameters respectively:

$$[\sum_{i = 1}^n ((y_i - \beta_0 - x_i\beta_{TV})^2)]$$

I'll leave that as an exercise for the reader though.

## **Simple Linear Regression Example in Python**

Let's use two common python packages to create a simple linear regression model for the relationship between sales and tv spending: 

  * **pandas** is a common data science tool that allows us to upload data into dataframes for complex operations. 
  * **statsmodels** is another common data science tool that allows us to create descriptive models in the style of R outputs. Typically, if making production models, one might use the package **sklearn**, but because we want more readable outputs for interpretation purposes, we'll use **statsmodels**.
