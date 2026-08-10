---
title: "Acute Tubular Injury Prediction"
description: "A binary classifier to predict acute tubular injury from patient and clinical data; 2nd place in Boston University's inaugural MedAI hackathon"
date: 2026-04-01
lastmod: 2026-04-01
draft: false
role: "Coding and ML Engineering"
period: "Apr 2026"
status: "Completed"
featured: true
category: "Machine Learning"
stack:
  - Python
  - pandas
  - scikit-learn
  - XGBoost
  - Lasso LR
links: []
---

## Context

This project was completed while participating in the inaugural MedAI hackathon at Boston University, which provided participants with the opportunity to apply machine learning to real clinical data to solve real prediction problems.

## Contribution

I worked in a team of five. My main role was the coder and engineer; I did most of the coding and tinkering with the model parameters while others provided the background on biological data analysis.

## Approach

All teams were given starter models, but the first working iteration was an XGBoost model where the syntax and implementation was hardcoded to that specific library. I refactored the training and inference pipeline into a model-agnostic factory that could swap XGBoost, Lasso logistic regression, Elastic Net, and feature-selected XGBoost under a shared stratified cross-validation protocol. The dataset had 6,595 features and only 426 patients, and flexible tree ensembles tend to overfit small external cohorts. Switching to L1-regularized logistic regression with StandardScaler and 5-fold cross validation cut leaderboard log loss by 26.6%, from 0.439 to 0.323. I then iterated on the regularization strength, solver, and convergence settings, landing on a sparse pipeline that retained only 139 of 6,595 coefficients.

## Outcome and learning

My team finished in 2nd place in our track, out of 22 teams. The model reached a log-loss of 0.32 on the held-out test set.

## Reflections

Prior to this hackathon, I had no experience with practical ML implementations and applications. Most of what I knew came from my class in data science foundations and my own reading. Ironically, this resulted in a scenario where most of the log-loss reductions came from leveraging my coding skills instead; refactoring the starter code and switching to Lasso LR helped immensely. Regardless, this hackathon ended up being a valuable experience on choosing, training and testing ML models, and will forever be remembered as my first hackathon win :D.
