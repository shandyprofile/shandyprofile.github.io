---
title: "Priority Queue"
description: >-
  Practice for Array-based Sequences such as Priority Queue
author: [shandy]
date: 2025-09-05
categories: [(Python) Data Structure and Algorithms, Exercises]
tags: [(Python) Data Structure and Algorithms - Exercises]
sort_index: 405
# pin: true
# media_subpath: '/posts/01'
---

## Priority Queue

![](/assets/img/2026-01-27-11-42-55.png)

You are required to implement a Priority Queue using **A singly linked list** in Python.

Each element in the queue consists of:
- a value
- a priority (lower number means higher priority)

The queue must always be maintained in sorted order of priority.

### Data Model

| Component     | Description              |
| ------------- | ------------------------ |
| Node          | Stores data and priority |
| PriorityQueue | Manages the linked list  |

### Node Model

| Attribute | Type | Description             |
| --------- | ---- | ----------------------- |
| value     | any  | Data stored in the node |
| priority  | int  | Priority of the element |
| next      | Node | Reference to next node  |


### Functions

| Function                   | Description                                          |
| -------------------------- | ---------------------------------------------------- |
| `enqueue(value, priority)` | Insert an element based on priority                  |
| `dequeue()`                | Remove and return the highest-priority element       |
| `peek()`                   | Return the highest-priority element without removing |
| `is_empty()`               | Check if the queue is empty                          |
| `display()`                | Print all elements in priority order                 |
| `size()`                   | Length of all elements in structure                  |

## Task Scheduling System Using Priority Queue

In many real-world systems, tasks are not processed in the order they arrive.
Instead, more important tasks must be handled first.

In this assignment, you will design and implement a **Task Scheduling System** using a **Priority Queue** implemented with a **Linked List**.

You are **not allowed** to use built-in priority queue libraries (e.g., heapq).

You are asked to build a Task Scheduler that manages tasks based on their priority.

Each task contains:
- a task name
- a priority value (lower number = higher priority)

Tasks are inserted into the system and must be executed in priority order. If two tasks have the same priority, they must be processed in FIFO order.

### Data Model

| Attribute | Type   | Description                              |
| --------- | ------ | ---------------------------------------- |
| task_name | string | Name of the task                         |
| priority  | int    | Priority level (lower = higher priority) |
| next      | Node   | Reference to the next task               |

### Create TaskSchedulerManagement class:

| Function                   | Description                                 |
| -------------------------- | ------------------------------------------- |
| `add_task(name, priority)` | Insert task in correct priority position    |
| `execute_task()`           | Remove and return the highest-priority task |
| `peek_task()`              | View the next task to be executed           |
| `is_empty()`               | Check whether the scheduler is empty        |
| `display_tasks()`          | Show all pending tasks in order             |

#### 1. Add a task

```python
ADD <task_name> <priority>
```

Example:

```
ADD Backup 3
ADD EmergencyFix 1
```

Output:

```
Task 'Backup' added with priority 3
Task 'EmergencyFix' added with priority 1
```

#### 2. Execute a task

```
EXECUTE
```

**Output:**

```
Executing task: EmergencyFix (priority 1)
```

If queue is empty:

```
No tasks to execute.
```

### 3. Peek next task

```
PEEK
```

Output:

```
Next task to execute: Backup (priority 3)
```

If queue is empty:

```
No tasks to peek.
```

#### 4. Display all tasks

```
DISPLAY
```

Output:

```
Current Task Queue:
[EmergencyFix | priority=1] -> [Backup | priority=3] -> None
```

#### 5. Exit program

```
EXIT
```