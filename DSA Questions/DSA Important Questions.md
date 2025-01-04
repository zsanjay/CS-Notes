
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

8. [Game of Life](https://leetcode.com/problems/game-of-life/)

```
class Solution {

public void gameOfLife(int[][] board) {

int cells = board.length * board[0].length;
nextState(board, 0, cells);

}

  

private void nextState(int[][] board, int position, int cells) {

if(position >= cells) return;

  

int n = board.length, m = board[0].length;

int row = position/m, col = position % m;

int live = 0;

  

int[][] neighbours = { {-1,-1},{-1, 0},{-1, 1},

{ 0,-1}, { 0, 1},

{ 1,-1},{ 1, 0},{ 1, 1} };

  

for (int[] neighbour : neighbours) {

int r = row + neighbour[0], c = col + neighbour[1];

if (r >= 0 && r < n && c >= 0 && c < m && board[r][c] == 1) live++;

}

  

if(board[row][col] == 0 && live == 3) {

nextState(board, position+1, cells);

board[row][col] = 1;

} else if(board[row][col] == 1 && (live == 2 || live == 3)) {

nextState(board, position+1, cells);

} else {

nextState(board, position+1, cells);

board[row][col] = 0;

}

}

}
```

9. [Group Anagrams](https://leetcode.com/problems/group-anagrams/)
```
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        
        List<List<String>> output = new ArrayList<>();
        Map<String, List<String>> map = new HashMap<>();

        for(int i = 0; i < strs.length; i++) {
          String sortedStr = sortString(strs[i]);
          if(map.containsKey(sortedStr)) {
            map.get(sortedStr).add(strs[i]);
          } else {
            List<String> list = new ArrayList<>();
            list.add(strs[i]);
            map.put(sortedStr , list);
          }
        }

        for(Map.Entry<String, List<String>> entry : map.entrySet()) {
            output.add(entry.getValue());
        }

        return output;
    }

    public String sortString(String str){

        if(str.equals("")) {
            return str;
        }

        char[] ch = str.toCharArray();
        Arrays.sort(ch);
        return new String(ch);
    }
}
```

Time Complexity : O(nlogn * n)
Space Complexity : O(n * n)


10. [Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)

```
class Solution {

public int findKthLargest(int[] nums, int k) {

Queue<Integer> minHeap = new PriorityQueue<>();

  

for(int i = 0; i < k; i++) {

minHeap.add(nums[i]);

}

  

for(int i = k; i < nums.length; i++) {

if(nums[i] > minHeap.peek()) {

minHeap.poll();

minHeap.add(nums[i]);

}

}

  

return minHeap.peek();

}

}
```

Time Complexity : O(n) and Space Complexity : O(n)

11. [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)

```
class Solution {

public int[] topKFrequent(int[] nums, int k) {

  

List<Integer>[] bucket = new List[nums.length + 1];

Map<Integer, Integer> map = new HashMap<>();

  

for(int i = 0; i < nums.length; i++) {

map.put(nums[i] , map.getOrDefault(nums[i] , 0) + 1);

}

  
  

for(int key : map.keySet()) {

int freq = map.get(key);

if(bucket[freq] == null) {

bucket[freq] = new ArrayList<>();

}

bucket[freq].add(key);

}

  
  

int[] ans = new int[k];

int pos = 0;

for(int i = bucket.length - 1; i >= 0; i--) {

if(bucket[i] != null) {

for(int j = 0; j < bucket[i].size() && pos < k; j++) {

ans[pos] = bucket[i].get(j);

pos++;

}

}

}

  

return ans;

}

}
```

- Time complexity:  
    O(n) --> bucket storing only nums values.
- Space complexity:  
    O(n)


12. [Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)

```
class Solution {

public int[] productExceptSelf(int[] nums) {

int[] temp = new int[nums.length];

temp[0] = 1;

  

for(int i = 1; i < nums.length; i++) {

temp[i] = temp[i - 1] * nums[i - 1];

}

  

int right = 1;

for(int i = nums.length - 1; i >= 0; i--) {

temp[i] = right * temp[i];

right = right * nums[i];

}

  

return temp;

}

}
```

13. [Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

```
class Solution {

public int maxProfit(int[] prices) {

  

if(prices.length == 1) {

return 0;

}

  

int buy = prices[0];

int profit = 0;

for(int i = 1; i < prices.length; i++) {

// buy is greater than current price

if(buy > prices[i]) {

buy = prices[i];

} else {

int day = i;

while(day < prices.length - 1 && prices[day] < prices[day + 1]) {

day++;

}

profit += prices[day] - buy;

buy = prices[day];

i = day;

}

}

return profit;

}

}
```

14. [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)

```java
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

public List<List<Integer>> levelOrder(TreeNode root) {

List<List<Integer>> list = new ArrayList<>();

if(root == null) {

return list;

}
  

if(root.left == null && root.right == null) {

List<Integer> temp = new ArrayList<>();

temp.add(root.val);

list.add(temp);

return list;

}

  

Queue<TreeNode> queue = new LinkedList<>();

queue.add(root);

  

while(!queue.isEmpty()) {

int size = queue.size();

List<Integer> levels = new ArrayList<>();


while(size-- > 0) {

TreeNode node = queue.poll();

levels.add(node.val);

  

if(node.left != null) {

queue.add(node.left);

}

  

if(node.right != null) {

queue.add(node.right);

}

}

list.add(levels);

}


return list;

}

}

```

Time Complexity : O(n)
Space Complexity : O(n)

15. [Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/)

```java
class Solution {

public List<List<String>> partition(String s) {

  

List<List<String>> list = new ArrayList<>();

partition(s, 0, list, new ArrayList<>());

return list;

}

  

public void partition(String s, int index, List<List<String>> list, List<String> temp) {

if(index == s.length()) {

list.add(new ArrayList<>(temp));

return;

}

  

for(int i = index; i < s.length(); i++) {

if(isPalindrome(s, index, i)) {

temp.add(s.substring(index, i + 1));

partition(s, i + 1, list, temp);

temp.remove(temp.size() - 1);

}

}

}

public boolean isPalindrome(String str, int start, int end) {

if(start == end) {

return true;

}

  

while(start < end) {

if(str.charAt(start) != str.charAt(end)) {

return false;

}

start++;

end--;

}
  
return true;

}

}
```

Time Complexity : O(n * 2^n)
Space Complexity : O(1)


16. [Unique Paths](https://leetcode.com/problems/unique-paths/)

```java
class Solution {

public int uniquePaths(int m, int n) {

int[][] cache = new int[m][n];

int paths = findUniquePaths(m, n , cache);

return paths;

}

  

public int findUniquePaths(int m, int n, int[][] cache) {

  

if(m == 1 && n == 1) {

return 1;

}

  

if(m < 1 || n < 1) {

return 0;

}

  

if(cache[m - 1][n - 1] != 0) {

return cache[m - 1][n - 1];

}

  

int left = findUniquePaths(m - 1 , n, cache);

int up = findUniquePaths(m , n - 1, cache);

cache[m - 1][n - 1] = left + up;

return left + up;
}

}
```


17. [Kth Smallest Element in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/)

https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/solutions/394294/using-binary-search-in-java-and-analysis

https://www.youtube.com/watch?v=gQuH27Xz5mk

```java
class Solution {

public int kthSmallestUsingSorting(int[][] matrix, int k) {

  

if(matrix.length == 1) {

return matrix[0][k - 1];

}

  

int[] nums = new int[matrix.length * matrix.length];

int idx = 0;

  

for(int i = 0; i < matrix.length; i++) {

for(int j = 0; j < matrix[i].length; j++) {

nums[idx++] = matrix[i][j];

}

}

  

Arrays.sort(nums);

return nums[k - 1];

  

}

  

public int kthSmallest(int[][] matrix, int k) {

  

int m = matrix.length, n = matrix[0].length;

int low = matrix[0][0], high = matrix[m - 1][n - 1];

  

while(low < high) {

  

int mid = (high - low) / 2 + low;

int count = 0;

int j = n - 1;

for(int i = 0; i < m; i++) {

while(j >= 0 && matrix[i][j] > mid) {

j--;

}

count += j + 1;

}

  

if(count < k) {

low = mid + 1;

} else {

high = mid;

}

  

}

return low;

}

}
```

Time Complexity : O(N ^ 3)
Space Complexity : O(1)

```
Example 1:

Input: matrix = [[1,5,9],[10,11,13],[12,13,15]], k = 8
Output: 13
Explanation: The elements in the matrix are [1,5,9,10,11,12,13,13,15], and the 8th smallest number is 13

Console Ouput:

low = 1 and high 15 
mid = 8 
count = 2 
low = 9 and high 15 
low = 9 and high 15 
mid = 12 
count = 6 
low = 13 and high 15 
low = 13 and high 15 
mid = 14 
count = 8 
low = 13 and high 14 
low = 13 and high 14 
mid = 13 
count = 8 
low = 13 and high 13

Example 2:

Input: matrix = [[-5]], k = 1
Output: -5

```

18. [Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/)

```java
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

public int minDepthDFS(TreeNode root) {

if(root == null) {

return 0;

}

int left = minDepth(root.left);

int right = minDepth(root.right);

return (left == 0 || right == 0) ? left + right + 1 : Math.min(left,right) + 1;

}


public int minDepth(TreeNode root) {

if(root == null) {

return 0;

}

Queue<TreeNode> queue = new LinkedList<>();

queue.add(root);

  

int depth = 1;

  

while(!queue.isEmpty()) {

  

int size = queue.size();

  

while(size-- > 0) {

TreeNode node = queue.poll();

  

if(node.left == null && node.right == null) {

return depth;

}

  

if(node.left != null) {

queue.add(node.left);

}

if(node.right != null) {

queue.add(node.right);

}

}

depth++;

}

  

return depth;

}

}
```

Time Complexity : O(n)
Space Complexity : O(n)

19. [Flatten Nested List Iterator](https://leetcode.com/problems/flatten-nested-list-iterator/)

```java

/**

* // This is the interface that allows for creating nested lists.

* // You should not implement it, or speculate about its implementation

* public interface NestedInteger {

*

* // @return true if this NestedInteger holds a single integer, rather than a nested list.

* public boolean isInteger();

*

* // @return the single integer that this NestedInteger holds, if it holds a single integer

* // Return null if this NestedInteger holds a nested list

* public Integer getInteger();

*

* // @return the nested list that this NestedInteger holds, if it holds a nested list

* // Return empty list if this NestedInteger holds a single integer

* public List<NestedInteger> getList();

* }

*/

public class NestedIterator implements Iterator<Integer> {

  

private final List<NestedInteger> nestedList;

private final List<Integer> list;

private int currentIndex = 0;

private int nestedIndex = 0;

  

public NestedIterator(List<NestedInteger> nestedList) {

this.nestedList = nestedList;

this.list = new ArrayList<>();

populateList();

}

  

@Override

public Integer next() {

Integer nextInteger = list.get(currentIndex++);

if(currentIndex == list.size()) {

if(nestedIndex < nestedList.size()) {

populateList();

}

}

return nextInteger;

}

  

@Override

public boolean hasNext() {

return currentIndex < list.size();

}

  

private void populateList() {

if(nestedIndex >= nestedList.size()) {

return;

}

NestedInteger nestedInteger = this.nestedList.get(nestedIndex++);

populateList(nestedInteger);

}

  

private void populateList(NestedInteger nestedInteger) {

if(nestedInteger.isInteger()) {

this.list.add(nestedInteger.getInteger());

} else {

if(nestedInteger.getList().isEmpty()) {

populateList();

} else {

for(NestedInteger nested : nestedInteger.getList()) {

populateList(nested);

}

}

}

}

}

  

/**

* Your NestedIterator object will be instantiated and called as such:

* NestedIterator i = new NestedIterator(nestedList);

* while (i.hasNext()) v[f()] = i.next();

*/
```

Time Complexity : O(n * n)
Space Complexity : O(n * n)

20. [Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/)

```java

class Trie {

  

Node root;

  

public Trie() {

root = new Node();

}

public void insert(String word) {

root.insert(word, word.length(), 0);

}

public boolean search(String word) {

return root.search(word, word.length(), 0);

}

public boolean startsWith(String prefix) {

return root.startsWith(prefix, prefix.length(), 0);

}

  

class Node {

  

Node[] nodes;

boolean isEnd;

  

public Node() {

nodes = new Node[26];

}

  

private void insert(String word, int len, int idx) {

if(idx >= len) return;

int i = word.charAt(idx) - 'a';

if(nodes[i] == null) {

nodes[i] = new Node();

}

  

if(idx == len - 1) nodes[i].isEnd = true;

nodes[i].insert(word, len, idx + 1);

}

  

private boolean search(String word, int len, int idx) {

if(idx >= len) return false;

int i = word.charAt(idx) - 'a';

if(nodes[i] == null) return false;

if(idx == len - 1 && nodes[i].isEnd) return true;

  

return nodes[i].search(word, len, idx + 1);

}

  

private boolean startsWith(String prefix, int len, int idx) {

if(idx >= len) return false;

Node node = nodes[prefix.charAt(idx) - 'a'];

if(node == null) return false;

if(idx == len - 1) return true;

  

return node.startsWith(prefix, len, idx + 1);

}

  
  

}

  

}

  

/**

* Your Trie object will be instantiated and called as such:

* Trie obj = new Trie();

* obj.insert(word);

* boolean param_2 = obj.search(word);

* boolean param_3 = obj.startsWith(prefix);

*/
```

21. [Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

```java
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

public TreeNode buildTree(int[] preorder, int[] inorder) {

if(preorder.length == 1) {

return new TreeNode(preorder[0]);

}

  

int[] current = {0};

  

Map<Integer, Integer> inOrderMap = new HashMap<>();

for(int i = 0; i < inorder.length; i++) {

inOrderMap.put(inorder[i], i);

}

  

return buildTree(preorder, current, 0, preorder.length - 1, inOrderMap);

}

  

public TreeNode buildTree(int[] preorder, int[] current, int low, int high, Map<Integer, Integer> inMap){

if (current[0] >= preorder.length) return null;

TreeNode root = new TreeNode(preorder[current[0]]);

if (low > high) return null;

else {

current[0] += 1;

int i = inMap.get(root.val);

root.left = buildTree(preorder, current, low, i - 1, inMap);

root.right = buildTree(preorder, current, i + 1, high, inMap);

}

return root;

}

}
```

Time Complexity : O(N)
Space Complexity : O(N)

22. [Odd Even Linked List](https://leetcode.com/problems/odd-even-linked-list/) - https://leetcode.com/problems/odd-even-linked-list/solutions/1607118/topic
```java
/**

* Definition for singly-linked list.

* public class ListNode {

* int val;

* ListNode next;

* ListNode() {}

* ListNode(int val) { this.val = val; }

* ListNode(int val, ListNode next) { this.val = val; this.next = next; }

* }

*/

class Solution {

public ListNode oddEvenList(ListNode head) {

  

if(head == null) {

return head;

}

ListNode odd = head, even = head.next, temp = even;

  

while(even != null && even.next != null) {

odd.next = even.next;

odd = odd.next;

even.next = odd.next;

even = even.next;

}

  

odd.next = temp;

return head;

}

}

```

Time Complexity : O(N)
Space Complexity : O(1)

23. [Populating Next Right Pointers in Each Node](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/)

```java
/*

// Definition for a Node.

class Node {

public int val;

public Node left;

public Node right;

public Node next;

  

public Node() {}

public Node(int _val) {

val = _val;

}

  

public Node(int _val, Node _left, Node _right, Node _next) {

val = _val;

left = _left;

right = _right;

next = _next;

}

};

*/

  

class Solution {

public Node connect(Node root) {

if(root == null) {

return root;

}

  

Queue<Node> queue = new LinkedList<>();

queue.add(root);

  

while(!queue.isEmpty()) {

  

int size = queue.size();

Node prev = null;

  

while(size-- > 0) {

Node node = queue.poll();

  

if(prev != null) {

prev.next = node;

}

prev = node;

  

if(node.left != null) {

queue.add(node.left);

}

  

if(node.right != null) {

queue.add(node.right);

}

}

prev.next = null;

}

  

return root;

}

}
```

Time Complexity = O(n)
Space Complexity = O(n)

24. [Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/)

```java

class Solution {

public int findDuplicate(int[] nums) {

for(int i = 0; i < nums.length; i++) {

int index = Math.abs(nums[i]);

if(nums[index] < 0) {

return index;

}

nums[index] = -1 * nums[index];

}

return nums.length;

}

}
```

Time Complexity : O(n)
Space Complexity : O(1)

25.[Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)

Prerequisite -  https://takeuforward.org/data-structure/print-root-to-node-path-in-a-binary-tree/

```java

/**

* Definition for a binary tree node.

* public class TreeNode {

* int val;

* TreeNode left;

* TreeNode right;

* TreeNode(int x) { val = x; }

* }

*/

class Solution {

public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {

List<TreeNode> pathToP = new ArrayList<>();

List<TreeNode> pathToQ = new ArrayList<>();

getPath(root , p , pathToP);

getPath(root , q , pathToQ);

  

for(int i = 0; i < pathToP.size(); i++) {

  

if(pathToQ.contains(pathToP.get(i))) {

return pathToP.get(i);

}

}

  

return null;

}

  

public boolean getPath(TreeNode root, TreeNode node, List<TreeNode> list) {

  

if(root == null) {

return false;

}

  

if(root.val == node.val) {

list.add(root);

return true;

}

  

if(getPath(root.left , node, list) || getPath(root.right , node, list)) {

list.add(root);

return true;

}

  

return false;

}

}
```

Time Complexity : O(h * h)
Space Complexity : O(h)

26. [Shuffle an Array](https://leetcode.com/problems/shuffle-an-array/)

```java

class Solution {

  

int[] nums;

int[] originalArray;

Random rand;

  

public Solution(int[] nums) {

this.nums = nums;

this.originalArray = new int[nums.length];

rand = new Random();

  

for(int i = 0; i < nums.length; i++) {

originalArray[i] = nums[i];

}

}

public int[] reset() {

for(int i = 0; i < nums.length; i++) {

nums[i] = originalArray[i];

}

return nums;

}

public int[] shuffle() {

for (int i = 0; i < nums.length; i++) {

int randomIndexToSwap = rand.nextInt(nums.length);

int temp = nums[randomIndexToSwap];

nums[randomIndexToSwap] = nums[i];

nums[i] = temp;

}

return nums;

}

}

  

/**

* Your Solution object will be instantiated and called as such:

* Solution obj = new Solution(nums);

* int[] param_1 = obj.reset();

* int[] param_2 = obj.shuffle();

*/
```

27. [4Sum II](https://leetcode.com/problems/4sum-ii/)

```java
class Solution {

public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {

int noOfTuples = 0;

int len = nums1.length;

  

if(len == 1 && nums1[0] == nums2[0] && nums2[0] == nums3[0] && nums3[0] == nums4[0]) {

return 1;

}

  

Map<Integer, Integer> sumCount = new HashMap<>();

  

for(int i = 0; i < len; i++) {

for(int j = 0; j < len; j++) {

int sum = nums1[i] + nums2[j];

sumCount.put(sum , sumCount.getOrDefault(sum , 0) + 1);

}

}

  

for(int i = 0; i < len; i++) {

for(int j = 0; j < len; j++) {

int sum = (nums3[i] + nums4[j]) * -1;

if(sumCount.containsKey(sum)) {

noOfTuples += sumCount.get(sum);

}

}

}

return noOfTuples;

}

}
```

Time Complexity : O(n * n)
Space Complexity : O(n)

28. [Sort Colors](https://leetcode.com/problems/sort-colors/)

```java
class Solution {

public void sortColors(int[] nums) {

if(nums.length == 1) {

return;

}

  

int zeros = 0, ones = 0;

  

for(int num : nums) {

if(num == 0) {

zeros++;

} else if(num == 1) {

ones++;

}

}

  

int idx = 0;

  

while(zeros-- > 0) {

nums[idx++] = 0;

}

  

while(ones-- > 0) {

nums[idx++] = 1;

}

  

while(idx < nums.length) {

nums[idx++] = 2;

}

}

}
```

Time Complexity : O(n)
Space Complexity : O(1)

29. [Valid Sudoku](https://leetcode.com/problems/valid-sudoku/)

```java

class Solution {

public boolean isValidSudoku(char[][] board) {

Set<Character> rowSet = new HashSet<>();

Set<Character> colSet = new HashSet<>();

Set<Character> subBox1Set = new HashSet<>();

Set<Character> subBox2Set = new HashSet<>();

Set<Character> subBox3Set = new HashSet<>();

int rowCharCount = 0, colCharCount = 0, subBox1Count = 0, subBox2Count = 0, subBox3Count = 0;

  

for(int i = 0; i < board.length; i++) {

for(int j = 0; j < board[i].length; j++) {

  

if(board[j][i] != '.') {

colSet.add(board[j][i]);

colCharCount++;

}

if(board[i][j] != '.') {

rowSet.add(board[i][j]);

rowCharCount++;

  

if(j < 3) {

subBox1Set.add(board[i][j]);

subBox1Count++;

}

  

if(j > 2 && j < 6) {

subBox2Set.add(board[i][j]);

subBox2Count++;

}

if(j > 5) {

subBox3Set.add(board[i][j]);

subBox3Count++;

}

}

}

  

if(i == 2 || i == 5 || i == 8) {

  

if(subBox1Set.size() != subBox1Count || subBox2Set.size() != subBox2Count || subBox3Set.size() != subBox3Count) {

return false;

}

subBox1Set.clear();

subBox2Set.clear();

subBox3Set.clear();

subBox1Count = 0;

subBox2Count = 0;

subBox3Count = 0;

  

}

  

if(rowSet.size() != rowCharCount || colSet.size() != colCharCount) {

return false;

}

  

rowSet.clear();

colSet.clear();

rowCharCount = 0;

colCharCount = 0;

}

return true;

}

  

}
```

Time Complexity : O(n * n)
Space Complexity : O(n)

30. [Number of Islands](https://leetcode.com/problems/number-of-islands/)

```java

class Solution {

public int numIslands(char[][] grid) {

int m = grid.length, n = grid[0].length;

int noOfIsland = 0;

  

for(int i = 0; i < m; i++) {

for(int j = 0; j < n; j++) {

if(grid[i][j] == '1') {

noOfIsland++;

visitNeighbours(grid, i , j, m , n);

}

}

}

  

return noOfIsland;

}

  

private void visitNeighbours(char[][] grid, int i, int j, int m, int n) {

if( i < 0 || j < 0 || i == m || j == n || grid[i][j] == '0') {

return;

}

grid[i][j] = '0';

visitNeighbours(grid, i - 1 , j, m , n);

visitNeighbours(grid, i , j - 1, m , n);

visitNeighbours(grid, i + 1 , j, m , n);

visitNeighbours(grid, i , j + 1, m , n);

}

}
```

Time Complexity : O(N * N * max(m , n))
Space Complexity : O(max(m,n))

31. [Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

```java
class Solution {

public List<String> letterCombinations(String digits) {

  

List<String> combinations = new ArrayList<>();

  

if(digits.length() == 0) {

return combinations;

}

Map<Character, String> dLMap = new HashMap<>();

  

dLMap.put('2' , "abc");

dLMap.put('3' , "def");

dLMap.put('4' , "ghi");

dLMap.put('5' , "jkl");

dLMap.put('6' , "mno");

dLMap.put('7' , "pqrs");

dLMap.put('8' , "tuv");

dLMap.put('9' , "wxyz");

  

List<String> letters = new ArrayList<>();

  

for(int i = 0; i < digits.length(); i++) {

char ch = digits.charAt(i);

String letter = dLMap.get(ch);

letters.add(letter);

}

getCombinations(letters, 0 , letters.size(), new StringBuilder(), combinations);

return combinations;

}

  

private void getCombinations(List<String> letters, int index, int listSize, StringBuilder sb, List<String> combinations) {

  

if(listSize == sb.length()) {

combinations.add(new String(sb.toString()));

return;

}

  

for(int i = index; i < listSize; i++) {

String word = letters.get(i);

for(int j = 0; j < word.length(); j++) {

sb.append(word.charAt(j));

getCombinations(letters, i + 1 , listSize, sb, combinations);

sb.deleteCharAt(sb.length() - 1);

}

}

}

  

}
```

Time Complexity : O(8 * 4 * 4)
Space Complexity : O(4 * N) - where n is the number of combinations and 4 is the maximum character in each string. 

32. [Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)

```java

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

public List<List<Integer>> zigzagLevelOrder(TreeNode root) {

if(root == null) {

return Collections.emptyList();

}

List<List<Integer>> outputList = new ArrayList<>();

  

if(root.left == null && root.right == null) {

List<Integer> output = new ArrayList<>();

output.add(root.val);

outputList.add(output);

return outputList;

}

  

Queue<TreeNode> queue = new LinkedList<>();

queue.add(root);

boolean isLevelOdd = false;

  

while(!queue.isEmpty()) {

int size = queue.size();

LinkedList<Integer> tempList = new LinkedList<>();

  

while(size-- > 0) {

TreeNode node = queue.poll();

  

if(isLevelOdd) {

tempList.addFirst(node.val);

} else {

tempList.add(node.val);

}

  

if(node.left != null) {

queue.add(node.left);

}

  

if(node.right != null) {

queue.add(node.right);

}

}

isLevelOdd = !isLevelOdd;

outputList.add(tempList);

}

return outputList;

}

}

```

Time Complexity : O(n) - Traversing all nodes
Space Complexity : O(k)  - where is k is the maximum number of elements in the queue at a time. 

33. [Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

```java
class Solution {

public int maxArea(int[] height) {

  

int start = 0, end = height.length - 1;

int maxArea = 0;

  

while(start < end) {

  

int leftHeight = height[start];

int rightHeight = height[end];

  

int minHeight = Math.min(leftHeight , rightHeight);

int area = minHeight * (end - start);

  

if(area > maxArea)

maxArea = area;

if(leftHeight < rightHeight) {

start++;

} else {

end--;

}

}

  

return maxArea;

}

}
```

Time Complexity : O(n)
Space Complexity : O(1)

34. [Sort List](https://leetcode.com/problems/sort-list/)

```java

/**

* Definition for singly-linked list.

* public class ListNode {

* int val;

* ListNode next;

* ListNode() {}

* ListNode(int val) { this.val = val; }

* ListNode(int val, ListNode next) { this.val = val; this.next = next; }

* }

*/

class Solution {

public ListNode sortList(ListNode head) {

if(head == null || head.next == null) {

return head;

}

  

ListNode prev = null, slow = head, fast = head;

  

while(fast != null && fast.next != null) {

prev = slow;

slow = slow.next;

fast = fast.next.next;

}

  

prev.next = null;

  

ListNode l1 = sortList(head);

ListNode l2 = sortList(slow);

  

return merge(l1, l2);

  

}

  

private ListNode merge(ListNode l1, ListNode l2) {

  

ListNode l = new ListNode(0) , p = l;

  

while(l1 != null && l2 != null) {

if(l1.val < l2.val) {

p.next = l1;

l1 = l1.next;

} else {

p.next = l2;

l2 = l2.next;

}

p = p.next;

}

  

if(l1 != null)

p.next = l1;

  

if(l2 != null)

p.next = l2;

  

return l.next;

}

}
```

Time Complexity : O(nlogn)
Space Complexity : O(1)

35. [Perfect Squares](https://leetcode.com/problems/perfect-squares/)

```java

class Solution {

public int numSquares(int n) {

if(n == 1) {

return n;

}

  

int[] dp = new int[n + 1];

Arrays.fill(dp, n);

dp[0] = 0;

return helper(n, dp);

}

  

private int helper(int n, int[] dp) {

  

for(int i = 1; i <= n; i++) {

for(int j = 1; j <= i; j++) {

int square = j * j;

if(i - square < 0) {

break;

}

dp[i] = Math.min(dp[i] , 1 + dp[i - square]);

}

}

  

return dp[n];

}

}
```

Time Complexity : O(n * n ^ 1/2)  - n * square root of N
Space Complexity : O(n)

https://www.youtube.com/watch?v=HLZLwjzIVGo

36. [Insert Delete GetRandom O(1)](https://leetcode.com/problems/insert-delete-getrandom-o1/)


```java
class RandomizedSet {

    private Map<Integer, Integer> map;
    private List<Integer> list;
    private Random random;


    public RandomizedSet() {
        list = new ArrayList<>();
        map = new HashMap<>();
        random = new Random();
    }
    
    public boolean insert(int val) {
        if(map.containsKey(val)) {
            return false;
        }

        list.add(val);
        map.put(val, list.size() - 1);
        return true;
    }
    
    public boolean remove(int val) {
        if(!map.containsKey(val)) return false;
        
        int index = map.get(val);
        // Update the list index to the last value
        list.set(index, list.get(list.size() - 1));
        // Remapping of value in the map.
        map.put(list.get(index) , index);

        // Remove the last value from the list
        list.remove(list.size() - 1);
        // Remove the value from the Map
        map.remove(val);

        return true;
    }
    
    public int getRandom() {
       return list.get(random.nextInt(list.size()));
    }
}

/**
 * Your RandomizedSet object will be instantiated and called as such:
 * RandomizedSet obj = new RandomizedSet();
 * boolean param_1 = obj.insert(val);
 * boolean param_2 = obj.remove(val);
 * int param_3 = obj.getRandom();
 */
```

Time Complexity : O(1)
Space Complexity : O(n)

37. [Min Stack](https://leetcode.com/problems/min-stack/)

```java

class MinStack {

  

private Stack<Integer> stack;

private int min = Integer.MAX_VALUE;

  

public MinStack() {

stack = new Stack<>();

}

public void push(int val) {

stack.add(val);

min = Math.min(min , val);

}

public void pop() {

int val = stack.pop();

if(min == val) {

min = Integer.MAX_VALUE;

for(int num : stack) {

min = Math.min(min , num);

}

}

}

public int top() {

return stack.peek();

}

public int getMin() {

return min;

}

}

  

/**

* Your MinStack object will be instantiated and called as such:

* MinStack obj = new MinStack();

* obj.push(val);

* obj.pop();

* int param_3 = obj.top();

* int param_4 = obj.getMin();

*/
```

Time Complexity : O(1)
Space Complexity : O(n)

38. [Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)


```java
class Solution {

public int lengthOfLIS(int[] nums) {

int[] dp = new int[nums.length];

Arrays.fill(dp, -1);

int maxLen = Integer.MIN_VALUE;

  

for(int i = 0; i < nums.length; i++) {

int val = 0;

for(int j = i - 1; j >= 0; j--) {

if(nums[j] < nums[i]) {

val = Math.max(val , dp[j]);

}

}

dp[i] = val + 1;

maxLen = Math.max(dp[i] , maxLen);

}

return maxLen;

}

}

```

Time Complexity = O(n * n)
Space Complexity = O(n)

39. [Count and Say](https://leetcode.com/problems/count-and-say/)

```java

class Solution {

public String countAndSay(int n) {

String result = "1";

  

if(n == 1) {

return result;

}

  

for(int i = 1; i < n; i++) {

result = rle(result);

}

return result;

}

  

private String rle(String str) {

int index = 0;

String rle = "";

while(index < str.length()) {

int count = 1;

int j = index;

while(j < str.length() - 1 && str.charAt(j) == str.charAt(j + 1)) {

count++;

j++;

}

rle += count + "" + str.charAt(index);

index = j + 1;

}

  

return rle;

}

}
```

40. [Sum of Two Integers](https://leetcode.com/problems/sum-of-two-integers/)

Video Link - https://www.youtube.com/watch?v=gVUrDV4tZfY

![[20250102190023.png]]

```java
class Solution {
	public int getSum(int a, int b) {
		if(a == 0) return b;
		if(b == 0) return a;
		
		while(b != 0) {
			int carry = a & b;
			a = a ^ b;
			b = carry << 1;
		}
		return a;
	}
}
```

```js
var getSum = function(a, b) {
	if(a === 0) return b;
	if(b === 0) return a;

	while(b !== 0) {
		let carry = a & b;
		a = a ^ b;
		b = carry << 1;
	}
	return a;
};
```

41. [Search a 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii/)

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        if(matrix == null || matrix.length < 1 || matrix[0].length < 1) {
            return false;
        }
        int col = matrix[0].length - 1;
        int row = 0;
        while(col >= 0 &&  row <= matrix.length - 1) {
            if(target == matrix[row][col]) {
                return true;
            } else if(target < matrix[row][col]) {
                col--;
            } else if(target > matrix[row][col]) {
                row++;
            }
        }
        return false;
    }
}
```

```js
var searchMatrix = function(matrix, target) {
	if(matrix === null || matrix.length < 1 || matrix[0].length < 1) {
		return false;
	}

	let col = matrix[0].length - 1;
	let row = 0;
	while(col >= 0 && row <= matrix.length - 1) {
		if(target == matrix[row][col]) {
			return true;
		} else if(target < matrix[row][col]) {
			col--;
		} else if(target > matrix[row][col]) {
			row++;
		}
	}
	return false;
};
```

Time Complexity = O(m + n)

### References

https://serhatgiydiren.com/step-by-step-tech-interview-preparation-guide/


