---
title: "Data Structure: Tree"
description: >-
  A Binary search tree is a binary tree where the values of the left sub-tree are less than the root node and the values of the right sub-tree are greater than the value of the root node. In this article, we will discuss the binary search tree in Python.
author: [shandy]
date: 2026-02-24
categories: [(Python) Data Structure and Algorithms, Theosis]
tags: [(Python) Data Structure and Algorithms]
sort_index: 107
# pin: true
# media_subpath: '/posts/01'
---

- Author: Hieu Nguyen
- Subject: CSD203

# Data Structure: Tree

## 1. Abtract Tree

```python
class Tree:
    """Abstract base class representing a tree structure."""

    # -------------------------- nested Position class --------------------------
    class Position:
        """An abstraction representing the location of a single element."""

        def element(self):
            """Return the element stored at this Position."""
            raise NotImplementedError('must be implemented by subclass')

        def __eq__(self, other):
            """Return True if other Position represents the same location."""
            raise NotImplementedError('must be implemented by subclass')

        def __ne__(self, other):
            """Return True if other does not represent the same location."""
            return not (self == other)

    # ---------------- abstract methods that concrete subclass must support ----------------
    def root(self):
        """Return Position representing the tree's root (or None if empty)."""
        raise NotImplementedError('must be implemented by subclass')

    def parent(self, p):
        """Return Position representing p's parent (or None if p is root)."""
        raise NotImplementedError('must be implemented by subclass')

    def num_children(self, p):
        """Return the number of children that Position p has."""
        raise NotImplementedError('must be implemented by subclass')

    def children(self, p):
        """Generate an iteration of Positions representing p's children."""
        raise NotImplementedError('must be implemented by subclass')

    def __len__(self):
        """Return the total number of elements in the tree."""
        raise NotImplementedError('must be implemented by subclass')

    # ---------------- concrete methods implemented in this class ----------------
    def is_root(self, p):
        """Return True if Position p represents the root of the tree."""
        return self.root() == p

    def is_leaf(self, p):
        """Return True if Position p does not have any children."""
        return self.num_children(p) == 0

    def is_empty(self):
        """Return True if the tree is empty."""
        return len(self) == 0
```

## 2. ADT Tree

### 2.1 Position class:

Position is a nested class inside the Tree class that represents the location of a node within the tree rather than exposing the actual node directly.

This class acts as an abstraction layer to:
- Hide the internal implementation details of the tree
- Improve data encapsulation and security
- Prevent direct manipulation of internal nodes

Each Position object typically represents:
- A specific node in the tree
- The element stored in that node
- The node’s location within the tree structure

The Position class usually provides operations such as:
- Accessing the stored element (element())
- Comparing two positions (_`_eq__`())
- Checking whether two positions are different (`__ne__`())

Example:

A node containing "A" in the tree may be represented by a Position object.
Users interact with Position objects instead of accessing internal _Node objects directly.

| Function              | Description                                                                                                                                                             |
| --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `element(self)`       | Returns the element stored at the current position in the tree. This method is used to access the data contained in a node.                                             |
| `__eq__(self, other)` | Compares the current position with another position to determine whether they represent the same node in the tree. Returns `True` if they are equal, otherwise `False`. |
| `__ne__(self, other)` | Checks whether two positions are different. This method is the opposite of `__eq__()`. Returns `True` if the positions are not the same.                                |

1. element(self)

This function returns the element stored in the current position.

```python
def element(self):
    return self._node._element
```

- self._node refers to the internal node associated with the current Position.
- _element is the actual data stored inside that node.
- The function simply accesses and returns that value.

2. `__eq__`(self, other)

This function checks whether two Position objects represent the same location in the tree.

```python
def __eq__(self, other):
    return type(other) is type(self) and other._node is self._node
```

The comparison has two conditions:
- type(other) is type(self): Ensures both objects are of the same class type.
- other._node is self._node
  - Checks whether both positions reference the exact same internal node object.
  - The keyword is compares object identity, not just value equality.

The function returns:
- True if both positions point to the same node
- False otherwise

Using:

```python
p1 == p2
```

3. `__ne__`(self, other)

This function checks whether two positions are different.

```python
def __ne__(self, other):
    return not (self == other)
```

- It calls `__eq__`() internally.
- The result is negated using not.

Equivalent meaning:
- If `__eq__`() returns True, then `__ne__`() returns False
- If `__eq__`() returns False, then `__ne__`() returns True

Using:

```python
p1 != p2
```

### 2.2 __init__(self)

Initializes an empty tree.

```python
def __init__(self):
    self._root = None
    self._size = 0
```

- self._root = None: The tree initially has no root node.
- self._size = 0: The tree starts with zero nodes.

### 2.3. _make_position(self, node)

Converts an internal node object into a Position object.

```python
def _make_position(self, node):
    if node is None:
        return None
    return self.Position(self, node)
```

- If node is None, the function returns None.
- Otherwise, it creates and returns a new Position object associated with that node.

### 2.4 _validate(self, p)

Validates whether p is a proper Position object.

```python
def _validate(self, p):
    if not isinstance(p, self.Position):
        raise TypeError("p must be proper Position type")
    return p._node
```

Checks if p is an instance of Position.
- If not, raises a TypeError.
- If valid, returns the internal node associated with p.

### 2.5 root(self)

Returns the root position of the tree.

```python
def root(self):
    return self._make_position(self._root)
```

- Retrieves the internal root node (self._root)
- Converts it into a Position object using _make_position()

**Return**
- Root Position
- None if the tree is empty

### 2.6 parent(self, p)

Returns the parent position of p.

```python
def parent(self, p):
    node = self._validate(p)
    return self._make_position(node._parent)
```

- Validates p
- Accesses the parent node using node._parent
- Converts the parent node into a Position

**Return**
- Parent Position
- None if p is the root

### 2.7 num_children(self, p)

Returns the number of children of position p.

```python
def num_children(self, p):
    node = self._validate(p)
    return len(node._children)
Description
```

- Validates p
- Accesses the list of child nodes
- Uses len() to count them

### 2.8 children(self, p)

Generates all child positions of p.

``` python
def children(self, p):
    node = self._validate(p)

    for child in node._children:
        yield self._make_position(child)
```

- Validates p
- Iterates through all child nodes
- Converts each child node into a Position
- Uses yield to generate them one by one

Supports tree traversal and iteration.

### 2.9 `__len__`(self)

```python
def __len__(self):
    return self._size
```

- Returns the total number of nodes in the tree.
- Returns the value stored in `_size`

Using

```
len(tree)
```

### 2.10 is_root(self, p)

Checks whether p is the root of the tree.

```python
def is_root(self, p):
    return self.root() == p
```

- Compares p with the tree’s root position.

Return
- True if p is the root
- False otherwise


### 2.11 is_leaf(self, p)

Checks whether p is a leaf node.

```python
def is_leaf(self, p):
    return self.num_children(p) == 0
```

- Counts the number of children of p
- If the count is 0, the node is a leaf

Return
- True if p has no children
- False otherwise

### 2.12 is_empty(self)

Checks whether the tree is empty.

```python
def is_empty(self):
    return len(self) == 0
```

- Calls len(self)
- Compares the result with 0

Return
- True if the tree contains no nodes
- False otherwise

### 2.13 add_root(self, e)

Creates the root node of the tree.

```python
def add_root(self, e):
    if self._root is not None:
        raise ValueError("Root already exists")

    self._root = self._Node(e)
    self._size = 1

    return self._make_position(self._root)
```

1. Checks whether the tree already has a root
2. If yes, raises an error
3. Creates a new node storing e
4. Sets it as the root
5. Updates tree size to 1

Returns the root position

## 3. Traversals
### 3.1 preorder(self)

Performs a preorder traversal of the tree.

**Traversal Order**
- Visit the current node
- Traverse each child subtree recursively (left - right)

```python
def _subtree_preorder(self, p):
    yield p

    for child in self.children(p):
        for other in self._subtree_preorder(child):
            yield other

def preorder(self):
    if not self.is_empty():
        for p in self._subtree_preorder(self.root()):
            yield p
```

Used when the parent node should be processed before its children. Recursive helper function for preorder traversal.

```
   A
 / | \
B  C  D
```

return 
```
A B C D
```

### 3.2 postorder(self)

Performs a postorder traversal of the tree.

**Traversal Order**
- Traverse all child subtrees (left - right)
- Visit the current node

```python
def _subtree_postorder(self, p):
    for child in self.children(p):
        for other in self._subtree_postorder(child):
            yield other

    yield p

def postorder(self):
    if not self.is_empty():
        for p in self._subtree_postorder(self.root()):
            yield p
```

Useful when child nodes must be processed before their parent. Recursive helper function for postorder traversal.

```
   A
 / | \
B  C  D
```

return 
```
B C D A
```

### 3.3 inorder(self)

Performs an inorder traversal of the tree.

Traversal Order
1. Traverse the left subtree
2. Visit the current node
3. Traverse the right subtree

```python
def _subtree_inorder(self, p):

    children = list(self.children(p))

    if len(children) > 0:
        for other in self._subtree_inorder(children[0]):
            yield other

    yield p

    if len(children) > 1:
        for other in self._subtree_inorder(children[1]):
            yield other

def inorder(self):
    if not self.is_empty():
        for p in self._subtree_inorder(self.root()):
            yield p
```

```
   A
 / | \
B  C  D
```

return 
```
B A C D
```
Recursive helper function for inorder traversal.

### Full code

```python
class Tree:
    """ADT base class representing a tree structure."""
    class Position:
        def __init__(self, container, node):
            self._container = container
            self._node = node

        def element(self):
            return self._node._element

        def __eq__(self, other):
            return type(other) is type(self) and other._node is self._node

        def __ne__(self, other):
            return not (self == other)

    class _Node:
        def __init__(self, element, parent=None):
            self._element = element
            self._parent = parent
            self._children = []

    # ---------------- Tree constructor ----------------
    def __init__(self):
        self._root = None
        self._size = 0

    # ---------------- Utility methods ----------------
    def _make_position(self, node):
        if node is None:
            return None
        return self.Position(self, node)

    def _validate(self, p):
        if not isinstance(p, self.Position):
            raise TypeError("p must be proper Position type")
        return p._node

    # ---------------- Main methods ----------------
    def root(self):
        """Return root Position."""
        return self._make_position(self._root)

    def parent(self, p):
        """Return parent Position of p."""
        node = self._validate(p)
        return self._make_position(node._parent)

    def num_children(self, p):
        """Return number of children of p."""
        node = self._validate(p)
        return len(node._children)

    def children(self, p):
        """Generate children Positions."""
        node = self._validate(p)

        for child in node._children:
            yield self._make_position(child)

    def __len__(self):
        """Return total number of elements."""
        return self._size

    # ---------------- Concrete methods ----------------
    def is_root(self, p):
        return self.root() == p

    def is_leaf(self, p):
        return self.num_children(p) == 0

    def is_empty(self):
        return len(self) == 0

    # ---------------- Update methods ----------------
    def add_root(self, e):
        """Create root node."""
        if self._root is not None:
            raise ValueError("Root already exists")

        self._root = self._Node(e)
        self._size = 1

        return self._make_position(self._root)

    def add_child(self, p, e):
        """Add child node to p."""
        node = self._validate(p)

        child = self._Node(e, node)
        node._children.append(child)

        self._size += 1

        return self._make_position(child)

    def _subtree_preorder(self, p):
        yield p

        for child in self.children(p):
            for other in self._subtree_preorder(child):
                yield other

    def preorder(self):
        if not self.is_empty():
            for p in self._subtree_preorder(self.root()):
                yield p
                
    def _subtree_postorder(self, p):
        for child in self.children(p):
            for other in self._subtree_postorder(child):
                yield other

        yield p

    def postorder(self):
        if not self.is_empty():
            for p in self._subtree_postorder(self.root()):
                yield p

    def _subtree_inorder(self, p):
        children = list(self.children(p))

        if len(children) > 0:
            for other in self._subtree_inorder(children[0]):
                yield other

        yield p

        if len(children) > 1:
            for other in self._subtree_inorder(children[1]):
                yield other

    def inorder(self):
        if not self.is_empty():
            for p in self._subtree_inorder(self.root()):
                yield p
```

**Using:**

```python
t = Tree()

r = t.add_root("A")

b = t.add_child(r, "B")
c = t.add_child(r, "C")

print(t.root().element())
print(t.num_children(r))
print(t.is_leaf(b))

for child in t.children(r):
    print(child.element())
```

**Result:**

```
A
2
True
B
C
```