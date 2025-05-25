
This is the basis for the question [**Non-Overlapping Intervals**](https://www.hellointerview.com/learn/code/intervals/non-overlapping-intervals), in which we are given a list of intervals and asked to find the maximum number of non-overlapping intervals.

We sort the intervals by their end times, and then iterate over each interval, keeping a count of all intervals that overlap with the last interval in the non-overlapping set. We return the total number of intervals minus the count of overlapping intervals.

### Problem

###### DESCRIPTION (credit [Leetcode.com](https://leetcode.com/problems/non-overlapping-intervals/))

Write a function to return the minimum number of intervals that must be removed from a given array intervals, where intervals[i] consists of a starting point starti and an ending point endi, to ensure that the remaining intervals do not overlap.

###### EXAMPLES

Input:

`intervals = [[1,3],[5,8],[4,10],[11,13]]`

Output:

`1`

Explanation: Removing the interval [4,10] leaves all other intervals non-overlapping.


## Explanation

This question reduces to finding the maximum number of non-overlapping intervals. Once we know that value, then we can subtract it from the total number of intervals to get the minimum number of intervals that need to be removed.

To find the maximum number of non-overlapping intervals, we can sort the intervals by their **end** time. We then use a greedy approach: we iterate over each sorted interval, and repeatedly try to add that interval to the set of non-overlapping intervals. Sorting by the end time allows us to choose the intervals that end the earliest first, which frees up more time for intervals to be included later.

We start by keeping track of a variable end which represents the end time of the latest interval in our set of non-overlapping intervals, as well as a variable count which represents the number of non-overlapping intervals we have found so far.

### Solution

```python
def nonOverlappingIntervals(intervals):
  if not intervals:
    return 0

  intervals.sort(key=lambda x: x[1])
  end = intervals[0][1]
  count = 1

  for i in range(1, len(intervals)):
    # Non-overlapping interval found
    if intervals[i][0] >= end:
      end = intervals[i][1]
      count += 1
      
  return len(intervals) - count
```

Time Complexity : O(nlogn)
Space Complexity : O(1)






