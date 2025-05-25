
###### DESCRIPTION (credit [Leetcode.com](https://leetcode.com/problems/move-zeroes/))

Given an integer array nums, write a function to rearrange the array by moving all zeros to the end while keeping the order of non-zero elements unchanged. Perform this operation in-place without creating a copy of the array.

###### EXAMPLES

Input:

`nums = [2,0,4,0,9]`

Output:

`[2,4,9,0,0]`

### Solution

Keeping track of zero and once you encountered the number which is not equal to zero swap it and increment the variable by 1 which is keeping track of zero.

We can solve this problem by keeping a pointer i that iterates through the array and another pointer nextNonZero that points to the position where the next non-zero element should be placed. We can then swap the elements at i and nextNonZero if the element at i is non-zero. This way, we can maintain the relative order of the non-zero elements while moving all the zeroes to the end of the array.

```python
def moveZeroes(nums):
  nextNonZero = 0
  for i in range(len(nums)):
    if nums[i] != 0:
      nums[nextNonZero], nums[i] = nums[i], nums[nextNonZero]
      nextNonZero += 1
```


Time Complexity : O(n)
Space Complexity : O(1)

