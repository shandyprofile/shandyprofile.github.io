---
title: "Sort Algorithms"
description: >-
  Practice for sort-algorithms
author: [shandy]
date: 2026-02-24
categories: [(Python) Data Structure and Algorithms, Theosis]
tags: [(Python) Data Structure and Algorithms]
sort_index: 106
# pin: true
# media_subpath: '/posts/01'
---

## 1. Merge sort

Merge sort is a popular sorting algorithm known for its efficiency and stability. It follows the Divide and Conquer approach. It works by recursively dividing the input array into two halves, recursively sorting the two halves and finally merging them back together to obtain the sorted array.

![](/assets/img/2026-02-24-07-19-46.png)

Here's a step-by-step explanation of how merge sort works:

Divide: Divide the list or array recursively into two halves until it can no more be divided.
Conquer: Each subarray is sorted individually using the merge sort algorithm.
Merge: The sorted subarrays are merged back together in sorted order. The process continues until all elements from both subarrays have been merged.

Let's sort the array or list [38, 27, 43, 10] using Merge Sort

- Step 1: Spliting the Array into two equal halves

![](/assets/img/2026-02-24-07-20-49.png)

- Step 2: Spliting the subarray into two halves

![](/assets/img/2026-02-24-07-21-01.png)

- Step 3: Merging unit length cells into sorted subarrays

![](/assets/img/2026-02-24-07-23-15.png)

- Step 4: Merging sorted subarrays into the sorted array

![](/assets/img/2026-02-24-07-24-04.png)

```
Let's look at the working of above example: 

Divide: 

[38, 27, 43, 10]  is divided into  [38, 27] and  [43, 10]  . 
[38, 27]  is divided into  [38]  and  [27]  . 
[43, 10]  is divided into  [43]  and  [10]  . 
Conquer: 

[38]  is already sorted. 
[27]  is already sorted. 
[43]  is already sorted. 
[10]  is already sorted. 
Merge: 

Merge  [38]  and  [27]  to get  [27, 38]  . 
Merge  [43]  and  [10]  to get  [10,43]  . 
Merge  [27, 38]  and  [10,43]  to get the final sorted list  [10, 27, 38, 43] 
Therefore, the sorted list is  [10, 27, 38, 43]  . 
```

```python 
def merge(arr, left, mid, right):
    n1 = mid - left + 1
    n2 = right - mid

    # Create temp arrays
    L = [0] * n1
    R = [0] * n2

    # Copy data to temp arrays L[] and R[]
    for i in range(n1):
        L[i] = arr[left + i]
    for j in range(n2):
        R[j] = arr[mid + 1 + j]
        
    i = 0  
    j = 0  
    k = left  

    # Merge the temp arrays back
    # into arr[left..right]
    while i < n1 and j < n2:
        if L[i] <= R[j]:
            arr[k] = L[i]
            i += 1
        else:
            arr[k] = R[j]
            j += 1
        k += 1

    # Copy the remaining elements of L[],
    # if there are any
    while i < n1:
        arr[k] = L[i]
        i += 1
        k += 1

    # Copy the remaining elements of R[], 
    # if there are any
    while j < n2:
        arr[k] = R[j]
        j += 1
        k += 1

def mergeSort(arr, left, right):
    if left < right:
        mid = (left + right) // 2

        mergeSort(arr, left, mid)
        mergeSort(arr, mid + 1, right)
        merge(arr, left, mid, right)

# Driver code
if __name__ == "__main__":
    arr = [38, 27, 43, 10]
   
    mergeSort(arr, 0, len(arr) - 1)
    for i in arr:
        print(i, end=" ")
    print()
```

> Output

```
10 27 38 43
```

### Recurrence Relation of Merge Sort

The recurrence relation of merge sort is:

$$
T(n) =
\begin{cases}
\Theta(1) & \text{if } n = 1 \\
2T\left(\frac{n}{2}\right) + \Theta(n) & \text{if } n > 1
\end{cases}
$$

- T(n) Represents the total time time taken by the algorithm to sort an array of size n.
- 2T(n/2) represents time taken by the algorithm to recursively sort the two halves of the array. Since each half has n/2 elements, we have two - - recursive calls with input size as (n/2).
- O(n) represents the time taken to merge the two sorted halves

### Time & Space Complexity of Merge Sort

| Case                | Time Complexity | Condition                            | Explanation                                |
| ------------------- | --------------- | ------------------------------------ | ------------------------------------------ |
| **Best Case**       | O(n log n)      | Array already sorted / nearly sorted | Still needs to divide and merge subarrays  |
| **Average Case**    | O(n log n)      | Randomly ordered array               | Balanced divide-and-conquer process        |
| **Worst Case**      | O(n log n)      | Reverse sorted array                 | Same merging cost at every recursion level |
| **Auxiliary Space** | O(n)            | Temporary arrays used during merging | Required for stable merging process        |

### Applications of Merge Sort

| Application Area        | Description                                   |
| ----------------------- | --------------------------------------------- |
| Large Dataset Sorting   | Efficient for handling huge input sizes       |
| External Sorting        | Suitable when data cannot fit into RAM        |
| Inversion Counting      | Used to count number of inversions            |
| Count Smaller on Right  | Applied in range query problems               |
| Surpasser Count         | Useful in comparative analysis problems       |
| Library Implementations | Used in built-in sorting methods              |
| Linked List Sorting     | Preferred due to sequential access nature     |
| Parallel Processing     | Subarrays can be sorted independently         |
| Set Operations          | Efficient union/intersection of sorted arrays |

### Advantages and Disadvantages

| Advantage                | Explanation                       |
| ------------------------ | --------------------------------- |
| Stability                | Maintains order of equal elements |
| Guaranteed Worst-case    | Always O(n log n)                 |
| Simple Implementation    | Divide-and-conquer approach       |
| Naturally Parallelizable | Independent sorting of subarrays  |

| Disadvantage       | Explanation                       |
| ------------------ | --------------------------------- |
| High Space Usage   | Requires additional memory        |
| Not In-place       | Needs temporary arrays            |
| Cache Inefficiency | Slower than QuickSort in practice |

## 2. Quick sort

QuickSort is a sorting algorithm based on the Divide and Conquer that picks an element as a pivot and partitions the given array around the picked pivot by placing the pivot in its correct position in the sorted array.

There are mainly three steps in the algorithm:

![](/assets/img/2026-02-24-08-12-52.png)

- Step 1 (Choose a Pivot): Select an element from the array as the pivot. The choice of pivot can vary (e.g., first element, last element, random element, or median).

- Step 2 (Partition the Array): Re arrange the array around the pivot. After partitioning, all elements smaller than the pivot will be on its left, and all elements greater than the pivot will be on its right.

- Step 3 (Recursively Call): Recursively apply the same process to the two partitioned sub-arrays.

- Step 4 (Base Case): The recursion stops when there is only one element left in the sub-array, as a single element is already sorted.

### Choice of Pivot

| Strategy                   | Definition                                                      | Advantages                                     | Disadvantages                                     | Time Complexity Impact   |
| -------------------------- | --------------------------------------------------------------- | ---------------------------------------------- | ------------------------------------------------- | ------------------------ |
| First / Last Element Pivot | Selects the first or last element of the array as the pivot     | Easy to implement                              | Performs poorly on sorted or nearly sorted arrays | Worst Case: O(n²)        |
| Random Pivot               | Selects a pivot randomly from the array                         | Reduces chance of worst-case partitioning      | Requires random number generation                 | Average Case: O(n log n) |
| Median Pivot               | Selects the median value of the array as the pivot              | Produces balanced partitions                   | Median finding has high constant cost             | Best Case: O(n log n)    |
| Median-of-Three Pivot      | Chooses median of first, middle, and last elements as the pivot | Avoids worst-case in many practical situations | Slightly more complex implementation              | Average Case: O(n log n) |

### Example:

- Step 01:
Pivot Selection: The last element arr[4] = 40 is chosen as the pivot. Initial Pointers: i = -1 and j = 0.

![](/assets/img/2026-02-24-08-34-38.png)

- Step 02:
Since arr[j] < pivot (10 < 40)
Increment i to 0 and swap arr[i] with arr[j]. Increment j by 1.

![](/assets/img/2026-02-24-08-35-05.png)

- Step 03:
Since arr[j] > pivot (80 > 40)
No swap needed. Increment j by 1.

![](/assets/img/2026-02-24-08-35-17.png)

- Step 04:
Since arr[j] < pivot (30 < 40)
Increment i by 1 and swap arr[i] with arr[j]. Increment j by 1.

![](/assets/img/2026-02-24-08-35-31.png)

- Step 05:
Since arr[j] > pivot (90 > 40)
No swap needed. Increment j by 1.

![](/assets/img/2026-02-24-08-35-42.png)

- Step 06:
Since traversal of j has ended, now move pivot to its correct position.
Swap arr[i + 1] = arr[2] with arr[4] = 40.

![](/assets/img/2026-02-24-08-36-01.png)

```python

// partition function
function partition(arr, low, high)
{

    // choose the pivot
    let pivot = arr[high];

    // index of smaller element and indicates
    // the right position of pivot found so far
    let i = low - 1;

    // traverse arr[low..high] and move all smaller
    // elements to the left side. Elements from low to
    // i are smaller after every iteration
    for (let j = low; j <= high - 1; j++) {
        if (arr[j] < pivot) {
            i++;
            swap(arr, i, j);
        }
    }

    // move pivot after smaller elements and
    // return its position
    swap(arr, i + 1, high);
    return i + 1;
}

// swap function
function swap(arr, i, j)
{
    let temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
}

// the QuickSort function implementation
function quickSort(arr, low, high)
{
    if (low < high) {

        // pi is the partition return index of pivot
        let pi = partition(arr, low, high);

        // recursion calls for smaller elements
        // and greater or equals elements
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}


// Driver Code
let arr = [ 10, 7, 8, 9, 1, 5 ];
let n = arr.length;

// call QuickSort on the entire array
quickSort(arr, 0, n - 1);
for (let i = 0; i < arr.length; i++) {
    process.stdout.write(arr[i] + " ");
}

```

> Output

```
1 5 7 8 9 10 
```

## 3. Radix Sort

Radix Sort is a linear sorting algorithm (for fixed length digit counts) that sorts elements by processing them digit by digit. It is an efficient sorting algorithm for integers or strings with fixed-size keys. 

- It repeatedly distributes the elements into buckets based on each digit's value. This is different from other algorithms like Merge Sort or Quick Sort where we compare elements directly.
- By repeatedly sorting the elements by their significant digits, from the least significant to the most significant, it achieves the final sorted order.
- We use a stable algorithm like Counting Sort to sort the individual digits so that the overall algorithm remains stable.

Example:

To perform radix sort on the array [170, 45, 75, 90, 802, 24, 2, 66], we follow these steps:

- Step 1: Find the largest element, which is 802. It has three digits, so we will iterate three times.


- Step 2: Sort the elements based on the unit place digits (X=0).  

![](/assets/img/2026-02-24-09-31-48.png)

- Step 3: Sort the elements based on the tens place digits.

![](/assets/img/2026-02-24-09-32-36.png)

- Step 4: Sort the elements based on the hundreds place digits.

![](/assets/img/2026-02-24-09-32-56.png)

- Step 5: The array is now sorted in ascending order.

![](/assets/img/2026-02-24-09-33-12.png)

```python
# Python program for implementation of Radix Sort
# A function to do counting sort of arr[] according to
# the digit represented by exp.


def countingSort(arr, exp1):

    n = len(arr)

    # The output array elements that will have sorted arr
    output = [0] * (n)

    # initialize count array as 0
    count = [0] * (10)

    # Store count of occurrences in count[]
    for i in range(0, n):
        index = arr[i] // exp1
        count[index % 10] += 1

    # Change count[i] so that count[i] now contains actual
    # position of this digit in output array
    for i in range(1, 10):
        count[i] += count[i - 1]

    # Build the output array
    i = n - 1
    while i >= 0:
        index = arr[i] // exp1
        output[count[index % 10] - 1] = arr[i]
        count[index % 10] -= 1
        i -= 1

    # Copying the output array to arr[],
    # so that arr now contains sorted numbers
    i = 0
    for i in range(0, len(arr)):
        arr[i] = output[i]

# Method to do Radix Sort


def radixSort(arr):

    # Find the maximum number to know number of digits
    max1 = max(arr)

    # Do counting sort for every digit. Note that instead
    # of passing digit number, exp is passed. exp is 10^i
    # where i is current digit number
    exp = 1
    while max1 / exp >= 1:
        countingSort(arr, exp)
        exp *= 10


# Driver code
arr = [170, 45, 75, 90, 802, 24, 2, 66]

# Function Call
radixSort(arr)

for i in range(len(arr)):
    print(arr[i], end=" ")

# This code is contributed by Mohit Kumra
# Edited by Patrick Gallagher
```

> Output

```
2 24 45 66 75 90 170 802 
```

Complexity Analysis of Radix Sort

Time Complexity:

Radix Sort is a non-comparative integer sorting algorithm that sorts elements by processing individual digits from the least significant digit (LSD) to the most significant digit (MSD), or vice versa.

The overall time complexity of Radix Sort is:

$$ O(d \times (n + b)) $$

Where:
- d: number of digits in the largest number
- n: number of elements in the input array
- b: base of the numbering system (e.g., 10 for decimal, 2 for binary)

In practice, Radix Sort often performs faster than comparison-based algorithms such as Quick Sort or Merge Sort when handling large datasets with fixed-length integer keys. However, since its running time depends linearly on the number of digits (d), it may not be efficient for small datasets or numbers with a large digit length.

Auxiliary Space Complexity:

Radix Sort requires additional memory for storing temporary buckets during the digit-wise sorting process. Therefore, its auxiliary space complexity is:

$$ O((n + b)) $$

Where:
- n: number of input elements
- b: number of possible digit values (base)

This extra space is needed to distribute elements into buckets and then reconstruct the array after sorting based on each digit.