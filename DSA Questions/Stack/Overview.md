## Stack

## Fundamentals

The stack data structure is a collection of elements that follow the Last-In, First-Out (LIFO) principle, which means that the last element added to the stack is the first one to be removed.

Elements that are added to the stack are said to be "pushed" onto the stack, while elements that are removed are said to be "popped" from the stack. Both of these operations take O(1)time.

We can visualize as a stack as a vertical column of elements, where elements are both pushed and popped from the top of the stack.

A sequence of push() and pop() operations.

### Using an Array as a Stack

Arrays are frequently used to implement stacks, with the end of the array acting as the top of the stack.

In Python, the append and pop array methods can be used to push and pop elements from the stack, respectively:

```python
  
# Using an array as a stack in Python
stack = []
stack.append(9)
stack.append(8)
stack.pop()
stack.append(7)
stack.pop()
```

### Note:

Stack problem can be solved either using index or value.
## Nested Sequences

Stacks are effective for managing the ordering of nested sequences, as the order in which we must process the sequences matches the order in which they are popped from the stack.

### Problem: Valid Parentheses

###### DESCRIPTION (credit [Leetcode.com](https://www.leetcode.com/problems/valid-parentheses/))

Given an input string s consisting solely of the characters '(', ')', ', ', '[' and ']', determine whether s is a valid string. A string is considered valid if every opening bracket is closed by a matching type of bracket and in the correct order, and every closing bracket has a corresponding opening bracket of the same type.

**Example** Input: s = "(){({})}" Output: true

**Example** Input: s = "(){({}})" Output: false


```java
class Solution {

public boolean isValid(String s) {

	Map<Character, Character> map = new HashMap<>();
	map.put(')', '(');
	map.put(']', '[');
	map.put('}', '{');
	
	Stack<Character> stack = new Stack<>();
	
	for(int i = 0; i < s.length(); i++) {
		char ch = s.charAt(i);

	if(!stack.isEmpty() && map.containsKey(ch) && map.get(ch).equals(stack.peek())) {
		stack.pop();
	} else {
			stack.push(ch);
		}
	}
		return stack.isEmpty();
}

}
```

Time Complexity : O(n)
Space Complexity : O(n)

### Explanation

A valid input string with nested parentheses follows the Last-In, First-Out (LIFO) principle: the most recently opened parentheses is the first one that should be closed. For example, if our input string is {[, then the closing ] must come before before the closing }. This property makes it a natural candidate for a stack.

The solution iterates through the each character in the input string.

- If the current character is an opening bracket, it is pushed onto the stack.

- If the current character is a closing bracket, it checks if it is the corresponding closing bracket for the bracket at the top of the stack.

    - If it is, the bracket is popped from the stack, and the iteration continues.
    - If it is not, the input string is invalid.


If the stack is empty at the end of the iteration, then the input string is valid.


### Solution

```python
  
def isValid(s):

  stack = []

  mapping = {")": "(", "}": "{", "]": "["}

  for char in s:

    if char in mapping:

      if not stack or stack[-1] != mapping[char]:

        return False

      stack.pop()

    else:

      stack.append(char)

  return len(stack) == 0
```







