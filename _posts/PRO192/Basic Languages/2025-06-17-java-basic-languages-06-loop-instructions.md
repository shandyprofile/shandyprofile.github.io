---
title: Loop Instructions
description: >-
  Loops allow a set of instructions to be executed repeatedly based on a condition.
author: [shandy]
date: 2025-06-17
updateDate: 
categories: [(Java) Object-oriented programming, (Java) Basic Language]
tags: [(Java) Basic Language]
sort_index: 106
# pin: true
# media_subpath: '/posts/02'
---

Java has three main types of loops:

| Loop Type  | Use Case                                              |
| ---------- | ----------------------------------------------------- |
| `for`      | Known number of repetitions                           |
| `while`    | Unknown repetitions, check before each iteration      |
| `do-while` | Similar to `while`, but always runs **at least once** |

## 1. For Loop
- Syntax:
```java
for (initialization; condition; update) {
    // code block
}
```
- Example:
```java
for (int i = 1; i <= 5; i++) {
    System.out.println("Count: " + i);
}
```
- Flow:
  1. Initialize i = 1
  2. Check condition i <= 5
  3. Execute code
  4. Update i++
  5. Repeat step 2.

## 2. While Loop
- Syntax:
```java
while (condition) {
    // code block
}
```

- Example:
```java
int i = 1;
while (i <= 5) {
    System.out.println("Count: " + i);
    i++;
}
```
- Flow:
  1. Check condition.
  2. If true, run the block and repeat step 1.
  3. Else, end process.

## 3. Do-while Loop
- Syntax:
```java
do {
    // code block
} while (condition);
```
- Example:
```java
int i = 1;
do {
    System.out.println("Count: " + i);
    i++;
} while (i <= 5);
```
- Flow:
  1. Run the block once
  2. Check the condition.
  3. If true, run the block and repeat step 2.
  4. Else, end process.

## 4. Loop Control: break and continue

- `break`: exit the loop early.
- `continue`: skip the current iteration.
---
- Example:
```java
for (int i = 1; i <= 5; i++) {
    if (i == 3) continue;  // skip 3
    if (i == 5) break;     // stop at 4
    System.out.println(i);
}
```
## Code Demo
```java
public class LoopExamples {
    public static void main(String[] args) {
        // for loop
        for (int i = 0; i < 3; i++) {
            System.out.println("for loop: " + i);
        }

        // while loop
        int j = 0;
        while (j < 3) {
            System.out.println("while loop: " + j);
            j++;
        }

        // do-while loop
        int k = 0;
        do {
            System.out.println("do-while loop: " + k);
            k++;
        } while (k < 3);
    }
}
```
**Output:**
```console
for loop: 0
for loop: 1
for loop: 2
while loop: 0
while loop: 1
while loop: 2
do-while loop: 0
do-while loop: 1
do-while loop: 2
```