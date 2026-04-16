---
title: "Advanced Trees from BST"
description: >-
  A Binary search tree is a binary tree where the values of the left sub-tree are less than the root node and the values of the right sub-tree are greater than the value of the root node. In this article, we will discuss the binary search tree in Python.
author: [shandy]
date: 2026-02-24
categories: [(Python) Data Structure and Algorithms, Theosis]
tags: [(Python) Data Structure and Algorithms]
sort_index: 108
# pin: true
# media_subpath: '/posts/01'
---

## AVL Tree

An AVL tree defined as a self-balancing Binary Search Tree (BST) where the difference between heights of left and right subtrees for any node cannot be more than one.

> **Balance Factor = left subtree height - right subtree height**

For a Balanced Tree(for every node): 

$$-1 ≤ Balance Factor ≤ 1$$

Example of an AVL Tree:

The balance factors for different nodes are: 

![](/assets/img/2026-04-16-11-48-58.png)

12 : +1, 8 : +1, 18 : +1, 5 : +1, 11 : 0, 17 : 0 and 4 : 0. 

> Since all differences are lies between -1 to +1, so the tree is an AVL tree.

Example of a BST which is not an AVL Tree:

The Below Tree is not an AVL Tree as the balance factor for nodes 8 and 12 is more than 1.

![](/assets/img/2026-04-16-11-49-46.png)

> Define Node of AVL:

```python
class AVLNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None
        self.height = 1
```

### Important Points about AVL Tree:

| Category | Description |
| -------- | ----------- |
| Rotations | Rotations restore balance in **O(1)** time. Four cases: **LL, RR, LR, RL**.|
| Insertion and Deletion | Insertion uses upward traversal + rotation. Deletion is more complex and may require **multiple rebalancing steps**. |
| Use Cases | Ideal for **frequent lookups**, database indexing, and systems requiring **predictable performance**. |
| Drawbacks Compared to Other Trees | Stricter balancing -> higher overhead in insert/delete. Less commonly used than Red-Black Trees in standard libraries. |
| In-order Traversal | Always returns elements in **sorted order** (same as BST). |

### Operations

| Operation | Description                                                                                  | Time Complexity |        |          |
| --------- | -------------------------------------------------------------------------------------------- | --------------- | ------ | -------- |
| Searching | Same as BST because AVL is always a BST.                                                     | O(log n)        |        |          |
| Insertion | Performs standard BST insertion + rotations to ensure **                                     | balance factor  | ≤ 1**. | O(log n) |
| Deletion  | Performs standard BST deletion + rotations. May require **multiple rotations** to rebalance. | O(log n)        |        |          |

### Insertion

AVL tree is a self-balancing Binary Search Tree (BST) where the difference between heights of left and right subtrees cannot be more than one for all nodes. 
Insertion in an AVL Tree follows the same basic rules as in a Binary Search Tree (BST):
- A new key is placed in its correct position based on BST rules (left < node < right).
- After the insertion, the balance factor of each node is checked during the path back up to the root. If any node becomes unbalanced (i.e., its balance factor becomes less than -1 or greater than +1), a rotation is required to restore the AVL property.

- **Case 1: Left-Left**

![](/assets/img/2026-04-16-11-56-07.png)

- **Case 2: Right-Right**

![](/assets/img/2026-04-16-11-56-29.png)

- **Case 3: Left-Right**

![](/assets/img/2026-04-16-11-56-41.png)

- **Case 4: Right-Left**

![](/assets/img/2026-04-16-11-56-50.png)

---

> **Defind Rotation function**

![](/assets/img/2026-04-16-12-01-05.png)

To make sure that the given tree remains AVL after every insertion, we must augment the standard BST insert operation to perform some re-balancing.  Following are two basic operations that can be performed to balance a BST without violating the BST property (keys(left) < key(root) < keys(right)). 
- Left Rotation 
- Right Rotation

```python
    def _getHeight(self, node):
        return node.height if node else 0

    def _getBalance(self, node):
        return self._getHeight(node.left) - self._getHeight(node.right) if node else 0

    def _updateHeight(self, node):
        node.height = 1 + max(self._getHeight(node.left), self._getHeight(node.right))
        return self._getHeight(node)

    def _rightRotate(self, y):
        x = y.left
        T2 = x.right

        x.right = y
        y.left = T2

        self._updateHeight(y)
        self._updateHeight(x)

        return x

    def _leftRotate(self, x):
        y = x.right
        T2 = y.left

        y.left = x
        x.right = T2

        self._updateHeight(x)
        self._updateHeight(y)

        return y
```
---

> Implement insertion at AVL Tree

![](/assets/img/2026-04-16-12-03-03.png)

![](/assets/img/2026-04-16-12-03-11.png)

![](/assets/img/2026-04-16-12-03-21.png)

![](/assets/img/2026-04-16-12-03-30.png)

![](/assets/img/2026-04-16-12-03-45.png)

![](/assets/img/2026-04-16-12-03-54.png)

![](/assets/img/2026-04-16-12-04-03.png)

![](/assets/img/2026-04-16-12-04-21.png)

![](/assets/img/2026-04-16-12-04-29.png)

![](/assets/img/2026-04-16-12-04-39.png)

![](/assets/img/2026-04-16-12-04-47.png)

![](/assets/img/2026-04-16-12-04-57.png)

> **Pseudo**
> - Step 1: BST Insert: Insert key `k` into the tree using standard BST rules.
> - Step 2: Backtrack: Traverse upward from the inserted node to the root (bottom-up via recursion).
> - Step 3: Update Height: each node `x`: `h(x) = 1 + max(h(x.left), h(x.right))`.
> - Step 4: Compute Balance: For each node `x`: `BF(x) = h(x.left) - h(x.right)`.
> - Step 5: Detect Imbalance: 
>   - If `BF(x) > 1`: check **LL** (`k < x.left.key`) or **LR** (`k > x.left.key`). 
>   - If `BF(x) < -1`: check **RR** (`k > x.right.key`) or **RL** (`k < x.right.key`).
> - Step 6: Rebalance: LL -> Right rotate. RR -> Left rotate. LR -> Left rotate (left child) + Right rotate. RL -> Right rotate (right child) + Left rotate.
> - Step 7: Continue: Repeat until reaching the root.

```python
    def insert(self, value):
        self.root = self._insert(self.root, value)

    def _insert(self, root, value):
        if not root:
            return AVLNode(value)

        if value < root.value:
            root.left = self._insert(root.left, value)
        elif value > root.value:
            root.right = self._insert(root.right, value)
        else:
            return root

        self._updateHeight(root)

        balance = self._getBalance(root)

        # LL
        if balance > 1 and value < root.left.value:
            return self._rightRotate(root)

        # RR
        if balance < -1 and value > root.right.value:
            return self._leftRotate(root)

        # LR
        if balance > 1 and value > root.left.value:
            root.left = self._leftRotate(root.left)
            return self._rightRotate(root)

        # RL
        if balance < -1 and value < root.right.value:
            root.right = self._rightRotate(root.right)
            return self._leftRotate(root)

        return root
```

### Deletion

![](/assets/img/2026-04-16-12-12-04.png)

![](/assets/img/2026-04-16-12-12-13.png)

![](/assets/img/2026-04-16-12-12-25.png)

![](/assets/img/2026-04-16-12-12-35.png)

![](/assets/img/2026-04-16-12-12-44.png)

![](/assets/img/2026-04-16-12-12-56.png)

```python
    def delete(self, value):
        self.root = self._delete(self.root, value)

    def _delete(self, root, value):
        if not root:
            return root

        if value < root.value:
            root.left = self._delete(root.left, value)
        elif value > root.value:
            root.right = self._delete(root.right, value)
        else:
            # 0 or 1 child
            if root.left is None:
                return root.right
            elif root.right is None:
                return root.left

            # 2 children
            temp = self._successor(root.right)
            root.value = temp.value
            root.right = self._delete(root.right, temp.value)

        self._updateHeight(root)

        balance = self._getBalance(root)

        # LL
        if balance > 1 and self._getBalance(root.left) >= 0:
            return self._rightRotate(root)

        # LR
        if balance > 1 and self._getBalance(root.left) < 0:
            root.left = self._leftRotate(root.left)
            return self._rightRotate(root)

        # RR
        if balance < -1 and self._getBalance(root.right) <= 0:
            return self._leftRotate(root)

        # RL
        if balance < -1 and self._getBalance(root.right) > 0:
            root.right = self._rightRotate(root.right)
            return self._leftRotate(root)

        return root
```