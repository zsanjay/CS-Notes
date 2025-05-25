This technique refers to using two pointers that start at opposite ends of an array and gradually move towards each other.

###### DESCRIPTION

Given a **sorted array** of integers nums, determine if there exists a pair of numbers that sum to a given target.

**Example**: Input: nums = [1,3,4,6,8,10,13], target = 13 Output: True (3 + 10 = 13)

Input: nums = [1,3,4,6,8,10,13], target = 6 Output: False

The naive approach to this problem uses two-pointers i and j in a nested for-loop to consider each pair in the input array, for a total of O(n2) pairs considered.

```python
def isPairSum(nums, target):

  for i in range(len(nums)):

    for j in range(i + 1, len(nums)):

      if nums[i] + nums[j] == target:

        return True

  return False
```

### Eliminate the number of pairs

However, if we put a bit more thought into how we initialize our pointers and how we move them, we can **eliminate the number of pairs we consider down to O(n)**. Understanding why we are able to eliminate pairs is key to understanding the two-pointer technique.


### Eliminating Pairs

The two-pointer technique leverages the fact that the input array is sorted.

Let's use it to solve the Two Sum problem when nums = [1, 3, 4, 6, 8, 10, 13] and target = 13.

```python
  
def twoSum(nums, target):

  left, right = 0, len(nums) - 1

  while left < right:

    current_sum = nums[left] + nums[right]

    if current_sum == target:

        return True

    if current_sum < target:

        left += 1

    else:

        right -= 1

  return False
```

### Summary

- The two-pointer technique leverages the fact that the input array is sorted to eliminate the number of pairs we consider from O(n2)down to O(n).
    
- The two-pointers start at opposite ends of the array, and represent the pair of numbers we are currently considering.
    
- We repeatedly compare the sum of the current pair to the target, and move a pointer in a way that eliminates unnecessary pairs from our search.

## When Do I Use This?

Consider using the two-pointer technique for questions that involve searching for a pair (or more) of items in an array that meet a certain criteria.

_Examples:_

- Finding a pair of items that sum to a given target in an array.
    
- Finding a triplet of items that sum to 0 in a given array.
    
- Finding the maximum amount of water that can be held between two array items representing wall heights.


### Reference 

https://www.hellointerview.com/learn/code/two-pointers/overview




