
## Heaps

We can think of a heap as an array with a special property: the smallest value in the array is always in the first index of the array.

If we remove the smallest value from the heap, the elements of the array efficiently re-arrange so that the next smallest value takes its place at the front of the array.

Heaps are most frequently used in coding interviews to solve a class of problems known as "Top K" problems, which involve finding the k smallest or largest elements in a collection of elements.

This module prepares you to use heaps during the coding interview by covering:

- How heaps are able to achieve their special property efficiently, which enable you to discuss heaps effectively during the interview.

- How to use heaps in Python.
- The types of coding interview questions that are best solved using heaps.

### Heap Properties

Although we can _think_ of a heap as an array, it's more helpful to _visualize_ a heap as a binary tree. For example, we can visualize the heap [1, 2, 4, 5, 8, 6, 9] like this:

![[20250421103326.png]]

The elements correspond to a _level-order_ traversal of the binary tree. The root of the binary tree is stored at index 0, its left and right children are stored at indices 1 and 2, respectively, and so on.

![[20250421103409.png]]

The array representation is shown above the heap. Each node in the binary tree is labeled with the corresponding index in the array.

These two representations of a heap are **equivalent**. The binary tree representation is a more intuitive way to understand how a heap works. But during the coding interview, you will most likely be working with the array representation of a heap, as it more efficient to work with.

Our heap is a **min-heap** because the smallest value in the heap is at the root node (compare that to a **max-heap**, in which the largest value is at the root).

The binary tree satisfies the **heap property**: each node in the tree has a value that is less than or equal to the values of both its children.

![[20250421103451.png]]

Node 2 has a value that is less than both its children (5 and 8).

Any binary tree that satisfies the heap property is a heap:

![[20250421103540.png]]

Left: A valid heap, right: an invalid heap.

Heaps are **complete binary trees**, which means that all levels of the tree are fully filled except for the last level, which is filled from left to right.

![[20250421103607.png]]

Left: A valid heap, right: an invalid heap.

Since heaps are complete binary trees, the **height of a heap is O(log n)**, where n is the number of elements in the heap. This is an important property to keep in mind when analyzing the time complexity of heap operations.

**Max Heap**

The heap property for a **max heap** is that each node has a value that is greater than or equal to the values of both its children.

![[20250421103645.png]]

### Parent-Child Relationship

We can express the parent-child relationships of the binary tree representation of a heap using the indexes of the array. Given a node at index i in the array:

|Node|Index|
|---|---|
|Left Child|2 * i + 1|
|Right Child|2 * i + 2|
|Parent|⌊(i - 1) / 2⌋ (floor division)|

For example, for the node at index i = 2:

- The left child L is at index 2 * 2 + 1 = 5:
- The right child R is at index 2 * 2 + 2 = 6:
- The parent P is at index ⌊(2 - 1) / 2⌋ = 0:


![[20250421103724.png]]

## Heap Operations

A heap supports the following operations:

- _push(element)_: Add a new element to the heap.

- _pop()_: Remove the root element from the heap.

- _peek()_: Get the root element without removing it.

- _heapify([elements])_: Convert an array into a heap in-place.


We'll learn about each of these operations on a **min-heap** by visualizing how both the array and binary tree representation of a heap change after each operation. Doing so will help us understand the time complexity of each operation.

```
We omit the acutal code implementation for these operations, as Python provides a built-in heapq module that you can use to create and manipulate heaps during your interviews.

However, understanding how the operations work conceptually allow you to **discuss heaps** at the level required for the interview. In particular, you need to know how these operations allow the heap to **efficiently maintain** the heap property.
```

### Push

The _push_ operation takes a new element and adds it to the heap. The element is added in a way such that the heap property is maintained.

We'll visualize each step of how the heap on the left changes after we add a new value of 3:

![[20250421103829.png]]

![[20250421103853.png]]

**Time Complexity**

The time complexity of the push operation is O(log n), where n is the number of items in the heap.

In the worst case, the new element will start at the last level of the tree and will "bubble-up" to the root, which takes O(log n) swaps, or the height of the tree. Since we are swapping indexes in an array, each swap operation takes O(1) time.

### Pop

The _pop_ operation removes and returns the minimum value from the heap. When the pop operation is complete, the new root of the heap is the new minimum value in the heap, and the heap property is restored.

We'll visualize each step of how the heap on the left changes after we remove the root element (1):

![[20250421103948.png]]

![[20250421104025.png]]

**Time Complexity**

The time complexity of the push operation is O(log n), where n is the number of items in the heap.

In the worst case, after removing the root element, the new root starts at the top of the tree and "bubbles-down" to the last level of the tree, which takes O(log n) swaps, or the height of the tree.

### Peek

The peek operation returns the minimum value in the heap without removing it. The minimum value is always the root of the heap.

**Time Complexity** O(1): The peek operation has a constant time complexity since it only involves accessing the root of the heap, which is always index 0 in the array.

### Heapify

The heapify operation takes a list of elements and converts it into a heap in O(n) time.

We'll start with [4, 6, 9, 3, 2, 8, 3] and convert it into a heap using the heapify operation:

![[20250421104102.png]]


![[20250421104125.png]]


![[20250421104153.png]]

![[20250421104219.png]]


![[20250421104246.png]]

**Time Complexity**

O(n). The formal proof for the time complexity of the heapify operation is very math heavy, and beyond the scope of coding interviews, and thus, this lesson as well. If you're interested in learning more about the time complexity of the heapify operation, you can refer to the [Wikipedia page](https://en.wikipedia.org/wiki/Binary_heap#Building_a_heap).

### Summary

**If you only takeaway one thing from this section, remember that the pop and push operations both have a time complexity of O(log n).** The worst case for each operation involves swapping elements from the root of the heap to the last level of the tree, or vice versa, which takes O(log n) time.

|Operation|Time Complexity|Notes|
|---|---|---|
|pop|O(log n)|Visualize bubbling down the new root to the last level of the tree.|
|push|O(log n)|Visualize bubbling up the new element to the root of the tree.|
|peek|O(1)|Access the root of the heap.|
|heapify|O(n)|Just memorize this!|
