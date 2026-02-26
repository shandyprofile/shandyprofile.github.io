---
title: "Binary Search Tree (BST)"
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

# 1. Binary Search Tree (BST)

A Binary Search Tree is a data structure used in computer science for organizing and storing data in a sorted manner. Each node in a Binary Search Tree has at most two children, a left child and a right child.

## Properties

- The left side of a node only has smaller values.
- The right side of a node only has bigger values.
- Both the left and right sides follow the same rule, forming a smaller binary search tree.

![](/assets/img/2026-02-26-07-07-22.png)

In this binary tree
- Root node: 8
- Left subtree (3): values < 8
- Right subtree (10): values > 8
- Each subtree maintains the same rule, with values relative to their respective roots.

## Creation of Binary Search Tree (BST) in Python

Below, are the steps to create a Binary Search Tree (BST).

1. Create a class Tree
2. In the parameterized constructor, we have the default parameter val = None
3. When the Tree class is instantiated without any parameter then it creates an empty node with left and right pointers
4. If we pass the value while instantiating then it creates a node having that value and left, right pointers are created with None Types.

```python
class Node:
    def __init__(self, key):
        self.value = key  
        self.left = None  
        self.right = None    

# Creating the root node
root = Node(5)
```

## Operations on Binary Search Tree (BST)

### 1. Insertion in Binary Search Tree

Given the root of a Binary Search Tree, we need to insert a new node with given value in the BST. All the nodes have distinct values in the BST and we may assume that the the new value to be inserted is not present in BST.

Example:

![](/assets/img/2026-02-26-07-10-08.png)

A new key is inserted at the position that maintains the BST property. We start from the root and move downward: if the key is smaller, go left; if larger, go right. We continue until we find an unoccupied spot where the node can be placed without violating the BST property, and insert it there as a new leaf.

- Step 1: Consider the following BST:

![](/assets/img/2026-02-26-07-10-26.png)

- Step 2: Since 22 is greater than key (15), move the pointer to left child

![](/assets/img/2026-02-26-07-20-05.png)

- Step 3: Since 12 is less than key (15), move the pointer to the right

![](/assets/img/2026-02-26-07-20-58.png)

....

```python
from collections import deque

# Node structure
class Node:
    def __init__(self, val):
        self.data = val
        self.left = None
        self.right = None

def getHeight(root, h):
    if root is None:
        return h - 1
    return max(getHeight(root.left, h + 1), getHeight(root.right, h + 1))

def levelOrder(root):
    queue = deque()
    queue.append((root, 0))

    lastLevel = 0
    height = getHeight(root, 0)

    while queue:
        node, lvl = queue.popleft()

        if lvl > lastLevel:
            print()
            lastLevel = lvl

        # all levels are printed
        if lvl > height:
            break

        # printing null node
        print("N" if node.data == -1 else node.data, end=" ")

        # null node has no children
        if node.data == -1:
            continue

        if node.left is None:
            queue.append((Node(-1), lvl + 1))
        else:
            queue.append((node.left, lvl + 1))

        if node.right is None:
            queue.append((Node(-1), lvl + 1))
        else:
            queue.append((node.right, lvl + 1))
#Driver Code Ends

def insert(root, key):
 
    # If the tree is empty, return a new node
    if root is None:
        return Node(key)

    # Otherwise, recur down the tree
    if key < root.data:
        root.left = insert(root.left, key)
    else:
        root.right = insert(root.right, key)

    # Return the (unchanged) node pointer
    return root
#Driver Code Starts

# Create BST
#       22
#      /  \
#     12   30
#     / \   
#    8  20
#       / \
#      15  30

root = None
root = insert(root, 22)
root = insert(root, 12)
root = insert(root, 30)
root = insert(root, 8)
root = insert(root, 20)
root = insert(root, 30)
root = insert(root, 15)

# print the level order 
# traversal of the BST
levelOrder(root)
```

Output:

```
22 
12 30 
8 20 N 30 
N N 15 N N N 
```

### 2. Searching in Binary Search Tree (BST)

Given the root of a Binary Search Tree and a value key, find if key is present in the BST or not.

Note: The key may or may not be present in the BST.

![](/assets/img/2026-02-26-07-22-30.png)

Let's say we want to search for the number key, We start at the root. Then:

1. We compare the value to be searched with the value of the root. 
2. If it's equal we are done with the search.
3. If it's smaller we know that we need to go to the left subtree.
4. If it's greater we search in the right subtree. 
5. Repeat the above step till no more traversal is possible
6. If at any iteration, key is found, return True. If the node is null, return False.

```python
# Node structure
class Node:
    def __init__(self, item):
        self.data = item
        self.left = None
        self.right = None

def search(root, key):
   
    # root is null -> return false
    if root is None:
        return False

    # if root has key -> return true
    if root.data == key:
        return True

    if key > root.data:
        return search(root.right, key)

    return search(root.left, key)

# Creating BST
#     6
#   / \
#   2   8
#      / \
#     7   9
root = Node(6)
root.left = Node(2)
root.right = Node(8)
root.right.left = Node(7)
root.right.right = Node(9)

key = 7
# Searching for key in the BST
print(search(root, key))
```

### 3. Deletion in Binary Search Tree (BST)

Given the root of a Binary Search Tree (BST) and an integer x, delete the node with value x from the BST while maintaining the BST property.

Input: x = 15

![](/assets/img/2026-02-26-07-24-02.png)

Output: [[10], [5, 18], [N, N, 12, N]]
Explanation:  The node with value x (15) is deleted from BST.

![](/assets/img/2026-02-26-07-53-37.png)

Deleting a node in a BST means removing the target node while ensuring that the tree remains a valid BST. Depending on the structure of the node to be deleted, there are three possible scenarios:

- Case 1: Node has No Children (Leaf Node)

If the target node is a leaf node, it can be directly removed from the tree since it has no child to maintain.

Working:

![](/assets/img/2026-02-26-07-24-36.png)

- Case 2: Node has One Child(Left or Right Child)

If the target node has only one child, we remove the node and connect its parent directly to its only child. This way, the tree remains valid after deletion of target node.

Working:

![](/assets/img/2026-02-26-07-25-12.png)

![](/assets/img/2026-02-26-07-25-26.png)

![](/assets/img/2026-02-26-07-25-34.png)

![](/assets/img/2026-02-26-07-25-41.png)

![](/assets/img/2026-02-26-07-25-55.png)

![](/assets/img/2026-02-26-07-26-04.png)

- Case 3: Node has Two Children

If the target node has two children, deletion is slightly more complex.

To maintain the BST property, we need to find a replacement node for the target. The replacement can be either:

- The inorder successor — the smallest value in the right subtree, which is the next greater value than the target node.
- The inorder predecessor — the largest value in the left subtree, which is the next smaller value than the target node.
- Once the replacement node is chosen, we replace the target node’s value with that node’s value, and then delete the replacement node, which will now fall under Case 1 (no children) or Case 2 (one child).

> Note: Inorder predecessor can also be used.

![](/assets/img/2026-02-26-07-26-40.png)

![](/assets/img/2026-02-26-07-26-47.png)

![](/assets/img/2026-02-26-07-26-55.png)

![](/assets/img/2026-02-26-07-27-04.png)

![](/assets/img/2026-02-26-07-27-28.png)

The deletion process in BST depends on the number of children of the node.

- No children means simply remove the node.
- One child means remove the node and connect its parent to the node’s only child.
- Two children means replace the node with its inorder successor/predecessor and delete that node.

This ensures that the BST property remains intact after every deletion.

```python
# Get inorder successor (smallest in right subtree)
def getSuccessor(curr):
    curr = curr.right
    while curr is not None and curr.left is not None:
        curr = curr.left
    return curr

# Delete a node with value x from BST
def delNode(root, x):
    if root is None:
        return root

    if root.data > x:
        root.left = delNode(root.left, x)
    elif root.data < x:
        root.right = delNode(root.right, x)
    else:
        
        # node with 0 or 1  children
        if root.left is None:
            return root.right
        if root.right is None:
            return root.left
        
        #  Node with 2 children
        succ = getSuccessor(root)
        root.data = succ.data
        root.right = delNode(root.right, succ.data)

    return root

if  __name__ == "__main__":
    root = Node(10)
    root.left = Node(5)
    root.right = Node(15)
    root.right.left = Node(12)
    root.right.right = Node(18)
    
    x = 15
    root = delNode(root, x)
    levelOrder(root)
```

Output

```
10 
5 18 
N N 12 N 
```

### 4. Binary Search Tree (BST) Traversals – Inorder, Preorder, Post Order

Given a Binary Search Tree, The task is to print the elements in inorder, preorder, and postorder traversal of the Binary Search Tree. 

Input: 

![](/assets/img/2026-02-26-07-28-45.png)

Output: 

```
Inorder Traversal: 10 20 30 100 150 200 300
Preorder Traversal: 100 20 10 30 200 150 300
Postorder Traversal: 10 30 20 150 300 200 100
```

Input: 

![](/assets/img/2026-02-26-07-29-14.png)

Output: 

```
Inorder Traversal: 8 12 20 22 25 30 40
Preorder Traversal: 22 12 8 20 30 25 40
Postorder Traversal: 8 20 12 25 40 30 22
```

#### Inorder Traversal:

Below is the idea to solve the problem:

> At first traverse left subtree then visit the root and then traverse the right subtree.

Follow the below steps to implement the idea:

- Traverse left subtree
- Visit the root and print the data.
- Traverse the right subtree

The inorder traversal of the BST gives the values of the nodes in sorted order. To get the decreasing order visit the right, root, and left subtree.

Below is the implementation of the inorder traversal.

```python

# Class describing a node of tree
class Node:
    def __init__(self, v):
        self.left = None
        self.right = None
        self.data = v

# Inorder Traversal
def printInorder(root):
    if root:
        # Traverse left subtree
        printInorder(root.left)
        
        # Visit node
        print(root.data,end=" ")
        
        # Traverse right subtree
        printInorder(root.right)

# Driver code
if __name__ == "__main__":
    # Build the tree
    root = Node(100)
    root.left = Node(20)
    root.right = Node(200)
    root.left.left = Node(10)
    root.left.right = Node(30)
    root.right.left = Node(150)
    root.right.right = Node(300)

    # Function call
    print("Inorder Traversal:",end=" ")
    printInorder(root)

```

Output:

```
Inorder Traversal: 10 20 30 100 150 200 300 
```

#### Preorder Traversal:

Below is the idea to solve the problem:

> At first visit the root then traverse left subtree and then traverse the right subtree.

Follow the below steps to implement the idea:
- Visit the root and print the data.
- Traverse left subtree
- Traverse the right subtree
Below is the implementation of the preorder traversal.

```python

class Node:
    def __init__(self, v):
        self.data = v
        self.left = None
        self.right = None

# Preorder Traversal
def printPreOrder(node):
    if node is None:
        return
    # Visit Node
    print(node.data, end = " ")

    # Traverse left subtree
    printPreOrder(node.left)

    # Traverse right subtree
    printPreOrder(node.right)

# Driver code
if __name__ == "__main__":
    # Build the tree
    root = Node(100)
    root.left = Node(20)
    root.right = Node(200)
    root.left.left = Node(10)
    root.left.right = Node(30)
    root.right.left = Node(150)
    root.right.right = Node(300)

    # Function call
    print("Preorder Traversal: ", end = "")
    printPreOrder(root)

```

Output

```
Preorder Traversal: 100 20 10 30 200 150 300 
```

#### Postorder Traversal:

Below is the idea to solve the problem:

> At first traverse left subtree then traverse the right subtree and then visit the root.

Follow the below steps to implement the idea:

- Traverse left subtree
- Traverse the right subtree
- Visit the root and print the data.

Below is the implementation of the postorder traversal:

```python
class Node:
    def __init__(self, v):
        self.data = v
        self.left = None
        self.right = None

# Preorder Traversal
def printPostOrder(node):
    if node is None:
        return

    # Traverse left subtree
    printPostOrder(node.left)

    # Traverse right subtree
    printPostOrder(node.right)
    
    # Visit Node
    print(node.data, end = " ")

# Driver code
if __name__ == "__main__":
    # Build the tree
    root = Node(100)
    root.left = Node(20)
    root.right = Node(200)
    root.left.left = Node(10)
    root.left.right = Node(30)
    root.right.left = Node(150)
    root.right.right = Node(300)

    # Function call
    print("Postorder Traversal: ", end = "")
    printPostOrder(root)

```

Output

```
PostOrder Traversal: 10 30 20 150 300 200 100 
```