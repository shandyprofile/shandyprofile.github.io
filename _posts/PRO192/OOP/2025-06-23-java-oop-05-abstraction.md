---
title: 'IV. Abstraction'
description: >-
  Data abstraction is the process of hiding certain details and showing only essential information to the user.
  Abstraction can be achieved with either abstract classes or interfaces (which you will learn more about in the next chapter).
author: [shandy]
date: 2025-06-23
updateDate:
categories: [(Java) Object-oriented programming, (Java) OOP]
tags: [(Java) OOP]
sort_index: 204
# pin: true
# media_subpath: '/posts/02'
---

## A. Abstraction
### 1. What is an Abstract Class?
An abstract class in Java is a class that cannot be instantiated on its own and is meant to be extended by other classes.

It may include:
- Abstract methods (methods without a body)
- Non-abstract methods (with implementation)
- Fields and constructors
- Static and final methods

It provides a base for subclasses to build upon.

### 2. Declaring an Abstract Class
```java
abstract class Animal {
    abstract void speak(); // abstract method

    void breathe() {
        System.out.println("Breathing...");
    }
}
```
> Any class with at least one abstract method must be declared abstract.

### 3. Creating a Subclass
```java
class Dog extends Animal {
    @Override
    void speak() {
        System.out.println("Bark");
    }
}
```
> Subclasses must implement all abstract methods, unless they are also declared abstract.

## 4. Key Features

| Feature                | Abstract Class                           |
| ---------------------- | ---------------------------------------- |
| Can be instantiated    | No                                       |
| Can have constructors  | Yes                                      |
| Can contain fields     | Yes                                      |
| Can have method bodies | Yes (both abstract and concrete methods) |
| Supports inheritance   | Single class inheritance                 |

### 5. Use Case
Use an abstract class when:
- You want to provide common functionality to subclasses
- You want to partially define a class and force subclasses to complete it

### 6. Example: Template Pattern
```java
abstract class Payment {
    void process() {
        authenticate();
        pay();
        receipt();
    }

    void authenticate() {
        System.out.println("Authenticating...");
    }

    abstract void pay();

    void receipt() {
        System.out.println("Transaction complete.");
    }
}

class CreditCardPayment extends Payment {
    void pay() {
        System.out.println("Paid by credit card");
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Payment payment = new CreditCardPayment();
        payment.process();
    }
}
```

> This is an example of the Template Method Pattern.

### 7. Abstract Class vs Interface

| Feature              | Abstract Class              | Interface                              |
| -------------------- | --------------------------- | -------------------------------------- |
| Multiple inheritance | No                          | Yes                                    |
| Method types         | Both abstract and concrete  | Abstract, default, static              |
| Fields               | Variables allowed           | Only constants (`public static final`) |
| Constructors         | Yes                         | No                                     |
| Use case             | Shared base with some logic | Common capability contract             |

### 8. Abstract vs Concrete Class

| Type           | Can instantiate? | Has abstract methods? | Has method bodies? |
| -------------- | ---------------- | --------------------- | ------------------ |
| Abstract class | No               | Optional              | Yes                |
| Concrete class | Yes              | No                    | Yes                |

## B. Interface
### 1. What is an Interface?
An interface in Java is a reference type, similar to a class, that can contain:
- Abstract method signatures (without implementation)
- Default methods (since Java 8)
- Static methods
- Constants (implicitly public, static, and final)

Interfaces are used to define a contract that implementing classes must follow.

### 2. Why Use Interfaces?
- To achieve abstraction and multiple inheritance
- To define a standard set of behaviors across unrelated classes
- To support loose coupling and flexible architecture

### 3. Declaring an Interface
```java
public interface Animal {
    void speak(); // abstract method
}
```

### 4. Implementing an Interface
```java
public class Dog implements Animal {
    public void speak() {
        System.out.println("Bark");
    }
}
```

> A class must implement all methods declared in the interface, unless the class is abstract.

### 5. Multiple Interfaces
A class can implement multiple interfaces, unlike class inheritance.

```java
interface Flyable {
    void fly();
}

interface Swimmable {
    void swim();
}

class Duck implements Flyable, Swimmable {
    public void fly() {
        System.out.println("Duck flying");
    }

    public void swim() {
        System.out.println("Duck swimming");
    }
}
```

### 6. Interface Features by Java Version

| Feature          | Available Since | Description                                     |
| ---------------- | --------------- | ----------------------------------------------- |
| Abstract methods | Java 1          | Method signatures without body                  |
| Default methods  | Java 8          | Methods with body; allow backward compatibility |
| Static methods   | Java 8          | Utility methods within interfaces               |
| Private methods  | Java 9          | Helper methods inside default/static methods    |

### 7. Default Methods
```java
interface Logger {
    default void log(String msg) {
        System.out.println("LOG: " + msg);
    }
}
```
> A class implementing Logger can use or override log().

### 8. Constants in Interfaces
All fields in interfaces are implicitly:
- public
- static
- final

```java
interface Config {
    int MAX_USERS = 100;
}
```

### 9. Interface vs Abstract Class

| Feature     | Interface                   | Abstract Class               |
| ----------- | --------------------------- | ---------------------------- |
| Inheritance | Multiple (via `implements`) | Single (via `extends`)       |
| Methods     | Abstract, default, static   | Abstract and concrete        |
| Fields      | Constants only              | Variables and constants      |
| Constructor | No                          | Yes                          |
| Use Case    | Capability/Contract         | Common base class with logic |

### 10. Real-world Example
```java
interface Payment {
    void pay(double amount);
}

class CreditCardPayment implements Payment {
    public void pay(double amount) {
        System.out.println("Paid $" + amount + " by credit card");
    }
}

class PayPalPayment implements Payment {
    public void pay(double amount) {
        System.out.println("Paid $" + amount + " using PayPal");
    }
}

class Checkout {
    void completePayment(Payment method, double amount) {
        method.pay(amount); // Polymorphism via interface
    }
}
```