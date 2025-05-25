
[86. Partition List](https://leetcode.com/problems/partition-list/)

Given the `head` of a linked list and a value `x`, partition it such that all nodes **less than** `x` come before nodes **greater than or equal** to `x`.

You should **preserve** the original relative order of the nodes in each of the two partitions.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/04/partition.jpg)

**Input:** head = [1,4,3,2,5,2], x = 3
**Output:** [1,2,2,4,3,5]

**Example 2:**

**Input:** head = [2,1], x = 2
**Output:** [1,2]

**Constraints:**

- The number of nodes in the list is in the range `[0, 200]`.
- `-100 <= Node.val <= 100`
- `-200 <= x <= 200`

### Solution

```python
# Definition for singly-linked list.

# class ListNode(object):

# def __init__(self, val=0, next=None):

# self.val = val

# self.next = next

class Solution(object):

	def partition(self, head, x):
	
		if not head or not head.next:
			return head
	
		
		less_head = ListNode(0)
		greater_head = ListNode(0)
	
		
		less = less_head
		greater = greater_head

		
		while head:
			if head.val < x:
				less.next = head
				less = less.next
			else:
				greater.next = head
				greater = greater.next
			head = head.next
		
		greater.next = None
		less.next = greater_head.next
		
		return less_head.next
```

Time Complexity : O(n)
Space Complexity : O(1)

