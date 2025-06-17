---
title: Operators and Expressions
description: >-
  In Java, **operators** are special symbols used to perform operations on variables and values. An **expression** is a combination of variables, operators, and values that produces a result.
author: [shandy]
date: 2025-06-17
updateDate: 
categories: [(Java) Object-oriented programming, (Java) Basic Language]
tags: [(Java) Basic Language]
sort_index: 4
# pin: true
# media_subpath: '/posts/02'
---

## 1. Arithmetic Operators
Used for basic math operations.

| Operator | Description         | Example |
| -------- | ------------------- | ------- |
| `+`      | Addition            | `a + b` |
| `-`      | Subtraction         | `a - b` |
| `*`      | Multiplication      | `a * b` |
| `/`      | Division            | `a / b` |
| `%`      | Modulus (remainder) | `a % b` |


## 2. Relational / Comparison Operators
Used to compare values.

| Operator | Description      | Example  | Result     |
| -------- | ---------------- | -------- | ---------- |
| `==`     | Equal to         | `a == b` | true/false |
| `!=`     | Not equal to     | `a != b` | true/false |
| `>`      | Greater than     | `a > b`  | true/false |
| `<`      | Less than        | `a < b`  | true/false |
| `>=`     | Greater or equal | `a >= b` | true/false |
| `<=`     | Less or equal    | `a <= b` | true/false |

## 3. Logical Operators
Used to combine multiple conditions.

| Operator | Description | Example           |  
| -------- | ----------- | ----------------- | 
| `&&`     | Logical AND | `a > 0 && b < 10` | 
| `\|\|`         | Logical OR | `a > 0 \|\| b < 10` |
| `!`      | Logical NOT | `!(a > 0)`        | 

## 4. Assignment Operators
Used to assign values.

| Operator | Example  | Equivalent    |
| -------- | -------- | ------------- |
| `=`      | `a = b`  | assign b to a |
| `+=`     | `a += b` | `a = a + b`   |
| `-=`     | `a -= b` | `a = a - b`   |
| `*=`     | `a *= b` | `a = a * b`   |
| `/=`     | `a /= b` | `a = a / b`   |
| `%=`     | `a %= b` | `a = a % b`   |

🔹 5. Unary Operators
Operate on a single operand.

| Operator | Description | Example        |
| -------- | ----------- | -------------- |
| `+`      | Unary plus  | `+a`           |
| `-`      | Unary minus | `-a`           |
| `++`     | Increment   | `a++` or `++a` |
| `--`     | Decrement   | `a--` or `--a` |
| `!`      | Logical NOT | `!true`        |

## 6. Ternary Operator (? Operator)
A shorthand for if-else.

```java
int max = (a > b) ? a : b;
```

## Code Demo
```java
public class OperatorDemo {
    public static void main(String[] args) {
        int a = 10, b = 3;

        // Arithmetic
        System.out.println("Sum: " + (a + b));
        System.out.println("Remainder: " + (a % b));

        // Comparison
        System.out.println("a > b? " + (a > b));

        // Logical
        boolean result = (a > 5) && (b < 5);
        System.out.println("Logical AND: " + result);

        // Assignment
        int c = 5;
        c += 3; // same as c = c + 3
        System.out.println("After += : " + c);

        // Ternary
        int max = (a > b) ? a : b;
        System.out.println("Max: " + max);
    }
}
```
**Output:**
```console
Sum: 13
Remainder: 1
a > b? true
Logical AND: true
After += : 8
Max: 10
```