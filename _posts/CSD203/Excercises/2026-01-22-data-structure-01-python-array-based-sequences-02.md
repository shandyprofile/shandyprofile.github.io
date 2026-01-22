---
title: "Array-based Sequences - LinkedList"
description: >-
  Practice for Array-based Sequences such as LinkedList
author: [shandy]
date: 2025-09-05
categories: [(Python) Data Structure and Algorithms, Exercises]
tags: [(Python) Data Structure and Algorithms - Exercises]
sort_index: 402
# pin: true
# media_subpath: '/posts/01'
---

The objective of this assignment is to help students understand the internal structure and operations of Linked Lists by implementing them from scratch using Python classes, without using built-in list types.

Students will implement:
- A Singly Linked List
- A Doubly Linked List

## Singly Linked List (SLL)

![](/assets/img/2026-01-27-09-57-04.png)

### Task 1: Create Node Class

| Attribute | Type        | Description                |
| --------- | ----------- | -------------------------- |
| `data`    | any         | Value stored in the node   |
| `next`    | Node / None | Reference to the next node |

### Task 2: Create SinglyLinkedList Class

| Function Name          | Parameters | Return Type | Description                                        |
| ---------------------- | ---------- | ----------- | -------------------------------------------------- |
| `insert_at_beginning`  | `data`     | None        | Insert a new node at the beginning of the list     |
| `insert_at_end`        | `data`     | None        | Insert a new node at the end of the list           |
| `delete`               | `data`     | None        | Delete the **first occurrence** of the given value |
| `search`               | `data`     | bool        | Return `True` if value exists, otherwise `False`   |
| `display`              | None       | None        | Print all elements in order                        |
| `length`               | None       | int         | Return the number of nodes in the list             |
| `reverse` *(optional)* | None       | None        | Reverse the linked list                            |

### Apply assignment: Student Management using Singly Linked List

Implement a student management system using a Singly Linked List, where each node stores information about one student.

**Data Model:** Each node represents a student with:
- student_id (int)
- name (string)
- gpa (float)

**Create StudentManagement class**

| Function                   | Description                         |
| -------------------------- | ----------------------------------- |
| `insert_at_end(student)`   | Add a new student to the list       |
| `delete_by_id(student_id)` | Remove a student by ID              |
| `search_by_id(student_id)` | Return student info if found        |
| `display()`                | Display all students                |
| `count()`                  | Return number of students           |
| `average_gpa()`            | Compute average GPA of all students |

#### insert_at_end(student)

| Item         | Description                                 |
| ------------ | ------------------------------------------- |
| **Input**    | `student`: dictionary `{id, name, gpa}`     |
| **Output**   | None                                        |
| **Behavior** | Insert a new student at the end of the list |

#### delete_by_id(student_id)

| Item         | Description                               |
| ------------ | ----------------------------------------- |
| **Input**    | `student_id`: int                         |
| **Output**   | None                                      |
| **Behavior** | Remove the first student with matching ID |

#### search_by_id(student_id)

| Item         | Description                                   |
| ------------ | --------------------------------------------- |
| **Input**    | `student_id`: int                             |
| **Output**   | Student dictionary if found, otherwise `None` |
| **Behavior** | Traverse the list and search by ID            |

#### display()

| Item         | Description                 |
| ------------ | --------------------------- |
| **Input**    | None                        |
| **Output**   | Printed student list        |
| **Behavior** | Print all students in order |

#### count()

| Item         | Description               |
| ------------ | ------------------------- |
| **Input**    | None                      |
| **Output**   | Integer                   |
| **Behavior** | Return number of students |

#### average_gpa()

| Item         | Description                        |
| ------------ | ---------------------------------- |
| **Input**    | None                               |
| **Output**   | Float                              |
| **Behavior** | Return average GPA of all students |

## Doubly Linked List (DLL)

![](/assets/img/2026-01-27-09-57-40.png)

### Task 1: Create Node Class

| Attribute | Type              | Description                    |
| --------- | ----------------- | ------------------------------ |
| `data`    | any               | Value stored in the node       |
| `next`    | DoublyNode / None | Reference to the next node     |
| `prev`    | DoublyNode / None | Reference to the previous node |

### Task 2: Create SinglyLinkedList Class

| Function Name         | Parameters | Return Type | Description                                |
| --------------------- | ---------- | ----------- | ------------------------------------------ |
| `insert_at_beginning` | `data`     | None        | Insert a new node at the beginning         |
| `insert_at_end`       | `data`     | None        | Insert a new node at the end               |
| `delete`              | `data`     | None        | Delete the first node with the given value |
| `display_forward`     | None       | None        | Display list from head to tail             |
| `display_backward`    | None       | None        | Display list from tail to head             |
| `clear` *(optional)*  | None       | None        | Remove all nodes from the list             |

### Using Doubly Linked List

Implement a music playlist application using a Doubly Linked List, allowing users to move forward and backward through songs.

**Data Model** Each node represents a song with:
- title (string)
- artist (string)
- duration (int, seconds)

**Create MusicManagement class**

| Function                 | Description                         |
| ------------------------ | ----------------------------------- |
| `insert_at_end(song)`    | Add song to playlist                |
| `delete_by_title(title)` | Remove a song                       |
| `play_forward()`         | Display playlist from first to last |
| `play_backward()`        | Display playlist from last to first |
| `total_duration()`       | Calculate total playlist duration   |

#### insert_at_end(song)

| Item         | Description                                    |
| ------------ | ---------------------------------------------- |
| **Input**    | `song`: dictionary `{title, artist, duration}` |
| **Output**   | None                                           |
| **Behavior** | Add a song to the end of the playlist          |

#### delete_by_title(title)

| Item         | Description                               |
| ------------ | ----------------------------------------- |
| **Input**    | `title`: string                           |
| **Output**   | None                                      |
| **Behavior** | Remove the first song with matching title |

Ex:

```
Song A -> Song B -> Song C -> None
```

#### play_forward()

| Item         | Description                    |
| ------------ | ------------------------------ |
| **Input**    | None                           |
| **Output**   | Printed playlist (head → tail) |
| **Behavior** | Traverse using `next` pointers |

Ex:

```
Song C -> Song B -> Song A -> None
```

#### total_duration()

| Item         | Description               |
| ------------ | ------------------------- |
| **Input**    | None                      |
| **Output**   | Integer (seconds)         |
| **Behavior** | Sum duration of all songs |

Ex:

```
Total duration: 380 seconds
```