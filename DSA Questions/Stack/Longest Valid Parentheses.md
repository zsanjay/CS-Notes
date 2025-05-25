
###### DESCRIPTION (credit [Leetcode.com](https://leetcode.com/problems/longest-valid-parentheses))

Given a string containing just the characters '(' and ')', find the length of the longest valid (well-formed) parentheses substring. A well-formed parentheses string is one that follows these rules:

- Open brackets must be closed by a matching pair in the correct order.
    

For example, given the string "(()", the longest valid parentheses substring is "()", which has a length of 2. Another example is the string ")()())", where the longest valid parentheses substring is "()()", which has a length of 4.

###### EXAMPLES

**Example 1**:  
Inputs:

`s = "())))"`

Output:

2

(Explanation: The longest valid parentheses substring is "()")

**Example 2**:  
Inputs:

`s = "((()()())"`

Output:

8

(Explanation: The longest valid parentheses substring is "(()()())" with a length of 8)

**Example 3**:  
Inputs:

`s = ""`

Output:

`0`

### Solution

## Explanation

At a high level, we can solve this problem by iterating over each index of the string, and then calculating the length of the longest valid parentheses substring that ends at that index. We can then take the maximum of these lengths to get the final answer.

Each time we encounter a closing parenthesis ')', it has the potential to close a valid parentheses substring. In order to calculate the length of the longest valid parentheses substring that ends at a given index, we need to know the index of the last unmatched opening parenthesis '('.

The length of the valid substring ending at the current index can be calculated by taking the difference between the current index and the index of the last unmatched opening parenthesis.

Let's visualize a few examples to understand how that calculation works:

(()201

Current index: 2. Last unmatched opening parenthesis at index 0. Length: 2 - 0 = 2

(()(3012

Current index: 3. Last unmatched opening parenthesis at index 1. Length: 3 - 1 = 2

(()(()301524

Current index: 5. Last unmatched opening parentheses = 1. Length = 5 - 1 = 4.

If there aren't any unmatched opening parentheses remaining after using the current closing parentheses, then we need to know the "start" of the current valid substring. Setting this value to -1 makes our calculation easier, as we can simply take the difference between the current index and the start of the valid substring to get the length.

())(3-1120

Current index: 3. The start of the current valid substring is -1, so length = 3 - (-1) = 4

However, if the closing parenthesis ')' doesn't close any valid substring (as shown below), then we can simply ignore it, and set the start of the current valid substring to the current index.

())) 3012

Current index: 3. But there is no unmatched opening parenthesis. Length: 0

This leads us to the following algorithm:

1. Initialize a stack. The stack will always contain the index of the last unmatched opening parenthesis, or the "start" of the current valid substring. Initially, the stack will contain -1 as the start of the current valid substring.
    
2. Each time we encounter an opening parenthesis '(', we'll push its index onto the stack, which represents the index of the last unmatched opening parenthesis.
    
3. Each time we encounter a closing parenthesis ')', we'll do the following:
    
    1. We first pop the top element from the stack, as this closing parenthesis has the potential to close a valid parentheses substring.
        
    2. Now, after popping, there are two possible cases:
        
        1. The stack is not empty, and the top of the stack represents the index of the last unmatched opening parenthesis. We calculate the length of the valid substring ending at the current index by taking the difference between the current index and the index of the last unmatched opening parenthesis.
            
        2. The stack is empty. This means that this closing parenthesis was unmatched. We'll update the start of the valid substring to the current index by pushing the current index onto the stack.

```java
class Solution {

public int longestValidParentheses(String s) {

Stack<Integer> stack = new Stack<>();

int len = s.length();

if(len == 0) {

return 0;

}

  

stack.push(-1);

int index = 0;

int maxLen = 0;

  

while(index < len) {

char ch = s.charAt(index);

if(ch == '(') {

stack.push(index);

} else {

stack.pop();

if(stack.isEmpty()) {

stack.push(index);

} else {

maxLen = Math.max(maxLen , index - stack.peek());

}

}

index++;

}

  

return maxLen;

}

}
```


Time Complexity : O(n)
Space Complexity : O(n)

