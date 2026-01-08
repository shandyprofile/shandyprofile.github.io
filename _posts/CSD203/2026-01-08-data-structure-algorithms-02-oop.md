---
title: Object-Oriented Programming
description: >-
  This chapter introduces the Python programming language, focusing on basic data types and operators. It explains how the Python interpreter works, how variables and objects are created, and the difference between mutable and immutable types. Students also learn about common built-in data structures such as lists, tuples, sets, and dictionaries, as well as how to use arithmetic, logical, and comparison operators in Python.
author: [shandy]
date: 2026-01-08
categories: [(Python) Data Structure and Algorithms, Theosis]
tags: [(Python) Data Structure and Algorithms]
sort_index: 102
# pin: true
# media_subpath: '/posts/01'
---

## Introduction to Object-Oriented Programming (OOP)

Object-Oriented Programming (OOP) is a programming paradigm that organizes software design around objects rather than functions.

An object represents a real-world entity and contains:
- Attributes: data (what the object has)
- Methods: behavior (what the object does)

OOP helps developers build programs that are:
- Modular
- Reusable
- Easy to maintain and extend

OOP is based on four fundamental principles:
- Encapsulation
- Abstraction
- Inheritance
- Polymorphism

### 1. Encapsulation

Encapsulation means bundling data and methods together inside a class and restricting direct access to some of the object’s data.

**Purpose**

- Protect data from unintended modification
- Control how data is accessed or changed
- Improve code reliability and security

**Python Example**

```python
class BankAccount:
    def __init__(self, balance):
        self.__balance = balance   # private attribute

    def deposit(self, amount):
        self.__balance += amount

    def get_balance(self):
        return self.__balance
```

**Explanation**
- __balance is private
- Users cannot access it directly
- Data is accessed through public methods like get_balance()

### 2. Abstraction

Abstraction means showing only essential features of an object while hiding implementation details.

**Purpose**
- Reduce complexity
- Focus on what an object does, not how it does it

Python Example (Abstract Class)

```python
from abc import ABC, abstractmethod

class Vehicle(ABC):
    @abstractmethod
    def move(self):
        pass

class Car(Vehicle):
    def move(self):
        print("The car is moving")

```

**Explanation**

- Vehicle defines a common interface
- Car provides its own implementation
- The user only knows that a vehicle can move()

### 3. Inheritance

Inheritance allows a class (child) to reuse properties and methods from another class (parent).

**Purpose**
- Promote code reuse
- Create logical class hierarchies
- Reduce duplication

**Python Example**

```python
class Animal:
    def speak(self):
        print("Animal makes a sound")

class Dog(Animal):
    def speak(self):
        print("Dog barks")
```

**Explanation**
- Dog inherits from Animal
- Dog overrides the speak() method

### 4. Polymorphism

Polymorphism means one method name, multiple behaviors, depending on the object.

**Purpose**

- Write flexible and extensible code
- Allow different objects to respond differently to the same method call

**Python Example**

```python
animals = [Dog(), Animal()]

for animal in animals:
    animal.speak()
```

**Output**

```text
Dog barks
Animal makes a sound
```

**Explanation**
- Same method call: speak()
- Different behavior based on object type