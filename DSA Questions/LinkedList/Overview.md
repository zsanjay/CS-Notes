## Basics

A linked list is a data structure consisting of sequence of a nodes, where each node contains a value and reference to the next node in the sequence.

In Python, we can represent a linked list node with a ListNode class, where val is a piece of data and next is a reference to the next node in the linked list.

```python
# Definition of a ListNode
class ListNode:
  def __init__(self, val=0, next=None):
    self.val = val
    self.next = next
```

We can visualize a linked list as a sequence of nodes connected by arrows that point to the next node in the sequence. The first node in a linked list is referred to as the head, and the last node is referred to as the tail. The next field of the tail node is None (unless the linked list contains a cycle) which indicates the end of the linked list.

```python
# visualizing linked lists
head = ListNode(1)
head.next = ListNode(2)
head.next.next = ListNode(3)
head.next.next.next = ListNode(4)
```

## Basic Operations

These operations demonstrate some of the fundamentals of working with linked lists.
### Traversing a Linked List

When traversing a linked list, we initialize a pointer current that starts at the head node of the linked list and follows next pointers until it is None. This allows us to visit each node in the linked list, and perform operations such as finding the length of the linked list.

```python
def findLength(head):
  length = 0
  current = head
  while current:
    length += 1
    current = current.next
  return length
```

#### Complexity Analysis

**Time Complexity**: The time complexity of this algorithm is O(n) where n is the number of nodes in the linked list. The algorithm iterates through each node in the linked list once.

**Space Complexity**: The space complexity of this algorithm is O(1) since we only use one pointer to traverse the linked list regardless of the number of nodes in the linked list.


### Deleting a Node With a Given Target

To delete a node with a given target, we need a reference to both the node we want to delete, and the node right before it.

We'll keep two pointers, curr and prev, and update curr until it reaches the target node, or None. We'll also update prev to be the node before curr at each step. When curr reaches the target node, we can delete it by setting prev.next = curr.next.

```python
We have to handle deleting the head of the linked list as a special case, because there is no node before the head. We can simplify this by using a [dummy node](https://www.hellointerview.com/learn/code/linked-list/overview
#dummy-nodes), which we'll cover later.
```

```python
def deleteNode(head, target):
  if head.val == target:
    return head.next

  prev = None
  curr = head
  
  while curr:
    if curr.val == target:
      prev.next = curr.next
      break
      
    prev = curr
    curr = curr.next
    
  return head;
```

#### Complexity Analysis

**Time Complexity**: The time complexity of this algorithm is O(n) where n is the number of nodes in the linked list. The algorithm iterates through each node in the linked list once in the worst case (when the target does not exist in the linked list). 

**Space Complexity**: The space complexity of this algorithm is O(1) since we only use two pointers to traverse the linked list regardless of the number of nodes in the linked list.


## Operations to Know for Interviews

Linked list interview questions require manipulating pointers in specific ways that depend entirely on the requirements of the problem. However, there are a few core operations that you should be familiar with, as they show up in multiple linked list questions.

This section covers those operations. At the end, we'll look at problems that will give you more practice with these operations.

### 1. Fast and Slow Pointers

Fast and slow pointers is a technique that is used to find the middle node in a linked list. We initialize two pointers, slow and fast, that start at the head of the linked list. We then iterate until fast reaches the end of the list. During each iteration, the slow pointer advances by one node, while the fast pointer advances by two nodes. When the fast pointer reaches tail of the list, the slow pointer points to the middle node.

```python
def fastAndSlow(head):
  fast = head
  slow = head
  
  while fast && fast.next:
    fast = fast.next.next
    slow = slow.next

  return slow
```

```
In general, when working with linked list questions, having a clear understanding of what each pointer in your algorithm represents, and being able to visualize where they should end up when the iteration finishes will help you avoid off-by-one errors and null pointer exceptions.
```
#### Complexity Analysis

**Time Complexity**: The time complexity of this algorithm is O(n) where n is the number of nodes in the linked list. The fast pointer iterates through each node in the linked list once. 

**Space Complexity**: The space complexity of this algorithm is O(1) since we only use two pointers to traverse the linked list regardless of the number of nodes in the linked list.

#### Cycle Detection

The same fast and slow pointers technique can also be used to determine if a linked list contains a cycle. If we follow the same iteration pattern and the linked list contains a cycle, the fast pointer will eventually overlap the slow pointer and they will point to the same node.

This is a common interview question, and a good problem to practice using the fast and slow pointers technique (see question #1, Leetcode 141 in [Practice Problems](https://www.hellointerview.com/learn/code/linked-list/overview#practice-problems)).

### 2. Reversing a Linked List

Reversing a linked list involves changing the direction of the next pointers in a linked list so the last node becomes the head of the reversed linked list.

The algorithm for reversing a linked list is an iterative algorithm which involves 3 pointers, prev, current, and next_.

1. current points to the node we are currently reversing.
2. prev is the last node that was reversed, and also the node that current.next will point to after reversing.
3. next_ is the next node we will reverse. We need a pointer to this node before we overwrite the current.next so we can continue reversing the list in the next iteration.

When the iteration completes, current will be None, and prev will be the new head of the linked list.

```python
def reverse(head):
  prev = None
  current = head

  while current:
    next_ = current.next
    current.next = prev
    prev = current
    current = next_
  return prev
```

#### Complexity Analysis

**Time Complexity**: The time complexity of this algorithm is O(n) where n is the number of nodes in the linked list. The algorithm iterates through each node in the linked list once.

**Space Complexity**: The space complexity of this algorithm is O(1) since we only use three pointers to reverse the linked list regardless of the number of nodes in the linked list.

### 3. Merging Two Linked Lists

The last operation is merging two linked lists. As an example of this operation, we'll look at how to merge two sorted linked lists.

As an input to this problem, we are given the heads of two sorted linked lists, l1 and l2, and we need to return the head of a new linked list that contains all the nodes from the two input linked lists in sorted order.

To merge two sorted linked lists, we start by determining the head of the merged linked list by comparing the values of l1 and l2, and setting the head to the smaller of the two nodes. We then advance l1 = l1.next or l2 = l2.next depending on which node we chose as the head of the merged linked list.

```python
def merge_lists(l1, l2):

  if not l1: return l2
  if not l2: return l1

  if l1.val < l2.val:
    head = l1
    l1 = l1.next
  else:
    head = l2
    l2 = l2.next
    
  tail = head

  while l1 and l2:
    if l1.val < l2.val:
      tail.next = l1
      l1 = l1.next
    else:
      tail.next = l2
      l2 = l2.next
	  tail = tail.next

  tail.next = l1 or l2
  
  return head
```

#### Complexity Analysis

**Time Complexity**: The time complexity of this algorithm is O(n + m) where n and m are the number of nodes in the two input linked lists. The algorithm iterates through each node in the two linked lists once. 

**Space Complexity**: The space complexity of this algorithm is O(1) since we only use a constant amount of space to merge the two linked lists regardless of the number of nodes in the linked lists. This is because we are modifying the next pointers of the input linked lists, rather than creating new nodes.

## Dummy Nodes

Merging two sorted linked lists is an example of a problem where using a dummy node can simplify the logic of the code.

Notice that in the solution for merging two lists above, the logic for choosing the head of the merged linked list is the same as the logic for choosing the next node to append. We need to handle it as a special case because without it, we wouldn't have a starting point for the merged linked list.

We can avoid this by creating a dummy node to represent the starting point of the merged linked list. This allows us to move directly into the iteration processes without having to introduce a special case to initialize the head of the merged linked list. When the iteration finishes we return dummy.next as the head of the merged linked list.

Note: The term "dummy node" refers to creating a new node that isn't part of the input linked list(s) (line 2 in the code below).

```python
  
def merge_two_lists(l1, l2):

  dummy = ListNode()

  tail = dummy

  while l1 and l2:

    if l1.val < l2.val:

      tail.next = l1

      l1 = l1.next

    else:

      tail.next = l2

      l2 = l2.next

    tail = tail.next

  tail.next = l1 or l2

  return dummy.next
```

### Advantages of a Dummy Node

The advantage of using a dummy node for this question is that it allows us to avoid having to initializing the head of the merged linked list as a special case. This simplifies the logic of the code, and also reduces the need to check if either of the merged linked lists are None (which we need to do if we don't use a dummy node becuase we reference either l1.next or l2.next as part of initializing the head of the merged linked list. If either l1 or l2 are None, then that reference would throw a null pointer exception).

### When to Use a Dummy Node

If you find yourself writing a solution where you need to introduce a special case to initialize the head of a linked list, and the logic for handling the head is the same as the logic for handling the rest of the linked list, you should consider using a dummy node to simplify your solution.

Using a dummy node under these conditions involves the following 3 steps:

1. Creating the dummy node to represent the head of the linked list you are constructing.
2. Now, you can iteratively append nodes to the end that linked list based on the logic of the problem.
3. Returning dummy.next as the head of the linked list you constructed.


This might be confusing, so the best way to understand this concept is through practice.

#### Other Use Cases

Dummy nodes can also simplify the logic of removing a node in a linked list. As we saw above, removing a node in a linked list requires a reference to the previous node of the node you want to remove. By prepending a dummy node to the head of the link list, we can ensure that each node (including the head) has a previous node, and we can avoid handling the head of the linked list as a special case.

#### Removing A Node In A Linked List With A Dummy Node


```python
def deleteNode(head, target):

  dummy = ListNode(0)

  dummy.next = head

  prev = dummy

  curr = head

  while curr:

    if curr.val == target:

      prev.next = curr.next

      break

    prev = curr

    curr = curr.next

  return dummy.next;
```


### Practice Problems

**Swap Nodes in Pairs** [Leetcode #24](https://www.leetcode.com/problems/swap-nodes-in-pairs) | [Solution](https://www.hellointerview.com/learn/code/linked-list/swap-nodes-in-pairs)

Hint: Start by figuring out the pointers you need to manipulate in order to swap two nodes in the middle of a linked list, then think about how using a dummy node can simplify your solution.

**Partition List** [Leetcode #86](https://www.leetcode.com/problems/partition-list)

Hint: Use two dummy nodes!

**Remove Nth Node From End of List** [Leetcode #19](https://www.leetcode.com/problems/remove-nth-node-from-end-of-list) | [Solution](https://www.hellointerview.com/learn/code/linked-list/remove-nth-node-from-end-of-list)

Hint: Use a dummy node to avoid handling the case of removing the head of the linked list as a special case.


































