---
title: 'III. Polymorphism'
description: >-
  Polymorphism allows objects of different classes to be treated as objects of a common superclass. The correct method is selected at runtime based on the actual object type.
author: [shandy]
date: 2025-06-23
updateDate:
categories: [(Java) Object-oriented programming, (Java) OOP]
tags: [(Java) OOP]
sort_index: 4
# pin: true
# media_subpath: '/posts/02'
---

# Polymorphism in Java

Polymorphism is one of the four core principles of Object-Oriented Programming (OOP), alongside Encapsulation, Inheritance, and Abstraction.

Definition: Polymorphism allows objects of different classes to be treated as objects of a common superclass. The correct method is selected at runtime based on the actual object type.

Benefits of Polymorphism
- Improves `code flexibility` and `reusability`
- Supports `extensibility` without changing existing code
- Enables `dynamic method` binding (runtime decision-making)

**Types of Polymorphism in Java:**

| Type                      | Description                             | Example                            |
| ------------------------- | --------------------------------------- | ---------------------------------- |
| **Compile-time (Static)** | Method **Overloading**                  | `print(int)` vs `print(String)`    |
| **Runtime (Dynamic)**     | Method **Overriding** using Inheritance | `Animal a = new Dog(); a.speak();` |


## A. Overloading and Overriding in Java
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

## B. Interface in Java
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

## C. Abstract Class in Java
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

### 4. Key Features

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

8. Abstract vs Concrete Class

| Type           | Can instantiate? | Has abstract methods? | Has method bodies? |
| -------------- | ---------------- | --------------------- | ------------------ |
| Abstract class | No               | Optional              | Yes                |
| Concrete class | Yes              | No                    | Yes                |

## D. Inner Classes in Java
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

## E. `final` and `static` Keywords
These two keywords are widely used in Java and play important roles in defining behavior for variables, methods, and classes.

### 1. final Keyword
The final keyword is used to declare constants or prevent modification.

**a. Final Variables**
- A final variable can only be assigned once.
- It becomes a constant after initialization.

```java
final int MAX_USERS = 100;
// MAX_USERS = 200; // ❌ Compile-time error
With Objects:
```

```java
final StringBuilder sb = new StringBuilder("Hi");
sb.append(" there");  // ✅ Allowed (modifying object)
sb = new StringBuilder("New"); // ❌ Not allowed (reassigning)
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

**b. Final Methods**

A final method cannot be overridden by subclasses.

```java
class Parent {
    final void show() {
        System.out.println("Final method");
    }
}

class Child extends Parent {
    // void show() {} // ❌ Error: cannot override final method
}
```

**c. Final Classes**

A final class cannot be extended (no subclasses).

```java
final class MathUtils {
    static int square(int x) {
        return x * x;
    }
}

// class ExtendedUtils extends MathUtils {} // ❌ Error
```

### 2. static Keyword
The static keyword is used to define class-level members, which belong to the class rather than any object.

**a. Static Variables**
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

**b. Static Methods**
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

**c. Static Blocks**

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
**d. Static Classes (Nested)**
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
