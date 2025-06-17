---
title: 'II. Inheritance'
description: >-
  Inheritance is a mechanism in Java where one class (subclass or child) can inherit the fields and methods of another class (superclass or parent).
  </br>
  This promotes code reuse, hierarchical classification, and polymorphism.
author: [shandy]
date: 2025-06-17
updateDate:
categories: [(Java) Object-oriented programming, (Java) OOP]
tags: [(Java) OOP]
sort_index: 2
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

3. Basic Example
java
Sao chép
Chỉnh sửa
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
🧠 Dog inherits makeSound() from Animal. It also defines its own method bark().

🔄 4. Multilevel Inheritance
Java supports multilevel (not multiple) inheritance:

java
Sao chép
Chỉnh sửa
class Animal {
    void eat() { System.out.println("eating..."); }
}

class Dog extends Animal {
    void bark() { System.out.println("barking..."); }
}

class Puppy extends Dog {
    void weep() { System.out.println("weeping..."); }
}
🚫 5. No Multiple Inheritance (of classes)
Java does not support multiple inheritance for classes to avoid ambiguity (known as the diamond problem).

```java
class A {}
class B {}
// class C extends A, B {} // ❌ Compilation Error```
✅ Java handles this via interfaces.

🔧 6. Access Modifiers and Inheritance
Modifier	Inherited in Subclass?	Accessible in Subclass?
public	Yes	Yes
protected	Yes	Yes (even from another package)
default	Yes (same package only)	Yes (same package only)
private	No	No

📦 7. Constructor Behavior
Constructors are not inherited.

A subclass constructor must call a superclass constructor (implicitly or explicitly).

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
🧾 Summary
Concept	Description
Inheritance	One class inherits another
Superclass	Provides common functionality
Subclass	Specializes or extends superclass behavior
extends	Keyword for inheritance
Constructors	Not inherited
Access	Controlled via access modifiers
