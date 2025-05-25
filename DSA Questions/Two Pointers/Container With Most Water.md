
###### DESCRIPTION (credit [Leetcode.com](https://leetcode.com/problems/container-with-most-water/))

Given an integer input array heights representing the heights of vertical lines, write a function that returns the maximum area of water that can be contained by two of the lines (and the x-axis). The function should take in an array of integers and return an integer.

###### EXAMPLES

**Example 1**:

Inputs:

`heights = [3,4,1,2,2,4,1,3,2]`

Output:

`21`

**Example 2**:

Inputs:

`heights = [1,2,1]`

Output:

`2`


### Solution

```python
class Solution:

def maxArea(self, height: List[int]) -> int:

left , right, maxWater = 0, len(height) - 1, 0

  

while left < right:

minHeight = min(height[left], height[right])

maxWater = max(maxWater, (right - left) * minHeight)

  

if height[left] < height[right]:

left = left + 1

else:

right = right - 1

  

return maxWater
```


Time Complexity : O(n)
Space Complexity : O(1)



