---
title: Python Primer 1 – Types and Operators
description: >-
  This chapter introduces the Python programming language, focusing on basic data types and operators. It explains how the Python interpreter works, how variables and objects are created, and the difference between mutable and immutable types. Students also learn about common built-in data structures such as lists, tuples, sets, and dictionaries, as well as how to use arithmetic, logical, and comparison operators in Python.
author: [shandy]
date: 2026-01-06
categories: [(Python) Data Structure and Algorithms, Theosis]
tags: [(Python) Data Structure and Algorithms]
sort_index: 101
# pin: true
# media_subpath: '/posts/01'
---

## 1. What Is Python? How Does the Python Interpreter Work?

- Python as an Interpreted Language
- Python is an interpreted language, which means:
  - Code is executed line by line
  - There is no separate compilation step like in C or C++

- The Python interpreter:
  - Receives a command
  - Evaluates the command
  - Displays the result immediately
  - Python Source Files

- Python programs are stored in text files with the .py extension
- Example:

```bash
demo.py
```

## 2. A Simple Python Program

Example:

```
print("Hello, Python!")
```

Explanation:
- print is a built-in function
- "Hello, Python!" is a string object (str)
- The interpreter prints the string to the screen

## 3. Objects in Python

- Python is an object-oriented language.
> Everything in Python is an object

- Common built-in classes:

  - int → integers
  - float → floating-point numbers
  - str → strings

Example:

```python
x = 10        # int
y = 3.14      # float
name = "AI"   # str
```

## 4. Identifiers and Assignment

- Assignment Statement
- temperature = 98.6
- Meaning:
  - temperature is an identifier (variable name)
  - 98.6 is a float object = binds the name to the object

> Python assigns references, not copies of values.

## 5. Identifier Rules

Case-sensitive:

- temp != Temp

Can contain:

- Letters
- Numbers
- Underscores (_)
- Cannot start with a number
- Cannot use reserved keywords (e.g., if, for, class)

## 6. Dynamic Typing

Python uses dynamic typing:

- No need to declare variable types
- A variable can reference objects of different types at different times

Example:

```python
x = 10        # int
x = "hello"   # str
```

> Variables do not have types; objects do

## 7. Objects and Constructors

Creating Objects (Instantiation)

**General syntax:**

```python
obj = ClassName()
```

Examples:

```python
numbers = list()
value = int()
```

Some objects are created using literals:

```python
a = 10        # int
b = 3.14      # float
c = "text"    # str
```

## 8. Functions vs. Methods

- Functions: sorted(data)
- Methods: data.sort()

Difference:

- A function takes the object as a parameter
- A method is called directly on an object using the dot (.)

## 9. Immutable and Mutable Types

- Immutable Types (cannot be changed)
- int, float, bool, str, tuple
- Mutable Types (can be changed)
- list, set, dict

Example:

```python
x = 10
x = x + 1   # creates a new object
```

### 9.1. Boolean Type (bool)

Only two Boolean values:
- True
- False

Type conversion:

```python
bool(0)        # False
bool(5)        # True
bool("")       # False
bool("hi")     # True
```

### 9.2. Integer Type (int)

Supports integers of arbitrary size

Type conversion:

```python
int(3.14)   # 3
int(-3.9)   # -3
int("137")  # 137
```

### 9.3. Floating-Point Type (float)

Examples:

```python
2.0
6.022e23
```

Type conversion:

```python
float(2)       # 2.0
float("3.14")  # 3.14
```

### 9.4. Lists

- Ordered sequence
- Mutable
- Indexed from 0

Example:

```python
colors = ["red", "green", "blue"]
colors[0]   # "red"
```

Creating lists:

```python
list()
list("hello")  # ['h', 'e', 'l', 'l', 'o']
```

### 9.5. Tuples

Similar to lists but immutable

Examples:

```python
t = (1, 2, 3)
single = (17,)
```

### 9.6. Strings (str)

```text
"hello"
'hello'
"""multi
line
string"""
```

16. Sets

- Unordered
- No duplicate elements

Example:

```python
s = {"red", "green", "blue"}
empty = set()
```

> {} creates an empty dictionary, not a set

### 9.7. Dictionaries (dict)

- Key–value mapping

Example:

```python
lang = {"ga": "Irish", "de": "German"}
```

Alternative:

```text
dict([("ga", "Irish"), ("de", "German")])
```

## 10. Expressions and Operators

Operators behave differently depending on operand types:

```python
3 + 4        # 7
"a" + "b"    # "ab"
```

### 10.1. Logical Operators

- and, or, not
- Use short-circuit evaluation

Example:

```python
False and func()   # func() is not executed
```

### 10.2. Equality and Identity

a == b   # value equality
a is b   # same object in memory

### 10.3. Arithmetic Operators

- / => floating-point division
- // => integer division (truncates result)

Example:

```python
5 / 2   # 2.5
5 // 2  # 2
```

### 10.3. Bitwise Operators

- &, |, ^, <<, >>

### 10.4. Sequence Operators

Supported by str, list, tuple:
- +
- *
- in

### 10.5. Sequence Comparisons

Compared lexicographically (left to right):

```python
[5, 6, 9] < [5, 7]   # True
```

### 10.6. Set Operators

- Union |
- Intersection &
- Difference -
- Symmetric difference ^

### 10.7. Dictionary Operators

- in checks for keys, not values:

```python
"ga" in lang
```

### 10.8. Operator Precedence

- Operators have different precedence levels
- Use parentheses () to avoid ambiguity
