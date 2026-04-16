---
title: "Binary Search Tree"
description: >-
  Practice for Binary Search Tree
author: [shandy]
date: 2026-02-24
categories: [(Python) Data Structure and Algorithms, Exercises]
tags: [(Python) Data Structure and Algorithms - Exercises]
sort_index: 407
# pin: true
# media_subpath: '/posts/01'
---

## Assignment 1: Parity-Based Preorder Traversal in Binary Search Tree (BST)

### Problem Description

You are given a sequence of distinct integers. Your task is to construct a Binary Search Tree (BST) using the standard insertion rule:

- If the inserted value is less than the current node -> go to the left subtree
- If the inserted value is greater than the current node -> go to the right subtree

After constructing the BST, instead of performing a standard preorder traversal (Root -> Left -> Right), you must implement a Parity-Based Preorder Traversal defined as follows:

### Parity-Based Preorder Traversal Rule

At each node during traversal:

- If the node's value is even, visit in this order:

```
Root -> Right -> Left
```

- If the node's value is odd, visit in this order:

```
Root -> Left -> Right
```

This means that the traversal order dynamically changes depending on whether the current node contains an even or odd value.

### Input Format

The first line contains an integer n — the number of elements in the BST.

The second line contains n distinct integers.

### Print the result of the Parity-Based Preorder Traversal of the constructed BST.

### Examples

Input

```
7
50 30 70 20 40 60 80
```

Output

```
50 70 80 60 30 40 20
```

## Implementing in templates exam:

```python
# --FIXED PART - DO NOT EDIT ANY THINGS HERE--
# --START FIXED PART--------------------------
import sys
# --END FIXED PART--------------------------

# --SYSTEM MODULES - @STUDENT: IMPORT SYSTEM MODULES HERE:


# --YOUR-OWN MODULES - @STUDENT: IMPORT YOUR-OWN MODULES HERE:
# from <fileName> import <className>
# For example, the file Node.py contains a class named Node,
# The statement to import class Node is: from Node import Node


# --Change the name of input and output file based on practical paper
input_file = "input.txt"
output_file = "output.txt"

# --VARIABLES - @STUDENT: DECLARE YOUR GLOBAL VARIABLES HERE:



# --ALGORITHM - @STUDENT: ADD YOUR-OWN CLASS OR METHODS HERE (IF YOU NEED):


# --FIXED PART - DO NOT EDIT ANY THINGS HERE--
# --This part is used for Automated Marking Software
# --START FIXED PART--------------------------
def set_file():
    global input_file, output_file
    length = len(sys.argv)
    input_file = input_file if length < 2 else sys.argv[1]
    output_file = output_file if length < 2 else sys.argv[2]


all_lines_of_input = ""
result_for_output = ""


def read_file():
    global input_file, all_lines_of_input  # import more global variables your own (if you need)
    with open(input_file, 'r') as file:
        all_lines_of_input = file.readlines()


# --START FIXED PART--------------------------
def solve():
    global all_lines_of_input, result_for_output  # import more global variables your own (if you need)
    result_for_output = ""
    # --END FIXED PART--------------------------
    # ALGORITHM - @STUDENT: ADD YOUR CODE HERE:


# --START FIXED PART--------------------------
def print_result():
    global output_file, result_for_output  # import more global variables your own (if you need)
    with open(output_file, 'w') as file:
        # --END FIXED PART--------------------------
        # ALGORITHM - @STUDENT: ADD YOUR CODE HERE:
        file.write(result_for_output)


# --START FIXED PART--------------------------
if __name__ == "__main__":
    set_file()
    read_file()
    solve()
    print_result()
    # --END FIXED PART--------------------------
```

1. STEP 1 — Declare Node Class
  
Add this inside:

```python
# --ALGORITHM
class Node:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None
```

2. STEP 2 — BST Insert Function (Standard)

```python
def insert(root, value):
    if root is None:
        return Node(value)

    if value < root.value:
        root.left = insert(root.left, value)
    else:
        root.right = insert(root.right, value)

    return root
```

3. STEP 3 — Parity-Based Preorder Traversal

```python
def parityPreorder(root, result):
    if root is None:
        return

    result.append(str(root.value))

    if root.value % 2 == 0:
        parityPreorder(root.right, result)
        parityPreorder(root.left, result)
    else:
        parityPreorder(root.left, result)
        parityPreorder(root.right, result)
```

4. STEP 4 — Implement solve()

Replace:

```
# ALGORITHM - @STUDENT: ADD YOUR CODE HERE:
```

with:

```python
lines = all_lines_of_input

n = int(lines[0].strip())
values = list(map(int, lines[1].split()))

root = None

for v in values:
    root = insert(root, v)

result = []
parityPreorder(root, result)

result_for_output = " ".join(result)
```

5. STEP 5 - Create a file text case - input.txt

```
7
50 30 70 20 40 60 80
```

## Assignment 2: Enhanced Binary Search Tree for Numeric Dataset with Duplicate Management

Implement an Enhanced Binary Search Tree (BST) that is capable of:
- Storing numeric values (integer or floating-point)
- Handling duplicate values
- Supporting dynamic insertion and deletion
- Maintaining frequency count for duplicates
- Performing advanced queries on the dataset

In real-world systems such as:
- Online transaction processing
- Sensor data monitoring
- Log analysis
- Inventory quantity tracking

Duplicate numeric values frequently occur.

A traditional BST typically does not handle duplicates efficiently.

In this assignment, instead of inserting duplicate values as separate nodes, you must:

> Store only one node per unique value
> Maintain an additional field called: **count**

### Input Specification

The input consists of:
1. A list of numeric values used to construct the BST
2. A list of values to delete
3. A value to search
4. A range [L, R] for range query

**Example Input:**

Format
```
<command> <value> 
```

```
Insert 50 30 70 30 90 70 20 30 60 80 70
Delete 30 70 100
Search 60
Range 30 80
```

### Output Specification

1. In-order Traversal (with count)

```
value(count)
```

```
20(1) 30(3) 50(1) 60(1) 70(3) 80(1) 90(1)
```

2. After Deletion Operation

Deletion Rules:
- If count > 1 -> Decrease count only
- If count == 1 -> Remove the node from BST
- If value not found -> print: "Value x not found"

3. Search Result

Example:

```
Found: 60(1)
```

or

```
Not Found
```

4. Range Query Result [L, R]

Return all nodes such that:

```
L ≤ value ≤ R
```

Format:

```
value(count)
```

Example:

```
30(2) 50(1) 60(1) 70(2) 80(1)
```

### Test case

```
Insert 50 30 70 30 90 70 20 30 60 80 70
Delete 30 70 100
Search 60
Range 30 80
```

```
In-order 20(1) 30(3) 50(1) 60(1) 70(3) 80(1) 90(1)

---Deletion---
Value 100 not found
20(1) 30(2) 50(1) 60(1) 70(2) 80(1) 90(1)

---Search---
Found: 60(1)

---Range---
From 30 to 80: 30(2) 50(1) 60(1) 70(2) 80(1)
```

## Assignment 3: ASSIGNMENT: AVL TREE (SELF-BALANCING BST)

Implement an AVL Tree that supports insertion, deletion, searching, and traversal operations.

The tree must automatically rebalance itself after every insertion or deletion to maintain:

Balance factor in { -1, 0, +1 }

**INPUT FORMAT**

| Line    | Content  | Description                    |
| ------- | -------- | ------------------------------ |
| 1       | `n`      | Number of commands             |
| 2 to n+1 | Commands | Each line contains one command |

**COMMAND TABLE**

| Command | Description | Rules  | Output |
| ---- | ---- | --- | --- |
| **ADD x**     | Insert value `x` into AVL Tree | - Insert like BST<br>- Rebalance using rotations (LL, RR, LR, RL)<br>- Ignore if duplicate | **No output** |
| **REMOVE x**  | Delete value `x` from AVL Tree | - Delete like BST<br>- Rebalance after deletion<br>- Ignore if not found | **No output** |
| **SEARCH x**  | Check if `x` exists | Traverse AVL Tree | `"Found"` / `"Not Found"` |
| **INORDER**   | In-order traversal             | Left -> Root ->  Right | Print values in ascending order |
| **PREORDER**  | Pre-order traversal | Root -> Left -> Right | Print values separated by spaces |
| **POSTORDER** | Post-order traversal | Left -> Right -> Root | Print values separated by spaces |
| **HEIGHT**  | Get height of tree  | Height of empty tree = 0 | Print integer height |
| **BALANCE x** | Get balance factor of node `x` | balance = height(left) - height(right) | Print balance factor or `"Not Found"` |

**SAMPLE TEST CASE**
25
ADD 50
ADD 30
ADD 70
ADD 20
ADD 40
ADD 60
ADD 80
ADD 10
ADD 25
ADD 35
ADD 45
ADD 55
ADD 65
ADD 75
ADD 85
INORDER
PREORDER
HEIGHT
BALANCE 50
BALANCE 30
SEARCH 65
SEARCH 100
REMOVE 70
REMOVE 30
INORDER
POSTORDER
HEIGHT

**Output**
10 20 25 30 35 40 45 50 55 60 65 70 75 80 85
50 30 20 10 25 40 35 45 70 60 55 65 80 75 85
4
0
0
Found
Not Found
10 20 25 35 40 45 50 55 60 65 75 80 85
10 25 20 35 45 40 55 65 60 75 85 80 50
4