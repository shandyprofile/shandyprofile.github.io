---
title: Algorithms Programming
description: >-
  This chapter introduces the Python programming language, focusing on basic data types and operators. It explains how the Python interpreter works, how variables and objects are created, and the difference between mutable and immutable types. Students also learn about common built-in data structures such as lists, tuples, sets, and dictionaries, as well as how to use arithmetic, logical, and comparison operators in Python.
author: [shandy]
date: 2026-01-12
categories: [(Python) Data Structure and Algorithms, Theosis]
tags: [(Python) Data Structure and Algorithms]
sort_index: 103
# pin: true
# media_subpath: '/posts/01'
---

Analysis of Algorithms is a fundamental aspect of computer science that involves evaluating performance of algorithms and programs. Efficiency is measured in terms of time and space.

## Running Time

### Why is Performance of Algorithms Important ?

There are many important things that should be taken care of, like user-friendliness, modularity, security, maintainability, etc. Why worry about performance?  

The answer to this is simple, we can have all the above things only if we have performance. So performance is like currency through which we can buy all the above things.
Another reason for studying performance is - speed is fun!
Why Analysis of Algorithms is important ?
Algorithm analysis is an important part of computational complexity theory, which provides theoretical estimation for the required resources of an algorithm to solve a specific computational problem. Analysis of algorithms is the determination of the amount of time and space resources required to execute it.

To predict the behavior of an algorithm for large inputs (Scalable Software).
It is much more convenient to have simple measures for the efficiency of an algorithm than to implement the algorithm and test the efficiency every time a certain parameter in the underlying computer system changes.
More importantly, by analyzing different algorithms, we can compare them to determine the best one for our purpose.

### Order of Growth

Let f(n) and g(n) be the time taken by two algorithms where n >= 0 and f(n) and g(n) are also greater than equal to 0. A function f(n) is said to be growing faster than g(n) if g(n)/f(n) for n tends to infinity is 0 (or f(n)/g(n) for n tends to infinity is infinity).

```
Example 1:  f(n) = 1000,  g(n) = n + 1
For n > 999, g(n) would always be greater than f(n) because order of growth of g(n) is more than f(n).

Example 2: f(n) = 4n2 ,  g(n) = 2n + 2000
f(n) has higher order of growth as it grows quadratically in terms of input size.
```

![](/assets/img/2026-04-13-11-53-38.png)

![](/assets/img/2026-04-13-11-56-34.png)

![](/assets/img/2026-04-13-11-58-17.png)

## Big O Notation

Big O notation is used to describe the time or space complexity of algorithms. Big-O is a way to express an upper bound of an algorithm’s time or space complexity.

- Describes the asymptotic behavior (order of growth of time or space in terms of input size) of a function, not its exact value.
- Can be used to compare the efficiency of different algorithms or data structures.
- It provides an upper limit on the time taken by an algorithm in terms of the size of the input. We mainly consider the worst case scenario of the algorithm to find its time complexity in terms of Big O

### Big O Definition

Given two functions **f(n)** and **g(n)**, we say that **f(n)** is **O(g(n))** if there exist constants **c > 0** and **n0 >= 0** such that **f(n)** <= **c*g(n)** for all **n >= n0**.

In simpler terms, **f(n)** is **O(g(n))** if **f(n)** grows no faster than c*g(n) for all n >= n0 where c and n0 are constants.

![](/assets/img/2026-04-13-12-02-11.png)

### A Quick Way to find Big O of an Expression

- Ignore the lower order terms and consider only highest order term.
- Ignore the constant associated with the highest order term.

```
Example 1:  f(n) = 3n2 + 2n + 1000Logn +  5000
After ignoring lower order terms, we get the highest order term as 3n2
After ignoring the constant 3, we get n2
Therefore the Big O value of this expression is O(n2)

Example 2 :  f(n) = 3n3 + 2n2 + 5n + 1
Dominant Term: 3n3
Order of Growth: Cubic (n3)
Big O Notation: O(n3)
```

### Common Big-O Notations

Big-O notation is a way to measure the time and space complexity of an algorithm. It describes the upper bound of the complexity in the worst-case scenario. Let’s look into the different types of time complexities:

#### Time Complexity

Imagine a classroom of 100 students in which you gave your pen to one person. You have to find that pen without knowing to whom you gave it. 

Here are some ways to find the pen and what the O order is.

- **O(n2)**: You go and ask the first person in the class if he has the pen. Also, you ask this person about the other 99 people in the classroom if they have that pen and so on. This is what we call O(n2). 
- **O(n)**: Going and asking each student individually is O(n). 
- **O(log n)**: Now I divide the class into two groups, then ask: "Is it on the left side, or the right side of the classroom?" Then I take that group and divide it into two and ask again, and so on. Repeat the process till you are left with one student who has your pen. This is what you mean by O(log n). 

> **Time Complexity Same as Time of Execution**

- Instead of measuring actual time required in executing each statement in the code, Time Complexity considers how many times each statement executes. 
- We measure rate of growth over time with respect to the inputs taken during the program execution.

**Example 1:** Consider the below simple code to print "Hello World"

```python
print("Hello World")
```

> The time complexity is constant: O(1)

**Example 2:** 

```python
n = 8
for i in range(1, n + 1):
    print("Hello World !!!")
```

Output:

```
Hello World !!!
Hello World !!!
Hello World !!!
Hello World !!!
Hello World !!!
Hello World !!!
Hello World !!!
Hello World !!!
```

> Time Complexity: In the above code “Hello World !!!” is printed only n times. The time complexity is linear: **O(n)**


#### Find The Time Complexity

**Q1. Find the sum of all elements of a list/array**

```python
def listSum(arr, n):
    sum = 0
    for i in range(n):
        sum = sum + arr[i]
    return sum

arr = [5, 6, 1, 2]
n = len(arr)
print(listSum(arr, n))
```

```
Tsum = 1 + 2 * (n+1) + 2 * n + 1 = 4n + 4 =C1 * n + C2 = O(n)
```

> The time complexity of the above code is **O(n)**

**Q2. Find the sum of all elements of a matrix**

For this one, the complexity is a polynomial equation (quadratic equation for a square matrix)
- Matrix of size n*n => Tsum = a.n2 + b.n + c
- Since Tsum is in order of n2, therefore Time Complexity = O(n2).

```python
n = 3
m = 3
arr = [[3, 2, 7], [2, 6, 8], [5, 1, 9]]
sum = 0

# Iterating over all 1-D arrays in 2-D array
for i in range(n):
    
    # Printing all elements in ith 1-D array
    for j in range(m):
        
        # Printing jth element of ith row
        sum += arr[i][j]

print(sum)
```

> Time Complexity = ?

