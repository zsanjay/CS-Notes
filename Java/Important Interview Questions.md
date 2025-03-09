
Reverse a String using StringBuilder 

```java
class Main {
    public static void main(String[] args) {
        String str = "Hello World!!";
        
        System.out.println(new StringBuilder(str).reverse().toString());
    }
}
```

Reverse a String Recursive Solution

```java
    public static void recursiveSol(String str, int len, StringBuilder builder, int index) {
        
        if(index == len) {
            return;
        }
        
        recursiveSol(str, len, builder, index + 1);
        builder.append(str.charAt(index));
    }
```

Iterative Solution

```java
  public static void iterativeSol(String str, int len, StringBuilder builder) {
        
        for(int i = len - 1; i >= 0; i--) {
            builder.append(str.charAt(i));
        }
        
    }
```

Find Duplicates

```java
import java.util.HashSet;

public class FindDuplicates {
    public static void findDuplicates(int[] arr) {
        HashSet<Integer> set = new HashSet<>();
        
        for (int num : arr) {
            if (!set.add(num)) {
                System.out.println("Duplicate element found: " + num);
            }
        }
    }
    
    public static void main(String[] args) {
        int[] arr = {1, 2, 3, 4, 5, 2, 3, 6};
        findDuplicates(arr);
    }
}
```


```java
import java.util.HashMap;

public class FindDuplicates {
    public static void findDuplicates(int[] arr) {
        HashMap<Integer, Integer> map = new HashMap<>();
        
        for (int num : arr) {
            map.put(num, map.getOrDefault(num, 0) + 1);
        }
        
        for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
            if (entry.getValue() > 1) {
                System.out.println("Duplicate element found: " + entry.getKey());
            }
        }
    }
    
    public static void main(String[] args) {
        int[] arr = {1, 2, 3, 4, 5, 2, 3, 6};
        findDuplicates(arr);
    }
}

```


Find the First non repeating character.

```java
import java.util.HashMap;

public class FirstNonRepeatingChar {
    public static char firstUniqChar(String s) {
        HashMap<Character, Integer> map = new HashMap<>();
        
        // Count frequency of each character
        for (char c : s.toCharArray()) {
            map.put(c, map.getOrDefault(c, 0) + 1);
        }
        
        // Find the first character with frequency 1
        for (char c : s.toCharArray()) {
            if (map.get(c) == 1) {
                return c;
            }
        }
        
        // If no non-repeating character exists
        return '\0'; // Or you can return any placeholder like '-1' or some other indicator.
    }

    public static void main(String[] args) {
        String s = "swiss";
        char result = firstUniqChar(s);
        
        if (result != '\0') {
            System.out.println("First non-repeating character: " + result);
        } else {
            System.out.println("No non-repeating character found.");
        }
    }
}

```

Find the Missing Number

```java
public class MissingNumber {
    public static int findMissingNumber(int[] arr, int n) {
        int xorArr = 0;
        int xorRange = 0;
        
        // XOR all elements in the array
        for (int num : arr) {
            xorArr ^= num;
        }
        
        // XOR all numbers from 1 to n
        for (int i = 1; i <= n; i++) {
            xorRange ^= i;
        }
        
        // The missing number will be the XOR of these two results
        return xorArr ^ xorRange;
    }

    public static void main(String[] args) {
        int[] arr = {1, 2, 4, 5, 6};  // n = 6
        int n = 6;  // The array should have numbers from 1 to 6
        
        int missingNumber = findMissingNumber(arr, n);
        System.out.println("The missing number is: " + missingNumber);
    }
}

```

Swap two numbers without using a third variable.

```java
a = a ^ b;  // Step 1: a becomes XOR of a and b
b = a ^ b;  // Step 2: b becomes original a
a = a ^ b;  // Step 3: a becomes original b
```

 Sort an Array Without Using Built-in Sort (Bubble Sort, QuickSort, MergeSort).

Bubble Sort

```java
public class BubbleSort {
    public static void bubbleSort(int[] arr) {
        int n = arr.length;
        
        // Outer loop for each pass through the array
        for (int i = 0; i < n - 1; i++) {
            // Inner loop for comparing adjacent elements
            for (int j = 0; j < n - 1 - i; j++) {
                if (arr[j] > arr[j + 1]) {
                    // Swap elements if they are in the wrong order
                    int temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                }
            }
        }
    }

    public static void main(String[] args) {
        int[] arr = {64, 34, 25, 12, 22, 11, 90};
        bubbleSort(arr);
        
        System.out.println("Sorted Array:");
        for (int num : arr) {
            System.out.print(num + " ");
        }
    }
}
```

Quick Sort

```java
public class QuickSort {
    public static void quickSort(int[] arr, int low, int high) {
        if (low < high) {
            // Partitioning index
            int pi = partition(arr, low, high);
            
            // Recursively sort the two sub-arrays
            quickSort(arr, low, pi - 1);
            quickSort(arr, pi + 1, high);
        }
    }

    // Partition the array around the pivot
    public static int partition(int[] arr, int low, int high) {
        int pivot = arr[high];  // Choose the last element as the pivot
        int i = low - 1;  // Index of smaller element
        
        for (int j = low; j < high; j++) {
            if (arr[j] < pivot) {
                i++;
                // Swap arr[i] and arr[j]
                int temp = arr[i];
                arr[i] = arr[j];
                arr[j] = temp;
            }
        }
        
        // Swap the pivot with the element at i+1
        int temp = arr[i + 1];
        arr[i + 1] = arr[high];
        arr[high] = temp;
        
        return i + 1;  // Return partition index
    }

    public static void main(String[] args) {
        int[] arr = {10, 7, 8, 9, 1, 5};
        int n = arr.length;
        
        quickSort(arr, 0, n - 1);
        
        System.out.println("Sorted Array:");
        for (int num : arr) {
            System.out.print(num + " ");
        }
    }
}

```

Merge Sort

```java
public class MergeSort {
    public static void mergeSort(int[] arr, int left, int right) {
        if (left < right) {
            int mid = (left + right) / 2;
            
            // Recursively sort the left and right halves
            mergeSort(arr, left, mid);
            mergeSort(arr, mid + 1, right);
            
            // Merge the sorted halves
            merge(arr, left, mid, right);
        }
    }

    public static void merge(int[] arr, int left, int mid, int right) {
        int n1 = mid - left + 1;
        int n2 = right - mid;
        
        // Create temporary arrays
        int[] leftArr = new int[n1];
        int[] rightArr = new int[n2];
        
        // Copy 
        data to temporary arrays
        System.arraycopy(arr, left, leftArr, 0, n1);
        System.arraycopy(arr, mid + 1, rightArr, 0, n2);
        
        // Merge the temporary arrays back into the original array
        int i = 0, j = 0, k = left;
        while (i < n1 && j < n2) {
            if (leftArr[i] <= rightArr[j]) {
                arr[k] = leftArr[i];
                i++;
            } else {
                arr[k] = rightArr[j];
                j++;
            }
            k++;
        }
        
        // Copy remaining elements from leftArr (if any)
        while (i < n1) {
            arr[k] = leftArr[i];
            i++;
            k++;
        }
        
        // Copy remaining elements from rightArr (if any)
        while (j < n2) {
            arr[k] = rightArr[j];
            j++;
            k++;
        }
    }

    public static void main(String[] args) {
        int[] arr = {38, 27, 43, 3, 9, 82, 10};
        int n = arr.length;
        
        mergeSort(arr, 0, n - 1);
        System.out.println("Sorted Array:");
        
        for (int num : arr) {
            System.out.print(num + " ");
        }
    }
}
```

### Comparison:

- **Bubble Sort**: Simple but inefficient for large datasets (O(n2)O(n2)).
- **QuickSort**: Very efficient on average (O(nlog⁡n)O(nlogn)), but can degrade to O(n2)O(n2) in the worst case.
- **MergeSort**: Efficient with a consistent time complexity of O(nlog⁡n)O(nlogn), but requires additional space.

Remove Duplicates using Java Streams

```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class RemoveDuplicates {
    public static void main(String[] args) {
        // Example list with duplicates
        List<Integer> numbers = Arrays.asList(1, 2, 2, 3, 4, 4, 5, 5, 6);

        // Remove duplicates using Streams
        List<Integer> distinctNumbers = numbers.stream()
                                                .distinct()  // Removes duplicates
                                                .collect(Collectors.toList());  // Collects the result into a List

        // Print the result
        System.out.println("List without duplicates: " + distinctNumbers);
    }
}
```

Find the second highest number in an array using Streams

```java
import java.util.Arrays;
import java.util.List;
import java.util.Optional;

public class SecondHighest {
    public static void main(String[] args) {
        // Example array of numbers
        List<Integer> numbers = Arrays.asList(10, 20, 4, 45, 99, 99, 23, 56);

        // Find the second-highest number using Streams
        Optional<Integer> secondHighest = numbers.stream()
                                                  .distinct()          // Remove duplicates
                                                  .sorted((a, b) -> b - a)  // Sort in descending order
                                                  .skip(1)             // Skip the highest value
                                                  .findFirst();        // Get the first element (second-highest)

        // Print the second-highest number
        secondHighest.ifPresentOrElse(
            System.out::println, 
            () -> System.out.println("No second-highest number found")
        );
    }
}
```

Group employees by department using Collectors.groupingBy()

```java
import java.util.*;
import java.util.stream.Collectors;

class Employee {
    private String name;
    private String department;

    // Constructor
    public Employee(String name, String department) {
        this.name = name;
        this.department = department;
    }

    // Getters
    public String getName() {
        return name;
    }

    public String getDepartment() {
        return department;
    }

    @Override
    public String toString() {
        return name;
    }
}

public class GroupByDepartment {
    public static void main(String[] args) {
        // Sample list of employees
        List<Employee> employees = Arrays.asList(
            new Employee("Alice", "HR"),
            new Employee("Bob", "IT"),
            new Employee("Charlie", "HR"),
            new Employee("David", "IT"),
            new Employee("Eve", "Finance"),
            new Employee("Frank", "Finance")
        );

        // Group employees by department using Collectors.groupingBy()
        Map<String, List<Employee>> groupedByDepartment = employees.stream()
            .collect(Collectors.groupingBy(Employee::getDepartment));

        // Print grouped employees by department
        groupedByDepartment.forEach((department, empList) -> {
            System.out.println(department + ": " + empList);
        });
    }
}
```


Find duplicate elements in a list using Collectors.toSet()

```java
import java.util.*;
import java.util.stream.Collectors;

public class FindDuplicates {
    public static void main(String[] args) {
        // Example list with some duplicate elements
        List<Integer> numbers = Arrays.asList(1, 2, 2, 3, 4, 4, 5, 5, 6);

        // Find duplicates by comparing original list and the set
        Set<Integer> uniqueNumbers = numbers.stream()
                                            .collect(Collectors.toSet());

        // Find duplicates by checking the difference in sizes
        List<Integer> duplicates = numbers.stream()
                                          .filter(n -> Collections.frequency(numbers, n) > 1)
                                          .distinct()
                                          .collect(Collectors.toList());

        // Print the duplicate elements
        System.out.println("Duplicate elements: " + duplicates);
    }
}
```


Sort a list of objects based on a field using Comparator.comparing()

```java
import java.util.*;
import java.util.stream.Collectors;

class Person {
    private String name;
    private int age;

    // Constructor
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // Getters
    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    // Override toString() for better printing
    @Override
    public String toString() {
        return name + " (" + age + ")";
    }
}

public class SortByField {
    public static void main(String[] args) {
        // Sample list of Person objects
        List<Person> people = Arrays.asList(
            new Person("Alice", 30),
            new Person("Bob", 25),
            new Person("Charlie", 35),
            new Person("David", 28)
        );

        // Sort the list of people by age using Comparator.comparing()
        List<Person> sortedByAge = people.stream()
                                         .sorted(Comparator.comparing(Person::getAge))  // Sort by age in ascending order
                                         .collect(Collectors.toList());

        // Print the sorted list
        System.out.println("Sorted by age (ascending): " + sortedByAge);
    }
}
```

Sorting in Descending Order:

```java
List<Person> sortedByAgeDesc = people.stream()
                                     .sorted(Comparator.comparing(Person::getAge).reversed())  // Sort by age in descending order
                                     .collect(Collectors.toList());

System.out.println("Sorted by age (descending): " + sortedByAgeDesc);
```

Reverse a Linked List:

```java
class Main {
    public static void main(String[] args) {
        
        int[] arr = {1, 2, 3, 4, 5};
        Main.Node head = new Main.Node(0);
        populateLinkedList(head, arr);
        head = head.next;
        
        printList(head);
        head = reverseNode(head);
        printList(head);
    }
    
    
    public static Main.Node reverseNode(Main.Node head) {
        
        Main.Node prev = null;
        Main.Node current = head;
        Main.Node next = null;
        
        while (current != null) {
            // Save the next node
            next = current.next;

            // Reverse the current node's pointer
            current.next = prev;

            // Move pointers one position ahead
            prev = current;
            current = next;
        }
        
        // prev will be the new head
        head = prev;
        return head;

    }
    
    public static void printList(Main.Node head) {
        
        Main.Node temp = head;
        
        while(temp != null) {
            System.out.print(temp.data + " ");
            temp = temp.next;
        }
        System.out.println();
    }
    
static class Node {
    int data;
    Node next;
    
    public Node(int data) {
        this.data = data;
       // this.next = null;
    }
}

public static void populateLinkedList(Main.Node head , int[] arr) {
    
    Main.Node temp = head;
    for(int a : arr) {
            if(temp == null) {
                temp = new Main.Node(a);
            } else {
                temp.next = new Main.Node(a);
                temp = temp.next;
            }
            
        }
}
}
```

Find a middle node of a LinkedList

```java
import java.util.*;

class Node {
    int value;
    Node next;

    // Constructor
    public Node(int value) {
        this.value = value;
        this.next = null;
    }
}

public class MiddleNode {
    public static void main(String[] args) {
        // Create a LinkedList
        Node head = new Node(1);
        head.next = new Node(2);
        head.next.next = new Node(3);
        head.next.next.next = new Node(4);
        head.next.next.next.next = new Node(5);
        head.next.next.next.next.next = new Node(6);

        // Find and print the middle node
        Node middle = findMiddleNode(head);
        if (middle != null) {
            System.out.println("Middle Node: " + middle.value);
        } else {
            System.out.println("The list is empty.");
        }
    }

    // Method to find the middle node
    public static Node findMiddleNode(Node head) {
        if (head == null) {
            return null; // Empty list, no middle node
        }

        // Initialize two pointers: slow and fast
        Node slow = head;
        Node fast = head;

        // Traverse the list: slow moves 1 step, fast moves 2 steps
        while (fast != null && fast.next != null) {
            slow = slow.next;           // Move slow pointer 1 step
            fast = fast.next.next;      // Move fast pointer 2 steps
        }

        return slow; // slow is now at the middle node
    }
}
```

Detect a Loop in a Linked List (Floyd’s Cycle Detection)

```java
class Node {
    int value;
    Node next;

    public Node(int value) {
        this.value = value;
        this.next = null;
    }
}

public class DetectLoop {
    public static void main(String[] args) {
        // Creating a Linked List with a loop
        Node head = new Node(1);
        head.next = new Node(2);
        head.next.next = new Node(3);
        head.next.next.next = new Node(4);
        head.next.next.next.next = new Node(5);

        // Creating a loop: 5 -> 3
        head.next.next.next.next.next = head.next.next;

        // Detecting loop in the linked list
        if (hasLoop(head)) {
            System.out.println("Loop detected.");
        } else {
            System.out.println("No loop detected.");
        }
    }

    // Function to detect loop using Floyd’s Cycle Detection
    public static boolean hasLoop(Node head) {
        if (head == null) {
            return false; // Empty list, no loop
        }

        Node slow = head;
        Node fast = head;

        // Traverse the list with slow and fast pointers
        while (fast != null && fast.next != null) {
            slow = slow.next; // Move slow pointer by 1 step
            fast = fast.next.next; // Move fast pointer by 2 steps

            // If slow and fast pointers meet, there is a loop
            if (slow == fast) {
                return true; // Loop detected
            }
        }

        // If fast pointer reaches the end of the list, there is no loop
        return false;
    }
}
```

Lowest Common Ancestor in a Binary Tree

```java
import java.util.*;

class Main {
    
    static class TreeNode {
	    int value;
	    TreeNode left;
	    TreeNode right;

	    // Constructor
	    public TreeNode(int value) {
	        this.value = value;
	        this.left = null;
	        this.right = null;
	    }
	}
    
    public static void main(String[] args) {
        // Creating a binary tree:
        //        3
        //       / \
        //      5   1
        //     / \ / \
        //    6  2 0  8
        //      / \
        //     7   4

        Main.TreeNode root = new Main.TreeNode(3);
        root.left = new Main.TreeNode(5);
        root.right = new Main.TreeNode(1);
        root.left.left = new Main.TreeNode(6);
        root.left.right = new Main.TreeNode(2);
        root.right.left = new Main.TreeNode(0);
        root.right.right = new Main.TreeNode(8);
        root.left.right.left = new Main.TreeNode(7);
        root.left.right.right = new Main.TreeNode(4);
        
        printTree(root);

        // Find the LCA of nodes 5 and 1
        Main.TreeNode lca = findLCA(root, root.left, root.right);
        if (lca != null) {
            System.out.println("LCA of 5 and 1: " + lca.value);
        } else {
            System.out.println("LCA not found.");
        }

        // Find the LCA of nodes 5 and 4
        lca = findLCA(root, root.left, root.left.right.right);
        if (lca != null) {
            System.out.println("LCA of 5 and 4: " + lca.value);
        } else {
            System.out.println("LCA not found.");
        }
    }

    // Function to find the Lowest Common Ancestor (LCA)
    public static Main.TreeNode findLCA(Main.TreeNode root, Main.TreeNode p, Main.TreeNode q) {
        // Base case: if root is null or root is one of the nodes (p or q)
        if (root == null || root == p || root == q) {
            return root;
        }
        

        // Recur for left and right subtrees
        Main.TreeNode left = findLCA(root.left, p, q);
        Main.TreeNode right = findLCA(root.right, p, q);

        // If both left and right are non-null, root is the LCA
        if (left != null && right != null) {
            return root;
        }

        // If one of them is null, return the non-null child (LCA)
        return left != null ? left : right;
    }
    
    public static void printTree(Main.TreeNode root) {
        
        Queue<Main.TreeNode> queue = new LinkedList<>();
        queue.add(root);
        
        while(!queue.isEmpty()) {
            
            int size = queue.size();
            
            while(size-- > 0) {
                
                Main.TreeNode treeNode = queue.poll();
                System.out.print(treeNode.value + " ");
                
                if(treeNode.left != null) {
                    queue.add(treeNode.left);
                }
                
                if(treeNode.right != null) {
                    queue.add(treeNode.right);
                }
                
            }
            
            System.out.println();
        }
    }
}
```
#### Level Order Traversal of a binary tree.

```java
public static void printTree(Main.TreeNode root) {
        
        Queue<Main.TreeNode> queue = new LinkedList<>();
        queue.add(root);
        
        while(!queue.isEmpty()) {
            
            int size = queue.size();
            
            while(size-- > 0) {
                
                Main.TreeNode treeNode = queue.poll();
                System.out.print(treeNode.value + " ");
                
                if(treeNode.left != null) {
                    queue.add(treeNode.left);
                }
                
                if(treeNode.right != null) {
                    queue.add(treeNode.right);
                }
                
            }
            
            System.out.println();
        }
}
```

#### Check if a Binary Tree is Balanced

```java
class TreeNode {
    int value;
    TreeNode left;
    TreeNode right;

    // Constructor
    public TreeNode(int value) {
        this.value = value;
        this.left = null;
        this.right = null;
    }
}

public class BalancedBinaryTree {

    public static void main(String[] args) {
        // Example binary tree:
        //        1
        //       / \
        //      2   3
        //     / \
        //    4   5

        TreeNode root = new TreeNode(1);
        root.left = new TreeNode(2);
        root.right = new TreeNode(3);
        root.left.left = new TreeNode(4);
        root.left.right = new TreeNode(5);

        // Check if the binary tree is balanced
        if (isBalanced(root)) {
            System.out.println("The tree is balanced.");
        } else {
            System.out.println("The tree is unbalanced.");
        }
    }

    // Function to check if the tree is balanced
    public static boolean isBalanced(TreeNode root) {
        // Call the helper function to check balance and height
        return checkHeight(root) != -1;
    }

    // Helper function to check the height and balance of the tree
    private static int checkHeight(TreeNode root) {
        // Base case: if the node is null, it's balanced with height -1
        if (root == null) {
            return 0;
        }

        // Recursively check the height of the left subtree
        int leftHeight = checkHeight(root.left);
        if (leftHeight == -1) {
            return -1; // If the left subtree is unbalanced, return -1
        }

        // Recursively check the height of the right subtree
        int rightHeight = checkHeight(root.right);
        if (rightHeight == -1) {
            return -1; // If the right subtree is unbalanced, return -1
        }

        // If the current node is unbalanced, return -1
        if (Math.abs(leftHeight - rightHeight) > 1) {
            return -1;
        }

        // Return the height of the current node's subtree
        return 1 + Math.max(leftHeight, rightHeight);
    }
}
```

Two Sum Problem

```java 
import java.util.HashMap;

public class TwoSum {

    public static void main(String[] args) {
        int[] nums = {2, 7, 11, 15};
        int target = 9;
        
        // Find two numbers whose sum is equal to target
        int[] result = twoSum(nums, target);
        
        if (result != null) {
            System.out.println("Indices of numbers that add up to " + target + ": [" + result[0] + ", " + result[1] + "]");
        } else {
            System.out.println("No solution found.");
        }
    }

    // Function to find two sum indices
    public static int[] twoSum(int[] nums, int target) {
        // Create a HashMap to store the value and its index
        HashMap<Integer, Integer> map = new HashMap<>();
        
        // Iterate through the array
        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];
            
            // Check if the complement exists in the HashMap
            if (map.containsKey(complement)) {
                // Return the indices of the complement and current number
                return new int[] {map.get(complement), i};
            }
            
            // Otherwise, add the current number and its index to the HashMap
            map.put(nums[i], i);
        }
        
        // Return null if no solution found
        return null;
    }
}

```


Find all pairs that sum to target.

```java
import java.util.HashSet;
import java.util.ArrayList;
import java.util.List;

public class AllPairsThatSumToTarget {
    
    public static void main(String[] args) {
        int[] nums = {2, 4, 3, 6, 5, 7, 8};
        int target = 10;
        
        // Find all pairs whose sum equals target
        List<int[]> pairs = findPairs(nums, target);
        
        // Print all pairs
        System.out.println("Pairs that sum to " + target + ":");
        for (int[] pair : pairs) {
            System.out.println("[" + pair[0] + ", " + pair[1] + "]");
        }
    }

    // Function to find all pairs whose sum equals the target
    public static List<int[]> findPairs(int[] nums, int target) {
        // HashSet to store the numbers we have seen so far
        HashSet<Integer> seen = new HashSet<>();
        // List to store the result pairs
        List<int[]> result = new ArrayList<>();
        
        // Traverse the array
        for (int num : nums) {
            int complement = target - num; // The value we're looking for
            
            // If complement exists in the HashSet, it's a valid pair
            if (seen.contains(complement)) {
                result.add(new int[]{complement, num});
            }
            
            // Add the current number to the HashSet for future pairs
            seen.add(num);
        }
        
        return result;
    }
}

```

Find the Longest Substring Without Repeating Characters

```java
import java.util.HashSet;
import java.util.Set;

public class LongestSubstringWithoutRepeatingCharacters {

    public static void main(String[] args) {
        String s = "abcabcbb";
        System.out.println("Length of the longest substring without repeating characters: " + lengthOfLongestSubstring(s));
    }

    // Function to find the length of the longest substring without repeating characters
    public static int lengthOfLongestSubstring(String s) {
        Set<Character> set = new HashSet<>();
        int start = 0;  // The start pointer for the sliding window
        int maxLength = 0;  // Variable to keep track of the maximum length
        
        // Iterate through the string with the 'end' pointer
        for (int end = 0; end < s.length(); end++) {
            // If the character is already in the set, move 'start' to the right
            while (set.contains(s.charAt(end))) {
                set.remove(s.charAt(start));  // Remove the character at 'start'
                start++;  // Move 'start' to the right to shrink the window
            }
            
            // Add the current character to the set
            set.add(s.charAt(end));
            
            // Calculate the length of the current substring and update maxLength if necessary
            maxLength = Math.max(maxLength, end - start + 1);
        }
        
        return maxLength;
    }
}
```

#### Find the Intersection of Two Arrays - Common Unique Elements

```java
import java.util.HashSet;
import java.util.Arrays;

public class IntersectionOfTwoArrays {

    public static void main(String[] args) {
        int[] nums1 = {4, 9, 5};
        int[] nums2 = {9, 4, 9, 8, 4};
        
        int[] intersection = intersection(nums1, nums2);
        
        // Print the intersection
        System.out.println("Intersection of two arrays: " + Arrays.toString(intersection));
    }

    // Function to find the intersection of two arrays
    public static int[] intersection(int[] nums1, int[] nums2) {
        // Step 1: Convert the first array to a HashSet for quick lookups
        HashSet<Integer> set1 = new HashSet<>();
        for (int num : nums1) {
            set1.add(num);
        }

        // Step 2: Traverse the second array and find common elements
        HashSet<Integer> resultSet = new HashSet<>();
        for (int num : nums2) {
            if (set1.contains(num)) {
                resultSet.add(num);  // Add common element to result set
            }
        }

        // Convert the result set to an array and return
        int[] result = new int[resultSet.size()];
        int i = 0;
        for (int num : resultSet) {
            result[i++] = num;
        }
        
        return result;
    }
}
```

#### Check if a String is a Valid Anagram

```java
public class ValidAnagram {

    public static void main(String[] args) {
        String s = "anagram";
        String t = "nagaram";
        
        System.out.println("Is Valid Anagram: " + isAnagram(s, t));
    }

    // Function to check if t is an anagram of s
    public static boolean isAnagram(String s, String t) {
        // If lengths are different, they can't be anagrams
        if (s.length() != t.length()) {
            return false;
        }

        // Initialize a frequency array for 26 lowercase English letters
        int[] count = new int[26];

        // Traverse both strings
        for (int i = 0; i < s.length(); i++) {
            count[s.charAt(i) - 'a']++;  // Increment frequency for characters in s
            count[t.charAt(i) - 'a']--;  // Decrement frequency for characters in t
        }

        // Check if all frequencies are zero
        for (int i = 0; i < 26; i++) {
            if (count[i] != 0) {
                return false;  // If any frequency is not zero, return false
            }
        }

        return true;  // If all frequencies are zero, it's a valid anagram
    }
}

```

LRU Cache

```java
import java.util.LinkedHashMap;
import java.util.Map;

public class LRUCache {
    private final int capacity;
    private final Map<Integer, Integer> cache;

    // Constructor to initialize the cache with a given capacity
    public LRUCache(int capacity) {
        this.capacity = capacity;
        // Initialize the LinkedHashMap with accessOrder=true to maintain access order
        this.cache = new LinkedHashMap<>(capacity, 0.75f, true) {
            // Override the method to remove the eldest entry when capacity is exceeded
            @Override
            protected boolean removeEldestEntry(Map.Entry<Integer, Integer> eldest) {
                return size() > capacity; // Remove eldest entry if cache size exceeds capacity
            }
        };
    }

    // Get the value from the cache
    public int get(int key) {
        return cache.getOrDefault(key, -1);  // Return -1 if the key is not found
    }

    // Put a key-value pair into the cache
    public void put(int key, int value) {
        cache.put(key, value);  // Insert the key-value pair into the cache
    }

    // Method to print the current state of the cache (for debugging purposes)
    public void printCache() {
        System.out.println(cache);
    }

    public static void main(String[] args) {
        // Test the LRUCache
        LRUCache lru = new LRUCache(3);

        // Add key-value pairs to the cache
        lru.put(1, 1);
        lru.put(2, 2);
        lru.put(3, 3);
        lru.printCache(); // Expected: {1=1, 2=2, 3=3}

        // Access key 2 to make it the most recently used
        System.out.println(lru.get(2)); // Expected: 2
        lru.printCache(); // Expected: {1=1, 3=3, 2=2}

        // Add a new key-value pair (this should evict key 1 as it is the least recently used)
        lru.put(4, 4);
        lru.printCache(); // Expected: {3=3, 2=2, 4=4}

        // Access key 3 to make it the most recently used
        System.out.println(lru.get(3)); // Expected: 3
        lru.printCache(); // Expected: {2=2, 4=4, 3=3}

        // Add a new key-value pair (this should evict key 2 as it is the least recently used)
        lru.put(5, 5);
        lru.printCache(); // Expected: {4=4, 3=3, 5=5}
    }
}
```

Find the Kth Largest Element in an Array

```java
import java.util.PriorityQueue;

public class KthLargestElement {

    public static void main(String[] args) {
        int[] nums = {3, 2, 1, 5, 6, 4};
        int k = 2;
        System.out.println("Kth Largest Element: " + findKthLargest(nums, k));  // Expected: 5
    }

    // Function to find the Kth largest element using a Min-Heap
    public static int findKthLargest(int[] nums, int k) {
        // Min-heap to store the largest k elements
        PriorityQueue<Integer> minHeap = new PriorityQueue<>(k);
        
        for (int num : nums) {
            minHeap.offer(num);
            if (minHeap.size() > k) {
                minHeap.poll();  // Remove the smallest element if the size exceeds k
            }
        }

        // The root of the min-heap is the Kth largest element
        return minHeap.peek();
    }
}
```


### SQL Questions

#### Find the Second Highest Salary?

```sql
SELECT MAX(salary) AS SecondHighestSalary
FROM employees
WHERE salary < (SELECT MAX(salary) FROM employees);
```

#### Ensure uniqueness

```sql
SELECT MAX(salary) AS SecondHighestSalary
FROM (SELECT DISTINCT salary FROM employees) AS unique_salaries
WHERE salary < (SELECT MAX(salary) FROM employees);
```

#### Retrieve Employees Who Earn More Than Their Manager

```sql
SELECT e.name AS EmployeeName, e.salary AS EmployeeSalary, m.name AS ManagerName, m.salary AS ManagerSalary
FROM employees e
JOIN employees m ON e.manager_id = m.employee_id
WHERE e.salary > m.salary;
```

#### Find the Department with the Highest Number of Employees

```sql
SELECT department, COUNT(*) AS num_employees
FROM employees
GROUP BY department
ORDER BY num_employees DESC
LIMIT 1;
```

#### Retrieve the Top 3 Salaries from the Employee Table

```sql
SELECT salary
FROM employees
ORDER BY salary DESC
LIMIT 3;
```

#### Write an SQL Query to Get a Running Total (Cumulative Sum)

To calculate a **running total** (or **cumulative sum**) in SQL, you can use the **`SUM()`** window function along with **`OVER()`**. Here's an example assuming you have an `employees` table with columns:

- `employee_id`: The unique identifier for each employee.
- `salary`: The salary of the employee.

```sql
SELECT employee_id, salary,
       SUM(salary) OVER (ORDER BY employee_id) AS running_total
FROM employees;
```

### Explanation:

- `SUM(salary) OVER (ORDER BY employee_id)` calculates the cumulative sum of salaries, ordered by `employee_id`.
    - The `ORDER BY employee_id` ensures that the cumulative sum is calculated in the correct order (typically by some column, such as `employee_id` or `date`).
- The result is the **running total** of salaries up to each employee.

### Sample Output:

If you have data like this:

|employee_id|salary|
|---|---|
|1|5000|
|2|6000|
|3|7000|
|4|8000|

The query will return:

|employee_id|salary|running_total|
|---|---|---|
|1|5000|5000|
|2|6000|11000|
|3|7000|18000|
|4|8000|26000|

### Notes:

- If you want the running total to be partitioned by a specific column (e.g., if employees are grouped by department), you can use the `PARTITION BY` clause within the `OVER()` function:

```sql
SELECT employee_id, salary, SUM(salary) OVER (PARTITION BY department ORDER BY employee_id) AS running_total FROM employees;
```

#### Difference Between LEFT JOIN, RIGHT JOIN, INNER JOIN, FULL JOIN


Here’s a breakdown of the differences between **`LEFT JOIN`**, **`RIGHT JOIN`**, **`INNER JOIN`**, and **`FULL JOIN`** in SQL:

### 1. **INNER JOIN**

- **Definition:** The `INNER JOIN` returns only the rows where there is a match in both tables.
- **Behavior:** It excludes rows from both tables where no match is found.

**Example:**

`SELECT *  FROM table1 INNER JOIN table2 ON table1.id = table2.id;`

**Result:**

- Only rows with matching `id` values in both `table1` and `table2` will be returned.
- **Non-matching rows are excluded.**

---

### 2. **LEFT JOIN** (or **LEFT OUTER JOIN**)

- **Definition:** The `LEFT JOIN` returns all rows from the **left** table, along with matching rows from the **right** table. If no match is found in the right table, `NULL` values are returned for the columns of the right table.
- **Behavior:** It keeps all rows from the **left** table, regardless of whether there is a matching row in the **right** table.

**Example:**

`SELECT *  FROM table1 LEFT JOIN table2 ON table1.id = table2.id;`

**Result:**

- All rows from `table1` are included.
- If there is a match with `table2`, the corresponding values will be returned.
- If there is no match, `NULL` values are shown for `table2` columns.

---

### 3. **RIGHT JOIN** (or **RIGHT OUTER JOIN**)

- **Definition:** The `RIGHT JOIN` is similar to the `LEFT JOIN`, but it returns all rows from the **right** table and the matching rows from the **left** table. If no match is found in the left table, `NULL` values are returned for the columns of the left table.
- **Behavior:** It keeps all rows from the **right** table, regardless of whether there is a matching row in the **left** table.

**Example:**

`SELECT *  FROM table1 RIGHT JOIN table2 ON table1.id = table2.id;`

**Result:**

- All rows from `table2` are included.
- If there is a match with `table1`, the corresponding values will be returned.
- If there is no match, `NULL` values are shown for `table1` columns.

---

### 4. **FULL JOIN** (or **FULL OUTER JOIN**)

- **Definition:** The `FULL JOIN` returns all rows when there is a match in **either** the left or the right table. It combines the behavior of both `LEFT JOIN` and `RIGHT JOIN`. If there is no match in one of the tables, `NULL` values are shown for the columns of that table.
- **Behavior:** It includes all rows from **both** tables, with `NULL` values where there is no match.

**Example:**

`SELECT *  FROM table1 FULL JOIN table2 ON table1.id = table2.id;`

**Result:**

- All rows from both `table1` and `table2` are included.
- If there is a match, the corresponding values from both tables are returned.
- If there is no match, `NULL` values are shown for the non-matching table.

---

### Visual Example:

Let's say we have two tables with the following data:

**Table1:**

|id|name|
|---|---|
|1|John|
|2|Sara|
|3|Tom|

**Table2:**

|id|department|
|---|---|
|2|HR|
|3|Engineering|
|4|Marketing|

#### INNER JOIN:


`SELECT *  FROM table1 INNER JOIN table2 ON table1.id = table2.id;`

**Result:**

|id|name|id|department|
|---|---|---|---|
|2|Sara|2|HR|
|3|Tom|3|Engineering|

#### LEFT JOIN:


`SELECT *  FROM table1 LEFT JOIN table2 ON table1.id = table2.id;`

**Result:**

|id|name|id|department|
|---|---|---|---|
|1|John|NULL|NULL|
|2|Sara|2|HR|
|3|Tom|3|Engineering|

#### RIGHT JOIN:

`SELECT *  FROM table1 RIGHT JOIN table2 ON table1.id = table2.id;`

**Result:**

|id|name|id|department|
|---|---|---|---|
|2|Sara|2|HR|
|3|Tom|3|Engineering|
|NULL|NULL|4|Marketing|

#### FULL JOIN:

`SELECT *  FROM table1 FULL JOIN table2 ON table1.id = table2.id;`

**Result:**

|id|name|id|department|
|---|---|---|---|
|1|John|NULL|NULL|
|2|Sara|2|HR|
|3|Tom|3|Engineering|
|NULL|NULL|4|Marketing|

---

### Summary:

- **`INNER JOIN`**: Only matching rows from both tables.
- **`LEFT JOIN`**: All rows from the left table, matched rows from the right table (or `NULL` if no match).
- **`RIGHT JOIN`**: All rows from the right table, matched rows from the left table (or `NULL` if no match).
- **`FULL JOIN`**: All rows from both tables, with `NULL` for non-matching rows.


#### System Design & Architecture (For L2+ Rounds, but good to know) - Refer to System Design Notes

  
✔ Design a URL Shortener (Like Bit.ly)
✔ Design a Rate Limiter
✔ Design a Messaging Queue System
✔ Design a Microservices-Based E-Commerce Platform
✔ Explain How to Scale a Web Application

### **Optimistic Locking vs Pessimistic Locking:**

Both **optimistic** and **pessimistic** locking are strategies used to manage concurrent access to data in databases and multi-user applications. They aim to prevent issues like **race conditions** (where two users simultaneously modify the same data) and ensure data integrity. The main difference lies in how and when locks are applied to the data.

### **1. Pessimistic Locking:**

Pessimistic locking is a strategy where a lock is **acquired** on the data as soon as it is read, ensuring that no other transaction can modify the data until the lock is released.

#### **Characteristics of Pessimistic Locking:**

- **Locking Early**: When a user reads a piece of data (e.g., a row in a database), the system locks that data immediately to prevent any other users from accessing or modifying it.
- **Blocking Other Transactions**: Other transactions trying to access the same data are blocked until the lock is released.
- **Use Case**: Useful when data is likely to be updated frequently, and you need to ensure no conflicting updates happen.
- **Examples**: SQL's `SELECT FOR UPDATE`, using `LOCK` statements in databases, or using explicit locking mechanisms in application code.

#### **Advantages of Pessimistic Locking:**

- **Data Integrity**: It guarantees that only one user can modify a piece of data at a time, preventing concurrent modification issues.
- **Simplicity**: The logic is simpler since you're ensuring no other process can touch the data while you're working with it.

#### **Disadvantages of Pessimistic Locking:**

- **Performance**: It can lead to performance issues, especially if multiple users are trying to access the same data, causing bottlenecks or deadlocks.
- **Concurrency**: It reduces concurrency since transactions are blocked from accessing the locked data, which might be unnecessary if there is a low likelihood of conflicts.

#### **Example in SQL (Pessimistic Locking)**:

```sql
BEGIN;
SELECT * FROM account WHERE account_id = 123 FOR UPDATE;
-- Do some work on the data
UPDATE account SET balance = balance - 100 WHERE account_id = 123;
COMMIT;
```

Here, the `FOR UPDATE` statement locks the selected row for the duration of the transaction, preventing others from modifying the same row until the transaction is completed.

---

### **2. Optimistic Locking:**

Optimistic locking, on the other hand, allows **concurrent access** to the data and assumes that conflicts will be rare. It only checks for data conflicts at the **end** of the transaction, typically before it commits.

#### **Characteristics of Optimistic Locking:**

- **No Immediate Locking**: Data is not locked when it is read. Multiple users can read the same data at the same time.
- **Conflict Check Before Commit**: Before committing the changes, the system checks if the data has been modified by another transaction during the transaction's lifetime.
- **Use Case**: Ideal when conflicts are rare, and performance is a concern. It’s often used in high-concurrency environments.
- **Example**: Storing a **version number** or **timestamp** in the record, and checking that it hasn’t changed before committing the changes.

#### **Advantages of Optimistic Locking:**

- **Better Concurrency**: Since no locks are acquired at the start, multiple users can read and work on the same data without blocking each other.
- **Performance**: It can offer better performance in systems where conflicts are rare because it avoids locking overhead.
- **Avoids Deadlocks**: Since locks are not held on the data during the transaction, there is no risk of deadlock.

#### **Disadvantages of Optimistic Locking:**

- **Conflict Detection**: If two users try to update the same data simultaneously, one of them will encounter a conflict and have to retry or be notified about the issue.
- **Complexity**: The application logic becomes more complex because you have to handle the conflict resolution and retry mechanisms.

#### **Example of Optimistic Locking:**

Let’s assume we have an `account` table with a `version` column that is used to detect changes:

```sql
-- First, user 1 reads the data
SELECT account_id, balance, version FROM account WHERE account_id = 123;

-- User 1 performs some calculation and then attempts to update
UPDATE account 
SET balance = balance - 100, version = version + 1 
WHERE account_id = 123 AND version = 1;  -- Check the version column

-- If no rows were affected (i.e., the version didn't match), user 1 knows there was a conflict
```

Here, the `version` field ensures that if the data was modified by another transaction in between, the update will fail because the version will not match, and the transaction can be retried.

---

### **Summary Comparison:**

| Feature                    | **Pessimistic Locking**                               | **Optimistic Locking**                                 |
| -------------------------- | ----------------------------------------------------- | ------------------------------------------------------ |
| **When locks are applied** | Immediately when data is read or written              | At the time of commit (based on version check)         |
| **Concurrency**            | Lower, as data is locked and unavailable to others    | Higher, as data is not locked while being read         |
| **Performance**            | Can suffer due to lock contention and blocking        | Better performance in scenarios with rare conflicts    |
| **Conflict Resolution**    | No conflict, as only one process can modify data      | Conflict is checked before commit, may require retries |
| **Risk of Deadlocks**      | Possible, as transactions can block each other        | No deadlocks, as locks are not held during processing  |
| **Use Case**               | High contention environments, critical data integrity | Low contention environments, more relaxed requirements |

---

### **Choosing Between the Two:**

- **Use Pessimistic Locking** when:
    
    - You are dealing with high-risk operations, and it’s critical that no other process can modify the data while you're working on it (e.g., banking systems or inventory systems where updates should not conflict).
    - You expect frequent conflicts or updates to the same data.

- **Use Optimistic Locking** when:
    
    - The probability of conflict is low (e.g., in scenarios where most users are likely working on different records or data).
    - Performance and high concurrency are more important than preventing occasional conflicts (e.g., large-scale web applications).

WebClient vs Rest Template

Here's a comparison of **WebClient** and **RestTemplate**:

### 1. **WebClient (Reactive)**

- **Reactive and Asynchronous**: WebClient is part of Spring WebFlux and supports reactive programming, meaning it can handle asynchronous and non-blocking I/O operations. This is useful for highly scalable applications, particularly those that need to handle a large number of requests concurrently without blocking threads.
- **Modern API**: WebClient is considered the more modern, preferred approach to making HTTP requests in Spring applications, especially in reactive environments.
- **Non-blocking**: It uses reactive streams (Project Reactor) and operates asynchronously, making it suitable for highly concurrent applications.
- **Functional Style**: WebClient uses a fluent API, which allows for a more readable and flexible setup for requests and responses.
- **Error Handling**: WebClient provides better integration for error handling, retries, and advanced handling of responses, like custom status code handling and other reactive-based features.

Example:

```java
WebClient webClient = WebClient.create("http://example.com");
webClient.get()
         .uri("/api/data")
         .retrieve()
         .bodyToMono(String.class)
         .block();  // block() is used to wait for the result in case of non-reactive environment
```

### 2. **RestTemplate (Blocking)**

- **Synchronous and Blocking**: RestTemplate is the traditional, blocking HTTP client used in Spring-based applications. It performs synchronous I/O, meaning each HTTP request will block the thread until the response is received.
- **Legacy and Deprecated**: RestTemplate is still widely used in many Spring applications, but it is considered legacy now. It will continue to be supported, but Spring recommends using WebClient for new projects.
- **Simpler API**: It has a simpler, imperative API compared to WebClient, which can be easier for developers who are accustomed to traditional synchronous HTTP requests.
- **Less Flexibility**: While RestTemplate is powerful, it doesn't provide the same level of flexibility and modern features as WebClient, especially when working with complex error handling or non-blocking, high-concurrency scenarios.

Example:

java

```java
RestTemplate restTemplate = new RestTemplate();
String result = restTemplate.getForObject("http://example.com/api/data", String.class);
```

### Key Differences:

| Feature            | **WebClient**                                          | **RestTemplate**                                    |
| ------------------ | ------------------------------------------------------ | --------------------------------------------------- |
| **Reactive**       | Yes (non-blocking, reactive streams)                   | No (synchronous, blocking)                          |
| **Preferred for**  | Reactive programming and high concurrency applications | Traditional, blocking applications                  |
| **API Style**      | Fluent, functional style                               | Imperative, more straightforward                    |
| **Error Handling** | Advanced error handling and retry mechanisms           | Basic error handling                                |
| **Performance**    | Better performance for highly concurrent workloads     | Suitable for lower concurrency workloads            |
| **Deprecation**    | Future-facing and actively supported in Spring         | Deprecated in favor of WebClient                    |
| **Complexity**     | Slightly more complex due to its non-blocking nature   | Simpler and easier to use for traditional workflows |

### When to Use Which:

- **WebClient** is recommended for **new projects** or applications that need to handle a lot of concurrent HTTP requests, especially in a **reactive** environment or those requiring high scalability.
- **RestTemplate** is useful for **existing** projects where a **synchronous, blocking** HTTP client is sufficient or when migrating from older versions of Spring.

In summary, **WebClient** is the preferred choice for modern Spring applications, especially for those utilizing Spring WebFlux or requiring non-blocking behavior. **RestTemplate** is suitable for legacy applications but is being phased out in favor of **WebClient**.

### Future vs CompletableFuture

### **1. Future**

The `Future` interface represents the result of an asynchronous computation, but it provides very basic functionality for interacting with that computation.

#### **Key Features of `Future`:**

- **Blocking Operations**: The most common way to interact with a `Future` is by calling its `get()` method, which blocks until the result is available.
    - Example: `T get() throws InterruptedException, ExecutionException;`
- **No Direct Support for Combining Tasks**: `Future` does not provide built-in methods to combine or chain multiple asynchronous tasks.
- **Task Completion**: Once a task has been completed (successfully or not), it cannot be modified or updated. `Future` does not support the ability to complete or manipulate a task after it has been submitted.
- **Cancellation**: You can cancel a task using the `cancel()` method, but it is only an attempt to cancel and may not always succeed.

#### **Methods in `Future`:**

- **`get()`**: Blocks the current thread and waits for the computation to complete.
- **`cancel()`**: Attempts to cancel the task. It returns `true` if successful, `false` otherwise.
- **`isDone()`**: Returns `true` if the task is completed (either normally, through cancellation, or by throwing an exception).
- **`isCancelled()`**: Returns `true` if the task was cancelled before it was completed.

#### **Example using `Future`:**

```java
ExecutorService executor = Executors.newSingleThreadExecutor();
Future<Integer> future = executor.submit(() -> {
    Thread.sleep(1000); // Simulate a task
    return 42;
});

try {
    Integer result = future.get();  // Blocks until the result is available
    System.out.println("Result: " + result);
} catch (InterruptedException | ExecutionException e) {
    e.printStackTrace();
} finally {
    executor.shutdown();
}
```

---

### **2. CompletableFuture**

`CompletableFuture` is an extension of `Future` that provides more advanced capabilities, such as non-blocking methods, the ability to chain multiple asynchronous tasks, and handling the result or exceptions asynchronously.

#### **Key Features of `CompletableFuture`:**

- **Non-blocking**: Unlike `Future`, `CompletableFuture` provides methods like `thenApply()`, `thenAccept()`, and `thenCompose()` that allow chaining tasks without blocking the thread.
- **Completion**: `CompletableFuture` allows tasks to be completed manually using `complete()` or `completeExceptionally()`, making it more flexible for controlling the completion of tasks.
- **Combining Multiple Futures**: You can combine multiple `CompletableFuture` objects using methods like `thenCombine()`, `allOf()`, and `anyOf()`, allowing complex workflows to be built.
- **Async Callbacks**: You can register callbacks that are executed when the computation completes, providing a more declarative way to handle asynchronous results.
- **Exception Handling**: `CompletableFuture` provides better exception handling through `handle()`, `exceptionally()`, and `whenComplete()`.
- **Cancellation**: Like `Future`, you can also cancel a task with `cancel()`, but `CompletableFuture` provides more flexibility in handling cancellations.

#### **Methods in `CompletableFuture`:**

- **`thenApply()`**: Allows you to apply a function to the result once the computation is complete (non-blocking).
- **`thenAccept()`**: Used for consuming the result of a task without modifying it.
- **`thenCompose()`**: Enables you to chain further asynchronous operations, where the next operation depends on the result of the previous one.
- **`handle()`**: Allows you to handle both the result and any exception in a single callback.
- **`exceptionally()`**: Allows you to handle exceptions in a non-blocking way.
- **`complete()`**: Allows you to manually complete a `CompletableFuture` by providing the result.
- **`allOf()`**: Waits for the completion of all provided `CompletableFuture` instances.
- **`anyOf()`**: Waits for the first `CompletableFuture` to complete.

#### **Example using `CompletableFuture`:**

```java
ExecutorService executor = Executors.newSingleThreadExecutor();
CompletableFuture<Integer> future = CompletableFuture.supplyAsync(() -> {
    try {
        Thread.sleep(1000); // Simulate a task
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
    return 42;
}, executor);

future.thenApply(result -> {
    System.out.println("Task completed with result: " + result);
    return result * 2;  // Apply some transformation
}).thenAccept(finalResult -> {
    System.out.println("Transformed result: " + finalResult);
});

executor.shutdown();
```

### **Key Differences:**

| Feature                 | **`Future`**                                         | **`CompletableFuture`**                                                          |
| ----------------------- | ---------------------------------------------------- | -------------------------------------------------------------------------------- |
| **Blocking**            | Requires `get()` to block until result is available  | Supports non-blocking methods like `thenApply()`, `thenAccept()`                 |
| **Task Completion**     | Can’t manually complete a task                       | You can manually complete a task using `complete()` or `completeExceptionally()` |
| **Chaining**            | Doesn’t support chaining of tasks                    | Supports chaining with `thenApply()`, `thenCompose()`, etc.                      |
| **Exception Handling**  | No built-in mechanism for handling exceptions        | Supports exception handling with `exceptionally()`, `handle()`                   |
| **Concurrency**         | Limited concurrency features                         | Supports combining multiple futures using `allOf()`, `anyOf()`, etc.             |
| **Use Case**            | Basic asynchronous tasks that block until completion | Complex asynchronous workflows, combining multiple tasks, handling errors.       |
| **Cancellation**        | Can cancel task using `cancel()`                     | Can cancel task with `cancel()` and handle completion manually.                  |
| **Completion Callback** | Not supported                                        | Allows setting callbacks like `thenAccept()` on completion                       |

---

### **When to Use `Future` vs `CompletableFuture`**

- **Use `Future`**:
    
    - For simple, blocking tasks where you don’t need chaining, exception handling, or complex workflows.
    - When you don’t need to combine multiple asynchronous tasks.
    - For simpler use cases where performance overhead due to additional features of `CompletableFuture` is unnecessary.

- **Use `CompletableFuture`**:
    
    - For non-blocking operations where you need to chain multiple tasks or compose multiple futures.
    - When you need to manually complete tasks or handle exceptions asynchronously.
    - For complex asynchronous workflows, where you might need callbacks, transformations, or combining multiple futures.

---
### **Conclusion:**

While `Future` is good for basic asynchronous tasks that block until a result is available, `CompletableFuture` provides a much richer API for handling complex asynchronous workflows. With its ability to chain operations, handle exceptions, and manually complete tasks, `CompletableFuture` is generally preferred for more advanced use cases involving concurrency.

### 1. **`Optional.ofNullable()`**

- **Purpose**: This method is used to create an `Optional` instance that may or may not contain a value. It allows for a `null` value to be passed as a parameter and handles it by creating an empty `Optional` if the value is `null`.
- **Behavior**: If the value is `null`, it returns an empty `Optional`. If the value is non-null, it returns an `Optional` containing the value.

**Example:**

```java
String str = null;
Optional<String> optional = Optional.ofNullable(str);
System.out.println(optional);  // Output: Optional.empty
```

```java
String str = "Hello, World!";
Optional<String> optional = Optional.ofNullable(str);
System.out.println(optional);  // Output: Optional[Hello, World!]
```

- **Key Point**: `Optional.ofNullable()` is more flexible, as it can handle `null` inputs without throwing an exception.

### 2. **`Optional.of()`**

- **Purpose**: This method is used to create an `Optional` instance that must contain a non-null value. It **throws an exception** (`NullPointerException`) if the value passed is `null`.
- **Behavior**: If the value is `null`, it throws a `NullPointerException`. If the value is non-null, it returns an `Optional` containing the value.

**Example:**

```java
String str = null;
Optional<String> optional = Optional.of(str);  // Throws NullPointerException
```

```java
String str = "Hello, World!";
Optional<String> optional = Optional.of(str);
System.out.println(optional);  // Output: Optional[Hello, World!]
```

- **Key Point**: `Optional.of()` is stricter because it **does not allow** `null` values. It's typically used when you are sure the value is non-null.

### Key Differences:

|**Method**|**Description**|**Null Handling**|**When to Use**|
|---|---|---|---|
|`Optional.ofNullable()`|Creates an `Optional` that can handle `null`|Allows `null`, returns `Optional.empty()` for `null`|Use when the value may or may not be `null`|
|`Optional.of()`|Creates an `Optional` that must contain a non-null value|Throws `NullPointerException` if the value is `null`|Use when you are sure the value will never be `null`|

### Best Practices:

- **Use `Optional.ofNullable()`** when you're not sure whether the value can be `null`. It handles the `null` case gracefully and prevents `NullPointerException`.
- **Use `Optional.of()`** when you are confident the value cannot be `null` (e.g., in code paths where the value is always initialized).

In summary:

- `Optional.of()` is for situations where the value is guaranteed to be non-null.
- `Optional.ofNullable()` is for situations where the value might be `null`, and you want to safely handle that.

#### Purpose of `@NoRepositoryBean`:

By default, when you create an interface extending a repository interface (e.g., `JpaRepository`, `CrudRepository`), Spring Data JPA will automatically treat it as a repository bean and will create a proxy for it. However, in some cases, you may want to define a repository interface that is not directly used for creating a bean but serves as a **base interface** for other repository interfaces to inherit from.

This is where `@NoRepositoryBean` comes in. It marks a repository interface to be **abstract** and **not** automatically instantiated by Spring. This is useful when creating **custom repository functionality** that is meant to be shared among multiple repositories.

### How to Use `@NoRepositoryBean`:

1. **Define a custom base repository interface** (or an abstract one) with `@NoRepositoryBean`, and add custom methods or functionality to it.
2. Other repository interfaces can extend this custom base interface.

### Example:

#### Step 1: Create a Custom Base Repository (with `@NoRepositoryBean`)

```java
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.repository.NoRepositoryBean;
import java.util.List;

@NoRepositoryBean
public interface CustomRepository<T, ID> extends JpaRepository<T, ID> {
    // Custom query methods or utility methods can go here
    List<T> findBySomeCustomCriteria(String criteria);
}
```

In the example above:

- The `CustomRepository` interface extends `JpaRepository`, but it is marked with `@NoRepositoryBean`.
- This prevents Spring from automatically creating an implementation of `CustomRepository` as a bean.
- The `findBySomeCustomCriteria()` method can be used by child repositories that extend this interface.

#### Step 2: Create a Concrete Repository Interface that Inherits from the Custom Repository

```java
import org.springframework.stereotype.Repository;

@Repository
public interface MyEntityRepository extends CustomRepository<MyEntity, Long> {
    // You can define additional custom methods specific to this repository
}
```

In this case:

- `MyEntityRepository` extends `CustomRepository` to inherit the custom functionality.
- Spring Data JPA will create a proxy and handle the implementation of both the methods in `CustomRepository` and `MyEntityRepository`.

#### Why Use `@NoRepositoryBean`?

- **Reusability**: If you want to reuse common functionality or query methods in multiple repositories, you can define them in a base interface and prevent Spring from creating a bean for that base interface.
- **Avoid Unwanted Instantiation**: You might define abstract repository interfaces with shared behavior, but you don't want Spring to treat them as individual beans in the Spring context.

### Example Without `@NoRepositoryBean` (What Happens if You Forget It):

If you don't use `@NoRepositoryBean`, Spring will try to create a bean for `CustomRepository`, which is not your intention because it's meant to be a base interface:

```java
public interface CustomRepository<T, ID> extends JpaRepository<T, ID> {
    List<T> findBySomeCustomCriteria(String criteria);
}
```

Here, Spring would automatically create a bean for `CustomRepository`, which you don't want, because it's meant to be inherited by concrete repositories.

### Summary:

- **`@NoRepositoryBean`** prevents Spring Data JPA from creating an implementation for the interface.
- It's used for **abstract or base repository interfaces** that should provide shared functionality but should not be instantiated on their own.
- It’s typically used when creating **custom repositories** that extend `JpaRepository` or similar repositories, and you don't want them to be treated as a Spring bean.

This approach is useful for structuring your repository layer in a clean and reusable way.



### @AutoConfigureMockMvc`

`@AutoConfigureMockMvc` is an annotation in Spring Boot that is used to **auto-configure `MockMvc`** in the test context. It is particularly useful when writing **integration tests** for Spring MVC applications. The annotation helps you to test web layers (controllers) without needing to start an actual HTTP server. `MockMvc` allows you to simulate HTTP requests and test your controllers in a Spring context, which is ideal for unit and integration testing.

### What is `MockMvc`?

`MockMvc` is a Spring testing utility that allows you to perform HTTP requests (e.g., `GET`, `POST`, etc.) and assert their responses (e.g., status, body, headers) without actually starting a web server. This makes it very useful for testing controllers in Spring applications in a fast and isolated manner.

### Purpose of `@AutoConfigureMockMvc`

When you're testing a Spring Boot application, you may want to test the behavior of the web layer, especially your controllers, without starting up the whole web server. `@AutoConfigureMockMvc` automatically configures a `MockMvc` bean for use in your test classes, which simplifies testing of your controllers and web layers.

### How to Use `@AutoConfigureMockMvc`?

1. **Include the necessary dependencies**:
    
    - Make sure to include the `spring-boot-starter-test` dependency in your `pom.xml` (for Maven) or `build.gradle` (for Gradle). This starter includes the necessary libraries for testing, including `MockMvc`.
    
    **For Maven**:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```


2. **Write the Test Class with `@AutoConfigureMockMvc`**:
    
    - You can use `@AutoConfigureMockMvc` in your test class along with `@SpringBootTest` or `@WebMvcTest` (depending on the type of test).
    - `@SpringBootTest` is typically used for integration tests where the whole application context is loaded.
    - `@WebMvcTest` is used for testing only the web layer (controllers) and doesn't load the full application context.

### Example 1: Integration Test Using `@AutoConfigureMockMvc` and `@SpringBootTest`


```java
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.result.MockMvcResultMatchers;

@SpringBootTest
@AutoConfigureMockMvc
public class MyControllerIntegrationTest {

    @Autowired
    private MockMvc mockMvc; // Automatically injected by @AutoConfigureMockMvc

    @Test
    public void testGetEndpoint() throws Exception {
        // Perform a GET request and assert that the response status is OK (200)
        mockMvc.perform(get("/api/my-endpoint"))
               .andExpect(status().isOk())
               .andExpect(jsonPath("$.message").value("Hello World"));
    }
}
```


In the example above:

- `@SpringBootTest`: Starts the full application context.
- `@AutoConfigureMockMvc`: Automatically configures `MockMvc` so you can inject it into the test class.
- The test sends a `GET` request to `/api/my-endpoint` and verifies that the response status is `200 OK` and that the `message` field in the response body is `"Hello World"`.

### Example 2: Controller Unit Test with `@AutoConfigureMockMvc` and `@WebMvcTest`


```java
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.result.MockMvcResultMatchers;

@WebMvcTest(MyController.class) // Only load the web layer (controller)
@AutoConfigureMockMvc
public class MyControllerTest {

    @Autowired
    private MockMvc mockMvc; // Automatically injected by @AutoConfigureMockMvc

    @Test
    public void testGetEndpoint() throws Exception {
        mockMvc.perform(get("/api/my-endpoint"))
               .andExpect(status().isOk())
               .andExpect(content().string("Hello World"));
    }
}
```

In this case:

- `@WebMvcTest(MyController.class)`: This is used to load only the web layer for `MyController`, so it's ideal for testing just the controller's behavior.
- `@AutoConfigureMockMvc`: Configures the `MockMvc` instance for testing.

### Key Points:

- **`@AutoConfigureMockMvc`** is used to automatically configure `MockMvc` in Spring Boot tests.
- It allows you to test controllers without needing to start the full web server.
- You can use `@AutoConfigureMockMvc` with either `@SpringBootTest` (for full integration tests) or `@WebMvcTest` (for controller-specific tests).
- You can perform HTTP requests and validate the responses using assertions like `status().isOk()`, `jsonPath()`, etc.

### Common Use Cases for `@AutoConfigureMockMvc`:

- **Unit tests for controllers**: When testing controllers independently without starting the whole application.
- **Integration tests**: When you want to test the web layer with the full application context but without starting an HTTP server.
- **Validation of HTTP requests and responses**: You can use `MockMvc` to simulate user interactions (GET, POST, etc.) and verify expected results.

In summary, `@AutoConfigureMockMvc` makes it easier to write tests for the web layer by automatically configuring `MockMvc` for your test class, allowing you to simulate HTTP requests and validate responses in a Spring Boot application.


```java
package com.sivalabs.bookmarker.api;

import com.sivalabs.bookmarker.domain.Bookmark;
import com.sivalabs.bookmarker.domain.BookmarkRepository;
import org.hamcrest.CoreMatchers;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.CsvSource;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.http.MediaType;
import org.springframework.test.context.TestPropertySource;
import org.springframework.test.web.servlet.MockMvc;

import java.time.Instant;
import java.util.ArrayList;
import java.util.List;

import static org.hamcrest.Matchers.hasSize;
import static org.hamcrest.Matchers.is;
import static org.hamcrest.Matchers.notNullValue;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.post;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.header;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.jsonPath;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
@AutoConfigureMockMvc
@TestPropertySource(properties = {
        "spring.datasource.url=jdbc:tc:postgresql:17-alpine:///demo"
})
class BookmarkControllerTest {
    @Autowired
    private MockMvc mvc;

    @Autowired
    BookmarkRepository bookmarkRepository;

    private List<Bookmark> bookmarks;

    @BeforeEach
    void setUp() {
        bookmarkRepository.deleteAllInBatch();
        bookmarks = new ArrayList<>();

        bookmarks.add(new Bookmark(null, "SivaLabs", "https://sivalabs.in", Instant.now()));
        bookmarks.add(new Bookmark(null, "SpringBlog", "https://spring.io/blog", Instant.now()));
        bookmarks.add(new Bookmark(null, "Quarkus", "https://quarkus.io/", Instant.now()));
        bookmarks.add(new Bookmark(null, "Micronaut", "https://micronaut.io/", Instant.now()));
        bookmarks.add(new Bookmark(null, "JOOQ", "https://www.jooq.org/", Instant.now()));
        bookmarks.add(new Bookmark(null, "VladMihalcea", "https://vladmihalcea.com", Instant.now()));
        bookmarks.add(new Bookmark(null, "Thoughts On Java", "https://thorben-janssen.com/", Instant.now()));
        bookmarks.add(new Bookmark(null, "DZone", "https://dzone.com/", Instant.now()));
        bookmarks.add(new Bookmark(null, "DevOpsBookmarks", "http://www.devopsbookmarks.com/", Instant.now()));
        bookmarks.add(new Bookmark(null, "Kubernetes docs", "https://kubernetes.io/docs/home/", Instant.now()));
        bookmarks.add(new Bookmark(null, "Expressjs", "https://expressjs.com/", Instant.now()));
        bookmarks.add(new Bookmark(null, "Marcobehler", "https://www.marcobehler.com", Instant.now()));
        bookmarks.add(new Bookmark(null, "baeldung", "https://www.baeldung.com", Instant.now()));
        bookmarks.add(new Bookmark(null, "devglan", "https://www.devglan.com", Instant.now()));
        bookmarks.add(new Bookmark(null, "linuxize", "https://linuxize.com", Instant.now()));

        bookmarkRepository.saveAll(bookmarks);
    }

    @ParameterizedTest
    @CsvSource({
            "1,15,2,1,true,false,true,false",
            "2,15,2,2,false,true,false,true"
    })
    void shouldGetBookmarks(int pageNo, int totalElements, int totalPages, int currentPage,
                            boolean isFirst, boolean isLast,
                            boolean hasNext, boolean hasPrevious) throws Exception {
        mvc.perform(get("/api/bookmarks?page="+pageNo))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.totalElements", CoreMatchers.equalTo(totalElements)))
                .andExpect(jsonPath("$.totalPages", CoreMatchers.equalTo(totalPages)))
                .andExpect(jsonPath("$.currentPage", CoreMatchers.equalTo(currentPage)))
                .andExpect(jsonPath("$.isFirst", CoreMatchers.equalTo(isFirst)))
                .andExpect(jsonPath("$.isLast", CoreMatchers.equalTo(isLast)))
                .andExpect(jsonPath("$.hasNext", CoreMatchers.equalTo(hasNext)))
                .andExpect(jsonPath("$.hasPrevious", CoreMatchers.equalTo(hasPrevious)))
        ;
    }

    @Test
    void shouldCreateBookmarkSuccessfully() throws Exception {
        this.mvc.perform(
            post("/api/bookmarks")
                    .contentType(MediaType.APPLICATION_JSON)
                    .content("""
            {
                "title": "SivaLabs Blog",
                "url": "https://sivalabs.in"
            }
            """)
        )
        .andExpect(status().isCreated())
        .andExpect(jsonPath("$.id", notNullValue()))
        .andExpect(jsonPath("$.title", is("SivaLabs Blog")))
        .andExpect(jsonPath("$.url", is("https://sivalabs.in")));
    }

    @Test
    void shouldFailToCreateBookmarkWhenUrlIsNotPresent() throws Exception {
        this.mvc.perform(
                post("/api/bookmarks")
                        .contentType(MediaType.APPLICATION_JSON)
                        .content("""
                {
                    "title": "SivaLabs Blog"
                }
                """)
            )
            .andExpect(status().isBadRequest())
            .andExpect(header().string("Content-Type", is("application/problem+json")))
            .andExpect(jsonPath("$.type", is("https://zalando.github.io/problem/constraint-violation")))
            .andExpect(jsonPath("$.title", is("Constraint Violation")))
            .andExpect(jsonPath("$.status", is(400)))
            .andExpect(jsonPath("$.violations", hasSize(1)))
            .andExpect(jsonPath("$.violations[0].field", is("url")))
            .andExpect(jsonPath("$.violations[0].message", is("Url should not be empty")))
            .andReturn();
    }
}
```

### Consistency in CAP vs ACID


The term **"Consistency"** appears in both the **ACID** properties (used in relational databases) and the **CAP theorem** (used in distributed systems), but it means different things in each context.

### **1. Consistency in ACID (Relational Databases)**

In the context of **ACID** (which stands for **Atomicity, Consistency, Isolation, and Durability**), **Consistency** refers to the idea that a transaction will bring the database from one valid state to another valid state.

#### **Explanation**:

- **Consistency** ensures that any transaction executed on the database will leave it in a valid state.
- This means the database must always adhere to predefined rules, such as constraints, triggers, and business logic. These rules guarantee that data remains accurate and correct after each transaction.
- It doesn't necessarily mean that the database is fully consistent across all systems; rather, it's **locally** consistent with respect to the transaction it is processing.

#### Example:

Imagine you have a bank database with the following rule: "No account balance can go negative." If a transaction attempts to withdraw more money than the account has, the database should reject the transaction and not allow the balance to go negative.

In summary, **ACID Consistency** focuses on maintaining the integrity and correctness of data within a single database system.

---

### **2. Consistency in CAP (Distributed Systems)**

In the context of the **CAP Theorem** (which stands for **Consistency, Availability, and Partition Tolerance**), **Consistency**has a different meaning. It refers to the idea that **every read** from the system will return the **most recent write** (or an error).

#### **Explanation**:

- **Consistency** in CAP means that after a write operation is performed, all nodes in the distributed system will have the **same** data at any given time. This is often described as **linearizability** or **strong consistency**.
- If the system is consistent, every node must reflect the same data after a successful write, ensuring that no conflicting data is returned to clients.

#### Example:

Consider a distributed database where you have multiple replicas of data. If one node accepts a write (e.g., updating a user's profile), **all other nodes** in the system should reflect that update before any subsequent reads can occur. If a user reads from any of the nodes, they should see the most recent data, ensuring consistency across the system.

In distributed systems, **Consistency** is about making sure that all copies of the data across distributed nodes are in sync, i.e., they all see the same version of the data after any write operation.

---

### **Comparison of Consistency in ACID vs CAP**

|**Aspect**|**Consistency in ACID**|**Consistency in CAP**|
|---|---|---|
|**Scope**|Refers to the state of data in a single, centralized database.|Refers to consistency across multiple nodes in a distributed system.|
|**Definition**|Ensures that transactions bring the database from one valid state to another, adhering to rules (e.g., constraints, data integrity).|Ensures that all replicas of the data in the distributed system are in sync and return the same value for reads.|
|**Focus**|Data integrity and correctness for individual transactions.|Data synchronization and coordination across distributed nodes.|
|**Examples**|Ensuring no invalid data (e.g., no negative bank balances).|Ensuring all nodes reflect the same data after a write operation in a distributed system.|
|**Behavior on Failure**|The transaction is aborted if consistency cannot be maintained.|Reads may return old or inconsistent data during network partitioning, depending on the system's trade-off (e.g., between consistency and availability).|

---

### **How ACID Consistency Relates to CAP Consistency**

- **ACID Consistency** is **local** to a single database system, ensuring that individual transactions do not violate database integrity rules (like constraints).
    
- **CAP Consistency** is about how **multiple nodes in a distributed system** behave in the face of failures, network partitions, and data synchronization. A distributed system may trade off consistency in certain situations to prioritize availability or tolerance to partitioning (the "P" in CAP).
    

For example, in **AP (Availability and Partition Tolerance)** systems (as per CAP), the system may allow reads from a replica that is not up-to-date, which leads to **eventual consistency** instead of strong consistency. This is a trade-off where **the system prefers availability over consistency** in situations where there are network issues or partitions between nodes.

---

### **To Summarize:**

- **ACID Consistency** ensures that a database remains in a valid state and follows all integrity rules during and after a transaction.
- **CAP Consistency** ensures that all nodes in a distributed system are in sync and return the most recent data after a write operation, but might be sacrificed in case of network partitioning or high availability needs.

### Example in Practice:

- **ACID Consistency**: When you transfer money from one account to another, a bank's relational database will ensure that the transaction is either fully completed or rolled back to avoid inconsistencies (e.g., one account gets debited but the other doesn't get credited).
    
- **CAP Consistency**: In a distributed database like **Cassandra**, if you write data to one node but another node is temporarily unavailable (due to partitioning), the system might allow the data to be inconsistent between nodes for a while but guarantees eventual consistency once the partition is resolved.
    