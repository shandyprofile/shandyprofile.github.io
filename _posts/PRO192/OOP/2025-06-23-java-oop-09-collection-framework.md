---
title: 'Collection Framework'
description: >-
  The Java Collection Framework (JCF) is a unified architecture for representing and manipulating groups of objects.
author: [shandy]
date: 2025-06-23
updateDate:
categories: [(Java) Object-oriented programming, (Java) OOP]
tags: [(Java) OOP]
sort_index: 9
# pin: true
# media_subpath: '/posts/02'
---

## 1. Collection Framework
The Java Collection Framework (JCF) is a unified architecture for representing and manipulating groups of objects.

It provides:
- Interfaces (e.g., List, Set, Map)
- Implementations (e.g., ArrayList, HashSet, HashMap)
- Algorithms (e.g., sorting, searching)
- Utility classes (e.g., Collections, Arrays)

## 2. Using the Collection Framework
- Provides ready-to-use data structures
- Reduces development time
- Increases code reusability
- Offers flexible and powerful APIs

## 3. Core Interfaces

| Interface | Description                                | Common Implementations        |
| --------- | ------------------------------------------ | ----------------------------- |
| `List`    | Ordered collection with duplicates allowed | `ArrayList`, `LinkedList`     |
| `Set`     | Unordered collection with no duplicates    | `HashSet`, `TreeSet`          |
| `Map`     | Key-value pairs (no duplicate keys)        | `HashMap`, `TreeMap`          |
| `Queue`   | Ordered for processing (FIFO, LIFO)        | `LinkedList`, `PriorityQueue` |

## 4. Interface Hierarchy Diagram (Simplified)
```
             Collection (interface)
             /        |        \
        List        Set       Queue
         |           |           |
   ArrayList     HashSet     LinkedList
                            PriorityQueue

             Map (interface)
                 |
             HashMap
```

## 5. Key Components Overview
### A. List – Ordered, Indexed, Allows Duplicates
**Key Characteristics**
- Maintains insertion order
- Allows duplicate elements
- Access elements using index
- Supports null elements

**Common Implementations**
| Class        | Description                               |
| ------------ | ----------------------------------------- |
| `ArrayList`  | Fast random access, resizable array       |
| `LinkedList` | Good for frequent insertions/deletions    |
| `Vector`     | Legacy, synchronized version of ArrayList |

**Useful Methods in List**
| Method               | Description                     |
| -------------------- | ------------------------------- |
| `add(E e)`           | Adds element to the end         |
| `add(int index, E)`  | Adds element at specified index |
| `get(int index)`     | Returns element at index        |
| `remove(int index)`  | Removes element at index        |
| `contains(Object o)` | Checks if list contains element |
| `size()`             | Returns number of elements      |
| `clear()`            | Removes all elements            |

**Looping Through a List**
```java
for (String fruit : fruits) {
    System.out.println(fruit);
}
```

Or using indices:
```java
for (int i = 0; i < fruits.size(); i++) {
    System.out.println(fruits.get(i));
}
```

**Example**
```java
import java.util.*;

public class ListExample {
    public static void main(String[] args) {
        List<String> fruits = new ArrayList<>();
        
        fruits.add("Apple");
        fruits.add("Banana");
        fruits.add("Orange");
        fruits.add("Apple"); // duplicate allowed

        System.out.println("List: " + fruits);
        System.out.println("First fruit: " + fruits.get(0));
    }
}
```

### B. Set – No Duplicates, Unordered

**Key Characteristics**
- No duplicate elements allowed.
- No index-based access (unlike List).
- At most one null element (in most implementations).
- Order is not guaranteed (unless using specific implementations).

**Common Implementations**
| Class           | Description                                 |
| --------------- | ------------------------------------------- |
| `HashSet`       | Fastest access, does **not** maintain order |
| `LinkedHashSet` | Maintains **insertion order**               |
| `TreeSet`       | Automatically **sorted**, no `null` allowed |

**Example**
```java
import java.util.*;

public class SetExample {
    public static void main(String[] args) {
        Set<String> cities = new HashSet<>();

        cities.add("Hanoi");
        cities.add("Da Nang");
        cities.add("Ho Chi Minh");
        cities.add("Hanoi"); // duplicate, will be ignored

        System.out.println("Cities: " + cities);
    }
}

// Output
// Cities: [Da Nang, Ho Chi Minh, Hanoi]
```

**Useful Methods in Set**
| Method               | Description                                    |
| -------------------- | ---------------------------------------------- |
| `add(E e)`           | Adds an element (returns `false` if duplicate) |
| `remove(Object o)`   | Removes element                                |
| `contains(Object o)` | Checks if element exists                       |
| `size()`             | Number of elements                             |
| `clear()`            | Removes all elements                           |
| `isEmpty()`          | Checks if the set is empty                     |

**Example: Using TreeSet (Sorted)**
```java
Set<Integer> numbers = new TreeSet<>();
numbers.add(5);
numbers.add(1);
numbers.add(3);

System.out.println(numbers); // [1, 3, 5] — sorted automatically
```

**Looping Through a Set**
```java
for (String city : cities) {
    System.out.println(city);
}
```

**Use Set**
| Use Case                            | Recommended         |
| ----------------------------------- | ------------------- |
| Ensure uniqueness (no duplicates)   | Yes                   |
| Need fast lookups (check existence) | Yes                   |
| Want sorted order                   | Use `TreeSet`       |
| Need order of insertion             | Use `LinkedHashSet` |

### C. Map – Key-Value Pairs, No Duplicate Keys

**Key Characteristics**
- Stores key-value pairs
- Keys are unique, but values can duplicate
- Allows null keys (only one) and null values (multiple)
- No direct indexing like List

**Common Implementations**
| Class           | Description                                  |
| --------------- | -------------------------------------------- |
| `HashMap`       | Unordered, allows one `null` key             |
| `LinkedHashMap` | Maintains insertion order                    |
| `TreeMap`       | Automatically sorted by keys, no `null` keys |

**Example:**
```java
import java.util.*;

public class MapExample {
    public static void main(String[] args) {
        Map<String, Integer> ages = new HashMap<>();

        ages.put("Alice", 25);
        ages.put("Bob", 30);
        ages.put("Charlie", 22);
        ages.put("Alice", 28); // Key already exists, value is updated

        System.out.println("Alice's age: " + ages.get("Alice")); // 28
        System.out.println("All entries: " + ages);
    }
}
```
**Useful Methods in Map**
| Method                    | Description                          |
| ------------------------- | ------------------------------------ |
| `put(K key, V value)`     | Adds or updates entry                |
| `get(Object key)`         | Retrieves value for the key          |
| `remove(Object key)`      | Deletes entry by key                 |
| `containsKey(Object k)`   | Checks if key exists                 |
| `containsValue(Object v)` | Checks if a value exists             |
| `keySet()`                | Returns a `Set` of all keys          |
| `values()`                | Returns a `Collection` of all values |
| `entrySet()`              | Returns a `Set` of key-value pairs   |

**Looping Through a Map**
```java
for (Map.Entry<String, Integer> entry : ages.entrySet()) {
    System.out.println(entry.getKey() + " is " + entry.getValue() + " years old.");
}
```
**Example: Sorted Map (TreeMap)**
```java
Map<Integer, String> idNames = new TreeMap<>();
idNames.put(3, "C");
idNames.put(1, "A");
idNames.put(2, "B");

System.out.println(idNames); // Sorted by keys: {1=A, 2=B, 3=C}
```
**Use Map**
| Use Case                   | Recommended Implementation |
| -------------------------- | -------------------------- |
| Fast lookups by unique key | `HashMap`                  |
| Maintain insertion order   | `LinkedHashMap`            |
| Automatically sorted keys  | `TreeMap`                  |

## 6. Utility Class: Collections
Java provides the Collections class for sorting, searching, shuffling, etc.

```java
List<Integer> nums = Arrays.asList(3, 1, 4, 1, 5);
Collections.sort(nums);
System.out.println(nums); // [1, 1, 3, 4, 5]
```
## 7. Generics in Collections
Collections use generics to ensure type safety:

```java
List<String> strings = new ArrayList<>();
strings.add("hello");
// strings.add(123); // compile-time error
```

## Summary:

| Feature           | `List`    | `Set`                | `Map`                      |
| ----------------- | --------- | -------------------- | -------------------------- |
| Allows Duplicates | Yes         | -                    | - (keys)                   |
| Maintains Order   | Yes (index) | - (unless `TreeSet`) | - (unless `LinkedHashMap`) |
| Access by Index   | Yes         | -                   | -                          |
| Key-Value Store   | -         | -                    | Yes                          |