
### Problem

###### DESCRIPTION (credit [Leetcode.com](https://leetcode.com/problems/insert-interval))

Given a list of intervals intervals and an interval newInterval, write a function to insert newInterval into a list of existing, non-overlapping, and sorted intervals based on their starting points. The function should ensure that after the new interval is added, the list remains sorted without any overlapping intervals, merging them if needed.

###### EXAMPLES

Input:

`intervals = [[1,3],[6,9]] newInterval = [2,5]`

Output:

`[[1,5],[6,9]]`

Explanation: The new interval [2,5] overlaps with [1,3], so they are merged into [1,5].


### Solution

```java
class Solution {

public int[][] insert(int[][] intervals, int[] newInterval) {

	List<int[]> result = new ArrayList<>();
	
	// Iterate through intervals and add non-overlapping intervals before newInterval
	int i = 0;
	while(i < intervals.length && intervals[i][1] < newInterval[0]) {
		result.add(intervals[i]);
		i++;
	}
	
	// Merge overlapping intervals
	while(i < intervals.length && newInterval[1] >= intervals[i][0]) {
		newInterval[0] = Math.min(intervals[i][0] , newInterval[0]);
		newInterval[1] = Math.max(intervals[i][1] , newInterval[1]);
		i++;
	}
	
	result.add(newInterval);
	
	// Add remaining non-overlapping intervals after newInterval
	while(i < intervals.length) {
		result.add(intervals[i]);
		i++;
	}
	
		return result.toArray(new int[result.size()][]);
	}
}
```

Time Complexity : O(n)
Space Complexity : O(n)