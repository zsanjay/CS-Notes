
### Problem

###### DESCRIPTION (credit [Leetcode.com](https://www.leetcode.com/problems/maximum-sum-of-distinct-subarrays-with-length-k/))

Given an integer array nums and an integer k, write a function to identify the highest possible sum of a subarray within nums, where the subarray meets the following criteria: its length is k, and all of its elements are unique.

###### EXAMPLES

**Example 1**: Input:

`nums = [3, 2, 2, 3, 4, 6, 7, 7, -1] k = 4`

Output:

`20`

Explanation: The subarrays of nums with length 4 are:

`[3, 2, 2, 3] # elements 3 and 2 are repeated. [2, 2, 3, 4] # element 2 is repeated. [2, 3, 4, 6] # meets the requirements and has a sum of 15. [3, 4, 6, 7] # meets the requirements and has a sum of 20. [4, 6, 7, 7] # element 7 is repeated. [6, 7, 7, -1] # element 7 is repeated.`

We return 20 because it is the maximum subarray sum of all the subarrays that meet the conditions.

### Solution

```python
class Solution(object):

def maximumSubarraySum(self, nums, k):
	maxSum = 0
	start = 0
	state = {}
	currSum = 0

  

	for end in range(len(nums)):
		currSum = currSum + nums[end]
		state[nums[end]] = state.get(nums[end], 0) + 1
	
	if end - start + 1 == k:
		if len(state) == k:
			maxSum = max(maxSum, currSum)
	
		currSum = currSum - nums[start]
		state[nums[start]] = state[nums[start]] - 1
		if state[nums[start]] == 0:
			del state[nums[start]]
		start += 1

	return maxSum
```

Time Complexity : O(n)
Space Complexity : O(n) - Because we are using map here


