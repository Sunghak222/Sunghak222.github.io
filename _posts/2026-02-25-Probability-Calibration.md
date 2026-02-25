---
title: "Probability Calibration"
date: 2026-02-25
layout: post
categories: [Data Science, Concepts]
tags: [Probability]
---

Probability Calibration
=============

Concepts
-------------
Let's say a model is predicting the colors of balls in a box, and the model says it is blue with the probability p of 0.7. To evaluate, you collect all balls that are predicted to be blue with p = 0.7, but only 50 percent of them were blue.

In the real world, that gap matters. If the model says 70%, then about 70 out of 100 should be blue. If only 50 out of 100 are blue, the model is overconfident and not well calibrated. Poor calibration can lead to bad decisions: trusting predictions too much, misjudging risks, or wasting resources. Good calibration means predicted probabilities match real outcomes, so you can trust the model's numbers when making choices.

Mathematical Operations
-------------
$$ E[Y\mid\hat{p}] = \hat{p} $$

What this means: When you collect all cases where the model predicted probability $\hat{p}$, the actual outcome (true positive rate) should be $\hat{p}$. For example, if the model predicts 70% chance of blue balls in the box, then about 70% of those balls should actually be blue. This is perfect calibration — predicted probabilities match reality.


Platt Scaling
-------------

Core Idea
-------------
Platt Scaling is a method to turn a model’s output score into a well-calibrated probability.
Platt Scaling uses the model's prediction score as input to the logistic regression.

Why Logistic Regression?
-------------
- It outputs values between 0 and 1
- It is a monotonic function (ranking is preserved)
- It keeps the order of predictions (AUC does not change much)
- It is smooth and simple
- It works well when distortion looks like a sigmoid curve

Formula
-------------
The Calibrated Probability is defined as:
$$ p = \frac{1}{1+\exp(As+B)} $$
- Where A and B are parameters to learn
- s is the original model output score

How to learn A and B?
-------------
Since we use logistic regression, A and B are trained by minimizing logistic loss, defined as below:
$$
L(A,B) = -\sum_{i=1}^{n} [y_i\log p_i + (1-y_i)\log(1-p_i)] 
$$

How to apply?
-------------
We do NOT use the training set again.

We use a separate validation set:
- Train original model on training data
- Get prediction scores on validation data
- Train Platt Scaling on validation scores
- Apply the learned A, B to test data

This is to prevent data leakage.

Why Platt Scaling?
-------------
- Keeps the ranking
- Adjusts the probability scale
- Fixes overconfidence or underconfidence
- Makes probabilities more reliable


