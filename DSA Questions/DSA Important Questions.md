
1. [Delete Node in a Linked List](https://leetcode.com/problems/delete-node-in-a-linked-list/)

```
/**

* Definition for singly-linked list.

* public class ListNode {

* int val;

* ListNode next;

* ListNode(int x) { val = x; }

* }

*/

class Solution {

public void deleteNode(ListNode node) {

  

ListNode temp = node;

ListNode prev = null;

while(temp.next != null) {

temp.val = temp.next.val;

prev = temp;

temp = temp.next;

}

prev.next = null;

}

}
```

	Time Complexity : O(n) and Space Complexity : O(1)
	
2. [Permutations](https://leetcode.com/problems/permutations/)


**Recursive Algorithm (Backtracking)**:

```
class Solution {

public List<List<Integer>> permute(int[] nums) {

  

List<List<Integer>> output = new ArrayList<>();

if(nums.length == 1) {

output.add(Arrays.asList(nums[0]));

return output;

}

getPermutations(nums, 0, output);

return output;

}

  
  

public void getPermutations(int[] nums , int start, List<List<Integer>> output) {

  

if(start == nums.length) {

List<Integer> temp = new ArrayList();

for(int num : nums) {

temp.add(num);

}

output.add(temp);

}

  

for(int i = start; i < nums.length; i++) {

swap(nums, i , start);

getPermutations(nums, start + 1, output);

swap(nums, start , i);

}

}

  

public void swap(int[] nums , int i , int j) {

int temp = nums[i];

nums[i] = nums[j];

nums[j] = temp;

}

}
```

### Time Complexity for **Generating Permutations**:

The time complexity is determined by two factors:

1. **Number of permutations**: There are n! permutations of n distinct elements.
2. **Time to generate each permutation**: Generating each individual permutation generally takes O(n) time because it involves arranging n elements in some specific order.

Thus, the total time complexity for generating all permutations is:

	O(n! * n)

Space Complexity : O(n * n)


3. [Subsets](https://leetcode.com/problems/subsets/)

```
class Solution {

public List<List<Integer>> subsets(int[] nums) {

List<List<Integer>> sets = new ArrayList<>();

int n = nums.length;

int subsetsCount = 1 << n; // 2^n subsets

  

for (int i = 0; i < subsetsCount; ++i) {

List<Integer> subset = new ArrayList<>();

for (int j = 0; j < n; ++j) {

if ((i & (1 << j)) != 0) {

subset.add(nums[j]);

}

}

sets.add(subset);

}

return sets;

}

}
```

Time Complexity : O(2^n * n)
Space Complexity : O(2^n * n)


4. [Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)

```
class Solution {

public List<String> generateParenthesis(int n) {

List<String> result = new ArrayList<>();

backtrack(result , n , n, n,"");

return result;

}

  

public void backtrack(List<String> result, int left, int right, int n , String temp) {

  

if(left == 0 && right == 0) {

result.add(temp);

return;

}

  

if(left > 0) {

backtrack(result , left - 1, right , n, temp + "(");

}

  

if(right > left) {

backtrack(result , left, right - 1 , n, temp + ")");

}

}

}

```

Time Complexity : O(2 * n)  -> O(n)
Space Complexity: O(2 * n) -> O(n)


5. - [Find the greatest integer smaller than N with the same set of digits as N](https://serhatgiydiren.com/coding-interview-question-find-the-greatest-integer-smaller-than-n-with-the-same-set-of-digits-as-n/)

```
class Main {
    
    static int max = -1;
    public static void main(String[] args) {
       int output =  findGreatest(630);
       System.out.println(output);
    }
    
    public static int findGreatest(int num) {
        String str = String.valueOf(num);
        getAllPossibleCombinations(str.toCharArray(), 0, num);
        return max;
    }
    
    public static void getAllPossibleCombinations(char[] ch, int index , int actualNum) {
        
        if(index == ch.length) {
           int num = Integer.valueOf(new String(ch));
           if(num < actualNum) {
            max = Math.max(num, max);
           }
        }
        
        for(int i = index; i < ch.length; i++) {
            swap(ch , i , index);
            getAllPossibleCombinations(ch , index + 1 , actualNum);
            swap(ch , i , index);
        }
    }
    
    
    public static void swap(char[] ch, int i, int j) {
        if(i == j) {
            return;
        }
        char temp = ch[i];
        ch[i] = ch[j];
        ch[j] = temp;
    }
}
```


 Time Complexity : O(n * 2) - N is the number of digits
 Space Complexity : O(n) - Stack space due to recursion.


6. [Rotate Image](https://leetcode.com/problems/rotate-image/)

```
class Solution {

public void rotate(int[][] matrix) {

for(int i = 0; i < matrix.length; i++) {

for(int j = 0; j < i; j++) {

int temp = matrix[i][j];

matrix[i][j] = matrix[j][i];

matrix[j][i] = temp;

}

}

  
  

for(int i = 0; i < matrix.length; i++) {

int start = 0, end = matrix[0].length - 1;

  

while(start < end) {

int temp = matrix[i][start];

matrix[i][start] = matrix[i][end];

matrix[i][end] = temp;

start++;

end--;

}

}

}

}
```

Time Complexity : O(N * N)
Space Complexity : O(1)

7. [Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)

```
/**

* Definition for a binary tree node.

* public class TreeNode {

* int val;

* TreeNode left;

* TreeNode right;

* TreeNode() {}

* TreeNode(int val) { this.val = val; }

* TreeNode(int val, TreeNode left, TreeNode right) {

* this.val = val;

* this.left = left;

* this.right = right;

* }

* }

*/

class Solution {

public int kthSmallest(TreeNode root, int k) {

List<Integer> list = new ArrayList<>();

traverseBST(root , list);

return list.get(k - 1);

}

  

public static void traverseBST(TreeNode root, List<Integer> list) {

  

if(root == null) {

return;

}

  

traverseBST(root.left, list);

list.add(root.val);

traverseBST(root.right, list);

}

}
```

Time Complexity : O(n)
Space Complexity : O(n)