
## Sliding Window

This technique refers to creating a window that "slides" through an input sequence (typically an array or string).


Sliding windows can be either variable or fixed length. On this page, we'll cover **variable-length** sliding windows by looking at:

1. An example problem that illustrates the motivation for each type of sliding window, as well as how to implement it.

2. The types of problems for which each type of sliding window is useful, as well as templates you can use as a starting point.

3. A list of practice problems (with animated solutions and explanations!) for you to try to build upon the concepts covered here.


### Problem 

###### DESCRIPTION (credit [Leetcode.com](https://www.leetcode.com/problems/fruit-into-baskets))

Write a function to calculate the maximum number of fruits you can collect from an integer array fruits, where each element represents a type of fruit. You can start collecting fruits from any position in the array, but you must stop once you encounter a third distinct type of fruit. The goal is to find the longest subarray where at most two different types of fruits are collected.

**Example**: Input: fruits = [3, 3, 2, 1, 2, 1, 0] Output: 4 Explanation: We can pick up 4 fruit from the subarray [2, 1, 2, 1]


### Naive Apporach

```python
def fruit_into_baskets(fruits):

    max_length = 0

    # i and j are the start and end indices of the subarray

    for i in range(len(fruits)):

        for j in range(i, len(fruits)):

            if len(set(fruits[i:j + 1])) <= 2:

                max_length = max(max_length, j - i + 1)

            else:

                # the subarray starting at i is invalid

                # so we break and move to the next one

                break

    return max_length
```

This approach considers O(n2) subarrays. For each subarray, it checks if it contains at most 2 distinct fruits by converting it to a set and checking its length, which takes O(n) time, for a total a time complexity of O(n3). We'll gradually improve this approach until we reach the sliding window pattern, which solves this problem in O(n) time.


### Optimized Solution

```python
def fruit_into_baskets(fruits):

  start = 0

  state = {}

  max_fruit = 0

  for end in range(len(fruits)):

    state[fruits[end]] = state.get(fruits[end], 0) + 1

    while len(state) > 2:

      state[fruits[start]] -= 1

      if state[fruits[start]] == 0:

        del state[fruits[start]]

      start += 1

    max_fruit = max(max_fruit, end - start + 1)

  return max_fruit
```

#### Complexity

- The time complexity of this algorithm is O(n), where n is the length of the input array. end iterates through the array once, and start iterates through the array at most once. Each time either moves, we arrive at a new window, which requires O(1) time to check if its valid.
    
- The space complexity of this algorithm is O(1), since state never contains more than 3 keys.

