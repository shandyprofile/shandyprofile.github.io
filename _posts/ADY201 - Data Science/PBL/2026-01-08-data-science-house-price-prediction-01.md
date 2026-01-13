---
title: House Price Prediction – Business Understanding
description: >-
  Business Understanding defines the real-world problem that the Data Science project aims to solve. In the house price prediction case, the goal is to help real estate agents and customers estimate property prices more quickly and consistently using historical data. This step identifies the business goal, stakeholders, and success criteria, ensuring that the project focuses on supporting pricing decisions rather than just building a technical model.
author: [shandy]
date: 2025-09-05
categories: [Data Sciences, (PBL) House Price Prediction]
tags: [Data Sciences - House Price Prediction]
sort_index: 401
# pin: true
# media_subpath: '/posts/01'
---

## I. Problem Approach (Business-Oriented Thinking)

![](/assets/img/2026-01-13-16-00-52.png)

### 1. Why start with the Problem Approach?

Data Science methodology begins by **seeking clarification of the business problem** in order to achieve a solid business understanding.

This step is placed at the beginning because:
- A clearly defined problem determines **what data is needed**
- It directly influences **how the analysis will be performed**
- It prevents solving *the wrong problem correctly*

Having a **clearly defined question** is vital, because it ultimately directs the analytic approach that will be required.

### 2. Understanding the GOAL

Establishing a clearly defined question starts with understanding the **GOAL** of the person asking the question.

In the House Price Prediction case:
- **Who is asking the question?** => A real estate company / property platform
- **What is the goal?** => To estimate a reasonable house price quickly and consistently

> **Goal:** Support pricing decisions by providing a reliable house price estimation based on historical data.

### 3. Identifying Supporting Objectives

Once the goal is clarified, the next step is to identify the **objectives that support this goal**.

Examples of objectives:
- Reduce the time needed to estimate house prices
- Minimize pricing inconsistency among agents
- Provide a data-driven reference price for customers

Objectives are **measurable and actionable**, unlike the high-level goal.

### 4. Identifying Stakeholders

Depending on the problem, different stakeholders must be involved to clarify requirements and constraints.

| Stakeholder        | Role                     | Interest          |
| ------------------ | ------------------------ | ----------------- |
| Real estate agents | Use price estimates      | Speed & usability |
| Company managers   | Control pricing strategy | Consistency       |
| Customers          | View estimated prices    | Transparency      |
| Data team          | Build the system         | Data quality      |

### 5. Business Problem Statement (House Price)

Real estate agents currently estimate house prices manually based on experience, which leads to inconsistent pricing and longer response times. The company needs a data-driven approach to support faster and more reliable price estimation.

> **Case Study: Identifying the business requirements**

### Case Analysis – Business Requirements (Problem Approach)

> **Business Question: What is the best way to estimate house prices accurately and consistently using historical data?**

From the problem analysis, the business requires:
- A method to estimate house prices based on property characteristics
- A solution that supports decision-making (not replacing human judgment)
- Results that are explainable to customers

**Proposed Business-Level Solution (No Models Yet)**

Build a data-driven system that estimates house prices using historical transaction data and property features to support pricing decisions.

> Note: At this stage:
> - No machine learning algorithm is mentioned
> - No evaluation metric is discussed
> - Focus remains on business value

## II. BUSINESS PROBLEM STATEMENT (*Important*)
(Professional Standard Template for Data Science Projects)

### 1. Business Context
*(What is happening in the business environment?)*

*Briefly describe the business domain and the current situation that motivates this project. Focus on the real-world context rather than technical details.*

> Example (House Price):
The real estate market experiences frequent price fluctuations, and property valuation is often performed manually by agents based on experience. This process lacks consistency and scalability as the number of listings grows.

### 2. Business Problem
(What specific problem needs to be solved?)

*Clearly state the core business problem. Avoid mentioning data science techniques or models.*

> Example:
The organization does not have a standardized and data-driven method to estimate house prices, resulting in inconsistent pricing and longer response times for customers.

### 3. Business Goal

*(What does the business ultimately want to achieve?)*

*Define the high-level goal the business aims to accomplish.*

> Example:
To support pricing decisions by providing reliable and consistent house price estimates.

### 4. Business Objectives

*(What measurable objectives support the goal?)*

*List concrete objectives that help achieve the business goal.*

> - Reduce the time required for price estimation
> - Improve consistency across price estimates
> - Provide a data-driven reference price

### 5. Stakeholders

*(Who is involved or affected?)*

*Identify key stakeholders and their interests.*

| Stakeholder        | Role                 | Key Interest     |
| ------------------ | -------------------- | ---------------- |
| Real estate agents | Price estimation     | Speed, usability |
| Managers           | Pricing strategy     | Consistency      |
| Customers          | Price transparency   | Trust            |
| Data team          | Solution development | Data quality     |

### 6. Success Criteria

*(How will success be measured?)*

*Define clear and measurable criteria linked to business value.*

> - Average prediction error below an acceptable threshold
> - Price estimates generated within an acceptable time
> - Results that can be explained to non-technical users

### 7. Constraints

*(What limitations must be considered?)*

*Use of historical data only*

> - Prototype-level implementation
> - No real-time market updates

### 8. Assumptions

*(What assumptions are being made?)*

> - Historical transaction data reflects current market trends
> - Relevant property features are available and reliable

### 9. Out of Scope

*(What is explicitly excluded?)*

> - Image-based valuation
> - Fully automated pricing without human review
> - Commercial deployment

### 10. Expected Business Value

*(Why does this project matter?)*

*Describe the anticipated benefits.*

> - Improved pricing efficiency
> - Increased consistency and transparency
> - Better decision support for agents and managers

### 11. Summary

*(One-paragraph executive summary)*

> This project aims to address inconsistencies in house price estimation by developing a data-driven reference system based on historical transaction data. The solution is intended to support faster, more consistent pricing decisions while maintaining human oversight.