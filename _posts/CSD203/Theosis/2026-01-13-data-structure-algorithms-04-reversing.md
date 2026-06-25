---
title: Reversing
description: >-
  This chapter introduces the Python programming language, focusing on basic data types and operators. It explains how the Python interpreter works, how variables and objects are created, and the difference between mutable and immutable types. Students also learn about common built-in data structures such as lists, tuples, sets, and dictionaries, as well as how to use arithmetic, logical, and comparison operators in Python.
author: [shandy]
date: 2026-01-13
categories: [(Python) Data Structure and Algorithms, Theosis]
tags: [(Python) Data Structure and Algorithms]
sort_index: 104

# pin: true
# media_subpath: '/posts/01'
---

## Examples:

### Problem 1: Fibonacci 

The Fibonacci sequence is a series where each number is the sum of the two preceding ones, typically starting with 0 and 1. 

- **Recursive (Simple but Inefficient):** 

This follows the mathematical definition but is slow for large n due to redundant calculations, running in O(2^n) time.

```python
def fibonacci_recursive(n):
    if n <= 1:
        return n
    return fibonacci_recursive(n - 1) + fibonacci_recursive(n - 2)
```

- **Memoization (Optimized Recursion):**

Stores previously computed values to avoid recalculation, drastically improving performance to O(n) time.

```python
from functools import lru_cache

@lru_cache(maxsize=None)
def fibonacci_memoized(n):
    if n <= 1:
        return n
    return fibonacci_memoized(n - 1) + fibonacci_memoized(n - 2)
```

- **Common Implementation Methods**

Iterative (Most Efficient for Small to Medium 
): This method uses a loop and two variables to track the sequence, running in O(n) time with O(1) space.

```python
def fibonacci_iterative(n):
    a, b = 0, 1
    for _ in range(n):
        print(a, end=' ')
        a, b = b, a + b
```

- **Generators (Memory Efficient):** 

Yields numbers one at a time, which is useful for processing infinite sequences without storing them all in memory.

```python
def fibonacci_generator(n):
    a, b = 0, 1
    for _ in range(n):
        yield a
        a, b = b, a + b
```