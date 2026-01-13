---
title: House Price Prediction – Business Understanding
description: >-
  Practice for Python Primer such as Parking System Programming Assignments
author: [shandy]
date: 2025-09-05
categories: [Data Sciences, (PBL) House Price Prediction]
tags: [Data Sciences - House Price Prediction]
sort_index: 401
# pin: true
# media_subpath: '/posts/01'
---

## I. Analytic Approach (Data-Oriented Thinking)
### 1. Purpose of the Analytic Approach

Once the business problem is clearly understood, the next step is to determine how data analysis can answer the question.

The analytic approach involves:
- Clarifying the type of question being asked
- Selecting the most appropriate analytical method

### 2. Mapping the Question to an Analytic Approach

The refined analytical question is:
- “Given the characteristics of a house, what price should we estimate?”

This question requires:
- A numerical output
- Prediction based on historical patterns

> This is a **Regression Problem**

### 3. Selecting the Type of Analysis

| Question Type           | Analytic Approach    |
| ----------------------- | -------------------- |
| Predict a numeric value | Regression           |
| Yes / No decision       | Classification       |
| Discover patterns       | Clustering           |
| Describe trends         | Descriptive Analysis |

For house price prediction:
- **Regression analysis** is the most appropriate
- Machine Learning may be used to capture complex relationships

### 4. Why Machine Learning May Be Used

Machine Learning can:
- Identify non-linear relationships between features and price
- Improve prediction accuracy compared to simple rules

However:
- ML is a tool, not the objective
- The choice of model comes after this step

## II. Comparison of the Two Approaches

| Problem Approach           | Analytic Approach         |
| -------------------------- | ------------------------- |
| Focuses on goals and value | Focuses on methods        |
| Business-driven            | Data-driven               |
| Defines what success means | Defines how to measure it |
| No models mentioned        | Model types identified    |

## III. Analysis Design

### 1. Type of Data Science Problem

| Business Need                    | Analytics Interpretation           |
| -------------------------------- | ---------------------------------- |
| Estimate a numerical house price | Predict a continuous numeric value |

> This is **Supervised Learning – Regression**

### 2. Frome **Input** to **Output Mapping**

| Role       | Business Meaning    | Data Representation                     |
| ---------- | ------------------- | --------------------------------------- |
| Input (X)  | Property attributes | Area, location, rooms, year built, etc. |
| Output (y) | House value         | Sale price                              |

Mathematically:

$$Price=f(Property,Location,Market)$$

### 3. What Is the Model Expected to Do?

The model must:
- Learn the relationship between house characteristics and historical prices
- Generalize to unseen houses
- Produce a numeric price estimate

> **Not:**
> - Classify houses
> - Rank houses
> - Recommend houses

### 4. Success Metrics (Analytics View)

Business Success Criteria: **“Average prediction error below an acceptable threshold”**

Mapping:
| Business Concern             | Analytics Metric               |
| ---------------------------- | ------------------------------ |
| How far off is the estimate? | MAE (Mean Absolute Error)      |
| Penalize large mistakes      | RMSE (Root Mean Squared Error) |
| Relative performance         | R² score                       |

> - Primary metric: MAE
> - Secondary: RMSE

### 5. Baseline Definition

Business wants to know:
**“Is this better than current practice?”**

Baseline:
- Mean house price
- Or price per m² × area

| Model            | Purpose                |
| ---------------- | ---------------------- |
| Simple heuristic | Business benchmark     |
| ML model         | Value-added comparison |


### 6. Output Form

| Field                          | Description           |
| ------------------------------ | --------------------- |
| predicted_price                | Estimated house value |
| confidence_interval (optional) | Uncertainty           |

### 7. Deployment Implication (from Analytics)

- User inputs numeric & categorical features
- System returns a number
- UI must support structured input

### 8. Analytic Design Summary

This project is a supervised regression problem where historical house transaction data will be used to train a model that predicts continuous house prices from property, location, and market features. The solution will be evaluated primarily using MAE and RMSE against a baseline pricing heuristic.