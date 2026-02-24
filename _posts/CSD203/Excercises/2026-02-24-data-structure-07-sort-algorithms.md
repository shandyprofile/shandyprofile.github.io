---
title: "Sort Algorithms"
description: >-
  Practice for sort-algorithms
author: [shandy]
date: 2026-02-24
categories: [(Python) Data Structure and Algorithms, Exercises]
tags: [(Python) Data Structure and Algorithms - Exercises]
sort_index: 406
# pin: true
# media_subpath: '/posts/01'
---

## 1. Merge sort - Sorting Customer Orders by Multi-Level Priority

An e-commerce platform needs to sort a list of customer orders before processing shipments.

Each order is represented as an object with the following attributes:
- orderID (String)
- customerName (String)
- orderValue (double)
- isPremiumCustomer (boolean)
- orderDate (Date)

Your task is to implement Merge Sort to sort the orders based on the following rules:
1. Premium customers' orders come first
2. If both customers are the same type → sort by orderValue (descending)
3. If order values are equal → sort by orderDate (ascending)

> You must implement this sorting using Merge Sort only (no built-in sorting functions allowed).

**Input**

- A list of n Order objects
- Each object contains:

```
orderID customerName orderValue isPremiumCustomer orderDate
```

Examples:

```
5
O01 Alice 120.5 true 2024-01-02
O02 Bob 80.0 false 2024-01-03
O03 Carol 120.5 true 2023-12-30
O04 David 200.0 false 2024-01-01
O05 Eva 120.5 true 2024-01-01
```

**Output**

Sorted list of orders based on the priority rules above:
```
O03 Carol 120.5 true 2023-12-30
O05 Eva 120.5 true 2024-01-01
O01 Alice 120.5 true 2024-01-02
O04 David 200.0 false 2024-01-01
O02 Bob 80.0 false 2024-01-03
```

## 2. Quick Sort - Sorting Products by Custom Rating Score

An online marketplace ranks products using a custom score calculated from multiple attributes:

Each product has:
- productID (String)
- price (double)
- rating (float)
- reviewCount (int)

Define a custom score:

$$\text{score} = \text{rating} \times \log(\text{reviewCount} + 1) - 0.01 \times \text{price}$$

Your task:
- Implement Quick Sort
- Sort the products based on: score (**descending**)

**Input**

```
4
P01 100.0 4.5 200
P02 80.0 4.2 500
P03 120.0 4.8 150
P04 60.0 3.9 1000
```

**Output**

```
P02 80.0 4.2 500
P01 100.0 4.5 200
P03 120.0 4.8 150
P04 60.0 3.9 1000
```

## 3. Radix Sort - Sorting Transaction Records by Timestamp

A banking system stores transaction records using a timestamp string in the format:

```
YYYYMMDDHHMMSS
```

Example:

```
20240102153045
```

Each transaction contains:
- transactionID (String)
- timestamp (String)
- amount (double)

You must:
- Implement Radix Sort
- Sort transactions by: timestamp (**ascending**)

**Input**

```
4
T01 20240102153045 1500
T02 20231231120000 2000
T03 20240102153045 1800
T04 20240101100000 900
```

**Output**

```
T02 20231231120000 2000
T04 20240101100000 900
T03 20240102153045 1800
T01 20240102153045 1500
```