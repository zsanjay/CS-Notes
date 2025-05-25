
Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed.)

**Example 1:**

**Input:** head = [1,2,3,4]

**Output:** [2,1,4,3]

**Explanation:**

![](https://assets.leetcode.com/uploads/2020/10/03/swap_ex1.jpg)

**Example 2:**

**Input:** head = []

**Output:** []

**Example 3:**

**Input:** head = [1]

**Output:** [1]

**Example 4:**

**Input:** head = [1,2,3]

**Output:** [2,1,3]

**Constraints:**

- The number of nodes in the list is in the range `[0, 100]`.
- `0 <= Node.val <= 100`


### Solution

```python
class Solution(object):
	def swapPairs(self, head):
	
		if not head or not head.next:
			return head
		
		dummy = ListNode(0)
		tail , first = dummy , head
		
		while first and first.next:
			
			second = first.next
			
			tail.next = second
			first.next = second.next
			second.next = first
			
			tail = first
			first = first.next
		
		return dummy.next
```

Time Complexity : O(n)
Space Complexity : O(1)

#### Reference

https://www.youtube.com/watch?v=b8_ZINq9yhA