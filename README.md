# Grind 75 Notes

This document contains concise notes for each question from [Grind 75](https://www.techinterviewhandbook.org/grind75/). Use these as [flash cards](https://en.wikipedia.org/wiki/Flashcard) to aid your study.


## Two Sum
[LeetCode Question](https://leetcode.com/problems/two-sum/description/)

#### Solutions
1. **Brute Force:** Iterate through all pairs until the target sum is found.
2. **Hash Table:** Use a hash table to store values as you iterate, achieving an efficient single-loop solution.

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

**Complexity Analysis**

- Time Complexity: `O(n^2)`, where `n` is the length of the array, as for each element in the array, the code loops the array again.
- Space Complexity: `O(1)`, since there the code only allocates memory to the `n` constant only.
  
#### Hash Table Solution

Store each number’s index in a hash table, allowing for `O(1)` lookup of the needed complement (target - current number) at each step.

```python
array = [1, 4, 5] # array of numbers
target = 2 # the target value

hash_table = {1: 0, 4: 1, 5: 2} # Keys are the array's items and values are their indexes in the array.
````

The hash table approach reduces the time complexity to `O(n)` by storing each number and its index. For each element, check if the complement (target - current element) exists in the hash table. If so, return indices; otherwise, store the element in the hash table.

```python
def twoSum(self, nums: List[int], target: int) -> List[int]:
        missing_values = {}

        for idx, num in enumerate(nums):
            missing = target - num

            if missing in missing_values:
                return [missing_values[missing], idx]

            missing_values[num] = idx
```

**Complexity Analysis**

- Time Complexity: `O(n)`, where `n` is the length of the array.
- Space Complexity: `O(n)`, since in the worst case, the code may insert each element of the array into the hash table, hence the space used by the hash table grows linearly with the number of elements in the array.

## Valid Parentheses
[LeetCode Question](https://leetcode.com/problems/valid-parentheses/description/)

#### Solutions:
1. **Stack:** Use a stack to track opening brackets and ensure they are closed in the correct order.

#### [Good To Know 📚] Stack
[Stack](https://en.wikipedia.org/wiki/Stack_(abstract_data_type)) is a data structure following the `first in, last out` (FILO) principle. It supports three main operations:

1. **Push**: Insert an item into the stack.
2. **Peek**: Look at the top item of the stack without removing it.
3. **Pop**: Remove the top item from the stack.

![image](https://github.com/user-attachments/assets/a9fbea37-a904-4169-8612-ccd85a2cffc0)

The stack data structure is useful in various scenarios. For example, [recursion](https://en.wikipedia.org/wiki/Recursion) uses a stack under the hood, and [Depth-First Search (DFS)](https://en.wikipedia.org/wiki/Depth-first_search) relies on a stack, either explicitly or through recursion.

A stack can be implemented using either an array or a linked list.

- **Array Implementation**: A variable _top_ keeps track of the size (length) of the stack, pointing to the next available slot in the array. The push operation adds an element and increments _top_ (checking for overflow), while the pop operation decrements _top_ (checking for underflow) and returns the removed item. A dynamic array allows the stack to grow or shrink as needed, making it an efficient stack implementation with amortized `O(1)` time complexity for push and pop operations. [Visualize array-based stack](https://www.cs.usfca.edu/~galles/visualization/StackArray.html).

- **Linked List Implementation**: The stack is represented by the head of the linked list, where pushing and popping occur. This implementation doesn't face overflow (unless memory is exhausted). [Visualize linked list-based stack](https://www.cs.usfca.edu/~galles/visualization/StackLL.html).

#### Stack Solution

In this approach, we use a set to store valid pairs of parentheses, e.g, `()`, `[]`, and `{}`. For each character in the string:

1. If it’s an opening parenthesis, we push it onto the stack.
2. If it’s a closing parenthesis, we check if the stack is empty or if the top of the stack, when paired with the current character, doesn’t form a valid pair. If either condition is met, the string is invalid, and we return `False`.
3. After iterating through the string, the stack should be empty for the string to be valid.


```python
def isValid(self, s: str) -> bool:
    stack = []
    valid_pairs = {"()", "[]", "{}"}

    for char in s:
        if char in "({[":
            stack.append(char)
        elif not stack or stack.pop() + char not in valid_pairs:
            return False

    return not stack
```

**Complexity Analysis**

- Time Complexity: `O(n)`, as we iterate through the string once.
- Space Complexity: `O(n)` in the worst case (when all characters are opening brackets and are pushed onto the stack). The actual space usage depends on the number of unmatched opening brackets, which can vary from O(1) (e.g., empty string or single valid pair) to O(n) (when all characters are a opening bracket).


## Merge Two Sorted Lists
[LeetCode Question](https://leetcode.com/problems/merge-two-sorted-lists/description/)

#### Solutions:
1. **Two Pointer:** Traverse both lists simultaneously, always selecting the next smallest element to add to the merged linked list.

#### [Good To Know 📚] Linked List
[Linked list](https://en.wikipedia.org/wiki/Linked_list) is a linear collection of elements where each element points to the next. It is a data structure consisting of a collection of nodes that represent a sequence.

Each node contains two fields: data and a reference to the next node:

![image](https://github.com/user-attachments/assets/a4cc8ba7-c2e7-4e34-8ed9-97ffd14fbaf0)

Elements in a linked list can be easily inserted or removed without reallocation or reorganization of the entire structure because the data items do not need to be stored contiguously in memory or on disk. This is the main benefit over a conventional array, where restructuring at run-time is a much more expensive operation. However, data access time is linear relative to the number of nodes in the list. Because nodes are sequentially linked, accessing any node requires accessing the prior node first (which introduces difficulties in pipelining). Faster access, such as random access, is not feasible. Arrays have better cache locality compared to linked lists.


Variations of linked lists:

- **Singly linked list**: Contains nodes with a 'value' field and a 'next' field, which points to the next node in the sequence. Operations include insertion, deletion, and traversal.

![image](https://github.com/user-attachments/assets/5a5e4322-9ea4-460d-b6bb-fcc3d0248211)

- **Doubly linked list**: Each node contains, besides the next-node link, a second link field pointing to the 'previous' node in the sequence. The two links may be called 'forward' and 'backward', or 'next' and 'prev' (previous).

![image](https://github.com/user-attachments/assets/6611caa5-8be6-43cc-a362-89f95030bf1d)

- **Multiply linked list**: Each node contains two or more link fields, each used to connect the same set of data arranged in different orders (e.g., by name, by department, by date of birth, etc.). While a doubly linked list can be seen as a special case of a multiply linked list, having two or more orderings that are opposite to each other leads to simpler and more efficient algorithms, so they are usually treated as a separate case.

- **Circular linked list**: In a circular linked list, the last node points back to the first node instead of containing a null reference. This makes the list circular. If the last node's "next" pointer points to null, it is an open or linear list.

![image](https://github.com/user-attachments/assets/ac42f5ab-b7ff-4adc-bbb0-50fae40206ad)

**Time Complexities:**

1. **Peek (access by index)**: `O(n)`
2. **Insert or delete at beginning**: `O(1)`
3. **Insert or delete at end**: `O(1)` if the end is known; otherwise `O(n)`
4. **Insert or delete in the middle**: `O(n)`

#### Two Pointers Solution

Since both lists are already sorted, we can compare the nodes from both lists and select the smaller one to add to the merged list until one of the lists is exhausted.

1. **Create a dummy node**: A dummy node is used to simplify edge cases where the head of the merged list is not known initially.
2. **Initialize a pointer `curr`**: This pointer tracks the end of the merged list and starts at the dummy node.
3. **Traverse both lists**:
    - Compare the current nodes of both lists.
    - Append the smaller node to `curr.next`.
    - Move the pointer of the list from which the node was taken forward.
    - Move `curr` to the next node.
4. **Append the remaining nodes**: Once one list is exhausted, append the remaining nodes from the other list.

```python
def mergeTwoLists(
        self, list1: Optional[ListNode], list2: Optional[ListNode]
    ) -> Optional[ListNode]:
        dummy = ListNode()
        curr = dummy

        while list1 and list2:
            if list1.val <= list2.val:
                curr.next = list1
                list1 = list1.next
            else:
                curr.next = list2
                list2 = list2.next

            curr = curr.next

        curr.next = list1 if list1 else list2

        return dummy.next
```

**Complexity Analysis**

- Time Complexity: `O(n + m)`, where `n` and `m` are the lengths of the first and second list. 
- Space Complexity: `O(1)`. The solution creates a dummy node and a `curr` pointer, but their space usage does not scale with the input size.

## Best Time to Buy and Sell Stock
[LeetCode Question](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/)

#### Solutions:
1. **Greedy Algorithm (One Pass)**: Calculate the maximum profit in a single iteration by tracking the minimum price seen so far and updating the profit accordingly.

#### Greedy Algorithm (One Pass) Solution

This solution maintains two variables while iterating through the `prices` array:

1. `min_price`: Tracks the lowest stock price observed so far, initialized to infinity to simulate that no stock has been bought yet.
2. `profit`: Tracks the maximum profit observed so far, initialized to `0` since no transaction has occurred yet.

For each price in the array:
- Simulate selling by calculating the difference between the current price and `min_price` to get the current profit.
- If the current profit exceeds the previously recorded maximum, update `profit`.
- If the current price is lower than `min_price`, update `min_price`.

By the end of the iteration, `profit` will hold the maximum achievable profit.

```python
def maxProfit(self, prices: List[int]) -> int:
        profit = 0
        min_price = float('inf')

        for price in prices:
            profit = max(profit, price - min_price)
            min_price = min(min_price, price)

        return profit
```

**Complexity Analysis**

- Time Complexity: `O(n)`. The solution iterates through the prices array once, where `n` is the length of the array.
- Space Complexity: `O(1)`. Only two variables (`profit` and `min_price`) are used, making the space usage constant.

## Valid Palindrome
[LeetCode Question](https://leetcode.com/problems/valid-palindrome/description/)

#### Solutions:
1. **Two Pointer**: Use two pointers starting from the edges of the string and moving towards the center. Check if the characters match, skipping non-alphanumeric characters.
2. **Pythonic**: Format the string to remove non-alphanumeric characters and compare it with its reversed version.

#### Two Pointer Solution

This approach uses two pointers:

- **Left pointer (`l`)**: Starts at the beginning of the string.
- **Right pointer (`r`)**: Starts at the end of the string.

**Steps**:
1. Skip non-alphanumeric characters by incrementing/decrementing the pointers as needed.
2. If the characters at `l` and `r` are equal (ignoring case), move both pointers inward.
3. If the characters differ, return `False` as the string is not a palindrome.
4. If the loop completes, return `True` since the string is a valid palindrome.

```python
def isPalindrome(self, s: str) -> bool:
        l, r = 0, len(s) - 1

        while l < r:
            while l < r and not s[l].isalnum():
                l += 1
            while l < r and not s[r].isalnum():
                r -= 1
            if s[l].lower() != s[r].lower(): 
                return False
                
            l += 1
            r -= 1

        return True
```

Note that a boundary check is performed in the inner while loop, since by incrementing/decrementing l/r, the outer loop condition `l < r` might be violated.

**Complexity Analysis**

- Time Complexity: `O(n)`, as each character is processed at most once.
- Space Complexity: `O(1)`, since no extra space is used.

#### Pythonic Solution

This approach simplifies the problem using Python's built-in string manipulation capabilities.

**Steps**:
1. Create a "cleaned" version of the string by:
   - Removing non-alphanumeric characters.
   - Converting all characters to lowercase.
2. Compare the cleaned string to its reversed version.

```python
def isPalindrome(self, s: str) -> bool:
        clean_str = ''.join(char.lower() for char in s if char.isalnum())
        return clean_str == clean_str[::-1]
```

**Complexity Analysis**

- Time Complexity: `O(2*n)`, as the string is iterated twice (once for cleaning, once for reversing).
- Space Complexity: `O(n)`, due to the extra memory required for the `clean_str`.
