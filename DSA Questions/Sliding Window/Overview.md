
There are two types of sliding window approaches:

1. Variable length sliding window
2. Fixed Length sliding window

#### Template for Variable Length

```python
def variable_length_sliding_window(nums):

  state = # choose appropriate data structure
  start = 0
  max_ = 0

  for end in range(len(nums)):
    # extend window
    # add nums[end] to state in O(1) in time

    while state is not valid:
      # repeatedly contract window until it is valid again
      # remove nums[start] from state in O(1) in time
      start += 1

    # INVARIANT: state of current window is valid here.
    max_ = max(max_, end - start + 1)

  return max_
```


#### Template for Fixed Length

```python
def fixed_length_sliding_window(nums, k):

  state = # choose appropriate data structure
  start = 0
  max_ = 0

  for end in range(len(nums)):
    # extend window
    # add nums[end] to state in O(1) in time

    if end - start + 1 == k:

      # INVARIANT: size of the window is k here.
      max_ = max(max_, contents of state)
      
      # contract window
      # remove nums[start] from state in O(1) in time
      start += 1

  return max_
```

