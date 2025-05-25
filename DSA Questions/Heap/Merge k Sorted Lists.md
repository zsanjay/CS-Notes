
You are given an array of `k` linked-lists `lists`, each linked-list is sorted in ascending order.

_Merge all the linked-lists into one sorted linked-list and return it._

**Example 1:**

**Input:** lists = [[1,4,5],[1,3,4],[2,6]]
**Output:** [1,1,2,3,4,4,5,6]
**Explanation:** The linked-lists are:
[
  1->4->5,
  1->3->4,
  2->6
]
merging them into one sorted list:
1->1->2->3->4->4->5->6

**Example 2:**

**Input:** lists = []
**Output:** []

**Example 3:**

**Input:** lists = [[]]
**Output:** []

**Constraints:**

- `k == lists.length`
- `0 <= k <= 104`
- `0 <= lists[i].length <= 500`
- `-104 <= lists[i][j] <= 104`
- `lists[i]` is sorted in **ascending order**.
- The sum of `lists[i].length` will not exceed `104`.


### Solution

```java
class Solution {

public ListNode mergeKLists(ListNode[] lists) {

	if(lists == null || lists.length == 0) {
		return null;
	}
	
	Queue<Integer> queue = new PriorityQueue<>();
	
	for(ListNode node : lists) {
		while(node != null) {
			queue.add(node.val);
			node = node.next;
		}
	}
	
	
	ListNode mergedLinkedList = new ListNode(0);
	ListNode temp = mergedLinkedList;
	
	while(!queue.isEmpty()) {
		temp.next = new ListNode(queue.poll());
		temp = temp.next;
	}
	
	return mergedLinkedList.next;
	
	}

}
```

Time Complexity : O(m * n * log n)
Space Complexity : O(m * n)