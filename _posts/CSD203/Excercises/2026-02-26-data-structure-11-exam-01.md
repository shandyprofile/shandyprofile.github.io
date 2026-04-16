---
title: "Graphs Advance"
description: >-
  Practice for Graphs Advance
author: [shandy]
date: 2026-03-10
categories: [(Python) Data Structure and Algorithms, Exercises]
tags: [(Python) Data Structure and Algorithms - Exercises]
sort_index: 410
# pin: true
# media_subpath: '/posts/01'
---

# Examination 1 - Data Structure and Algorithms

- Subject: CSD203
- Author: Hieu Nguyen

## Assignment: Binary Search Tree Query System
### Problem Description

You are asked to implement a Binary Search Tree (BST) to manage a collection of integer keys.

The program must support several commands to manipulate and analyze the BST.

All keys in the BST must follow the standard BST property:
- Left subtree keys < node key
- Right subtree keys > node key
- Duplicate keys must be ignored.

**Input Format**

The input consists of a sequence of commands.

```
COMMAND parameters
```

Supported commands:

| Command   | Description            |
| --------- | ---------------------- |
| INSERT x  | insert key x           |
| DELETE x  | delete key x           |
| TRAVERSE  | print traversals       |
| KTH k     | find k-th smallest key |
| RANGE a b | print keys in range    |
| END       | Input ends             |

### 1. TRAVERSE

Print the three standard BST traversals.

Output format:

```
INORDER: ...
PREORDER: ...
POSTORDER: ...
```

If tree empty:

```
EMPTY
```

### 2. KTH k

Find the k-th smallest element in the BST.

Rules:
- Counting starts from 1
- If k is larger than number of nodes → print INVALID

Output:

```
KTH: value
```

Example:

```
KTH: 15
```

### 3. RANGE a b

Print all keys between a and b (inclusive) in ascending order.

Use BST pruning to avoid scanning the entire tree.

Output:

```
RANGE: v1 v2 v3 ...
```

If none:

```
RANGE: NONE
```

### Examples

```
INSERT 20
INSERT 10
INSERT 30
INSERT 5
INSERT 15
INSERT 25
INSERT 40
TRAVERSE
KTH 3
RANGE 12 35
END
```

Output

```
INORDER: 5 10 15 20 25 30 40
PREORDER: 20 10 5 15 30 25 40
POSTORDER: 5 15 10 25 40 30 20
KTH: 15
RANGE: 15 20 25 30
```

## Assignment 2: Command-Based Graph

### Problem Description

You are developing a graph analysis system for a transportation network.

The system reads a graph and then processes commands to perform different graph algorithms.

Each command corresponds to a specific graph problem.

**Input Format:**

The input consists of three sections:
- Graph definition
- Commands
- End signal

**Graph Definition**

```
GRAPH N M
```

> **Where**
> - N = number of vertices (0 -> N-1)
> - M = number of edges

**Next M lines:**

```
EDGE u v w
```

> Where
> - u, v = vertices
> - w = edge weight

**Graph rules:**
- Undirected graph
- For shortest path, treat edge as directed u -> v

**Commands**

Commands begin after all edges are read.

```
COMMAND_NAME parameters
```

Supported commands:

| Command    | Description           |
| ---------- | --------------------- |
| PRINT      | adjacency list        |
| BFS S      | BFS traversal         |
| DIST S     | BFS shortest distance |
| CYCLE      | detect cycle          |
| SHORTEST S | Bellman-Ford          |
| MST        | Minimum Spanning Tree |
| BRIDGE     | find bridges          |
| END        | Input ends             |

### 1. PRINT

Print adjacency list.

```
0: v1 v2 ...
1: v1 v2 ...
```

Vertices sorted.

### 2. BFS S

Breadth First Search from vertex S.

```
BFS: v1 v2 v3 ...
```

### 3. DIST S

Shortest distance (unweighted BFS).

```
DIST: d0 d1 d2 ... d(N-1)
```

Unreachable = -1.

### 4. CYCLE

Check if graph contains a cycle.

```
CYCLE
```

or

```
NO CYCLE
```

### 5. SHORTEST S

Bellman-Ford shortest paths.

If negative cycle exists:

```
NEGATIVE CYCLE
```

Otherwise:

```
DIST: d0 d1 d2 ... d(N-1)
```

### 6. MST

Minimum Spanning Tree.

```
COST: X
EDGES:
u1 v1
u2 v2
...
```

If impossible:

```
IMPOSSIBLE
```

### 7. BRIDGE

Find all bridges.

````
BRIDGES: k
u1 v1
u2 v2
...
````

Edges sorted.

### Examples

Example Input

```
GRAPH 5 6
EDGE 0 1 4
EDGE 0 2 3
EDGE 1 2 2
EDGE 1 3 5
EDGE 3 4 1
EDGE 2 4 6
PRINT
BFS 0
DIST 0
CYCLE
SHORTEST 0
MST
BRIDGE
END
```

Example Output

```
0: 1 2
1: 0 2 3
2: 0 1 4
3: 1 4
4: 2 3
BFS: 0 1 2 3 4
DIST: 0 1 1 2 2
CYCLE
DIST: 0 4 3 9 9
COST: 11
EDGES:
3 4
1 2
0 2
1 3
BRIDGES: 0
```