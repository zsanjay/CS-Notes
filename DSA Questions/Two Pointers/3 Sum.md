
###### DESCRIPTION (credit [Leetcode.com](https://leetcode.com/problems/3sum/))

Given an input integer array nums, write a function to find all unique triplets [nums[i], nums[j], nums[k]] such that i, j, and k are distinct indices, and the sum of nums[i], nums[j], and nums[k] equals zero. Ensure that the resulting list does not contain any duplicate triplets.

###### EXAMPLES

Input:

`nums = [-1,0,1,2,-1,-1]`

Output:

`[[-1,-1,2],[-1,0,1]]`

Explanation: Both nums[0], nums[1], nums[2] and nums[1], nums[2], nums[4] both include [-1, 0, 1] and sum to 0. nums[0], nums[3], nums[4] ([-1,-1,2]) also sum to 0.

Since we are looking for unique triplets, we can ignore the duplicate [-1, 0, 1] triplet and return [[-1, -1, 2], [-1, 0, 1]].

The order of the triplets and the order of the elements within the triplets do not matter.


### Solution


We can leverage the two-pointer technique to solve this problem by first sorting the array. We can then iterate through each element in the array. The problem then reduces to finding two numbers in the rest of the array that sum to the negative of the current element, which follows the same logic as the [Two Sum (Sorted Array)](https://www.hellointerview.com/learn/code/two-pointers/overview) problem from the overview.

#### Important Points

1. i needs to be iterated till len(nums) - 2
2. Skip the duplicate (if i > 0 and nums[i] == nums[i -1] then continue)
3. Implement Two Sum
4. If we got the target which is zero, then add it to the list and skip the duplicates in this case as well for ( nums[left] == nums[left + 1] ) and (nums[right] == nums[right - 1])

```python
def threeSum(nums):

  nums.sort()

  result = []

  for i in range(len(nums) - 2):

    if i > 0 and nums[i] == nums[i - 1]:

      continue

    left = i + 1

    right = len(nums) - 1

    while left < right:

      total = nums[i] + nums[left] + nums[right]

      if total < 0:

        left += 1

      elif total > 0:

        right -= 1

      else:

        result.append([nums[i], nums[left], nums[right]])

        while left < right and nums[left] == nums[left + 1]:

          left += 1

        while left < right and nums[right] == nums[right - 1]:

          right -= 1

        left += 1

        right -= 1

  return result
```

Time Complexity : O(n * n)
Space Complexity : O(n * n) - Triplets

**Time Complexity:** O(n2), where n is the length of the input array. This is due to the nested loops in the algorithm. We perform n iterations of the outer loop, and each iteration takes O(n) time to use the two-pointer technique.

**Space Complexity:** O(n2), where n is the length of the input array. We need to store all distinct triplets that sum to 0, which can be at most O(n2) triplets. [See here](https://stackoverflow.com/questions/57641003/what-is-the-time-and-space-complexity-of-the-3sum-problem-with-the-following-alg) for a detailed derivation of the space complexity.



