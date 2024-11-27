
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