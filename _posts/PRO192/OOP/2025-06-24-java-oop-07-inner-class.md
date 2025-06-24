---
title: 'Inner Classes'
description: >-
  Data abstraction is the process of hiding certain details and showing only essential information to the user.
  Abstraction can be achieved with either abstract classes or interfaces (which you will learn more about in the next chapter).
author: [shandy]
date: 2025-06-23
updateDate:
categories: [(Java) Object-oriented programming, (Java) OOP]
tags: [(Java) OOP]
sort_index: 7
# pin: true
# media_subpath: '/posts/02'
---

## A. Inner Classes in Java
### 1. What is an Inner Class?
An inner class is a class that is defined within another class. It is logically associated with its enclosing class and can access its private members.

Java supports several types of inner classes:
- Non-static (regular) inner classes
- Static nested classes
- Local classes (defined within a method)
- Anonymous inner classes

### 2. Why Use Inner Classes?
- To logically group classes that are used only in one place
- To encapsulate helper functionality
- To improve readability and maintainability
- To have access to members of the enclosing class

### 3. Types of Inner Classes
**a. Member Inner Class (Non-static)**

- Defined inside a class but outside any method, and not static.
```java
class Outer {
    private String message = "Hello from Outer";

    class Inner {
        void showMessage() {
            System.out.println(message); // can access private field
        }
    }
}
```

- Usage:
```java
Outer outer = new Outer();
Outer.Inner inner = outer.new Inner();
inner.showMessage();
```

**b. Static Nested Class**

Declared as static, meaning it does not require an instance of the outer class.
```java
class Outer {
    static class StaticInner {
        void display() {
            System.out.println("Inside static nested class");
        }
    }
}
```

Usage:
```java
Outer.StaticInner inner = new Outer.StaticInner();
inner.display();
```

**c. Local Inner Class**

Declared within a method and can access local variables if they are effectively final.

```java
class Outer {
    void display() {
        int number = 10;

        class LocalInner {
            void show() {
                System.out.println("Local number: " + number);
            }
        }

        LocalInner inner = new LocalInner();
        inner.show();
    }
}
```
**d. Anonymous Inner Class**
A class without a name, used to create one-time implementations, typically of interfaces or abstract classes.

```java
abstract class Animal {
    abstract void speak();
}

public class Test {
    public static void main(String[] args) {
        Animal dog = new Animal() {
            void speak() {
                System.out.println("Bark!");
            }
        };
        dog.speak();
    }
}
```
Used widely in event handling, threading, and GUI programming.

### 4. Access Rules
- Non-static inner classes can access all members (even private) of the outer class.
- Static nested classes can only access static members of the outer class.
- Inner classes can shadow variables from outer classes (though not recommended).