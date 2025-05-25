
An adjacency list is a common way to represent a graph. In an adjacency list, we are given a list of nodes, where each node is mapped to a list of its neighbors.

In Python, we can create an adjacency list using a dictionary where the keys are the nodes and the values are the list of nodes each node is connected to.

![[20250430094143.png]]

Adjacency lists allow you to look up the neighbors of any node in O(1) time, which is a necessary step for depth-first search.

#### Constructing Adjacency Lists

Some questions require you to build an adjacency list from a list of edges, which is shown below:

###### DESCRIPTION

Given an integer n which represents the number of nodes in a graph, and a list of edges edges, where edges[i] = [ui, vi] represents a bidirectional edge between nodes ui and vi, write a function to return the adjacency list representation of the graph as a dictionary. The keys of the dictionary should be the nodes, and the values should be a list of the nodes each node is connected to.

**Example:**

n = 4 edges = [[0, 1], [1, 2], [2, 3], [3, 0], [0, 2]]

![[20250430094332.png]]

**Output:**

```
{     
	0: [1, 3, 2],
	1: [0, 2],
    2: [1, 3, 0],
	3: [2, 0]
}
```

### Solution

```python
def build_adj_list(n, edges):

    adj_list = {i: [] for i in range(n)}

    for u, v in edges:

        adj_list[u].append(v)

        adj_list[v].append(u)

    return adj_list
```

#### DFS on an Adjacency List

DFS for a graph is conceptually similar to DFS on a binary tree. The algorithm tries to go as deep as possible along a path before backtracking to explore other paths.

The key differences are:

1. Each node can have any amount of neighbors, so we need a for loop to iterate over each neighbor rather than making calls to the left and right children of the current node.

2. Because graphs can contain cycles, **we need to keep track of nodes we have already visited**. If we encounter a node that has already been visited, we should return immediately without making any further recursive calls (or skip it in the loop altogether). Otherwise, we may end up in an infinite loop.

3. We don't need an explicit base case like we do in the trees. Eventually, we will visit all nodes in the graph, and the recursion will stop on its own (with the help of the visited set).

Here's a basic implementation of DFS on an adjacency list. Notice how we use a visited set to keep track of the nodes we have visited, and how we return immediately if we encounter a node that has already been visited.

```python
adjList = {
    "1": ["2", "4"],
    "2": ["1", "3"],
    "3": ["2", "4"],
    "4": ["1", "3", "5"],
    "5": ["4"]
}
```

![[20250430094515.png]]

#### Summary

- Use a set to keep track of visited nodes. Each time you visit a node, add it to the set.

- If you encounter a node that has already been visited, return immediately without making any further recursive calls.

- Use a for loop to iterate over each neighbor of the current node, and recursively call dfs on each neighbor.

The next step is to practice using DFS to solve graph questions involving adjacency lists.





