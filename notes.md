# Grind 75 Notes

This document contains concise notes for each question from [Grind 75](https://www.techinterviewhandbook.org/grind75/). Use these as [flash cards](https://en.wikipedia.org/wiki/Flashcard) to aid your study.


## Two Sum
[Leet Code Question](https://leetcode.com/problems/two-sum/description/)

**Solutions:**
1. **Brute Force** - `O(n^2)`: Iterate through all pairs until the target sum is found.
2. **Hash Table** - `O(n)`: Use a hash table to store values as you iterate, achieving an efficient single-loop solution.

#### Brute Force Solution

Uses two loops to check each pair of elements until target is found.

```python
def twoSum(self, nums: List[int], target: int) -> List[int]:
        n = len(nums)
        for i in range(n - 1):
            for j in range(i + 1, n):
                if nums[i] + nums[j] == target:
                    return [i, j]
        return []  # No solution found
```

#### Hash Table Solution

Store each number’s index in a hash table, allowing for O(1) lookup of the needed complement (target - current number) at each step.

```python
array = [1, 4, 5] # array of numbers
target = 2 # the target value

hash_table = {1: 0, 4: 1, 5: 2} # Keys are the array's items and values are their indexes in the array.
````

The hash table approach reduces complexity to O(n) by storing each number and its index. For each element, check if the complement (target - current element) exists in the hash table. If so, return indices; otherwise, store the element in the hash table.

```python
def twoSum(self, nums: List[int], target: int) -> List[int]:
        missing_values = {}

        for idx, num in enumerate(nums):
            missing = target - num

            if missing in missing_values:
                return [missing_values[missing], idx]

            missing_values[num] = idx
```
