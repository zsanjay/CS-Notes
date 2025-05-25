
Depth-First Search (DFS) is an algorithm that is used to visit all nodes in a tree or graph-like data structure. We'll start by learning the order in which visits the nodes in a **binary tree**.

### Binary Tree Properties

The top node of a binary tree is called the **root**. Each node in a binary tree can have at most two children, referred to as the **left child** and **right child**. A node that does not have any children is called a **leaf node**:

![[0250423095701.png]]

**Height of a Binary Tree** 

The height of a binary tree is the number of edges on the longest path between the root node and a leaf node. This is also known as the **depth** of the tree.

![[20250423095735.png]]

**Balanced Binary Tree** 

A binary tree is balanced if the height of the left and right subtrees of every node differ by at most 1.

![[20250423095824.png]]


**Complete Binary Tree** 

A binary tree is complete if every level, except possibly the last, is completely filled, and all nodes are as far left as possible. Complete binary trees are an important concept in [heap data structures](https://www.hellointerview.com/learn/code/heap/overview).

![[20250423095902.png]]

A complete binary tree has a height of O(log(n)), where n is the number of nodes in the tree.

**Binary Search Tree** A binary search tree (BST) is a binary tree where:

- All nodes in the left subtree of the root have a value less than the root.
- All nodes in the right subtree of the root have a value greater than the root.

The same property applies to all subtrees in the tree. This property allows for efficient search, insertion, and deletion of nodes in the tree.

![[20250423095942.png]]

### Depth-First Search (DFS)

Depth-First Search is an algorithm used to traverse each node in a binary tree. It starts at the root node and tries to go "down" as far as possible until reaching a leaf node. When it reaches a leaf node, it "backtracks" to the parent node to explore the next path.

This "go deep as far as possible and then backtrack" behavior differentiates DFS from Breadth-First Search (BFS), which explores all the nodes at "level" before moving on to the next level.

Let's now look at the implementation of DFS on a binary tree. We'll pay special attention to the role that recursion and the call stack play in the algorithm.

#### Recursion and the Call Stack

Depth-First Search is typically implemented as a recursive function, or a function that calls itself within the body of the function.

![[20250423100125.png]]

Working with any recursive function requires a good understanding of the **call stack**. The call stack is a stack-like data structure that keeps track of the function calls that are currently being executed.

Below, we'll visualize how the call stack grows and shrinks as we traverse a binary tree using DFS. In particular, we'll learn how the call stack allows DFS to **backtrack**.

#### Call Frame

The initial call to dfs is made on the root node of the binary tree. This creates a **call frame** (the left panel below), which contains the current line of code being executed and the variables that are local to the function call.

#### Pushing to the Call Stack

Since node is not None, the first step of our dfs function is to visit the left child of the root node by making a recursive call to dfs(node.left).

This pushes a new call frame onto the **call stack**, where node: Node(2). Execution begins at the first line of code in this new call frame.

#### Base Case

As we make recursive calls to traverse down the tree, we keep pushing call frames onto the call stack until we reach our first **base case**, where node is None.

A base case is a condition that stops DFS from traversing down.

#### Backtracking

Here, we reach our first return statement. When the function returns, the call frame is popped off the call stack, and execution returns to the call frame that is now at the top of the call stack. **This is backtracking!**

After backtracking, we make a recursive call to dfs(node.right), which is another base case that returns immediately.

#### Backtracking (continued)

At this point, we have finished visiting all the nodes in the left and right subtrees of the current node Node(1). The current call frame is popped off the call stack, and we backtrack to the parent node Node(2), which then makes a recursive call to dfs(node.right).

This process continues until we have visited all the nodes in the binary tree.

### Time and Space Complexity

If there a N nodes in a binary tree, then a depth-first search based solution will visit each node exactly once.

**Time Complexity**: Find the work done per recursive call, and multiply it by N. All of the examples (aside from the merging lists example) perform a constant amount of work per recursive call, so the time complexity is O(N).

**Space Complexity**: The memory that each recursive call occupies on the call stack is part of the space complexity of a problem. In depth-first search problems, a recursive call is made for each node in the tree, so the space complexity is at least O(N). This is in addition to the amout of space required for each recursive call itself, which differs from problem to problem.

### Summary

- Depth-First Search visits every node in a binary tree by going "down" as far as possible before backtracking to visit the nodes on the next path.

- Depth-First Search is typically implemented as a recursive function. It visits new nodes in the tree by making recurisve calls. When a recursive call is made, a new call frame is pushed onto the call stack.

- Backtracking occurs whenever a recursive call returns. The call frame is popped off the call stack, and execution returns to the next call frame on the call stack.


Another key insight is that whenever a recursive function returns, we have finished visiting all nodes in the left and right subtrees of the current node. This leads us to **Return Values**, which is the first unit in solving binary tree problems with DFS.










