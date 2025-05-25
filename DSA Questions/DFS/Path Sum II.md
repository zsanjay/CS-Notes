
Given the `root` of a binary tree and an integer `targetSum`, return _all **root-to-leaf** paths where the sum of the node values in the path equals_ `targetSum`_. Each path should be returned as a list of the node **values**, not node references_.

A **root-to-leaf** path is a path starting from the root and ending at any leaf node. A **leaf** is a node with no children.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/18/pathsumii1.jpg)

**Input:** root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
**Output:** [[5,4,11,2],[5,8,4,5]]
**Explanation:** There are two paths whose sum equals targetSum:
5 + 4 + 11 + 2 = 22
5 + 8 + 4 + 5 = 22

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/18/pathsum2.jpg)

**Input:** root = [1,2,3], targetSum = 5
**Output:** []

**Example 3:**

**Input:** root = [1,2], targetSum = 0
**Output:** []

**Constraints:**

- The number of nodes in the tree is in the range `[0, 5000]`.
- `-1000 <= Node.val <= 1000`
- `-1000 <= targetSum <= 1000`

### Solution

```java
class Solution {

public List<List<Integer>> pathSum(TreeNode root, int targetSum) {
	List<List<Integer>> list = new ArrayList<>();

	if(root == null) {
		return list;
	}
	
	findPathSum(root, targetSum, list, 0, new ArrayList<>());
	return list;
}

  

public void findPathSum(TreeNode root, int targetSum, List<List<Integer>> list, int currSum, List<Integer> temp) {

	temp.add(root.val);
	currSum += root.val;
	
	if(root.left == null && root.right == null) {
		if(targetSum == currSum) {
			list.add(new ArrayList<>(temp));
		}
	}
	
	if(root.left != null) {
		findPathSum(root.left , targetSum, list, currSum, temp);
	}
	
	if(root.right != null) {
		findPathSum(root.right , targetSum, list, currSum, temp);
	}
	
	temp.remove(temp.size() - 1);
	
	}

}
```

Time Complexity : O(N)
Space Complexity : O(H) - call stack