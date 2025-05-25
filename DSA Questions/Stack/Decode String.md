
###### DESCRIPTION (credit [Leetcode.com](https://leetcode.com/problems/decode-string))

Given an encoded string, write a function to return its decoded string that follows a specific encoding rule: k[encoded_string], where the encoded_string within the brackets is repeated exactly k times. Note that k is always a positive integer. The input string is well-formed without any extra spaces, and square brackets are properly matched. Also, assume that the original data doesn't contain digits other than the ones that specify the number of times to repeat the following encoded_string.

###### EXAMPLES

Inputs:

`s = "3[a2[c]]"`

Output:

`"accaccacc"`


### Solution

## Explanation

We start by initializing our stack, and the variables curr_string and current_number. The stack allows us to account for **nested sequences** correctly. curr_string represents the current string we currently decoding, and current_number represents the number of times we need to repeat it when the current decode sequence is completed (i.e. when we encounter a closing "]" bracket).

We then iterate over each character in the encoded string, handling each character as follows:

#### "[": Start Of A New Sequence

When we encounter an opening bracket, we push the current string curr_string and the current number current_number to the stack and reset curr_string and current_number to empty strings. These values that we pushed onto the stack represent the "context" of the current sequence we are decoding. We use current_number to keep track of the number of times we need to repeat the current string we are about to decode, while curr_string represents the value of the string that will be prepended to the result of the current sequence.


#### "]": End Of A Sequence

When we encounter a closing bracket, we pop the last element from the stack and repeat the current string curr_string by the number of times current_number and append it to the string at the top of the stack.

#### Digit

When char is a digit, we update current_number by multiplying it by 10 and adding the value of the digit. current_number is used to keep track of the number of times we need to repeat the current string we are just about to decode.

#### Letter

When we encounter a letter, we append it to the current string curr_string.

```java
class Solution {

public String decodeString(String s) {

  

int length = s.length();

if(length == 1) {

return s;

}

  

Stack stack = new Stack();

String currentStr = "";

int currNum = 0;

  

int index = 0;

while(index < length) {

char ch = s.charAt(index);

if(ch == '[') {

stack.push(currentStr);

stack.push(currNum);

currentStr = "";

currNum = 0;

} else if(ch == ']') {

int num = (int)stack.pop();

String prevStr = (String)stack.pop();

while(num-- > 0) {

prevStr += currentStr;

}

currentStr = prevStr;

} else if(Character.isDigit(ch)) {

currNum = currNum * 10 + Character.getNumericValue(ch);

} else {

currentStr += ch;

}

index++;

}

  

return currentStr;

}

}
```

Time Complexity : O(n)
Space Complexity : O(n)

