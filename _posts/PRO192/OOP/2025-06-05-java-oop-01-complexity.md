---
title: '[Complexity] A Language for Complex Applications'
description: >-
  Get started with Chirpy basics in this comprehensive overview.
  You will learn how to install, configure, and use your first Chirpy-based website, as well as deploy it to a web server.
author: [shandy]
date: 2025-06-05
updateDate: 2025-06-05
categories: [(Java) Object-oriented programming, (Java) OOP]
tags: [(Java) OOP]
sort_index: 1
# pin: true
# media_subpath: '/posts/02'
---

Modern software applications are intricate, dynamic and complex. The number of lines of code can exceed the hundreds of thousands or millions. These applications evolve over time. Some take years of programming effort to mature. Creating such applications involves many developers with different levels of expertise. Their work consists of smaller stand alone and testable sub-projects; sub-projects that are transferrable, practical, upgradeable and possibly even usable within other future applications. The principles of software engineering suggest that each component should be highly cohesive and that the collection of components should be loosely coupled. Object-oriented languages provide the tools for implementing these principles

## Complexity

Large applications are complex. We address their complexity by identifying the most important features of the problem domain; that is, the area of expertise that needs to be examined to solve the problem. We express the features in terms of **data** and **activities**. We identify the data objects and the activities on those objects as complementary tasks.

Consider a course enrollment system for a program in a college or university. Each participant:
- enrolls in several face-to-face courses
- enrolls in several hybrid courses
- earns a grade in each course

The following structure diagram identifies the activities.

![alt text](assets/img/PRO192/OOP/oop-1.png)


If we switch our attention to the objects involved, we find a Course and a Hybrid Course. Focusing on a Course, we observe that it has a Course Code. We lookup the Code in the institution's Calendar to determine when that Course is offered.

We say that a Course has a Code and uses a Grading Scheme and that a Hybrid Course is a kind of Course. The diagram below shows these relationships between the objects in this problem domain. The connectors identify the types of relationships. The closed circle connector identifies a has-a relationship, the open circle connector identifies a uses-a relationship and the arrow connector identifies an is-a-kind-of relationship.

![alt text](assets/img/PRO192/OOP/oop-2.png)