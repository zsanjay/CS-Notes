
Given the `root` of a binary tree, return _the length of the longest path, where each node in the path has the same value_. This path may or may not pass through the root.

**The length of the path** between two nodes is represented by the number of edges between them.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/13/ex1.jpg)

**Input:** root = [5,4,5,1,1,null,5]
**Output:** 2
**Explanation:** The shown image shows that the longest path of the same value (i.e. 5).

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/13/ex2.jpg)

**Input:** root = [1,4,5,4,4,null,5]
**Output:** 2
**Explanation:** The shown image shows that the longest path of the same value (i.e. 4).

**Constraints:**

- The number of nodes in the tree is in the range `[0, 104]`.
- `-1000 <= Node.val <= 1000`
- The depth of the tree will not exceed `1000`.

### Solution

```java
class Solution {

	private int Lpath = 0;

	public int longestUnivaluePath(TreeNode root) {

	if(root == null || (root.left == null && root.right == null)) {
		return 0;
	}
	
		pathCalculator(root);
		return Lpath;
	
	}
	
	public int pathCalculator(TreeNode root) {
	
		if(root == null || (root.left == null && root.right == null)) {
			return 0;
		
		}
	
		int left = pathCalculator(root.left);
		int right = pathCalculator(root.right);
		int Tleft = 0, Tright = 0;
		
		//. Left Edge
		if(root.left != null && root.val == root.left.val) {
		
			Tleft += left + 1;
		
		}
		
		//. Right Edge
		
		if(root.right != null && root.val == root.right.val) {
		
			Tright += right + 1;
		
		}
		
		Lpath = Math.max(Lpath, Tleft + Tright);
		return Math.max(Tleft, Tright);

	}

}
```

#### TimeComplexity : O(n)

#### Space Complexity : O(h)

