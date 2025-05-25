
Given an integer array `nums` and an integer `k`, return _the_ `kth` _largest element in the array_.

Note that it is the `kth` largest element in the sorted order, not the `kth` distinct element.

Can you solve it without sorting?

**Example 1:**

**Input:** nums = [3,2,1,5,6,4], k = 2
**Output:** 5

**Example 2:**

**Input:** nums = [3,2,3,1,2,4,5,5,6], k = 4
**Output:** 4

**Constraints:**

- `1 <= k <= nums.length <= 105`
- `-104 <= nums[i] <= 104`


### Solution

```java
import java.util.*;

class Solution {

	public int findKthLargest(int[] nums, int k) {
	
		Queue<Integer> queue = new PriorityQueue<>(Collections.reverseOrder());
	
		for(int num : nums) {
			queue.add(num);
		}
		
		
		while(k-- > 1) {
			queue.remove();
		}
		
		return queue.remove();
	}
}
```

Time Complexity : O(n log k)
Space Complexity : O(n)




