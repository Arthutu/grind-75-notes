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
2. **Breadth-first Search (BFS)**: 

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
