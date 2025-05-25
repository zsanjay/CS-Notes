
###### DESCRIPTION (credit [Leetcode.com](https://leetcode.com/problems/valid-triangle-number/))

Write a function to count the number of triplets in an integer array nums that could form the sides of a triangle. The triplets do not need to be unique.

###### EXAMPLES

Input:

`nums = [11,4,9,6,15,18]`

Output:

`10`

Explanation: Valid combinations are...

4, 15, 18
6, 15, 18
9, 15, 18
11, 15, 18
9, 11, 18
6, 11, 15
9, 11, 15
4, 6, 9


The expression `range(len(nums) - 1, 1, -1)` in Python is used to generate a sequence of numbers starting from `len(nums) - 1` and decrementing down to (but not including) `1`.

Let's break it down:

- **`len(nums) - 1`**: This is the starting point of the range. It starts from the last index of the list `nums` because the length of a list is one greater than its highest index. For example, if `nums` has 5 elements, `len(nums)` will be `5`, so `len(nums) - 1` will be `4`, the index of the last element.
    
- **`1`**: This is the stopping point. The range will stop just before reaching `1` (i.e., it will include values down to 2).
    
- **`-1`**: This is the step size. The `-1` means that the range will decrement by 1 in each iteration.

```python
nums = [10, 20, 30, 40, 50]

# Create the range
for i in range(len(nums) - 1, 1, -1):
    print(i)
```

### Solution

```python

class Solution(object):

def triangleNumber(self, nums):

nums.sort()

count = 0

for i in range(len(nums) - 1, 1 , -1):

left = 0

right = i - 1

while left < right:

if nums[left] + nums[right] > nums[i]:

count += right - left

right -= 1

else:

left += 1

  

return count
```


#### Explanation

In order for a triplet to be valid lengths of a triangle, the sum of any two sides must be greater than the third side. By sorting the array, we can leverage the two-pointer technique to count all valid triplets in O(n2) time and O(1) space.

The key to this question is realizing that if we have 3 numbers, such as 4, 8, 9, arranged from smallest to largest, and the sum of the two smallest numbers is greater than the largest number, then we have a valid triplet ( 4 + 8 > 9).


Time Complexity : O(n * n)
Space Complexity : O(1) 