---
title: '`Final` and `Static` Keyword'
description: >-
  These two keywords are widely used in Java and play important roles in defining behavior for variables, methods, and classes.
author: [shandy]
date: 2025-06-24
updateDate:
categories: [(Java) Object-oriented programming, (Java) OOP]
tags: [(Java) OOP]
sort_index: 206
# pin: true
# media_subpath: '/posts/02'
---


## 1. `final` Keyword
The final keyword is used to declare constants or prevent modification.

### a. Final Variables
- A final variable can only be assigned once.
- It becomes a constant after initialization.

```java
final int MAX_USERS = 100;
// MAX_USERS = 200; // Compile-time error
With Objects:
```

```java
final StringBuilder sb = new StringBuilder("Hi");
sb.append(" there");  // Allowed (modifying object)
sb = new StringBuilder("New"); // Not allowed (reassigning)
```

With Constructors: You can initialize final variables inside constructors.

```java
class Example {
    final int id;

    Example(int id) {
        this.id = id;
    }
}
```

### b. Final Methods

A final method cannot be overridden by subclasses.

```java
class Parent {
    final void show() {
        System.out.println("Final method");
    }
}

class Child extends Parent {
    // void show() {} // Error: cannot override final method
}
```

### c. Final Classes

A final class cannot be extended (no subclasses).

```java
final class MathUtils {
    static int square(int x) {
        return x * x;
    }
}

// class ExtendedUtils extends MathUtils {} // Error
```

## 2. `static` Keyword
The static keyword is used to define class-level members, which belong to the class rather than any object.

### a. Static Variables
- Shared across all instances.
- Created once when the class is loaded.

```java
class Counter {
    static int count = 0;

    Counter() {
        count++;
    }
}
```
Usage:

```    java
Counter a = new Counter();
Counter b = new Counter();
System.out.println(Counter.count); // 2
```

### b. Static Methods
- Can be called without an object.
- Cannot access instance variables or methods directly.

```java
class MathUtils {
    static int add(int a, int b) {
        return a + b;
    }
}
```

Usage:
```java
int sum = MathUtils.add(5, 10);
```

### c. Static Blocks

Used to initialize static variables.

```java
class InitExample {
    static int x;

    static {
        x = 10;
        System.out.println("Static block initialized");
    }
}
```

### d. Static Classes (Nested)
- Only nested classes can be static.

```java
class Outer {
    static class Inner {
        void display() {
            System.out.println("Inside static nested class");
        }
    }
}
```

Usage:

```java
Outer.Inner obj = new Outer.Inner();
obj.display();
```
