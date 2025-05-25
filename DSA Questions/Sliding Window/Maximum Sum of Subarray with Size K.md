
### Problem

###### DESCRIPTION

Given an array of integers nums and an integer k, find the maximum sum of any contiguous subarray of size k.

**Example**: Input: nums = [2, 1, 5, 1, 3, 2], k = 3 Output: 9 Explanation: The subarray with the maximum sum is [5, 1, 3] with a sum of 9.


We start by extending the window to size k. Whenever our window is of size k, we first compute the sum of the window and update max_sum if it is larger than max_sum. Then, we contract the window by removing the leftmost element to prepare for the next iteration. Note how we calculate the sum of the window incrementally by adding the new element and removing from the previous sum.

### Solution

```python
def max_subarray_sum(nums, k):
  max_sum = 0
  state = 0
  start = 0

  for end in range(len(nums)):
    state += nums[end]

    if end - start + 1 == k:
      max_sum = max(max_sum, state)
      state -= nums[start]
      start += 1
      
  return max_sum
```


Time Complexity : O(n)
Space Complexity : O(1)


## When Do I Use This?

Consider using the sliding window pattern for questions that involve **searching for a continuous subsequence** in an array or string that satisfies a certain constraint.

If you know the length of the subsequence you are looking for, use a fixed-length sliding window. Otherwise, use a variable-length sliding window.

_Examples:_

- Finding the largest substring without repeating characters in a given string (variable-length).

- Finding the largest substring containing a single character that can be made by replacing at most k characters in a given string (variable-length).

- Finding the largest sum of a subarray of size k without duplicate elements in a given array (fixed-length).
