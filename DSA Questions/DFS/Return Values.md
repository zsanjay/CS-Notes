
In the previous section, we learned how Depth-First Search traverses each node in a binary tree via a series of recursive calls. To solve binary tree interview problems, the next step is to have each recursive call to DFS return a value.
## Recursion

This template is a starting point for solving binary tree problems with Depth-First Search, which takes the base implementation of DFS from the previous section and adds **return values** to each recursive call.

```python
def dfs(node):

    # base case
    if node is None:
        return some value

    ...
    left = dfs(node.left)
    right = dfs(node.right)
    return value based on left and right
```

To solve binary tree problems with DFS, we have to get used to solving problems recursively, which we do in the problem below:

## Problem: Sum of Nodes

###### DESCRIPTION

Given a binary tree, use Depth-First Search to find the sum of all nodes in the tree.

**Input**

![[20250423100720.png]]

**Output** 4 + 2 + 1 + 3 + 7 + 6 + 9 = 32

#### Thinking Recursively

To solve this problem, let's start with an observation:

In the binary tree below, the sum of all nodes equals the value of the root node (4) + the sum of all nodes in the left subtree (6) + the sum of all nodes in the right subtree (22).


![[20250423100753.png]]


Note this applies to every subtree in the tree. The sum of the subtree rooted at Node(2) is equal to 2 + the sum of its left subtree (1) + the sum of its right subtree (3).

![[20250423101125.png]]

The subtrees rooted at the leaf nodes are equal to the value of the leaf nodes, since their left and right subtrees are empty.

In other words, if we know the sum of our left and right subtrees, then we know the sum of our subtree.

```python
sum(node) = sum(node.left) + sum(node.right) + node.val
```

What we've done is expressed the solution to the problem recursively: in terms of smaller subproblems to the same problem (the sum of a tree in terms of its left and right subtrees).

So how can we leverage this observation to solve the problem? By using Depth-First Search!

### Depth-First Search Approach

Let's recall a key point about Depth-First Search: when a recursive call to dfs on a subtree returns, execution returns to the parent of that subtree.

If each recursive call to dfs returns with the sum of its subtree, then the parent node will _receive that value as the sum of either its left or right subtree_. It can then use that value as part of its own subtree sum based on the recursive equation from above.

```
Note how the answer "bubbles up" from the leaf nodes up to the parent nodes until we reach the root node, which is true of all binary tree problems that are solved with Depth-First Search.
```

### Implementation

Now that we know that each recursive call should return the sum of its subtree, we can implement our solution:

The base cases are the subproblems we can solve directly (without making any recursive calls):

- An empty subtree has a sum of 0.
- The subtree rooted at a leaf node has a sum equal to the value of the leaf node.


Otherwise, we make recursive calls to get the sum of our left and right subtrees. We then return the sum of the left subtree, right subtree, and the current node's value.

```python
def dfs(node):

    # base case: empty subtree
    if node is None:
        return 0

    # base case: leaf node
    if node.left is None and node.right is None:
        return node.val

    left = dfs(node.left)
    right = dfs(node.right)
    return left + right + node.val
```

### Solving Problems with Recursion

When solving a binary tree problem with recursion, the first step is to figure out the return value of each recursive call. In the problem above, each recursive call returned the sum of the subtree rooted at the current node.

To determine what the return value should be for a different problem, imagine you're at a node in the tree and ask yourself: "What information do I need from my left and right subtrees to solve the problem for my subtree?"

**Problem**

Find the maximum value in a binary tree

**Explanation** 

If I'm at a node in the tree, what values do I need from my left and right subtrees to find the maximum value for my subtree?

I need to know the maximum value in my left subtree, and the maximum value in my right subtree. The maximum value in my subtree is the maximum of those two values and the value of my node.

![[20250423101629.png]]


This tells me that each recursive call should return the maximum value in the subtree rooted at the current node.

In code, I'll get the max values of my left and right subtrees via recursive calls, and the return statement of each recursive function becomes:

```python
def maxValue(node):
    ... 
    left = maxValue(node.left)
    right = maxValue(node.right)
    return max(left, right, node.val)
```

Finally, we need to add our base case, which are the subproblems we can solve directly:

- An empty subtree has a maximum value of negative infinity.
- The subtree rooted at a leaf node has a maximum value equal to the value of the leaf node.

```python
def maxValue(node):
    if node is None:
        return float('-inf')

    if node.left is None and node.right is None:
        return node.val

    left = maxValue(node.left)
    right = maxValue(node.right)
    return max(left, right, node.val)
```

### Common Mistakes

**Returns Value** 
Not being able to clearly define what each recursive call returns in terms of the node it is called on. This leads to incorrect return values, particulary in the base cases.

**Base Cases** 
Make sure that the return value of the base case and the return value of the recursive case are of the same type. A common mistake is to return None for the base case and an integer in the recursive case.





