---
title: Logic Instructions
description: >-
  Java provides control flow statements that allow your program to make decisions and execute different **blocks of code** based on conditions.
author: [shandy]
date: 2025-06-17
updateDate: 
categories: [(Java) Object-oriented programming, (Java) Basic Language]
tags: [(Java) Basic Language]
sort_index: 105
# pin: true
# media_subpath: '/posts/02'
---

| Statement    | Use for                                            |
| ------------ | -------------------------------------------------- |
| `if`         | Run code if a condition is true                    |
| `if-else`    | Choose between two alternatives                    |
| `if-else if` | Handle multiple conditions                         |
| `switch`     | Match exact values (cleaner than many `if` chains) |

## 1. If Statement
- Syntax:
```java
if (condition) {
    // code to execute if condition is true
}
```
- Example:
```java
int age = 20;
if (age >= 18) {
    System.out.println("You are an adult.");
}
```
## 2. If-else Statement
- Syntax:
Chỉnh sửa
if (condition) {
    // if true
} else {
    // if false
}
- Example:
```java
int number = 7;
if (number % 2 == 0) {
    System.out.println("Even");
} else {
    System.out.println("Odd");
}
```
## 3. If-else if-else chain
- Syntax:
```java
if (condition1) {
    // if condition1 true
} else if (condition2) {
    // if condition2 true
} else {
    // if none true
}
```
- Example:
```java
int score = 85;
if (score >= 90) {
    System.out.println("Grade A");
} else if (score >= 75) {
    System.out.println("Grade B");
} else {
    System.out.println("Grade C");
}
```
## 4. Switch Statement
> Used when you have multiple exact matches for a single variable (usually int, char, String, or enum).

- Syntax:
```java
switch (expression) {
    case value1:
        // code block
        break;
    case value2:
        // code block
        break;
    default:
        // code if no match
}
```
- Example:
```java
int day = 3;
switch (day) {
    case 1:
        System.out.println("Monday");
        break;
    case 2:
        System.out.println("Tuesday");
        break;
    case 3:
        System.out.println("Wednesday");
        break;
    default:
        System.out.println("Other day");
}
```
> **Important Notes:**
> - **break** is used to exit the switch after a case matches.
> - **default** is optional and runs if no cases match.

## Code Demo

```java
public class LogicInstructions {
    public static void main(String[] args) {
        int number = 75;

        // if-else if
        if (number >= 90) {
            System.out.println("Excellent");
        } else if (number >= 60) {
            System.out.println("Good");
        } else {
            System.out.println("Needs Improvement");
        }

        // switch
        String grade;
        switch (number / 10) {
            case 10:
            case 9:
                grade = "A";
                break;
            case 8:
                grade = "B";
                break;
            case 7:
                grade = "C";
                break;
            case 6:
                grade = "D";
                break;
            default:
                grade = "F";
        }
        System.out.println("Grade: " + grade);
    }
}
```
**Output:**

```console
Good
Grade: C
```