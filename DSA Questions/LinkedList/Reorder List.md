
You are given the head of a singly linked-list. The list can be represented as:

L0 → L1 → … → Ln - 1 → Ln

_Reorder the list to be on the following form:_

L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …

You may not modify the values in the list's nodes. Only nodes themselves may be changed.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/04/reorder1linked-list.jpg)

**Input:** head = [1,2,3,4]
**Output:** [1,4,2,3]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/09/reorder2-linked-list.jpg)

**Input:** head = [1,2,3,4,5]
**Output:** [1,5,2,4,3]

**Constraints:**

- The number of nodes in the list is in the range `[1, 5 * 104]`.
- `1 <= Node.val <= 1000`

### Solution

```python
# Definition for singly-linked list.

# class ListNode(object):

# def __init__(self, val=0, next=None):

# self.val = val

# self.next = next

class Solution(object):
	def reorderList(self, head):
	
		if not head or not head.next:
			pass
		
		fast = head
		slow = head
		
		while fast and fast.next:
			slow = slow.next
			fast = fast.next.next
		
		secondHalf = slow.next
		slow.next = None


		prev = None
		curr = secondHalf

		while curr:
			next = curr.next
			curr.next = prev
			prev = curr
			curr = next
		
		list1 = head
		list2 = prev
		
		while list1 and list2:
			# Getting the next from first list
			temp = list1.next
			# Putting the next element from second list
			list1.next = list2
			# Replacing the reference of first list to second to use second list
			list1 = list2
			# Putting the next element of first list into second
			list2 = temp
```


Time Complexity : O(n)
Space Complexity : O(1)
