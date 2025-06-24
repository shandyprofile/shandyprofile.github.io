---
title: 'III. Polymorphism'
description: >-
  Polymorphism allows objects of different classes to be treated as objects of a common superclass. The correct method is selected at runtime based on the actual object type.
author: [shandy]
date: 2025-06-23
updateDate:
categories: [(Java) Object-oriented programming, (Java) OOP]
tags: [(Java) OOP]
sort_index: 204
# pin: true
# media_subpath: '/posts/02'
---

## Polymorphism in Java

Polymorphism is one of the four core principles of Object-Oriented Programming (OOP), alongside Encapsulation, Inheritance, and Abstraction.

Definition: Polymorphism allows objects of different classes to be treated as objects of a common superclass. The correct method is selected at runtime based on the actual object type.

Polymorphism means "many forms."
In Java, it lets the same method name or object type behave differently depending on context.

Benefits of Polymorphism
- Improves `code flexibility` and `reusability`
- Supports `extensibility` without changing existing code
- Enables `dynamic method` binding (runtime decision-making)

**Types of Polymorphism in Java:**

| Type                      | Description                             | Example                            |
| ------------------------- | --------------------------------------- | ---------------------------------- |
| **Compile-time (Static)** | Method **Overloading**                  | `print(int)` vs `print(String)`    |
| **Runtime (Dynamic)**     | Method **Overriding** using Inheritance | `Animal a = new Dog(); a.speak();` |


## A. Overloading and Overriding
These are two fundamental techniques used to implement polymorphism in Java. While they may sound similar, they are very different in how and when they are applied.

### 1. Method Overloading (Compile-Time Polymorphism)
**Definition:**
Overloading means having multiple methods in the same class with the same name but different parameter lists.

**Characteristics:**
- Happens within the same class.
- Parameters must differ by type, number, or order.
- Return type can be the same or different.
- Happens at compile time.

**Example:**
```java
class Printer {
    void print(String s) {
        System.out.println("String: " + s);
    }

    void print(int n) {
        System.out.println("Integer: " + n);
    }

    void print(String s, int n) {
        System.out.println("String & Integer: " + s + ", " + n);
    }
}
```
**Usage:**
```java
Printer p = new Printer();
p.print("Hello");     // calls print(String)
p.print(42);          // calls print(int)
p.print("Age", 30);   // calls print(String, int)
```

### 2. Method Overriding (Runtime Polymorphism)
**Definition:**
Overriding is when a subclass provides its own version of a method already defined in its superclass.

**Characteristics:**
- Must have same method name, parameters, and return type (or covariant).
- Must be in a subclass.
- The method must be inherited, not private.
- Uses @Override annotation (recommended).
- Happens at runtime.

**Example:**
```java
class Animal {
    void speak() {
        System.out.println("Some animal sound");
    }
}

class Dog extends Animal {
    @Override
    void speak() {
        System.out.println("Bark");
    }
}
```


**Usage:**
```java
Animal a = new Dog();
a.speak(); // Bark — Dog's version overrides Animal's
```

### 3. Comparison Table

| Feature         | Overloading  | Overriding                               |
| --------------- | ------------ | ---------------------------------------- |
| Location        | Same class   | Subclass (inherits from superclass)      |
| Parameters      | Must differ  | Must be identical                        |
| Return type     | Can differ   | Must be same (or covariant)              |
| Timing          | Compile-time | Runtime                                  |
| Access modifier | Can be any   | Cannot reduce visibility                 |
| Static methods  | Yes        | No (static methods are not overridden) |
| Annotation      | No. Optional   | Yes. `@Override` (strongly recommended)     |


> **Notes:**
> - Constructor Overloading is allowed, but constructors cannot be overridden.
> - Static Methods: Can be overloaded, but not overridden (they are class-level, not instance-level).
> - Final Methods: Cannot be overridden.
> - Private Methods: Cannot be overridden (they are not inherited).