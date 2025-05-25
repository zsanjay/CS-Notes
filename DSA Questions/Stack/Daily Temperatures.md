
###### DESCRIPTION (credit [Leetcode.com](https://leetcode.com/problems/daily-temperatures))

Given an integer array temps representing daily temperatures, write a function to calculate the number of days one has to wait for a warmer temperature after each given day. The function should return an array answer where answer[i] represents the wait time for a warmer day after the ith day. If no warmer day is expected in the future, set answer[i] to 0.

###### EXAMPLES

Inputs:

`temps = [65, 70, 68, 60, 55, 75, 80, 74]`

Output:

`[1,4,3,2,1,1,0,0]`


## Explanation

This question uses a **[monotonically decreasing stack](https://www.hellointerview.com/learn/code/stack/monotonic-stack)** to find the next greatest temperature for each day in O(n) time, compared to the O(n2) time of the brute-force approach.

A monotonically decreasing stack is one where all elements decrease in value (from the bottom of the stack to the top). When pushing an item on a monotonically decreasing stack, it must be smaller than all the other elements that are currently on the stack.

73727268stack

A montonically decreasing stack

First, we initialize our stack and our results array. Our stack holds indices of the temperatures in the input array that are waiting for a higher temperature, and our results array holds the number of days we have to wait after the ith day to get a warmer temperature.


### Solution

```java
class Solution {

public int[] dailyTemperatures(int[] temperatures) {

	Stack<Integer> stack = new Stack<>();
	int[] result = new int[temperatures.length];

	for(int i = 0; i < result.length; i++) {
		while(!stack.isEmpty() && temperatures[i] > temperatures[stack.peek()]) {
			int index = stack.pop();
			result[index] = i - index;
		}
		
		stack.push(i);
	}
	
	return result;
}
}
```


Time Complexity : O(n)
Space Complexity : O(n)

