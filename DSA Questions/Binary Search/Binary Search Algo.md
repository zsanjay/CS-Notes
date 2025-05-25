
```python
def binary_search(nums, target):

  left = 0
  right = len(nums) - 1

  while left <= right:
    mid = (left + right) // 2

    if nums[mid] == target:
      return mid

    if nums[mid] < target:
      left = mid + 1
    else:
      right = mid - 1

  return -1
```

Time Complexity : O(logn)
Space Complexity : O(1)

