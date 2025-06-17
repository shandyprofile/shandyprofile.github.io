---
title: 'I. Encapsulation'
description: >-
  Encapsulation is one of the four fundamental Object-Oriented Programming (OOP) principles, along with Abstraction, Inheritance, and Polymorphism.
  </br>It refers to the concept of wrapping data (fields) and methods (functions) into a single unit: a class, while restricting direct access to some of the object’s internal components.
author: [shandy]
date: 2025-06-17
updateDate:
categories: [(Java) Object-oriented programming, (Java) OOP]
tags: [(Java) OOP]
sort_index: 2
# pin: true
# media_subpath: '/posts/02'
---

# Encapsulation in Java

Encapsulation is one of the four fundamental **Object-Oriented Programming (OOP)** principles, along with Abstraction, Inheritance, and Polymorphism.

It refers to the concept of **wrapping data (fields)** and **methods (functions)** into a single unit: a **class**, while restricting direct access to some of the object’s internal components.

**Overview:**
- Class and Object
- Constructors
- Getter and setter
- Current object
- Other member funtions
- Access modifier

## A. Class and Object
### 1. What is a Class?
**Definition:**
A class is a blueprint or template from which individual objects are created. It defines:
- Fields (also known as attributes or member variables): Represent the state of the object.
- Methods (also called member functions): Define the behavior of the object.

**Constructors:** Special methods to initialize objects.

**Syntax:**
```java
class ClassName {
    // Fields (state)
    // Constructor(s)
    // Methods (behavior)
}
```
### 2. What is an Object?
**Definition:**
An object is a runtime instance of a class. When an object is created:
- Java allocates memory for its fields.
- The object can invoke the class's methods.

Each object has its own copy of instance variables.

**Syntax:**
```java
ClassName obj = new ClassName();
```
**Example:**
```java
class Car {
    String brand;
    int speed;

    void drive() {
        System.out.println(brand + " is driving at " + speed + " km/h");
    }
}

public class Main {
    public static void main(String[] args) {
        Car myCar = new Car();      // Creating an object
        myCar.brand = "Toyota";     // Setting fields
        myCar.speed = 100;
        myCar.drive();              // Calling method
    }
}
```
- Output:
```bash
Toyota is driving at 100 km/h
```
### 3. Key Concepts:

| Concept      | Explanation                                   |
| ------------ | --------------------------------------------- |
| `class`      | Defines the structure and behavior of objects |
| `object`     | Instance of a class with its own field values |
| `new`        | Allocates memory and initializes the object   |
| `.` operator | Accesses fields or methods of an object       |

### 4. Important Notes:

- Multiple objects can be created from the same class:

```java
Car car1 = new Car();
Car car2 = new Car();
```
> Each object has `its own copy` of the brand and speed fields.

- Objects are `reference types`, meaning variables like car1 and car2 are references pointing to memory where the actual object is stored.

## B. Constructors
### 1. What is a Constructor?
A constructor is a special method that is automatically called when a new object is created using the new keyword.

Key Features:
- Has the same name as the class.
- No return type (not even void).
- Used to initialize the fields of an object.

### 2. Types of Constructors
**a) Default `Constructor`**

- Takes no parameters.
- Java automatically provides a default constructor if no constructors are defined.

```java
class Book {
    String title;

    Book() {
        title = "Untitled";
    }
}
```
**b) Parameterized Constructor**

Takes parameters to allow object initialization with custom values.

```java
class Book {
    String title;

    Book(String t) {
        title = t;
    }
}
```

**c) Constructor Overloading**

A class can have multiple constructors with different parameter lists.

```java
class Book {
    String title;
    int year;

    Book() {
        title = "Unknown";
        year = 0;
    }

    Book(String t) {
        title = t;
        year = 0;
    }

    Book(String t, int y) {
        title = t;
        year = y;
    }
}
```
### 3. Using Constructors to Encapsulate Initialization
Constructors help enforce encapsulation by ensuring that an object is always created with valid or required values.

`Person.java`
```java
class Person {
    private String name;
    private int age;

    Person(String n, int a) {
        name = n;
        age = a;
    }

    public void introduce() {
        System.out.println("Hi, I'm " + name + " and I'm " + age + " years old.");
    }
}
```

`Main.java`
```java
public class Main {
    public static void main(String[] args) {
        Person p = new Person("Alice", 30);
        p.introduce();
    }
}
```

### 4.Why Use Constructors?

| Benefit                  | Explanation                                                           |
| ------------------------ | --------------------------------------------------------------------- |
| Automatic Initialization | Called immediately when object is created                             |
| Encapsulation            | Ensures required data is provided at creation time                    |
| Flexibility              | Constructor overloading provides multiple ways to construct an object |
| No Return Type           | Simplifies syntax and eliminates ambiguity                            |

> Things to Watch For:
> - If you define any constructor, Java will not automatically create a default constructor.
> - Constructors can call other constructors using this(...).

```java
class Student {
    String name;
    int age;

    Student() {
        this("Unknown", 0); // calls another constructor
    }

    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

### 5. Parameters vs. Arguments

| Term          | Definition                                                                   |
| ------------- | ---------------------------------------------------------------------------- |
| **Parameter** | A **placeholder variable** declared in the method or constructor definition. |
| **Argument**  | The **actual value** passed to the method or constructor when it is called.  |

**Example:**

```java
class Book {
    // title is a **parameter**
    Book(String title) {
        System.out.println("Book title: " + title);
    }
}

public class Main {
    public static void main(String[] args) {
        // "Java Basics" is an **argument**
        Book myBook = new Book("Java Basics");
    }
}
```
> **Rule of Thumb:**
> - Parameter = **variable** in the method/constructor definition
> - Argument = **value** passed during method/constructor call

## C. Getters and Setters
### 1. What Are Getters and Setters?
- **Getters** are methods used to read (get) the value of a private field.
- **Setters** are methods used to write (set) the value of a private field.

> This is a core part of **encapsulation** — where class fields are **kept private** and accessed via **public methods**.

### 2. Why Use Getters and Setters?

| Benefit           | Description                                               |
| ----------------- | --------------------------------------------------------- |
| Data Protection   | Keeps fields `private` to prevent unauthorized access     |
| Controlled Access | Allows validation before setting values                   |
| Abstraction       | Hides internal implementation from external users         |
| Flexibility       | You can change field logic without affecting outside code |

### 3. Example

`Person.java`
```java
class Person {
    private String name;      // private field
    private int age;

    // Getter for name
    public String getName() {
        return name;
    }

    // Setter for name
    public void setName(String name) {
        this.name = name;
    }

    // Getter for age
    public int getAge() {
        return age;
    }

    // Setter for age
    public void setAge(int age) {
        if (age >= 0) {
            this.age = age;
        }
    }
}
```
`Main.java`
```java
public class Main {
    public static void main(String[] args) {
        Person p = new Person();

        p.setName("Alice");           // Set value using setter
        p.setAge(25);

        System.out.println(p.getName()); // Get value using getter
        System.out.println(p.getAge());
    }
}
```
### 4. Benefits of Encapsulation via Getters and Setters
- Prevents direct access to class fields.
- Adds logic or validation (e.g. prevent setting age < 0).
- Allows future changes (e.g. converting age to birth year) without changing method names.

**Common Mistakes**
| Mistake                        | Fix                                             |
| ------------------------------ | ----------------------------------------------- |
| Making fields `public`         | Use `private` + getter/setter                   |
| Not validating input in setter | Add logic inside the setter                     |
| Forgetting `this` in setter    | Use `this.field = parameter` to avoid ambiguity |

> Tip: 
In **NetBeans** can automatically generate getters and setters for all fields — saving time and reducing typos. (**Right Mouse/Insert code.../Geter and setter**)

---
## D. Current Object

| Use Case                     | Syntax Example      | Purpose                            |
| ---------------------------- | ------------------- | ---------------------------------- |
| Disambiguate field/parameter | `this.name = name;` | Clarifies which variable is which  |
| Constructor chaining         | `this("John", 25);` | Reuse constructor logic            |
| Method chaining              | `return this;`      | Enable fluent interfaces           |
| Pass current object          | `someMethod(this);` | Send current object as an argument |

### 1. What is this in Java?
The this keyword refers to the current object — the instance of the class whose method or constructor is being executed.

It is used:
- To distinguish instance variables from parameters or local variables.
- To call other constructors in the same class.
- To return the current object from a method.

### 2. Why is this Useful?

| Use Case               | Description                                                         |
| ---------------------- | ------------------------------------------------------------------- |
| Disambiguation         | Resolves naming conflict between fields and parameters              |
| Constructor chaining   | Calls another constructor in the same class (`this(...)`)           |
| Return current object  | Useful for method chaining (`return this`)                          |
| Passing current object | You can pass `this` as a reference to another method or constructor |

### 3. Disambiguating Fields
```java
class Student {
    private String name;

    public void setName(String name) {
        this.name = name;  // 'this.name' = instance variable, 'name' = parameter
    }
}
```
Without this, name = name would just assign the parameter to itself. this.name ensures you're referring to the field.

### 4. Calling Another Constructor (this(...))
```java
class Student {
    String name;
    int age;

    Student() {
        this("Unknown", 0); // Calls the other constructor
    }

    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```
Rules:
- Must be the first line in the constructor.
- Used to reuse constructor logic and reduce redundancy.

### 5. Returning Current Object
This enables method chaining:

```java
class User {
    private String name;

    public User setName(String name) {
        this.name = name;
        return this;
    }

    public User sayHello() {
        System.out.println("Hello, " + name);
        return this;
    }
}
```
Usage:

```java
new User().setName("Anna").sayHello();
```
### 6. Passing this to Another Method
```java
class Printer {
    void printUser(User user) {
        System.out.println("User: " + user.name);
    }
}

class User {
    String name = "Bob";

    void printYourself() {
        Printer p = new Printer();
        p.printUser(this); // Passing current object
    }
}
```

> Tip:  Always use this when there's naming overlap, or when you're building flexible, chainable APIs.

---

## E. Other Member Functions (Methods in a Class)

| Element            | Purpose                         |
| ------------------ | ------------------------------- |
| Instance Methods   | Operate on object data          |
| Static Methods     | Utility logic shared by all     |
| Getters/Setters    | Encapsulation helpers           |
| Custom Methods     | Add unique behaviors to objects |
| Overloaded Methods | Provide method flexibility      |


### 1. What Are Member Functions?
Member functions (commonly known as methods) are functions defined inside a class that operate on objects' data.

They represent the behavior of an object.

### 2. General Structure
```java
class ClassName {
    // Method definition
    returnType methodName(parameters) {
        // method body
        return value;
    }
}
```

**Example:**
```java
class Calculator {
    int a, b;

    void setValues(int x, int y) {
        a = x;
        b = y;
    }

    int add() {
        return a + b;
    }

    int multiply() {
        return a * b;
    }
}
```
**Usage:**
```java
public class Main {
    public static void main(String[] args) {
        Calculator calc = new Calculator();
        calc.setValues(3, 4);
        System.out.println("Sum: " + calc.add());
        System.out.println("Product: " + calc.multiply());
    }
}
```

### 3. Types of Methods

| Type                | Description                                                 |
| ------------------- | ----------------------------------------------------------- |
| **Setter methods**  | Set (assign) values to fields (`void setName(String name)`) |
| **Getter methods**  | Retrieve field values (`String getName()`)                  |
| **Business logic**  | Custom methods like `calculateSalary()`, `drive()`, etc.    |
| **Utility methods** | Helper functions (e.g., `formatDate()`, `isValidEmail()`)   |

### 4. Instance vs. Static Methods

| Instance Method                     | Static Method                        |
| ----------------------------------- | ------------------------------------ |
| Called on an **object**             | Called on the **class** itself       |
| Can access **instance variables**   | Can only access **static variables** |
| Needs object to use: `obj.method()` | Called as `ClassName.method()`       |

```java
class MathTool {
    static int square(int n) {
        return n * n;
    }

    int cube(int n) {
        return n * n * n;
    }
}
```
**Usage:**

```java
MathTool tool = new MathTool();
System.out.println(tool.cube(3));       // instance method
System.out.println(MathTool.square(4)); // static method
```
### 5. Void vs Returning Methods

| Type            | Use When                                | Example                |
| --------------- | --------------------------------------- | ---------------------- |
| `void`          | No value needs to be returned           | `void printName()`     |
| Returning value | Result is needed for further processing | `int calculateTotal()` |


### 6. Method Overloading
You can define multiple methods with the same name but different parameters.

```java
class Printer {
    void print(String msg) {
        System.out.println(msg);
    }

    void print(int number) {
        System.out.println(number);
    }
}
```

---

## F. Access Level in Java
### 1. What Are Access Modifiers?
Access modifiers define the visibility (or access level) of classes, methods, and variables from other classes or packages.

They are crucial for:
- Implementing encapsulation
- Protecting internal data
- Controlling who can access or modify parts of your code

### 2. Types of Access Modifiers

| Modifier                | Class | Package | Subclass | World |
| ----------------------- | ----- | ------- | -------- | ----- |
| `public`                | ✅     | ✅       | ✅        | ✅     |
| `protected`             | ✅     | ✅       | ✅        | ❌     |
| *default* (no modifier) | ✅     | ✅       | ❌        | ❌     |
| `private`               | ✅     | ❌       | ❌        | ❌     |

> ✅ = accessible, ❌ = not accessible

### 3. Modifier Details
**a) public**
- Accessible from anywhere
- Often used for main classes and APIs

```java
public class User {
    public String name;
}
```
**b) private**
- Accessible only within the same class
- Most secure — ideal for fields that should not be accessed directly

```java
public class Account {
    private double balance;

    public double getBalance() {
        return balance;
    }

    public void deposit(double amount) {
        if (amount > 0) balance += amount;
    }
}
```
**c) protected**
- Accessible within the same package and in subclasses (even in different packages)
- Common in inheritance

```java
class Person {
    protected String name;
}
```
**d) default (package-private)**
- No keyword is specified
- Accessible only within the same package

```java
class Helper {
    void log(String message) {
        System.out.println(message);
    }
}
```
### 4. Applying Access Level

| Target      | Usable Access Level                            |
| ----------- | ------------------------------------------- |
| Class       | `public`, *default*                         |
| Field       | `public`, `private`, `protected`, *default* |
| Method      | `public`, `private`, `protected`, *default* |
| Constructor | Same as methods                             |

### 5. Access Level in Encapsulation
Encapsulation typically involves:
- `private` fields
- `public` getters and setters
- Keeping implementation hidden and exposing only what's needed

Example:
```java
public class Product {
    private double price;

    public double getPrice() {
        return price;
    }

    public void setPrice(double p) {
        if (p >= 0) price = p;
    }
}
```

> Tips:
- Always keep fields private.
- Use public only when necessary.
- Avoid protected unless you're designing for inheritance.
- Default access is fine for package-scoped utilities.