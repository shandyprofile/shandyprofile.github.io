---
title: Exception Handling
description: >-
  An exception is an unexpected event that occurs during the execution of a program, disrupting its normal flow.Example: Dividing by zero, accessing an invalid array index, or opening a missing file.
author: [shandy]
date: 2025-06-17
updateDate: 
categories: [(Java) Object-oriented programming, (Java) Basic Language]
tags: [(Java) Basic Language]
sort_index: 108
# pin: true
# media_subpath: '/posts/02'
---

## 1. Types of Exceptions

| Category      | Description                           | Examples                                      |
| ------------- | ------------------------------------- | --------------------------------------------- |
| **Checked**   | Must be handled with `try-catch`      | `IOException`, `SQLException`                 |
| **Unchecked** | Occur at runtime, optional to handle  | `NullPointerException`, `ArithmeticException` |
| **Errors**    | Serious problems, usually not handled | `OutOfMemoryError`, `StackOverflowError`      |

## 2. Try-catch Block
- Syntax:
```java
try {
    // Code that may throw an exception
} catch (ExceptionType name) {
    // Code to handle the exception
}
```
- Example:
```java
public class TryCatchExample {
    public static void main(String[] args) {
        try {
            int result = 10 / 0; // This causes ArithmeticException
            System.out.println("Result: " + result);
        } catch (ArithmeticException e) {
            System.out.println("Error: Cannot divide by zero.");
        }
    }
}
```
- Output:
```
Error: Cannot divide by zero.
```

## 3. Multiple Catch Blocks
You can catch different types of exceptions separately:

```java
try {
    int[] arr = {1, 2, 3};
    System.out.println(arr[5]); // ArrayIndexOutOfBoundsException
} catch (ArithmeticException e) {
    System.out.println("Math error.");
} catch (ArrayIndexOutOfBoundsException e) {
    System.out.println("Invalid array index.");
}
```
> Note: Catch blocks are **priorities** from **top** to **bottom** 


## 4. Finally Block
The finally block is always executed, even if an exception occurs or not. Great for cleanup code (e.g., closing files or scanners).

```java
Scanner sc = new Scanner(System.in);
try {
    System.out.println("Enter a number: ");
    int n = sc.nextInt();
    System.out.println("Number: " + n);
} catch (Exception e) {
    System.out.println("Invalid input.");
} finally {
    sc.close(); // Always runs
    System.out.println("Scanner closed.");
}
```

## 5. Throw Keyword
Use throw to manually raise an exception.

```java
throw new IllegalArgumentException("Negative age not allowed");
```

## 6. Throws Keyword
Used to declare that a method may throw exceptions.

```java
public void readFile() throws IOException {
    // read file here
}
```

## Code demo
```java
public class ExceptionDemo {
    public static int divide(int a, int b) throws ArithmeticException {
        if (b == 0) {
            throw new ArithmeticException("Cannot divide by zero.");
        }
        return a / b;
    }

    public static void main(String[] args) {
        try {
            int result = divide(10, 0);
            System.out.println("Result: " + result);
        } catch (ArithmeticException e) {
            System.out.println("Caught an error: " + e.getMessage());
        } finally {
            System.out.println("Division attempt complete.");
        }
    }
}
```
**Output:**
```console
Caught an error: Cannot divide by zero.
Division attempt complete.
```