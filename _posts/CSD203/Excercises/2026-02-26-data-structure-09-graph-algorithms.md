---
title: "Graphs"
description: >-
  Practice for Graphs
author: [shandy]
date: 2026-03-10
categories: [(Python) Data Structure and Algorithms, Exercises]
tags: [(Python) Data Structure and Algorithms - Exercises]
sort_index: 408
# pin: true
# media_subpath: '/posts/01'
---

## Assignment 1: Graph Traversal (BFS & DFS)

### Problem Description

You are given an undirected graph representing connections between buildings in a university campus.

Starting from a specified building, perform:
- **Breadth First Search (BFS)** traversal
- **Depth First Search (DFS)** traversal

Vertices should be visited in **ascending numerical order** when multiple choices are available.

### Input/output

**Input Format**

```
N M
u1 v1
u2 v2
...
uM vM
S
```

> Where:
> - N — number of vertices (0 -> N-1)
> - M — number of edges
> - ui vi — undirected edge between vertex ui and vi
> - S — starting vertex

Constraints:
- 1 ≤ N ≤ 1000
- 0 ≤ M ≤ 5000

**Output Format:** Two lines

```
BFS: v1 v2 v3 ...
DFS: v1 v2 v3 ...
```

### Example

Input

```
5 5
0 1
0 2
1 3
2 4
3 4
0
```

Output

```
BFS: 0 1 2 3 4
DFS: 0 1 3 4 2
```

## Assignment 2: Cycle Detection in Graph
### Problem Description

A city road network is represented as an undirected graph.

Your task is to determine whether the network contains any cycle.

If a cycle exists, print YES, otherwise print NO.

You may use either:
- DFS-based cycle detection
- Union-Find
- BFS with parent tracking

### Input/output

**Input Format**

```
N M
u1 v1
u2 v2
...
uM vM
```

> Where
> - 1 ≤ N ≤ 1000
> - 0 ≤ M ≤ 5000

Output Format

YES or NO

### Example

Input

```
4 4
0 1
1 2
2 3
3 1
```

Output

```
YES
```

## Assignment 3: Shortest Path (Dijkstra)
### Problem Description

A company network contains several buildings connected by communication cables.

Each cable has a transmission cost.

Find the minimum transmission cost from building 0 to all other buildings.

If a building is unreachable, print INF.

### 

**Input Format**

```
N M
u1 v1 w1
u2 v2 w2
...
uM vM wM
```

> Where
> - N — number of vertices
> - M — number of edges
> - ui vi wi — edge from ui to vi with weight wi

Constraints:

- 1 ≤ N ≤ 1000
- 0 ≤ M ≤ 10000
- 1 ≤ wi ≤ 1000

Graph is **directed**.

**Output Format**: Single line

```
d0 d1 d2 ... d(N-1)
```

Where di is the shortest distance from node 0 to node i.

### Example

Input

```
5 6
0 1 10
0 2 3
1 2 1
2 1 4
2 3 2
3 4 7
```

Output

```
0 7 3 5 12
```

## Assignment 4: Minimum Spanning Tree (MST)
### Problem Description

A company wants to connect all its buildings using network cables.

The cost of installing cables between buildings is given.

Find the minimum total cost required so that all buildings are connected.

You may implement either:
- Kruskal Algorithm
- Prim Algorithm

**Input Format**

```
N M
u1 v1 w1
u2 v2 w2
...
uM vM wM
```

> Where
> - N — number of vertices
> - M — number of edges
> - ui vi wi — undirected weighted edge

Constraints:
- 1 ≤ N ≤ 1000
- 0 ≤ M ≤ 10000

**Output Format**

```
Minimum Cost: X
```

If the graph is not connected, output:

```
IMPOSSIBLE
```

### Example
Input

```
4 5
0 1 10
0 2 6
0 3 5
1 3 15
2 3 4
```

Output

```
Minimum Cost: 19
```