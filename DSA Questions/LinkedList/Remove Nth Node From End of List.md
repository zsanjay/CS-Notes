
Given the `head` of a linked list, remove the `nth` node from the end of the list and return its head.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg)

**Input:** head = [1,2,3,4,5], n = 2
**Output:** [1,2,3,5]

**Example 2:**

**Input:** head = [1], n = 1
**Output:** []

**Example 3:**

**Input:** head = [1,2], n = 1
**Output:** [1]

**Constraints:**

- The number of nodes in the list is `sz`.
- `1 <= sz <= 30`
- `0 <= Node.val <= 100`
- `1 <= n <= sz`

### Solution

```python
# Definition for singly-linked list.

# class ListNode(object):

# def __init__(self, val=0, next=None):

# self.val = val

# self.next = next

class Solution(object):

	def removeNthFromEnd(self, head, n):
		
		size = 0
		temp = head
		while temp:
			temp = temp.next
			size += 1
		
		if size == n:
			return head.next
		
		position = size - n
		
		temp = head
		
		while position > 1:
			temp = temp.next
			position -= 1
		
		  
		temp.next = temp.next.next if temp.next else None
		return head
```

Time Complexity : O(n)
Space Complexity : O(1)