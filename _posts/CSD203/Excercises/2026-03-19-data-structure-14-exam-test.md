---
title: "Exam Test"
description: >-
  TIC-TAC-TOE Game using Heuristic Alpha-Beta Tree Search Algorithm
author: [shandy]
date: 2026-03-17
categories: [(Python) Data Structure and Algorithms, Exercises]
tags: [(Python) Data Structure and Algorithms - Exercises]
sort_index: 413
# pin: true
# media_subpath: '/posts/01'
---

## Assignment 1: Binary Search Tree with Structural Duplicate Handling

Implement a Binary Search Tree (BST) that stores numeric values and supports multiple queries.

Unlike standard implementations, this BST must handle duplicates using a structural rule, not frequency counting.

You are required to build a BST from a given list of numbers and process a sequence of queries.
- Duplicate Handling Rule
- When inserting a value x:
  - If x < node.value: insert into LEFT subtree
  - If x ≥ node.value: insert into RIGHT subtree

This means:
- Duplicate values are allowed
- Each duplicate is stored as a separate node
- Duplicates will form a right-skewed chain

### Input Specification

The input consists of:

```
n
v1 v2 v3 ... vn
q
query_1
query_2
...
query_q
```

Where:
- n — number of values (1 ≤ n ≤ 10⁵)
- Values are separated by space
- q — number of queries

Each query is one of the supported operations below

**Example Input**

```
9
10 12 5 4 20 8 7 15 13
5
DEPTH 13
WIDTH
LONGEST_PATH
DELETE 12
PRINT_LEVEL
```

### Output Specification

#### 1. DEPTH x

- Return the depth (level) of node x.
- Root has depth = 0
- If multiple nodes have value x, return the depth of the first encountered node during search

Example

```
DEPTH 13
```

Output

```
4
```

If not found:

```
Not Found
```

#### 2. WIDTH

Return the maximum number of nodes at any level of the tree.

#### 3. LONGEST_PATH

Return the longest path from root to a leaf.

Format

```
v1 -> v2 -> v3 -> ... -> vk
```

If multiple paths have the same length, return any one.

#### 4. DELETE x

Delete the first occurrence of value x encountered during BST search.

Rules:
- Standard BST deletion applies:
- Leaf node -> remove directly
- One child -> replace with child
- Two children -> replace with inorder successor

If value not found:

```
Value not found
```

After deletion, print in-order traversal:

```
v1 v2 v3 ... vk
```

#### 5. PRINT_LEVEL

Print the tree level by level (Breadth-First Traversal)

Format

Each level on one line:

```
level_0
level_1
level_2
...
```

### Test case

**Example Input**

```
9
10 12 5 4 20 8 7 15 13
5
DEPTH 13
WIDTH
LONGEST_PATH
DELETE 12
PRINT_LEVEL
```

**Example Output**

```
4
3
10 -> 12 -> 20 -> 15 -> 13
4 5 7 8 10 13 15 20
10
5 12
4 8 20
7 15
13
```

## Assignment 2: Dynamic Graph Query System

You are given a weighted graph representing a network.

After building the graph, you must process a sequence of queries that dynamically analyze and modify the graph.

**Input Format**

```
N M
u1 v1 w1
u2 v2 w2
...
uM vM wM
[query_number]
[query ...]
```

- Undirected graph
- Vertices: 0 -> N-1

### 1. PATH u v

- Determine whether there exists any path between vertex u and vertex v.
- Use BFS or DFS
- Ignore edge weights
- If u == v -> output YES
- Empty graph	-> NO

**Input**

```
PATH u v
```

**Output**

```
YES
```

or

```
NO
```

### 2. SHORTEST u v

- Compute the shortest path distance from u to v.
- Use Bellman–Ford algorithm
- If u == v -> DIST: 0
- No path exists: None

Input

```
SHORTEST u v
```

Output

```
DIST: X
```

or

```
None
```

### 3. ADD u v w

- Add an edge between u and v with weight w.
- Because graph is undirected: Store both (u -> v) and (v -> u)
- u == v: Ignore but still print ADDED
- Duplicate edge: Update weight


Input

```
ADD u v w
```

Output

```
ADDED
```

### 4. REMOVE u v

- Remove the edge between u and v.
- Remove both directions (u - v)
- If edge does not exist -> NOT FOUND

Input

```
REMOVE u v
Output
REMOVED
```

or

```
NOT FOUND
```

### 5. COMPONENTS

- Return the number of connected components in the graph.
- Use BFS or DFS
- Must traverse the entire graph
- Empty graph: 0
- No edges: None

Input

```
COMPONENTS
```

Output

```
COMPONENTS: k
```

### Test Case 1

```
5 4
0 1 2
1 2 3
2 3 4
3 4 5
6
PATH 0 4
SHORTEST 0 4
COMPONENTS
REMOVE 2 3
PATH 0 4
COMPONENTS
```

Output

```
YES
DIST: 14
COMPONENTS: 1
REMOVED
NO
COMPONENTS: 2
```

### Test case 2:

```
6 2
0 1 1
4 5 2
5
COMPONENTS
PATH 0 5
SHORTEST 0 5
PATH 4 5
COMPONENTS
```

Output:

```
COMPONENTS: 4
NO
INF
YES
COMPONENTS: 4
```
