---
title: "Maze Solving: Using DFS and BFS"
description: "Maze Solving: Using DFS and BFS"
author: [shandy]
date: 2026-07-02
categories: [(Python) Data Structure and Algorithms, Games]
tags: [(Python) Data Structure and Algorithms - Games]
sort_index: 501
# pin: true
# media_subpath: '/posts/01'
---

## Problem Description

Given a maze represented as a 2D grid:

- `0` represents an open cell that can be visited.
- `1` represents a wall that cannot be crossed.
- `S` is the starting position.
- `E` is the destination.

The objective is to find a valid path from `S` to `E`.

Example maze:

```text
S 0 1 0 0
0 0 1 0 1
1 0 0 0 1
1 1 1 0 0
1 1 1 1 E
```

Python representation:

```python
maze = [
    ['S', 0, 1, 0, 0],
    [0,   0, 1, 0, 1],
    [1,   0, 0, 0, 1],
    [1,   1, 1, 0, 0],
    [1,   1, 1, 1, 'E']
]

start = (0, 0)
end = (4, 4)
```

---

## Solution 1: Depth-First Search (DFS)

### Idea

Depth-First Search explores one path as deeply as possible before backtracking.

Algorithm:

1. Push the starting cell onto a stack.
2. Mark visited cells.
3. Pop the top cell.
4. If it is the destination, return the path.
5. Push all valid neighboring cells.
6. Repeat until the stack is empty.

Characteristics:

- Uses a **Stack (LIFO)**.
- Does **not** guarantee the shortest path.
- Usually requires less memory than BFS.

---

### Python Code

```python
def dfs(maze, start, end):
    rows = len(maze)
    cols = len(maze[0])

    stack = [(start, [start])]
    visited = set()

    directions = [
        (-1, 0),   # Up
        (1, 0),    # Down
        (0, -1),   # Left
        (0, 1)     # Right
    ]

    while stack:
        (x, y), path = stack.pop()

        if (x, y) == end:
            return path

        if (x, y) in visited:
            continue

        visited.add((x, y))

        for dx, dy in directions:
            nx = x + dx
            ny = y + dy

            if (0 <= nx < rows and
                0 <= ny < cols and
                maze[nx][ny] != 1 and
                (nx, ny) not in visited):

                stack.append(((nx, ny), path + [(nx, ny)]))

    return None


maze = [
    ['S', 0, 1, 0, 0],
    [0,   0, 1, 0, 1],
    [1,   0, 0, 0, 1],
    [1,   1, 1, 0, 0],
    [1,   1, 1, 1, 'E']
]

start = (0, 0)
end = (4, 4)

path = dfs(maze, start, end)

print("DFS Path:")
print(path)
```

---

## Solution 2: Breadth-First Search (BFS)

### Idea

Breadth-First Search explores the maze level by level.

Algorithm:

1. Enqueue the starting cell.
2. Mark visited cells.
3. Dequeue the front cell.
4. If it is the destination, return the path.
5. Enqueue all valid neighboring cells.
6. Continue until the queue becomes empty.

Characteristics:

- Uses a **Queue (FIFO)**.
- Always finds the **shortest path** in an unweighted maze.
- Requires more memory than DFS.

---

### Python Code

```python
from collections import deque


def bfs(maze, start, end):
    rows = len(maze)
    cols = len(maze[0])

    queue = deque()
    queue.append((start, [start]))

    visited = set()

    directions = [
        (-1, 0),
        (1, 0),
        (0, -1),
        (0, 1)
    ]

    while queue:
        (x, y), path = queue.popleft()

        if (x, y) == end:
            return path

        if (x, y) in visited:
            continue

        visited.add((x, y))

        for dx, dy in directions:
            nx = x + dx
            ny = y + dy

            if (0 <= nx < rows and
                0 <= ny < cols and
                maze[nx][ny] != 1 and
                (nx, ny) not in visited):

                queue.append(((nx, ny), path + [(nx, ny)]))

    return None


maze = [
    ['S', 0, 1, 0, 0],
    [0,   0, 1, 0, 1],
    [1,   0, 0, 0, 1],
    [1,   1, 1, 0, 0],
    [1,   1,   1, 1, 'E']
]

start = (0, 0)
end = (4, 4)

path = bfs(maze, start, end)

print("BFS Path:")
print(path)
```

---

## Visualizing the Solution Path

```python
def print_path(maze, path):
    result = [row[:] for row in maze]

    for x, y in path:
        if result[x][y] == 0:
            result[x][y] = '*'

    for row in result:
        print(*row)


path = bfs(maze, start, end)
print_path(maze, path)
```

**Using**

```python
maze = [
    ['S', 0, 1, 0, 0],
    [0,   0, 1, 0, 1],
    [1,   0, 0, 0, 1],
    [1,   1, 1, 0, 0],
    [1,   1, 1, 1, 'E']
]

start = (0, 0)
end = (4, 4)

path = dfs(maze, start, end)

print_path(maze, path)
```

**Output**:

```
S * 1 0 0
0 * 1 0 1
1 * * * 1
1 1 1 * *
1 1 1 1 E
```

## Complexity Analysis

Assume the maze has:

- **R** rows
- **C** columns

Number of cells:

$$V = R × C$$

Each cell has at most four neighbors.

| Algorithm | Time Complexity | Space Complexity |
|------------|-----------------|------------------|
| DFS | O(R × C) | O(R × C) |
| BFS | O(R × C) | O(R × C) |