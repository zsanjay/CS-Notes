
###### DESCRIPTION (credit [Leetcode.com](https://leetcode.com/problems/sort-colors/))

Write a function to sort a given integer array nums in-place (and without the built-in sort function), where the array contains n integers that are either 0, 1, and 2 and represent the colors red, white, and blue. Arrange the objects so that same-colored ones are adjacent, in the order of red, white, and blue (0, 1, 2).

###### EXAMPLES

Input:

`nums = [2,1,2,0,1,0,1,0,1]`

Output:

`[0,0,0,1,1,1,1,2,2]`


### Solution 

We need to keep two pointers, one is to track zero and the second is to track two, because zero should be come in the front of the array and two should come at the end of the array.


```python
class Solution(object):

def sortColors(self, nums):
left, right = 0, len(nums) - 1
i = 0
while i <= right:
	if nums[i] == 2:
		nums[i], nums[right] = nums[right], nums[i]
		right -= 1
	elif nums[i] == 0:
		nums[i], nums[left] = nums[left], nums[i]
		left += 1
		i += 1
	else:
		i += 1
```


Time Complexity : O(n)
Space Complexity : O(1)







