---
title: Django Getting Started
description: >-
  Django is a high-level Python web framework that encourages rapid development and clean, pragmatic design. Built by experienced developers, it takes care of much of the hassle of web development, so you can focus on writing your app without needing to reinvent the wheel. It’s free and open source.
author: [shandy]
date: 2026-02-26
categories: [Data Sciences, Django]
tags: [Django Tutorial]
sort_index: 301
# pin: true
# media_subpath: '/posts/01'
---

## Requirement

To install Django, you must have Python installed, and a package manager like PIP.

PIP is included in Python from version 3.4.

To check if your system has Python installed, run this command in the command prompt:

```
python --version
```

> Python 3.13.2

```
pip --version
```

> pip 24.3.1

Now, that we have created a virtual environment, we are ready to install Django.

> Note: Remember to install Django while you are in the virtual environment! 

> Suggestion: Using Python in **Anaconda**

## Install Django

Django is installed using pip, with this command:

```
python -m pip install Django
```

Which will give a result that looks like this (at least on my Windows machine):

![](/assets/img/2026-02-26-12-32-07.png)

**Check Django Version**

You can check if Django is installed by asking for its version number like this:

```
django-admin --version
```

If Django is installed, you will get a result with the version number:

```
5.1.7
```


