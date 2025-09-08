---
title: "Systems of Linear: two variables"
description: >-
  Mermaid demo
author: [shandy]
date: 2025-09-05
categories: [Machine Learning, Linear Algebra for Machine Learning]
tags: [Machine Learning, Linear Algebra for Machine Learning]
sort_index: 101
# pin: true
# media_subpath: '/posts/01'
---

## 1. Machine Learning Motivation

-  Many machine learning tasks involve solving systems of equations to find model parameters.

- Example: fitting a line 
$$ y = ax + b $$ 
through data points leads to a system of two-variable equations.

- Understanding systems of equations helps us see how ML models find optimal solutions.

## 2. Systems of Sentences

- Analogy: a sentence is like an equation — it encodes constraints.
- A system of sentences combines multiple constraints, which may be consistent or contradictory.
- Example:
- Sentence 1: “x is even.”
- Sentence 2: “x is odd.” → contradiction (no solution).
- Sentence 2: “x is divisible by 4.” → consistent system (solutions exist).

## 3. Systems of Equations

General form for two equations in two variables:

$$
a_1x + b_1y = c_1
$$
$$
a_2x + b_2y = c_2
$$

Possible outcomes:
- One unique solution.
- No solution (inconsistent).
- Infinitely many solutions (dependent).

## 4. Systems of Equations as Lines

- Each linear equation in two variables represents a line in the plane.
- The solution to the system is the intersection point of the lines.
- Cases:
  - Two lines intersect → one unique solution.
  - Two lines are parallel → no solution.
  - Two lines coincide → infinitely many solutions.

## 5. A Geometric Notion of Singularity

- “Singular” means a collapse of geometric structure.
- For two variables:

  - If the row (or column) vectors are independent, they span an area ≠ 0 → unique solution.

  - If dependent, they collapse into a single line → no solution or infinitely many solutions.

## 6. Singular vs Nonsingular Matrices

- Coefficient matrix:

$$
  A = \begin{bmatrix} a_1 & b_1 \\ a_2 & b_2 \end{bmatrix}
$$

- **Nonsingular** (invertible): 

$$
  A^{-1} \ \text{exists} \ \Rightarrow \ \text{unique solution}
$$


- **Singular** (non-invertible): no inverse → either no solution or infinitely many solutions.

## 7. Linear Dependence and Independence

- If two row (or column) vectors are linearly independent, they span a nonzero area → unique solution.
- If dependent, they lie on the same line → collapse of dimension.
- Examples:

$$(2,4) and (1,2) → dependent.$$  
$$(2,3) and (1,2) → independent. $$

## 8. The Determinant
- For a matrix (2x2):  

$$
  \det(A) = a_1b_2 - a_2b_1
$$  

- Interpretation:  
  - Absolute value is the area of the parallelogram formed by the vectors.  
  - If det(A) = 0 → vectors are dependent → singular matrix.  
  - If det(A) ≠ 0 → independent → unique solution.  
