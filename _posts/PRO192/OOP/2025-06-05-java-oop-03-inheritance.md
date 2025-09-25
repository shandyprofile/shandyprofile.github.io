---
title: 'II. Inheritance'
description: >-
  Inheritance is a mechanism in Java where one class (subclass or child) can inherit the fields and methods of another class (superclass or parent).
  </br>
  This promotes code reuse, hierarchical classification, and polymorphism.
author: [shandy]
date: 2025-06-17
updateDate: 2025-06-23
categories: [(Java) Object-oriented programming, (Java) OOP]
tags: [(Java) OOP]
sort_index: 203
# pin: true
# media_subpath: '/posts/02'
---

# Inheritance in Java

What is Inheritance?
Inheritance is a mechanism in Java where one class (subclass or child) can inherit the fields and methods of another class (superclass or parent).

This promotes code reuse, hierarchical classification, and polymorphism.

**Overview:**
- SuperClass ans SubClass
- Common relationships
- Functions in a hierarchy
- Instanceof operator
- Casting 

## A. SuperClass ans SubClass

| Term                         | Meaning                                             |
| ---------------------------- | --------------------------------------------------- |
| **Superclass** (Base Class)  | The class whose properties are inherited (`Parent`) |
| **Subclass** (Derived Class) | The class that inherits the superclass (`Child`)    |
| **`extends` keyword**        | Used to declare that one class inherits another     |

---

| Concept      | Description                                |
| ------------ | ------------------------------------------ |
| Inheritance  | One class inherits another                 |
| Superclass   | Provides common functionality              |
| Subclass     | Specializes or extends superclass behavior |
| extends      | Keyword for inheritance                    |
| Constructors | Not inherited                              |
| Access       | Controlled via access modifiers            |

### 1. Basic Example
```java
// Superclass
class Animal {
    void makeSound() {
        System.out.println("Animal makes sound");
    }
}

// Subclass
class Dog extends Animal {
    void bark() {
        System.out.println("Dog barks");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog d = new Dog();
        d.makeSound();  // Inherited from Animal
        d.bark();       // Specific to Dog
    }
}
```

> Dog inherits makeSound() from Animal. It also defines its own method bark().

### 2. Multilevel Inheritance
Java supports multilevel (not multiple) inheritance:

```java
class Animal {
    void eat() { System.out.println("eating..."); }
}

class Dog extends Animal {
    void bark() { System.out.println("barking..."); }
}

class Puppy extends Dog {
    void weep() { System.out.println("weeping..."); }
}
```
### 3. No Multiple Inheritance (of classes)
Java does not support multiple inheritance for classes to avoid ambiguity (known as the diamond problem).

> **However, OOP has multiple inheritance properties.**

```java
class A {}
class B {}
// class C extends A, B {} // => Compilation Error
```

> Java handles this via **interfaces**.

### 4. Access Modifiers and Inheritance

| Modifier    | Inherited in Subclass?  | Accessible in Subclass?         |
| ----------- | ----------------------- | ------------------------------- |
| `public`    | Yes                     | Yes                             |
| `protected` | Yes                     | Yes (even from another package) |
| *default*   | Yes (same package only) | Yes (same package only)         |
| `private`   | No                      | No                              |

### 5. Constructor Behavior
- Constructors are not inherited.
- A subclass constructor must call a superclass constructor (implicitly or explicitly).

```java
class Animal {
    Animal() {
        System.out.println("Animal constructor");
    }
}

class Dog extends Animal {
    Dog() {
        super(); // optional if superclass has no-arg constructor
        System.out.println("Dog constructor");
    }
}
```

## B. Common Relationships in Inheritance
Inheritance is used to model "is-a" relationships in object-oriented design. This section covers how classes relate to each other in Java using inheritance, including practical examples and guidelines.

| Topic                      | Summary                                             |
| -------------------------- | --------------------------------------------------- |
| is-a relationship          | Subclass inherits behavior from superclass          |
| Common hierarchies         | Employee–Manager, Animal–Dog, etc.                  |
| Use cases                  | Reusability, extensibility                          |
| Composition vs Inheritance | Use composition for "has-a", inheritance for "is-a" |


### 1. The "is-a" Relationship
- In Java, inheritance represents an **"is-a" relationship**.
- If class `B` extends class `A`, then `B is-a A`.

Example:

```java
class Animal { }
class Dog extends Animal { }

Dog d = new Dog();
System.out.println(d instanceof Animal); // true
```
> A Dog is an Animal

### 2. Common Real-World Examples

| Superclass | Subclasses                         |
| ---------- | ---------------------------------- |
| `Animal`   | `Dog`, `Cat`, `Bird`               |
| `Shape`    | `Circle`, `Rectangle`, `Triangle`  |
| `Employee` | `Manager`, `Developer`, `Intern`   |
| `Vehicle`  | `Car`, `Truck`, `Bicycle`          |
| `Account`  | `SavingAccount`, `CheckingAccount` |

Each subclass inherits shared behavior and adds specific behavior.

### 3. Inheritance Design Patterns
**a) Generalization to Specialization**

Start with a general concept and refine it.

```java
class Vehicle {
    void move() { System.out.println("Moving..."); }
}

class Car extends Vehicle {
    void playMusic() { System.out.println("Playing music..."); }
}
```
**b) Common Interface Extraction**

When multiple classes share behavior, extract a superclass or interface:

```java
class Dog {
    void bark() { }
}

class Cat {
    void meow() { }
}

// Both can be made subclasses of Animal
class Animal {
    void makeSound() { }
}
```
### 4. "Has-a" vs "Is-a"
Relationship	Meaning	Implementation
is-a	One class is a type of another	Use extends / implements
has-a	One class contains another	Use object composition

Example:

```java
class Engine { }

class Car {
    private Engine engine; // has-a relationship
}
```
### 5. When to Use Inheritance
Use inheritance when:
- There's a clear "is-a" relationship.
- You want to reuse common logic.
- You plan to use polymorphism (override methods in subclasses).

Avoid inheritance if:
- The relationship is "has-a" (prefer composition).
- It would create tight coupling or reduce flexibility.

### 6. UML Class Diagram Example
```markdown
           Animal
           /    \
        Dog     Cat
```

- Dog and Cat both extend Animal
- Both can override methods like makeSound()

## C. Functions in a Hierarchy
When using inheritance, functions (methods) play a central role in how behavior is shared and customized across a class hierarchy. Java allows you to inherit, override, and extend methods in subclasses.

### 1. Inheriting Methods
If a superclass has a method, the subclass automatically inherits it — unless it overrides the method.

```java
class Animal {
    void makeSound() {
        System.out.println("Some animal sound");
    }
}

class Dog extends Animal {
    // Inherits makeSound() by default
}
```

```java
Dog d = new Dog();
d.makeSound(); // prints "Some animal sound"
```

### 2. Overriding Methods
A subclass can override a method to provide its own version of the behavior.

```java
class Animal {
    void makeSound() {
        System.out.println("Some animal sound");
    }
}

class Dog extends Animal {
    @Override
    void makeSound() {
        System.out.println("Bark!");
    }
}
```

```java
Animal a = new Dog();
a.makeSound(); // prints "Bark!" – Dog's version
```
> Overriding enables runtime polymorphism

### 3. Rules of Method Overriding
- The method name, return type, and parameters must match exactly.
- The overridden method must not be less accessible than the superclass method.
- You can use the @Override annotation for clarity and safety.
- Only non-static methods can be overridden.

### 4. Calling Superclass Method with super
You can call the original version of the method from the superclass using super.

```java
class Animal {
    void makeSound() {
        System.out.println("Generic animal sound");
    }
}

class Dog extends Animal {
    @Override
    void makeSound() {
        super.makeSound(); // Call parent version
        System.out.println("...and also Bark!");
    }
}
```

### 5. Method Accessibility in Hierarchy

| Modifier    | Inherited?       | Overridable? | Accessible in Subclass? |
| ----------- | ---------------- | ------------ | ----------------------- |
| `public`    | ✅                | ✅            | ✅                       |
| `protected` | ✅                | ✅            | ✅                       |
| *default*   | ✅ (same package) | ✅            | ✅ (same package)        |
| `private`   | ❌                | ❌            | ❌                       |

### 6. Abstract Methods (in Abstract Classes)
An abstract method is a method declared without an implementation in an abstract class. Subclasses must implement it.

```java
abstract class Animal {
    abstract void makeSound();
}

class Cat extends Animal {
    @Override
    void makeSound() {
        System.out.println("Meow");
    }
}
```

### 7. Method Overloading vs Overriding

| Concept     | Overloading                            | Overriding                     |
| ----------- | -------------------------------------- | ------------------------------ |
| Definition  | Same method name, different parameters | Redefining inherited method    |
| Class       | Same class                             | Parent–child relationship      |
| Return Type | Can vary                               | Must be same (or subtype)      |
| Static      | Applies to static methods              | Not allowed for static methods |


## D. `instanceof` Operator
### 1. What is instanceof?
The instanceof operator is used to test whether an object is an instance of a specific class, subclass, or interface.

It returns a boolean value:
- true → the object is an instance of the specified type.
- false → it is not.

**Syntax:**
```java
object instanceof ClassName
```

**Example:**
```java
class Animal {}
class Dog extends Animal {}

public class Main {
    public static void main(String[] args) {
        Animal a = new Dog();

        System.out.println(a instanceof Dog);     // true
        System.out.println(a instanceof Animal);  // true
        System.out.println(a instanceof Object);  // true
    }
}
```
> A Dog is an Animal and also an Object, so all checks return true.

### 2. Null-Safe Behavior
If the object is null, instanceof always returns false.

```java
Dog d = null;
System.out.println(d instanceof Dog); // false
```

### 3. Use Cases

| Use Case                              | Description                                        |
| ------------------------------------- | -------------------------------------------------- |
| Type checking before casting        | Prevents `ClassCastException`                      |
| Polymorphic method logic            | Different actions based on object type             |
| Runtime verification of class types | Helps debug or validate dynamic object assignments |


### 4. With Interfaces
You can use instanceof to check if an object implements an interface:

```java
interface Pet {}

class Cat implements Pet {}

public class Main {
    public static void main(String[] args) {
        Pet p = new Cat();
        System.out.println(p instanceof Cat); // true
        System.out.println(p instanceof Pet); // true
    }
}
```
### 5. Pattern Matching
Since Java 16, instanceof supports pattern matching, which simplifies syntax:

```java
if (obj instanceof Dog d) {
    d.bark(); // No cast needed
}
```

> This avoids explicit casting and is type-safe.

### 6. instanceof vs Bad Design
Overuse of instanceof may signal bad object-oriented design. Prefer polymorphism:

**[NO]** Instead of:

```java
if (shape instanceof Circle) {
    // special logic
}
```
**[Yes]** Use method overriding:

```java
shape.draw();
```

Let each subclass define its own behavior.

## E. Casting in Inheritance
In Java, casting is used to convert one object reference to another type—typically between classes in the same inheritance hierarchy. It is essential when working with polymorphism.

### 1. Types of Casting
There are two main types:

| Type            | Description    | Syntax               |
| --------------- | -------------- | -------------------- |
| **Upcasting**   | Child → Parent | Implicit             |
| **Downcasting** | Parent → Child | Explicit (must cast) |


### 2. Upcasting (Safe & Implicit)
Assigning a subclass object to a superclass reference.

```java
class Animal {
    void makeSound() { System.out.println("Some sound"); }
}

class Dog extends Animal {
    void bark() { System.out.println("Bark!"); }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();
        Animal a = dog; // Upcasting
        a.makeSound();  // OK
        // a.bark();    // Not accessible
    }
}
```

> Upcasting is always safe. The reference type (Animal) controls accessible methods.

### 3. Downcasting (Explicit & Risky)
Assigning a superclass reference to a subclass type. Requires explicit casting and is only safe if the object is really an instance of the subclass.

```java
Animal a = new Dog(); // Upcast
Dog d = (Dog) a;       // Downcast
d.bark();              // Now accessible
```
**Unsafe Downcasting:**

```java
Animal a = new Animal(); 
Dog d = (Dog) a;  // ClassCastException at runtime!
```

### 4. Safe Downcasting with `instanceof`
Always check type before downcasting to avoid runtime errors:

```java
if (a instanceof Dog) {
    Dog d = (Dog) a;
    d.bark();
} else {
    System.out.println("Not a Dog");
}
```

### 5. Common Mistake: Wrong Assumptions
```java
Animal a = new Animal();
Cat c = (Cat) a; // Compiles, but runtime error!
```
> This throws a ClassCastException because a is not actually a Cat.