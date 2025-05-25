
Heaps are useful for solving problems that require finding the "top k" elements in an array. For example, let's use a heap to find the 3 largest elements in an array.

###### DESCRIPTION

Given an integer array nums, return the 3 largest elements in the array in any order.

**Example**

`Input: nums = [9, 3, 7, 1, -2, 6, 8] Output: [8, 7, 9] # or [7, 9, 8] or [9, 7, 8] ...`

Here's how we can solve this problem using a min-heap:

- Create a min-heap that stores the first 3 elements of the array. These represent the 3 largest elements we have seen so far, with the smallest of the 3 at the root of the heap.

![[20250421104439.png]]

- Iterate through the remaining elements in the array.
    
    - If the current element is larger than the root of the heap, pop the root and push the current element into the heap.

    - Otherwise, continue to the next element.

### Solution

```python
import heapq

def three_largest(nums):
    # create a min-heap with the first 3 elements
    heap = nums[:3]
    heapq.heapify(heap)
    
    # iterate through the remaining elements
    for num in nums[3:]:
        if num > heap[0]:
            heapq.heappop(heap)
            heapq.heappush(heap, num)
            
    return heap
```


#### Complexity Analysis

**Time Complexity**:

- The heapify function takes O(3) = O(1) time.
- We iterate through all the elements in the array once, which takes O(n) time.
- The heappop and heappush operations take O(log 3) = O(1) time.


Therefore, the total time complexity is O(n).

**Space Complexity**

- We use a heap of size 3 to store the 3 largest elements, which takes O(3) = O(1) space.
- The space complexity of the algorithm is O(1).

The time and space complexity become more interesting when 3 is a variable number, which is the case in the first practice problem.




