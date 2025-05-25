
###### DESCRIPTION (credit [Leetcode.com](https://leetcode.com/problems/merge-intervals/))

Write a function to consolidate overlapping intervals within a given array intervals, where each interval intervals[i] consists of a start time starti and an end time endi. The function should return an array of the merged intervals so that no two intervals overlap and all the intervals collectively cover all the time ranges in the original input.

###### EXAMPLES

Input:

`intervals = [[3,5],[1,4],[7,9],[6,8]]`

Output:

`[[1,5],[6,9]]`

Explanation: The intervals [3,5] and [1,4] overlap and are merged into [1,5]. Similarly, [7,9] and [6,8] overlap and are merged into [6,9].


```python
class Solution(object):

def merge(self, intervals):

	intervals.sort(key=lambda x:x[0])
	
	mergedIntervals = []
	prevInterval = intervals[0]
	for i in range(1 , len(intervals)):
		if prevInterval[1] >= intervals[i][0]:
			prevInterval[1] = max(prevInterval[1] , intervals[i][1])
		else:
			mergedIntervals.append(prevInterval)
			prevInterval = intervals[i]

	mergedIntervals.append(prevInterval)

return mergedIntervals
```


Time Complexity : O(nlogn)
Space Complexity : O(n)


