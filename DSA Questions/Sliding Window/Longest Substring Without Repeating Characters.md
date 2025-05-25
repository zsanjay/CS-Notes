

###### DESCRIPTION (credit [Leetcode.com](https://leetcode.com/problems/longest-substring-without-repeating-characters/))

Write a function to return the length of the longest substring in a provided string s where all characters in the substring are distinct.

###### EXAMPLES

**Example 1**: Input:

`s = "eghghhgg"`

Output:

`3`

The longest substring without repeating characters is "egh" with length of 3.

**Example 2**: Input:

`s = "substring"`

Output:

`8`

The answer is "ubstring" with length of 8.

### Solution

```python
class Solution:

def longestSubstringWithoutRepeat(self, s: str):
	start = 0
	myset = set()
	maxLen = 0

	for end in range(len(s)):
		while s[end] in myset:
			myset.remove(s[start])
			start += 1
		myset.add(s[end])
		end += 1
		
		maxLen = max(maxLen , end - start)
	
	return maxLen
```


Time Complexity : O(N)
Space Complexity : O(N)


