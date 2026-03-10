---
title: "Graphs Advance"
description: >-
  Practice for Graphs Advance
author: [shandy]
date: 2026-03-10
categories: [(Python) Data Structure and Algorithms, Exercises]
tags: [(Python) Data Structure and Algorithms - Exercises]
sort_index: 409
# pin: true
# media_subpath: '/posts/01'
---

## Assignment 5: Graph Analysis System

### Problem Description

A company manages a network of buildings connected by communication links.
The network can be represented as a weighted graph.

Your program must perform three tasks:

1. Traverse the entire graph using BFS and DFS, even if the graph is disconnected.
2. Compute the shortest path from building 0 to all other buildings using Bellman-Ford algorithm (the graph may contain negative edges).
3. Construct a Minimum Spanning Tree (MST) and output the list of edges included in the MST.

If the graph is not connected, the MST cannot be formed.

### Input Format

```
N M
u1 v1 w1
u2 v2 w2
...
uM vM wM
```

> Where:
> - N — number of vertices (0 → N-1)
> - M — number of edges
> - ui vi wi — undirected edge between ui and vi with weight wi

**Constraints**
- 1 ≤ N ≤ 1000
- 0 ≤ M ≤ 10000
- -1000 ≤ wi ≤ 1000

### Output Format

The program must print three sections.

1. BFS Traversal

Traverse the entire graph using BFS.
If the graph has multiple components, continue BFS from the smallest unvisited vertex.

```
BFS: v1 v2 v3 ...
```

2. DFS Traversal

Traverse the entire graph using DFS.
When multiple neighbors exist, visit vertices in ascending order.

```
DFS: v1 v2 v3 ...
```

3. Shortest Paths (Bellman-Ford)

Compute the shortest path from vertex 0 to all vertices.

If a vertex is unreachable, print INF.

```
ShortestPath: d0 d1 d2 ... d(N-1)
```

If a negative cycle exists, print:

```
NEGATIVE CYCLE
```

4. Minimum Spanning Tree

If the graph is connected:

```
MST Edges:
u1 v1
u2 v2
...
Total Cost: X
```

If the graph is disconnected:

```
MST: IMPOSSIBLE
```

Edges can be printed in any order.

### Example

```
Input 1
6 7
0 1 4
0 2 3
1 2 -2
1 3 5
2 3 2
3 4 7
4 5 1
```

Output 1

```
BFS: 0 1 2 3 4 5
DFS: 0 1 2 3 4 5
ShortestPath: 0 1 3 5 12 13
MST Edges:
1 2
0 2
2 3
4 5
3 4
Total Cost: 11
```

---
**Additional Example (Disconnected Graph)**

Input 2

```
5 2
0 1 3
3 4 2
```

Output

```
BFS: 0 1 2 3 4
DFS: 0 1 2 3 4
ShortestPath: 0 3 INF INF INF
MST: IMPOSSIBLE
```

## Assignment 6: Smart Transportation Network Analyzer

### Problem Description

A smart city manages its transportation network using a graph system.

Each intersection is represented as a vertex, and each road is an edge.

Roads may be:
- bidirectional (two-way)
- one-way

Each road has a travel cost, which may be negative due to toll discounts.

Your task is to build a network analyzer that processes several types of queries about the transportation network.

### Input Format

Input consists of multiple commands.

```
GRAPH N M
EDGE u v w type
EDGE u v w type
...
QUERY command parameters
QUERY command parameters
...
END
```

#### 1. GRAPH

```
GRAPH N M
```

> - N — number of intersections (0 -> N-1)
> - M — number of roads

#### 2. EDGE

```
EDGE u v w type
```

> Where:
> - u, v = intersections
> - w = travel cost
> - type:
>   - B -> bidirectional
>   - O -> one-way (u → v)

**Example**

```
EDGE 0 1 4 B
EDGE 1 3 5 O
```

#### 3. Query Types

The system must support the following queries.

**Case 1 — Network Exploration**

```
QUERY EXPLORE S
```

Starting from intersection S, perform:
- BFS traversal
- DFS traversal

Vertices must be visited in ascending order.

Output:

```
BFS: ...
DFS: ...
```

**Case 2 — Shortest Routes**

```
QUERY SHORTEST S
```

Compute shortest paths from vertex S.

Rules:
- Graph may contain negative edges
- Must use Bellman-Ford

If a negative cycle is reachable from S:

```
NEGATIVE CYCLE
```

Otherwise:

```
DIST: d0 d1 d2 ... d(N-1)
```

Use **INF** if unreachable.

**Case 3 — Build Minimum Network**

```
QUERY MST
```

Construct a minimum spanning tree using only bidirectional roads.

Output:

```
MST COST X
EDGES k
u1 v1
u2 v2
...
```

If impossible:

```
MST IMPOSSIBLE
```

Case 4 — Critical Intersections

```
QUERY CRITICAL
```

Find all articulation points in the graph.

Output:

```
CRITICAL: v1 v2 v3 ...
```

If none:

```
CRITICAL: NONE
```

### Example

```
GRAPH 6 7
EDGE 0 1 4 B
EDGE 0 2 3 B
EDGE 1 2 -2 B
EDGE 1 3 5 O
EDGE 2 3 2 B
EDGE 3 4 7 B
EDGE 4 5 1 B
QUERY EXPLORE 0
QUERY SHORTEST 0
QUERY MST
QUERY CRITICAL
END
```

Output

```
BFS: 0 1 2 3 4 5
DFS: 0 1 2 3 4 5
DIST: 0 1 3 5 12 13
MST COST 11
EDGES 5
1 2
0 2
2 3
4 5
3 4
CRITICAL: 3 4
```