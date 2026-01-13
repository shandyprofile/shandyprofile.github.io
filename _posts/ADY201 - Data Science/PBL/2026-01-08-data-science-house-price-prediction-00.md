---
title: House Price Prediction – Overview
description: >-
  Business Understanding defines the real-world problem that the Data Science project aims to solve. In the house price prediction case, the goal is to help real estate agents and customers estimate property prices more quickly and consistently using historical data. This step identifies the business goal, stakeholders, and success criteria, ensuring that the project focuses on supporting pricing decisions rather than just building a technical model.
author: [shandy]
date: 2025-09-05
categories: [Data Sciences, (PBL) House Price Prediction]
tags: [Data Sciences - House Price Prediction]
sort_index: 400
# pin: true
# media_subpath: '/posts/01'
---

# Project: House Price Prediction

The Business Understanding step is the foundation of the entire Data Science project. Its purpose is to clearly define what problem the business is trying to solve and why it matters, before any data or models are considered.

In the case of house price prediction, the business context is the real estate market, where property prices are often estimated manually by agents based on experience and market intuition. This manual approach can lead to inconsistent prices, slow response times, and reduced customer trust, especially when large numbers of properties need to be evaluated.

The company’s main goal is to support real estate agents and customers by providing a reliable and consistent way to estimate house prices using historical data. The system is not intended to replace human experts, but to serve as a data-driven reference that helps improve pricing decisions.

To achieve this goal, several business objectives are defined:

Reduce the time required to estimate house prices

Improve consistency between different agents’ price estimates

Increase transparency and trust in pricing decisions

Several stakeholders are involved in this problem. Real estate agents use the estimated prices during negotiations, managers monitor pricing strategy, customers rely on the prices to make buying decisions, and data analysts are responsible for building the solution.

The project will be considered successful if the estimated prices are accurate enough to be useful, are generated quickly, and can be explained in a simple way to non-technical users. At the same time, there are constraints, such as using only historical data and building only a prototype rather than a full commercial system.

In summary, the Business Understanding step ensures that the project focuses on solving a real business problem—providing better house price estimates—rather than simply building a technical model. It defines the purpose, scope, and success criteria that guide all later stages of the Data Science process

## Step by step to start for Data Science Projects

![](/assets/img/2026-01-13-16-00-15.png)

| **Step** | **Step Name**              | **Main Objective**                | **Key Questions Students Must Answer**                        | **Deliverable**                        |
| -------- | -------------------------- | --------------------------------- | ------------------------------------------------------------- | -------------------------------------- |
| 1        | **Business Understanding** | Understand the real-world problem | Who needs house price predictions? What are they used for?    | Business Problem Statement             |
| 2        | **Analytic Approach**      | Identify the type of problem      | Is this a regression or a classification problem?             | Analytic Design (problem type, metric) |
| 3        | **Data Requirements**      | Define required data              | Which features affect house prices?                           | Data Requirement Table                 |
| 4        | **Data Collection**        | Collect the data                  | Where does the data come from? Is it sufficient and reliable? | Dataset + Data Source Report           |
| 5        | **Data Understanding**     | Understand the initial data       | Are there missing values, outliers, or bias?                  | EDA Notebook                           |
| 6        | **Data Preparation**       | Clean and transform the data      | How should missing data be handled? Are new features created? | Data Preparation Notebook              |
| 7        | **Modeling**               | Build the model                   | Which model is appropriate? What is the baseline?             | Modeling Notebook                      |
| 8        | **Evaluation**             | Evaluate the results              | Are RMSE / MAE acceptable?                                    | Evaluation Report                      |
| 9        | **Deployment (Mock)**      | Simulate deployment               | How will users enter data to get predictions?                 | Demo / App / Mock API                  |
| 10       | **Feedback & Iteration**   | Improve and refine the solution   | If the results are not good enough, what should be improved?  | Reflection Report                      |


