###### DESCRIPTION (credit [Lintcode.com](https://www.lintcode.com/problem/178/))

You are given an integer n and a list of undirected edges where each entry in the list is a pair of integers representing an edge between nodes 1 and n. You have to write a function to check whether these edges make up a valid tree.

There will be no duplicate edges in the edges list. (i.e. [0, 1] and [1, 0] will not appear together in the list).
###### EXAMPLES

Input:

```js
n = 4  edges = [[0, 1], [2, 3]]
```

![[20250501124541.png]]

Output:

```js
false # the graph is not connected.
```

### Solution

```java
public class Solution {

	/**
	
	* @param n: An integer
	
	* @param edges: a list of undirected edges
	
	* @return: true if it's a valid tree, or false
	
	*/

	List<List<Integer>> adjList = null;
	Set<Integer> set = new HashSet<>();
	
	public boolean validTree(int n, int[][] edges) {
	
		if(n == 0) {
			return true;
		}
	
		if(edges.length != n - 1) {
			return false;
		}

		for(int i = 0; i < n; i++) {
			adjList.add(new ArrayList<>());
		}
	
		for(int[] edge : edges) {
			adjList.get(edge[0]).add(edge[1]);
			adjList.get(edge[1]).add(edge[0]);
		}
	
		boolean noCycle = dfs(0);
		return noCycle && visited.size() == n;
	
	}
	
	private boolean dfs(int currNode) {
	
		if(set.contains(currNode)) return false;

		set.add(currNode);
		for(Integer node : adjList.get(currNode)) {
			dfs(node);
		}
		
		return true;
	}

}
```

#### Time Complexity : O(n * n)
#### Space Complexity : O(n * n)


