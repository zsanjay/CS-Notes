
###### DESCRIPTION (credit [Leetcode.com](https://leetcode.com/problems/maximum-points-you-can-obtain-from-cards))

Given an array of integers representing the value of cards, write a function to calculate the maximum score you can achieve by selecting exactly k cards from either the beginning or the end of the array.

For example, if k = 3, then you have the option to select:

- the first three cards,
    
- the last three cards,
    
- the first card and the last two cards
    
- the first two cards and the last card
    

###### EXAMPLES

**Example 1**: Input:

`cards = [2,11,4,5,3,9,2] k = 3`

Output:

`17`

Explanation:

- Selecting the first three cards from the beginning (2 + 11 + 4) gives a total of 17.
    
- Selecting the last three cards (3 + 9 + 2) gives a total of 14.
    
- Selecting the first card and the last two cards (2 + 9 + 2) gives a total of 13.
    
- Selecting the first two cards and the last card (2 + 11 + 2) gives a total of 15.
    

So the maximum score is 17.


### Solution 

```python
class Solution:

def maxScore(self, cards: list[int], k: int):

	start = 0
	max_sum = 0
	curr_sum = 0
	totalSum = 0
	n = len(cards)
	
	
	for i in range(n):
		totalSum += cards[i]
		
	if k == n:
		return totalSum
		
		  
		
	for end in range(n):
		curr_sum += cards[end]
		
		if end - start + 1 == n - k:
			max_sum = max(max_sum , totalSum - curr_sum)
			curr_sum = curr_sum - cards[start]
			start += 1
	
	return max_sum
```


Time Complexity : O(n)
Space Complexity : O(1)

