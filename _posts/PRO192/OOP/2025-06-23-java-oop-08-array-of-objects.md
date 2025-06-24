---
title: 'Array of Objects'
description: >-
  In Java, an array of objects is simply an array where each element is a reference to an object. Instead of holding primitive types (int, char, etc.), the array holds instances of classes.
author: [shandy]
date: 2025-06-23
updateDate:
categories: [(Java) Object-oriented programming, (Java) OOP]
tags: [(Java) OOP]
sort_index: 8
# pin: true
# media_subpath: '/posts/02'
---

## 1. What is an Array of Objects?
- In Java, an array of objects is simply an array where each element is a reference to an object.
- Instead of holding primitive types (int, char, etc.), the array holds instances of classes.

## 2. Declaration and Initialization
**a. Declare an array of a class type:**
```java
ClassName[] arrayName;
```

**b. Create the array (allocate memory for references):**
```java
arrayName = new ClassName[size];
```

**c. Create and assign actual objects:**
```java
arrayName[0] = new ClassName();
```

**Example:**
```java
class Student {
    String name;
    int age;

    Student(String n, int a) {
        name = n;
        age = a;
    }

    void show() {
        System.out.println(name + " is " + age + " years old.");
    }
}

public class Main {
    public static void main(String[] args) {
        Student[] students = new Student[3];

        students[0] = new Student("Alice", 20);
        students[1] = new Student("Bob", 21);
        students[2] = new Student("Charlie", 19);

        for (Student s : students) {
            s.show();
        }
    }
}
```

## 3. Accessing Elements
You can access or update any object in the array using indices:

```java
students[1].name = "Bobby";
students[1].show(); // Updated object
```

## 4. Use Case: Storing Related Objects
Imagine a library system storing multiple Book objects:

```java
Book[] books = new Book[10];
```
> This allows you to manage multiple books with shared logic and structure.

## 5. Common Operations on Object Arrays

| Operation          | Example                              |
| ------------------ | ------------------------------------ |
| Create array       | `Employee[] emps = new Employee[5];` |
| Assign object      | `emps[0] = new Employee(...);`       |
| Access property    | `emps[0].getName();`                 |
| Iterate over array | `for (Employee e : emps) {...}`      |

## 6. Important Notes
- The array itself holds references, not the actual objects.
- If you don't initialize each element with new, accessing it will throw a NullPointerException.
- Arrays are fixed in size. Use ArrayList if you need a resizable collection.

## 7. Comparison with Array of Primitive Types

| Feature        | Primitive Array       | Object Array               |
| -------------- | --------------------- | -------------------------- |
| Type           | `int[]`, `double[]`   | `Student[]`, `Book[]`      |
| Stores         | Data values           | Object references          |
| Initialization | `new int[5]`          | `new Student[5]` + objects |
| Flexibility    | Limited to data types | Full OOP support           |