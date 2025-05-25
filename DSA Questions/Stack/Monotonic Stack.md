
A **monotonic stack** is a special type of stack in which all elements on the stack are sorted in either descending or ascending order. It is used to solve problems that require finding the next greater or next smaller element in an array.

![[20250415092840.png]]

### Problem: Next Greater Element

###### DESCRIPTION

Given an array of integers, find the next greater element for each element in the array. The next greater element of an element x is the first element to the right of x that is greater than x. If there is no such element, then the next greater element is -1.

**Example** Input: [2, 1, 3, 2, 4, 3] Output: [3, 3, 4, 4, -1, -1]


The solution iterates over each index in the input array. For each index, it checks if the element at that index is the next greater element for any previous elements in the array. In order to perform that check efficiently, we'll use a **monotonic decreasing stack**.

##### Initialization

We start by initializing our stack and our results array, with each value in the results array initialized to -1. Our stack stores the indexes of the elements in the input array that have not yet found their next greater element.

```python
def nextGreaterElement(nums):

  n = len(nums)
  result = [-1] * n
  stack = []

  for i in range(n):
    while stack and nums[i] > nums[stack[-1]]:
      idx = stack.pop()
      result[idx] = nums[i]
    stack.append(i)
    
  return result
```

##### Iteration

We then iterate over the input array. To check if the current element nums[i] is the next greater element for any of the previous elements in the array, we compare the current element with the element at the index at the top of the stack nums[stack[-1]].

If the stack is empty, or if nums[i] is less than nums[stack[-1]], we push the current index onto the stack.


![[20250415104034.png]]