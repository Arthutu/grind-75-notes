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

## Invert Binary Tree
[LeetCode Question](https://leetcode.com/problems/invert-binary-tree/description/)

#### Solutions:
1. **Depth-first Search (DFS)**: Recursively traverse the tree and swap the left and right subtrees.
2. **Breadth-first Search (BFS)**: 

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
