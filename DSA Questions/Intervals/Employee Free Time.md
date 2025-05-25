
### Problem

###### DESCRIPTION (credit [Leetcode.com](https://leetcode.com/problems/employee-free-time/))

Write a function to find the common free time for all employees from a list called schedule. Each employee's schedule is represented by a list of non-overlapping intervals sorted by start times. The function should return a list of finite, non-zero length intervals where all employees are free, also sorted in order.

###### EXAMPLES

Input:

`schedule = [[[2,4],[7,10]],[[1,5]],[[6,9]]]`

Output:

`[(5,6)]`

Explanation: The three employees collectively have only one common free time interval, which is from 5 to 6.

### Solution

## Explanation

This problems builds upon the concept of [merging intervals](https://www.hellointerview.com/learn/code/intervals/merge-intervals). We can solve this problem by first merging all the employee meeting intervals into a single list. The free times are then the gaps between those merged intervals.

##### Phase 1

We first want to flatten the list of intervals into a single list, and then sorting them by their start time to make the merge process easier.


### Solution

```java
import java.util.*;

class Main {
    public static void main(String[] args) {
       int[][] schedule = {{2,4},{7,10},{1,5},{6,9}};
       
       // Sort by Start Time
       Arrays.sort(schedule, (a , b) -> a[0] - b[0]);
       
       // Merge Intervals
       List<int[]> result = new ArrayList<>();
       int[] temp = schedule[0];
       
       int i = 1;
       while(i < schedule.length) {
           if(schedule[i][0] <= temp[1]) {
              temp[1] = Math.max(temp[1], schedule[i][1]);
           } else {
              result.add(temp);
              temp = schedule[i];
          }
          i++;
        }
       result.add(temp);
       
       // Get the gap between intervals
       List<int[]> freeTimeInterval = new ArrayList<>();
      
       i = 0;
       while(i < result.size() - 1) {
           if(result.get(i)[1] < result.get(i + 1)[0]) {
               freeTimeInterval.add(new int[]{result.get(i)[1] , result.get(i + 1)[0]});
           }
           i++;
       }
       
       
      for(int[] t : freeTimeInterval) {
          System.out.println(t[0] + " " + t[1]);
      }
       
    }
}
```

