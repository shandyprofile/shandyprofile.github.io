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

## Assignment 1: Binary Search Tree with Analytical Queries

Implement an enhanced Binary Search Tree (BST) that supports complex analytical and structural queries.

The BST must still follow the structural duplicate rule:

- If x < node.value → go LEFT
- If x ≥ node.value → go RIGHT

This creates **right-skewed duplicate chains**.

### Input Specification

```
n
v1 v2 v3 ... vn
q
query_1
query_2
...
query_q
```

### Output Specification

| **Query**            | **Description**                             | **Output Format**       | **Special Case**                   |
| -------------------- | ------------------------------------------- | ----------------------- | ---------------------------------- |
| `DEPTH x`            | Return depth of node x (root = 0)           | Integer                 | `Not Found` if x does not exist    |
| `WIDTH`              | Maximum number of nodes at any level        | Integer                 | —                                  |
| `LONGEST_PATH`       | Longest path from root to leaf              | `v1 -> v2 -> ... -> vk` | Any valid path if multiple         |
| `DELETE x`           | Delete first occurrence of x                | *(No output required)*  | `Value not found`                  |
| `KTH k`              | k-th smallest element (1-based)             | Integer                 | `Invalid` if k out of range        |
| `RANGE_SUM l r`      | Sum of values in range [l, r]               | Integer                 | `0` if no values in range          |
| `LCA x y`            | Lowest Common Ancestor of x and y           | Integer                 | `Not Found` if either node missing |
| `PATH_SUM x`         | Sum from root → node x                      | Integer                 | `Not Found`                        |
| `VALIDATE`           | Check BST validity (duplicate rule applied) | `VALID` / `INVALID`     | —                                  |
| `PRINT_LEVEL_ZIGZAG` | Zigzag level order traversal                | Each level on new line  | —                                  |

**PRINT_LEVEL_ZIGZAG**:

This involves traversing the tree in level-order, but with alternating directions between levels.

**Example:**

```
        10
       /  \
      5    20
     / \   /
    4   8 15
```

Normal Order:

```
10
5 20
4 8 15
```

ZIGZAG:

```
10          (->)
20 5        (<-)
4 8 15      (->)
```

### Examples:

**Input 1**

```
12
10 5 15 3 7 12 18 7 6 8 20 1
7
KTH 5
RANGE_SUM 5 15
LCA 6 8
PATH_SUM 8
VALIDATE
DELETE 7
PRINT_LEVEL_ZIGZAG
```

**Output 1**

```
7
70
7
30
VALID
10
15 5
3 7 12 18
20 8 6 1
7
```

**Input 2**

```
10
8 8 8 8 4 12 2 6 10 14
8
KTH 3
KTH 20
RANGE_SUM 8 8
LCA 2 14
PATH_SUM 10
DELETE 8
VALIDATE
PRINT_LEVEL_ZIGZAG
```

**Output 2**

```
10
8 8 8 8 4 12 2 6 10 14
8
KTH 3
KTH 20
RANGE_SUM 8 8
LCA 2 14
PATH_SUM 10
DELETE 8
VALIDATE
PRINT_LEVEL_ZIGZAG
```

## Assignment 2: AVL Tree with Augmented Operations

Implement an AVL Tree that supports dynamic updates and advanced analytical queries.

Your AVL Tree must:
- Maintain balance factor ∈ {-1, 0, 1}
- Perform rotations automatically: LL, RR, LR, RL
- Support duplicate values using rule:
  - x < node.value → LEFT
  - x ≥ node.value → RIGHT

Duplicate vẫn tạo right chain, nhưng AVL sẽ rebalance lại.

**AVLNode**: Each node must maintain:
- height
- size -> number of nodes in subtree
- sum -> sum of subtree values

### Input

```
n
v1 v2 v3 ... vn
q
query_1
query_2
...
query_q
```

### Output

| **Query**       | **Description**                                             | **Output Format**      | **Special Case**  |
| --------------- | ----------------------------------------------------------- | ---------------------- | ----------------- |
| `INSERT x`      | Insert value x and rebalance AVL                            | *(No output)*          | —                 |
| `DELETE x`      | Delete first occurrence of x using **in-order predecessor** | *(No output)*          | `Value not found` |
| `KTH k`         | k-th smallest element (1-based)                             | Integer                | `Invalid`         |
| `RANK x`        | Number of elements ≤ x                                      | Integer                | —                 |
| `RANGE_SUM l r` | Sum of values in range [l, r]                               | Integer                | `0` if no values  |
| `HEIGHT`        | Height of AVL tree                                          | Integer                | —                 |
| `CHECK_BALANCE` | Verify AVL property (balance factor ∈ {-1,0,1})             | `YES` / `NO`           | —                 |
| `LCA x y`       | Lowest Common Ancestor                                      | Integer                | `Not Found`       |
| `PRINT_LEVEL`   | Level-order traversal (BFS)                                 | Each level on new line | —                 |

- **DELETE**: predecessor is max value in left-subtree.
- **LCA**: The deepest node that has both x and y as descendants

Example:

```
        10
       /  \
      5    20
     / \   / \
    4   8 15 25
```

LCA(4, 8)
- Path to 4: 10 -> 5 -> 4
- Path to 8: 10 -> 5 -> 8

> The lowest common node is 5

LCA(4, 15)
- Path to 4: 10 -> 5 -> 4
- Path to 15: 10 -> 20 -> 15

> The lowest common node is 10

- **PRINT_LEVEL**: Each level on a new line. From Left to Right order

Samples:

```
        10
       /  \
      5    20
     / \
    3   7
```

```
10
5 20
3 7
```

### Samples:

**Input 1:**

```
8
10 20 30 40 50 25 5 35
7
KTH 3
RANK 25
RANGE_SUM 10 40
HEIGHT
LCA 5 25
DELETE 30
PRINT_LEVEL
```

**Output 1**

```
20
4
160
3
20
25
20 40
10 35 50
5
```

**Input 2:**

```
10
15 10 20 8 12 17 25 19 16 18
8
KTH 5
RANK 18
RANGE_SUM 10 20
HEIGHT
LCA 8 12
DELETE 15
CHECK_BALANCE
PRINT_LEVEL
```

**Output 2**

```
16
7
127
4
10
YES
12
10 20
8 17 25
16 19
18
```