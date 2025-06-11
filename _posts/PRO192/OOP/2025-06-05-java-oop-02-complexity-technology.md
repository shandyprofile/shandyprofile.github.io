---
title: '[Complexity] Object Terminology'
description: >-
  Get started with Chirpy basics in this comprehensive overview.
  You will learn how to install, configure, and use your first Chirpy-based website, as well as deploy it to a web server.
author: [shandy]
date: 2025-06-05
updateDate: 2025-06-05
categories: [(Java) Object-oriented programming, (Java) OOP]
tags: [(Java) OOP]
sort_index: 2
# pin: true
# media_subpath: '/posts/02'
---

**Introduce objects and classes, Introduce encapsulation, inheritance and polymorphism**

Object-oriented programming reflects the way in which we manage day-to-day tasks. Professor Miller of Princeton University demonstrated that most of us can only comprehend about seven chunks of information at a time. As children, we learn to play with small sets of chunks at an early age. As we grow, we learn to break down the problems that we face into sets of manageable chunks.

A chunk in object-oriented programming is called an object. The shared structure of a set of similar objects is called their class. This shared structure includes the structure of the data in the similar objects as well as the logic that works on that data.

This chapter introduces the concepts of object, class, encapsulation, inheritance and polymorphism. Subsequent chapters elaborate on each concept in detail.

## Abstraction

Programming solutions to application problems consist of components. The process of designing these solutions involves abstraction. In the C programming language, we abstract common code, store it in a structure or function and refer to that structure or function from possibly multiple places in our source code, thereby avoiding code duplication.

An object-oriented programming solution to an application problem consists of components called objects. The process of designing an object-oriented solution likewise involves abstraction. We distinguish the most important features of the object, identify them publicly and hide the less important details within the object itself.

![alt text](assets/img/PRO192/OOP/oop-3.png)

Each object has a crisp conceptual boundary and acts in ways appropriate to itself. Compare a book with a set of notes. A book has pages that are bound and can be flipped. The page order is fixed. A set of notes consists of loose pages that can be rearranged in any order. We represent the book as an object and the set of notes as another object; each object has a different structure.

Example:
- An ear cannot see, an eye cannot listen and a mouth cannot smell.
- A horse cannot bark and a dog cannot croak.

## 