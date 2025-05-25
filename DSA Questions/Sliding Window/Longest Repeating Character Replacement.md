
###### DESCRIPTION (credit [Leetcode.com](https://leetcode.com/problems/longest-repeating-character-replacement))

Write a function to find the length of the longest substring containing the same letter in a given string s, after performing at most k operations in which you can choose any character of the string and change it to any other uppercase English letter.

###### EXAMPLES

Input:

`s = "BBABCCDD" k = 2`

Output:

`5`

### Solution

Explanation: Replace the first 'A' and 'C' with 'B' to form "BBBBBCCDD". The longest substring with identical letters is "BBBBB", which has a length of 5.

```python

class Solution:

def characterReplacement(self, s: str, k: int):

	start = 0
	state = {}
	maxLen = 0
	maxFreq = 0
	
	for end in range(len(s)):
		state[s[end]] = state.get(s[end] , 0) + 1
		maxFreq = max(maxFreq , state[s[end]])
		
		if k + maxFreq < end - start + 1:
			state[s[start]] -= 1
			start += 1
			
		maxLen = max(maxLen , end - start + 1)
		
	return maxLen
```

