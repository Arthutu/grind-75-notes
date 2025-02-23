# Grind 75 Notes

This document contains concise notes for each question from [Grind 75](https://www.techinterviewhandbook.org/grind75/). Use these as [flash cards](https://en.wikipedia.org/wiki/Flashcard) to aid your study.


## Two Sum
[LeetCode Question](https://leetcode.com/problems/two-sum/description/)

#### Solutions
1. **Brute Force:** Iterate through all pairs until the target sum is found.
2. **Hash Table:** Use a hash table to store values as you iterate, achieving an efficient single-loop solution.

---
#### [Good To Know 📚] Hash Table (Hash Map)

#### Hash Function

Before diving into hash tables, we first need to understand [hash functions](https://en.wikipedia.org/wiki/Hash_function). A hash function converts data of arbitrary size to a fixed-size value, such as a 32-bit integer. These values, called _hash values_, _hash codes_, _hash digests_, _digests_, or simply _hashes_ are critical in data structures like hash tables.

For instance, a simple hash function for an integer array could sum its elements and return the sum modulo 100:  
- Hashing `[5, 23, 84]` yields `(5 + 23 + 84) % 100 = 12`.  
- Hashing `[31, 22, 45, 61, 10]` yields `(31 + 22 + 45 + 61 + 10) % 100 = 69`.

A valid hash function must always return the same hash value for two items representing the same values. For example, the hash function mentioned in the previous paragraph always returns the same value for arrays of identical size and contents, regardless of other attributes such as memory address, etc. However, the converse is not necessarily true. Two items with the same hash values are not necessarily equal. For example, `[32, 10]` has the same hash value as `[18, 10, 14]`, even though they are not equal. Such cases of two different items hashing to the same value is known as a hash collision.

![image](https://github.com/user-attachments/assets/ce23ad85-210b-480c-a6d4-23e5ba561e6b)

Key attributes of a good hash function:  
1. Easy to compute the hash value (the function has low time complexity).  
2. Minimizes collisions.  
3. Distributes hash values evenly.  

A good hash function satisfying the above criteria can help improve the efficiency of hash tables.

#### Hash Table (Hash Map)

A [Hash Table](https://en.wikipedia.org/wiki/Hash_table) is a data structure that implements an associative array, also called a dictionary or simply map; an associative array is an abstract data type that maps keys to values. A hash table uses a hash function to compute an index, also called a hash code, into an array of _buckets_ or _slots_, from which the desired value can be found. During lookup, the key is hashed and the resulting hash indicates where the corresponding value is stored. A map implemented by a hash table is called a hash map.

![image](https://github.com/user-attachments/assets/772e7354-a947-4a18-a32e-df3016ac5284)

As the possibility of keys increases, [hash collision (where two different items have the same hash value)](https://en.wikipedia.org/wiki/Hash_collision) is unavoidable by the [pigeonhole principle](https://en.wikipedia.org/wiki/Pigeonhole_principle). To handle the collisions, two of the most common strategies are [open addressing](https://en.wikipedia.org/wiki/Open_addressing) and [separate chaining](https://en.wikipedia.org/wiki/Hash_table#Separate_chaining).

#### Open Addressing

Open addressing is a collision resolution technique in which every entry record is stored in the bucket array itself, and the hash resolution is performed through probing. When a new entry has to be inserted, the buckets are examined, starting with the hashed-to slot and proceeding in some probe sequence, until an unoccupied slot is found. When searching for an entry, the buckets are scanned in the same sequence, until either the target record is found, or an unused array slot is found, which indicates an unsuccessful search.

![image](https://github.com/user-attachments/assets/424a9a85-0e0b-4247-86f9-b4aa400d72a0)

#### Separate Chaining

In separate chaining, the process involves building a linked list with key–value pair for each search array index. The collided items are chained together through a single linked list, which can be traversed to access the item with a unique search key. Collision resolution through chaining with linked list is a common method of implementation of hash tables, although other lists that can dynamically increase in the size will do as well.

![image](https://github.com/user-attachments/assets/d842fb6d-2153-4448-b0a2-64ebe24528a6)

#### Time Complexities

1. **Search**: `O(1)` in average, `O(n)` in the worst case.
2. **Insert**: `O(1)` in average, `O(n)` in the worst case.
3. **Delete**: `O(1)` in average, `O(n)` in the worst case.
4. **Space**: `O(n)` in average, `O(n)` in the worst case.
---

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

--- 

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

---

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

---

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

---

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

## Invert Binary Tree
[LeetCode Question](https://leetcode.com/problems/invert-binary-tree/description/)

#### Solutions:
1. **Depth-first Search (DFS)**: Recursively traverse the tree and swap the left and right subtrees.
2. **Breadth-first Search (BFS)**: Iteratively tranverse the tree and swap the left and right nodes.

---

#### [Good To Know 📚] Trees
[Tree](https://en.wikipedia.org/wiki/Tree_(abstract_data_type)) is a type of graph data structure composed of nodes and edges. Its main properties are:

1. Acyclic, meaning it does not contain any cycles.
2. There exists a path from the root to any node.
3. Has `N - 1` edges, where `N` is the number of nodes in the tree.
4. Each node has exactly one parent node, except the root node.

![image](https://github.com/user-attachments/assets/267dfaf4-a1f5-4eb5-8535-8cc775016284)

####  Terminology of a tree

1. Root: The topmost node in the tree.
2. Internal node: Every node of a tree that has child nodes.
3. Leaf: Every node in a tree that has no child nodes.
4. Subtree: A smaller tree formed from a node and its descendants.
5. Ancestor: All the nodes that are between the path from the root to the current node are the ancestors of the current node.
6. Descendent: All the nodes that are reachable from the current node when moving down the tree are descendants of the current node.
7. Level: The number of edges along the unique path between it and the root node. This is the same as depth.
8. Breadth: The number of leaves.
9. Size of a tree: Number of nodes in the tree.
10. Distance: The number of edges along the shortest path between two nodes.
11. Width: The number of nodes in a level.

#### Binary Tree

[Binary tree](https://en.wikipedia.org/wiki/Binary_tree) is a type of tree in which each node has at most two children, in other words, every node in a binary tree has 0 to 2 children.

![image](https://github.com/user-attachments/assets/5859c324-a904-4628-99f8-d9cb9e2d281e).

There are three types of binary trees:

1. Full Binary Tree: Every node has 0 or 2 children.

![image](https://github.com/user-attachments/assets/226e0d4f-38a3-40fc-abb0-db3d0cb0ce24)

2. Complete Binary Tree: All levels are filled except possibly the last, which is filled from left to right.

![image](https://github.com/user-attachments/assets/c416db85-1f63-42be-ba6a-f001fe4d8ca2)

3. Perfect Binary Tree: All internal nodes have two children, and all leaf nodes are at the same level.

#### Binary Search Tree

A [Binary Search Tree (BST)](https://en.wikipedia.org/wiki/Binary_search_tree) is a rooted binary tree with the value of each internal node being greater than all the values in the respective node's left subtree and lest than the ones in its right subtree. In other words, all left descendants < node < all right descendats.

![image](https://github.com/user-attachments/assets/60f5c41f-6568-4555-bade-616f6e29e291)

**Search**

The main purpose of a BTS is to search efficiently. To search for an item, look at the value of the top node and see if its greater, smaller or equal to the item being looked for. If it is equal, then item is found. If it is smaller, look at the left subtree. If it is larger, look at the right subtree. The time complexity of each search is `O(n)` for the worst case and `O(log(n))` for the avarege case, where `n` is the height of the tree.

**Insertion**

Unlike a sorted list, inserting an item to the BST does not require each item in the list to move down an index. Instead, when inserting an item, first perform the searching for that item in that BST. However, if we find an empty tree, instead we replace that empty tree with a new node containing the inserted value in the BST. The time complexity is `O(n)` for the worst case and `O(log(n))` for the avarege case, where `n` is the height of the tree.

**Deletion**

Deleting an element from the tree is the same as finding an element in the tree. After you find the node, if the node's right subtree is empty, bring its left subtree to its current position and remove the node. Otherwise, delete the leftmost node of the right subtree and put it in its current position. The time complexity is `O(n)` for the worst case and `O(log(n))` for the avarege case, where `n` is the height of the tree.

#### Balanced Binary Tree

A [balanced binary tree](https://en.wikipedia.org/wiki/Self-balancing_binary_search_tree) is a binary tree in which, for every node in the tree, the difference in height (depth) of its left and right subtrees is at most 1.

#### Tree Traversal

[Tree Traversal](https://en.wikipedia.org/wiki/Tree_traversal) refers to the process of visiting each node in the tree, exactly once.

**In-order Traversal**

In-order traversal vists the left branch subtree, then the current node, and finally the right subtree.

**Pre-order Traversal**

In-order traversal vists the current node first, then the left subtree, and finally the right subtree.

**Post-order Traversal**

In-order traversal vists the left subtree first, then the right subtree, and finally the current node.

#### Depth First Search

[Depth-first search (DFS)](https://en.wikipedia.org/wiki/Depth-first_search) is an algorithm for traversing or searching tree or graph data structures. The algorithm starts at the root node and explores as far as possible along each branch before [backtracking](https://en.wikipedia.org/wiki/Backtracking). In terms of tree traverasl it is a pre-order traversal. Extra memory, usually a stack, is needed to keep track of the nodes discovered so far along a specified branch which helps in backtracking of the graph.

DFS is better at finding nodes far away from the root.

#### Breadth First Search

[Breadth First Search](https://en.wikipedia.org/wiki/Breadth-first_search) is an algorithm for searching a tree data structure for a node that satisfies a given property. It starts at the tree root and explores all nodes at the present depth prior to moving on to the nodes at the next depth level. Extra memory, usually a queue, is needed to keep track of the child nodes that were encountered but not yet explored.

BFS is better for finding nodes close/closest to the root.

---

#### DFS Solution

To invert a binary tree, we need to swtich the left and right nodes. Thinking in DFS, we first need to decide the return value, which in this case is the inverted version of the current subtree. Second, we need to identify the states. In this problem, there is no additional state that need to be carried other than the current tree that we are inverting.

The recursion ends at a leaft node. From thereafter, the left and right subtrees are swaped based on the recursion call stack.

```python
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if root is None:
            return None

        root.left, root.right = self.invertTree(root.right), self.invertTree(root.left)

        return root
```

**Complexity Analysis**

- Time Complexity: `O(n)`, where n is the number of nodes in the tree, since every node is visited once.
- Space Complexity: `O(h)`, where h is the height of the tree. This represents the maximum depth of the recursion stack. In the worst case (skewed tree), `h = n`.

#### BFS Solution

To invert a binary tree, we need to swtich the left and right nodes. Thinking in BFS, we create a queue and the curret node to it. Then, while the queue is not empty, we pop an item from the queue, swap its left and right values, and add its child to the queue.


```python
def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        q = collections.deque([root])
        while q:            
            if cur:= q.popleft():
                cur.right, cur.left = cur.left, cur.right
                q.extend([cur.left, cur.right])
        return root
```

**Complexity Analysis**

- Time Complexity: `O(n)`, where n is the number of nodes in the tree, since every node is visited once.
- Space Complexity: `O(n)`, where n is the number of nodes in the queue.

## Valid Anagram
[LeetCode Question](https://leetcode.com/problems/valid-anagram/description/)

#### Solutions:

1. **Characters Frequency**: Count the frequency of each character in both strings and compare.

#### Characters Frequency

An [anagram](https://en.wikipedia.org/wiki/Anagram) is a rearrangement of letters from one word to form another, using all original letters exactly once. To verify whether two strings are anagrams:  

1. **Lengths**: If the strings differ in length, they cannot be anagrams.  
2. **Frequency Count**: Iterate through both strings, incrementing and decrementing the count for each character in an array of size 26 (to represent lowercase English letters).  
3. **Validation**: If all counts in the array are `0` after processing, the strings are anagrams.

```python
def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False

        freq = [0] * 26
        for i in range(len(s)):
            freq[ord(s[i]) - ord("a")] += 1
            freq[ord(t[i]) - ord("a")] -= 1

        return all(f == 0 for f in freq)
```

**Complexity Analysis**

- Time Complexity: `O(n)`, where `n` is the length of `s`.
- Space Complexity: `O(C)`, where `C` is equal to 26, all lowercase English letters.

## Binary Search
[LeetCode Question](https://leetcode.com/problems/binary-search/)

#### Solutions:

1. **Two Pointers**: Utilize two pointers, `left` and `right`, to represent the search bounds within the array. Repeatedly narrow the search interval by comparing the middle value with the target.

#### Two Pointers

To achieve the `O(log n)` runtime requirement, a [binary search algorithm](#good-to-know--trees) is employed. The array is divided in half during each iteration:

1. Compute the midpoint of the current range using `(left + right) // 2`.  
2. Compare the value at `nums[mid]` to the target:  
   - If they are equal, return the `mid` index.  
   - If the target is smaller, adjust the search range to the left by updating `right = mid - 1`.  
   - If the target is larger, adjust the search range to the right by updating `left = mid + 1`.  
3. Repeat until the search range is empty (`left > right`).  

If the target is not found, return `-1`.  

```python
def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1

        while left < right:
            mid = (left + right) // 2

            if nums[mid] == target:
                return mid
            elif nums[mid] > target:
                right = mid - 1
            else:
                left = mid + 1

        return -1
```

**Complexity Analysis**

- Time Complexity: `O(log n)`, where `n` is the length of `nums`.
- Space Complexity: `O(1)`, since the `left`, `right`, and `mid` variables use a constant amount of extra space.

## Flood Fill
[LeetCode Question](https://leetcode.com/problems/flood-fill/description/)

#### Solutions:

1. **Depth-First Search (DFS):** Recursively visit adjacent cells to change their color.  
2. **Breadth-First Search (BFS):** Iteratively visit adjacent cells to change their color using a queue.  

#### Depth-First Search (DFS) 

This approach uses recursion to explore and update all connected pixels that share the same color as the starting pixel. The process continues until all valid pixels are updated.

**Steps:**

1. From the starting pixel, update its color to color.
2. Look at adjacent pixels. For each adjacent pixel:
   - If the pixel is within bounds, and
   - If the pixel's color is the same as the starting pixel's original color, and the pixel isn't already the new color (to prevent infinite recursion),              then recursively apply the flood fill to that pixel.

```python
def floodFill(
        self, image: List[List[int]], sr: int, sc: int, color: int
    ) -> List[List[int]]:
        def dfs(row, col):
            if (
                not 0 <= row < img_height
                or not 0 <= col < img_width
                or image[row][col] != original_color
                or image[row][col] == color
            ):
                return

            image[row][col] = color
            for delta_row, delta_column in zip(directions[:-1], directions[1:]):
                dfs(row + delta_row, col + delta_column)

        directions = [-1, 0, 1, 0, -1]
        img_height, img_width = len(image), len(image[0])
        original_color = image[sr][sc]

        dfs(sr, sc)

        return image
```

**Complexity Analysis**

- Time Complexity: `O(M * N)`, where `M` is the number of rows and `N` is the number of columns in the image.
- Space Complexity: `O(M * N)`, for the recursive stack in the worst case (entire grid is one connected component).

#### Breadth-first Search (BFS)

The BFS solution uses a queue to iteratively process connected pixels. It ensures that all adjacent pixels are visited in layers, starting from the initial pixel.

```python
from collections import deque

def flood_fill(r: int, c: int, replacement: int, image: List[List[int]]) -> List[List[int]]:
    num_rows, num_cols = len(image), len(image[0])
    def get_neighbors(coord, color):
        row, col = coord
        delta_row = [-1, 0, 1, 0]
        delta_col = [0, 1, 0, -1]
        for i in range(len(delta_row)):
            neighbor_row = row + delta_row[i]
            neighbor_col = col + delta_col[i]
            if 0 <= neighbor_row < num_rows and 0 <= neighbor_col < num_cols:
                if image[neighbor_row][neighbor_col] == color:
                    yield neighbor_row, neighbor_col

    def bfs(root):
        queue = deque([root])
        visited = [[False for c in range(num_cols)] for r in range(num_rows)]
        r, c = root
        color = image[r][c]
        image[r][c] = replacement
        visited[r][c] = True
        while len(queue) > 0:
            node = queue.popleft()
            for neighbor in get_neighbors(node, color):
                r, c = neighbor
                if visited[r][c]:
                    continue
                image[r][c] = replacement
                queue.append(neighbor)
                visited[r][c] = True

    bfs((r, c))
    return image
```

**Complexity Analysis**

- Time Complexity: `O(M * N)`, where `M` is the number of rows and `N` is the number of columns in the image.
- Space Complexity: `O(V + E)`, where `V` is the number of vertices and `E` is the number of edges in the graph.

## Lowest Common Ancestor of a Binary Search Tree
[LeetCode Question](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/description/)

#### Solutions:
1. **Depth First Search (DFS)**: Recursively traverse the tree until a parent node were its children are split, e.g, one in the right and the other in the left subtree, is found.
2. **Iterative Approach**: Iteratively update the root to the next node until a node were its children are split, e.g, one in the right and the other in the left subtree, is found.

---

#### [Good To Know 📚] Lowest common ancestor

The [lowest common ancestor](https://en.wikipedia.org/wiki/Lowest_common_ancestor) is defined between two nodes `p` and `q` as the lowest node in `T` that has both `p` and `q` as descendants (where we allow a node to be a descendant of itself).

---

#### DFS Solution

To find the LCA in a BST, consider three scenarios:
1. **Both nodes are in the left subtree**: If both `p` and `q` are smaller than the current node's value, continue the search in the left subtree.
2. **Both nodes are in the right subtree**: If both `p` and `q` are greater than the current node's value, continue the search in the right subtree.
3. **Nodes are split across subtrees (or one equals the current node)**: The current node is the LCA.

```python
def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        if p.val < root.val and q.val < root.val:
            return self.lowestCommonAncestor(root.left, p, q)
        elif p.val > root.val and q.val > root.val:
            return self.lowestCommonAncestor(root.right, p, q)
        else:
            return root
```

**Complexity Analysis**

- Time Complexity: `O(h)`, where `h` is the height of the tree. In the best case (balanced tree), `h = log n`. In the worst case (skewed tree), `h = n`.
- Space Complexity: `O(h)`, where h is the height of the tree. This represents the maximum depth of the recursion stack.

#### Iterative Solution

```python
def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        while root:
            if p.val < root.val and q.val < root.val:
                root = root.left
            elif p.val > root.val and q.val > root.val:
                root = root.right
            else:
                return root
```

**Complexity Analysis**

- Time Complexity: `O(h)`, where `h` is the height of the tree. In the best case (balanced tree), `h = log n`. In the worst case (skewed tree), `h = n`.
- Space Complexity: `O(1)`, since no additional memory is used beyond the traversal.

## Balanced Binary Tree
[LeetCode Question](https://leetcode.com/problems/balanced-binary-tree/description/)

#### Solutions:
1. **Depth First Search (DFS)**: Recursively traverse the tree and check the height of both left and right subtrees for each node, verifying if the condition (height difference is no more than one), is met.


#### DFS Solution

This solution uses a helper function to recursively determine whether a subtree is balanced and calculate its height. The key logic is:

1. If the current node is `None`, return a height of `0` (base case).
2. Recursively calculate the height of the left and right subtrees.
3. If either subtree is unbalanced (indicated by a height of `-1`), propagate `-1` upward.
4. If the absolute difference between the heights of the left and right subtrees exceeds `1`, mark the current subtree as unbalanced by returning `-1`.
5. Otherwise, return the height of the current subtree as `max(left_height, right_height) + 1`

The tree is balanced if the helper function does not return `-1`.

```python
def isBalanced(self, root: Optional[TreeNode]) -> bool:
        def helper(node):
            if node is None:
                return 0
            
            right_height = helper(node.right)
            left_height = helper(node.left)

            if right_height == -1 or left_height == -1:
                return -1

            if abs(right_height - left_height) > 1:
                return -1
            
            return max(right_height, left_height) + 1


        return helper(root) != -1
```

**Complexity Analysis**

- Time Complexity: `O(n)`, where `n` is the number of nodes in the Tree.
- Space Complexity: `O(h)`, where h is the height of the tree. This represents the maximum depth of the recursion stack.

## Linked List Cycle
[LeetCode Question](https://leetcode.com/problems/linked-list-cycle/)

#### Solutions:

1. **Floyd's Cycle Finding Algorithm (Tortoise and Hare Algorithm)**: Utilizes two pointers: a slow pointer (`tortoise`) and a fast pointer (`hare`), moving through the list at different speeds. If there is a cycle, the two pointers will eventually meet inside it.

#### Floyd's Cycle Finding Algorithm

Floyd's cycle-finding algorithm employs two pointers that traverse the sequence at different speeds. It is commonly called the "tortoise and hare algorithm," referencing Aesop's fable *The Tortoise and the Hare*.

![image](https://github.com/user-attachments/assets/6cd3cdad-9be4-4618-b305-4484a8bbaff3)

To solve the problem:

1. The faster pointer (`hare`) moves at twice the speed of the slow pointer (`tortoise`). 
2. If there is a cycle in the linked list, the tortoise and the hare will meet at some point within the cycle.  
3. If no cycle exists, the hare will reach the end of the list (`null`).


```python
def hasCycle(self, head: Optional[ListNode]) -> bool:
        slow = fast = head

        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next

            if slow == fast:
                return True
        
        return False
```

**Complexity Analysis**

- Time Complexity: `O(n)`, where n is the number of nodes in the linked list. Each node is visited at most twice (once by the slow pointer and once by the fast pointer).
- Space Complexity: `O(1)`, since only two pointers are used for the traversal.

## First Bad Version
[LeetCode Question](https://leetcode.com/problems/first-bad-version/description/)

#### Solutions:

1. **Binary Search**: Efficiently narrows down the search space by halving it at each step to find the first bad version.

#### Binary Search

Since the input array is ordered from "good" to "bad," and the first bad version causes all subsequent versions to also be bad, binary search is an effective solution:

1. Initialize two pointers: `left` starting at the first version, and `right` starting at the last version.  
2. While the search space is not narrowed down to a single version:  
   - Calculate the midpoint: `mid = (left + right) // 2`.  
   - Check if `mid` is a bad version using the provided `isBadVersion()` API:  
     - If `mid` is bad, the first bad version must be to the left or could be `mid` itself, so move the `right` pointer to `mid`.  
     - If `mid` is not bad, move the `left` pointer to `mid + 1`.  
3. When `left` equals `right`, the first bad version is found.


```python
def firstBadVersion(self, n: int) -> int:
        left, right = 0, n 

        while left < right:
            mid = (left + right) // 2

            if isBadVersion(mid):
                right = mid
            else:
                left = mid + 1

        return right
```

**Complexity Analysis**

- Time Complexity: `O(log n)`, since the search space is halved in each iteration, resulting in logarithmic complexity.
- Space Complexity: `O(1)`, since only two pointers, `lef`t and `right`, and the `mid` variable are used which require constant space.

## Ransom Note
[LeetCode Question](https://leetcode.com/problems/ransom-note/description/)

#### Solutions:

1. **Hash Table**: Counts the letters in `magazine` and stores them in a hash table. Then, checks if the letters are sufficient to construct the `ransomNote`.

#### Hash Table

The solution counts the occurrences of each character in `magazine` using a hash table. It then iterates through each character in `ransomNote`, decrementing the count in the hash table. If a required character is missing, the function returns `False`. If the loop completes, the `ransomNote` can be constructed.  

```python
def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        available_letters = {}

        for char in magazine:
            available_letters[char] = available_letters.get(char, 0) + 1
        
        for char in ransomNote:
            if available_letters.get(char, 0) > 0:
                available_letters[char] -= 1
            else:
                return False
        
        return True
```

Note that in Python, the [Counter](https://docs.python.org/3/library/collections.html#collections.Counter) collection could be used to count maganize's characters instead:

```python
from collections import Counter

def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        available_letters = Counter(magazine)
        
        for char in ransomNote:
            if available_letters.get(char, 0) > 0:
                available_letters[char] -= 1
            else:
                return False
        
        return True
```

**Complexity Analysis**

- Time Complexity: `O(n + m)`, where `n` is the lengh of `magazine` and `m` is the length of `ransomNote`.
- Space Complexity: `O(k)`, where `k` is the number of unique characters in `magazine`.

## Climbing Stairs
[LeetCode Question](https://leetcode.com/problems/climbing-stairs/description/)

#### Solutions:

1. **Dynamic Programming**: Solves the problem by identifying a recurrence relation that represents the number of ways to climb the stairs, optimized to use constant space.

#### Dynamic Programming

This problem is a classic application of dynamic programming. To climb a staircase with `n` steps, one can either take a single step from step `(n-1)` or take two steps from step `(n-2)`. Thus, the total ways to reach step `n` is the sum of the ways to reach step `(n-1)` and `(n-2)`. For example, to reach the first step, there is only one way: climb 1 step. However, to reach the second step, there are two ways: climb the first step and then take another step or climb 2 steps directly. Therefore, we can generalize the recurrence relation as:  

$\text{ways}(n) = \text{ways}(n - 1) + \text{ways}(n - 2)$

**Steps:**

1. Initialize two variables:  
   - `prev_step` (ways to reach step `0`) = `0`.  
   - `curr_step` (ways to reach step `1`) = `1`.  
2. For each step up to `n`, update `prev_step` and `curr_step` to represent the ways to reach the current step using the recurrence relation.  
3. Return `curr_step`, which represents the total ways to reach the `n`-th step.  


```python
def climbStairs(self, n: int) -> int:
        prev_step, curr_step = 0, 1
      
        for _ in range(n):
            prev_step, curr_step = curr_step, prev_step + curr_step
      
        return curr_step
```

**Complexity Analysis**

- Time Complexity: `O(n)`, where `n` is the number of steps.
- Space Complexity: `O(1)`, as both variables `prev_step` and `curr_step` do not grow with the input size.

## Longest Palindrome
[LeetCode Question](https://leetcode.com/problems/longest-palindrome/description/)

#### Solutions:

1. **Greedy Algorithm**: Counts character occurrences and determines the longest possible palindrome length by leveraging the symmetry properties of palindromes. 

#### Greedy Algorithm

A [palindrome](https://en.wikipedia.org/wiki/Palindrome) is symmetrical around its center. This means that every character in the palindrome must appear an even number of times, except for one character (at most), which can serve as the center of the palindrome when its length is odd.  

**Steps:**

1. Use a `Counter` to count the occurrences of each character in the string.  
2. Iterate over the counts of each character:  
   - Add all even counts directly to the palindrome length.  
   - For odd counts, add the largest even part (`count - 1`) and, if the palindrome has no center yet, add 1 to accommodate the odd character in the center.  
3. Return the calculated palindrome length.  

```python
from collections import Counter

def longestPalindrome(self, s: str) -> int:
        count = Counter(s)
        longest_palindrome = 0

        for v in count.values():
            longest_palindrome += v - (v % 2)
            
            if longest_palindrome % 2 == 0 and v % 2 == 1:
                longest_palindrome += 1
        
        return longest_palindrome
```

**Complexity Analysis**

- Time Complexity: `O(n + k)`
  - `O(n)` for counting characters in the string using `Counter`.
  - `O(k)` to iterate over the counts, where `k` is the number of distinct characters in the string.
- Space Complexity: `O(n)`, since the `Counter` dictionary stores up to `k` keys, where `k` is the number of distinct characters in `s`.

## Reverse Linked List
[LeetCode Question](https://leetcode.com/problems/reverse-linked-list/description/)

#### Solutions:

1. **Recursion**: Uses a call stack to reverse the nodes by adjusting the pointers during the unwinding phase of the recursion.  
2. **Iterative**: Iterates through the list while rearranging the pointers to reverse the list in place.  

#### Recursion

This approach uses the recursive stack to reverse the linked list. The base case occurs when the list is empty (`head is None`) or has a single node (`head.next is None`). During the recursive backtracking, the links are reversed:

- `head.next.next = head` reverses the link to point back to the current node.  
- `head.next = None` disconnects the original forward link, preventing cycles.  

#### Example Walkthrough


- Input: `1 -> 2 -> 3 -> None`
- Call Stack:  
  1. `reverseList(1)` → Calls `reverseList(2)`.  
  2. `reverseList(2)` → Calls `reverseList(3)`.  
  3. `reverseList(3)` → Base case reached, returns `3`.  
- Backtracking:  
  1. Reverses `2 -> 3` to `3 -> 2`, disconnecting `2 -> 3`.  
  2. Reverses `1 -> 2` to `2 -> 1`, disconnecting `1 -> 2`.
- Output: `3 -> 2 -> 1 -> None`. 

```python
def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head is None or head.next is None:
            return head
        
        res = self.reverseList(head.next)
        head.next.next = head
        head.next = None
        
        return res
```

**Complexity Analysis**

- Time Complexity: `O(n)`, where `n` is the number of nodes in the list. Each node is visited once.
- Space Complexity: `O(n)`, due to the recursive call stack.

#### Iterative

This solution iteratively adjusts the next pointers of the nodes to reverse the list. A `dummy_node` helps track the head of the reversed list.

**Steps:**
1. Traverse the original list while maintaining a reference to the next node.
2. Redirect the next pointer of the current node to the reversed portion.
3. Update the head of the reversed list to the current node.
4. Proceed to the next node in the original list.

```python
def reverseList(self, head: ListNode) -> ListNode:
        dummy_node = ListNode()      
        current_node = head
      
        while current_node is not None:
            next_node = current_node.next
          
            current_node.next = dummy_node.next
            dummy_node.next = current_node
          
            current_node = next_node
      
        return dummy_node.next
```

**Complexity Analysis**

- Time Complexity: `O(n)`, where `n` is the number of nodes in the list.
- Space Complexity: `O(1)`, as no extra space is used apart from a few pointers.

## Majority Element
[LeetCode Question](https://leetcode.com/problems/majority-element/description/)

#### Solutions:

1. **Moore Voting Algorithm**

#### Moore Voting Algorithm

The [Moore Voting Algorithm](https://en.wikipedia.org/wiki/Boyer%E2%80%93Moore_majority_vote_algorithm) efficiently finds the majority element in a list, assuming that the majority element exists. The algorithm works by maintaining a `candidate` for the majority element and a `count` to track its dominance:  

- If `count` is `0`, update the candidate to the current number and set `count` to `1`.  
- If the current number equals the candidate, increment `count`.  
- Otherwise, decrement `count`.  

```python
def majorityElement(self, nums: List[int]) -> int:
        count = 0
        candidate = None

        for num in nums:
            if count == 0:
                candidate = num
                count += 1
            elif num == candidate:
                count += 1
            else:
                count -= 1
        
        return candidate
```

**Complexity Analysis**

- Time Complexity: `O(n)`, where `n` is the length of the `nums` array.
- Space Complexity: `O(1)`, as only two variables (`count` and `candidate`) are used.

## Add Binary
[LeetCode Question](https://leetcode.com/problems/add-binary/description/)

#### Solutions:

1. **Two Pointers:** Use two pointers to loop through the binary numbers and sum them.

#### Two Pointers

This solution mimics the manual addition of binary numbers by starting from the least significant bit (rightmost) and working toward the most significant bit (leftmost), while handling a carry. The key idea is to sum corresponding bits of the two binary strings `a` and `b` along with a carry, adjusting for differing lengths of the strings.

**Algorithm Steps**:  

1. Use two pointers `i` and `j` to iterate from the end of `a` and `b` toward the beginning.
2. At each step, calculate the sum of the current bits and the carry.
3. Compute the binary digit for the current position using modulo operation (`% 2`) and determine the carry for the next step using integer division (`// 2`). Note that [divmod](https://docs.python.org/3/library/functions.html#divmod) is used since it returns a tuple containing the quotient and the remainder. 
4. Append the calculated digit to the result list.
5. After finishing both strings, append any remaining carry.
6. Reverse the result list and join it to form the final binary string.

```python
def addBinary(self, a: str, b: str) -> str:
        ans = []  
        i, j, carry = len(a) - 1, len(b) - 1, 0

        while i >= 0 or j >= 0 or carry:
            carry += (0 if i < 0 else int(a[i])) + (0 if j < 0 else int(b[j]))

            carry, v = divmod(carry, 2)
            ans.append(str(v))
            i, j = i - 1, j - 1
      
        return "".join(ans[::-1])
```

**Complexity Analysis**

- Time Complexity: `O(n)`, where `n` is the length of the longer binary string (`a` or `b`).
- Space Complexity: `O(n)`, due to the storage required for the `ans` list that contains the result.

## Diameter of Binary Tree
[LeetCode Question](https://leetcode.com/problems/diameter-of-binary-tree/description/)

#### Solutions:

1. **Depth-First Search (DFS):** Transverse the tree, calculting the depth (or height) of each subtree, finding the max (global) diameter.

#### Depth-First Search (DFS)

The Diameter of a Binary Tree is defined as the length of the longest path between any two nodes in the tree, where the path may or may not pass through the root. Using DFS, we can calculate this efficiently.  

**Key Idea**:  

- For each node, the diameter passing through it is the sum of the depths of its left and right subtrees.
- The global diameter is the maximum diameter across all nodes.  
- During DFS, calculate and return the depth of the current subtree, while updating a global variable `max_diameter` to store the maximum diameter encountered.  

**Algorithm Steps**:  

1. Traverse the tree using DFS.  
2. For each node:  
   - Recursively calculate the depth of its left and right subtrees.  
   - Compute the diameter through this node as `left_depth + right_depth`.  
   - Update the global `max_diameter` if this is the largest diameter seen so far.  
3. Return the depth of the current subtree to the parent node as `1 + max(left_depth, right_depth)`.  


```python
def diameterOfBinaryTree(self, root: Optional[TreeNode]) -> int:
        def dfs(node):
            if node is None:
                return 0
            
            nonlocal max_diameter

            left_depth = dfs(node.left)
            right_depth = dfs(node.right)

            max_diamater = max(max_diameter, left_depth + right_depth)

            return 1 + max(left_depth, right_depth)

        max_diameter = 0
        dfs(root)

        return max_diameter
```

**Complexity Analysis**

- Time Complexity: `O(n)`, where `n` is the number of nodes in the binary tree.
- Space Complexity: `O(h)`, where `h` is the height of the binary tree.

## Middle of the Linked List
[LeetCode Question](https://leetcode.com/problems/middle-of-the-linked-list/description/)

#### Solutions:

1. **Two Pointers:** Utilizes the tortoise and hare algorithm to find the middle of the linked list.

#### Two Pointers

This approach utilizes the [tortoise and hare" algorithm](#linked-list-cycle), where two pointers, `slow` and `fast`, traverse the list at different speeds. The `slow` pointer moves one step at a time, while the `fast` pointer moves two steps. When the `fast` pointer reaches the end of the list (or becomes `None`), the `slow` pointer will be positioned at the middle node.  

```python
def middleNode(self, head: Optional[ListNode]) -> Optional[ListNode]:
        slow, fast = head, head

        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        
        return slow
```

**Complexity Analysis**

- Time Complexity: `O(n)`, where `n` is the number of nodes in the linked list.
- Space Complexity: `O(1)`, since the two additional pointers occupy a constant space.

## Maximum Depth of Binary Tree
[LeetCode Question](https://leetcode.com/problems/maximum-depth-of-binary-tree/)

#### Solutions:

1. **Depth-First Search (DFS)**: A recursive approach to calculate the depth of the tree by traversing its left and right subtrees.  

#### Depth-First Search (DFS)

The **maximum depth** of a binary tree is the number of nodes along the longest path from the root node to a leaf node. To solve this problem, we can use a recursive **depth-first search (DFS)** strategy:

1. **Base Case**: If the current node is `None` (i.e., a null reference), return 0.  
2. **Recursive Case**:  
   - Recursively calculate the depth of the left subtree.  
   - Recursively calculate the depth of the right subtree.  
   - The maximum depth from the current node is `1 + max(left_depth, right_depth)`, where `1` accounts for the current node itself.  

This algorithm effectively explores all paths in the binary tree, ensuring the depth of each subtree is calculated. Once all recursive calls are resolved, the final result represents the maximum depth of the entire tree. 

```python
def maxDepth(self, root: Optional[TreeNode]) -> int:
        def dfs(node):
            if node is None:
                return 0
            
            left_depth = dfs(node.left)
            right_depth = dfs(node.right)

            return 1 + max(left_depth, right_depth)
        
        return dfs(root)
```

**Complexity Analysis**

- Time Complexity: `O(n)`, where `n` is the number of nodes in the tree.
- Space Complexity: `O(h)`, where `h` is the height of the tree. In the worst case (skewed tree), this could be `O(n)`; for a balanced tree, it is `O(log n)`.

## Contains Duplicate
[LeetCode Question](https://leetcode.com/problems/contains-duplicate/description/)

#### Solutions:

1. **Counting:** Use a `Counter` to count occurrences of each element and check if any value exceeds 1.  
2. **Set:** Compare the length of the `set` created from the array with the original array length.  
3. **Hash Map:** Use a hash map (or set) to track seen elements during a single loop.  

#### Counting

This approach utilizes the `Counter` class from Python's `collections` module to count the frequency of each element in the array. If any element has a count greater than 1, the array contains duplicates.

```python
from collections import Counter

def containsDuplicate(self, nums: List[int]) -> bool:
        count = Counter(nums)
        
        return any([v > 1 for v in count.values()])
```

**Complexity Analysis**

- Time Complexity: `O(n)`, where `n` is the number of elements in the array. The counting and checking operations both take linear time.
- Space Complexity: `O(h)`, for the `Counter` object storing element frequencies. `n` is the number of elements in the array.

#### Set

By converting the array to a [set](https://docs.python.org/3/library/stdtypes.html#set), duplicates are removed. Comparing the length of the set to the array tells us if duplicates exist.

```python
def containsDuplicate(self, nums: List[int]) -> bool:
    return len(set(nums)) < len(nums)
```

**Complexity Analysis**

- Time Complexity: `O(n)`, where `n` is the number of elements in the array. Converting the array to a set and comparing lengths both take linear time.
- Space Complexity: `O(h)`, where `n` is the length of the array. Space is allocated for the `set` parse operation.

#### Hash Map

This method iterates through the array and uses a hash map (or set) to store seen elements. If an element is already present, we return True.

```python
def containsDuplicate(self, nums: List[int]) -> bool:
        count_map = set()

        for n in nums:
            if n in count_map:
                return True

            count_map.add(n)
        
        return False
```

**Complexity Analysis**

- Time Complexity: `O(n)`, where `n` is the number of elements in the array, as each element is processed once.
- Space Complexity: `O(h)`, for the hash map (set) tracking unique elements. `n` is the number of elements in the array.

## Maximum Subarray
[LeetCode Question](https://leetcode.com/problems/maximum-subarray/description/)

#### Solutions:

1. **Kadane's Algorithm**: An efficient way to find the largest sum of a contiguous subarray by maintaining a running sum and dynamically determining the maximum sum at each step.  

#### Kanade's Algorithm

While iterating through the array, calculate the running sum (`current_sum`) of the subarray ending at each index. If `current_sum` is negative, reset it to the current number, effectively "starting fresh." Maintain a `max_sum` variable to store the maximum subarray sum found so far.  

#### Steps  

1. Initialize `max_sum` to the first element of the array (since the array is guaranteed to have at least one element).  
2. Initialize `current_sum` to the first element as well.  
3. Iterate through the array starting from the second element:  
   - Update `current_sum` to be the larger of the current element or `current_sum + current element`.  
   - Update `max_sum` to be the larger of `max_sum` or `current_sum`.  
4. Return `max_sum` after the loop.  

This ensures that every possible subarray is considered, with efficient handling of negative contributions to the running sum.  

```python
def maxSubArray(self, nums: List[int]) -> int:
        max_sum = current_sum = nums[0]

        for num in nums[1:]:
            current_sum = max(current_sum + num, num)

            max_sum = max(max_sum, current_sum)

        return max_sum
```

**Complexity Analysis**

- Time Complexity: `O(n)`, where `n` is the number of elements in the input array.
- Space Complexity: `O(1)`, since only constant extra space is used to store max_sum and current_sum.

## Insert Interval
[LeetCode Question](https://leetcode.com/problems/insert-interval/description/)

#### Solutions:

1. **Sort & Merge**: Add the new interval to the input array, sort it and then merge overlapping intervals.
2. **Linear Pass**: Add all intervals smaller than the new interval to the result, merge any overlapping interval, and add all intervals greater then the new interval to the result.

--- 

#### [Good To Know 📚] Intervals

Questions related to intervals are quite common and they come down to detect or merge overlapping intervals. Nonetheless, the key to these types of problems is to find the overlap of two intervals.

#### Detecting Overlap

Intervals do not overlap when the end time of te first inverval is earlier (smaller) than the start time of the second interval.

```
x1  x2
|---|

        |---|
        y1  y2

y1  y2
|---|

        |---|
        x1  x2
```

In the example above, `x2 < y1 or y2 < x1`. Therefore, the overlapping condition is the oposite: `not (x2 < y1 or y2 < x1)`.

#### Finding the Overlap Interval

If two intervals, `[x1, x2]` and `[y1, y2]`, overlap, then the overlapping interval can be calculated by `[max(x1, y1), min(x2, y2)]`. 

#### Merging Intervals

To merge overlapping intervals, we first sort them by their start time and then iterate through the sorted list, updating the end time of the last merged interval if an overlap exists.

```python
def merge_intervals(intervals: List[List[int]]) -> List[List[int]]:
    # Sort intervals by their start time
    intervals.sort()

    def overlap(a: List[int], b: List[int]) -> bool:
        return not (b[1] < a[0] or a[1] < b[0])

    res: List[List[int]] = []
    for interval in intervals:
        if not res or not overlap(res[-1], interval):
            res.append(interval)
        else:
            # Merge intervals by getting the bigger end value since the start value is already the smallest b/c intervals are sorted
            res[-1][1] = max(res[-1][1], interval[1])

    return res
```

Time Complexity: O(n*log(n)), due to sorting.
Space Complexity: O(n), since memory is allocated for the `res` array.

---

#### Sort & Merge

This solution appends the new interval to the end of the input array list, sort the input array again and apply the merge algorithm describe above.

```python
def insert(
        self, intervals: List[List[int]], newInterval: List[int]
    ) -> List[List[int]]:
        intervals.append(newInterval)

        intervals.sort()

        def overlap(a, b):
            return not (b[1] < a[0] or a[1] < b[0])
        
        res: List[List[int]] = []

        for interval in intervals:
            if not res or not overlap(res[-1], interval):
                res.append(interval)
            else:
                res[-1][1] = max(res[-1][1], interval[1])
        
        return res
```

**Complexity Analysis**

- Time Complexity: `O(nlog(n)`, due to sorting.
- Space Complexity: `O(n)`, for the result list.

#### Linear Pass

1. Add the intervals whose end time is before the start_time of the enw interval. In other words, we add all the non-overlapping earlier intervals to the response.
2. For those intervals that overlap with the new interval, expand the new interval's start and end times.
3. Add the new interval to the final result
4. For intervals whose start time is after the end of the new interval, add them directly to the final results.

```python
def insert(
        self, intervals: List[List[int]], new_interval: List[int]
    ) -> List[List[int]]:
        res: List[List[int]] = []
        i = 0
        n = len(intervals)
        while i < n and intervals[i][1] < new_interval[0]:
            res.append(intervals[i])
            i += 1

        while i < n and intervals[i][0] <= new_interval[1]:
            new_interval[0] = min(new_interval[0], intervals[i][0])
            new_interval[1] = max(new_interval[1], intervals[i][1])
            i += 1

        res.append(new_interval)

        while i < n:
            res.append(intervals[i])
            i += 1

        return res
```

**Complexity Analysis**

- Time Complexity: `O(n)`, where `n` is the length of the intervals input.
- Space Complexity: `O(n)`, for the result list.

## 01 Matrix
[LeetCode Question](https://leetcode.com/problems/01-matrix/description/)

#### Solutions:

1. **Breadth-First Search (BFS)**: Efficiently calculates the distance of each cell to the nearest `0` using a queue.

#### Breadth-First Search (DFS)

The problem can be solved using BFS to calculate the shortest distance to a `0` for each cell in the matrix. BFS is well-suited for such problems since it explores neighbors layer by layer.

#### Steps:
1. **Initialize Distances**: Create a `distances` matrix of the same size as the input, initializing all cells to `-1`. Set the cells with `0` in the input matrix to `0` in the `distances` matrix.
2. **Queue Setup**: Enqueue all cells that contain `0` since their distance to the nearest `0` is already known.
3. **BFS Traversal**:
   - Dequeue a cell and check its four neighbors (up, right, down, left).
   - If a neighbor hasn’t been visited (distance is `-1`), update its distance as `current cell distance + 1` and enqueue it.
4. **Repeat**: Continue until the queue is empty.
5. **Return Result**: The `distances` matrix will now contain the shortest distance to the nearest `0` for each cell.

```python
from collections import deque

def updateMatrix(self, mat: List[List[int]]) -> List[List[int]]:
        rows, cols = len(mat), len(mat[0])
        distances = [[-1] * cols for _ in range(rows)]
        queue = deque()

        for i in range(rows):
            for j in range(cols):
                if mat[i][j] == 0:
                    distances[i][j] = 0
                    queue.append((i, j))
        
        directions = [(-1, 0), (0, 1), (1, 0), (0, -1)]

        while queue:
            i, j = queue.popleft()

            for delta_row, delta_col in directions:
                neighbour_i = i + delta_row
                neighbour_j = j + delta_col

                if  0 <= neighbour_i < rows and 0 <= neighbour_j < cols and distances[neighbour_i][neighbour_j] == -1:
                    distances[neighbour_i][neighbour_j] = distances[i][j] + 1
                    queue.append((neighbour_i, neighbour_j))
        
        return distances
```

**Complexity Analysis**

- Time Complexity: `O(m * n)`, where m is the number of rows, and n is the number of columns in the matrix.
- Space Complexity: `O(m * n)`, for the `distances` matrix and the BFS queue.

## K Closest Points to Origin
[LeetCode Question](https://leetcode.com/problems/k-closest-points-to-origin/description/)

#### Solutions

1. **Heap**: Use a priority queue to efficiently find the K closest points to the origin.

---
#### [Good To Know 📚] Priority Queue and Heap

[Priority Queue](https://en.wikipedia.org/wiki/Priority_queue) is an abstract data type, and [Heap](https://en.wikipedia.org/wiki/Heap_(data_structure)) is the concrete data structure we use to implement a priority queue.

#### Priority Queue

A priority queue organizes elements based on their priority, rather than their order of arrival. Elements with high priority are served before elements with low priority. In some implementations, if two elements have the same priority, they are served in the same order in which they were enqueued. In other implementations, the order of elements with the same priority is undefined. Operations include:

1. `is_empty`: Check whether the queue has no elements.
2. `insert_with_priority`: Insert an element with a priority.
3. `pull_highest_priority_element`: Remove and return the highest-priority element.

A priority queue can be implemented by:

1. Unsorted Array: Insert is `O(1)` but pulling the highest priority element is `O(n)` as all elements would have to be searched.
2. Sorted Array: Insert is `O(n)` as we would have to loop through to find the correct position, and pulling the highest priority element is `O(1)`.
3. Heap: `O(log(n))` for insert and for pulling the highest priority element.

#### Heap

A heap is a specialized tree-based data structure implementing priority queues. Usually, a binary tree is used (binary heap), but the tree might not always be binary. In particular, k-ary heap (A.K.A. k-heap) is a tree where the nodes have k children. As long the heap properties are satified, it is a heap:

1. Almost complete, i.e. every level is filled except possibly the last (deepest) level. The filled items in the last level are left-justified.
2. For any node,
   - its key (priority) is greater than its parent's key (Min Heap).
   - its key (priority) is smaller than its parent's key (Max Heap).

![Example of a binary max-heap with node keys being integers between 1 and 100](https://github.com/user-attachments/assets/1bae9dd6-52be-46a2-ae07-789c11cc6dc2)

#### Heap Operations

**Insert**

To insert a key into a heap, place the new key at the first free leaf. If property #2 is violated, perform a bubble-up:

```python
def bubble_up(node):
    while node.parent and node.parent.key > node.key:
        # swap node and node.parent
        node = node.parent
```

As the name of the algorithm suggests, it "bubbles up" the new node by swapping it with its parent until the order is correct. Since the height of a heap is `O(log(n))`, the complexity of bubble-up is `O(log(n))`.

**Pull Highest Priority Element**

What this operation does is:

1. Delete a node with min key and return it
2. Reorganize the heap so the two properties still hold

To do that, we:

1. Remove and return the root since the node with the minimum key is always at the root
2. Replace the root with the last node (the rightmost node at the bottom) of the heap
3. If property #2 is violated, perform a bubble-down

```python
def bubble_down(node):
    while node is not a leaf:
        smallest_child = child of node with smallest key
        if smallest_child < node:
            swap node and smallest_child
            node = smallest_child
        else:
            break
```

What this says is we keep swapping between the current node and its smallest child until we get to the leaf, hence a "bubble down". Again the time complexity is `O(log(N))` since the height of a heap is `O(log(N))`.

#### Implementation

Being a complete tree makes an array a natural choice to implement a heap since it can be stored compactly and no space is wasted. Pointers are not needed. The parent and children of each node can be calculated with index arithmetic.

For node `i`, its children are stored at `2i+1` and 2i+2, and its parent is at `floor((i-1)/2)`. So instead of `node.left` we'd do `2*i+1`. (Note that if we are implementing a k-ary heap, then the childrens are at `ki+1` to `ki+k`, and its parent is at `floor((i-1)/k)`.)

This is exactly how the [python language implements heaps](https://github.com/python/cpython/blob/d3a8d616faf3364b22fde18dce8c168de9368146/Lib/heapq.py#L263).

**Heap in Python**

Python comes with a built-in `heapq` we can use, and it is a **min heap**, i.e. element at top is smallest.

`heapq.heappush` takes two arguments, the first is the heap (an array/list) we want to push the element into, and the second argument can be anything as long as it can be used for comparison. Typically, we push a tuple since Python tuples are compared position by position. For example, `(1, 10)` is smaller than `(2, 0)` because the first element in `(1, 0)` is smaller than the first element in `(2, 0)`. `(1, 10)` is smaller than `(1, 20)` because when the first elements are equal, we compare the next one. In this case, 10 is smaller than 20.

`heapq.heappop` takes a single argument heap, and pop and return the smallest element in the heap.

```python
import heapq
>>> h = []
>>> heapq.heappush(h, (5, 'write code'))
>>> heapq.heappush(h, (7, 'release product'))
>>> heapq.heappush(h, (1, 'write spec'))
>>> heapq.heappush(h, (3, 'create tests'))
>>> heapq.heappop(h)
(1, 'write spec')
>>> h[0]
>>> (3, 'create tests')
```

The simplest way to implement a max heap is to reverse the sign of the number before we push it into the heap and reverse it again when we pop it out.

```python
import heapq
>>> h = []
>>> heapq.heappush(h, -1 * 10)
>>> heapq.heappush(h, -1 * 30)
>>> heapq.heappush(h, -1 * 20)
>>> print(-1 * heapq.heappop(h))
30
>>> print(-1 * h[0])
20
```

If the list is known beforehand, we can create a heap out of it by simply using `heapify`, which is actually an `O(N)` operation.

```python
import heapq
arr = [3, 1, 2]
heapq.heapify(arr)
```
---

#### Heap

This solution builds a min heap where the key (priority) is the distance to the origin. The distance to the origin can be found using the Euclidean distance provided in the problem description: `√(x1 - x2)2 + (y1 - y2)2`. After the heap is built, we need to pop the top `k` elements off the heap, which will be the `k` closest points to the origin.

```python
from heapq import heappop, heappush

def kClosest(self, points: List[List[int]], k: int) -> List[List[int]]:
        heap: List[Tuple[int, List[int]]] = []

        for point in points:
            heappush(heap, (point[0] ** 2 + point[1] ** 2, point))

        res: List[List[int]] = []

        for _ in range(k):
            priority, value = heappop(heap)
            res.append(value)

        return res
```

Note that the distance we stored was not the Euclidean distance. Instead, we did not take the square root because it is more accurate when the stored value is an integer.

**Complexity Analysis**

- Time Complexity: `O(n * log(n))`, where `n` is the number of points. Insertion into the heap is `O(log(n))`, done `n` times.
- Space Complexity: `O(n)`, for the heap.

## Longest Substring Without Repeating Characters
[LeetCode Question](https://leetcode.com/problems/longest-substring-without-repeating-characters/description/)

#### Solutions

1. **Brute Force:** Check every single substring and count the ones with non-repeating characters.
2. **Sliding Window:** Use a flexible window to track the longest substring while ensuring characters remain unique.

---
#### [Good To Know 📚] Sliding Window

Sliding Window Technique is a method used to efficiently solve problems that involve defining a window or range in the input data (arrays or strings) and then moving that window across the data to perform some operation within the window. This technique is commonly used in algorithms like finding subarrays with a specific sum, finding the longest substring with unique characters, or solving problems that require a fixed-size window to process elements efficiently.

#### Fixed Size Sliding Window

For problems involving a fixed size, slide a window of size `k` across the input, adjusting computations as the window moves.

```python
def sliding_window_fixed(input, window_size):
    ans = window = input[0:window_size]
    for right in range(window_size, len(input)):
        left = right - window_size
        remove input[left] from window
        append input[right] to window
        ans = optimal(ans, window)
    return ans
```

#### Variable Sliding Window

For problems like this one, dynamically adjust the window size based on validity conditions. Increase the right pointer to expand the window and, if invalid, adjust the left pointer to shrink it.
   
```python
def sliding_window_flexible_longest(input):
    initialize window, ans
    left = 0
    for right in range(len(input)):
        append input[right] to window
        while invalid(window):        # update left until window is valid again
            remove input[left] from window
            left += 1
        ans = max(ans, window)        # window is guaranteed to be valid here
    return ans
```
---

#### Brute Force

Iterate through all possible substrings and check for unique characters. If a substring is valid, update the longest substring length.

```python
def lengthOfLongestSubstring(self, s: str) -> int:
        n = len(s)
        longest = 0

        for start in range(n):
            for end in range(n):
                sub = s[start : end + 1]
                if len(set(sub)) == len(sub):
                    longest = max(longest, end + 1 - start)

        return longest
```

**Complexity Analysis**

- Time Complexity: `O(n^2)`, where `n` is the length of the string `s`.
- Space Complexity: `O(n)`, due to the set used to track unique characters in each substring.


#### Sliding Window

Using a hashmap, track the index of each character’s last occurrence. Adjust the left pointer directly to skip duplicate regions efficiently.

```python
def lengthOfLongestSubstring(self, s: str) -> int:
        char_map = {}
        left = 0
        longest = 0

        for right, char in enumerate(s):
            if char in char_map and char_map[char] >= left:
                left = char_map[char] + 1

            char_map[char] = right
            longest = max(longest, right - left + 1)

        return longest
```

**Complexity Analysis**

- Time Complexity: `O(n)`, as each character is processed once.
- Space Complexity: `O(k)`, where `k` is the size of the character set (limited by the number of unique characters in `s`).

## 3Sum
[LeetCode Question](https://leetcode.com/problems/3sum/description/)

#### Solutions:

1. **Two Pointers**: Sort the input array, and for each number, use two pointers to go through the array searching for the triples.

#### Two Pointers

#### Steps:
1. **Sort the Array:** Sorting ensures the array is in non-decreasing order, which simplifies duplicate handling and allows for efficient two-pointer traversal.
2. **Iterate with a Loop:**
   - Select the first number of the triplet `nums[i]`
   - Skip duplicates to ensure unique triplets.
   - Stop early if `nums[i] > 0` since the sorted array guarantees subsequent sums will be positive.
3. **Use Two Pointers:**
   - Place one pointer at the next element `left = i + 1` and the other at the end of the array `right = n - 1`
   - Compute the sum `current_sum = nums[i] + nums[left] + nums[right]`.
   - Adjust pointers based on the sum:
     - **If `current_sum < 0`:** Increment `left` to increase the sum.
     - **If `current_sum > 0`:** Decrement `right` to decrease the sum.
     - **If `current_sum = 0`:** Append the triplet and move both pointers, skipping duplicates.
4. **Repeat:** Continue until `left < right`.


```python
def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums.sort()

        triplets = []
        n = len(nums)

        for i in range(n - 2):
            # If current number is bigger than 0, then all following numbers are also greater than 0, since array is sorted.
            # This means they won't add up to zero.
            if nums[i] > 0:
                break

            # Skip Duplicates
            if i > 0 and nums[i] == nums[i - 1]:
                continue

            left = i + 1
            right = n - 1

            while left < right:
                current_sum = nums[i] + nums[left] + nums[right]

                if current_sum < 0:
                    left += 1
                elif current_sum > 0:
                    right -= 1
                else:
                    triplets.append([nums[i], nums[left], nums[right]])

                    # Move pointers to continue search
                    left, right = left + 1, right - 1

                    # Skip Duplicates
                    while left < right and nums[left] == nums[left - 1]:
                        left += 1

                    # Skip Duplicates
                    while left < right and nums[right] == nums[right + 1]:
                        right -= 1

        return triplets
```

**Complexity Analysis**

- Time Complexity: `O(n^2)`.
  - Sorting takes `O(n log(n))`
  - The two pointer loop runs `O(n)` for each iteration of the outer loop, leading to `O(n^2)` overall.
- Space Complexity: `O(log(n))`.
  - Sorting uses `O(log(n))` space for recursion in standard algorithms like QuickSort.
  - Ignoring output space, no additional space is used.

## Binary Tree Level Order Traversal
[LeetCode Question](https://leetcode.com/problems/binary-tree-level-order-traversal/)

#### Solutions:

1. **Breadth-First Search (BFS)**: Use BFS strategy to tranverse the tree horizontally, grouping the nodes of the same level.

#### Breadth-First Search (BFS)

To perform a level order traversal, we use the **Breadth-First Search (BFS)** algorithm, which processes nodes level by level. A queue is the ideal data structure for BFS since it ensures nodes are processed in the correct order.

#### Steps:
1. **Base Case:** If the tree is empty `root = None`, return an empty list.
2. **Initialization:**
   - Use a `queue` (implemented as a deque) to store nodes at each level, starting with the root node.
   - Create an empty list `res` to store the level-by-level traversal.
3. **Processing Levels:**
   - While the queue is not empty:
     - Determine the size of the current level `len(queue)`.
     - Initialize a temporary list `level` to hold node values for the current level.
     - For each node in the level:
       - Dequeue a node, add its value to `level`.
       - Enqueue the node's left and right children if they exist.
     - Append the `level` list to `res`.
4. **Return Results:** After processing all levels, return the `res` list.


```python
from collections import deque

def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        res = []

        if not root:
            return res

        queue = deque([root])

        while queue:
            level = []

            for _ in range(len(queue)):
                node = queue.popleft()
                level.append(node.val)

                for child in [node.left, node.right]:
                    if child is not None:
                        queue.append(child)

            res.append(level)

        return res
```

**Complexity Analysis**

- Time Complexity: `O(n)`, where `n` is the total number of nodes in the tree.
- Space Complexity: `O(w)`, where `w` is the maximum width of the tree, i.e., the largest number of nodes at any level.

## Clone Graph
[LeetCode Question](https://leetcode.com/problems/clone-graph/description/)

#### Solutions:

1. **Depth-First Search (DFS)**: Use DFS strategy to tranverse the tree recursively, cloning the adjacent nodes.

#### Depth-First Search (DFS)

The problem requires cloning a graph such that every node in the original graph has an exact copy in the cloned graph, with the same structure and neighbor relationships. This can be achieved using **Depth-First Search (DFS)**.

#### Steps:
1. **Initialize Data Structures:**
   - A dictionary `visited` is used to map each original node to its cloned counterpart. This prevents cycles and redundant cloning in the presence of undirected graphs.

2. **DFS Functionality:**
   - If the `node` is `None`, return `None` (base case for an empty graph).
   - If the `node` has already been visited (exists in `visited`), return its clone.
   - Otherwise, create a new clone of the `node`, store it in `visited`, and recursively clone its neighbors.

3. **Clone Neighbors:**
   - Iterate through the neighbors of the current `node`.
   - Use the DFS function to clone each neighbor, appending it to the cloned node's neighbors list.

4. **Return the Result:**
   - Once all nodes are visited and cloned, the root of the cloned graph is returned.

```python
from collections import defaultdict
from typing import Optional

def cloneGraph(self, root: Optional["Node"]) -> Optional["Node"]:
        visited = defaultdict(Node)

        def dfs(node):
            if node is None:
                return None

            if node in visited:
                return visited[node]

            clone_node = Node(node.val)
            visited[node] = clone_node

            for neighbor in node.neighbors:
                clone_node.neighbors.append(dfs(neighbor))

            return clone_node

        return dfs(root)
```

**Complexity Analysis**

- Time Complexity: `O(n + m)`, where `n` is the number of nodes and `m` is the number of edges. Each node is visited once, and all edges are traversed during neighbor processing.
- Space Complexity: `O(n)`, where `n` is the number of nodes in the graph.

## Evaluate Reverse Polish Notation
[LeetCode Question](https://leetcode.com/problems/evaluate-reverse-polish-notation/description/)

#### Solutions:

1. **Stack**: To evaluate a Reverse Polish Notation (RPN) expression, we use a **stack**. Each numeric token is pushed onto the stack, and operators pop operands from the stack to compute the result.

#### Stack

#### Steps:

1. **Initialize Stack**:
   - Use a list to act as a stack (`number_stack`) for storing numeric values during computation.

2. **Iterate Through Tokens**:
   - If the token is a number, convert it to an integer and push it onto the stack.
   - If the token is an operator (`+`, `-`, `*`, `/`), perform the following:
     - Pop the top two numbers from the stack (`num2` and `num1`).
     - Apply the operator, treating `num1` as the left operand and `num2` as the right operand.
     - For division (`/`), ensure the result truncates towards zero using `int(float(num1) / num2)`.

3. **Push Result Back**:
   - After applying the operator, push the result back onto the stack.

4. **Final Result**:
   - Once all tokens are processed, the stack will contain a single value, which is the result of the RPN expression.

```python
def evalRPN(self, tokens: List[str]) -> int:
    number_stack = []
    
    for token in tokens:
        if token.isdigit() or (len(token) > 1 and token[0] == '-'):
            # Push number to stack
            number_stack.append(int(token))
        else:
            # Pop two numbers for the operator
            num2 = number_stack.pop()
            num1 = number_stack.pop()
            
            if token == '+':
                number_stack.append(num1 + num2)
            elif token == '-':
                number_stack.append(num1 - num2)
            elif token == '*':
                number_stack.append(num1 * num2)
            elif token == '/':
                # Integer division truncating towards zero
                number_stack.append(int(float(num1) / num2))
    
    return number_stack[0]
```

**Complexity Analysis**

- Time Complexity: `O(n)`, where `n` is the number of tokens in the input list tokens.
- Space Complexity: `O(n)`, where `n` is the number of nodes in the graph.

## Course Schedule
[LeetCode Question](https://leetcode.com/problems/course-schedule/)

#### Solutions:

1. **Topological Sort (DFS):** Utilizes DFS to find conflicting courses
2. **Topological Sort (Kahn's Algorithm):** Utilizes Kahn's Algorithm to find conflicting courses

---
#### [Good To Know 📚] Topological Sort

[Topological sort](https://en.wikipedia.org/wiki/Topological_sorting) is the ordering of nodes in a [directed graph](https://en.wikipedia.org/wiki/Directed_graph) such that every node appears before all nodes it points to. This is useful in scenarios like task scheduling. 

- **Key Properties**:
  - A graph with cycles cannot have a valid topological ordering.
  - Topological sort is not unique; multiple valid orders can exist.

#### Topological Sort - Kahn's Algorithm

This algorithm is based on **Breadth-First Search (BFS)** and uses the concept of **in-degrees** (the number of incoming edges to a node).

#### **Steps**:
1. Build a graph and compute in-degrees of all nodes.
2. Add all nodes with zero in-degrees to a queue.
3. While the queue is not empty:
   - Remove a node from the queue, add it to the topological order.
   - Reduce the in-degree of its neighbors by 1.
   - Add neighbors with zero in-degrees to the queue.
4. If all nodes are processed, a valid topological order exists; otherwise, the graph contains a cycle.

#### Implementation

```
from collections import deque

def find_indegree(graph):
    indegree = { node: 0 for node in graph }  # dict
    for node in graph:
        for neighbor in graph[node]:
            indegree[neighbor] += 1

    return indegree


def topo_sort(graph):
    res = []
    q = deque()
    indegree = find_indegree(graph)
    for node in indegree:
        if indegree[node] == 0:
            q.append(node)

    while len(q) > 0:
        node = q.popleft()
        res.append(node)
        for neighbor in graph[node]:
            indegree[neighbor] -= 1
            if indegree[neighbor] == 0:
                q.append(neighbor)

    return res if len(graph) == len(res) else None
```

#### Topological Sort - DFS

This approach uses **Depth-First Search (DFS)** to detect cycles and find a valid topological order.

**Steps:**
1. Represent each node's state as:
   - **TO_VISIT:** Node has not been visited.
   - **VISITING:** Node is being visited (part of the current DFS path).
   - **VISITED:** Node has been fully processed.
2. Perform DFS for each node:
   - Mark it as **VISITING**.
   - Recursively visit all its neighbors.
   - If a neighbor is already **VISITING**, a cycle is detected.
   - Otherwise, mark the node as **VISITED** after processing all its neighbors.
3. If no cycles are detected, the nodes can be ordered topologically.

```python
from enum import Enum
from typing import List, Dict

class State(Enum):
    TO_VISIT = 0
    VISITING = 1
    VISITED = 2

def topo_sort_dfs(graph: Dict[str, List[int]]) -> bool:
    states = [State.TO_VISIT for _ in range(len(graph)]

    def dfs(start):
        # mark self as visiting
        states[start] = State.VISITING

        for next_vertex in graph[start]:
            # ignore visited nodes
            if states[next_vertex] == State.VISITED:
                continue

            # revisiting a visiting node, CYCLE!
            if states[next_vertex] == State.VISITING:
                return False

            # recursively visit neighbours
            # if a neighbour found a cycle, we return False right away
            if not dfs(next_vertex):
                return False

        # mark self as visited
        states[start] = State.VISITED

        # if we have gotten this far, our neighbours haven't found any cycle, return True
        return True

    return dfs(0)
```
---

#### Topological Sort (DFS)

```python
from typing import List
from enum import Enum
from collections import defaultdict

class State(Enum):
    TO_VISIT = 0
    VISITING = 1
    VISITED = 2


def is_valid_course_schedule(n: int, prerequisites: List[List[int]]) -> bool:
    def build_graph():
        graph = defaultdict(list)
        
        for src, dest in prerequisites:
            graph[src].append(dest)
        
        return graph
    
    def dfs(start, states):
        states[start] = State.VISITING
        
        for next_vertex in graph[start]:
            if states[next_vertex] == State.VISITED:
                continue
                
            if states[next_vertex] == State.VISITING:
                return False
            
            if not dfs(next_vertex, states):
                return False
       
        states[start] = State.VISITED
        return True
        
    graph = build_graph()
    states = [State.TO_VISIT for _ in range(n)]
    
    for i in range(n):
        if not dfs(i, states):
            return False
    
    return True
```

**Complexity Analysis**

- Time Complexity: `O(n + m)`, for traversing nodes and edges during DFS.
- Space Complexity: `O(n)`, for the graph and recursion stack.

#### Topological Sort (Kahn's Algorithm)

```python
from collections import defaultdict, deque

def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        graph = defaultdict(list)
        indegrees = [0] * numCourses

        for course, prereq in prerequisites:
            graph[prereq].append(course)
            indegrees[course] += 1

        queue = deque([i for i in range(numCourses) if indegrees[i] == 0])

        completed_count = 0

        while queue:
            course = queue.popleft()
            completed_count += 1

            for successor in graph[course]:
                indegrees[successor] -= 1

                if indegrees[successor] == 0:
                    queue.append(successor)

        return completed_count == numCourses
```

**Complexity Analysis**

- Time Complexity: `O(V + E)`, where `𝑉` is the number of courses (nodes), and `𝐸` is the number of dependencies (edges).
- Space Complexity: `O(V + E)`, for the graph and queue.

## Implement Trie (Prefix Tree)
[LeetCode Question](https://leetcode.com/problems/implement-trie-prefix-tree/description/)

---
#### [Good To Know 📚] Trie

A [Trie](https://en.wikipedia.org/wiki/Trie) (pronounced "try") is a tree-like data structure used to store collections of strings. It allows efficient insertions, lookups, and prefix-based searches, making it useful in applications like autocomplete, spellcheckers, and dictionaries.

- **Structure**:
  - Each node represents a single character.
  - Words are formed by traversing paths from the root.
  - An additional flag, `is_end_of_word`, marks the completion of a word.

![image](https://github.com/user-attachments/assets/7d9d6483-c7ff-4301-bb5f-e2c38a58db95)

- **Example**:
  For words `dog`, `cat`, and `cap`:
  - The root branches into `d` and `c`.
  - From `d`, the branch continues to `o` and `g` for "dog."
  - From `c`, it branches to `a`, then to `t` ("cat") and `p` ("cap").
---

#### Solution

1. `__init__`:
   - Initializes the Trie.
   - `children`: A dictionary to store child nodes (key: character, value: Trie node).
   - `is_end_of_word`: A boolean indicating if a node marks the end of a word.
2. `_search_prefix`:
   - Traverses the Trie for the given `prefix`.
   - Returns the last node of the prefix if it exists; otherwise, returns `None`.
   - Use case: Shared by both `search` and s`tartsWith` methods to avoid redundant traversal logic.
3. `insert`:
   - Inserts a word into the Trie.
   - For each character in the word:
     - Checks if the character exists in `children`. If not, creates a new node.
     - Moves to the next node.
   - Marks the final node as the end of a word (`is_end_of_word` = True).
4. `search`:
   - Checks if a word exists in the Trie.
   - Uses `_search_prefix` to find the node for the word.
   - Verifies that the node exists and marks the end of a word.
5.`startsWith`:
   - Checks if any words in the Trie start with the given prefix.
   - Uses `_search_prefix` to verify the existence of the prefix path.

```python
class Trie:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False

    def _search_prefix(self, prefix: str):
        node = self

        for char in prefix:
            if node.children.get(char, None) is None:
                return None

            node = node.children[char]

        return node

    def insert(self, word: str) -> None:
        node = self

        for char in word:
            if char not in node.children:
                node.children[char] = Trie()

            node = node.children[char]

        node.is_end_of_word = True

    def search(self, word: str) -> bool:
        node = self._search_prefix(word)

        return node is not None and node.is_end_of_word

    def startsWith(self, prefix: str) -> bool:
        node = self._search_prefix(prefix)

        return node is not None
```

**Complexity Analysis**

- Time Complexity:
  - **insert:** `O(n)`. Traverses the Trie, where `n` is the length of the word being inserted.
  - **search:** `O(n)`. Traverses up to `n` nodes for the word and checks is_end_of_word.
  - **startsWith:** `O(m)`. Traverses up to `m`, where `m` is the length of the prefix.
  - **_search_prefix:** `O(p)`, where `p` is the length of the prefix. It is traversed only once for each method call.
- Space Complexity: `O(c)`, is the number of characters across all words.

## Coin Change
[LeetCode Question](https://leetcode.com/problems/coin-change/)

#### Solutions

#### DFS + Memoization

```python
from math import inf


class Solution:
    def minCoins(self, coins, amount, sum, memo):
        if sum == amount:
            return 0

        if sum > amount:
            return inf

        if memo[sum] != -1:
            return memo[sum]

        ans = inf

        for coin in coins:
            result = self.minCoins(coins, amount, sum + coin, memo)

            if result == inf:
                continue

            ans = min(ans, result + 1)

        memo[sum] = ans
        return ans

    def coinChange(self, coins: List[int], amount: int) -> int:
        memo = [-1] * (amount + 1)
        result = self.minCoins(
            coins,
            amount,
            0,
            memo
        )
        return result if result != inf else -1
```

**Complexity Analysis**

- Time Complexity: `O(n * amount)`, where `n` is the number of coins, and `amount` is the target value.
- Space Complexity: `O(amount)`, due to the memoization array and recursive stack.

#### Dynamic Programming

```python
from math import inf


class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        if amount == 0:
            return 0

        dp = [inf] * (amount + 1)
        dp[0] = 0

        for i in range(1, amount + 1):
            for coin in coins:
                if i - coin >= 0:
                    dp[i] = min(dp[i - coin] + 1, dp[i])

        return dp[-1] if dp[-1] < inf else -1
```

**Complexity Analysis**

- Time Complexity: `O(n * amount)`), where `n` is the number of coins.
- Space Complexity: `O(amount)`, due to the `dp` array.

## Product of Array Except Self
[LeetCode Question](https://leetcode.com/problems/product-of-array-except-self)

#### Solution

1. Initialization:
   - Create an array `answer` initialized to `1`. This array will hold the result.
   - Initialize two variables, `left` and `right`, to `1`. These track the running product of elements to the left and right of the current index.
2. First Pass (Left Products):
   - Traverse the input array from left to right.
   - For each index `i`, set `answer[i]` to the current value of `left`.
   - Update `left` by multiplying it with `nums[i]`.
3. Second Pass (Right Products):
   - Traverse the input array from right to left.
   - Multiply the current value in `answer[i]` by `right`, which holds the product of elements to the right of `i`.
   - Update `right` by multiplying it with `nums[i]`.
4. Return Result:
   - The `answer` array now contains the product of all elements except the one at each index.

```python
def productExceptSelf(self, nums: List[int]) -> List[int]:
        num_length = len(nums)
      
        answer = [1] * num_length
      
        left = 1
        for i in range(num_length):
            answer[i] = left
            left *= nums[i]
      
        right = 1
        for i in range(num_length - 1, -1, -1):
            answer[i] *= right
            right *= nums[i]
      
        return answer
```

**Complexity Analysis**

- Time Complexity: `O(n)`. Two passes over the input array, one from left to right and one from right to left, resulting in `2n`, which simplifies to `O(n)`.
- Space Complexity: `O(1)`, excluding the output array `ans`.

## Min Stack
[LeetCode Question](https://leetcode.com/problems/min-stack/)

#### Solution

To maintain the minimum element in constant time, we use a strategy that tracks the minimum at each state of the stack. The solution stores a tuple in the stack, where each tuple contains the value and the minimum at that point.

With each push operation, the value is added to the stack along with the minimum of the new value and the current minimum (stored in the last element of the stack). This ensures that the minimum can be accessed in `O(1)` time for all stack states.

```python
class MinStack:

    def __init__(self):
        self.stack = []

    def push(self, val: int) -> None:
        curr_min = val if not self.stack else min(val, self.stack[-1][1])
        self.stack.append((val, curr_min))

    def pop(self) -> None:
        self.stack.pop()

    def top(self) -> int:
        return self.stack[-1][0]

    def getMin(self) -> int:
        return self.stack[-1][1]
```
**Complexity Analysis**

- Time Complexity:
  - `__init__`:  `O(1)`, as initializing an empty list is constant time.
  - `push`: `O(1)`, since appending to a list and calculating the minimum are constant-time operations.
  - `pop`: `O(1)`, because removing the last element from a list takes constant time.
  - `top`:  `O(1)`, as accessing the last element of a list is constant time.
  - `getMin`: `O(1)`, since the minimum is always stored in the last element of the stack.
- Space Complexity: `O(n)`, where `n` is the number of elements in the stack. Each element stores its value and the minimum at that point, resulting in linear space usage.

## Validate Binary Search Tree
[LeetCode Question](https://leetcode.com/problems/validate-binary-search-tree)

#### Solution

To validate a Binary Search Tree (BST), we ensure that for every node in the tree:
1. The value of the node is **greater than** all values in its left subtree.
2. The value of the node is **less than** all values in its right subtree.

This can be achieved using a Depth-First Search (DFS) approach, where each node is validated against a range of permissible values (`min_val` and `max_val`). These values are updated as we traverse the tree to ensure the BST properties are maintained.


```python
def isValidBST(self, root: Optional[TreeNode]) -> bool:
        def dfs(root, min_val, max_val):
            if not root:
                return True

            if not (min_val < root.val < max_val):
                return False

            return dfs(root.left, min_val, root.val) and dfs(
                root.right, root.val, max_val
            )

        return dfs(root, -inf, inf)
```
**Complexity Analysis**

- Time Complexity: `O(n)`, where `n` is the number of nodes in the tree. Each node is visited once during the traversal.
- Space Complexity: `O(h)`, where `h` is the height of the tree. This accounts for the recursive stack space. In the worst case of a skewed tree, `h` could be 
`O(n)`.

## Number of Islands
[LeetCode Question](https://leetcode.com/problems/number-of-islands)

#### Solutions:

1. **Depth-First Search (DFS):** Use recursion to traverse and mark all connected land cells.
2. **Breadth-First Search (BFS):** Use a queue to iteratively process connected land cells. 

---
#### [Good To Know 📚] Matrix as Graph

Matrices often represent graphs in problems where adjacent cells are connected. Each cell in the matrix can be thought of as a node, and connections between nodes are defined by adjacency (e.g., vertical and horizontal neighbors, or diagonal neighbors if allowed).

$`\begin{bmatrix}0 & 1 & 2\\3 & 4 & 5\\ 6 & 7 & 8 \end{bmatrix}`$ translates to:

```python
{
  0: [1, 3],
  1: [0, 2, 4],
  2: [1, 5],
  3: [0, 4, 6],
  4: [1, 3, 5, 7],
  5: [2, 4, 8],
  6: [3, 7],
  7: [4, 6, 8],
  8: [5, 7]
}
```

When solving a problem, one has to build the graph as they go. Nodes/vertices are represented by coordinates of matrix entries.

$`\begin{bmatrix}0,0 & 0,1 & 0,2\\1,0 & 1,1 & 1,2\\ 2,0 & 2,1 & 2,2 \end{bmatrix}`$.

The core of BFS/DFS is to add neighbors of the current vertex to a queue/stack. The `get_neighbors` function returns all 4 (or 8 if you are allowed to go diagonally) coordinates of neighboring nodes.

$`\begin{bmatrix} & r-1, c & \\r,c-1 & r,c & r,c+1\\  & r+1, c &  \end{bmatrix}`$.

One way to get each neighbor's coordinates is to keep each neighbor's horizontal and vertical offsets (i.e., delta) in a list and add them to each vertex's coordinates.

BFS Template:

```python
num_rows, num_cols = len(grid), len(grid[0])
def get_neighbors(coord):
    row, col = coord
    delta_row = [-1, 0, 1, 0]
    delta_col = [0, 1, 0, -1]
    res = []
    for i in range(len(delta_row)):
        neighbor_row = row + delta_row[i]
        neighbor_col = col + delta_col[i]
        if 0 <= neighbor_row < num_rows and 0 <= neighbor_col < num_cols:
            res.append((neighbor_row, neighbor_col))
    return res

from collections import deque

def bfs(starting_node):
    queue = deque([starting_node])
    visited = set([starting_node])
    while len(queue) > 0:
        node = queue.popleft()
        for neighbor in get_neighbors(node):
            if neighbor in visited:
                continue
            # Do stuff with the node if required
            # ...
            queue.append(neighbor)
            visited.add(neighbor)
```

Sometimes, one can overwrite the value in the matrix to keep track of the visited nodes without using a visited set.

---

#### Depth-First Search (DFS) 

This approach recursively visits all connected land cells ('1') and marks them as visited ('0') to prevent revisiting.

```python
def numIslands(self, grid: List[List[str]]) -> int:
        def dfs(row, col):
            grid[row][col] = "0"

            for dx, dy in zip(directions[:-1], directions[1:]):
                new_row, new_col = row + dx, col + dy

                if (
                    0 <= new_row < rows
                    and 0 <= new_col < cols
                    and grid[new_row][new_col] == "1"
                ):
                    dfs(new_row, new_col)

        num_of_islands = 0
        directions = (-1, 0, 1, 0, -1)

        rows, cols = len(grid), len(grid[0])

        for row in range(rows):
            for col in range(cols):
                if grid[row][col] == "1":
                    dfs(row, col)
                    num_of_islands += 1

        return num_of_islands
```

**Complexity Analysis**

- Time Complexity: `O(M * N)`, where `M` is the number of rows and `N` is the number of columns in the grid.
- Space Complexity: `O(M * N)`, for the recursive stack in the worst case (entire grid is one connected component).

#### Breadth-first Search (BFS)

This approach uses a queue to iteratively visit and mark all connected land cells.

```python
from collections import deque

def numIslands(self, grid: List[List[str]]) -> int:
        def get_neighbors(coord):
            res = []
            row, col = coord
            delta_row = [-1, 0, 1, 0]
            delta_col = [0, 1, 0, -1]

            for i in range(len(delta_row)):
                r = row + delta_row[i]
                c = col + delta_col[i]

                if 0 <= r < num_rows and 0 <= c < num_cols:
                    res.append((r, c))

            return res
            
        def bfs(start):
            queue = deque([start])
            r, c = start
            grid[r][c] = "0"

            while queue:
                node = queue.popleft()
                for neighbor in get_neighbors(node):
                    r, c = neighbor
                    if grid[r][c] == "0":
                        continue

                    queue.append(neighbor)
                    grid[r][c] = "0"

        num_rows = len(grid)
        num_cols = len(grid[0])

        num_islands = 0
        for r in range(num_rows):
            for c in range(num_cols):
                if grid[r][c] == "0":
                    continue

                bfs((r, c))
                num_islands += 1

        return num_islands
```

**Complexity Analysis**

- Time Complexity: `O(M * N)`, where `M` is the number of rows and `N` is the number of columns in the image.
- Space Complexity: `O(V + E)`, where `V` is the number of vertices and `E` is the number of edges in the graph.

## Rotting Oranges
[LeetCode Question](https://leetcode.com/problems/rotting-oranges)

#### Solutions:

To solve the problem, we use **Breadth-First Search (BFS)**. This approach efficiently simulates the spread of rot from rotten oranges to adjacent fresh ones. The BFS processes all the rotten oranges in parallel for each time step.

#### Steps:

1. **Initialization**:
   - Count the fresh oranges.
   - Add all initially rotten oranges to a queue.

2. **Simulate Rotting Process**:
   - While there are fresh oranges and the queue is not empty:
     - Increment the timer (minutes).
     - Process all rotten oranges in the current queue level.
     - For each rotten orange, attempt to rot its adjacent fresh oranges. If successful, add them to the queue.

3. **Result**:
   - If there are still fresh oranges left after the BFS, return `-1`. Otherwise, return the total time elapsed.


```python
def orangesRotting(self, grid: List[List[int]]) -> int:
        rows = len(grid)
        cols = len(grid[0])
        fresh_count = 0
        queue = deque()

        for r in range(rows):
            for c in range(cols):
                if grid[r][c] == 2:
                    queue.append((r, c))
                elif grid[r][c] == 1:
                    fresh_count += 1

        minutes = 0

        while queue and fresh_count > 0:
            minutes += 1

            for _ in range(len(queue)):
                r, c = queue.popleft()

                for delta_row, delta_col in [[0, 1], [0, -1], [1, 0], [-1, 0]]:
                    x, y = r + delta_row, c + delta_col

                    if 0 <= x < rows and 0 <= y < cols and grid[x][y] == 1:
                        fresh_count -= 1
                        grid[x][y] = 2
                        queue.append((x, y))

        return minutes if fresh_count == 0 else -1
```

**Complexity Analysis**

- Time Complexity: `O(M * N)`
  - Each cell is processed at most once during the BFS traversal.
  - `M` and `N` are the grid's rows and columns.
- Space Complexity: `O(M * N)`
  - The queue can hold up to all cells in the grid in the worst case.

## Search in Rotated Sorted Array
[LeetCode Question](https://leetcode.com/problems/search-in-rotated-sorted-array)

The key observation is that at least one half of the array (either left or right) will always be sorted. Using this property, we can adapt binary search as follows:

1. **Determine the Sorted Half**:
   - Compare the middle element (`nums[mid]`) with the leftmost (`nums[left]`) or rightmost (`nums[right]`) element to determine which half is sorted.

2. **Narrow the Search Range**:
   - If the target lies within the sorted half, adjust the pointers (`left` and `right`) to search within this half.
   - Otherwise, search in the other half.

3. **Repeat Until Found**:
   - Continue until the target is found or the search range is exhausted.

```python
def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1

        while left < right:
            mid = (left + right) // 2

            if nums[0] <= nums[mid]:
                if nums[0] <= target <= nums[mid]:
                    right = mid
                else:
                    left = mid + 1
            else:
                if nums[mid] < target <= nums[-1]:
                    left = mid + 1
                else:
                    right = mid

        return left if nums[left] == target else -1
```

**Complexity Analysis**

- Time Complexity: `O(log n)`. The array is halved at each step, resulting in logarithmic time complexity.
- Space Complexity: `O(1)`. The search is performed in place, requiring no additional space.

## Combination Sum
[LeetCode Question](https://leetcode.com/problems/combination-sum)

This problem is best solved using **Depth-First Search (DFS)** with backtracking. The key idea is to explore all potential combinations by recursively subtracting candidate numbers from the target and adding them to the current path. To avoid duplicate combinations, we impose an ordering constraint where we only use candidates starting from the current index or later.

### Steps

1. **Sort the Candidates**:
   - Sorting helps in pruning branches early when the remaining target becomes negative.

2. **DFS with Backtracking**:
   - Start from the first candidate.
   - Subtract the current candidate from the remaining target.
   - If the remaining target becomes zero, add the current path to the result.
   - If the remaining target becomes negative, terminate the current path.

3. **Pruning**:
   - Skip candidates that exceed the remaining target.

4. **Avoid Duplicate Combinations**:
   - Only consider candidates from the current index or later in the recursion to maintain order and avoid duplicates.


```python
def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        res = []

        def dfs(nums, start_index, remaining, path):
            if remaining == 0:
                res.append(path[:])
                return

            for i in range(start_index, len(nums)):
                num = nums[i]
                if remaining - num < 0:
                    continue

                dfs(nums, i, remaining - num, path + [num])

        candidates.sort()
        dfs(candidates, 0, target, [])

        return res
```

**Complexity Analysis**

- Time Complexity: `O(n^target/min(candidates))`. The depth of the recursion tree is proportional to 
`target/min(candidates)`and each level has `n` branches.
- Space Complexity: `O(target/min(candidates))`. The maximum depth of the recursion tree determines the space usage.

## Permutations
[LeetCode Question](https://leetcode.com/problems/permutations)

This problem is best solved using **Depth-First Search (DFS)** with backtracking. The key idea is to explore all possible orders of the given numbers by recursively building permutations and marking numbers as used during the recursion.

### Steps

1. **Initialization**:
   - Create a `res` list to store the results.
   - Use a `path` list to build permutations during the DFS traversal.
   - Maintain a `used` list to track which numbers have been included in the current path.

2. **DFS with Backtracking**:
   - If the length of the `path` equals the length of `nums`, add the current path to `res` and return.
   - Iterate through the `nums` array:
     - Skip numbers that are already marked as `used`.
     - Add the current number to the `path` and mark it as `used`.
     - Recursively call DFS to continue building the permutation.
     - Backtrack by removing the number from the `path` and marking it as not `used`.

```python
def permute(self, nums: List[int]) -> List[List[int]]:
        res = []
        path = []
        used = [False] * len(nums)

        def dfs(start_index):
            if start_index == len(nums):
                res.append(path[:])
                return
            
            for i, num in enumerate(nums):
                if used[i]:
                    continue
                
                path.append(num)
                used[i] = True
                dfs(start_index + 1)

                path.pop()
                used[i] = False
        
        dfs(0)
        return res
```

**Complexity Analysis**

- Time Complexity: `O(n!)`. There are `n!` permutations for an array of size `n`, and generating each permutation takes `O(n)` due to the path copy operation.
- Space Complexity: `O(n)`. The recursion depth is `O(n)`, and the path and used lists each take `O(n)` space.

## Merge Intervals
[LeetCode Question](https://leetcode.com/problems/merge-intervals/)

This problem is solved using the strategy to merge intervals explained in the [Insert Interval question](#good-to-know--intervals).

```python
def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        intervals.sort()
        res = []

        def overlap(a, b):
            return not (a[1] < b[0] or b[1] < a[0])

        for interval in intervals:
            if not res or not overlap(res[-1], interval):
                res.append(interval)
            else:
                res[-1][1] = max(res[-1][1], interval[1])

        return res
```

**Complexity Analysis**

- Time Complexity: `O(n log n)`.
  - Sorting the intervals takes `O(n log n)`, where `n` is the number of intervals.
  - Merging intervals requires a single traversal of the list, which is `O(n)`. Combined, the complexity is dominated by the sorting step, which is the higher order term, resulting in `O(n log n)`.
- Space Complexity: `O(n)`. The result list `res` can store up to `n` intervals in the worst case, where no intervals overlap.

## Lowest Common Ancestor of a Binary Tree
[LeetCode Question](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree)

The solution uses a **Postorder Depth-First Search (DFS)** traversal. Postorder traversal ensures that we process the current node only after its left and right subtrees have been processed. This is particularly useful for determining the LCA, as we can gather information from both subtrees before making a decision.

### Steps

1. **Base Case**:
   - If the current node is `null`, return `null`.
   - If the current node is either `p` or `q`, return the current node.

2. **Recursive Calls**:
   - Recursively find the LCA in the left and right subtrees.

3. **Decision**:
   - If both the left and right subtrees return `non-null` values, the current node is the LCA.
   - If only one subtree returns a `non-null` value, propagate that value upward.
   - If both subtrees return `null`, propagate `null`.


```python
def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        if not root:
            return None
        
        if root in (p, q):
            return root
        
        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)

        if left and right:
            return root
        
        if left or right:
            return left or right
        
        return None
```

**Complexity Analysis**

- Time Complexity: `O(n)`. The algorithm visits each node exactly once in the worst case, where `n` is the total number of nodes in the tree.
- Space Complexity: `O(h)`. The recursion stack depth depends on the height `h` of the tree. In the worst case (skewed tree), `h=O(n)`.

## Time Based key-Value Store
[LeetCode Question](https://leetcode.com/problems/time-based-key-value-store/)

The solution uses a dictionary where each key maps to a list of `[timestamp, value]` pairs. The `set` operation appends new entries to this list, and the `get` operation performs a binary search to find the largest timestamp less than or equal to the given timestamp.

```python
from collections import defaultdict


class TimeMap:

    def __init__(self):
        self.kv = defaultdict(list)

    def set(self, key: str, value: str, timestamp: int) -> None:
        self.kv[key].append([timestamp, value])

    def get(self, key: str, timestamp: int) -> str:
        if key not in self.kv:
            return ""

        left, right, pos = 0, len(self.kv[key]) - 1, -1

        while left <= right:
            mid = (left + right) // 2

            if self.kv[key][mid][0] <= timestamp:
                left = mid + 1
                pos = mid
            else:
                right = mid - 1

        if pos == -1:
            return ""

        return self.kv[key][pos][1]
```

**Complexity Analysis**

- Time Complexity:
  - `set`: `O(1)`, appending to the list is constant time.
  - `get`: `O(log n)`, where `n` is the number of timestamps for the key, due to binary search.
- Space Complexity: `O(n)`, where `n` is the total number of set operations. Each operation stores a key, timestamp, and value.

## Accounts Merge
[LeetCode Question](https://leetcode.com/problems/accounts-merge)

---
#### [Good To Know 📚] Disjoint Set Union (Union Find)

The **Union-Find** data structure is ideal for problems involving merging or grouping disjoint sets of elements. It supports two primary operations:

1. **Find**: Determines the "root" or representative of a set.
2. **Union**: Merges two sets into one.

### Implementation

The sets are represented as trees, where the root of each tree serves as the unique identifier (Set ID). Initially, every element is its own parent, forming individual sets. Merging two sets involves connecting one tree's root to the other, uniting the two sets. The find operation traverses up the tree to locate the root, which represents the set ID.

```python
class UnionFind:
    def __init__(self):
        # Maps each element to its parent; initially, each element is its own parent
        self.id = {}

    def find(self, x):
        # Retrieve the parent of x; default to x if not in the map
        y = self.id.get(x, x)
        # If x is not its own parent, recursively find the root
        if y != x:
            y = self.find(y)
        return y

    def union(self, x, y):
        # Merge the sets by connecting their roots
        self.id[self.find(x)] = self.find(y)
```

#### Path Compression Optimization

The performance of the `find` operation can degrade if the trees become unbalanced, leading to deep recursion. **Path compression** addresses this issue by flattening the tree during the `find` operation. Each node visited during the traversal is directly linked to the root, reducing the tree's height and improving future queries.

With path compression, the amortized time complexity of both `find` and `union` becomes nearly constant `(O(α(n)))`, where `α` is the [inverse Ackermann function](https://en.wikipedia.org/wiki/Ackermann_function).

```python
class UnionFind:
    def __init__(self):
        self.id = {}

    def find(self, x):
        y = self.id.get(x, x)
        if y != x:
            # Path compression: update the parent of x to the root
            self.id[x] = y = self.find(y)
        return y

    def union(self, x, y):
        # Merge the sets by connecting their roots
        self.id[self.find(x)] = self.find(y)
```

### Key Insights

- **Initial Setup**: Each element starts as its own set.
- **Union Operation**: Links one set's root to another, merging the sets.
- **Find Operation**: Traverses up the tree to find the root (set ID), with path compression for efficiency.
- **Efficiency**: With path compression, the data structure achieves near-constant time complexity for both operations.

### Practical Applications
Union-Find is widely used in algorithms and problems involving connected components, such as:

- Kruskal's algorithm for finding Minimum Spanning Trees.
- Detecting cycles in a graph.
- Dynamic connectivity problems.
---

### Solution

Each account consists of a username and a list of email addresses. If two accounts share at least one email, they belong to the same group and should be merged. The challenge is to identify these groups efficiently and output the merged accounts with emails sorted lexicographically.

#### Steps

1. **Model the Problem:**
   - Treat each email address as a node in a graph.
   - Add edges between nodes that belong to the same account.
   - The problem reduces to finding connected components in this graph.
2. **Union-Find Data Structure:**
   - Use Union-Find to group emails into connected components.
   - Each component represents a set of emails belonging to a single user.
3. **Steps to Solve**:
   - **Step 1**: Iterate through the accounts. For each account:
     - Treat the first email as a "parent" or representative.
     - Union all other emails in the account with the first email.
   - **Step 2**: After processing all accounts, use the find operation to determine the root (representative) of each email.
   - **Step 3**: Group emails by their root, and associate each group with the corresponding username.
   - **Step 4**: Sort the emails in each group lexicographically and format the result.

```python
class UnionFind:
    def __init__(self):
        self.id = {}

    def find(self, x):
        y = self.id.get(x, x)

        if y != x:
            self.id[x] = y = self.find(y)

        return y

    def union(self, x, y):
        self.id[self.find(x)] = self.find(y)


class Solution:
    def accountsMerge(self, accounts: List[List[str]]) -> List[List[str]]:
        union_find = UnionFind()
        all_user_emails = set()

        for one_account in accounts:
            username = one_account[0]
            email_parent = None

            for email in one_account[1:]:
                user_email_pair = (username, email)
                all_user_emails.add(user_email_pair)

                if email_parent is None:
                    email_parent = user_email_pair
                else:
                    union_find.union(email_parent, user_email_pair)

        account_associations = {}
        for user_email_pair in all_user_emails:
            ancestor = union_find.find(user_email_pair)

            if ancestor not in account_associations:
                account_associations[ancestor] = []

            account_associations[ancestor].append(user_email_pair)

        return_res = []
        for user in account_associations:
            one_user = [user[0]]

            for email in sorted(account_associations[user]):
                one_user.append(email[1])
                
            return_res.append(one_user)

        return sorted(return_res, key=lambda a: (a[0], a[1]))
```

**Complexity Analysis**

- Time Complexity:
  - Union-Find operations: `O(n⋅α(n))`, where `n` is the total number of emails.
  - Sorting emails: `O(nlogn)`
  - Overall: `O(nlogn)`
- Space Complexity: `O(n), storage for email_to_name and parent.

## Sort Colors
[LeetCode Question](https://leetcode.com/problems/sort-colors)

### Solutions

1. **Sorting Algorithms** 
2. **Dutch National Flag Algorithm**
3. **Couting**

#### Sorting Algorithms

This solution employs standard sorting techniques, such as Bubble Sort, Merge Sort, or Quick Sort. These algorithms are general-purpose and can sort the array effectively, though they may not be the most optimal for this problem.

#### Example: Bubble Sort
[Bubble Sort](https://en.wikipedia.org/wiki/Bubble_sort) iterates through the array multiple times, swapping adjacent elements if they are out of order.

```python
def sortColors(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """

        length = len(nums)

        for i in range(length):
            swapped = False

            for j in range(0, length - i - 1):
                if nums[j] > nums[j + 1]:
                    nums[j], nums[j + 1] = nums[j + 1], nums[j]
                    swapped = True

            if not swapped:
                break
```

**Complexity Analysis**

- Time Complexity: `O(n^2)` (not efficient for large arrays).
- Space Complexity: `O(1)`.

#### Dutch National Flag Algorithm

[The Dutch National Flag algorithm proposed by Edsger Dijkstra](https://en.wikipedia.org/wiki/Dutch_national_flag_problem) is a highly efficient method specifically tailored for three-way partitioning problems. It divides the array into three regions:

- `low`: All elements before this index are 0s (lowest value).
- `mid`: Pointer to the current element being inspected (unknown value).
- `high`: All elements after this index are 2s (high value).

#### Steps

1. Initialize pointers `low`, `mid`, and `high`.
2. Traverse the array:
   - If the current element is 0, swap it with the element at `low` and increment both `low` and `mid`.
   - If the current element is 1, simply move `mid` forward.
   - If the current element is 2, swap it with the element at `high` and decrement `high`.
   - Repeat until `mid` exceeds `high`.

```python
def sortColors(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """

        low, mid, high = 0, 0, len(nums) - 1

        while mid <= high:
            if nums[mid] == 0:
                nums[mid], nums[low] = nums[low], nums[mid]
                mid += 1
                low += 1
            elif nums[mid] == 1:
                mid += 1
            else:
                nums[mid], nums[high] = nums[high], nums[mid]
                high -= 1 
```

**Complexity Analysis**

- Time Complexity: `O(n)`, as each element is processed at most once.
- Space Complexity: `O(1)`, as the sorting is done in-place.
  
#### Counting Sort

This solution counts the occurrences of each color (0, 1, 2) and then updates the array based on these counts.

#### Steps:

1. Traverse the array and count the occurrences of 0s, 1s, and 2s.
2. Overwrite the array:
   - Fill the first `count[0]` positions with 0.
   - Fill the next `count[1]` positions with 1.
   - Fill the remaining positions with 2.

```python
def sortColors(self, nums: List[int]) -> None:
    nums_red = nums_white = nums_blue = 0
    for num in nums:
        if num == 0:
            nums_red += 1
        elif num == 1:
            nums_white += 1
        else:
            nums_blue += 1

    nums[:nums_red] = [0] * nums_red
    nums[nums_red:nums_red + nums_white] = [1] * nums_white
    nums[nums_red + nums_white:] = [2] * nums_blue
```

**Complexity Analysis**

- Time Complexity: `O(n)`, as we traverse the array twice.
- Space Complexity: `O(1)`, as the sorting is done in-place.

## Word Break
[LeetCode Question](https://leetcode.com/problems/word-break)

### Solution

To determine if a string `s` can be segmented into a space-separated sequence of words from a given dictionary `wordDict`, we can explore a recursive approach with memoization. 

The idea is to check if any word in `wordDict` is a prefix of the current substring of `s`. If a word matches, we treat it as "matched" and recursively check the remaining part of the string. This process can be visualized as a state-space tree, where each level represents a substring of `s` being matched with words from the dictionary.



```python
def wordBreak(self, s: str, wordDict: List[str]) -> bool:
    memo = {}

    def dfs(start_index):
        # If we've reached the end of the string, return True
        if start_index == len(s):
            return True

        # If the result is already computed, return it
        if start_index in memo:
            return memo[start_index]

        # Check each word in the dictionary
        for word in wordDict:
            # If the word is a prefix of the current substring
            if s[start_index:].startswith(word):
                # Recursively check the remaining substring
                if dfs(start_index + len(word)):
                    memo[start_index] = True
                    return True

        # If no match is found, store False in memo and return
        memo[start_index] = False
        return False

    return dfs(0)
```

**Complexity Analysis**

- Time Complexity: `O(n * m * k)`
  - The memoization array has a size of `O(n)`, where `n` is the length of `s`.
  - For each state, we iterate over all words in `wordDict `(of size `m`) and perform a prefix check, which takes `O(k)` time, where `k` is the maximum length of a word in the dictionary.
- Space Complexity: `O(n)`
  - The recursion stack can go as deep as `O(n)` (height of the state-space tree).
  - The memoization array also requires `O(n)` space.

## Partition Equal Subset Sum
[LeetCode Question](https://leetcode.com/problems/partition-equal-subset-sum)

### Solution

To determine if an array can be partitioned into two subsets with equal sums, we can use a **bottom-up dynamic programming (DP)** approach. In this approach, we iteratively build the solution using smaller subproblems. The key idea is to use a DP table to track whether a specific sum can be achieved with a subset of the array elements.

#### Steps:

1. **Target Sum Calculation:**  If the total sum of the array is odd, it is impossible to partition it into two equal subsets. Otherwise, the target sum for each subset is `total_sum / 2`.
2. **DP Table Definition:**  Let `dp[s]` represent whether a sum `s` can be formed using a subset of the array.
3. **Transition Formula:**  For each number in the array, update the `dp` table:
   - If `dp[s - num]` is `True`, set `dp[s]` to `True`. This means that if we can form the sum `s - num`, then we can form the sum `s` by including `num`.
5. **Base Case:**  `dp[0] = True` because a sum of 0 can always be achieved by choosing no elements.
6. **Optimization:**  Instead of maintaining a 2D table, we use a 1D array to save space. Update the DP array in reverse to avoid overwriting values during the iteration.

```python
def canPartition(nums: List[int]) -> bool:
    total_sum = sum(nums)
    
    # If the total sum is odd, it's impossible to partition into two equal subsets
    if total_sum % 2 != 0:
        return False

    target = total_sum // 2
    dp = [False] * (target + 1)
    dp[0] = True  # Base case

    for num in nums:
        # Update the DP array in reverse
        for s in range(target, num - 1, -1):
            dp[s] = dp[s] or dp[s - num]

    return dp[target]
```

**Complexity Analysis**

- Time Complexity: `O(n * target)`, where `n` is the number of elements in the array and `target` is the target sum `(total_sum / 2)`.
- Space Complexity: `O(target)`, as we use a 1D array of size `target + 1`.

## String to Integer (atoi)
[LeetCode Question](https://leetcode.com/problems/string-to-integer-atoi)

### Solution

The solution implements the conversion process through a series of steps that correspond to the rules provided in the problem description:

1. **Initialization:**  
   - The function first checks if the string `s` is not empty.  
   - Important variables are initialized: `n` for the length of the string, `i` for the current index, and `res` for the resulting integer value.

2. **Whitespace Skipping:**  
   - Using a `while` loop, the function skips over leading whitespace characters (`' '`) until a non-whitespace character is encountered by incrementing the `i` index.  
   - If `i` becomes equal to `n` (the length of the string), the string contains only whitespace, and the function returns `0`.

3. **Sign Detection:**  
   - The next character is checked to determine the sign of the resultant integer.  
   - If it's `'-'`, the variable `sign` is set to `-1`, otherwise `1`.  
   - If a sign character is found, `i` is incremented to move past it.

4. **Integer Conversion:**  
   - A `while` loop iterates over the remaining characters of the string as long as they are digits.  
   - Each digit is converted to an integer using `int(s[i])` and added to `res` after multiplying the current `res` by 10 (shifting the number one decimal place to the left).  
   - The loop stops if a non-digit character is encountered.

5. **Overflow Handling:**  
   - Before adding the new digit to `res`, potential overflow is checked.  
   - For positive numbers, the function checks if `res > (2**31 - 1) // 10` or if `res == (2**31 - 1) // 10` and the next digit (`c`) is greater than `7`.  
   - For negative numbers, the same logic is applied but with the limits of `-2**31`.  
   - If overflow is detected, the maximum or minimum 32-bit signed integer value is returned based on the sign.

6. **Result Return:**  
   - Once the loop finishes, the integer is adjusted with the sign and returned as the result.


```python
def myAtoi(self, s: str) -> int:
    # Return 0 if the string is empty
    if not s:
        return 0

    length_of_string = len(s)
    index = 0

    # Skip leading whitespaces
    while index < length_of_string and s[index] == " ":
        index += 1

    # After skipping whitespaces, if we're at the end it means it's an empty string
    if index == length_of_string:
        return 0

    # Check if we have a sign, and set the sign accordingly
    sign = -1 if s[index] == "-" else 1
    if s[index] in ["-", "+"]:  # Skip the sign for next calculations
        index += 1

    result = 0
    max_safe_int = (2**31 - 1) // 10  # Precomputed to avoid overflow

    while index < length_of_string:
        # If the current character is not a digit, break from loop
        if not s[index].isdigit():
            break

        digit = int(s[index])

        # Check for overflow cases
        if result > max_safe_int or (result == max_safe_int and digit > 7):
            return 2**31 - 1 if sign == 1 else -(2**31)  # Clamp to INT_MIN or INT_MAX

        # Append current digit to result
        result = result * 10 + digit
        index += 1

    # Apply sign and return final result
    return sign * result
```

**Complexity Analysis**

- Time Complexity: `O(n)`, where `n` is the length of the string.
- Space Complexity: `O(1)`. The function uses a fixed amount of space for variables and does not allocate additional space that grows with the input size.

## Spiral Matrix
[LeetCode Question](https://leetcode.com/problems/spiral-matrix/)

### Solution

To solve this problem, we simulate the process of traversing the matrix in a spiral order. The key idea is to keep moving in the current direction (starting with "right") until we hit a boundary or a previously visited cell. At that point, we turn clockwise and continue the traversal.

```python
def spiralOrder(self, matrix):
    """
    :type matrix: List[List[int]]
    :rtype: List[int]
    """
    c, r = len(matrix[0]), len(matrix)  # Columns and rows
    x, y, dx, dy = 0, 0, 1, 0  # Starting position and direction (right)
    ans = []  # Resultant spiral order
    
    for _ in range(c * r):  # Traverse all cells
        # Append the current cell to the result
        ans.append(matrix[y][x])
        
        # Mark the current cell as visited
        matrix[y][x] = "*"
        
        # Check if the next move is valid
        if not (0 <= x + dx < c) or not (0 <= y + dy < r) or matrix[y + dy][x + dx] == "*":
            # Change direction (clockwise turn)
            dx, dy = -dy, dx
        
        # Move to the next cell
        x, y = x + dx, y + dy
    
    return ans
```

**Complexity Analysis**

- Time Complexity: `O(c * r)`, where `c` is the number of columns and `r` is the number of rows. Each cell is visited exactly once.
- Space Complexity: `O(1)`, since the input matrix is modified in place to mark visited cells, avoiding the need for additional storage.

## Subsets
[LeetCode Question](https://leetcode.com/problems/subsets/description/)

### Solution

The problem requires generating all possible subsets (the power set) of a given list of integers. This can be efficiently achieved using Depth-First Search (DFS) with backtracking.

The DFS-based approach works as follows:

1. **Recursive Traversal:**  
   At each step, we decide whether to include the current number in the subset or not. This is achieved by iterating through the numbers starting from the current index.

2. **Building Subsets:**  
   - The `path` variable represents the current subset being constructed.
   - At every node in the DFS tree, the current `path` is added to the result list `res`.

3. **Backtracking:**  
   After exploring all subsets that include the current number, we backtrack by removing it from the `path` to explore subsets that exclude it.

4. **Optimization:**  
   - The recursion starts at the current index `i`, ensuring that each subset is unique and avoiding backward branches.
   - This approach avoids duplicate subsets by pruning unnecessary branches.

```python
def subsets(self, nums: List[int]) -> List[List[int]]:
    n = len(nums)
    res = []

    def dfs(i, path):
        # Add the current subset to the result
        res.append(path[:])

        # Iterate through the remaining numbers
        for num_idx in range(i, len(nums)):
            # Include the current number in the subset
            path.append(nums[num_idx])
            
            # Recurse with the next index
            dfs(num_idx + 1, path)
            
            # Backtrack: remove the last number added
            path.pop()

    # Start DFS with an empty path
    dfs(0, [])
    return res
```

**Complexity Analysis**

- Time Complexity: `O(n * 2^n)`
  - There are `2^n` subsets for a list of size `n`.
  - Each subset takes `O(n)` time to construct due to the `path[:]` operation.
- Space Complexity: `O(n * 2^n)`
  - The space required for storing all subsets is proportional to the total number of subsets (`2^n`) multiplied by the average subset size (`n`).
  - Additionally, the recursion stack depth is `O(n)`.

## Binary Tree Right Side View
[LeetCode Question](https://leetcode.com/problems/binary-tree-right-side-view)

### Solution

To solve the problem of finding the right-side view of a binary tree, we can use **Breadth-First Search (BFS)**. The idea is to traverse the tree level by level and capture the first node we encounter at each level, which corresponds to the rightmost node for that level when we process the nodes in the correct order.

1. **Initialization:**  
   - Use a queue to store nodes level by level. Start with the root node.

2. **Traversal:**  
   - For each level, capture the first node in the queue (this is the rightmost node for that level since we add right children before left children to the queue).

3. **Processing Nodes:**  
   - Dequeue all nodes of the current level, and enqueue their children (right child first, then left child) to ensure the rightmost node appears first in subsequent levels.

4. **Result Construction:**  
   - Append the value of the rightmost node for each level to the result list.


```python
def rightSideView(self, root: Optional[TreeNode]) -> List[int]:
    if not root:
        return []

    res = []
    queue = deque([root])

    while queue:
        n = len(queue)
        # Append the first node's value (rightmost for this level)
        res.append(queue[0].val)

        for _ in range(n):
            node = queue.popleft()
            # Add right child first, then left child
            for child in [node.right, node.left]:
                if child is not None:
                    queue.append(child)

    return res
```

**Complexity Analysis**

- Time Complexity: `O(n)`. Each node is visited once during the traversal.
- Space Complexity: `O(n)`. In the worst case, the queue can contain all nodes at the last level of the tree, which is proportional to `n` for a balanced binary tree.

## Longest Palindromic Substring
[LeetCode Question](https://leetcode.com/problems/longest-palindromic-substring)

### Solution

The approach involves treating each character (and the space between two characters) as a potential center of a palindrome. From each center, expand outward while the characters on both sides are equal. Track the longest palindrome during this process.

### Steps

1. **Expand from Center:**  
   - Define a helper function `expand_from_center(left, right)` that expands outward from the given indices as long as the characters are equal. This function returns the longest palindrome substring for the given center.

2. **Iterate Over Possible Centers:**  
   - For each character in the string, consider it as a center for an odd-length palindrome.
   - For each pair of consecutive characters, consider them as a center for an even-length palindrome.

3. **Update Longest Palindrome:**  
   - Compare the lengths of palindromes returned by the helper function for odd and even centers.
   - Keep track of the longest palindrome found.

```python
def longestPalindrome(self, s: str) -> str:
    if len(s) <= 1:
        return s

    def expand_from_center(left, right):
        while left >= 0 and right < len(s) and s[left] == s[right]:
            left -= 1
            right += 1
        # Return the longest palindrome substring for this center
        return s[left + 1:right]

    max_str = s[0]  # At least one character is always a palindrome

    for i in range(len(s) - 1):
        # Check for odd-length palindromes
        odd = expand_from_center(i, i)
        # Check for even-length palindromes
        even = expand_from_center(i, i + 1)

        # Update the maximum palindrome if a longer one is found
        if len(odd) > len(max_str):
            max_str = odd
        if len(even) > len(max_str):
            max_str = even

    return max_str
```

**Complexity Analysis**

- Time Complexity: `O(n^2)`. For each character, we expand outward to check for palindromes, which takes linear time in the worst case. Since this is done for all characters, the total time complexity is quadratic.
- Space Complexity: `O(1)`. The function uses a constant amount of extra space, as it only tracks indices and the maximum palindrome string.

## Unique Paths
[LeetCode Question](https://leetcode.com/problems/unique-paths)

### Solution

This is a dynamic programming (DP) problem where the goal is to compute the number of unique ways a robot can move from the top-left corner of a grid to the bottom-right corner, given that it can only move right or down.

### Steps

1. **State Definition:**  
   Let `dp[r][c]` represent the number of unique paths to reach cell `(r, c)`.

2. **State Transition:**  
   To reach cell `(r, c)`, the robot can come from:
   - The cell above `(r - 1, c)`, or
   - The cell to the left `(r, c - 1)`.  
   
   Hence, `dp[r][c] = dp[r-1][c] + dp[r][c-1]`

3. **Base Cases:**  
   - For the first row (`r = 0`), all cells can only be reached from the left. Thus, `dp[0][c] = 1` for all `c`.
   - For the first column (`c = 0`), all cells can only be reached from above. Thus, `dp[r][0] = 1` for all `r`.

4. **Final Answer:**  
   The value at `dp[m-1][n-1]` (bottom-right corner) represents the total number of unique paths.


```python
def uniquePaths(self, m: int, n: int) -> int:
    # Initialize a 2D DP array with dimensions m x n
    dp = [[0 for _ in range(n)] for _ in range(m)]

    # Set base cases for the first row and first column
    for c in range(n):
        dp[0][c] = 1  # First row
    for r in range(m):
        dp[r][0] = 1  # First column

    # Fill the DP table using the state transition
    for r in range(1, m):
        for c in range(1, n):
            dp[r][c] = dp[r - 1][c] + dp[r][c - 1]

    # Return the value at the bottom-right corner
    return dp[-1][-1]
```

**Complexity Analysis**

- Time Complexity: `O(n * m)`. We iterate through all the cells in the grid to compute their values.
- Space Complexity: `O(n * m)`. The `dp` table requires space proportional to the size of the grid.

## Construct Binary Tree from Preorder and Inorder Traversal
[LeetCode Question](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal)

1. **Preorder Traversal:**  
   The first element is always the root of the tree.  
   Traversal order: **root → left subtree → right subtree**.

2. **Inorder Traversal:**  
   The root splits the array into the left and right subtrees.  
   Traversal order: **left subtree → root → right subtree**.

### Steps

1. **Mapping Values for Fast Lookup:**  
   Use a dictionary to map each value in the `inorder` traversal to its index. This allows `O(1)` lookup for the root's position in the `inorder` array.

2. **Recursive Construction:**  
   Define a recursive function `build_tree(preorder_index, inorder_start, size)`:
   - **Base Case:** If `size <= 0`, return `None` (empty subtree).
   - **Root Identification:** The root of the current subtree is `preorder[preorder_index]`.
   - **Inorder Split:** Use the `value_to_index` map to find the root's index in the `inorder` array. The elements to the left form the left subtree, and the elements to the right form the right subtree.
   - **Recursive Calls:** Construct the left and right subtrees:
     - **Left Subtree:** Start from the next element in the `preorder` array, with size determined by the left portion of the `inorder` array.
     - **Right Subtree:** Start after the left subtree in the `preorder` array, with size determined by the right portion of the `inorder` array.

3. **TreeNode Construction:**  
   Create a `TreeNode` with the root value, and attach the left and right subtrees.


```python
def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
    # Create a map for quick index lookup in the inorder array
    value_to_index = {val: idx for idx, val in enumerate(inorder)}

    # Helper function to recursively build the tree
    def build_tree(preorder_index, inorder_start, size):
        if size <= 0:
            return None  # Base case: empty subtree

        # Identify the root
        root_value = preorder[preorder_index]
        inorder_root_index = value_to_index[root_value]
        left_subtree_size = inorder_root_index - inorder_start

        # Recursively construct the left and right subtrees
        left = build_tree(preorder_index + 1, inorder_start, left_subtree_size)
        right = build_tree(preorder_index + 1 + left_subtree_size, inorder_root_index + 1, size - 1 - left_subtree_size)

        # Return the constructed TreeNode
        return TreeNode(root_value, left, right)

    # Initiate the recursive construction
    return build_tree(0, 0, len(preorder))
```

**Complexity Analysis**

- Time Complexity: `O(n)`
  - Building the `value_to_index` map: `O(n`), where `n` is the number of nodes.
  - Recursive Tree Construction: Each node is visited once, resulting in `O(n)`.

- Space Complexity: `O(n)`
  - `value_to_index` map: `O(n)`.
  - Recursive Call Stack: `O(h)`, where `h` is the height of the tree (worst case `O(n)` for a skewed tree).

## Container With Most Water
[LeetCode Question](https://leetcode.com/problems/container-with-most-water)

### Solution

To solve this problem efficiently, we use the **two-pointers technique**:

1. **Initialize Pointers:**  
   Place one pointer (`left`) at the beginning of the array and the other pointer (`right`) at the end.

2. **Calculate Area:**  
   The area between the two pointers is determined by the formula:  
   $`\text{Area} = (\text{right} - \text{left}) \times \min(\text{height[left]}, \text{height[right]})`$
   
   Keep track of the maximum area encountered so far.

4. **Move the Pointer with the Shorter Line:**  
   - If `height[left] < height[right]`, move the `left` pointer inward.  
   - Otherwise, move the `right` pointer inward.  
   
   This ensures that we explore configurations that might result in a larger area, as moving the taller line would not increase the area.

5. **Repeat Until Pointers Meet:**  
   Continue the process until the two pointers converge.


```python
def maxArea(self, height: List[int]) -> int:
    left, right, max_area = 0, len(height) - 1, 0

    while left < right:
        # Calculate the current area
        current_area = (right - left) * min(height[right], height[left])
        # Update the maximum area
        max_area = max(max_area, current_area)

        # Move the pointer with the shorter line
        if height[left] < height[right]:
            left += 1
        else:
            right -= 1

    return max_area
```

**Complexity Analysis**

- Time Complexity: `O(n)`. The while loop runs until the two pointers meet, which takes `O(n)` time.
- Space Complexity: `O(1)`. The algorithm uses only a few variables for tracking pointers and area.

## Letter Combinations of a Phone Number
[LeetCode Question](https://leetcode.com/problems/letter-combinations-of-a-phone-number)

### Solution

The problem involves generating all possible letter combinations that the digits in a phone number can represent. This is a combinatorial problem that can be efficiently solved using backtracking.

#### Steps

1. **Mapping Digits to Letters**:
   Create a dictionary to map digits to their corresponding letters, as found on a phone keypad. For example:
   ```python
   digit_to_letters = {
    '2': 'abc', '3': 'def', '4': 'ghi', '5': 'jkl',
    '6': 'mno', '7': 'pqrs', '8': 'tuv', '9': 'wxyz'
   }
   ```

2. **Backtracking**:
   Use a recursive function to explore all possible combinations:
   - Start with an empty string as the current combination.
   - For each digit, iterate over its corresponding letters and add one letter at a time to the current combination.
   - If the combination's length matches the number of digits in the input, add it to the result list.

3. **Base Case**:
   If the input is empty, return an empty list.

```python
def letterCombinations(self, digits: str) -> List[str]:
        if not digits:
            return []

        KEYBOARD = {
            "2": "abc",
            "3": "def",
            "4": "ghi",
            "5": "jkl",
            "6": "mno",
            "7": "pqrs",
            "8": "tuv",
            "9": "wxyz",
        }

        res = []

        def dfs(start_index, path):
            if start_index == len(digits):
                res.append("".join(path))
                return

            next_number = digits[start_index]
            for letter in KEYBOARD[next_number]:
                path.append(letter)
                dfs(start_index + 1, path)
                path.pop()

        dfs(0, [])
```

**Complexity Analysis**

- Time Complexity: `O(4^n * n)`. Each digit can map to up to 4 letters, resulting in `4^n` combinations. Constructing each combination takes `O(n)` time to concatenate strings.
- Space Complexity: `O(4^n)`. This accounts for the space used to store all combinations in the result list.

## Word Search
[LeetCode Question](https://leetcode.com/problems/word-search)

### Solution

To solve this problem, we employ Depth-First Search (DFS). The intuition is to explore all potential paths from a starting cell to see if we can spell out the word.

#### Steps

1. **Start at Every Cell**: Consider every cell in the grid as a potential starting point of the word.
2. **Explore Adjacent Cells**: From the starting cell, recursively check adjacent cells to match the next character of the word.
3. **Mark Visited Cells**: Temporarily mark a cell as visited to avoid revisiting it during the current path.
4. **Backtracking**: After exploring all possibilities from a cell, reset it to its original state.
5. **Termination**: 
   - If all characters are matched, return `True`.
   - If no valid path exists, return `False`.

This logic is implemented in a recursive `search_word` function, which explores all possible paths from a given cell.

```python
def exist(self, board: List[List[str]], word: str) -> bool:
    # Depth-first search function to search the word in the board
    def search_word(x: int, y: int, index: int) -> bool:
        # Check if the last character matches
        if index == len(word) - 1:
            return board[x][y] == word[index]
        # If current character does not match the word character at index, return False
        if board[x][y] != word[index]:
            return False
        # Store the current character and mark the cell as visited with "0"
        temp = board[x][y]
        board[x][y] = "0"
        # Define directions for exploration: up, right, down, left
        directions = [(-1, 0), (0, 1), (1, 0), (0, -1)]
        # Loop through all possible directions
        for dx, dy in directions:
            new_x, new_y = x + dx, y + dy
            # Check boundaries and if the next cell is not visited
            if 0 <= new_x < rows and 0 <= new_y < cols and board[new_x][new_y] != "0":
                # Recur with the new position and the next character index
                if search_word(new_x, new_y, index + 1):
                    return True
        # Restore the current cell's value after exploring all directions
        board[x][y] = temp
        return False

    # Retrieve the dimensions of the board
    rows, cols = len(board), len(board[0])
    # Iterate through each cell in the board, trying to match the first character
    for i in range(rows):
        for j in range(cols):
            # If the first character matches and the word can be found from here, return True
            if board[i][j] == word[0] and search_word(i, j, 0):
                return True
    # If the word cannot be found, return False
    return False
```

**Complexity Analysis**

- Time Complexity: `O(m * n * 3^(l-1))`
  - `m * n`: We start DFS from each cell.
  - `3^(l-1)`: At each step, there are at most 3 valid directions to explore (excluding the direction we came from).
  - `l`: Length of the word.
- Space Complexity: `O(l)`. The recursion stack can go as deep as the length of the word.

## Find All Anagrams in a String
[LeetCode Question](https://leetcode.com/problems/find-all-anagrams-in-a-string)

### Solution

This problem can be efficiently solved using the **sliding window** technique. The idea is to maintain a window of the same size as the target string `p` and check whether the characters in the window form an anagram of `p`. We use two frequency arrays to compare the character counts in the window and `p`.

#### Steps

1. **Initialization**:
   - Create two frequency arrays: one for the target string `p` (`check_counter`) and another for the current window in `s` (`window`).
   - Populate these arrays for the first `p_len` characters of `s`.

2. **Sliding Window**:
   - Slide the window one character at a time:
     - Remove the leftmost character from the window.
     - Add the next character to the window.
   - Compare the two frequency arrays. If they match, the current window is an anagram, and we record the starting index.

3. **Result**:
   - Return all starting indices of anagrams found in `s`.

```python
def findAnagrams(self, s: str, p: str) -> List[int]:
    s_len, p_len = len(s), len(p)

    if s_len < p_len:
        return []

    res = []
    check_counter = [0] * 26  # Frequency array for `p`
    window = [0] * 26  # Frequency array for the current window in `s`
    a = ord("a")  # ASCII value of 'a'

    # Initialize the frequency arrays
    for i in range(p_len):
        check_counter[ord(p[i]) - a] += 1
        window[ord(s[i]) - a] += 1

    # Check if the initial window matches
    if window == check_counter:
        res.append(0)

    # Slide the window across the string
    for i in range(p_len, s_len):
        # Remove the leftmost character from the window
        window[ord(s[i - p_len]) - a] -= 1
        # Add the new character to the window
        window[ord(s[i]) - a] += 1

        # Check if the current window matches `check_counter`
        if window == check_counter:
            res.append(i - p_len + 1)

    return res
```

**Complexity Analysis**

- Time Complexity: `O(n)`. We traverse the string `s` once, updating the frequency arrays in constant time for each character.
- Space Complexity: `O(1)`. The frequency arrays have a fixed size of 26 (for lowercase English letters), making the space usage constant.

## Minimum Height Trees
[LeetCode Question](https://leetcode.com/problems/minimum-height-trees/)

### Solution

The key insight for solving this problem is understanding that the root of a Minimum Height Tree (MHT) will be near the center of the tree. If we pick a node on the periphery of the tree (like a leaf), the height will be larger because it takes more edges to reach the opposite side of the tree. The MHTs can be found by iteratively trimming the leaves until one or two nodes remain, which will be our answer.

This process is similar to peeling an onion: we remove the outer layer (leaves) one by one until we reach the core. Here's the step-by-step breakdown:

1. **Identify Leaf Nodes**:
   - Leaf nodes are nodes with only one connection (degree of 1).
   
2. **Trim Leaves**:
   - Remove the leaf nodes and their edges, exposing a new layer of leaf nodes.

3. **Repeat Until Core**:
   - Continue the process until one or two nodes remain. These nodes will be the roots of the MHTs.

**Why One or Two Nodes?**

- A tree with an odd number of nodes will have one central node as the root.
- A tree with an even number of nodes will have two central nodes.

This approach uses a **breadth-first search (BFS)** to trim the leaves layer by layer. A queue is employed to manage the current set of leaves, and the degree of each node is tracked to identify new leaves.


```python
from collections import defaultdict, deque

def findMinHeightTrees(self, n: int, edges: List[List[int]]) -> List[int]:
    if n == 1:
        return [0]  # A single node is its own MHT

    # Build the graph and track degrees
    graph = defaultdict(list)
    degrees = [0] * n

    for node1, node2 in edges:
        graph[node1].append(node2)
        graph[node2].append(node1)
        degrees[node1] += 1
        degrees[node2] += 1

    # Initialize the queue with all leaf nodes
    leaves_queue = deque(i for i in range(n) if degrees[i] == 1)

    # Perform BFS to trim leaves layer by layer
    while leaves_queue:
        min_height_trees = []  # Stores the current layer of leaves

        for _ in range(len(leaves_queue)):
            current_node = leaves_queue.popleft()
            min_height_trees.append(current_node)

            # Reduce the degree of neighbors and check for new leaves
            for neighbor in graph[current_node]:
                degrees[neighbor] -= 1
                if degrees[neighbor] == 1:
                    leaves_queue.append(neighbor)

    return min_height_trees
```

**Complexity Analysis**

- Time Complexity: `O(V + E)`, where `V` is the number of vertices (or nodes) and `E` is the number of edges.
- Space Complexity: `O(V + E)`. The graph and queue require `O(V + E)` space.

## Task Scheduler
[LeetCode Question](https://leetcode.com/problems/task-scheduler)

### Solution

To solve the problem, we need to determine the minimum time required to execute all tasks while adhering to the cooling period `n` between two identical tasks. The key idea is to use a **priority queue (max heap)** to schedule the most frequent tasks first, ensuring that we utilize the cooling period efficiently.

### Steps

1. **Count Task Frequencies**:
   - Use a `Counter` to calculate the frequency of each task.

2. **Max Heap**:
   - Push the negative frequencies of tasks into a max heap (Python's `heapq` is a min heap, so we use negative values to simulate a max heap).

3. **Queue for Cooling**:
   - Use a `deque` to track tasks that are cooling down. Each entry in the queue contains the task's remaining count and the time it will be available again.

4. **Simulate Time**:
   - At each time step, either execute a task from the heap or wait (if no tasks are available).
   - If a task is executed and still has remaining instances, add it to the cooling queue with its next available time.
   - Check the cooling queue at each step to move tasks back into the heap when their cooling period ends.

5. **Repeat Until All Tasks Are Executed**:
   - Continue the process until both the heap and the cooling queue are empty.

```python
from collections import Counter, deque
import heapq

def leastInterval(self, tasks: List[str], n: int) -> int:
    # Count the frequency of each task
    count = Counter(tasks)
    # Create a max heap with negative frequencies
    maxHeap = [-cnt for cnt in count.values()]
    heapq.heapify(maxHeap)

    time = 0
    # Queue to manage cooling tasks
    q = deque()  # Stores pairs of [remaining count, available time]

    while maxHeap or q:
        time += 1

        # Execute the most frequent task if available
        if maxHeap:
            cnt = 1 + heapq.heappop(maxHeap)  # Decrease the task count
            if cnt:  # If there are remaining instances of the task
                q.append([cnt, time + n])  # Add it to the cooling queue

        # Check if any task in the cooling queue is ready to be executed
        if q and q[0][1] == time:
            heapq.heappush(maxHeap, q.popleft()[0])  # Move task back to heap

    return time
```

**Complexity Analysis**

- Time Complexity: `O(T * log T)`, where `T` is the number of tasks. Each insertion or removal from the heap takes `O(log T)`, and we process each task at most twice (once in the heap and once in the queue).
- Space Complexity: `O(T)`. Space is used for the heap, the cooling queue, and the task frequency counter.

## LRU Cache
[LeetCode Question](https://leetcode.com/problems/lru-cache)

### Solution

The **Least Recently Used (LRU)** cache requires constant-time `get` and `put` operations while maintaining the order of access. This can be efficiently implemented using a combination of a **hash map** and a **doubly linked list**.

### Key Insights

1. **Hash Map**:
   - Stores the key-value pairs for `O(1)` access.
   - Instead of storing the value directly, it stores a reference to a node in the doubly linked list.

2. **Doubly Linked List**:
   - Maintains the order of usage:
     - The most recently used node is at the head.
     - The least recently used node is at the tail.
   - Supports `O(1)` insertion and removal of nodes.

### Operations

1. **`get(key)`**:
   - If the key exists in the cache, move the corresponding node to the head of the list (marking it as most recently used) and return its value.
   - If the key does not exist, return `-1`.

2. **`put(key, value)`**:
   - If the key exists, update its value and move the node to the head of the list.
   - If the key does not exist:
     - Add a new node to the head of the list.
     - If the cache exceeds its capacity, remove the tail node (least recently used) and delete it from the hash map.


```python
class Node:
    def __init__(self, key=0, value=0):
        self.key = key
        self.value = value
        self.prev = None
        self.next = None


class LRUCache:
    def __init__(self, capacity: int):
        self.cache = {}  # Hash map to store key-node pairs
        self.head = Node()  # Dummy head
        self.tail = Node()  # Dummy tail
        self.capacity = capacity
        self.size = 0

        # Initialize doubly linked list
        self.head.next = self.tail
        self.tail.prev = self.head

    def _remove_node(self, node):
        """Removes a node from the doubly linked list."""
        node.prev.next = node.next
        node.next.prev = node.prev

    def _add_to_head(self, node):
        """Adds a node to the head of the doubly linked list."""
        node.prev = self.head
        node.next = self.head.next
        self.head.next.prev = node
        self.head.next = node

    def _move_to_head(self, node):
        """Moves an existing node to the head of the doubly linked list."""
        self._remove_node(node)
        self._add_to_head(node)

    def _remove_tail(self):
        """Removes the tail node (least recently used)."""
        node = self.tail.prev
        self._remove_node(node)
        return node

    def get(self, key: int) -> int:
        if key not in self.cache:
            return -1

        # Move the accessed node to the head
        node = self.cache[key]
        self._move_to_head(node)

        return node.value

    def put(self, key: int, value: int) -> None:
        if key in self.cache:
            # Update the value and move the node to the head
            node = self.cache[key]
            node.value = value
            self._move_to_head(node)
        else:
            # Create a new node
            node = Node(key, value)
            self.cache[key] = node
            self._add_to_head(node)

            self.size += 1

            # If the cache exceeds capacity, remove the least recently used node
            if self.size > self.capacity:
                removed_node = self._remove_tail()
                del self.cache[removed_node.key]
                self.size -= 1
```

**Complexity Analysis**

- Time Complexity: `O(1)` for both `get` and `put` operations.
  - Hash map lookups, node removal, and node insertion are all `O(1)`.
- Space Complexity: `O(capacity)` for storing up to `capacity` nodes and their references in the hash map and doubly linked list.

## Kth Smallest Element in a BST
[LeetCode Question](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)

### Solution

The task is to find the `k-th` smallest element in a Binary Search Tree (BST). The BST property ensures that an **in-order traversal** (left, root, right) yields the elements in sorted order. Thus, the `k-th` smallest element corresponds to the `k-th` node visited during an in-order traversal.

## Approach
To optimize, we use an **iterative in-order traversal** with a stack:

1. Traverse to the leftmost node, pushing nodes onto the stack.
2. Pop nodes from the stack one by one, decrementing `k` each time.
3. When `k = 0`, the current node is the `k-th` smallest element.

This approach avoids unnecessary traversal beyond the `k-th` node, making it efficient.

```python
def kthSmallest(self, root: Optional[TreeNode], k: int) -> int:
    stack = []

    # Iterative in-order traversal
    while root or stack:
        # Traverse to the leftmost node
        while root:
            stack.append(root)
            root = root.left

        # Process the current node
        root = stack.pop()
        k -= 1
        if k == 0:
            return root.val

        # Move to the right subtree
        root = root.right
```

**Complexity Analysis**

- Time Complexity
  - Worst Case: `O(H + k)`, where `H` is the height of the tree.
    - Traversing to the leftmost node takes `O(H)`.
    - Visiting `k` nodes to find the result adds `O(k)`.
- Average Case: For a balanced BST, `H ~= log(N)`, so the time complexity is: `O(log(N) + k)`

- Space Complexity
  - Worst Case: `O(H)`, where `H` is the height of the tree.
    - The stack stores nodes up to the height of the tree.
  - Average Case: For a balanced BST, `H ~= log(N)`, so the space complexity is: `O(log(N))`

## Minimum Window Substring
[LeetCode Question](https://leetcode.com/problems/minimum-window-substring)

### Solution

To tackle the "Minimum Window Substring" problem, we employ a sliding window approach combined with character frequency counting. This method allows us to efficiently find the smallest substring in `s` that contains all characters of `t`, including duplicates.

**Algorithm Steps:**

1. **Initialize Counters:**
   - Use `Counter` from Python's `collections` module to count character frequencies in `t` (`target_counter`).
   - Initialize another `Counter` (`window_counter`) to keep track of character frequencies within the current window in `s`.

2. **Set Pointers and Variables:**
   - `left`: Marks the start of the sliding window.
   - `min_left`: Records the starting index of the minimum window found.
   - `min_size`: Stores the size of the minimum window, initialized to infinity.
   - `needed_char_count`: Counts how many characters in the current window match the required frequency in `t`.

3. **Expand the Window:**
   - Iterate over `s` with the `right` pointer.
   - For each character at `right`, update `window_counter`.
   - If the character is needed (i.e., its count in `window_counter` doesn't exceed its count in `target_counter`), increment `needed_char_count`.

4. **Contract the Window:**
   - While `needed_char_count` equals the length of `t` (indicating all required characters are present in the current window):
     - Check if the current window size is smaller than `min_size`. If so, update `min_size` and `min_left`.
     - Attempt to shrink the window from the left by incrementing `left`. Update `window_counter` and adjust `needed_char_count` accordingly.

5. **Result:**
   - After processing, if `min_left` remains `-1`, it means no valid window was found; return an empty string.
   - Otherwise, return the substring starting at `min_left` with length `min_size`.


```python
from collections import Counter
from math import inf

def minWindow(s: str, t: str) -> str:
    target_counter = Counter(t)
    window_counter = Counter()
    needed_char_count = 0
    left = 0
    min_left = -1
    min_size = inf

    for right, char in enumerate(s):
        window_counter[char] += 1

        if target_counter[char] >= window_counter[char]:
            needed_char_count += 1

        while needed_char_count == len(t):
            if right - left + 1 < min_size:
                min_size = right - left + 1
                min_left = left

            if target_counter[s[left]] >= window_counter[s[left]]:
                needed_char_count -= 1

            window_counter[s[left]] -= 1
            left += 1

    return "" if min_left < 0 else s[min_left : min_left + min_size]
```

**Complexity Analysis**

- Time Complexity: `O(m + n)`. The algorithm processes each character in `s` once, and updating counters is an `O(1)` operation, leading to linear time complexity.
- Space Complexity: `O(C)`, where `C` is the number of unique characters in `s` and `t`.

## Serialize and Deserialize Binary Tree
[LeetCode Question](https://leetcode.com/problems/serialize-and-deserialize-binary-tree)

### Solution

The problem requires converting a binary tree into a string format (serialization) and reconstructing the tree from the string (deserialization).

#### Serialization
- We use a **Depth-First Search (DFS)** approach to traverse the tree.
- If a node exists, append its value to the serialized string.
- If a node is `None`, append `'x'` to represent a null node.
- The traversal follows **preorder (root → left → right)**.
- The serialized values are stored in a list and joined into a space-separated string.

#### Deserialization
- We split the serialized string into a list of tokens.
- Using **DFS**, we reconstruct the tree:
  - If the token is `'x'`, return `None` (indicating a null node).
  - Otherwise, create a new `TreeNode` with the value and recursively build its left and right children.

This approach ensures that we reconstruct the tree in the exact order it was serialized.

```python
class Codec:
    def serialize(self, root):
        """Encodes a tree to a single string.
        
        :type root: TreeNode
        :rtype: str
        """
        res = []

        def dfs(root):
            if not root:
                res.append("x")
                return
            
            res.append(str(root.val))
            dfs(root.left)
            dfs(root.right)
        
        dfs(root)
        return " ".join(res)

    def deserialize(self, data):
        """Decodes your encoded data to tree.
        
        :type data: str
        :rtype: TreeNode
        """
        def dfs(nodes):
            val = next(nodes)
            if val == "x":
                return None
            
            cur = TreeNode(int(val))
            cur.left = dfs(nodes)
            cur.right = dfs(nodes)
            return cur
        
        return dfs(iter(data.split()))
```

**Complexity Analysis**

- Time Complexity: `O(n)`. Both serialization and deserialization visit each node exactly once
- Space Complexity:
  - The serialized string requires `O(n)` space.
  - The recursion stack in DFS takes `O(h)`, where `h` is the tree height.
    - Worst case (skewed tree): `O(n)`
    - Best case (balanced tree): `O(log n)`

## Trapping Rain Water
[LeetCode Question](https://leetcode.com/problems/serialize-and-deserialize-binary-tree)

### Solution

The problem requires finding the total amount of water that can be trapped between bars of different heights after raining.

#### Intuition
The key observation is that the water trapped above a bar depends on the **minimum** of the tallest bars to its **left** and **right**. Specifically, for a bar at index `i`, the trapped water is:

$\text{water}[i] = \max(0, \min(\text{maxLeft}[i], \text{maxRight}[i]) - \text{height}[i])\$

#### Approach

To efficiently compute the trapped water, we use **Dynamic Programming** with two auxiliary arrays:
1. **`max_left[i]`** → Stores the maximum height encountered from the **left** up to index `i`.
2. **`max_right[i]`** → Stores the maximum height encountered from the **right** up to index `i`.

#### Steps
1. **Precompute `max_left`**:
   - Traverse the array from left to right.
   - At each index `i`, `max_left[i]` stores the **maximum** height seen so far.

2. **Precompute `max_right`**:
   - Traverse the array from right to left.
   - At each index `i`, `max_right[i]` stores the **maximum** height seen so far.

3. **Calculate trapped water**:
   - At each index `i`, compute the trapped water using:
     $\text{water}[i] = \min(\text{maxLeft}[i], \text{maxRight}[i]) - \text{height}[i]$
   - Sum the trapped water for all indices.

```python
from typing import List

def trap(self, height: List[int]) -> int:
    if not height:
        return 0

    n = len(height)

    # Arrays to store max heights from left and right
    max_left = [0] * n
    max_right = [0] * n

    # Compute max_left
    max_left[0] = height[0]
    for i in range(1, n):
        max_left[i] = max(max_left[i - 1], height[i])

    # Compute max_right
    max_right[n - 1] = height[n - 1]
    for i in range(n - 2, -1, -1):
        max_right[i] = max(max_right[i + 1], height[i])

    # Compute trapped water
    trapped_water = sum(
        min(max_left[i], max_right[i]) - height[i] for i in range(n)
    )

    return trapped_water
```

**Complexity Analysis**

- Time Complexity: The algorithm processes the array three times (left pass, right pass, and final computation), leading to an overall complexity of `O(n)`.
- Space Complexity: We use two auxiliary arrays (`max_left` and `max_right`), each of size `O(n)`, leading to an `O(n)` space complexity.

## Find Median from Data Stream
[LeetCode Question](https://leetcode.com/problems/find-median-from-data-stream)

### Solution

To efficiently find the median of a continuously growing data stream, we use two heaps:
- A **max heap** (`small`) to store the **smaller** half of the numbers.
- A **min heap** (`large`) to store the **larger** half of the numbers.

This structure ensures that:
- The largest number in the **smaller half** is at the top of `small`.
- The smallest number in the **larger half** is at the top of `large`.
- If the total count is **odd**, the median is the top of the heap with more elements.
- If the total count is **even**, the median is the average of the tops of both heaps.

### Approach

1. **Adding a Number (`addNum`)**:
   - Insert the new number into `small` (max heap) by **negating** its value (Python only has min heaps).
   - Move the largest element of `small` to `large` (balancing step).
   - Ensure that `small` has at least as many elements as `large`. If `large` has more elements, move the smallest element of `large` back to `small`.

2. **Finding the Median (`findMedian`)**:
   - If `small` and `large` are of equal size, return the average of their tops.
   - If `small` has more elements, return its top.

```python
from heapq import heappush, heappop

class MedianFinder:

    def __init__(self):
        # Max heap (store smaller half, negative values)
        self.small = []
        # Min heap (store larger half)
        self.large = []

    def addNum(self, num: int) -> None:
        # Insert into max heap (small), stored as negative for max heap behavior
        heappush(self.small, -num)
        # Move the largest of small to large
        heappush(self.large, -heappop(self.small))

        # Ensure small has at least as many elements as large
        if len(self.large) > len(self.small):
            heappush(self.small, -heappop(self.large))

    def findMedian(self) -> float:
        if len(self.small) == len(self.large):
            return (-self.small[0] + self.large[0]) / 2.0
        return -self.small[0]
```

**Complexity Analysis**

- Time Complexity:
  - `addNum`: `O(log n)`. Each heap operation (heappush / heappop) takes `O(log n)` and we perform at most two heap operations per insertion.
  - `findMedian`: `O(1)`. Peeking the top elements is `O(1)`.

- Space Complexity:
  - `addNum`: `O(n)`.
  - `findMedian`: `O(1)`.

## Word Ladder
[LeetCode Question](https://leetcode.com/problems/word-ladder)

## Solution  

The problem can be visualized as a **graph traversal** where each word is a **node**, and an edge exists between words that differ by a single character. Since we need to find the **shortest** transformation sequence, **Breadth-First Search (BFS)** is the ideal approach, as it explores all possibilities level by level, ensuring the first time we reach the `endWord`, we have used the minimum number of steps.

To optimize this approach, we use **bidirectional BFS**, which starts searching from both `beginWord` and `endWord` simultaneously. This reduces the search space significantly, improving performance over a traditional BFS.

### Approach  

1. **Convert `wordList` to a set** for O(1) lookups.
2. **Initialize two queues** for BFS, starting from both `beginWord` and `endWord`.
3. **Maintain two maps** (`begin_visited` and `end_visited`) to track the shortest distance from each starting word.
4. **Expand the smaller frontier first** to keep the search balanced.
5. **Generate possible transformations** by changing one character at a time.
6. **Check if a word appears in the opposite BFS frontier**—if so, return the sum of distances from both sides.
7. If no transformation sequence is found, return `0`.

```python
from collections import deque

class Solution:
    def ladderLength(self, beginWord: str, endWord: str, wordList: list[str]) -> int:
        def extend_frontier(frontier, other_frontier, visited, other_visited, words):
            for _ in range(len(frontier)):
                word = frontier.popleft()
                steps = visited[word]

                word_chars = list(word)
                for i in range(len(word_chars)):
                    original_char = word_chars[i]
                    for j in range(26):
                        word_chars[i] = chr(ord('a') + j)
                        transformed_word = "".join(word_chars)

                        if transformed_word in other_visited:
                            return steps + 1 + other_visited[transformed_word]

                        if transformed_word in words and transformed_word not in visited:
                            visited[transformed_word] = steps + 1
                            frontier.append(transformed_word)

                    word_chars[i] = original_char
            return -1

        words = set(wordList)
        if endWord not in words:
            return 0

        begin_queue, end_queue = deque([beginWord]), deque([endWord])
        begin_visited, end_visited = {beginWord: 0}, {endWord: 0}

        while begin_queue and end_queue:
            if len(begin_queue) <= len(end_queue):
                result = extend_frontier(begin_queue, end_queue, begin_visited, end_visited, words)
            else:
                result = extend_frontier(end_queue, begin_queue, end_visited, begin_visited, words)

            if result != -1:
                return result + 1

        return 0
```

**Complexity Analysis**

- Time Complexity: `O(L * 26 * N)`.
  - `L` is the length of each word.
  - `N` is the number of words in `wordList` .
  - We iterate over `L` positions, trying 26 different letters, while checking at most `N` words.
  - Bidirectional BFS roughly halves the search space, improving efficiency.

- Space Complexity: `O(N)`.
  - We store visited words in two dictionaries, requiring `O(N)` space.
  - The queues hold at most `O(N)` elements in the worst case.

## Basic Calculator
[LeetCode Question](https://leetcode.com/problems/basic-calculator/)

## Solution  

To evaluate an arithmetic expression with `+`, `-`, and parentheses, we manually parse the string while maintaining an **accumulator**, a **sign tracker**, and a **stack** for handling parentheses.  

### Approach  

1. **Iterate through the string** while processing characters:  
   - If it's a digit, build the full number.
   - If it's `+` or `-`, update the current sign.
   - If it's `(`, push the current result and sign onto the stack (to save the state before the parentheses).
   - If it's `)`, compute the value inside the parentheses and apply the saved sign.

2. **Use a stack to handle nested expressions**  
   - Whenever `(` is encountered, the current result and sign are saved on the stack.  
   - When `)` is encountered, the calculated subexpression is multiplied by the saved sign and added to the saved result.

3. **Final computation**  
   - At the end of iteration, the accumulated `result` will be the final value.  
   - Parentheses are processed during the traversal, ensuring correct precedence handling.

```python
class Solution:
    def calculate(self, s: str) -> int:
        stack = []
        result, sign = 0, 1  # Accumulator and current sign
        index, length = 0, len(s)

        while index < length:
            if s[index].isdigit():
                number = 0
                while index < length and s[index].isdigit():
                    number = number * 10 + int(s[index])
                    index += 1
                result += sign * number
                index -= 1  # Adjust since index was incremented in loop

            elif s[index] == "+":
                sign = 1
            elif s[index] == "-":
                sign = -1
            elif s[index] == "(":
                stack.append(result)
                stack.append(sign)
                result, sign = 0, 1  # Reset result for the new scope
            elif s[index] == ")":
                result *= stack.pop()  # Apply sign before parentheses
                result += stack.pop()  # Add previous result before parentheses

            index += 1

        return result
```

**Complexity Analysis**

- Time Complexity: `O(n)`. We traverse the string once and perform constant-time operations for each character.
- Space Complexity: `O(n)`. In the worst case, the stack stores `O(n)` elements (when dealing with deeply nested parentheses).

##  Maximum Profit in Job Scheduling
[LeetCode Question](https://leetcode.com/problems/maximum-profit-in-job-scheduling)

## Solution  

This problem requires us to **schedule non-overlapping jobs** while maximizing the total profit. The challenge is that jobs have start and end times, so a greedy approach won't necessarily yield the optimal solution.  

### Approach  

1. **Sort the Jobs**  
   - First, sort jobs by their start times. This helps in efficiently finding the next available job when making decisions.

2. **Use Binary Search for Optimization**  
   - Instead of iterating over all possible next jobs, we use **binary search** (`bisect`) to quickly find the next job that starts **after the current job ends**.

3. **Use Recursion with Memoization**  
   - We define a recursive function `dfs(i)` that returns the maximum profit from index `i` onward.
   - We have two choices at each step:  
     - **Skip the current job** and move to the next (`dfs(i + 1)`).  
     - **Take the current job** and add its profit to the result of the next available job (`dfs(nextJobIndex)`).  
   - We use **memoization** (`cache`) to avoid redundant computations.

```python
from typing import List
import bisect

class Solution:
    def jobScheduling(
        self, startTime: List[int], endTime: List[int], profit: List[int]
    ) -> int:
        intervals = sorted(zip(startTime, endTime, profit))
        cache = {}

        def dfs(i):
            if i == len(intervals):
                return 0
            if i in cache:
                return cache[i]

            # Option 1: Skip the current job
            res = dfs(i + 1)

            # Option 2: Take the current job and find the next non-overlapping job
            nextIndex = bisect.bisect(intervals, (intervals[i][1], -1, -1))
            res = max(res, intervals[i][2] + dfs(nextIndex))

            cache[i] = res
            return res

        return dfs(0)
```

**Complexity Analysis**

- Time Complexity: `O(n log n)`.
  - Sorting takes `O(n log n)`.
  - Each job is processed once with a binary search (`O(log n)`) and memoization ensures each job is computed only once.
  - Hence, the total complexity is `O(n log n)`.
- Space Complexity: `O(n)`.
  - The cache stores results for each index `O(n)`.
  - The recursion stack in the worst case is `O(n)`.

## Merge K Sorted Linked Lists
[LeetCode Question](https://leetcode.com/problems/merge-k-sorted-lists)

## Solution

The goal is to merge `k` sorted linked lists into one sorted linked list. An efficient approach is to use a **min heap** to always select the smallest head among the lists.  

### Approach

1. **Initialization:**
   - Create a min heap to store tuples of the form `(node.val, index, node)`, where `index` is the list identifier. This helps break ties when node values are equal.
   - Push the head of each non-empty linked list into the heap.

2. **Merging Process:**
   - Use a dummy node to simplify list manipulation.
   - While the heap is not empty:
     - Pop the smallest element from the heap.
     - Append this node to the merged list.
     - If the popped node has a next node, push it into the heap.

3. **Return Result:**
   - The merged list is obtained from `dummy.next`.



```python
import heapq
from typing import List, Optional

# Definition for singly-linked list.
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def mergeKLists(self, lists: List[Optional[ListNode]]) -> Optional[ListNode]:
        heap = []
        
        # Initialize the heap with the head of each list
        for i, node in enumerate(lists):
            if node:
                heapq.heappush(heap, (node.val, i, node))
        
        dummy = ListNode()
        curr = dummy
        
        # Merge nodes using the heap
        while heap:
            val, i, node = heapq.heappop(heap)
            curr.next = node
            curr = node
            if node.next:
                heapq.heappush(heap, (node.next.val, i, node.next))
        
        return dummy.next
```

**Complexity Analysis**

- Time Complexity: `O(n log k)`.
  - `n` is the total number of nodes across all lists.
  - `k` is the number of linked lists.
  - Each node is processed once, and each heap operation takes `O(log k)` time.
- Space Complexity: `O(k)`. The heap stores at most one node from each list at a time.

## Largest Rectangle in Histogram
[LeetCode Question](https://leetcode.com/problems/largest-rectangle-in-histogram)

## Solution

The key idea is to use a **stack** to maintain a monotonically increasing sequence of heights. For each bar, we determine the maximum rectangle that can be formed with that bar as the smallest height. When a bar with a smaller height is encountered, we pop bars from the stack and calculate the area for each popped bar using the current index as the right boundary and the index stored with the popped bar as the left boundary. Finally, after processing all bars, we pop any remaining bars from the stack and compute their areas using the total length of the histogram.

```python
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        n = len(heights)
        stk = []
        max_area = 0

        for i, height in enumerate(heights):
            start = i
            # If the current height is less than the height of the bar at the top of the stack,
            # then calculate the area for the bars in the stack until the stack is empty or a lower height is found.
            while stk and height < stk[-1][0]:
                h, j = stk.pop()
                w = i - j
                max_area = max(max_area, h * w)
                start = j  # Update start to the leftmost index for the popped height
            stk.append((height, start))
        
        # Process any remaining bars in the stack
        while stk:
            h, j = stk.pop()
            w = n - j
            max_area = max(max_area, h * w)
        
        return max_area
```

**Complexity Analysis**

- Time Complexity: `O(n)`. Each bar is pushed and popped at most once.
- Space Complexity: `O(n)`. The stack stores at most `n` bars.
