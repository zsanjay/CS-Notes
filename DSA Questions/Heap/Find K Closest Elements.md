
Given a **sorted** integer array `arr`, two integers `k` and `x`, return the `k` closest integers to `x` in the array. The result should also be sorted in ascending order.

An integer `a` is closer to `x` than an integer `b` if:

- `|a - x| < |b - x|`, or
- `|a - x| == |b - x|` and `a < b`

**Example 1:**

**Input:** arr = [1,2,3,4,5], k = 4, x = 3

**Output:** [1,2,3,4]

**Example 2:**

**Input:** arr = [1,1,2,3,4,5], k = 4, x = -1

**Output:** [1,1,2,3]

**Constraints:**

- `1 <= k <= arr.length`
- `1 <= arr.length <= 104`
- `arr` is sorted in **ascending** order.
- `-104 <= arr[i], x <= 104`

### Solution

```java
class Solution {

	public List<Integer> findClosestElements(int[] arr, int k, int x) {
	
		Queue<Integer> queue = new PriorityQueue<>();
		
		for(int a : arr) {
			if(k-- > 0) {
				queue.add(a);
			} else if(Math.abs(queue.peek() - x) > Math.abs(a - x)) {
				queue.poll();
				queue.add(a);
			}
		}
		
		List<Integer> list = new ArrayList<>();
		
		while(!queue.isEmpty()) {
			list.add(queue.poll());
		}
		
		return list;
	
	}

}
```

**Time Complexity** : O(n * log k) -  We iterate over all the elements in the array. At each iteration, we comparing the current element with the root of the heap takes O(1) time. In the worst case, we both push and pop each element from the heap, which takes O(log k) time.

**Space Complexity** : O(k) - The space used by the heap to store the k closest elements to the target.