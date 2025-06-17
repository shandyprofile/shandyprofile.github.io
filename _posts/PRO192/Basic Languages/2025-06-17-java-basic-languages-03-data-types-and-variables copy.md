---
title: Data Types and Variables
description: >-
  In Java, a variable is a container that holds data during the execution of a program. A data type specifies the type of value a variable can hold.
author: [shandy]
date: 2025-06-17
updateDate: 
categories: [(Java) Object-oriented programming, (Java) Basic Language]
tags: [(Java) Basic Language]
sort_index: 3
# pin: true
# media_subpath: '/posts/02'
---

## 1. Primitive Data Types
Java has 8 primitive data types:

| Type      | Size    | Description                   | Example                |
| --------- | ------- | ----------------------------- | ---------------------- |
| `byte`    | 1 byte  | Whole number (small range)    | `byte a = 100;`        |
| `short`   | 2 bytes | Whole number                  | `short b = 500;`       |
| `int`     | 4 bytes | Whole number (default)        | `int x = 1000;`        |
| `long`    | 8 bytes | Very large whole number       | `long l = 900000000L;` |
| `float`   | 4 bytes | Decimal number (less precise) | `float f = 3.14f;`     |
| `double`  | 8 bytes | Decimal number (more precise) | `double d = 3.14159;`  |
| `char`    | 2 bytes | A single character            | `char c = 'A';`        |
| `boolean` | 1 bit   | true or false                 | `boolean b = true;`    |


## 2. Reference Data Types
These store references to objects:
- Arrays
- Strings
- Classes
- Interfaces

```java
String name = "John";
int[] scores = {90, 85, 78};
```
## 3. Variable Declaration
You must declare a variable before using it:

```java
// <dataType> <variableName> [assign = <value>];
int age;          // Declaration
age = 25;         // Initialization
```
You can also do both at once:

```java
int age = 25;
```

## 4. Naming Rules `<variableName>`
- Must start with a letter, _ or $.
- Can contain letters and digits.
- Case-sensitive (age ≠ Age).
- Use camelCase for convention: `studentScore`.

Code Demo
```java
public class DataTypeDemo {
    public static void main(String[] args) {
        // Primitive types
        int age = 30;
        double salary = 45000.50;
        char grade = 'A';
        boolean isEmployed = true;

        // Reference types
        String name = "Alice";
        int[] scores = {80, 85, 90};

        // Output
        System.out.println("Name: " + name);
        System.out.println("Age: " + age);
        System.out.println("Salary: $" + salary);
        System.out.println("Grade: " + grade);
        System.out.println("Is Employed: " + isEmployed);
        System.out.println("Scores: " + scores[0] + ", " + scores[1] + ", " + scores[2]);
    }
}
```

**Output:**
```console
Name: Alice
Age: 30
Salary: $45000.5
Grade: A
Is Employed: true
Scores: 80, 85, 90
```

