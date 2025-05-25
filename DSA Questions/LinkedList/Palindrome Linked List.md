
Given the `head` of a singly linked list, return `true` _if it is a_ _palindrome_ _or_ `false` _otherwise_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/03/pal1linked-list.jpg)

**Input:** head = [1,2,2,1]
**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/03/pal2linked-list.jpg)

**Input:** head = [1,2]
**Output:** false

**Constraints:**

- The number of nodes in the list is in the range `[1, 105]`.
- `0 <= Node.val <= 9`

**Follow up:** Could you do it in `O(n)` time and `O(1)` space?

### Solution

```python
class Solution(object):
	def isPalindrome(self, head):
		if not head.next:
			return True
		fast = head
		slow = head
	
		while fast and fast.next:
			slow = slow.next
			fast = fast.next.next
			
			curr = slow
			prev = None
		
			print(curr.val)
	
		while curr:
			next = curr.next
			curr.next = prev
			prev = curr
			curr = next
	
	
		while prev:
			if head.val != prev.val:
				return False
			prev = prev.next
			head = head.next
			
		return True
```

Time Complexity : O(n)
Space Complexity : O(1)

