# SWE Interview Prep — Question Bank

Questions are organized by difficulty and pattern. Each entry includes:
- **Problem name** (for the session report)
- **Prompt** (the stripped-down, hint-free problem description)
- **Pattern** (the correct answer for Part 1)
- **Why** (the correct reasoning for Part 2)
- **First line(s)** (acceptable initialization for Part 3)

Accept reasonable synonyms, equivalent initializations, and language variations
(Python, JavaScript, Java, Go, etc.) for Part 3.

---

## EASY

### Arrays & Hashing

**Two Sum**
- Prompt: Without using the same element twice, find the indices of two numbers in an array that add up to a target.
- Pattern: Hashmap
- Why: For each number, we need to check whether its complement (target - num) has already been seen. A hashmap gives O(1) lookup so we can do this in a single pass.
- First line(s): `num_map = {}` / `seen = {}` / `Map<Integer, Integer> map = new HashMap<>();`

**Contains Duplicate**
- Prompt: Determine whether any value appears more than once in an array.
- Pattern: Hashset
- Why: We need to detect if we've seen a value before. A set gives O(1) lookup and insertion, so we can check and add in one pass.
- First line(s): `seen = set()` / `Set<Integer> seen = new HashSet<>();`

**Valid Anagram**
- Prompt: Given two strings, determine if one is a rearrangement of the other.
- Pattern: Hashmap / Frequency Count
- Why: An anagram has identical character frequencies. We count the frequency of each character in both strings and compare.
- First line(s): `count = {}` / `from collections import Counter` / `int[] count = new int[26];`

---

### Two Pointers

**Valid Palindrome**
- Prompt: Given a string with mixed characters, determine if it reads the same forwards and backwards ignoring non-alphanumeric characters.
- Pattern: Two Pointers
- Why: We can check from both ends simultaneously. If characters match, move inward. If they don't, it's not a palindrome. O(n) time, O(1) space.
- First line(s): `left, right = 0, len(s) - 1` / `int left = 0, right = s.length() - 1;`

---

### Stack

**Valid Parentheses**
- Prompt: Given a string of brackets, determine if every opening bracket is closed in the correct order.
- Pattern: Stack
- Why: Opening brackets need to be matched by the most recently seen unmatched opener. A stack's LIFO property captures this "most recent" relationship naturally.
- First line(s): `stack = []` / `Deque<Character> stack = new ArrayDeque<>();`

---

### Linked List

**Linked List Cycle**
- Prompt: Determine whether a linked list contains a cycle.
- Pattern: Two Pointers (Fast & Slow / Floyd's)
- Why: A slow pointer and a fast pointer will eventually meet if there's a cycle — the fast pointer "laps" the slow one. No cycle means fast reaches null.
- First line(s): `slow, fast = head, head` / `ListNode slow = head, fast = head;`

**Reverse Linked List**
- Prompt: Reverse a singly linked list in place.
- Pattern: Iterative Pointer Reversal
- Why: We need to redirect each node's next pointer to the previous node. We keep track of prev, curr, and next to do this in one pass without losing references.
- First line(s): `prev, curr = None, head` / `ListNode prev = null, curr = head;`

---

### Binary Search

**Binary Search**
- Prompt: Given a sorted array and a target, return the index of the target or -1 if it doesn't exist.
- Pattern: Binary Search
- Why: The array is sorted, so we can eliminate half the search space at each step by comparing the midpoint to the target.
- First line(s): `left, right = 0, len(nums) - 1` / `int left = 0, right = nums.length - 1;`

---

## MEDIUM

### Arrays & Hashing

**Group Anagrams**
- Prompt: Given a list of strings, group the ones that are anagrams of each other.
- Pattern: Hashmap
- Why: Anagrams share the same sorted character sequence (or frequency tuple). Use that as a key to group them in a hashmap.
- First line(s): `groups = defaultdict(list)` / `Map<String, List<String>> map = new HashMap<>();`

**Top K Frequent Elements**
- Prompt: Given an array, return the k most frequently occurring elements.
- Pattern: Hashmap + Heap (or Bucket Sort)
- Why: Count frequencies with a hashmap, then use a min-heap of size k to efficiently track the top k elements without sorting everything.
- First line(s): `count = Counter(nums)` / `Map<Integer, Integer> count = new HashMap<>();`

**Product of Array Except Self**
- Prompt: Return an array where each element is the product of all other elements, without using division.
- Pattern: Prefix & Suffix Products
- Why: For each index, the answer is the product of everything to its left times everything to its right. We compute these in two passes and combine them.
- First line(s): `result = [1] * len(nums)` / `int[] result = new int[nums.length];`

---

### Two Pointers

**3Sum**
- Prompt: Find all unique triplets in an array that sum to zero.
- Pattern: Two Pointers (with Sort)
- Why: Sort first, then fix one element and use two pointers on the rest to find pairs that sum to its negation. Sorting also makes deduplication easy.
- First line(s): `nums.sort()` / `Arrays.sort(nums);`

**Container With Most Water**
- Prompt: Given an array of heights, find two lines that together with the x-axis form a container that holds the most water.
- Pattern: Two Pointers (Greedy)
- Why: Start with the widest container. To potentially increase area, move the shorter wall inward — moving the taller wall can only decrease area.
- First line(s): `left, right = 0, len(height) - 1` / `int left = 0, right = height.length - 1;`

---

### Sliding Window

**Longest Substring Without Repeating Characters**
- Prompt: Find the length of the longest substring in a string that contains no repeated characters.
- Pattern: Sliding Window
- Why: Maintain a window of unique characters. When we hit a duplicate, shrink from the left until it's gone. A set tracks what's in the current window.
- First line(s): `char_set = set()` / `left = 0` / `Set<Character> seen = new HashSet<>();`

**Minimum Window Substring**
- Prompt: Find the smallest substring of s that contains all characters of string t.
- Pattern: Sliding Window (with Frequency Map)
- Why: Expand the right pointer to include needed characters, then shrink the left pointer once all are covered to find the minimum valid window.
- First line(s): `need = Counter(t)` / `have, want = 0, len(need)` / `Map<Character, Integer> need = new HashMap<>();`

**Longest Repeating Character Replacement**
- Prompt: Given a string, find the length of the longest substring where you can replace at most k characters to make all characters the same.
- Pattern: Sliding Window
- Why: Track the count of the most frequent character in the window. If (window size - max frequency) > k, we need more than k replacements, so shrink from the left.
- First line(s): `count = {}` / `left = max_count = 0` / `int left = 0, maxCount = 0;`

---

### Stack

**Min Stack**
- Prompt: Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.
- Pattern: Stack (Auxiliary Stack)
- Why: Maintain a second stack that tracks the current minimum at each level. When we push, also push the new minimum. Both stacks stay in sync.
- First line(s): `self.stack = []` / `self.min_stack = []`

**Daily Temperatures**
- Prompt: Given daily temperatures, return how many days you have to wait until a warmer temperature. Return 0 if no warmer day exists.
- Pattern: Monotonic Stack
- Why: We need the next greater element for each index. A stack of indices in decreasing temperature order lets us resolve waiting days as soon as we see a warmer day.
- First line(s): `stack = []` / `result = [0] * len(temperatures)`

**Car Fleet**
- Prompt: Cars drive toward a destination at different speeds from different positions. Determine how many fleets arrive together.
- Pattern: Monotonic Stack (with Sort)
- Why: Sort by position descending. Compute time to reach the target for each car. If a car behind catches up (has less or equal time), it joins the fleet ahead — use a stack to track distinct fleet times.
- First line(s): `pairs = sorted(zip(position, speed), reverse=True)` / `stack = []`

---

### Binary Search

**Find Minimum in Rotated Sorted Array**
- Prompt: Find the minimum element in a sorted array that has been rotated at some unknown pivot.
- Pattern: Binary Search
- Why: The minimum is at the inflection point of the rotation. Compare mid to right: if mid > right, the min is in the right half; otherwise it's in the left half (including mid).
- First line(s): `left, right = 0, len(nums) - 1` / `int left = 0, right = nums.length - 1;`

**Search in Rotated Sorted Array**
- Prompt: Search for a target value in a sorted array that has been rotated at an unknown pivot.
- Pattern: Binary Search
- Why: One half of the array is always normally sorted. Identify which half, check if target falls in it, and eliminate the other half. Repeat.
- First line(s): `left, right = 0, len(nums) - 1`

**Koko Eating Bananas**
- Prompt: Find the minimum eating speed (bananas per hour) that lets Koko finish all piles within h hours.
- Pattern: Binary Search on Answer
- Why: The answer (speed) has a monotonic property — if speed k works, k+1 also works. Binary search on the range of possible speeds to find the minimum valid one.
- First line(s): `left, right = 1, max(piles)`

---

### Linked List

**Merge Two Sorted Lists**
- Prompt: Merge two sorted linked lists into one sorted list.
- Pattern: Two Pointers (Iterative Merge)
- Why: Compare the heads of both lists and take the smaller one. Advance that pointer. Repeat until one list is exhausted, then append the rest.
- First line(s): `dummy = ListNode(0)` / `curr = dummy`

**Reorder List**
- Prompt: Reorder a linked list so that nodes alternate between the front half and back half: L0 → Ln → L1 → Ln-1 → ...
- Pattern: Fast & Slow Pointer + Reverse
- Why: Find the midpoint with slow/fast pointers, reverse the second half, then interleave the two halves.
- First line(s): `slow, fast = head, head.next`

**Remove Nth Node From End of List**
- Prompt: Remove the nth node from the end of a linked list.
- Pattern: Two Pointers
- Why: Use two pointers separated by n nodes. When the front reaches the end, the back is at the node to remove.
- First line(s): `dummy = ListNode(0, head)` / `left, right = dummy, head`

---

### Trees

**Invert Binary Tree**
- Prompt: Flip a binary tree so that left and right subtrees are swapped at every node.
- Pattern: DFS (Recursive)
- Why: Swap the children at the current node, then recursively invert each subtree. A simple post-order or pre-order DFS handles this cleanly.
- First line(s): `if not root: return None`

**Maximum Depth of Binary Tree**
- Prompt: Find the maximum depth (number of nodes along the longest root-to-leaf path) of a binary tree.
- Pattern: DFS (Recursive)
- Why: The depth of a node is 1 + the max depth of its two subtrees. Base case: null node returns 0.
- First line(s): `if not root: return 0`

**Binary Tree Level Order Traversal**
- Prompt: Return all values in a binary tree level by level, left to right.
- Pattern: BFS
- Why: Level-by-level traversal is inherently BFS — a queue processes all nodes at one depth before moving to the next.
- First line(s): `queue = deque([root])` / `result = []`

**Lowest Common Ancestor of BST**
- Prompt: In a binary search tree, find the lowest common ancestor of two given nodes.
- Pattern: BST Property / DFS
- Why: In a BST, if both nodes are less than the current node, go left. If both are greater, go right. Otherwise, the current node is the LCA.
- First line(s): `curr = root`

**Validate Binary Search Tree**
- Prompt: Determine whether a binary tree is a valid binary search tree.
- Pattern: DFS with Bounds
- Why: Each node must fall within a valid (min, max) range inherited from its ancestors. Pass these bounds recursively — tighten the upper bound going left, lower bound going right.
- First line(s): `def validate(node, min_val, max_val):` / (or iterative with a stack of `(node, min, max)` tuples)

**Kth Smallest Element in BST**
- Prompt: Find the kth smallest value in a binary search tree.
- Pattern: DFS (In-order Traversal)
- Why: In-order traversal of a BST visits nodes in ascending order. The kth node visited is the kth smallest.
- First line(s): `stack = []` / `curr = root` / `n = 0`

---

### Tries

**Implement Trie**
- Prompt: Design a data structure that supports inserting words, searching for exact words, and checking if any word starts with a given prefix.
- Pattern: Trie
- Why: A trie stores characters as tree nodes, making prefix lookups O(m) where m is word length — faster than hashmap for prefix queries.
- First line(s): `self.children = {}` / `self.is_end = False`

---

### Heap / Priority Queue

**Kth Largest Element in a Stream**
- Prompt: Design a class that finds the kth largest element in a stream of integers as new values are added.
- Pattern: Min-Heap (size k)
- Why: A min-heap of size k always keeps the k largest elements seen so far. The smallest of those (the heap's root) is the kth largest.
- First line(s): `self.heap = nums` / `heapq.heapify(self.heap)`

**Find Median from Data Stream**
- Prompt: Design a data structure that can add integers and return the median at any point.
- Pattern: Two Heaps (Max-heap + Min-heap)
- Why: Maintain a max-heap for the lower half and a min-heap for the upper half. The median is always at the tops of the heaps. Keep them balanced in size.
- First line(s): `self.small = []  # max heap (negate values)` / `self.large = []  # min heap`

**Task Scheduler**
- Prompt: Given a list of tasks and a cooldown period n, find the minimum number of intervals to finish all tasks.
- Pattern: Greedy + Max-Heap
- Why: Always execute the most frequent remaining task to minimize idle time. A max-heap lets us greedily pick the highest-frequency task each interval.
- First line(s): `count = Counter(tasks)` / `max_heap = [-c for c in count.values()]`

---

### Graphs

**Number of Islands**
- Prompt: Given a 2D grid of '1's (land) and '0's (water), count the number of distinct islands.
- Pattern: DFS / BFS (Graph Traversal)
- Why: Each unvisited land cell is the start of a new island. DFS/BFS from it to mark all connected land cells as visited, then count how many times we start a new traversal.
- First line(s): `rows, cols = len(grid), len(grid[0])` / `visited = set()`

**Clone Graph**
- Prompt: Given a reference to a node in a connected undirected graph, return a deep copy of the graph.
- Pattern: DFS / BFS + Hashmap
- Why: Use a hashmap to map original nodes to their clones. DFS/BFS traverses the graph; the map prevents re-cloning already-visited nodes.
- First line(s): `old_to_new = {}`

**Course Schedule**
- Prompt: Given a list of course prerequisites, determine whether it's possible to finish all courses.
- Pattern: Topological Sort / Cycle Detection (DFS)
- Why: This is a cycle detection problem on a directed graph. If there's a cycle in the prerequisite chain, it's impossible to complete all courses.
- First line(s): `adj = defaultdict(list)` / `visited = set()`

**Pacific Atlantic Water Flow**
- Prompt: Find all cells in a matrix where water can flow to both the Pacific and Atlantic ocean.
- Pattern: BFS / DFS (Reverse Flow)
- Why: Instead of simulating flow from each cell, work backwards — BFS/DFS from the ocean borders inward. Cells reachable from both borders are the answer.
- First line(s): `pacific = set()` / `atlantic = set()`

**Walls and Gates**
- Prompt: Fill each empty room in a grid with the distance to its nearest gate. Leave walls unchanged.
- Pattern: BFS (Multi-source)
- Why: Start BFS simultaneously from all gates. BFS guarantees shortest path, and multi-source BFS fills all rooms with correct distances in one pass.
- First line(s): `queue = deque()` / (populate queue with all gate positions first)

---

### Dynamic Programming

**Climbing Stairs**
- Prompt: You can climb 1 or 2 steps at a time. How many distinct ways can you reach the top of n stairs?
- Pattern: Dynamic Programming (1D)
- Why: The number of ways to reach step n is the sum of ways to reach step n-1 and n-2 — it's Fibonacci. We only need the last two values, so O(1) space.
- First line(s): `one, two = 1, 1` / `int[] dp = new int[n + 1];`

**House Robber**
- Prompt: Given an array of house values, find the maximum amount you can rob without robbing two adjacent houses.
- Pattern: Dynamic Programming (1D)
- Why: At each house, we choose to rob it (add its value + best from two houses ago) or skip it (take the best from the previous house). We only need the last two results.
- First line(s): `prev, curr = 0, 0` / `rob1, rob2 = 0, 0`

**Coin Change**
- Prompt: Given coin denominations and a target amount, find the minimum number of coins needed to make that amount.
- Pattern: Dynamic Programming (Bottom-Up)
- Why: For each amount from 1 to target, try every coin and take the minimum. Subproblems (smaller amounts) must be solved first — classic bottom-up DP.
- First line(s): `dp = [float('inf')] * (amount + 1)` / `dp[0] = 0`

**Longest Common Subsequence**
- Prompt: Find the length of the longest subsequence present in both strings (characters don't need to be contiguous).
- Pattern: Dynamic Programming (2D)
- Why: Build a 2D table where dp[i][j] is the LCS of the first i characters of s1 and j characters of s2. If characters match, extend; otherwise take the max of excluding one.
- First line(s): `dp = [[0] * (len(text2) + 1) for _ in range(len(text1) + 1)]`

**Word Break**
- Prompt: Given a string and a dictionary of words, determine if the string can be segmented into dictionary words.
- Pattern: Dynamic Programming
- Why: dp[i] is true if s[0:i] can be segmented. For each position, check all substrings ending there — if a valid prefix exists and the current word is in the dict, dp[i] = true.
- First line(s): `dp = [False] * (len(s) + 1)` / `dp[0] = True`

**Unique Paths**
- Prompt: Count the number of unique paths from the top-left to the bottom-right of an m×n grid, moving only right or down.
- Pattern: Dynamic Programming (2D)
- Why: Each cell can only be reached from the cell above or the cell to the left. dp[i][j] = dp[i-1][j] + dp[i][j-1]. First row and column are all 1s.
- First line(s): `dp = [[1] * n for _ in range(m)]`

**Jump Game**
- Prompt: Given an array where each element is the max jump length from that position, determine if you can reach the last index.
- Pattern: Greedy
- Why: Track the farthest index reachable so far. At each step, update it. If the current index exceeds the farthest reachable, we're stuck.
- First line(s): `max_reach = 0`

---

### Backtracking

**Combination Sum**
- Prompt: Given a list of distinct integers and a target, find all unique combinations that sum to the target. Each number may be used unlimited times.
- Pattern: Backtracking
- Why: Explore all combinations recursively. At each step, either include the current element (and stay at it, since reuse is allowed) or move to the next. Prune when sum exceeds target.
- First line(s): `result = []` / `def backtrack(start, current, total):`

**Subsets**
- Prompt: Return all possible subsets of a list of unique integers.
- Pattern: Backtracking
- Why: At each element, we either include it or not. Recursively build all combinations — the decision tree naturally generates the power set.
- First line(s): `result = []` / `def backtrack(start, current):`

**Permutations**
- Prompt: Return all possible permutations of a list of distinct integers.
- Pattern: Backtracking
- Why: At each step, try placing each unused number in the current position. Recurse to fill the rest. Backtrack by un-choosing the number.
- First line(s): `result = []` / `def backtrack(current):`

**Word Search**
- Prompt: Given a 2D board of characters, determine if a given word exists by connecting adjacent cells horizontally or vertically. Each cell may only be used once.
- Pattern: Backtracking + DFS
- Why: Try starting the word from each cell. DFS explores adjacent cells recursively. Mark cells visited to prevent reuse; unmark on backtrack.
- First line(s): `rows, cols = len(board), len(board[0])`

---

### Intervals

**Merge Intervals**
- Prompt: Given a list of intervals, merge all overlapping ones.
- Pattern: Greedy (Sort + Sweep)
- Why: Sort by start time. Then sweep through — if the current interval overlaps the last merged one (current start ≤ last end), extend it. Otherwise, add a new interval.
- First line(s): `intervals.sort(key=lambda x: x[0])` / `merged = [intervals[0]]`

**Insert Interval**
- Prompt: Insert a new interval into a sorted list of non-overlapping intervals, merging if necessary.
- Pattern: Greedy (Linear Sweep)
- Why: Add all intervals that end before the new one starts, merge all that overlap, then add the rest. One linear pass.
- First line(s): `result = []` / `i = 0`

**Meeting Rooms II**
- Prompt: Given meeting time intervals, find the minimum number of conference rooms required.
- Pattern: Greedy + Min-Heap
- Why: Sort by start time. Use a min-heap tracking end times of ongoing meetings. If the earliest-ending meeting is done before the next starts, reuse its room; otherwise add a new room.
- First line(s): `intervals.sort(key=lambda x: x[0])` / `heap = []`

---

## HARD

### Arrays

**Trapping Rain Water**
- Prompt: Given an elevation map, compute how much water can be trapped between the bars after rain.
- Pattern: Two Pointers
- Why: Water at any point is limited by the shorter of the max heights to its left and right. Use two pointers moving inward, always processing the side with the shorter max wall.
- First line(s): `left, right = 0, len(height) - 1` / `left_max = right_max = 0`

**Sliding Window Maximum**
- Prompt: Given an array and window size k, return the maximum value in each sliding window.
- Pattern: Monotonic Deque
- Why: A deque stores indices of potentially useful elements in decreasing order of value. The front is always the current window's max. Pop elements that are out of range or smaller than the new element.
- First line(s): `from collections import deque` / `dq = deque()` / `result = []`

---

### Dynamic Programming

**Longest Increasing Subsequence**
- Prompt: Find the length of the longest strictly increasing subsequence in an array.
- Pattern: Dynamic Programming (or Binary Search)
- Why: dp[i] = length of LIS ending at index i. For each i, check all j < i where nums[j] < nums[i] and take the max. O(n²) DP or O(n log n) with binary search on a patience-sort array.
- First line(s): `dp = [1] * len(nums)`

**Edit Distance**
- Prompt: Given two strings, find the minimum number of insertions, deletions, or substitutions to convert one to the other.
- Pattern: Dynamic Programming (2D)
- Why: dp[i][j] = min edits to convert word1[0:i] to word2[0:j]. If characters match, no edit needed. Otherwise, take the min of insert, delete, or replace.
- First line(s): `dp = [[0] * (len(word2) + 1) for _ in range(len(word1) + 1)]`

---

### Graphs

**Word Ladder**
- Prompt: Find the shortest transformation sequence from a start word to an end word, changing one letter at a time, where each intermediate word must be in the dictionary.
- Pattern: BFS (Shortest Path)
- Why: Each word is a node; edges connect words that differ by one letter. BFS finds the shortest path between start and end words.
- First line(s): `queue = deque([(beginWord, 1)])` / `visited = {beginWord}`

**Alien Dictionary**
- Prompt: Given a list of words sorted in an alien language's alphabetical order, determine the order of characters in the alien alphabet.
- Pattern: Topological Sort
- Why: Compare adjacent words to derive ordering constraints (character A comes before character B). Then topological sort the resulting directed graph to get the full order.
- First line(s): `adj = {c: set() for w in words for c in w}` / `in_degree = {c: 0 for c in adj}`
