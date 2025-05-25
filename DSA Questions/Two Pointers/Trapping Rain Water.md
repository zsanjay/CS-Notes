
###### DESCRIPTION (credit [Leetcode.com](https://leetcode.com/problems/trapping-rain-water/))

Write a function to calculate the total amount of water trapped between bars on an elevation map, where each bar's width is 1. The input is given as an array of n non-negative integers height representing the height of each bar.

###### EXAMPLES

Input:

`height = [3, 4, 1, 2, 2, 5, 1, 0, 2]`

Output:

`10`

### Solution 

Calculate the left max and right max height for every index and for every index get the minimum height among two and subtract the current height of the poll to get the water it can trap.


```python
class Solution(object):

	def trap(self, height):
		trappedWater = 0
		n = len(height)
		leftHeight , rightHeight = [0] * n , [0] * n
		leftHeight[0] = height[0]
		rightHeight[n - 1] = height[n - 1]
	
		for i in range(1 , n):
			leftHeight[i] = max(leftHeight[i - 1] , height[i])
		
		for i in range(n - 2, -1, -1):
			rightHeight[i] = max(rightHeight[i + 1] , height[i])
		
		for i in range(n):
			trappedWater += min(leftHeight[i] , rightHeight[i]) - height[i]

return trappedWater
```

Time Complexity : O(n)
Space Complexity : O(n)








