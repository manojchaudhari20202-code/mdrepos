# DSA Interview Notes — Python
> Complete reference for Arrays & Strings, Linked Lists, Stacks & Queues, Hashing & Heaps

---

# Table of Contents

1. [Arrays & Strings](#1-arrays--strings)
   - [Array Insert / Delete](#11-array-insert--delete)
   - [Array Reversal](#12-array-reversal)
   - [2D Arrays](#13-2d-arrays-matrices)
   - [Pattern Matching](#14-pattern-matching)
   - [Array Rotation](#15-array-rotation)
   - [Palindrome Check](#16-palindrome-check)
2. [Linked Lists](#2-linked-lists)
   - [Singly Linked List](#21-singly-linked-list)
   - [Doubly Linked List](#22-doubly-linked-list)
   - [Cycle Detection](#23-cycle-detection)
   - [List Reversal](#24-list-reversal)
   - [Fast / Slow Pointers](#25-fast--slow-pointers)
   - [Merge Lists](#26-merge-lists)
3. [Stacks & Queues](#3-stacks--queues)
   - [Stack Implementation](#31-stack-implementation)
   - [Queue Implementation](#32-queue-implementation)
   - [Parentheses Matching](#33-parentheses-matching)
   - [Expression Evaluation](#34-expression-evaluation)
   - [Next Greater Element](#35-next-greater-element)
   - [Circular Queue](#36-circular-queue)
4. [Hashing & Heaps](#4-hashing--heaps)
   - [HashMap / HashSet](#41-hashmap--hashset)
   - [Hash-Based Problems](#42-coding-hash-based-problems)
   - [Min Heap / Max Heap](#43-min-heap--max-heap)
   - [Priority Queue](#44-priority-queue)
   - [Heapify Operations](#45-heapify-operations)
   - [Heap Sort](#46-heap-sort)

---

# 1. Arrays & Strings

---

## 1.1 Array Insert / Delete

### Key Concepts
- Python lists are **dynamic arrays** (amortized O(1) append, O(n) insert/delete at arbitrary index)
- Underlying array doubles in size when capacity exceeded → **amortized O(1)** for append
- Insert/delete at index i → shifts all elements after i → **O(n)**

### Interview Details
- Always clarify: insert at index, at end, in sorted order?
- Deletion by **value** vs deletion by **index** — different operations
- Watch for **index out of bounds** edge cases
- In-place vs returning new array

### Time/Space Complexity

| Operation | Time | Space |
|---|---|---|
| Insert at end | O(1) amortized | O(1) |
| Insert at index i | O(n) | O(1) |
| Delete by index | O(n) | O(1) |
| Delete by value | O(n) | O(1) |

```python
# ---- INSERT ----
arr = [1, 2, 4, 5]

arr.append(6)           # Insert at end → [1, 2, 4, 5, 6]
arr.insert(2, 3)        # Insert at index 2 → [1, 2, 3, 4, 5, 6]

# Manual insert without built-ins (common in interviews)
def insert_at(arr, index, val):
    arr.append(None)                    # Extend length
    for i in range(len(arr)-1, index, -1):
        arr[i] = arr[i-1]              # Shift right
    arr[index] = val
    return arr

# ---- DELETE ----
arr = [1, 2, 3, 4, 5]

arr.pop()               # Remove last → O(1)
arr.pop(2)              # Remove at index 2 → O(n)
arr.remove(3)           # Remove first occurrence of value 3 → O(n)

# Manual delete without built-ins
def delete_at(arr, index):
    for i in range(index, len(arr)-1):
        arr[i] = arr[i+1]              # Shift left
    arr.pop()                           # Remove last (now duplicate)
    return arr

# ---- Edge cases to mention ----
arr = []
# arr.pop()     → IndexError
# arr.remove(x) → ValueError if x not in arr
```

---

## 1.2 Array Reversal

### Key Concepts
- **Two-pointer technique** — swap left and right, move inward
- Python has `arr[::-1]` (returns new) and `arr.reverse()` (in-place)
- Interviews often ask for **in-place** reversal with O(1) extra space

### Interview Details
- Clarify: in-place vs new array
- Reversing a **subarray** (between indices l and r) — very common variant
- Reversing **words in a sentence** = split → reverse list → join
- Rotation problems often reduce to **three reversals**

```python
# ---- Basic reversal ----
arr = [1, 2, 3, 4, 5]

# Pythonic (new array)
rev = arr[::-1]               # [5, 4, 3, 2, 1]

# Built-in in-place
arr.reverse()                  # Modifies arr directly

# Two-pointer (interview preferred)
def reverse_array(arr):
    left, right = 0, len(arr) - 1
    while left < right:
        arr[left], arr[right] = arr[right], arr[left]
        left += 1
        right -= 1
    return arr

# ---- Reverse a subarray [l, r] ----
def reverse_subarray(arr, l, r):
    while l < r:
        arr[l], arr[r] = arr[r], arr[l]
        l += 1
        r -= 1
    return arr

arr = [1, 2, 3, 4, 5]
reverse_subarray(arr, 1, 3)   # [1, 4, 3, 2, 5]

# ---- Reverse words in a sentence ----
s = "hello world foo"
result = " ".join(s.split()[::-1])   # "foo world hello"

# ---- Recursive reversal ----
def reverse_recursive(arr, l=0, r=None):
    if r is None:
        r = len(arr) - 1
    if l >= r:
        return arr
    arr[l], arr[r] = arr[r], arr[l]
    return reverse_recursive(arr, l+1, r-1)
```

---

## 1.3 2D Arrays (Matrices)

### Key Concepts
- Represented as **list of lists** in Python
- Row-major order: `matrix[row][col]`
- Common operations: transpose, rotate 90°, spiral traversal, search
- **In-place rotation** = transpose + reverse rows (or reverse cols + transpose)

### Interview Details
- Always confirm: is it square (n×n) or rectangular (m×n)?
- Off-by-one errors at boundaries — very common bug
- Layer-by-layer thinking for rotation
- BFS/DFS on 2D grids → treat as graph with 4 or 8 neighbors

```python
# ---- Create & access ----
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]
rows, cols = len(matrix), len(matrix[0])
print(matrix[1][2])   # 6  → row 1, col 2

# ---- Transpose (swap rows and cols) ----
def transpose(matrix):
    n = len(matrix)
    for i in range(n):
        for j in range(i+1, n):             # Only upper triangle
            matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
    return matrix

# Pythonic transpose
transposed = [list(row) for row in zip(*matrix)]

# ---- Rotate 90° clockwise (in-place) ----
# Step 1: Transpose
# Step 2: Reverse each row
def rotate_90_clockwise(matrix):
    n = len(matrix)
    # Transpose
    for i in range(n):
        for j in range(i+1, n):
            matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
    # Reverse each row
    for row in matrix:
        row.reverse()
    return matrix

# Rotate 90° counter-clockwise = reverse each row, then transpose

# ---- Spiral order traversal ----
def spiral_order(matrix):
    result = []
    top, bottom = 0, len(matrix) - 1
    left, right = 0, len(matrix[0]) - 1

    while top <= bottom and left <= right:
        for col in range(left, right + 1):       # Left → Right
            result.append(matrix[top][col])
        top += 1
        for row in range(top, bottom + 1):       # Top → Bottom
            result.append(matrix[row][right])
        right -= 1
        if top <= bottom:
            for col in range(right, left - 1, -1):   # Right → Left
                result.append(matrix[bottom][col])
            bottom -= 1
        if left <= right:
            for row in range(bottom, top - 1, -1):   # Bottom → Top
                result.append(matrix[row][left])
            left += 1
    return result

# ---- Search in row-sorted, col-sorted matrix ----
# Start at top-right corner: O(m+n)
def search_matrix(matrix, target):
    row, col = 0, len(matrix[0]) - 1
    while row < len(matrix) and col >= 0:
        if matrix[row][col] == target:
            return (row, col)
        elif matrix[row][col] > target:
            col -= 1
        else:
            row += 1
    return (-1, -1)

# ---- 4-directional neighbors (BFS/DFS on grid) ----
def get_neighbors(r, c, rows, cols):
    directions = [(0,1),(0,-1),(1,0),(-1,0)]
    return [(r+dr, c+dc) for dr,dc in directions
            if 0 <= r+dr < rows and 0 <= c+dc < cols]
```

---

## 1.4 Pattern Matching

### Key Concepts
- **Naive approach**: O(n×m) — slide pattern over text, compare char by char
- **KMP (Knuth-Morris-Pratt)**: O(n+m) — uses failure function (prefix table)
- **Rabin-Karp**: O(n+m) average — rolling hash
- Python's `in` operator and `str.find()` use optimized C — mention but code naive/KMP in interviews

### Interview Details
- KMP is the gold standard for interviews asking O(n+m)
- Know how to build the **LPS (Longest Proper Prefix which is also Suffix)** array
- Rabin-Karp useful when asked about hashing approach
- Wildcard / regex matching → DP approach

```python
# ---- Naive O(n*m) ----
def naive_search(text, pattern):
    n, m = len(text), len(pattern)
    matches = []
    for i in range(n - m + 1):
        if text[i:i+m] == pattern:
            matches.append(i)
    return matches

# ---- KMP O(n+m) ----
def build_lps(pattern):
    """
    LPS[i] = length of longest proper prefix of pattern[0..i]
             that is also a suffix
    """
    m = len(pattern)
    lps = [0] * m
    length = 0    # length of previous longest prefix-suffix
    i = 1
    while i < m:
        if pattern[i] == pattern[length]:
            length += 1
            lps[i] = length
            i += 1
        else:
            if length != 0:
                length = lps[length - 1]   # Fallback — key insight
            else:
                lps[i] = 0
                i += 1
    return lps

def kmp_search(text, pattern):
    n, m = len(text), len(pattern)
    lps = build_lps(pattern)
    matches = []
    i = j = 0                # i → text, j → pattern
    while i < n:
        if text[i] == pattern[j]:
            i += 1
            j += 1
        if j == m:
            matches.append(i - j)
            j = lps[j - 1]              # Continue with fallback
        elif i < n and text[i] != pattern[j]:
            if j != 0:
                j = lps[j - 1]
            else:
                i += 1
    return matches

# Example
text = "aabaacaadaabaaba"
pattern = "aaba"
print(kmp_search(text, pattern))   # [0, 9, 12]

# ---- Rabin-Karp (rolling hash) ----
def rabin_karp(text, pattern, base=256, mod=10**9+7):
    n, m = len(text), len(pattern)
    if m > n:
        return []
    h_pat = h_txt = 0
    high = pow(base, m-1, mod)        # base^(m-1) for rolling
    matches = []

    for i in range(m):                # Initial hash
        h_pat = (base * h_pat + ord(pattern[i])) % mod
        h_txt = (base * h_txt + ord(text[i])) % mod

    for i in range(n - m + 1):
        if h_pat == h_txt:
            if text[i:i+m] == pattern:   # Verify (handle collision)
                matches.append(i)
        if i < n - m:
            h_txt = (base*(h_txt - ord(text[i])*high) + ord(text[i+m])) % mod

    return matches

# ---- Wildcard matching with DP ----
# '?' matches single char, '*' matches any sequence
def wildcard_match(s, p):
    m, n = len(s), len(p)
    dp = [[False]*(n+1) for _ in range(m+1)]
    dp[0][0] = True
    for j in range(1, n+1):
        if p[j-1] == '*':
            dp[0][j] = dp[0][j-1]
    for i in range(1, m+1):
        for j in range(1, n+1):
            if p[j-1] == '*':
                dp[i][j] = dp[i-1][j] or dp[i][j-1]
            elif p[j-1] == '?' or p[j-1] == s[i-1]:
                dp[i][j] = dp[i-1][j-1]
    return dp[m][n]
```

---

## 1.5 Array Rotation

### Key Concepts
- **Left rotation by k**: first k elements move to end
- **Right rotation by k**: last k elements move to front
- Most efficient: **Three-reversal algorithm** — O(n) time, O(1) space
- Alternative: Extra array copy — O(n) time, O(n) space

### Interview Details
- Normalize k: `k = k % n` (rotating by n = no change)
- Clarify direction: left or right?
- Three-reversal is the expected optimal answer
- Rotation is related to **circular array** problems

```python
# ---- Three-Reversal Algorithm ----
# Right rotate by k:
#   1. Reverse entire array
#   2. Reverse first k elements
#   3. Reverse remaining n-k elements

def reverse(arr, l, r):
    while l < r:
        arr[l], arr[r] = arr[r], arr[l]
        l += 1; r -= 1

def rotate_right(arr, k):
    n = len(arr)
    k = k % n                  # Handle k >= n
    reverse(arr, 0, n-1)       # Reverse all
    reverse(arr, 0, k-1)       # Reverse first k
    reverse(arr, k, n-1)       # Reverse rest
    return arr

def rotate_left(arr, k):
    n = len(arr)
    k = k % n
    reverse(arr, 0, k-1)       # Reverse first k
    reverse(arr, k, n-1)       # Reverse rest
    reverse(arr, 0, n-1)       # Reverse all
    return arr

arr = [1, 2, 3, 4, 5, 6, 7]
print(rotate_right(arr[:], 3))   # [5, 6, 7, 1, 2, 3, 4]
print(rotate_left(arr[:], 3))    # [4, 5, 6, 7, 1, 2, 3]

# ---- Slice approach (O(n) space) ----
def rotate_right_slice(arr, k):
    n = len(arr)
    k = k % n
    return arr[-k:] + arr[:-k]

def rotate_left_slice(arr, k):
    n = len(arr)
    k = k % n
    return arr[k:] + arr[:k]

# ---- Check if one array is rotation of another ----
def is_rotation(a, b):
    if len(a) != len(b):
        return False
    doubled = a + a
    return is_subarray(doubled, b)

def is_subarray(text, pattern):
    n, m = len(text), len(pattern)
    for i in range(n - m + 1):
        if text[i:i+m] == pattern:
            return True
    return False

# ---- Find minimum in rotated sorted array ----
# Binary search approach: O(log n)
def find_min_rotated(arr):
    lo, hi = 0, len(arr) - 1
    while lo < hi:
        mid = (lo + hi) // 2
        if arr[mid] > arr[hi]:
            lo = mid + 1          # Min is in right half
        else:
            hi = mid              # Min is in left half (including mid)
    return arr[lo]
```

---

## 1.6 Palindrome Check

### Key Concepts
- **String**: equal to its reverse; use two pointers from ends inward
- **Number**: convert to string OR reverse digits mathematically
- Ignore non-alphanumeric + case-insensitive for "valid palindrome" variants
- **Longest palindromic substring** → expand-around-center: O(n²) / Manacher's: O(n)

### Interview Details
- Two-pointer is O(n) time O(1) space — always preferred over slicing
- Watch for: empty string (palindrome), single char (palindrome), even vs odd length
- Manacher's algorithm rarely asked to code, but good to mention it exists
- **Palindrome partitioning** and **count palindromic substrings** use DP

```python
# ---- Basic string palindrome ----
def is_palindrome(s):
    left, right = 0, len(s) - 1
    while left < right:
        if s[left] != s[right]:
            return False
        left += 1
        right -= 1
    return True

# Pythonic (O(n) space — new string created)
def is_palindrome_pythonic(s):
    return s == s[::-1]

# ---- Valid palindrome (alphanumeric only, case-insensitive) ----
# LeetCode #125 style
def is_valid_palindrome(s):
    left, right = 0, len(s) - 1
    while left < right:
        while left < right and not s[left].isalnum():
            left += 1
        while left < right and not s[right].isalnum():
            right -= 1
        if s[left].lower() != s[right].lower():
            return False
        left += 1
        right -= 1
    return True

print(is_valid_palindrome("A man, a plan, a canal: Panama"))  # True

# ---- Number palindrome (no string conversion) ----
def is_palindrome_number(x):
    if x < 0 or (x % 10 == 0 and x != 0):
        return False
    reversed_half = 0
    while x > reversed_half:
        reversed_half = reversed_half * 10 + x % 10
        x //= 10
    return x == reversed_half or x == reversed_half // 10

# ---- Longest Palindromic Substring (expand around center) ----
def longest_palindrome(s):
    def expand(l, r):
        while l >= 0 and r < len(s) and s[l] == s[r]:
            l -= 1
            r += 1
        return s[l+1:r]          # Slice after loop exits

    result = ""
    for i in range(len(s)):
        odd  = expand(i, i)       # Odd length center
        even = expand(i, i+1)     # Even length center
        if len(odd)  > len(result): result = odd
        if len(even) > len(result): result = even
    return result

print(longest_palindrome("babad"))   # "bab" or "aba"
print(longest_palindrome("cbbd"))    # "bb"

# ---- Count palindromic substrings ----
def count_palindromes(s):
    count = 0
    def expand(l, r):
        nonlocal count
        while l >= 0 and r < len(s) and s[l] == s[r]:
            count += 1
            l -= 1; r += 1
    for i in range(len(s)):
        expand(i, i)         # Odd
        expand(i, i+1)       # Even
    return count

print(count_palindromes("aaa"))   # 6

# ---- Palindrome DP (for partitioning problems) ----
def build_palindrome_dp(s):
    n = len(s)
    dp = [[False]*n for _ in range(n)]
    for i in range(n):
        dp[i][i] = True                  # Single chars
    for i in range(n-1):
        dp[i][i+1] = (s[i] == s[i+1])   # Pairs
    for length in range(3, n+1):
        for i in range(n - length + 1):
            j = i + length - 1
            dp[i][j] = (s[i] == s[j]) and dp[i+1][j-1]
    return dp

# dp[i][j] = True means s[i..j] is a palindrome
```

### Arrays & Strings — Quick Complexity Cheatsheet

| Problem | Best Algorithm | Time | Space |
|---|---|---|---|
| Insert/Delete | Two-pointer / shift | O(n) | O(1) |
| Reversal | Two-pointer | O(n) | O(1) |
| 2D Rotation | Transpose + Reverse | O(n²) | O(1) |
| Pattern Matching | KMP | O(n+m) | O(m) |
| Array Rotation | Three-Reversal | O(n) | O(1) |
| Palindrome Check | Two-pointer | O(n) | O(1) |
| Longest Palindrome | Expand Center | O(n²) | O(1) |

### Arrays & Strings — Common Interview Traps & Tips

- **k % n** before any rotation — always normalize
- **Off-by-one** in 2D matrix bounds — draw it out first
- Palindrome: **even vs odd** length centers — two separate expand calls
- KMP LPS array — the fallback `length = lps[length-1]` line is what most candidates miss
- For 2D grid problems, always write the bounds check helper first
- Pattern matching: Python's `in` is O(n×m) worst case in CPython — always mention KMP as the theoretically optimal solution

---

# 2. Linked Lists

---

## Core Node Definitions

```python
# Singly linked list node
class ListNode:
    def __init__(self, val=0, next=None):
        self.val  = val
        self.next = next

# Doubly linked list node
class DListNode:
    def __init__(self, val=0, prev=None, next=None):
        self.val  = val
        self.prev = prev
        self.next = next

# Helper: build linked list from array
def build_list(arr):
    if not arr: return None
    head = ListNode(arr[0])
    cur  = head
    for val in arr[1:]:
        cur.next = ListNode(val)
        cur = cur.next
    return head

# Helper: convert linked list to array (for testing)
def to_array(head):
    result = []
    while head:
        result.append(head.val)
        head = head.next
    return result
```

---

## 2.1 Singly Linked List

### Key Concepts
- Each node holds **val + next pointer**
- No random access — must traverse from head: **O(n)**
- Insertion/deletion at known node: **O(1)** (once you have the pointer)
- Head pointer is the **only entry point** — never lose it

### Interview Details
- Always handle: **empty list**, **single node**, **head/tail operations**
- Most bugs come from losing the head reference or null pointer dereference
- **Dummy head node** pattern eliminates edge cases for head deletion/insertion
- Draw the pointer manipulation before coding

### Time/Space Complexity

| Operation | Time | Space |
|---|---|---|
| Access by index | O(n) | O(1) |
| Insert at head | O(1) | O(1) |
| Insert at tail | O(n) | O(1) |
| Insert at index | O(n) | O(1) |
| Delete by value | O(n) | O(1) |
| Search | O(n) | O(1) |

```python
class SinglyLinkedList:
    def __init__(self):
        self.head = None
        self.size = 0

    # ---- Insert ----
    def insert_at_head(self, val):
        node = ListNode(val)
        node.next = self.head
        self.head = node
        self.size += 1

    def insert_at_tail(self, val):
        node = ListNode(val)
        if not self.head:
            self.head = node
            self.size += 1
            return
        cur = self.head
        while cur.next:             # Walk to last node
            cur = cur.next
        cur.next = node
        self.size += 1

    def insert_at_index(self, index, val):
        if index < 0 or index > self.size:
            raise IndexError("Index out of bounds")
        if index == 0:
            self.insert_at_head(val)
            return
        dummy = ListNode(0)         # Dummy head simplifies edge cases
        dummy.next = self.head
        cur = dummy
        for _ in range(index):
            cur = cur.next
        node = ListNode(val)
        node.next = cur.next
        cur.next = node
        self.size += 1

    # ---- Delete ----
    def delete_by_value(self, val):
        if not self.head:
            return
        if self.head.val == val:    # Edge case: delete head
            self.head = self.head.next
            self.size -= 1
            return
        cur = self.head
        while cur.next:
            if cur.next.val == val:
                cur.next = cur.next.next    # Bypass the node
                self.size -= 1
                return
            cur = cur.next

    # Dummy head pattern — cleaner, handles head deletion uniformly
    def delete_by_value_dummy(self, val):
        dummy = ListNode(0)
        dummy.next = self.head
        cur = dummy
        while cur.next:
            if cur.next.val == val:
                cur.next = cur.next.next
                self.size -= 1
                break
            cur = cur.next
        self.head = dummy.next

    # ---- Search ----
    def search(self, val):
        cur, index = self.head, 0
        while cur:
            if cur.val == val:
                return index
            cur = cur.next
            index += 1
        return -1

    # ---- Get kth from end ----
    # Two-pointer: advance first pointer k steps, then move both
    def kth_from_end(self, k):
        fast = slow = self.head
        for _ in range(k):
            if not fast:
                return None          # k > length
            fast = fast.next
        while fast:
            fast = fast.next
            slow = slow.next
        return slow.val              # slow is now kth from end

# ---- Delete node given only that node (no head) ----
# Classic trick: copy next node's val, delete next node
def delete_node(node):
    # Assumption: node is NOT the tail
    node.val  = node.next.val
    node.next = node.next.next
```

---

## 2.2 Doubly Linked List

### Key Concepts
- Each node holds **val + prev + next pointers**
- Can traverse **both directions** — O(1) delete given a node pointer
- More memory per node (extra prev pointer)
- Basis for **LRU Cache** (dict + doubly linked list)

### Interview Details
- When inserting/deleting always update **both** prev and next pointers — most common bug
- Sentinel nodes (dummy head + dummy tail) eliminate ALL boundary conditions
- Know the LRU cache implementation — extremely common interview question

```python
class DoublyLinkedList:
    def __init__(self):
        # Sentinel nodes — never removed
        self.head = DListNode(0)     # Dummy head
        self.tail = DListNode(0)     # Dummy tail
        self.head.next = self.tail
        self.tail.prev = self.head
        self.size = 0

    def _insert_after(self, node, new_node):
        """Insert new_node immediately after node."""
        new_node.prev = node
        new_node.next = node.next
        node.next.prev = new_node    # Update successor's prev
        node.next = new_node         # Update node's next
        self.size += 1

    def _remove(self, node):
        """Remove an arbitrary node in O(1)."""
        node.prev.next = node.next
        node.next.prev = node.prev
        self.size -= 1

    def insert_at_front(self, val):
        self._insert_after(self.head, DListNode(val))

    def insert_at_back(self, val):
        self._insert_after(self.tail.prev, DListNode(val))

    def remove_front(self):
        if self.size == 0:
            return None
        node = self.head.next
        self._remove(node)
        return node.val

    def remove_back(self):
        if self.size == 0:
            return None
        node = self.tail.prev
        self._remove(node)
        return node.val

    def to_array(self):
        result = []
        cur = self.head.next
        while cur != self.tail:
            result.append(cur.val)
            cur = cur.next
        return result

# ---- LRU Cache (dict + doubly linked list) ----
# O(1) get and put
class LRUCache:
    def __init__(self, capacity):
        self.cap   = capacity
        self.cache = {}              # key → node
        self.head  = DListNode(0)    # Dummy: MRU side
        self.tail  = DListNode(0)    # Dummy: LRU side
        self.head.next = self.tail
        self.tail.prev = self.head

    def _remove(self, node):
        node.prev.next = node.next
        node.next.prev = node.prev

    def _insert_front(self, node):   # Insert as most recently used
        node.next = self.head.next
        node.prev = self.head
        self.head.next.prev = node
        self.head.next = node

    def get(self, key):
        if key not in self.cache:
            return -1
        node = self.cache[key]
        self._remove(node)
        self._insert_front(node)     # Mark as recently used
        return node.val

    def put(self, key, value):
        if key in self.cache:
            self._remove(self.cache[key])
        node = DListNode(value)
        node.key = key
        self.cache[key] = node
        self._insert_front(node)
        if len(self.cache) > self.cap:
            lru = self.tail.prev     # Least recently used
            self._remove(lru)
            del self.cache[lru.key]
```

---

## 2.3 Cycle Detection

### Key Concepts
- **Floyd's Tortoise and Hare** — two pointers, slow moves 1 step, fast moves 2
- If they meet → cycle exists
- To find **cycle start**: reset slow to head, advance both 1 step at a time — they meet at cycle entry
- **Proof**: if cycle length is C and pre-cycle length is F, when fast and slow meet inside cycle, slow has traveled F+a steps. Resetting slow to head and advancing both by 1 guarantees meeting at cycle start after F more steps

### Interview Details
- O(n) time, **O(1) space** — always mention this vs hash set O(n) space approach
- Hash set approach: store visited nodes, first revisit = cycle start (simpler, O(n) space)
- Always ask: just detect, or find the start node too?
- Edge cases: empty list, single node, cycle at head

```python
# ---- Detect cycle — Floyd's algorithm ----
def has_cycle(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow is fast:            # Identity check, not equality
            return True
    return False

# ---- Find cycle start node ----
def detect_cycle_start(head):
    slow = fast = head

    # Phase 1: Detect meeting point inside cycle
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow is fast:
            break
    else:
        return None                  # No cycle

    # Phase 2: Find entry point
    slow = head                      # Reset slow to head
    while slow is not fast:          # Both move 1 step
        slow = slow.next
        fast = fast.next
    return slow                      # Cycle start node

# ---- Find cycle length ----
def cycle_length(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow is fast:
            # Count steps until they meet again
            count = 1
            fast = fast.next
            while slow is not fast:
                fast = fast.next
                count += 1
            return count
    return 0

# ---- Hash set approach (O(n) space, simpler) ----
def has_cycle_hashset(head):
    visited = set()
    cur = head
    while cur:
        if id(cur) in visited:
            return True
        visited.add(id(cur))
        cur = cur.next
    return False

# ---- Build a cycle for testing ----
def build_cycle(arr, pos):
    head = build_list(arr)
    if pos < 0:
        return head
    cycle_entry = head
    tail = head
    for _ in range(pos):
        cycle_entry = cycle_entry.next
    while tail.next:
        tail = tail.next
    tail.next = cycle_entry         # Create cycle
    return head
```

---

## 2.4 List Reversal

### Key Concepts
- **Iterative**: three pointers — prev, curr, next — O(n) time O(1) space
- **Recursive**: reverse rest of list, fix pointers on the way back — O(n) time O(n) stack space
- **Reverse sublist [l, r]**: reach position l-1, reverse k nodes, reconnect
- Group reverse (k-group) is a popular hard variant

### Interview Details
- Iterative is always preferred (O(1) space)
- Draw pointer arrows before coding — prevents bugs
- For sublist reversal: **track the tail of reversed section** for reconnection
- Don't forget to set the original head's next to None (or successor node)

```python
# ---- Iterative reversal — O(n) time, O(1) space ----
def reverse_list(head):
    prev = None
    curr = head
    while curr:
        next_node  = curr.next    # Save next
        curr.next  = prev         # Reverse pointer
        prev       = curr         # Advance prev
        curr       = next_node    # Advance curr
    return prev                   # New head

# ---- Recursive reversal — O(n) time, O(n) stack ----
def reverse_list_recursive(head):
    if not head or not head.next:
        return head
    new_head = reverse_list_recursive(head.next)  # Reverse rest
    head.next.next = head         # Make next node point back
    head.next = None              # Disconnect current
    return new_head

# ---- Reverse sublist from position l to r (1-indexed) ----
def reverse_between(head, l, r):
    if not head or l == r:
        return head

    dummy = ListNode(0)
    dummy.next = head
    prev = dummy

    for _ in range(l - 1):       # Move prev to node before l
        prev = prev.next

    curr = prev.next              # curr starts at position l
    for _ in range(r - l):
        next_node    = curr.next
        curr.next    = next_node.next
        next_node.next = prev.next
        prev.next    = next_node

    return dummy.next

# Example: 1→2→3→4→5, l=2, r=4 → 1→4→3→2→5

# ---- Reverse in k-groups ----
def reverse_k_group(head, k):
    # Check if k nodes remain
    count, node = 0, head
    while node and count < k:
        node = node.next
        count += 1
    if count < k:
        return head               # Not enough nodes, leave as-is

    # Reverse k nodes
    prev, curr = None, head
    for _ in range(k):
        nxt       = curr.next
        curr.next = prev
        prev      = curr
        curr      = nxt

    # head is now tail of reversed group; recurse for rest
    head.next = reverse_k_group(curr, k)
    return prev                   # New head of this group
```

---

## 2.5 Fast / Slow Pointers

### Key Concepts
- Also called **Floyd's two-pointer** or **hare & tortoise**
- Fast moves **2 steps**, slow moves **1 step** per iteration
- When fast reaches end, slow is at **middle**
- Used for: middle node, cycle detection, kth from end, palindrome list
- Can generalize: fast moves k steps per iteration for kth-from-end

### Interview Details
- Middle of even-length list: slow ends at **left-middle** by default; to get right-middle use `while fast.next` condition swap
- Extremely versatile — when you see "linked list + O(1) space", think fast/slow
- Palindrome linked list = find middle + reverse second half + compare

```python
# ---- Find middle node ----
def find_middle(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    return slow                   # Left-middle for even length

# 1→2→3→4→5 → returns node 3
# 1→2→3→4   → returns node 2  (left-middle)

def find_middle_right(head):     # Right-middle for even length
    slow = fast = head
    while fast.next and fast.next.next:
        slow = slow.next
        fast = fast.next.next
    return slow

# ---- Kth node from end ----
def kth_from_end(head, k):
    fast = slow = head
    for _ in range(k):            # Advance fast by k steps
        if not fast:
            return None           # k > list length
        fast = fast.next
    while fast:                   # Move both until fast hits end
        fast = fast.next
        slow = slow.next
    return slow

# ---- Palindrome linked list — O(n) time, O(1) space ----
def is_palindrome_list(head):
    if not head or not head.next:
        return True

    # Step 1: Find middle
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next

    # Step 2: Reverse second half
    prev, curr = None, slow
    while curr:
        nxt = curr.next
        curr.next = prev
        prev = curr
        curr = nxt

    # Step 3: Compare both halves
    left, right = head, prev
    while right:                   # Right half is shorter or equal
        if left.val != right.val:
            return False
        left  = left.next
        right = right.next
    return True

# ---- Split list into two halves ----
def split_list(head):
    slow = fast = head
    prev = None
    while fast and fast.next:
        prev = slow
        slow = slow.next
        fast = fast.next.next
    if prev:
        prev.next = None          # Cut the list
    return head, slow             # (first_half, second_half)
```

---

## 2.6 Merge Lists

### Key Concepts
- **Merge two sorted lists**: compare heads, take smaller, advance that pointer — O(n+m)
- **Merge k sorted lists**: use a min-heap (priority queue) — O(N log k)
- **Divide and conquer** for k lists: merge pairs repeatedly — O(N log k)
- Always use **dummy head** to avoid special-casing the result head

### Interview Details
- Iterative merge is O(1) space vs recursive O(n) stack space
- For k lists: heap approach vs divide-and-conquer both O(N log k) — know both
- Merging is the backbone of **merge sort on linked lists**
- Edge cases: one or both lists empty, lists of different lengths

```python
import heapq

# ---- Merge two sorted lists — iterative ----
def merge_two_sorted(l1, l2):
    dummy = ListNode(0)
    cur = dummy
    while l1 and l2:
        if l1.val <= l2.val:
            cur.next = l1
            l1 = l1.next
        else:
            cur.next = l2
            l2 = l2.next
        cur = cur.next
    cur.next = l1 if l1 else l2    # Attach remaining nodes
    return dummy.next

# ---- Merge two sorted lists — recursive ----
def merge_two_recursive(l1, l2):
    if not l1: return l2
    if not l2: return l1
    if l1.val <= l2.val:
        l1.next = merge_two_recursive(l1.next, l2)
        return l1
    else:
        l2.next = merge_two_recursive(l1, l2.next)
        return l2

# ---- Merge k sorted lists — min-heap ----
def merge_k_sorted(lists):
    heap = []
    for i, node in enumerate(lists):
        if node:
            # (val, index, node) — index breaks ties when vals equal
            heapq.heappush(heap, (node.val, i, node))

    dummy = cur = ListNode(0)
    while heap:
        val, i, node = heapq.heappop(heap)
        cur.next = node
        cur = cur.next
        if node.next:
            heapq.heappush(heap, (node.next.val, i, node.next))

    return dummy.next

# ---- Merge k sorted lists — divide & conquer ----
def merge_k_divide(lists):
    if not lists:
        return None
    while len(lists) > 1:
        merged = []
        for i in range(0, len(lists), 2):
            l1 = lists[i]
            l2 = lists[i+1] if i+1 < len(lists) else None
            merged.append(merge_two_sorted(l1, l2))
        lists = merged
    return lists[0]

# ---- Merge sort on linked list — O(n log n), O(1) space ----
def merge_sort_list(head):
    if not head or not head.next:
        return head

    # Split into halves using slow/fast pointers
    slow, fast = head, head.next
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next

    mid       = slow.next
    slow.next = None              # Cut list in half

    left  = merge_sort_list(head)
    right = merge_sort_list(mid)
    return merge_two_sorted(left, right)

# ---- Intersection of two linked lists ----
# Find the node where two lists converge — O(n+m), O(1) space
def get_intersection(headA, headB):
    a, b = headA, headB
    while a is not b:
        a = a.next if a else headB   # Switch to other list at end
        b = b.next if b else headA
    return a                         # None if no intersection
```

### Linked Lists — Quick Complexity Cheatsheet

| Operation | Algorithm | Time | Space |
|---|---|---|---|
| Insert / Delete at node | Pointer update | O(1) | O(1) |
| Search / Access | Linear scan | O(n) | O(1) |
| Reverse list | Iterative (3 ptrs) | O(n) | O(1) |
| Reverse list | Recursive | O(n) | O(n) |
| Cycle detection | Floyd's | O(n) | O(1) |
| Find middle | Fast/slow | O(n) | O(1) |
| Merge 2 sorted | Iterative | O(n+m) | O(1) |
| Merge k sorted | Min-heap | O(N log k) | O(k) |
| Merge sort list | Divide & conquer | O(n log n) | O(log n) |

### Linked Lists — Common Interview Traps & Tips

- **Never lose the head** — store it in a variable before any traversal
- **Dummy node** pattern — use it for any insert/delete that may affect head
- `slow is fast` not `slow == fast` — compare **identity**, not value
- Reversing: save `curr.next` before overwriting it — most common bug
- Fast/slow middle: `while fast and fast.next` gives **left-middle**; swap condition for right-middle
- Merge k lists: heap needs a **tiebreaker** (index) because `ListNode` is not comparable in Python
- Cycle start proof: F = pre-cycle length; meeting point inside cycle is at distance F from start — memorize this for interviews
- For LRU cache: **sentinel dummy nodes** at both ends make `_remove` and `_insert` unconditional — no null checks needed

---

# 3. Stacks & Queues

---

## Core Concepts Upfront

```python
# Python built-in options — know these cold
# Stack  → list (append/pop from right)
# Queue  → collections.deque (append/popleft)
# PQueue → heapq module

from collections import deque
import heapq

stack = []
stack.append(1)       # push  → O(1)
stack.pop()           # pop   → O(1)
stack[-1]             # peek  → O(1)

queue = deque()
queue.append(1)       # enqueue → O(1)
queue.popleft()       # dequeue → O(1)
queue[0]              # peek    → O(1)
```

---

## 3.1 Stack Implementation

### Key Concepts
- **LIFO** — Last In, First Out
- Core ops: `push`, `pop`, `peek`, `is_empty` — all **O(1)**
- Python `list` is a dynamic array — works perfectly as a stack
- Linked list implementation gives true O(1) with no resizing overhead
- **Monotonic stack** — stack that maintains increasing or decreasing order — critical interview pattern

### Interview Details
- Know array-based AND linked-list-based implementations
- **Min stack** (get minimum in O(1)) — very common interview question
- **Two-stack queue** — implement queue using two stacks
- Monotonic stack pattern: "next greater/smaller element" family of problems
- Always clarify: what happens on pop/peek of empty stack?

```python
# ---- Array-based stack ----
class ArrayStack:
    def __init__(self):
        self._data = []

    def push(self, val):
        self._data.append(val)          # O(1) amortized

    def pop(self):
        if self.is_empty():
            raise IndexError("Pop from empty stack")
        return self._data.pop()         # O(1)

    def peek(self):
        if self.is_empty():
            raise IndexError("Peek at empty stack")
        return self._data[-1]           # O(1)

    def is_empty(self):
        return len(self._data) == 0

    def size(self):
        return len(self._data)

# ---- Linked-list-based stack ----
class StackNode:
    def __init__(self, val, next=None):
        self.val  = val
        self.next = next

class LinkedStack:
    def __init__(self):
        self._top  = None
        self._size = 0

    def push(self, val):
        self._top  = StackNode(val, self._top)  # New node points to old top
        self._size += 1

    def pop(self):
        if not self._top:
            raise IndexError("Pop from empty stack")
        val       = self._top.val
        self._top = self._top.next              # Advance top
        self._size -= 1
        return val

    def peek(self):
        if not self._top:
            raise IndexError("Peek at empty stack")
        return self._top.val

    def is_empty(self):
        return self._top is None

# ---- Min Stack — O(1) get_min ----
# Key insight: store (val, current_min) pairs
class MinStack:
    def __init__(self):
        self._stack = []             # Stores (value, min_so_far) tuples

    def push(self, val):
        current_min = min(val, self._stack[-1][1]) if self._stack else val
        self._stack.append((val, current_min))

    def pop(self):
        if not self._stack:
            raise IndexError("Pop from empty stack")
        return self._stack.pop()[0]

    def peek(self):
        return self._stack[-1][0]

    def get_min(self):
        return self._stack[-1][1]    # Always O(1)

s = MinStack()
s.push(5); s.push(3); s.push(7); s.push(2)
print(s.get_min())   # 2
s.pop()
print(s.get_min())   # 3

# ---- Queue using two stacks ----
# Amortized O(1) enqueue and dequeue
class QueueViaStacks:
    def __init__(self):
        self._inbox  = []            # For enqueue
        self._outbox = []            # For dequeue

    def enqueue(self, val):
        self._inbox.append(val)      # Always O(1)

    def dequeue(self):
        if not self._outbox:         # Lazy transfer
            while self._inbox:
                self._outbox.append(self._inbox.pop())
        if not self._outbox:
            raise IndexError("Dequeue from empty queue")
        return self._outbox.pop()    # Amortized O(1)

    def peek(self):
        if not self._outbox:
            while self._inbox:
                self._outbox.append(self._inbox.pop())
        return self._outbox[-1]
```

---

## 3.2 Queue Implementation

### Key Concepts
- **FIFO** — First In, First Out
- Core ops: `enqueue`, `dequeue`, `peek`, `is_empty` — all **O(1)**
- Python `collections.deque` is a **doubly-ended queue** backed by doubly linked list — true O(1) both ends
- `list` as queue is **BAD** — `list.pop(0)` is O(n) due to shifting
- **Monotonic deque** — used in sliding window maximum problems

### Interview Details
- Always use `deque` not `list` for queue in Python
- Know **circular queue** (fixed capacity)
- **Stack using two queues** — mirror of two-stack queue
- Deque supports O(1) operations at both ends — useful for sliding window problems

```python
from collections import deque

# ---- Deque-based queue ----
class Queue:
    def __init__(self):
        self._data = deque()

    def enqueue(self, val):
        self._data.append(val)          # Add to right → O(1)

    def dequeue(self):
        if self.is_empty():
            raise IndexError("Dequeue from empty queue")
        return self._data.popleft()     # Remove from left → O(1)

    def peek(self):
        if self.is_empty():
            raise IndexError("Peek at empty queue")
        return self._data[0]

    def is_empty(self):
        return len(self._data) == 0

    def size(self):
        return len(self._data)

# ---- Stack using two queues ----
class StackViaQueues:
    def __init__(self):
        self._q1 = deque()
        self._q2 = deque()

    def push(self, val):
        self._q2.append(val)
        while self._q1:              # Drain q1 into q2
            self._q2.append(self._q1.popleft())
        self._q1, self._q2 = self._q2, self._q1   # Swap

    def pop(self):
        if not self._q1:
            raise IndexError("Pop from empty stack")
        return self._q1.popleft()

    def peek(self):
        return self._q1[0]

# ---- Sliding window maximum (monotonic deque) ----
def sliding_window_max(nums, k):
    dq     = deque()                 # Stores indices, decreasing values
    result = []

    for i, num in enumerate(nums):
        # Remove elements outside window
        while dq and dq[0] < i - k + 1:
            dq.popleft()
        # Maintain decreasing order — remove smaller elements from back
        while dq and nums[dq[-1]] < num:
            dq.pop()
        dq.append(i)
        if i >= k - 1:               # Window is full
            result.append(nums[dq[0]])
    return result

print(sliding_window_max([1,3,-1,-3,5,3,6,7], 3))
# [3, 3, 5, 5, 6, 7]
```

---

## 3.3 Parentheses Matching

### Key Concepts
- Classic stack application — push open brackets, pop and verify on close bracket
- Three variants: `()`, `[]`, `{}` — use a **mapping dict** for clean code
- Extended variants: minimum removals, minimum additions to make valid, score of parentheses
- Key rule: stack must be **empty** at end for full validity

### Interview Details
- Mapping close→open is cleaner than multiple `if` conditions
- Always handle: empty string (valid), only open brackets (invalid), only close brackets (invalid)
- **Minimum add/remove** variants use a counter approach — no stack needed
- Score of parentheses uses stack with integer values

```python
# ---- Basic parentheses validation ----
def is_valid_parens(s):
    stack   = []
    mapping = {')': '(', ']': '[', '}': '{'}
    for ch in s:
        if ch in mapping:                        # Closing bracket
            top = stack.pop() if stack else '#'
            if mapping[ch] != top:
                return False
        else:
            stack.append(ch)                     # Opening bracket
    return len(stack) == 0                       # Must be fully matched

print(is_valid_parens("()[]{}"))   # True
print(is_valid_parens("([)]"))     # False
print(is_valid_parens("{[]}"))     # True

# ---- Minimum removals to make valid ----
def min_remove_to_valid(s):
    open_count = close_count = 0
    for ch in s:
        if ch == '(':
            open_count += 1
        elif ch == ')':
            if open_count > 0:
                open_count -= 1      # Match with an open
            else:
                close_count += 1     # Unmatched close
    return open_count + close_count  # Total removals needed

# ---- Return valid string after minimum removals ----
def min_remove_valid_string(s):
    stack    = []                    # Stores indices of unmatched '('
    to_remove = set()

    for i, ch in enumerate(s):
        if ch == '(':
            stack.append(i)
        elif ch == ')':
            if stack:
                stack.pop()          # Matched
            else:
                to_remove.add(i)     # Unmatched ')'
    to_remove |= set(stack)          # Remaining unmatched '('

    return ''.join(ch for i, ch in enumerate(s) if i not in to_remove)

# ---- Longest valid parentheses substring ----
def longest_valid_parens(s):
    stack = [-1]                     # Sentinel for base
    max_len = 0
    for i, ch in enumerate(s):
        if ch == '(':
            stack.append(i)
        else:
            stack.pop()
            if not stack:
                stack.append(i)      # New base
            else:
                max_len = max(max_len, i - stack[-1])
    return max_len

# ---- Score of parentheses ----
# "()" = 1, "(A)" = 2*A, "AB" = A+B
def score_of_parens(s):
    stack = [0]                      # Current score at depth
    for ch in s:
        if ch == '(':
            stack.append(0)          # New depth level
        else:
            v = stack.pop()
            stack[-1] += max(2*v, 1) # "()" scores 1, else doubles
    return stack[0]

print(score_of_parens("(()(()))"))   # 6
```

---

## 3.4 Expression Evaluation

### Key Concepts
- **Infix** (normal): `3 + 4 * 2` — needs precedence/parentheses handling
- **Postfix / RPN** (Reverse Polish Notation): `3 4 2 * +` — no precedence needed, stack-based
- **Prefix**: `+ 3 * 4 2` — evaluate right to left
- **Shunting-yard algorithm** (Dijkstra): converts infix → postfix using operator stack
- Two-stack approach evaluates infix directly: **operand stack** + **operator stack**

### Interview Details
- Postfix evaluation is the cleanest — O(n) with one stack
- Shunting-yard needs precedence dict + handling of left/right associativity
- Basic calculator (LeetCode #224) uses stack to handle `+`, `-`, `(`, `)`
- Know how to handle **unary minus** and **multi-digit numbers**

```python
# ---- Postfix (RPN) evaluation ----
def eval_rpn(tokens):
    stack = []
    ops = {
        '+': lambda a, b: a + b,
        '-': lambda a, b: a - b,
        '*': lambda a, b: a * b,
        '/': lambda a, b: int(a / b)   # Truncate toward zero
    }
    for token in tokens:
        if token in ops:
            b = stack.pop()            # Right operand (popped first)
            a = stack.pop()            # Left operand
            stack.append(ops[token](a, b))
        else:
            stack.append(int(token))
    return stack[0]

print(eval_rpn(["2","1","+","3","*"]))   # 9  → (2+1)*3
print(eval_rpn(["4","13","5","/","+"]))  # 6  → 4+(13/5)

# ---- Infix to Postfix (Shunting-Yard) ----
def infix_to_postfix(expression):
    precedence = {'+': 1, '-': 1, '*': 2, '/': 2, '^': 3}
    output  = []
    op_stack = []

    tokens = expression.split()
    for token in tokens:
        if token.lstrip('-').isdigit():         # Operand
            output.append(token)
        elif token == '(':
            op_stack.append(token)
        elif token == ')':
            while op_stack and op_stack[-1] != '(':
                output.append(op_stack.pop())
            op_stack.pop()                       # Remove '('
        else:                                    # Operator
            while (op_stack and
                   op_stack[-1] != '(' and
                   precedence.get(op_stack[-1], 0) >= precedence.get(token, 0)):
                output.append(op_stack.pop())
            op_stack.append(token)

    while op_stack:
        output.append(op_stack.pop())
    return ' '.join(output)

print(infix_to_postfix("3 + 4 * 2"))         # "3 4 2 * +"
print(infix_to_postfix("( 3 + 4 ) * 2"))     # "3 4 + 2 *"

# ---- Basic Calculator — handles +, -, ( ) ----
def basic_calculator(s):
    stack   = []
    result  = 0
    sign    = 1                      # +1 or -1
    num     = 0

    for ch in s:
        if ch.isdigit():
            num = num * 10 + int(ch) # Build multi-digit number
        elif ch in '+-':
            result += sign * num
            num    = 0
            sign   = 1 if ch == '+' else -1
        elif ch == '(':
            stack.append(result)     # Save current result
            stack.append(sign)       # Save current sign
            result = 0               # Reset for sub-expression
            sign   = 1
        elif ch == ')':
            result  += sign * num
            num      = 0
            result  *= stack.pop()   # Apply sign before '('
            result  += stack.pop()   # Add result before '('

    return result + sign * num

# ---- Basic Calculator II — handles +, -, *, / (no parentheses) ----
def calculate_ii(s):
    stack   = []
    num     = 0
    op      = '+'                    # Default first operation is +

    for i, ch in enumerate(s):
        if ch.isdigit():
            num = num * 10 + int(ch)
        if (ch in '+-*/' or i == len(s) - 1) and ch != ' ':
            if op == '+':   stack.append(num)
            elif op == '-': stack.append(-num)
            elif op == '*': stack.append(stack.pop() * num)
            elif op == '/': stack.append(int(stack.pop() / num))
            op  = ch
            num = 0

    return sum(stack)

print(calculate_ii("3+2*2"))         # 7
```

---

## 3.5 Next Greater Element

### Key Concepts
- **Monotonic stack** pattern — maintain a stack of elements in decreasing order
- For each element: pop all smaller elements from stack; they found their "next greater"
- Process left→right; stack holds elements **waiting** for their next greater
- For circular arrays: process array **twice** (index `i % n`)
- Variants: next greater, next smaller, previous greater, previous smaller

### Interview Details
- Monotonic stack is O(n) — each element pushed and popped at most once
- Result array initialized to `-1` (no greater element found = -1 by convention)
- For previous greater element: traverse **right to left**
- **Daily temperatures** (LeetCode #739) and **next greater element** (LeetCode #496, #503) are must-know

```python
# ---- Next Greater Element (basic) ----
def next_greater(nums):
    n      = len(nums)
    result = [-1] * n
    stack  = []                      # Stores indices of elements awaiting NGE

    for i in range(n):
        # Pop all elements smaller than current — current is their NGE
        while stack and nums[stack[-1]] < nums[i]:
            idx          = stack.pop()
            result[idx]  = nums[i]
        stack.append(i)
    return result

print(next_greater([2, 1, 2, 4, 3]))   # [4, 2, 4, -1, -1]

# ---- Next Greater Element — circular array ----
# LeetCode #503
def next_greater_circular(nums):
    n      = len(nums)
    result = [-1] * n
    stack  = []

    for i in range(2 * n):           # Traverse twice
        while stack and nums[stack[-1]] < nums[i % n]:
            result[stack.pop()] = nums[i % n]
        if i < n:
            stack.append(i)          # Only push in first pass

    return result

print(next_greater_circular([1,2,1]))   # [2,-1,2]

# ---- Daily temperatures (next warmer day) ----
# LeetCode #739
def daily_temperatures(temps):
    n      = len(temps)
    result = [0] * n
    stack  = []                      # Indices of unresolved days

    for i, temp in enumerate(temps):
        while stack and temps[stack[-1]] < temp:
            j          = stack.pop()
            result[j]  = i - j       # Days to wait
        stack.append(i)

    return result

print(daily_temperatures([73,74,75,71,69,72,76,73]))
# [1, 1, 4, 2, 1, 1, 0, 0]

# ---- Largest rectangle in histogram (classic monotonic stack) ----
# LeetCode #84
def largest_rectangle(heights):
    stack   = []                     # Stores indices
    max_area = 0
    heights  = heights + [0]         # Sentinel to flush stack

    for i, h in enumerate(heights):
        start = i
        while stack and heights[stack[-1]] > h:
            idx      = stack.pop()
            width    = i - (stack[-1] + 1 if stack else 0)
            max_area = max(max_area, heights[idx] * width)
            start    = idx
        stack.append(i)

    return max_area

print(largest_rectangle([2,1,5,6,2,3]))   # 10
```

---

## 3.6 Circular Queue

### Key Concepts
- Fixed-size queue using an array with **head and tail pointers**
- `tail = (tail + 1) % capacity` — wrap around using modulo
- **Full** condition: `(tail + 1) % capacity == head`
- **Empty** condition: `head == tail` (or use a size counter)
- Also called **Ring Buffer** — used in OS, networking buffers, producer-consumer

### Interview Details
- Modulo arithmetic is the key — practice it without mistakes
- Two approaches to distinguish full vs empty: **leave one slot empty** OR **use a size counter** (cleaner, preferred)
- `deque(maxlen=k)` in Python automatically acts as circular — mention in interviews
- Know how circular queue underlies **BFS** in graph problems

```python
# ---- Circular Queue with size counter ----
class CircularQueue:
    def __init__(self, capacity):
        self.cap    = capacity
        self.queue  = [None] * capacity
        self.head   = 0                  # Dequeue from here
        self.tail   = 0                  # Enqueue here
        self.size   = 0

    def enqueue(self, val):
        if self.is_full():
            raise OverflowError("Queue is full")
        self.queue[self.tail] = val
        self.tail = (self.tail + 1) % self.cap   # Wrap around
        self.size += 1

    def dequeue(self):
        if self.is_empty():
            raise IndexError("Queue is empty")
        val       = self.queue[self.head]
        self.head = (self.head + 1) % self.cap   # Wrap around
        self.size -= 1
        return val

    def peek_front(self):
        if self.is_empty():
            raise IndexError("Queue is empty")
        return self.queue[self.head]

    def peek_rear(self):
        if self.is_empty():
            raise IndexError("Queue is empty")
        return self.queue[(self.tail - 1) % self.cap]

    def is_empty(self):
        return self.size == 0

    def is_full(self):
        return self.size == self.cap

# ---- Python deque as fixed circular buffer ----
from collections import deque

circular = deque(maxlen=3)
circular.append(1)
circular.append(2)
circular.append(3)
circular.append(4)         # Automatically drops leftmost (1)
print(circular)            # deque([2, 3, 4], maxlen=3)
```

### Stacks & Queues — Quick Complexity Cheatsheet

| Operation | Stack (list) | Queue (deque) | Circular Queue |
|---|---|---|---|
| Push / Enqueue | O(1) amortized | O(1) | O(1) |
| Pop / Dequeue | O(1) | O(1) | O(1) |
| Peek | O(1) | O(1) | O(1) |
| Search | O(n) | O(n) | O(n) |
| Min/Max stack | O(1) | — | — |
| NGE (mono stack) | O(n) total | — | — |
| Sliding window max | — | O(n) total | — |

### Stacks & Queues — Common Interview Traps & Tips

- **Never use `list` as a queue** — `pop(0)` is O(n); always use `deque`
- Min stack: store **(value, min_at_this_point)** tuples — cleaner than parallel stack
- Circular queue **full vs empty**: size counter is foolproof; one-slot-empty approach is error-prone
- Monotonic stack: decide **strictly greater** vs **greater or equal** based on problem — read carefully
- RPN evaluation: pop **b first, then a** — order matters for `-` and `/`
- Basic calculator: when hitting `)`, apply the **saved sign** from before `(`, not current sign
- Shunting-yard: `^` (power) is **right-associative** — use `>` not `>=` for precedence comparison

---

# 4. Hashing & Heaps

---

## Core Concepts Upfront

```python
from collections import defaultdict, Counter, OrderedDict
import heapq

# HashMap  → dict         O(1) avg get/set/delete
# HashSet  → set          O(1) avg add/remove/lookup
# Min Heap → heapq        O(log n) push/pop, O(1) peek
# Max Heap → heapq        negate values (heapq is min-only)
# Counter  → freq map     most_common(), arithmetic ops
```

---

## 4.1 HashMap / HashSet

### Key Concepts
- **HashMap**: key → value mapping, keys must be **hashable** (immutable)
- **HashSet**: unordered collection of unique hashable elements
- Hash function maps key → bucket index; **collision** handled by chaining or open addressing
- Python `dict` uses open addressing with pseudo-random probing
- **Load factor** = n/capacity; Python resizes at ~⅔ capacity → O(1) amortized
- Worst case O(n) for all ops (all keys collide) — rare, mention in interviews

### Interview Details
- `dict` preserves insertion order since Python 3.7+
- Unhashable types: `list`, `dict`, `set` — use `tuple` instead as key
- `defaultdict` avoids KeyError with default factory
- `Counter` is a specialized dict for frequency counting
- Two-sum, anagram detection, frequency problems → always think HashMap first

### Time/Space Complexity

| Operation | Average | Worst |
|---|---|---|
| Insert / Update | O(1) | O(n) |
| Delete | O(1) | O(n) |
| Lookup | O(1) | O(n) |
| Iteration | O(n) | O(n) |

```python
# ---- HashMap basics ----
freq = {}
freq['a']  = freq.get('a', 0) + 1      # Safe increment
freq['b']  = freq.setdefault('b', 0)    # Set if missing, return value

# defaultdict — cleaner default handling
graph   = defaultdict(list)             # Adjacency list
freq    = defaultdict(int)              # Frequency map
buckets = defaultdict(set)              # Bucket grouping

# Counter — frequency map with extras
words  = ["apple", "banana", "apple", "cherry", "banana", "apple"]
count  = Counter(words)
print(count.most_common(2))             # [('apple', 3), ('banana', 2)]
print(count['apple'])                   # 3
c1 = Counter("aab"); c2 = Counter("abb")
print(c1 & c2)                          # Intersection: Counter({'a': 1, 'b': 1})
print(c1 | c2)                          # Union:        Counter({'a': 2, 'b': 2})
print(c1 - c2)                          # Subtraction:  Counter({'a': 1})

# ---- HashSet basics ----
seen  = set()
seen.add(3)
seen.discard(3)                         # No error if missing (vs remove)

# Set operations
a = {1, 2, 3, 4}
b = {3, 4, 5, 6}
print(a & b)                            # Intersection: {3, 4}
print(a | b)                            # Union:        {1,2,3,4,5,6}
print(a - b)                            # Difference:   {1, 2}
print(a ^ b)                            # Symmetric diff: {1,2,5,6}

# Frozenset — hashable set (usable as dict key)
key = frozenset([1, 2, 3])
d   = {key: "value"}

# ---- Custom hash map implementation ----
class HashMap:
    def __init__(self, capacity=1024):
        self.capacity = capacity
        self.buckets  = [[] for _ in range(capacity)]  # Chaining

    def _hash(self, key):
        return hash(key) % self.capacity

    def put(self, key, val):
        bucket = self.buckets[self._hash(key)]
        for i, (k, v) in enumerate(bucket):
            if k == key:
                bucket[i] = (key, val)          # Update existing
                return
        bucket.append((key, val))               # Insert new

    def get(self, key):
        for k, v in self.buckets[self._hash(key)]:
            if k == key:
                return v
        return -1

    def remove(self, key):
        bucket = self.buckets[self._hash(key)]
        self.buckets[self._hash(key)] = [
            (k, v) for k, v in bucket if k != key
        ]
```

---

## 4.2 Coding Hash-Based Problems

### Key Patterns
- **Two Sum** → value-to-index map, one pass
- **Anagram grouping** → sorted string or char-count tuple as key
- **Subarray sum = k** → prefix sum + frequency map
- **Longest consecutive sequence** → set for O(1) lookup, only start sequences
- **Sliding window + hash** → character/element frequency window

```python
# ---- Two Sum — O(n) ----
def two_sum(nums, target):
    seen = {}                               # val → index
    for i, num in enumerate(nums):
        complement = target - num
        if complement in seen:
            return [seen[complement], i]
        seen[num] = i
    return []

print(two_sum([2, 7, 11, 15], 9))           # [0, 1]

# ---- Group anagrams ----
def group_anagrams(strs):
    groups = defaultdict(list)
    for s in strs:
        key = tuple(sorted(s))              # Sorted chars as key
        groups[key].append(s)
    return list(groups.values())

# Alternative key: character count tuple O(n) — no sorting
def group_anagrams_v2(strs):
    groups = defaultdict(list)
    for s in strs:
        count = [0] * 26
        for ch in s:
            count[ord(ch) - ord('a')] += 1
        groups[tuple(count)].append(s)
    return list(groups.values())

# ---- Subarray sum equals k — O(n) ----
def subarray_sum(nums, k):
    prefix_freq = defaultdict(int)
    prefix_freq[0] = 1                      # Empty prefix
    prefix = count = 0
    for num in nums:
        prefix += num
        count  += prefix_freq[prefix - k]   # How many subarrays end here
        prefix_freq[prefix] += 1
    return count

print(subarray_sum([1, 1, 1], 2))           # 2
print(subarray_sum([1, 2, 3], 3))           # 2

# ---- Longest consecutive sequence — O(n) ----
def longest_consecutive(nums):
    num_set   = set(nums)
    best      = 0
    for num in num_set:
        if num - 1 not in num_set:          # Only start a new sequence
            curr, length = num, 1
            while curr + 1 in num_set:
                curr   += 1
                length += 1
            best = max(best, length)
    return best

print(longest_consecutive([100,4,200,1,3,2]))  # 4 → [1,2,3,4]

# ---- Top K frequent elements — O(n log k) ----
def top_k_frequent(nums, k):
    count  = Counter(nums)
    return heapq.nlargest(k, count.keys(), key=count.get)

# Bucket sort approach — O(n)
def top_k_frequent_bucket(nums, k):
    count   = Counter(nums)
    buckets = [[] for _ in range(len(nums) + 1)]
    for num, freq in count.items():
        buckets[freq].append(num)
    result = []
    for i in range(len(buckets) - 1, 0, -1):
        result.extend(buckets[i])
        if len(result) >= k:
            return result[:k]
    return result

print(top_k_frequent([1,1,1,2,2,3], 2))    # [1, 2]

# ---- Longest substring without repeating characters — O(n) ----
def length_of_longest_substring(s):
    char_index = {}
    left = best = 0
    for right, ch in enumerate(s):
        if ch in char_index and char_index[ch] >= left:
            left = char_index[ch] + 1       # Shrink window
        char_index[ch] = right
        best = max(best, right - left + 1)
    return best

print(length_of_longest_substring("abcabcbb"))  # 3
```

---

## 4.3 Min Heap / Max Heap

### Key Concepts
- **Heap**: complete binary tree satisfying heap property
- **Min heap**: parent ≤ children — root is always minimum
- **Max heap**: parent ≥ children — root is always maximum
- Stored as array: parent of i = `(i-1)//2`, children = `2i+1`, `2i+2`
- Python `heapq` is **min heap only** — negate values for max heap

### Time/Space Complexity

| Operation | Time | Notes |
|---|---|---|
| heappush | O(log n) | Sift up |
| heappop | O(log n) | Sift down |
| heapify | O(n) | Bottom-up build |
| peek (heap[0]) | O(1) | Min element |
| nlargest / nsmallest | O(n + k log n) | Uses heap internally |

```python
import heapq

# ---- Min heap operations ----
min_heap = []
heapq.heappush(min_heap, 5)
heapq.heappush(min_heap, 2)
heapq.heappush(min_heap, 8)
heapq.heappush(min_heap, 1)

print(min_heap[0])                      # Peek min → 1  (O(1))
print(heapq.heappop(min_heap))          # Pop min  → 1  (O(log n))

# ---- Max heap — negate values ----
max_heap = []
for val in [5, 2, 8, 1]:
    heapq.heappush(max_heap, -val)      # Store negated

print(-max_heap[0])                     # Peek max → 8
print(-heapq.heappop(max_heap))         # Pop max  → 8

# ---- Min heap from scratch ----
class MinHeap:
    def __init__(self):
        self.heap = []

    def push(self, val):
        self.heap.append(val)
        self._sift_up(len(self.heap) - 1)

    def pop(self):
        if len(self.heap) == 1:
            return self.heap.pop()
        root = self.heap[0]
        self.heap[0] = self.heap.pop()  # Move last to root
        self._sift_down(0)
        return root

    def peek(self):
        return self.heap[0] if self.heap else None

    def _sift_up(self, i):
        parent = (i - 1) // 2
        if i > 0 and self.heap[i] < self.heap[parent]:
            self.heap[i], self.heap[parent] = self.heap[parent], self.heap[i]
            self._sift_up(parent)

    def _sift_down(self, i):
        n        = len(self.heap)
        smallest = i
        left     = 2 * i + 1
        right    = 2 * i + 2

        if left  < n and self.heap[left]  < self.heap[smallest]:
            smallest = left
        if right < n and self.heap[right] < self.heap[smallest]:
            smallest = right
        if smallest != i:
            self.heap[i], self.heap[smallest] = self.heap[smallest], self.heap[i]
            self._sift_down(smallest)
```

---

## 4.4 Priority Queue

### Key Concepts
- Abstract data type: always access/remove the **highest priority** element
- Implemented via heap — O(log n) push/pop, O(1) peek
- Python: `heapq` module (min priority) or `queue.PriorityQueue` (thread-safe)
- Custom priority: use tuples `(priority, item)` or define `__lt__`
- **K-way merge**, **Dijkstra's**, **task scheduling** all use priority queues

### Interview Details
- `heapq` is not thread-safe — use `queue.PriorityQueue` for concurrency
- For objects without natural ordering: add index as tiebreaker, or define `__lt__`
- `heapq.nlargest(k, iterable)` and `heapq.nsmallest(k, iterable)` — O(n log k)
- Know when heap beats sorting: streaming data, online algorithms, top-k problems

```python
# ---- Kth largest element — O(n log k) ----
def kth_largest(nums, k):
    heap = []
    for num in nums:
        heapq.heappush(heap, num)
        if len(heap) > k:
            heapq.heappop(heap)         # Evict smallest
    return heap[0]                      # Root = kth largest

print(kth_largest([3,2,1,5,6,4], 2))   # 5

# ---- K closest points to origin — O(n log k) ----
def k_closest(points, k):
    heap = []
    for x, y in points:
        dist = x*x + y*y
        heapq.heappush(heap, (-dist, x, y))  # Max heap by negating
        if len(heap) > k:
            heapq.heappop(heap)
    return [[x, y] for (_, x, y) in heap]

# ---- Merge k sorted arrays — O(N log k) ----
def merge_k_sorted(arrays):
    heap   = []
    result = []
    for i, arr in enumerate(arrays):
        if arr:
            heapq.heappush(heap, (arr[0], i, 0))  # (val, arr_idx, elem_idx)

    while heap:
        val, i, j = heapq.heappop(heap)
        result.append(val)
        if j + 1 < len(arrays[i]):
            heapq.heappush(heap, (arrays[i][j+1], i, j+1))

    return result

print(merge_k_sorted([[1,4,7],[2,5,8],[3,6,9]]))
# [1,2,3,4,5,6,7,8,9]

# ---- Find median from data stream ----
# Two heaps: max heap (lower half) + min heap (upper half)
class MedianFinder:
    def __init__(self):
        self.lo = []    # Max heap (negate) — lower half
        self.hi = []    # Min heap — upper half

    def add_num(self, num):
        heapq.heappush(self.lo, -num)           # Push to lower
        heapq.heappush(self.hi, -heapq.heappop(self.lo))
        if len(self.hi) > len(self.lo):         # Keep lo >= hi in size
            heapq.heappush(self.lo, -heapq.heappop(self.hi))

    def find_median(self):
        if len(self.lo) > len(self.hi):
            return -self.lo[0]
        return (-self.lo[0] + self.hi[0]) / 2.0

mf = MedianFinder()
for n in [1, 2, 3, 4, 5]:
    mf.add_num(n)
    print(mf.find_median())             # 1, 1.5, 2, 2.5, 3
```

---

## 4.5 Heapify Operations

### Key Concepts
- `heapq.heapify(arr)` — converts list to heap **in-place** in **O(n)**
- Why O(n) not O(n log n)? Bottom-up build: leaf nodes are free, work decreases per level
- `heapreplace(heap, val)` — pop min then push new val in single O(log n) op
- `heappushpop(heap, val)` — push first then pop min — more efficient than two separate calls
- Manual heapify: start from last non-leaf `(n//2 - 1)` and sift down each node

```python
import heapq

# ---- heapify — O(n) ----
arr = [5, 3, 8, 1, 9, 2, 7]
heapq.heapify(arr)
print(arr)                              # Valid min heap
print(arr[0])                           # 1 — minimum guaranteed at root

# ---- heapreplace — pop then push, O(log n) ----
result = heapq.heapreplace(arr, 4)
print(result)                           # Returns old min

# ---- heappushpop — push then pop, O(log n) ----
result = heapq.heappushpop(arr, 0)
print(result)                           # Returns 0 (pushed value is still min)

# ---- Manual heapify from scratch ----
def heapify_manual(arr):
    n = len(arr)
    for i in range(n // 2 - 1, -1, -1):
        sift_down(arr, i, n)

def sift_down(arr, i, n):
    smallest = i
    left     = 2 * i + 1
    right    = 2 * i + 2
    if left  < n and arr[left]  < arr[smallest]: smallest = left
    if right < n and arr[right] < arr[smallest]: smallest = right
    if smallest != i:
        arr[i], arr[smallest] = arr[smallest], arr[i]
        sift_down(arr, smallest, n)

def sift_up(arr, i):
    while i > 0:
        parent = (i - 1) // 2
        if arr[i] < arr[parent]:
            arr[i], arr[parent] = arr[parent], arr[i]
            i = parent
        else:
            break

# ---- Why heapify is O(n) — key interview explanation ----
# Height of heap = log n
# Nodes at height h = ceil(n / 2^(h+1))
# Work per node at height h = O(h)
# Total = sum over h: n/2^(h+1) * h = O(n) * sum(h/2^h) = O(n) * O(2) = O(n)

# ---- When to use heapify vs repeated heappush ----
# heapify(arr)     → O(n)       build from existing list
# n x heappush     → O(n log n) build from scratch
# Rule: if you have all elements upfront → heapify
#       if elements arrive one by one    → heappush
```

---

## 4.6 Heap Sort

### Key Concepts
- **Phase 1**: Build max heap from array — O(n)
- **Phase 2**: Repeatedly extract max, place at end — O(n log n)
- **In-place**, **O(n log n)** worst case — unlike quicksort O(n²) worst
- **Not stable** (equal elements may reorder) — unlike merge sort
- Not cache-friendly (non-sequential memory access) — slower than quicksort in practice

```python
# ---- Heap Sort — in-place, O(n log n) ----
def heap_sort(arr):
    n = len(arr)

    # Phase 1: Build max heap (bottom-up)
    for i in range(n // 2 - 1, -1, -1):
        _sift_down_max(arr, i, n)

    # Phase 2: Extract elements one by one
    for end in range(n - 1, 0, -1):
        arr[0], arr[end] = arr[end], arr[0]   # Move max to end
        _sift_down_max(arr, 0, end)            # Restore heap on reduced array

    return arr

def _sift_down_max(arr, i, n):
    largest = i
    left    = 2 * i + 1
    right   = 2 * i + 2
    if left  < n and arr[left]  > arr[largest]: largest = left
    if right < n and arr[right] > arr[largest]: largest = right
    if largest != i:
        arr[i], arr[largest] = arr[largest], arr[i]
        _sift_down_max(arr, largest, n)

arr = [12, 11, 13, 5, 6, 7]
print(heap_sort(arr))                   # [5, 6, 7, 11, 12, 13]

# ---- Heap sort using Python heapq (not in-place) ----
def heap_sort_heapq(arr):
    heapq.heapify(arr)
    return [heapq.heappop(arr) for _ in range(len(arr))]

print(heap_sort_heapq([3, 1, 4, 1, 5, 9]))  # [1, 1, 3, 4, 5, 9]

# ---- Partial sort — get k sorted elements efficiently ----
def partial_sort(arr, k):
    # Only sort smallest k — O(n + k log n)
    return heapq.nsmallest(k, arr)

def partial_sort_largest(arr, k):
    # Only sort largest k — O(n + k log n)
    return heapq.nlargest(k, arr)
```

### Hashing & Heaps — Quick Complexity Cheatsheet

| Operation | HashMap/Set | Min/Max Heap | Priority Queue |
|---|---|---|---|
| Insert | O(1) avg | O(log n) | O(log n) |
| Delete | O(1) avg | O(log n) | O(log n) |
| Lookup / Peek min | O(1) avg | O(1) | O(1) |
| Build from list | O(n) | O(n) heapify | O(n log n) repeated push |
| Sort n elements | O(n log n) | O(n log n) | O(n log n) |
| k largest/smallest | O(n log k) | O(n log k) | O(n log k) |
| Heapify | — | O(n) | — |

### Hashing & Heaps — Common Interview Traps & Tips

- **Max heap**: always negate values — `heappush(h, -val)`, retrieve as `-heappop(h)`
- **Tuple tiebreaker**: if two tuples share priority, Python compares the second element — add a unique counter to avoid comparing non-comparable objects: `(priority, counter, item)`
- `heapify` is **O(n)**, not O(n log n) — be ready to explain why (bottom-up build proof)
- **Two-heap median**: lo is max heap (negated), hi is min heap; size invariant: `len(lo) >= len(hi)` and `len(lo) - len(hi) <= 1`
- **Subarray sum = k**: prefix sum + hashmap — NOT sliding window (negatives break sliding window)
- `Counter` subtraction ignores zero/negative counts — `Counter({'a':1}) - Counter({'a':2})` gives empty Counter
- `dict` in Python 3.7+ preserves insertion order but **set does not**
- For top-k frequent: if k is close to n, sorting beats heap; heap wins when k << n
- `heapreplace` is faster than `heappop` + `heappush` — use in hot loops
- Longest consecutive sequence: **only start counting from sequence beginnings** (`num-1 not in set`) — avoids O(n²) inner loop

---

*Generated for interview preparation — Python implementations throughout*



# Data Structures Interview Notes — Trees (Python)

---

# Part 1: Binary Trees

## Core Node Structure

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
```

---

## 1. Tree Traversals

### Preorder (Root → Left → Right)

**Key insight:** Root is always processed *first*. Used to clone/serialize a tree.

```python
# Recursive
def preorder(root):
    if not root:
        return []
    return [root.val] + preorder(root.left) + preorder(root.right)

# Iterative (stack)
def preorder_iterative(root):
    if not root:
        return []
    stack, result = [root], []
    while stack:
        node = stack.pop()
        result.append(node.val)
        if node.right: stack.append(node.right)  # right first (LIFO)
        if node.left:  stack.append(node.left)
    return result
```

---

### Inorder (Left → Root → Right)

**Key insight:** Inorder of a **BST always gives sorted output**. Most asked in interviews.

```python
# Recursive
def inorder(root):
    if not root:
        return []
    return inorder(root.left) + [root.val] + inorder(root.right)

# Iterative (explicit stack — very common interview question)
def inorder_iterative(root):
    stack, result = [], []
    curr = root
    while curr or stack:
        while curr:
            stack.append(curr)
            curr = curr.left
        curr = stack.pop()
        result.append(curr.val)
        curr = curr.right
    return result
```

---

### Postorder (Left → Right → Root)

**Key insight:** Root is processed *last*. Used to delete a tree or evaluate expression trees.

```python
# Recursive
def postorder(root):
    if not root:
        return []
    return postorder(root.left) + postorder(root.right) + [root.val]

# Iterative (two-stack trick)
def postorder_iterative(root):
    if not root:
        return []
    stack1, stack2 = [root], []
    while stack1:
        node = stack1.pop()
        stack2.append(node.val)
        if node.left:  stack1.append(node.left)
        if node.right: stack1.append(node.right)
    return stack2[::-1]
```

---

### Level Order / BFS

**Key insight:** Uses a **queue (deque)**. Returns nodes level by level. Foundation for many tree problems.

```python
from collections import deque

def level_order(root):
    if not root:
        return []
    queue = deque([root])
    result = []
    while queue:
        level_size = len(queue)
        level = []
        for _ in range(level_size):
            node = queue.popleft()
            level.append(node.val)
            if node.left:  queue.append(node.left)
            if node.right: queue.append(node.right)
        result.append(level)
    return result

# Output: [[1], [2, 3], [4, 5, 6, 7]]
```

> 💡 **Interview tip:** `level_size = len(queue)` before the inner loop is the key pattern to separate levels.

---

## 2. Tree Height

**Definition:** Number of edges on the longest root-to-leaf path. Some definitions use nodes (height + 1) — clarify with interviewer.

**Time:** O(n) — must visit every node.

```python
def height(root):
    if not root:
        return -1  # -1 for edges, 0 for nodes — clarify!
    return 1 + max(height(root.left), height(root.right))
```

**Iterative BFS approach:**

```python
def height_bfs(root):
    if not root:
        return -1
    queue = deque([root])
    h = -1
    while queue:
        h += 1
        for _ in range(len(queue)):
            node = queue.popleft()
            if node.left:  queue.append(node.left)
            if node.right: queue.append(node.right)
    return h
```

---

## 3. Tree Diameter

**Definition:** Longest path between **any two nodes** (path doesn't need to pass through root).

**Key insight:** At every node, diameter = `left_height + right_height`. Track global max.

```python
def diameter(root):
    max_d = [0]  # use list to allow mutation in nested function

    def height(node):
        if not node:
            return 0
        lh = height(node.left)
        rh = height(node.right)
        max_d[0] = max(max_d[0], lh + rh)  # update diameter at each node
        return 1 + max(lh, rh)

    height(root)
    return max_d[0]
```

> ⚠️ Common mistake: Computing height and diameter separately — O(n²). Always combine into one DFS pass — O(n).

---

## 4. Balanced Tree Check

**Definition:** A tree is balanced if for **every node**, `|left_height - right_height| <= 1`.

**Naive approach:** O(n²) — compute height separately per node.

**Optimal approach:** Return -1 as a sentinel for "unbalanced" during DFS — O(n).

```python
def is_balanced(root):
    def check(node):
        if not node:
            return 0
        left = check(node.left)
        if left == -1: return -1          # already unbalanced
        right = check(node.right)
        if right == -1: return -1         # already unbalanced
        if abs(left - right) > 1:
            return -1                     # this node unbalances it
        return 1 + max(left, right)

    return check(root) != -1
```

---

## 5. Mirror / Invert Tree

**Definition:** Swap left and right children at every node recursively.

```python
# Recursive
def mirror(root):
    if not root:
        return None
    root.left, root.right = mirror(root.right), mirror(root.left)
    return root

# Iterative BFS
def mirror_iterative(root):
    if not root:
        return root
    queue = deque([root])
    while queue:
        node = queue.popleft()
        node.left, node.right = node.right, node.left
        if node.left:  queue.append(node.left)
        if node.right: queue.append(node.right)
    return root
```

**Check if tree is symmetric (mirror of itself):**

```python
def is_symmetric(root):
    def is_mirror(l, r):
        if not l and not r: return True
        if not l or not r:  return False
        return (l.val == r.val and
                is_mirror(l.left, r.right) and
                is_mirror(l.right, r.left))

    return is_mirror(root.left, root.right) if root else True
```

---

## 6. Tree Boundary

**Definition:** Anti-clockwise boundary = left boundary (top-down) + leaves (left-to-right) + right boundary (bottom-up), **excluding duplicates**.

```python
def boundary(root):
    if not root:
        return []

    def is_leaf(node):
        return not node.left and not node.right

    def left_boundary(node):
        res = []
        while node:
            if not is_leaf(node): res.append(node.val)
            node = node.left if node.left else node.right
        return res

    def right_boundary(node):
        res = []
        while node:
            if not is_leaf(node): res.append(node.val)
            node = node.right if node.right else node.left
        return res[::-1]  # bottom-up

    def leaves(node):
        if not node: return []
        if is_leaf(node): return [node.val]
        return leaves(node.left) + leaves(node.right)

    return [root.val] + left_boundary(root.left) + leaves(root.left) + \
           leaves(root.right) + right_boundary(root.right)
```

> 💡 The three sections must be cleanly separated. Left/right boundaries **exclude** leaves to avoid duplicates at corners.

---

## 7. Revert (Invert) Tree

Revert = Invert = Mirror (same operation, different naming). See Mirror above.

```python
# If you're given the mirrored version and want original back,
# simply apply mirror() again — it's its own inverse.
def revert_tree(mirrored_root):
    return mirror(mirrored_root)
```

**Upside-down tree variant:**

```python
def upside_down(root):
    if not root or not root.left:
        return root
    new_root = upside_down(root.left)
    root.left.left  = root.right
    root.left.right = root
    root.left = None
    root.right = None
    return new_root
```

---

## Quick Complexity Reference

| Operation | Time | Space |
|---|---|---|
| All traversals | O(n) | O(h) recursive / O(n) BFS |
| Height | O(n) | O(h) |
| Diameter | O(n) | O(h) |
| Balanced check | O(n) | O(h) |
| Mirror/Invert | O(n) | O(h) |
| Boundary | O(n) | O(n) |

> `h` = tree height. Worst case (skewed tree): h = n. Best case (balanced): h = log n.

---

## Common Interview Patterns — Binary Trees

| Pattern | When to use |
|---|---|
| `return -1` as sentinel | Balanced check, any "validate while computing height" |
| `global max via list [0]` | Diameter, any path-sum max that updates mid-DFS |
| `level_size = len(queue)` | Any level-by-level BFS problem |
| Iterative inorder with `curr` pointer | BST problems, Morris traversal variations |
| Three-section split | Boundary, vertical order, zigzag |

---

---

# Part 2: Binary Search Tree (BST)

## Core Property

**Left subtree** values < node < **Right subtree** values. This holds for **every node**, not just root.

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
```

---

## 1. BST Operations Overview

| Operation | Average | Worst (skewed) |
|---|---|---|
| Search | O(log n) | O(n) |
| Insert | O(log n) | O(n) |
| Delete | O(log n) | O(n) |
| Inorder | O(n) | O(n) |

> ⚠️ Always clarify: is the BST **balanced**? If not, worst case is O(n). Interviewers may ask how to fix — answer: **AVL tree or Red-Black tree**.

---

## 2. Insert

**Key insight:** Recursively find the correct `None` slot. Never shifts existing nodes.

```python
# Recursive
def insert(root, val):
    if not root:
        return TreeNode(val)       # found the slot
    if val < root.val:
        root.left = insert(root.left, val)
    elif val > root.val:
        root.right = insert(root.right, val)
    # val == root.val: duplicate, do nothing (clarify with interviewer)
    return root

# Iterative
def insert_iterative(root, val):
    new_node = TreeNode(val)
    if not root:
        return new_node
    curr = root
    while True:
        if val < curr.val:
            if not curr.left:
                curr.left = new_node
                break
            curr = curr.left
        elif val > curr.val:
            if not curr.right:
                curr.right = new_node
                break
            curr = curr.right
        else:
            break   # duplicate
    return root
```

> 💡 Always return `root` — the root never changes on insert (unless tree was empty).

---

## 3. Delete

**The hardest BST operation.** Three cases:

```
Case 1: Node has NO children       → simply remove it
Case 2: Node has ONE child         → replace node with its child
Case 3: Node has TWO children      → replace with inorder successor
                                     (smallest in right subtree),
                                     then delete that successor
```

```python
def delete(root, val):
    if not root:
        return None

    if val < root.val:
        root.left = delete(root.left, val)
    elif val > root.val:
        root.right = delete(root.right, val)
    else:
        # Found the node to delete
        # Case 1 & 2: zero or one child
        if not root.left:
            return root.right
        if not root.right:
            return root.left

        # Case 3: two children
        # Find inorder successor (min of right subtree)
        successor = root.right
        while successor.left:
            successor = successor.left

        root.val = successor.val                         # copy successor value up
        root.right = delete(root.right, successor.val)  # delete successor

    return root
```

> ⚠️ Alternative: use **inorder predecessor** (max of left subtree). Both are valid — mention this to interviewer.

---

## 4. Search

```python
# Recursive
def search(root, val):
    if not root or root.val == val:
        return root
    if val < root.val:
        return search(root.left, val)
    return search(root.right, val)

# Iterative (preferred — no call stack overhead)
def search_iterative(root, val):
    curr = root
    while curr:
        if val == curr.val:
            return curr
        elif val < curr.val:
            curr = curr.left
        else:
            curr = curr.right
    return None  # not found
```

**Related — Find Min / Max:**

```python
def find_min(root):
    while root.left:
        root = root.left
    return root

def find_max(root):
    while root.right:
        root = root.right
    return root
```

---

## 5. Inorder Successor

**Definition:** The next node in **inorder traversal** (next greater value).

```
Scenario A: Node has a RIGHT subtree
            → successor = leftmost node in right subtree

Scenario B: Node has NO right subtree
            → successor = deepest ancestor where node is in LEFT subtree
```

```python
# When you have access to root (no parent pointer)
def inorder_successor(root, target):
    successor = None
    curr = root

    while curr:
        if target.val < curr.val:
            successor = curr        # curr could be the answer
            curr = curr.left
        elif target.val > curr.val:
            curr = curr.right
        else:
            # Found target — check right subtree
            if curr.right:
                successor = find_min(curr.right)
            break

    return successor


def find_min(node):
    while node.left:
        node = node.left
    return node
```

**Inorder Predecessor:**

```python
def inorder_predecessor(root, target):
    predecessor = None
    curr = root

    while curr:
        if target.val > curr.val:
            predecessor = curr      # curr could be the answer
            curr = curr.right
        elif target.val < curr.val:
            curr = curr.left
        else:
            if curr.left:
                predecessor = find_max(curr.left)
            break

    return predecessor
```

> 💡 **Interview tip:** No need to do inorder traversal and scan linearly — O(n). The BST property lets us find successor in O(h).

---

## 6. BST Validation

**Key insight:** Don't just check `left.val < root.val < right.val` at each node — that's **wrong** for deeper nodes.

**Wrong approach:**

```python
# BUG: only checks immediate children, misses deeper violations
def is_bst_wrong(root):
    if not root: return True
    if root.left and root.left.val >= root.val: return False
    if root.right and root.right.val <= root.val: return False
    return is_bst_wrong(root.left) and is_bst_wrong(root.right)
```

**Correct — pass min/max bounds down:**

```python
def is_valid_bst(root, min_val=float('-inf'), max_val=float('inf')):
    if not root:
        return True
    if not (min_val < root.val < max_val):
        return False
    return (is_valid_bst(root.left,  min_val, root.val) and
            is_valid_bst(root.right, root.val, max_val))

# Call: is_valid_bst(root)
```

**Alternative — inorder must be strictly increasing:**

```python
def is_valid_bst_inorder(root):
    prev = [float('-inf')]

    def inorder(node):
        if not node:
            return True
        if not inorder(node.left):
            return False
        if node.val <= prev[0]:        # must be strictly greater
            return False
        prev[0] = node.val
        return inorder(node.right)

    return inorder(root)
```

> 💡 The **bounds approach** is cleaner and more commonly expected in interviews.

---

## 7. Lowest Common Ancestor (LCA)

**Definition:** Lowest node that has both `p` and `q` as descendants.

```
If both p and q are less than root    → LCA is in LEFT subtree
If both p and q are greater than root → LCA is in RIGHT subtree
Otherwise                             → root IS the LCA
```

```python
# Recursive
def lca(root, p, q):
    if not root:
        return None
    if p.val < root.val and q.val < root.val:
        return lca(root.left, p, q)
    if p.val > root.val and q.val > root.val:
        return lca(root.right, p, q)
    return root     # split point — this is the LCA


# Iterative (O(1) space)
def lca_iterative(root, p, q):
    curr = root
    while curr:
        if p.val < curr.val and q.val < curr.val:
            curr = curr.left
        elif p.val > curr.val and q.val > curr.val:
            curr = curr.right
        else:
            return curr     # split point
    return None
```

> ⚠️ For a **general binary tree** (not BST), LCA requires a different recursive approach — mention this distinction.

---

## 8. Path to Node

**Definition:** Return the list of node values from root to the target node.

```python
def path_to_node(root, target):
    path = []

    def dfs(node):
        if not node:
            return False
        path.append(node.val)
        if node.val == target:
            return True                    # found it, stop
        # BST optimization: only go in one direction
        if target < node.val:
            if dfs(node.left): return True
        else:
            if dfs(node.right): return True
        path.pop()                         # backtrack
        return False

    dfs(root)
    return path
```

**Using path to find distance between two nodes:**

```python
def distance(root, p, q):
    def get_path(node, target):
        path = []
        while node:
            path.append(node.val)
            if target == node.val: break
            elif target < node.val: node = node.left
            else: node = node.right
        return path

    path_p = get_path(root, p)
    path_q = get_path(root, q)

    # Find where paths diverge
    i = 0
    while i < len(path_p) and i < len(path_q) and path_p[i] == path_q[i]:
        i += 1

    # Distance = remaining steps in each path after LCA
    return (len(path_p) - i) + (len(path_q) - i)
```

---

## Key Interview Patterns — BST

| Pattern | Technique |
|---|---|
| Validate BST | Pass `(min, max)` bounds through recursion |
| Find successor/predecessor | Leverage BST property — no full traversal |
| LCA in BST | Single pass using split-point logic |
| Path finding | DFS with backtracking + BST directional pruning |
| Delete node | Three cases: 0, 1, 2 children |
| Any "kth element" | Augment with subtree size OR inorder + counter |

---

## Classic Follow-up Questions — BST

**Q: Convert sorted array to BST?**

```python
def sorted_array_to_bst(nums):
    if not nums: return None
    mid = len(nums) // 2
    root = TreeNode(nums[mid])
    root.left  = sorted_array_to_bst(nums[:mid])
    root.right = sorted_array_to_bst(nums[mid+1:])
    return root
```

**Q: Find kth smallest in BST?**

```python
def kth_smallest(root, k):
    count = [0]
    result = [None]

    def inorder(node):
        if not node or result[0]: return
        inorder(node.left)
        count[0] += 1
        if count[0] == k:
            result[0] = node.val
            return
        inorder(node.right)

    inorder(root)
    return result[0]
```

---

---

# Part 3: Advanced Trees

---

## 1. AVL Trees

### Core Concept

A **self-balancing BST** where for every node: `|height(left) - height(right)| <= 1` (balance factor ∈ {-1, 0, 1}).

**Why AVL over plain BST?** Plain BST degrades to O(n) on sorted input. AVL guarantees O(log n) always.

```python
class AVLNode:
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None
        self.height = 1          # NEW: every node tracks its own height
```

### Height & Balance Factor

```python
def get_height(node):
    return node.height if node else 0

def get_balance(node):
    return get_height(node.left) - get_height(node.right) if node else 0

def update_height(node):
    node.height = 1 + max(get_height(node.left), get_height(node.right))
```

---

### The 4 Rotation Cases

```
Case 1 — Left Left (LL):    inserted into LEFT  of LEFT  child  → Right Rotate
Case 2 — Right Right (RR):  inserted into RIGHT of RIGHT child  → Left Rotate
Case 3 — Left Right (LR):   inserted into RIGHT of LEFT  child  → Left Rotate child, then Right Rotate
Case 4 — Right Left (RL):   inserted into LEFT  of RIGHT child  → Right Rotate child, then Left Rotate
```

```python
def right_rotate(z):
    #      z                y
    #     / \             /   \
    #    y   T4    →     x     z
    #   / \             / \   / \
    #  x   T3          T1 T2 T3 T4
    y = z.left
    T3 = y.right
    y.right = z
    z.left = T3
    update_height(z)
    update_height(y)
    return y          # y is the new root of this subtree

def left_rotate(z):
    y = z.right
    T2 = y.left
    y.left = z
    z.right = T2
    update_height(z)
    update_height(y)
    return y
```

### AVL Insert

```python
def avl_insert(root, val):
    # Step 1: Normal BST insert
    if not root:
        return AVLNode(val)
    if val < root.val:
        root.left = avl_insert(root.left, val)
    elif val > root.val:
        root.right = avl_insert(root.right, val)
    else:
        return root     # duplicate

    # Step 2: Update height
    update_height(root)

    # Step 3: Get balance factor
    balance = get_balance(root)

    # Step 4: Fix violation — 4 cases
    # LL
    if balance > 1 and val < root.left.val:
        return right_rotate(root)
    # RR
    if balance < -1 and val > root.right.val:
        return left_rotate(root)
    # LR
    if balance > 1 and val > root.left.val:
        root.left = left_rotate(root.left)
        return right_rotate(root)
    # RL
    if balance < -1 and val < root.right.val:
        root.right = right_rotate(root.right)
        return left_rotate(root)

    return root
```

> 💡 **Interview tip:** You rarely need to implement full AVL deletion. Know the rotation logic and balance factor restoration — that's what's tested.

---

## 2. Segment Tree

### Core Concept

A tree used for **range queries** (sum, min, max) and **point updates** in O(log n).

```
Array:  [1, 3, 5, 7, 9, 11]
Use cases:
  - Sum of elements from index 2 to 5?
  - Update index 3 to value 10, then re-query
  → Both in O(log n) vs O(n) brute force
```

### Array-based Segment Tree

```
For node at index i:
  Left child  → 2*i
  Right child → 2*i + 1
  Parent      → i // 2
Tree size = 4 * n (safe upper bound)
```

```python
class SegmentTree:
    def __init__(self, nums):
        self.n = len(nums)
        self.tree = [0] * (4 * self.n)
        if nums:
            self.build(nums, 0, self.n - 1, 1)

    def build(self, nums, start, end, node):
        if start == end:
            self.tree[node] = nums[start]   # leaf node
            return
        mid = (start + end) // 2
        self.build(nums, start, mid,   2 * node)
        self.build(nums, mid+1, end,   2 * node + 1)
        self.tree[node] = self.tree[2*node] + self.tree[2*node+1]  # merge

    def update(self, idx, val, start=None, end=None, node=1):
        if start is None: start = 0
        if end   is None: end   = self.n - 1

        if start == end:
            self.tree[node] = val           # update leaf
            return
        mid = (start + end) // 2
        if idx <= mid:
            self.update(idx, val, start, mid,   2 * node)
        else:
            self.update(idx, val, mid+1, end,   2 * node + 1)
        self.tree[node] = self.tree[2*node] + self.tree[2*node+1]

    def query(self, l, r, start=None, end=None, node=1):
        if start is None: start = 0
        if end   is None: end   = self.n - 1

        if r < start or end < l:
            return 0                        # out of range — identity for sum
        if l <= start and end <= r:
            return self.tree[node]          # fully inside range
        mid = (start + end) // 2
        left_sum  = self.query(l, r, start, mid,   2 * node)
        right_sum = self.query(l, r, mid+1, end,   2 * node + 1)
        return left_sum + right_sum


# Usage
nums = [1, 3, 5, 7, 9, 11]
st = SegmentTree(nums)
print(st.query(1, 3))     # sum of indices 1..3 = 3+5+7 = 15
st.update(1, 10)           # set index 1 to 10
print(st.query(1, 3))     # = 10+5+7 = 22
```

### Segment Tree Complexity

| Operation | Time | Space |
|---|---|---|
| Build | O(n) | O(n) |
| Query | O(log n) | O(log n) stack |
| Update | O(log n) | O(log n) stack |

> ⚠️ **For range updates**, you need **Lazy Propagation** — mention this if interviewer asks about bulk updates.

---

## 3. Fenwick Tree (Binary Indexed Tree / BIT)

### Core Concept

Simpler alternative to Segment Tree for **prefix sum queries** and **point updates**. Uses clever bit manipulation.

```
Key operation: i & (-i)
  Gives the lowest set bit of i
  Used to navigate the BIT structure

Example: i=6 → binary 110 → i&(-i) = 010 = 2
```

```python
class FenwickTree:
    def __init__(self, n):
        self.n = n
        self.tree = [0] * (n + 1)   # 1-indexed!

    def update(self, i, delta):
        """Add delta to index i (1-indexed)"""
        while i <= self.n:
            self.tree[i] += delta
            i += i & (-i)           # move to parent

    def prefix_sum(self, i):
        """Sum of elements from index 1 to i"""
        total = 0
        while i > 0:
            total += self.tree[i]
            i -= i & (-i)           # move to predecessor
        return total

    def range_sum(self, l, r):
        """Sum of elements from l to r (1-indexed)"""
        return self.prefix_sum(r) - self.prefix_sum(l - 1)

    def build(self, nums):
        """Build from array (0-indexed input)"""
        for i, val in enumerate(nums):
            self.update(i + 1, val) # convert to 1-indexed


# Usage
ft = FenwickTree(6)
ft.build([1, 3, 5, 7, 9, 11])
print(ft.range_sum(2, 4))    # 3+5+7 = 15 (1-indexed)
ft.update(2, 7)               # add 7 to index 2 → becomes 10
print(ft.range_sum(2, 4))    # 10+5+7 = 22
```

### BIT vs Segment Tree

| | Fenwick Tree | Segment Tree |
|---|---|---|
| Code complexity | Simple | More complex |
| Space | O(n) | O(4n) |
| Point update | O(log n) | O(log n) |
| Range query | O(log n) | O(log n) |
| Range update | Harder | Easy with lazy |
| Range min/max | ❌ Not straightforward | ✅ Easy |

> 💡 **Rule of thumb:** Use BIT for prefix sums/counts. Use Segment Tree when you need range min/max or range updates.

---

## 4. Trie (Prefix Tree)

### Core Concept

A tree where each **path from root to node** represents a string prefix. Every character is an edge, not stored in the node itself.

```
Insert: ["apple", "app", "apt", "bat"]

        root
       /    \
      a      b
      |      |
      p      a
     / \      \
    p   t      t
    |   |
    l   (end)
    |
    e
   (end)
```

```python
class TrieNode:
    def __init__(self):
        self.children = {}          # char → TrieNode
        self.is_end = False         # marks end of a valid word
        self.count = 0              # optional: how many words pass through

class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word):
        node = self.root
        for ch in word:
            if ch not in node.children:
                node.children[ch] = TrieNode()
            node = node.children[ch]
            node.count += 1         # track prefix frequency
        node.is_end = True

    def search(self, word):
        """Returns True if exact word exists"""
        node = self.root
        for ch in word:
            if ch not in node.children:
                return False
            node = node.children[ch]
        return node.is_end

    def starts_with(self, prefix):
        """Returns True if any word starts with prefix"""
        node = self.root
        for ch in prefix:
            if ch not in node.children:
                return False
            node = node.children[ch]
        return True

    def delete(self, word):
        """DFS delete — only removes nodes not shared by other words"""
        def dfs(node, word, depth):
            if depth == len(word):
                if not node.is_end:
                    return False        # word doesn't exist
                node.is_end = False
                return len(node.children) == 0  # safe to delete if leaf
            ch = word[depth]
            if ch not in node.children:
                return False
            should_delete = dfs(node.children[ch], word, depth + 1)
            if should_delete:
                del node.children[ch]
                return not node.is_end and len(node.children) == 0
            return False
        dfs(self.root, word, 0)
```

### Trie Complexity

| Operation | Time | Space |
|---|---|---|
| Insert | O(m) | O(m) per word |
| Search | O(m) | O(1) |
| Starts with | O(m) | O(1) |
| Delete | O(m) | O(1) |

`m` = length of word. Total space = O(n × m) for n words of average length m.

---

## 5. Prefix Search

**Definition:** Return all words in the Trie that begin with a given prefix.

```python
def prefix_search(self, prefix):
    """Return all words starting with prefix"""
    node = self.root

    # Navigate to end of prefix
    for ch in prefix:
        if ch not in node.children:
            return []               # prefix not found
        node = node.children[ch]

    # DFS from that node to collect all words
    results = []
    self._collect_words(node, prefix, results)
    return results

def _collect_words(self, node, current, results):
    if node.is_end:
        results.append(current)
    for ch, child in node.children.items():
        self._collect_words(child, current + ch, results)
```

**Count words with prefix (using stored count):**

```python
def count_prefix(self, prefix):
    node = self.root
    for ch in prefix:
        if ch not in node.children:
            return 0
        node = node.children[ch]
    return node.count       # pre-computed during insert
```

---

## 6. Auto-complete

Builds directly on Trie + prefix search. Real interview variant adds **ranking by frequency**.

### Basic Auto-complete

```python
class AutoComplete:
    def __init__(self):
        self.trie = Trie()

    def add_word(self, word):
        self.trie.insert(word)

    def suggest(self, prefix):
        return self.trie.prefix_search(prefix)
```

### Auto-complete with Frequency Ranking

```python
class FreqTrieNode:
    def __init__(self):
        self.children = {}
        self.is_end = False
        self.freq = 0               # how many times this word was searched

class SmartAutoComplete:
    def __init__(self):
        self.root = FreqTrieNode()

    def insert(self, word, freq=1):
        node = self.root
        for ch in word:
            if ch not in node.children:
                node.children[ch] = FreqTrieNode()
            node = node.children[ch]
        node.is_end = True
        node.freq += freq

    def suggest(self, prefix, top_k=3):
        node = self.root
        for ch in prefix:
            if ch not in node.children:
                return []
            node = node.children[ch]

        results = []
        self._dfs(node, prefix, results)
        # Sort by frequency descending, return top k
        results.sort(key=lambda x: -x[1])
        return [word for word, freq in results[:top_k]]

    def _dfs(self, node, path, results):
        if node.is_end:
            results.append((path, node.freq))
        for ch, child in node.children.items():
            self._dfs(child, path + ch, results)


# Usage
ac = SmartAutoComplete()
for word, freq in [("apple", 10), ("app", 25), ("application", 5),
                   ("apply", 15), ("bat", 8)]:
    ac.insert(word, freq)

print(ac.suggest("app"))
# → ["app", "apply", "apple"]   (sorted by freq: 25, 15, 10)
```

### Auto-complete with Top-K using Heap (optimized)

```python
import heapq

def suggest_topk(self, prefix, k=3):
    node = self.root
    for ch in prefix:
        if ch not in node.children:
            return []
        node = node.children[ch]

    heap = []   # min-heap of size k: (freq, word)

    def dfs(node, path):
        if node.is_end:
            heapq.heappush(heap, (node.freq, path))
            if len(heap) > k:
                heapq.heappop(heap)   # evict lowest freq
        for ch, child in node.children.items():
            dfs(child, path + ch)

    dfs(node, prefix)
    return [word for freq, word in sorted(heap, key=lambda x: -x[0])]
```

> 💡 **Min-heap of size k** is the standard pattern for "top-k" problems. Push everything, pop when size exceeds k — keeps the k largest.

---

## Combined Complexity Reference

| Structure | Build | Query | Update | Space |
|---|---|---|---|---|
| AVL Tree | O(n log n) | O(log n) | O(log n) | O(n) |
| Segment Tree | O(n) | O(log n) | O(log n) | O(4n) |
| Fenwick Tree | O(n log n) | O(log n) | O(log n) | O(n) |
| Trie | O(n×m) | O(m) | O(m) | O(n×m) |

---

## Interview Decision Guide

```
Range sum + point update only?        → Fenwick Tree (simpler code)
Range min/max or range updates?       → Segment Tree
Self-balancing BST guarantees?        → AVL Tree (or mention Red-Black)
Prefix matching / word search?        → Trie
Autocomplete with ranking?            → Trie + frequency + min-heap (top-k)
```

---

## Classic Follow-up Questions — Advanced Trees

**Q: Word Search II (find all words from board)?**

> Build a Trie from the word list, then DFS on board — prune using Trie instead of checking each word separately. O(m × 4 × 3^(L-1)) vs O(m × n × W).

**Q: Longest common prefix of all strings?**

```python
def longest_common_prefix(self):
    node = self.root
    prefix = ""
    # Keep going while only one child and not end of word
    while len(node.children) == 1 and not node.is_end:
        ch = next(iter(node.children))
        prefix += ch
        node = node.children[ch]
    return prefix
```

**Q: Segment Tree for range minimum instead of sum?**

```python
# Only change: merge function and identity value
self.tree[node] = min(self.tree[2*node], self.tree[2*node+1])
# out-of-range identity becomes float('inf') instead of 0
```


# Graph Interview Notes — Complete Reference

> **Topics Covered:** Graph Basics · Traversal · Shortest Path · Advanced Graph  
> **Language:** Python

---

# Table of Contents

1. [Graph Basics](#graph-basics)
   - [Adjacency List](#adjacency-list)
   - [Adjacency Matrix](#adjacency-matrix)
   - [Graph Representation](#graph-representation)
   - [Directed Graphs](#directed-graphs)
   - [Undirected Graphs](#undirected-graphs)
   - [Weighted Graphs](#weighted-graphs)
2. [Traversal](#traversal)
   - [Depth First Search (DFS)](#depth-first-search-dfs)
   - [Breadth First Search (BFS)](#breadth-first-search-bfs)
   - [Connected Components](#connected-components)
   - [Cycle Detection](#cycle-detection)
   - [Topological Sort](#topological-sort)
   - [Strongly Connected Components (SCC)](#strongly-connected-components-scc)
3. [Shortest Path](#shortest-path)
   - [Dijkstra's Algorithm](#dijkstras-algorithm)
   - [Bellman-Ford](#bellman-ford)
   - [Floyd-Warshall](#floyd-warshall)
   - [0-1 BFS](#0-1-bfs)
4. [Advanced Graph](#advanced-graph)
   - [Minimum Spanning Tree (MST)](#minimum-spanning-tree-mst)
   - [Union-Find (DSU)](#union-find-disjoint-set-union---dsu)
   - [Bipartite Graph Check](#bipartite-graph-check)
   - [Graph Coloring](#graph-coloring)

---

# Graph Basics

---

## Adjacency List

### Concept
- Stores a graph as a dictionary/array where each key is a node and its value is a list of neighbors.
- Most common representation in interviews — default choice unless asked otherwise.
- Space: **O(V + E)** — only stores existing edges.

### When to Use
- Sparse graphs (few edges relative to nodes)
- Most real-world graphs (social networks, road maps)
- When you need to iterate over neighbors efficiently

### Python Implementation

```python
# Using defaultdict — most interview-friendly
from collections import defaultdict

graph = defaultdict(list)

# Adding edges (undirected)
graph[1].append(2)
graph[2].append(1)
graph[1].append(3)
graph[3].append(1)

# Adding edges (directed)
graph[1].append(4)  # only 1 → 4, NOT 4 → 1

print(dict(graph))
# {1: [2, 3, 4], 2: [1], 3: [1]}
```

```python
# Building from edge list — very common in interviews
def build_graph(n, edges, directed=False):
    graph = defaultdict(list)
    for u, v in edges:
        graph[u].append(v)
        if not directed:
            graph[v].append(u)
    return graph

edges = [(1,2), (1,3), (2,4)]
g = build_graph(4, edges)
```

```python
# Weighted adjacency list
graph = defaultdict(list)
graph[1].append((2, 5))   # (neighbor, weight)
graph[1].append((3, 10))
graph[2].append((4, 3))
```

### Interview Traps
- Always clarify: **directed or undirected?** Undirected → add both directions.
- For weighted graphs, store tuples `(neighbor, weight)`.
- `defaultdict(list)` avoids KeyError — prefer it over plain `{}`.

---

## Adjacency Matrix

### Concept
- 2D array/matrix `matrix[i][j]` = 1 (or weight) if edge i→j exists, else 0.
- Space: **O(V²)** — stores all possible edges, even non-existent ones.
- Edge lookup: **O(1)** — direct index access.

### When to Use
- Dense graphs (many edges, E ≈ V²)
- When you need **O(1) edge existence checks**
- Floyd-Warshall, some DP on graphs
- Small graphs (V ≤ 1000 typically)

### Python Implementation

```python
# Build adjacency matrix
n = 4  # nodes 0..3
matrix = [[0] * n for _ in range(n)]

# Add undirected edge
def add_edge(matrix, u, v, weight=1):
    matrix[u][v] = weight
    matrix[v][u] = weight  # remove for directed

add_edge(matrix, 0, 1)
add_edge(matrix, 0, 2)
add_edge(matrix, 1, 3)

# Check if edge exists
print(matrix[0][1])  # 1 → exists
print(matrix[0][3])  # 0 → doesn't exist
```

```python
# Weighted adjacency matrix
INF = float('inf')
n = 4
matrix = [[INF] * n for _ in range(n)]

# Distance to self = 0
for i in range(n):
    matrix[i][i] = 0

matrix[0][1] = 5
matrix[1][2] = 3
matrix[0][2] = 10

# Floyd-Warshall uses this directly
for k in range(n):
    for i in range(n):
        for j in range(n):
            matrix[i][j] = min(matrix[i][j], matrix[i][k] + matrix[k][j])
```

### Interview Traps
- 1-indexed vs 0-indexed node labeling — clarify before coding.
- Memory blow-up for large sparse graphs (10⁵ nodes → 10¹⁰ cells = infeasible).
- Self-loops: `matrix[i][i]` — ask interviewer if they're possible.

---

## Graph Representation

### Comparison Table

| Feature | Adjacency List | Adjacency Matrix |
|---|---|---|
| Space | O(V + E) | O(V²) |
| Add edge | O(1) | O(1) |
| Remove edge | O(degree) | O(1) |
| Check edge (u,v) | O(degree) | O(1) |
| Get all neighbors | O(degree) | O(V) |
| Best for | Sparse graphs | Dense graphs |

### Edge List Representation

```python
# Simplest — just store all edges
# Used in Kruskal's MST, Union-Find problems
edges = [(0,1,4), (0,2,1), (1,3,2)]  # (u, v, weight)

# Sort edges by weight — common pattern
edges.sort(key=lambda x: x[2])
```

### Converting Between Representations

```python
# Edge list → Adjacency list
def edge_list_to_adj(n, edges, directed=False):
    graph = defaultdict(list)
    for u, v in edges:
        graph[u].append(v)
        if not directed:
            graph[v].append(u)
    return graph

# Adjacency matrix → Adjacency list
def matrix_to_adj(matrix):
    n = len(matrix)
    graph = defaultdict(list)
    for i in range(n):
        for j in range(n):
            if matrix[i][j] != 0:
                graph[i].append(j)
    return graph
```

---

## Directed Graphs

### Concept
- Edges have direction: u → v does NOT imply v → u.
- In-degree: number of edges **coming into** a node.
- Out-degree: number of edges **going out of** a node.

### Python Implementation

```python
from collections import defaultdict, deque

# Build directed graph
def build_directed(n, edges):
    graph = defaultdict(list)
    in_degree = [0] * (n + 1)
    for u, v in edges:
        graph[u].append(v)
        in_degree[v] += 1       # track in-degree for topo sort
    return graph, in_degree
```

```python
# Cycle detection in directed graph — DFS with 3 states
# WHITE=0 (unvisited), GRAY=1 (in stack), BLACK=2 (done)
def has_cycle_directed(graph, n):
    color = [0] * n

    def dfs(node):
        color[node] = 1  # GRAY — currently visiting
        for neighbor in graph[node]:
            if color[neighbor] == 1:
                return True   # back edge → cycle
            if color[neighbor] == 0:
                if dfs(neighbor):
                    return True
        color[node] = 2  # BLACK — fully processed
        return False

    for i in range(n):
        if color[i] == 0:
            if dfs(i):
                return True
    return False
```

```python
# Topological Sort — Kahn's Algorithm (BFS)
def topo_sort(n, edges):
    graph, in_degree = build_directed(n, edges)
    queue = deque([i for i in range(1, n+1) if in_degree[i] == 0])
    order = []

    while queue:
        node = queue.popleft()
        order.append(node)
        for neighbor in graph[node]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)

    return order if len(order) == n else []  # empty → cycle exists
```

### Key Interview Points
- **Topological sort only works on DAGs** (Directed Acyclic Graphs).
- Cycle detection: use **visited + recursion stack** (not just visited).
- Transpose graph (reverse all edges) → used in Kosaraju's SCC algorithm.
- Common problems: Course Schedule (LC 207/210), Alien Dictionary.

---

## Undirected Graphs

### Python Implementation

```python
# BFS traversal — undirected
from collections import deque

def bfs(graph, start, n):
    visited = set([start])
    queue = deque([start])
    order = []

    while queue:
        node = queue.popleft()
        order.append(node)
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
    return order
```

```python
# DFS traversal — undirected
def dfs(graph, node, visited):
    visited.add(node)
    for neighbor in graph[node]:
        if neighbor not in visited:
            dfs(graph, neighbor, visited)

# Count connected components
def count_components(n, edges):
    graph = build_graph(n, edges)
    visited = set()
    count = 0
    for i in range(1, n+1):
        if i not in visited:
            dfs(graph, i, visited)
            count += 1
    return count
```

```python
# Cycle detection in undirected graph — DFS
def has_cycle_undirected(graph, n):
    visited = set()

    def dfs(node, parent):
        visited.add(node)
        for neighbor in graph[node]:
            if neighbor not in visited:
                if dfs(neighbor, node):
                    return True
            elif neighbor != parent:   # visited AND not parent → cycle
                return True
        return False

    for i in range(n):
        if i not in visited:
            if dfs(i, -1):
                return True
    return False
```

```python
# Bipartite check — 2-coloring via BFS
def is_bipartite(graph, n):
    color = [-1] * n

    for start in range(n):
        if color[start] != -1:
            continue
        queue = deque([start])
        color[start] = 0

        while queue:
            node = queue.popleft()
            for neighbor in graph[node]:
                if color[neighbor] == -1:
                    color[neighbor] = 1 - color[node]  # flip color
                    queue.append(neighbor)
                elif color[neighbor] == color[node]:
                    return False  # same color → not bipartite
    return True
```

### Key Interview Points
- Cycle detection: track **parent** node to avoid false positives.
- **Union-Find** is often cleaner for cycle detection in undirected graphs.
- Bipartite = can be 2-colored = no odd-length cycles.
- Always handle **disconnected graphs** — loop over all nodes, not just node 0.

---

## Weighted Graphs

### Dijkstra's (non-negative weights)

```python
import heapq

def dijkstra(graph, start, n):
    dist = [float('inf')] * (n + 1)
    dist[start] = 0
    min_heap = [(0, start)]  # (distance, node)

    while min_heap:
        d, node = heapq.heappop(min_heap)

        if d > dist[node]:   # stale entry — skip
            continue

        for neighbor, weight in graph[node]:
            new_dist = dist[node] + weight
            if new_dist < dist[neighbor]:
                dist[neighbor] = new_dist
                heapq.heappush(min_heap, (new_dist, neighbor))

    return dist
```

```python
# Bellman-Ford — handles negative weights
def bellman_ford(n, edges, start):
    dist = [float('inf')] * (n + 1)
    dist[start] = 0

    for _ in range(n - 1):
        for u, v, w in edges:
            if dist[u] + w < dist[v]:
                dist[v] = dist[u] + w

    # Check for negative weight cycles
    for u, v, w in edges:
        if dist[u] + w < dist[v]:
            return None  # negative cycle detected

    return dist
```

```python
# Prim's MST — greedy, uses min-heap
def prims_mst(graph, n):
    visited = set()
    min_heap = [(0, 0)]  # (weight, node) — start from node 0
    total_cost = 0

    while min_heap and len(visited) < n:
        cost, node = heapq.heappop(min_heap)
        if node in visited:
            continue
        visited.add(node)
        total_cost += cost

        for neighbor, weight in graph[node]:
            if neighbor not in visited:
                heapq.heappush(min_heap, (weight, neighbor))

    return total_cost if len(visited) == n else -1  # -1 if disconnected
```

```python
# Kruskal's MST — Union-Find based
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])  # path compression
        return self.parent[x]

    def union(self, x, y):
        px, py = self.find(x), self.find(y)
        if px == py:
            return False  # already connected → adding edge = cycle
        if self.rank[px] < self.rank[py]:
            px, py = py, px
        self.parent[py] = px
        if self.rank[px] == self.rank[py]:
            self.rank[px] += 1
        return True

def kruskal_mst(n, edges):
    edges.sort(key=lambda x: x[2])  # sort by weight
    uf = UnionFind(n)
    mst_cost = 0
    edges_used = 0

    for u, v, w in edges:
        if uf.union(u, v):
            mst_cost += w
            edges_used += 1
            if edges_used == n - 1:
                break

    return mst_cost if edges_used == n - 1 else -1
```

### Algorithm Comparison

| Algorithm | Time | Use Case |
|---|---|---|
| Dijkstra | O((V+E) log V) | Non-negative weights, SSSP |
| Bellman-Ford | O(VE) | Negative weights, detect neg cycles |
| Floyd-Warshall | O(V³) | All-pairs shortest path |
| Prim's | O(E log V) | MST, dense graphs |
| Kruskal's | O(E log E) | MST, sparse graphs |

### Key Interview Points
- Dijkstra **fails** with negative weights — always flag this.
- Stale heap entries are handled by the `if d > dist[node]: continue` guard.
- Kruskal's needs edges upfront; Prim's works incrementally — mention this tradeoff.

---

### Quick Decision Tree

```
Need shortest path?
  → Unweighted: BFS
  → Weighted, non-negative: Dijkstra
  → Weighted, negative: Bellman-Ford
  → All pairs: Floyd-Warshall

Need ordering/scheduling?
  → Topological Sort (detect cycle = impossible schedule)

Need connected components / cycle detection?
  → Undirected: BFS/DFS or Union-Find
  → Directed: DFS with 3-color marking

Need minimum spanning tree?
  → Sparse: Kruskal's + Union-Find
  → Dense: Prim's + min-heap
```

---

# Traversal

---

## Depth First Search (DFS)

### Concept
- Explore as **deep as possible** before backtracking.
- Uses a **stack** (implicit via recursion, or explicit stack iteratively).
- Time: **O(V + E)** | Space: **O(V)** for visited + recursion stack.

### Recursive DFS

```python
from collections import defaultdict

def dfs_recursive(graph, node, visited):
    visited.add(node)
    print(node)  # process node

    for neighbor in graph[node]:
        if neighbor not in visited:
            dfs_recursive(graph, neighbor, visited)

# Driver
graph = defaultdict(list)
edges = [(0,1),(0,2),(1,3),(2,4)]
for u, v in edges:
    graph[u].append(v)
    graph[v].append(u)

visited = set()
dfs_recursive(graph, 0, visited)
```

### Iterative DFS

```python
def dfs_iterative(graph, start):
    visited = set()
    stack = [start]
    order = []

    while stack:
        node = stack.pop()          # LIFO — key difference from BFS
        if node in visited:
            continue
        visited.add(node)
        order.append(node)

        for neighbor in graph[node]:
            if neighbor not in visited:
                stack.append(neighbor)

    return order
```

### DFS on a Grid

```python
# LC 200 — Number of Islands pattern
def dfs_grid(grid, row, col, visited):
    rows, cols = len(grid), len(grid[0])
    directions = [(0,1),(0,-1),(1,0),(-1,0)]

    if (row < 0 or row >= rows or
        col < 0 or col >= cols or
        (row, col) in visited or
        grid[row][col] == '0'):
        return

    visited.add((row, col))

    for dr, dc in directions:
        dfs_grid(grid, row + dr, col + dc, visited)

def num_islands(grid):
    if not grid:
        return 0
    visited = set()
    count = 0

    for r in range(len(grid)):
        for c in range(len(grid[0])):
            if grid[r][c] == '1' and (r, c) not in visited:
                dfs_grid(grid, r, c, visited)
                count += 1
    return count
```

### DFS Path Finding

```python
# Check if path exists between src and dst
def has_path(graph, src, dst, visited):
    if src == dst:
        return True
    visited.add(src)

    for neighbor in graph[src]:
        if neighbor not in visited:
            if has_path(graph, neighbor, dst, visited):
                return True
    return False

# Collect ALL paths from src to dst (backtracking)
def all_paths(graph, src, dst):
    result = []

    def dfs(node, path):
        if node == dst:
            result.append(list(path))
            return
        for neighbor in graph[node]:
            path.append(neighbor)
            dfs(neighbor, path)
            path.pop()          # backtrack

    dfs(src, [src])
    return result
```

### Key Interview Points
- Python default recursion limit = **1000** — mention `sys.setrecursionlimit()` or switch to iterative for large inputs.
- Iterative DFS visits neighbors in **reverse order** vs recursive — acceptable unless exact order matters.
- Always mark node visited **before** recursing (not after) to avoid infinite loops.

---

## Breadth First Search (BFS)

### Concept
- Explore all nodes at current depth **before going deeper**.
- Uses a **queue** (deque for O(1) popleft).
- Guarantees **shortest path** in unweighted graphs.
- Time: **O(V + E)** | Space: **O(V)**

### Standard BFS

```python
from collections import deque

def bfs(graph, start):
    visited = set([start])
    queue = deque([start])
    order = []

    while queue:
        node = queue.popleft()      # FIFO — key difference from DFS
        order.append(node)

        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)   # mark visited ON ENQUEUE not dequeue
                queue.append(neighbor)

    return order
```

### BFS with Level Tracking

```python
# Returns {node: distance_from_start}
def bfs_distances(graph, start):
    visited = set([start])
    queue = deque([(start, 0)])     # (node, level)
    distances = {start: 0}

    while queue:
        node, level = queue.popleft()

        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                distances[neighbor] = level + 1
                queue.append((neighbor, level + 1))

    return distances

# Process level by level
def bfs_by_level(graph, start):
    visited = set([start])
    queue = deque([start])
    levels = []

    while queue:
        level_size = len(queue)         # snapshot current level
        current_level = []

        for _ in range(level_size):     # process exactly this level
            node = queue.popleft()
            current_level.append(node)
            for neighbor in graph[node]:
                if neighbor not in visited:
                    visited.add(neighbor)
                    queue.append(neighbor)

        levels.append(current_level)

    return levels
```

### BFS Shortest Path (reconstruct actual path)

```python
def bfs_shortest_path(graph, start, end):
    if start == end:
        return [start]

    visited = set([start])
    queue = deque([[start]])            # store full path in queue

    while queue:
        path = queue.popleft()
        node = path[-1]

        for neighbor in graph[node]:
            if neighbor not in visited:
                new_path = path + [neighbor]
                if neighbor == end:
                    return new_path     # found shortest path
                visited.add(neighbor)
                queue.append(new_path)

    return []   # no path exists
```

### Multi-Source BFS

```python
# LC 994 — Rotting Oranges pattern
def multi_source_bfs(grid):
    rows, cols = len(grid), len(grid[0])
    queue = deque()
    visited = set()

    # Enqueue ALL starting nodes first
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == 2:             # rotten orange
                queue.append(((r, c), 0))
                visited.add((r, c))

    max_time = 0
    directions = [(0,1),(0,-1),(1,0),(-1,0)]

    while queue:
        (r, c), time = queue.popleft()
        max_time = max(max_time, time)

        for dr, dc in directions:
            nr, nc = r + dr, c + dc
            if (0 <= nr < rows and 0 <= nc < cols and
                (nr, nc) not in visited and
                grid[nr][nc] == 1):
                visited.add((nr, nc))
                queue.append(((nr, nc), time + 1))

    return max_time
```

### BFS vs DFS

| | BFS | DFS |
|---|---|---|
| Data Structure | Queue (deque) | Stack / Recursion |
| Shortest Path | ✅ Guaranteed (unweighted) | ❌ Not guaranteed |
| Memory | O(V) — wide levels | O(V) — depth of graph |
| Cycle Detection | ✅ Yes | ✅ Yes |
| Topological Sort | ✅ Kahn's Algorithm | ✅ DFS-based |
| Best for | Shortest path, level order | Path existence, cycles, topo sort |

### Key Interview Points
- Mark visited **when enqueuing**, not dequeuing — prevents adding duplicates.
- `deque` is mandatory — `list.pop(0)` is O(n), `deque.popleft()` is O(1).
- BFS = shortest path **only for unweighted** — use Dijkstra for weighted.
- Multi-source BFS: enqueue ALL sources at time=0, then BFS simultaneously.

---

## Connected Components

### BFS/DFS Approach

```python
# Count + label connected components
def connected_components(n, edges):
    graph = defaultdict(list)
    for u, v in edges:
        graph[u].append(v)
        graph[v].append(u)

    visited = {}    # node → component_id
    component_id = 0

    def dfs(node, cid):
        visited[node] = cid
        for neighbor in graph[node]:
            if neighbor not in visited:
                dfs(neighbor, cid)

    for node in range(n):
        if node not in visited:
            dfs(node, component_id)
            component_id += 1

    return component_id, visited    # count, node→component mapping
```

### Union-Find Approach

```python
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank   = [0] * n
        self.count  = n             # track component count

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])  # path compression
        return self.parent[x]

    def union(self, x, y):
        px, py = self.find(x), self.find(y)
        if px == py:
            return False            # already in same component
        if self.rank[px] < self.rank[py]:
            px, py = py, px
        self.parent[py] = px
        if self.rank[px] == self.rank[py]:
            self.rank[px] += 1
        self.count -= 1             # merged two components
        return True

    def connected(self, x, y):
        return self.find(x) == self.find(y)

def count_components_uf(n, edges):
    uf = UnionFind(n)
    for u, v in edges:
        uf.union(u, v)
    return uf.count

print(count_components_uf(5, [(0,1),(1,2),(3,4)]))  # 2
```

### Largest Component

```python
def largest_component(graph, n):
    visited = set()

    def dfs_size(node):
        visited.add(node)
        size = 1
        for neighbor in graph[node]:
            if neighbor not in visited:
                size += dfs_size(neighbor)
        return size

    return max(dfs_size(i) for i in range(n) if i not in visited)
```

### Key Interview Points
- Union-Find is best for **dynamic edge additions** (online connectivity queries).
- BFS/DFS is better when you also need to **identify which nodes** are in each component.
- Always iterate over **all nodes** — don't assume the graph is connected.

---

## Cycle Detection

### Undirected Graph — DFS with Parent Tracking

```python
def has_cycle_undirected(n, edges):
    graph = defaultdict(list)
    for u, v in edges:
        graph[u].append(v)
        graph[v].append(u)

    visited = set()

    def dfs(node, parent):
        visited.add(node)
        for neighbor in graph[node]:
            if neighbor not in visited:
                if dfs(neighbor, node):
                    return True
            elif neighbor != parent:    # visited AND not parent → back edge → CYCLE
                return True
        return False

    for node in range(n):
        if node not in visited:
            if dfs(node, -1):
                return True
    return False
```

### Undirected Graph — Union-Find

```python
def has_cycle_undirected_uf(n, edges):
    uf = UnionFind(n)
    for u, v in edges:
        if not uf.union(u, v):  # already connected → adding edge = cycle
            return True
    return False
```

### Directed Graph — DFS with 3-Color Marking

```python
# WHITE=0: unvisited, GRAY=1: in current DFS stack, BLACK=2: fully processed
def has_cycle_directed(n, edges):
    graph = defaultdict(list)
    for u, v in edges:
        graph[u].append(v)

    color = [0] * n     # all WHITE initially

    def dfs(node):
        color[node] = 1     # GRAY — on current path
        for neighbor in graph[node]:
            if color[neighbor] == 1:
                return True     # back edge to GRAY node → cycle
            if color[neighbor] == 0:
                if dfs(neighbor):
                    return True
        color[node] = 2     # BLACK — done
        return False

    for i in range(n):
        if color[i] == 0:
            if dfs(i):
                return True
    return False
```

### Directed Graph — Kahn's BFS

```python
def has_cycle_kahn(n, edges):
    graph = defaultdict(list)
    in_degree = [0] * n

    for u, v in edges:
        graph[u].append(v)
        in_degree[v] += 1

    queue = deque([i for i in range(n) if in_degree[i] == 0])
    processed = 0

    while queue:
        node = queue.popleft()
        processed += 1
        for neighbor in graph[node]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)

    return processed != n   # True → cycle exists
```

### Cycle Detection — Which to Use?

| Scenario | Recommended |
|---|---|
| Undirected, simple | Union-Find |
| Undirected, need cycle path | DFS + parent |
| Directed | DFS 3-color |
| Directed + want topo order | Kahn's BFS |

---

## Topological Sort

### Concept
- Linear ordering of nodes where for every directed edge u→v, **u appears before v**.
- Only valid for **DAGs** (Directed Acyclic Graphs).
- Not unique — multiple valid orderings may exist.

### DFS-based Topological Sort

```python
def topo_sort_dfs(n, edges):
    graph = defaultdict(list)
    for u, v in edges:
        graph[u].append(v)

    visited   = set()
    rec_stack = set()
    order     = []
    has_cycle = [False]

    def dfs(node):
        visited.add(node)
        rec_stack.add(node)

        for neighbor in graph[node]:
            if neighbor not in visited:
                dfs(neighbor)
            elif neighbor in rec_stack:
                has_cycle[0] = True
                return

        rec_stack.discard(node)
        order.append(node)          # add AFTER processing all neighbors

    for i in range(n):
        if i not in visited:
            dfs(i)

    if has_cycle[0]:
        return []
    return order[::-1]              # reverse post-order = topological order
```

### Kahn's Algorithm — BFS-based (most interview-friendly)

```python
# LC 207/210 — Course Schedule pattern
def kahn_topo_sort(n, prerequisites):
    graph     = defaultdict(list)
    in_degree = [0] * n

    for course, prereq in prerequisites:
        graph[prereq].append(course)
        in_degree[course] += 1

    # Start with all nodes that have no prerequisites
    queue = deque([i for i in range(n) if in_degree[i] == 0])
    order = []

    while queue:
        node = queue.popleft()
        order.append(node)

        for neighbor in graph[node]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:    # all prereqs satisfied
                queue.append(neighbor)

    return order if len(order) == n else []

# Example — LC 210
n = 4
prereqs = [(1,0),(2,0),(3,1),(3,2)]
print(kahn_topo_sort(n, prereqs))  # [0, 1, 2, 3] or [0, 2, 1, 3]
```

### Topological Sort with Lexicographic Ordering

```python
import heapq

def topo_sort_lexicographic(n, edges):
    graph     = defaultdict(list)
    in_degree = [0] * n

    for u, v in edges:
        graph[u].append(v)
        in_degree[v] += 1

    heap = [i for i in range(n) if in_degree[i] == 0]
    heapq.heapify(heap)
    order = []

    while heap:
        node = heapq.heappop(heap)
        order.append(node)
        for neighbor in graph[node]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                heapq.heappush(heap, neighbor)

    return order if len(order) == n else []
```

### Key Interview Points
- Kahn's is easier to explain and implement — **prefer it in interviews**.
- DFS-based: append node **after** visiting all descendants, then **reverse**.
- If `len(order) != n` → cycle exists → no valid topological ordering.

---

## Strongly Connected Components (SCC)

### Concept
- SCC = maximal subgraph where **every node can reach every other node**.
- Only meaningful for **directed graphs**.
- Two main algorithms: **Kosaraju's** (2-pass DFS) and **Tarjan's** (1-pass DFS).

### Kosaraju's Algorithm

```python
# Step 1: DFS on original graph, record finish order (post-order)
# Step 2: Transpose graph (reverse all edges)
# Step 3: DFS on transposed graph in reverse finish order

def kosaraju(n, edges):
    graph   = defaultdict(list)
    reverse = defaultdict(list)

    for u, v in edges:
        graph[u].append(v)
        reverse[v].append(u)       # reversed edge

    # Step 1: DFS on original → get finish order
    visited = set()
    finish_order = []

    def dfs1(node):
        visited.add(node)
        for neighbor in graph[node]:
            if neighbor not in visited:
                dfs1(neighbor)
        finish_order.append(node)  # post-order

    for i in range(n):
        if i not in visited:
            dfs1(i)

    # Step 2: DFS on reversed graph in reverse finish order
    visited.clear()
    sccs = []

    def dfs2(node, component):
        visited.add(node)
        component.append(node)
        for neighbor in reverse[node]:
            if neighbor not in visited:
                dfs2(neighbor, component)

    for node in reversed(finish_order):
        if node not in visited:
            component = []
            dfs2(node, component)
            sccs.append(component)

    return sccs

# Example
n = 5
edges = [(0,1),(1,2),(2,0),(1,3),(3,4)]
print(kosaraju(n, edges))
# [[0,2,1], [3], [4]] — 3 SCCs
```

### Tarjan's Algorithm

```python
# Uses discovery time + low-link values + stack
def tarjan_scc(n, edges):
    graph = defaultdict(list)
    for u, v in edges:
        graph[u].append(v)

    disc     = [-1] * n
    low      = [0]  * n
    on_stack = [False] * n
    stack    = []
    timer    = [0]
    sccs     = []

    def dfs(node):
        disc[node] = low[node] = timer[0]
        timer[0] += 1
        stack.append(node)
        on_stack[node] = True

        for neighbor in graph[node]:
            if disc[neighbor] == -1:            # tree edge
                dfs(neighbor)
                low[node] = min(low[node], low[neighbor])
            elif on_stack[neighbor]:            # back edge (in current SCC)
                low[node] = min(low[node], disc[neighbor])

        # Root of SCC — pop all nodes in this SCC
        if low[node] == disc[node]:
            scc = []
            while True:
                w = stack.pop()
                on_stack[w] = False
                scc.append(w)
                if w == node:
                    break
            sccs.append(scc)

    for i in range(n):
        if disc[i] == -1:
            dfs(i)

    return sccs
```

### Kosaraju vs Tarjan

| | Kosaraju | Tarjan |
|---|---|---|
| Passes | 2 DFS passes | 1 DFS pass |
| Space | O(V+E) × 2 graphs | O(V+E) + stack |
| Simplicity | Easier to explain | Harder to implement |
| Time | O(V+E) | O(V+E) |
| Interview pick | ✅ Preferred to explain | Use if asked for optimized |

### Key Interview Points
- **Kosaraju in interviews**: easy 3-step explanation — original DFS → transpose → reverse DFS.
- Condensation graph (DAG of SCCs) = always a DAG, enabling topological sort.
- Tarjan's: node is SCC root when `low[node] == disc[node]`.

---

### Traversal Master Cheat Sheet

```
WHICH TRAVERSAL TO USE?
├── Need shortest path (unweighted)?         → BFS
├── Need path existence / explore all?       → DFS
├── Level-by-level processing?               → BFS (snapshot queue size)
├── Detect cycle in undirected?              → Union-Find or DFS+parent
├── Detect cycle in directed?                → DFS 3-color or Kahn's
├── Task ordering / scheduling?              → Topological Sort (Kahn's)
├── Find all strongly connected groups?      → Kosaraju or Tarjan
└── Dynamic connectivity queries?            → Union-Find

COMMON MISTAKES
├── Forgetting to handle disconnected graphs (loop all nodes)
├── Marking visited on DEQUEUE not ENQUEUE (BFS duplicates)
├── Using plain list.pop(0) instead of deque.popleft()
├── Forgetting parent check in undirected cycle detection
├── Using cycle detection for undirected on directed graphs
└── Not reversing post-order in DFS-based topological sort
```

---

# Shortest Path

---

## Dijkstra's Algorithm

### Concept
- **Single-source shortest path** for graphs with **non-negative weights**.
- Greedy approach — always process the node with the **smallest known distance** first.
- Uses a **min-heap** (priority queue).
- Time: **O((V + E) log V)** | Space: **O(V)**

> **Core Intuition:** "Among all unvisited nodes, pick the closest one. Its distance is now finalized — no shorter path can exist because all weights are non-negative."

### Standard Dijkstra

```python
import heapq
from collections import defaultdict

def dijkstra(graph, start, n):
    dist = [float('inf')] * n
    dist[start] = 0
    min_heap = [(0, start)]     # (distance, node)

    while min_heap:
        d, node = heapq.heappop(min_heap)

        # Stale entry — a shorter path was already found
        if d > dist[node]:
            continue

        for neighbor, weight in graph[node]:
            new_dist = dist[node] + weight
            if new_dist < dist[neighbor]:
                dist[neighbor] = new_dist
                heapq.heappush(min_heap, (new_dist, neighbor))

    return dist     # dist[i] = inf means node i is unreachable

# Build graph and run
graph = defaultdict(list)
edges = [(0,1,4),(0,2,1),(2,1,2),(1,3,1),(2,3,5)]
for u, v, w in edges:
    graph[u].append((v, w))
    graph[v].append((u, w))

print(dijkstra(graph, 0, 4))
# [0, 3, 1, 4]
```

### Dijkstra with Path Reconstruction

```python
def dijkstra_with_path(graph, start, end, n):
    dist = [float('inf')] * n
    dist[start] = 0
    prev = [-1] * n             # predecessor array
    min_heap = [(0, start)]

    while min_heap:
        d, node = heapq.heappop(min_heap)
        if d > dist[node]:
            continue

        if node == end:
            break

        for neighbor, weight in graph[node]:
            new_dist = dist[node] + weight
            if new_dist < dist[neighbor]:
                dist[neighbor] = new_dist
                prev[neighbor] = node
                heapq.heappush(min_heap, (new_dist, neighbor))

    # Reconstruct path by walking back through prev[]
    path = []
    curr = end
    while curr != -1:
        path.append(curr)
        curr = prev[curr]

    return dist[end], path[::-1]    # (cost, path)
```

### Dijkstra on Grid

```python
# LC 1631 — Path with Minimum Effort
def minimum_effort_path(heights):
    rows, cols = len(heights), len(heights[0])
    dist = [[float('inf')] * cols for _ in range(rows)]
    dist[0][0] = 0
    min_heap = [(0, 0, 0)]      # (effort, row, col)
    directions = [(0,1),(0,-1),(1,0),(-1,0)]

    while min_heap:
        effort, r, c = heapq.heappop(min_heap)

        if r == rows-1 and c == cols-1:
            return effort

        if effort > dist[r][c]:
            continue

        for dr, dc in directions:
            nr, nc = r + dr, c + dc
            if 0 <= nr < rows and 0 <= nc < cols:
                new_effort = max(effort, abs(heights[nr][nc] - heights[r][c]))
                if new_effort < dist[nr][nc]:
                    dist[nr][nc] = new_effort
                    heapq.heappush(min_heap, (new_effort, nr, nc))

    return dist[rows-1][cols-1]
```

### Dijkstra with State (advanced)

```python
# LC 787 — Cheapest flights within K stops
def find_cheapest_price(n, flights, src, dst, k):
    graph = defaultdict(list)
    for u, v, w in flights:
        graph[u].append((v, w))

    dist = [[float('inf')] * (k + 2) for _ in range(n)]
    dist[src][k + 1] = 0
    min_heap = [(0, src, k + 1)]    # (cost, node, stops_left)

    while min_heap:
        cost, node, stops = heapq.heappop(min_heap)

        if node == dst:
            return cost
        if stops == 0:
            continue

        if cost > dist[node][stops]:
            continue

        for neighbor, weight in graph[node]:
            new_cost = cost + weight
            if new_cost < dist[neighbor][stops - 1]:
                dist[neighbor][stops - 1] = new_cost
                heapq.heappush(min_heap, (new_cost, neighbor, stops - 1))

    return -1
```

### Key Interview Points
- **Fails with negative weights** — always flag this, suggest Bellman-Ford.
- The `if d > dist[node]: continue` guard handles stale heap entries — crucial.
- For state-space Dijkstra: encode full state in heap tuple `(cost, node, extra_state)`.

---

## Bellman-Ford

### Concept
- **Single-source shortest path** supporting **negative weight edges**.
- Detects **negative weight cycles**.
- Relaxes ALL edges **V-1 times**.
- Time: **O(V × E)** | Space: **O(V)**

> **Core Intuition:** "After k relaxations, dist[v] = shortest path using at most k edges. After V-1 relaxations, all shortest paths are found. If we can still relax on the Vth pass → negative cycle."

### Standard Bellman-Ford

```python
def bellman_ford(n, edges, start):
    # edges = list of (u, v, weight)
    dist = [float('inf')] * n
    dist[start] = 0

    # Relax all edges exactly V-1 times
    for i in range(n - 1):
        updated = False
        for u, v, w in edges:
            if dist[u] != float('inf') and dist[u] + w < dist[v]:
                dist[v] = dist[u] + w
                updated = True
        if not updated:
            break

    # V-th relaxation — check for negative weight cycles
    for u, v, w in edges:
        if dist[u] != float('inf') and dist[u] + w < dist[v]:
            return None     # negative cycle detected

    return dist
```

### SPFA — Shortest Path Faster Algorithm

```python
# Average case O(E), worst case still O(VE)
from collections import deque

def spfa(n, edges_dict, start):
    dist = [float('inf')] * n
    dist[start] = 0
    in_queue = [False] * n
    in_queue[start] = True
    queue = deque([start])
    count = [0] * n

    while queue:
        node = queue.popleft()
        in_queue[node] = False

        for neighbor, weight in edges_dict[node]:
            if dist[node] + weight < dist[neighbor]:
                dist[neighbor] = dist[node] + weight
                if not in_queue[neighbor]:
                    queue.append(neighbor)
                    in_queue[neighbor] = True
                    count[neighbor] += 1
                    if count[neighbor] >= n:    # relaxed n times → negative cycle
                        return None

    return dist
```

### Dijkstra vs Bellman-Ford

| | Dijkstra | Bellman-Ford |
|---|---|---|
| Negative weights | ❌ Fails | ✅ Handles |
| Negative cycles | ❌ Cannot detect | ✅ Detects |
| Time complexity | O((V+E) log V) | O(V·E) |
| Approach | Greedy + heap | DP / relaxation |
| Best for | Non-negative, sparse | Negative weights, dense |

### Key Interview Points
- V-1 iterations because the **longest simple path** in a graph with V nodes has V-1 edges.
- `dist[u] != inf` guard — don't relax from unreachable nodes.

---

## Floyd-Warshall

### Concept
- **All-pairs shortest path** — finds shortest distance between **every pair** of nodes.
- DP approach: `dist[i][j]` = shortest path from i to j using only nodes `{0..k}` as intermediaries.
- Time: **O(V³)** | Space: **O(V²)**

> **Core Intuition:** "For each intermediate node k, check if going through k gives a shorter path from i to j."
> `dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])`

### Standard Floyd-Warshall

```python
def floyd_warshall(n, edges):
    INF = float('inf')
    dist = [[INF] * n for _ in range(n)]

    for i in range(n):
        dist[i][i] = 0

    for u, v, w in edges:
        dist[u][v] = w
        # dist[v][u] = w   # add for undirected

    # Core DP — try every node k as intermediate
    for k in range(n):          # intermediate node — MUST be outermost
        for i in range(n):      # source
            for j in range(n):  # destination
                if dist[i][k] + dist[k][j] < dist[i][j]:
                    dist[i][j] = dist[i][k] + dist[k][j]

    # Check for negative cycles
    for i in range(n):
        if dist[i][i] < 0:
            return None     # negative cycle detected

    return dist
```

### Floyd-Warshall with Path Reconstruction

```python
def floyd_warshall_with_path(n, edges):
    INF = float('inf')
    dist = [[INF] * n for _ in range(n)]
    next_node = [[-1] * n for _ in range(n)]

    for i in range(n):
        dist[i][i] = 0

    for u, v, w in edges:
        dist[u][v] = w
        next_node[u][v] = v

    for k in range(n):
        for i in range(n):
            for j in range(n):
                if dist[i][k] + dist[k][j] < dist[i][j]:
                    dist[i][j] = dist[i][k] + dist[k][j]
                    next_node[i][j] = next_node[i][k]

    def get_path(u, v):
        if dist[u][v] == float('inf'):
            return []
        path = [u]
        while u != v:
            u = next_node[u][v]
            path.append(u)
        return path

    return dist, get_path
```

### Key Interview Points
- Loop order matters: **k must be outermost** — k represents the set of allowed intermediaries.
- Negative cycle: `dist[i][i] < 0` after running.
- Dense graph (E ≈ V²): Floyd-Warshall O(V³) vs V × Dijkstra O(V³ log V) → Floyd wins.

---

## 0-1 BFS

### Concept
- Shortest path when edge weights are **only 0 or 1**.
- Uses a **deque**: weight-0 edges → `appendleft` (front), weight-1 edges → `append` (back).
- Time: **O(V + E)** — no heap needed.

> **Core Intuition:** "A deque acts as a sorted structure when weights are only 0 or 1. Free edges go to the front (process immediately), costly edges go to the back (process later)."

### Standard 0-1 BFS

```python
from collections import deque

def zero_one_bfs(n, edges, start):
    graph = defaultdict(list)
    for u, v, w in edges:
        graph[u].append((v, w))
        graph[v].append((u, w))

    dist = [float('inf')] * n
    dist[start] = 0
    dq = deque([start])

    while dq:
        node = dq.popleft()

        for neighbor, weight in graph[node]:
            new_dist = dist[node] + weight
            if new_dist < dist[neighbor]:
                dist[neighbor] = new_dist
                if weight == 0:
                    dq.appendleft(neighbor)     # free edge → process ASAP
                else:
                    dq.append(neighbor)         # cost edge → process later

    return dist
```

### 0-1 BFS on Grid

```python
# LC 1368 — Minimum Cost to Make at Least One Valid Path in a Grid
def min_cost_grid(grid):
    rows, cols = len(grid), len(grid[0])
    # 1=right, 2=left, 3=down, 4=up
    directions = [(0,1),(0,-1),(1,0),(-1,0)]
    dir_map = {1:(0,1), 2:(0,-1), 3:(1,0), 4:(-1,0)}

    dist = [[float('inf')] * cols for _ in range(rows)]
    dist[0][0] = 0
    dq = deque([(0, 0)])

    while dq:
        r, c = dq.popleft()

        for i, (dr, dc) in enumerate(directions):
            nr, nc = r + dr, c + dc
            if 0 <= nr < rows and 0 <= nc < cols:
                cost = 0 if dir_map[grid[r][c]] == (dr, dc) else 1
                new_dist = dist[r][c] + cost
                if new_dist < dist[nr][nc]:
                    dist[nr][nc] = new_dist
                    if cost == 0:
                        dq.appendleft((nr, nc))
                    else:
                        dq.append((nr, nc))

    return dist[rows-1][cols-1]
```

---

### All Shortest Path Algorithms — Master Comparison

| Algorithm | Weights | Time | Space | Use Case |
|---|---|---|---|---|
| BFS | Unweighted (all = 1) | O(V+E) | O(V) | Unweighted SSSP |
| 0-1 BFS | 0 or 1 only | O(V+E) | O(V) | Binary-cost SSSP |
| Dijkstra | Non-negative | O((V+E)logV) | O(V) | General SSSP |
| Bellman-Ford | Any (no neg cycle) | O(VE) | O(V) | Negative edges |
| Floyd-Warshall | Any (no neg cycle) | O(V³) | O(V²) | All-pairs SP |
| SPFA | Any | O(VE) worst | O(V) | Avg-case BF speedup |

### Decision Tree

```
SSSP or APSP?
│
├── APSP → Floyd-Warshall
│
└── SSSP
      ├── All weights = 1 (unweighted)?      → BFS
      ├── Weights are only 0 or 1?           → 0-1 BFS
      ├── All weights non-negative?           → Dijkstra
      └── Negative weights present?
            ├── No negative CYCLE?            → Bellman-Ford
            └── Negative CYCLE?               → Bellman-Ford (detect & report)

COMMON MISTAKES
├── Using Dijkstra with negative weights → wrong answers silently
├── Forgetting stale entry guard in Dijkstra (d > dist[node])
├── Wrong loop order in Floyd-Warshall (k must be outermost)
├── Not checking dist[u] != inf before relaxing in Bellman-Ford
└── Using append instead of appendleft for 0-weight in 0-1 BFS
```

---

# Advanced Graph

---

## Minimum Spanning Tree (MST)

### Concept
- A **spanning tree** = subgraph connecting all V nodes with exactly **V-1 edges**, no cycles.
- **MST** = spanning tree with **minimum total edge weight**.
- Only exists on **connected, undirected, weighted** graphs.
- MST may not be unique if edge weights are equal.

### Kruskal's Algorithm

```python
# Core idea: greedily pick cheapest edge that doesn't form a cycle
# Sort all edges by weight, add edge if it connects two different components

class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank   = [0] * n
        self.count  = n

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union(self, x, y):
        px, py = self.find(x), self.find(y)
        if px == py:
            return False
        if self.rank[px] < self.rank[py]:
            px, py = py, px
        self.parent[py] = px
        if self.rank[px] == self.rank[py]:
            self.rank[px] += 1
        self.count -= 1
        return True

def kruskal(n, edges):
    # edges = [(weight, u, v)]
    edges.sort()                    # sort by weight O(E log E)
    uf = UnionFind(n)
    mst_cost  = 0
    mst_edges = []

    for w, u, v in edges:
        if uf.union(u, v):
            mst_cost += w
            mst_edges.append((u, v, w))
            if len(mst_edges) == n - 1:
                break               # MST complete

    if len(mst_edges) < n - 1:
        return -1, []               # disconnected graph

    return mst_cost, mst_edges

# Example
n = 4
edges = [(1,0,1),(3,0,2),(4,0,3),(2,1,2),(5,1,3),(6,2,3)]
cost, tree = kruskal(n, edges)
print(cost)     # 7
```

### Prim's Algorithm

```python
import heapq
from collections import defaultdict

def prims(n, edges):
    graph = defaultdict(list)
    for u, v, w in edges:
        graph[u].append((w, v))
        graph[v].append((w, u))

    visited   = set()
    min_heap  = [(0, 0, -1)]    # (weight, node, parent)
    mst_cost  = 0
    mst_edges = []

    while min_heap and len(visited) < n:
        w, node, parent = heapq.heappop(min_heap)

        if node in visited:
            continue

        visited.add(node)
        mst_cost += w
        if parent != -1:
            mst_edges.append((parent, node, w))

        for edge_w, neighbor in graph[node]:
            if neighbor not in visited:
                heapq.heappush(min_heap, (edge_w, neighbor, node))

    if len(visited) < n:
        return -1, []

    return mst_cost, mst_edges
```

### Maximum Spanning Tree

```python
# Trick: negate all weights, run Kruskal's/Prim's, negate result back
def max_spanning_tree(n, edges):
    negated = [(-w, u, v) for w, u, v in edges]
    cost, tree = kruskal(n, negated)
    return -cost, tree
```

### Kruskal vs Prim's

| | Kruskal's | Prim's |
|---|---|---|
| Approach | Edge-based | Node-based |
| Data Structure | Union-Find | Min-heap |
| Time | O(E log E) | O(E log V) |
| Best for | Sparse graphs | Dense graphs |
| Requires | All edges upfront | Adjacency list |

### Key Interview Points
- MST is unique when all edge weights are **distinct**.
- Kruskal: sort first, then union — always mention the sort step.
- Prim's: mark visited **on pop not push** — heap may have stale entries.
- Common problems: LC 1584 (Min Cost to Connect Points), LC 1135, LC 1168.

---

## Union-Find (Disjoint Set Union — DSU)

### Concept
- Tracks which elements belong to the **same component**.
- Two core operations: `find` (which component?) and `union` (merge two components).
- With path compression + union by rank: **O(α(n))** per operation ≈ O(1).

### Full Implementation

```python
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank   = [0] * n       # tree height upper bound
        self.size   = [1] * n       # component size
        self.count  = n             # number of distinct components

    def find(self, x):
        # Path compression — flatten tree during find
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union(self, x, y):
        px, py = self.find(x), self.find(y)
        if px == py:
            return False            # already connected

        # Union by rank
        if self.rank[px] < self.rank[py]:
            px, py = py, px
        self.parent[py] = px
        self.size[px] += self.size[py]
        if self.rank[px] == self.rank[py]:
            self.rank[px] += 1
        self.count -= 1
        return True

    def connected(self, x, y):
        return self.find(x) == self.find(y)

    def component_size(self, x):
        return self.size[self.find(x)]
```

### Weighted Union-Find

```python
# LC 399 — Evaluate Division
class WeightedUnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.weight = [1.0] * n     # weight[x] = ratio x / parent[x]

    def find(self, x):
        if self.parent[x] != x:
            root = self.find(self.parent[x])
            self.weight[x] *= self.weight[self.parent[x]]  # update weight
            self.parent[x]  = root
        return self.parent[x]

    def union(self, x, y, ratio):
        px, py = self.find(x), self.find(y)
        if px == py:
            return
        self.parent[px] = py
        self.weight[px] = ratio * self.weight[y] / self.weight[x]

    def query(self, x, y):
        px, py = self.find(x), self.find(y)
        if px != py:
            return -1.0
        return self.weight[x] / self.weight[y]
```

### Classic Problem Patterns

```python
# Pattern 1: Count connected components
def count_components(n, edges):
    uf = UnionFind(n)
    for u, v in edges:
        uf.union(u, v)
    return uf.count

# Pattern 2: Detect cycle in undirected graph
def has_cycle(n, edges):
    uf = UnionFind(n)
    for u, v in edges:
        if not uf.union(u, v):  # already connected → cycle
            return True
    return False

# Pattern 3: Largest component size
def largest_component(n, edges):
    uf = UnionFind(n)
    for u, v in edges:
        uf.union(u, v)
    return max(uf.component_size(i) for i in range(n))

# Pattern 4: Number of islands with Union-Find (LC 200)
def num_islands_uf(grid):
    if not grid:
        return 0
    rows, cols = len(grid), len(grid[0])
    uf = UnionFind(rows * cols)
    count = 0

    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == '1':
                count += 1
                for dr, dc in [(0,1),(1,0)]:    # only right + down to avoid duplicates
                    nr, nc = r + dr, c + dc
                    if (0 <= nr < rows and 0 <= nc < cols
                            and grid[nr][nc] == '1'):
                        if uf.union(r * cols + c, nr * cols + nc):
                            count -= 1

    return count
```

### Rollback Union-Find

```python
# Used when you need to UNDO unions (offline problems)
class RollbackUnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank   = [0] * n
        self.history = []           # stack of saved states

    def find(self, x):              # NO path compression — needed for rollback
        while self.parent[x] != x:
            x = self.parent[x]
        return x

    def union(self, x, y):
        px, py = self.find(x), self.find(y)
        if px == py:
            self.history.append(None)
            return False
        if self.rank[px] < self.rank[py]:
            px, py = py, px
        self.history.append((py, self.parent[py], px, self.rank[px]))
        self.parent[py] = px
        if self.rank[px] == self.rank[py]:
            self.rank[px] += 1
        return True

    def rollback(self):
        entry = self.history.pop()
        if entry is None:
            return
        py, old_py_parent, px, old_px_rank = entry
        self.parent[py] = old_py_parent
        self.rank[px]   = old_px_rank
```

### Key Interview Points
- **Always use both** path compression + union by rank — never just one.
- `find` returns the **root/representative** of the component.
- Use `size[]` (not `rank[]`) when you need actual component sizes.
- Union-Find **cannot** delete edges — use BFS/DFS or offline algorithms for that.

---

## Bipartite Graph Check

### Concept
- A graph is **bipartite** if nodes can be split into **two sets** (A, B) such that every edge connects A to B.
- Equivalent to: graph is **2-colorable** — no odd-length cycles.

### BFS 2-Coloring

```python
from collections import deque

def is_bipartite_bfs(graph, n):
    color = [-1] * n        # -1 = unvisited, 0 = group A, 1 = group B

    for start in range(n):
        if color[start] != -1:
            continue

        queue = deque([start])
        color[start] = 0

        while queue:
            node = queue.popleft()
            for neighbor in graph[node]:
                if color[neighbor] == -1:
                    color[neighbor] = 1 - color[node]   # flip color
                    queue.append(neighbor)
                elif color[neighbor] == color[node]:
                    return False    # same color → NOT bipartite

    return True

# Square → bipartite
graph = defaultdict(list)
for u, v in [(0,1),(1,2),(2,3),(3,0)]:
    graph[u].append(v)
    graph[v].append(u)
print(is_bipartite_bfs(graph, 4))   # True

# Triangle → NOT bipartite (odd cycle)
graph2 = defaultdict(list)
for u, v in [(0,1),(1,2),(2,0)]:
    graph2[u].append(v)
    graph2[v].append(u)
print(is_bipartite_bfs(graph2, 3))  # False
```

### DFS 2-Coloring

```python
def is_bipartite_dfs(graph, n):
    color = [-1] * n

    def dfs(node, c):
        color[node] = c
        for neighbor in graph[node]:
            if color[neighbor] == -1:
                if not dfs(neighbor, 1 - c):
                    return False
            elif color[neighbor] == c:      # conflict
                return False
        return True

    for i in range(n):
        if color[i] == -1:
            if not dfs(i, 0):
                return False
    return True
```

### Union-Find Bipartite Check

```python
def is_bipartite_uf(graph, n):
    uf = UnionFind(2 * n)       # node i and its "opposite" = node i+n

    for node in range(n):
        for neighbor in graph[node]:
            if uf.connected(node, neighbor):
                return False    # node and neighbor in same group → conflict
            uf.union(node + n, neighbor)
            uf.union(node, neighbor + n)

    return True
```

### Common Problem Patterns

```python
# LC 785 — Is Graph Bipartite?
def is_bipartite_lc(graph):
    n = len(graph)
    color = [-1] * n

    for start in range(n):
        if color[start] != -1:
            continue
        queue = deque([start])
        color[start] = 0
        while queue:
            node = queue.popleft()
            for neighbor in graph[node]:
                if color[neighbor] == -1:
                    color[neighbor] = 1 - color[node]
                    queue.append(neighbor)
                elif color[neighbor] == color[node]:
                    return False
    return True

# LC 886 — Possible Bipartition
def possible_bipartition(n, dislikes):
    graph = defaultdict(list)
    for u, v in dislikes:
        graph[u].append(v)
        graph[v].append(u)

    color = {}

    def dfs(node, c):
        color[node] = c
        for neighbor in graph[node]:
            if neighbor in color:
                if color[neighbor] == c:
                    return False
            else:
                if not dfs(neighbor, 1 - c):
                    return False
        return True

    for person in range(1, n + 1):
        if person not in color:
            if not dfs(person, 0):
                return False
    return True

# Maximum Bipartite Matching — Augmenting paths
def max_bipartite_matching(graph, left_nodes, right_nodes):
    match_left  = {}
    match_right = {}

    def dfs(left, visited):
        for right in graph[left]:
            if right in visited:
                continue
            visited.add(right)
            if right not in match_right or dfs(match_right[right], visited):
                match_left[left]   = right
                match_right[right] = left
                return True
        return False

    count = 0
    for left in left_nodes:
        visited = set()
        if dfs(left, visited):
            count += 1
    return count
```

### Key Interview Points
- Bipartite ↔ **no odd-length cycles** — say this in interviews.
- Always iterate over **all nodes** — graph may be disconnected.
- `1 - color` is the elegant flip: `1-0=1`, `1-1=0` — no if/else needed.
- Directed graphs: check bipartiteness on **underlying undirected graph**.

---

## Graph Coloring

### Concept
- Assign **colors to nodes** such that no two **adjacent nodes share the same color**.
- **Chromatic number χ(G)** = minimum colors needed.
- General graph coloring is **NP-hard** — focus on greedy and specific cases in interviews.

### Greedy Graph Coloring

```python
def greedy_coloring(graph, n):
    color = [-1] * n

    for node in range(n):
        neighbor_colors = set()
        for neighbor in graph[node]:
            if color[neighbor] != -1:
                neighbor_colors.add(color[neighbor])

        # Assign smallest available color
        c = 0
        while c in neighbor_colors:
            c += 1
        color[node] = c

    num_colors = max(color) + 1
    return color, num_colors
```

### Welsh-Powell Algorithm (greedy + degree ordering)

```python
def welsh_powell(graph, n):
    # Sort nodes by degree descending
    nodes = sorted(range(n),
                   key=lambda x: len(graph[x]),
                   reverse=True)
    color = [-1] * n

    for node in nodes:
        neighbor_colors = {color[nb] for nb in graph[node] if color[nb] != -1}
        c = 0
        while c in neighbor_colors:
            c += 1
        color[node] = c

    return color, max(color) + 1
```

### 3-Coloring via Backtracking (NP-hard, exponential)

```python
def can_color(graph, n, k):
    color = [-1] * n

    def is_valid(node, c):
        return all(color[nb] != c for nb in graph[node])

    def backtrack(node):
        if node == n:
            return True
        for c in range(k):
            if is_valid(node, c):
                color[node] = c
                if backtrack(node + 1):
                    return True
                color[node] = -1    # backtrack
        return False

    return backtrack(0), color
```

### Special Cases — Known Chromatic Numbers

```python
# Bipartite graph → χ = 2 (if at least one edge)
def chromatic_bipartite(graph, n):
    return 2 if any(graph[i] for i in range(n)) else 1

# Complete graph Kn → χ = n
def chromatic_complete(n):
    return n

# Cycle graph Cn → χ = 2 if n even, 3 if n odd
def chromatic_cycle(n):
    return 2 if n % 2 == 0 else 3

# Tree → always χ = 2 (trees are bipartite — no odd cycles)
def chromatic_tree():
    return 2

# Planar graph → χ ≤ 4  (Four Color Theorem)
```

### Interview Problem Patterns

```python
# Pattern 1: Minimum exam slots (no conflicting courses in same slot)
def min_exam_slots(n, conflicts):
    graph = defaultdict(list)
    for u, v in conflicts:
        graph[u].append(v)
        graph[v].append(u)
    _, k = welsh_powell(graph, n)
    return k

# Pattern 2: Register allocation (variables live at same time → different registers)
def min_registers(n, interferences):
    graph = defaultdict(list)
    for u, v in interferences:
        graph[u].append(v)
        graph[v].append(u)
    _, k = greedy_coloring(graph, n)
    return k
```

### Edge Coloring

```python
# Color EDGES such that no two adjacent edges (sharing a node) share a color
# Vizing's theorem: χ'(G) = Δ or Δ+1  (Δ = max degree)

def greedy_edge_coloring(n, edges):
    edge_color = {}
    node_used_colors = defaultdict(set)

    for u, v in edges:
        forbidden = node_used_colors[u] | node_used_colors[v]
        c = 0
        while c in forbidden:
            c += 1
        edge_color[(u, v)] = c
        node_used_colors[u].add(c)
        node_used_colors[v].add(c)

    return edge_color
```

---

### Advanced Graph Master Cheat Sheet

```
ALGORITHM SELECTION

Spanning Tree:
├── Sparse graph?                            → Kruskal's (sort + Union-Find)
├── Dense graph?                             → Prim's (min-heap)
└── Maximum spanning tree?                   → Negate weights, run either

Connectivity:
├── Static graph, one-time query?            → BFS/DFS
├── Dynamic edges, repeated queries?         → Union-Find
├── Need rollback (offline)?                 → Rollback DSU (rank only)
└── Weighted relationships?                  → Weighted Union-Find

Bipartite / Coloring:
├── Is graph 2-colorable?                    → BFS/DFS 2-coloring
├── Minimum colors, exact (small graph)?     → Backtracking k-coloring
└── Minimum colors, approximate?             → Welsh-Powell greedy

KEY THEOREMS TO KNOW:
├── Bipartite ↔ no odd cycles ↔ 2-colorable
├── Tree → always bipartite (χ=2)
├── Kn (complete) → χ=n
├── Planar graph → χ ≤ 4  (Four Color Theorem)
├── Vizing's theorem: χ'(G) ∈ {Δ, Δ+1}
└── König's theorem: in bipartite, max matching = min vertex cover

COMMON MISTAKES:
├── MST: Prim's marks visited on POP not push (stale heap entries)
├── Union-Find: forgetting path compression → O(log n) not O(α(n))
├── Bipartite: not handling disconnected graphs (loop all nodes)
├── Coloring: greedy result depends on ordering — not always optimal
└── MST: assuming MST is unique (only unique when all weights distinct)
```

---

# 📚 Interview Notes — Recursion, Combinatorics, Backtracking & Optimization (Python)

---

# 🔁 Recursion Basics

## 📌 What is Recursion?

A function that **calls itself** to solve a smaller subproblem, until it reaches a stopping condition.

**Two mandatory parts:**
- **Base case** → stops the recursion
- **Recursive case** → breaks problem into smaller version of itself

> ⚠️ **Interview tip:** Always mention: *"Without a base case, recursion leads to infinite calls and a `RecursionError: maximum recursion depth exceeded`"*

Python's default recursion limit is **1000** — you can check/change it:

```python
import sys
sys.getrecursionlimit()      # 1000
sys.setrecursionlimit(2000)  # change it
```

---

## 1. 🛑 Base Case / Recursive Case

The **base case** is the simplest input where the answer is known directly — no further recursion needed.
The **recursive case** reduces the problem toward the base case.

```python
def countdown(n):
    if n <= 0:          # base case
        print("Done!")
    else:               # recursive case
        print(n)
        countdown(n - 1)
```

**Call stack visualization for countdown(3):**

```
countdown(3) → prints 3 → calls countdown(2)
  countdown(2) → prints 2 → calls countdown(1)
    countdown(1) → prints 1 → calls countdown(0)
      countdown(0) → prints "Done!" ← base case hit
```

> 🎯 **Interview tip:** Be ready to trace the **call stack** manually. Interviewers love asking *"what happens step by step?"*

---

## 2. ➕ Factorial

`n! = n × (n-1) × (n-2) × ... × 1`, and `0! = 1`

**Recursive definition:**
- Base case: `factorial(0) = 1`
- Recursive case: `factorial(n) = n * factorial(n-1)`

```python
def factorial(n):
    if n < 0:
        raise ValueError("Negative input not allowed")
    if n == 0:          # base case
        return 1
    return n * factorial(n - 1)   # recursive case

print(factorial(5))  # 120
```

**Call stack for factorial(4):**

```
factorial(4) = 4 * factorial(3)
                   3 * factorial(2)
                       2 * factorial(1)
                           1 * factorial(0)
                                   1       ← base case
# Unwinding:
# 1*1=1, 2*1=2, 3*2=6, 4*6=24
```

**Tail-recursive style** (Python doesn't optimize it, but good to mention):

```python
def factorial_tail(n, accumulator=1):
    if n == 0:
        return accumulator
    return factorial_tail(n - 1, n * accumulator)
```

> 🎯 **Interview tips:**
> - Time: **O(n)**, Space: **O(n)** call stack
> - Mention **tail recursion** and that Python doesn't do TCO (Tail Call Optimization)
> - Iterative version is more memory-efficient for large `n`

---

## 3. 🌀 Fibonacci

`fib(n) = fib(n-1) + fib(n-2)`, with `fib(0)=0`, `fib(1)=1`

### Naive Recursive (⚠️ exponential time):

```python
def fib(n):
    if n <= 1:              # base case
        return n
    return fib(n-1) + fib(n-2)   # recursive case

print(fib(6))  # 8
```

**Problem:** `fib(5)` computes `fib(3)` **twice**, `fib(2)` **three times** — exponential overlap.
- Time: **O(2ⁿ)**, Space: **O(n)**

### Memoized (Top-Down DP) — ✅ Preferred in interviews:

```python
def fib_memo(n, memo={}):
    if n <= 1:
        return n
    if n not in memo:
        memo[n] = fib_memo(n-1, memo) + fib_memo(n-2, memo)
    return memo[n]

# Cleaner with lru_cache:
from functools import lru_cache

@lru_cache(maxsize=None)
def fib_cached(n):
    if n <= 1:
        return n
    return fib_cached(n-1) + fib_cached(n-2)
```

- Time: **O(n)**, Space: **O(n)**

### Bottom-Up DP (most optimal, no recursion):

```python
def fib_dp(n):
    if n <= 1:
        return n
    a, b = 0, 1
    for _ in range(2, n+1):
        a, b = b, a + b
    return b
```

- Time: **O(n)**, Space: **O(1)**

> 🎯 **Interview tips:**
> - Start by explaining the naive approach and its flaw (overlapping subproblems)
> - Then propose memoization → show you understand DP
> - Mention `@lru_cache` as a Pythonic solution
> - Know all 3 versions cold — interviewers often ask to optimize iteratively

---

## 4. 🗼 Tower of Hanoi

Move `n` disks from source peg to destination peg using an auxiliary peg.

**Rules:**
1. Only one disk moved at a time
2. A larger disk cannot be placed on a smaller disk
3. Use 3 pegs: `src`, `aux`, `dst`

**Recursive logic:**
1. Move top `n-1` disks from `src → aux` (using `dst`)
2. Move bottom disk from `src → dst`
3. Move `n-1` disks from `aux → dst` (using `src`)

```python
def hanoi(n, src='A', dst='C', aux='B'):
    if n == 1:                          # base case
        print(f"Move disk 1: {src} → {dst}")
        return
    hanoi(n-1, src, aux, dst)           # step 1
    print(f"Move disk {n}: {src} → {dst}")  # step 2
    hanoi(n-1, aux, dst, src)           # step 3

hanoi(3)
# Move disk 1: A → C
# Move disk 2: A → B
# Move disk 1: C → B
# Move disk 3: A → C
# Move disk 1: B → A
# Move disk 2: B → C
# Move disk 1: A → C
```

**Total moves for `n` disks = 2ⁿ − 1**
- 3 disks → 7 moves, 4 disks → 15 moves

> 🎯 **Interview tips:**
> - Time: **O(2ⁿ)**, Space: **O(n)** (call stack depth)
> - The key insight: *"trust the recursion"* — solve `n-1` correctly, then handle `n`
> - Common follow-up: *"how many moves for n disks?"* → `2ⁿ - 1`

---

## 5. ⚡ Power Calculation

`power(base, exp)` = base raised to exp

### Naive — O(n):

```python
def power_naive(base, exp):
    if exp == 0:        # base case
        return 1
    return base * power_naive(base, exp - 1)
```

### Fast Exponentiation (Exponentiation by Squaring) — ✅ O(log n):

```python
def power(base, exp):
    if exp == 0:                        # base case
        return 1
    if exp % 2 == 0:                    # even exponent
        half = power(base, exp // 2)
        return half * half
    else:                               # odd exponent
        return base * power(base, exp - 1)

print(power(2, 10))  # 1024
```

**Trace for power(2, 8):**

```
power(2,8) → half = power(2,4) → half = power(2,2) → half = power(2,1)
  power(2,1) = 2 * power(2,0) = 2*1 = 2
  power(2,2) = 2*2 = 4
  power(2,4) = 4*4 = 16
  power(2,8) = 16*16 = 256
```

> 🎯 **Interview tips:**
> - Naive is O(n), fast version is **O(log n)** — always push for the optimized version
> - Handle negative exponents: `power(base, -exp)` → `1 / power(base, exp)`
> - Python has `pow(base, exp, mod)` built-in for modular exponentiation

---

## 6. 📦 Array Recursion

### Sum of array:

```python
def arr_sum(arr, i=0):
    if i == len(arr):       # base case: past end
        return 0
    return arr[i] + arr_sum(arr, i + 1)

print(arr_sum([1, 2, 3, 4]))  # 10
```

### Maximum element:

```python
def arr_max(arr, i=0):
    if i == len(arr) - 1:   # base case: last element
        return arr[i]
    rest_max = arr_max(arr, i + 1)
    return arr[i] if arr[i] > rest_max else rest_max

print(arr_max([3, 1, 7, 2]))  # 7
```

### Binary Search (recursive):

```python
def binary_search(arr, target, low, high):
    if low > high:          # base case: not found
        return -1
    mid = (low + high) // 2
    if arr[mid] == target:  # base case: found
        return mid
    elif arr[mid] < target:
        return binary_search(arr, target, mid + 1, high)
    else:
        return binary_search(arr, target, low, mid - 1)

arr = [1, 3, 5, 7, 9, 11]
print(binary_search(arr, 7, 0, len(arr)-1))  # 3
```

### Reverse an array in-place:

```python
def reverse_arr(arr, left=0, right=None):
    if right is None:
        right = len(arr) - 1
    if left >= right:       # base case
        return
    arr[left], arr[right] = arr[right], arr[left]
    reverse_arr(arr, left + 1, right - 1)

arr = [1, 2, 3, 4, 5]
reverse_arr(arr)
print(arr)  # [5, 4, 3, 2, 1]
```

> 🎯 **Interview tips:**
> - Always pass index `i` rather than slicing (`arr[1:]`) — slicing creates new arrays → **O(n²) space** vs **O(n)**
> - Binary search: Time **O(log n)**, Space **O(log n)** recursive vs **O(1)** iterative

---

## 7. 🔤 String Recursion

### Reverse a string:

```python
def reverse_str(s):
    if len(s) <= 1:         # base case
        return s
    return reverse_str(s[1:]) + s[0]

print(reverse_str("hello"))  # "olleh"
```

### Palindrome check:

```python
def is_palindrome(s):
    if len(s) <= 1:                     # base case
        return True
    if s[0] != s[-1]:                   # mismatch
        return False
    return is_palindrome(s[1:-1])       # check inner substring

print(is_palindrome("racecar"))  # True
print(is_palindrome("hello"))    # False
```

### Count occurrences of a character:

```python
def count_char(s, ch):
    if not s:               # base case: empty string
        return 0
    return (1 if s[0] == ch else 0) + count_char(s[1:], ch)

print(count_char("mississippi", "s"))  # 4
```

### All permutations of a string:

```python
def permutations(s):
    if len(s) <= 1:         # base case
        return [s]
    result = []
    for i, ch in enumerate(s):
        rest = s[:i] + s[i+1:]          # remaining chars
        for perm in permutations(rest):
            result.append(ch + perm)
    return result

print(permutations("abc"))
# ['abc', 'acb', 'bac', 'bca', 'cab', 'cba']
```

### Check if string contains only digits:

```python
def all_digits(s):
    if not s:               # base case: empty = True
        return True
    if not s[0].isdigit():
        return False
    return all_digits(s[1:])
```

> 🎯 **Interview tips:**
> - String slicing in Python is **O(k)** — mention this for space/time analysis
> - For permutations: Time **O(n × n!)**, there are `n!` permutations each of length `n`
> - Palindrome recursion is a great example of **shrinking both ends** toward the center

---

## ⚡ Recursion: Complexity Cheat Sheet

| Problem | Time | Space |
|---|---|---|
| Factorial | O(n) | O(n) |
| Fibonacci (naive) | O(2ⁿ) | O(n) |
| Fibonacci (memo) | O(n) | O(n) |
| Tower of Hanoi | O(2ⁿ) | O(n) |
| Power (naive) | O(n) | O(n) |
| Power (fast exp) | O(log n) | O(log n) |
| Binary Search | O(log n) | O(log n) |
| Permutations | O(n × n!) | O(n!) |

## 🧠 Recursion: Key Interview Talking Points

- **"Always identify base case first"** — the most common mistake is missing or wrong base case
- **"Recursive calls must progress toward the base case"** — otherwise infinite recursion
- **"Each recursive call gets its own stack frame"** — this is why space is O(depth)
- **"Recursion can always be converted to iteration with an explicit stack"**
- **Memoization = recursion + caching** → bridges recursion and dynamic programming
- When asked to optimize: naive recursion → memoization → bottom-up DP → iterative

---

---

# 🔢 Combinatorics

## 📌 What is Combinatorics in CS Interviews?

Combinatorics problems involve **generating/counting all possible arrangements or selections** from a set. Almost always solved with **backtracking** — a recursive technique that builds candidates incrementally and **abandons (backtracks)** a path as soon as it's invalid.

**The Backtracking Template — memorize this:**

```python
def backtrack(state, choices):
    if is_solution(state):      # base case: valid complete solution
        result.append(state[:]) # add a COPY, not a reference
        return
    for choice in choices:
        state.append(choice)    # make choice
        backtrack(state, next_choices)
        state.pop()             # UNDO choice ← this is the "backtrack"
```

> ⚠️ **Most common interview mistake:** Appending `state` directly instead of `state[:]` or `state.copy()` — you'll get all identical results since lists are mutable references.

---

## 1. 📦 Generate Subsets (Power Set)

Given `nums = [1,2,3]`, return **all possible subsets** including `[]` and the full set.

Total subsets of n elements = **2ⁿ** (each element is either IN or OUT)

### Approach 1 — Backtracking (✅ most interview-friendly):

```python
def subsets(nums):
    result = []

    def backtrack(start, current):
        result.append(current[:])   # every state is a valid subset
        for i in range(start, len(nums)):
            current.append(nums[i])         # include nums[i]
            backtrack(i + 1, current)       # recurse with next elements
            current.pop()                   # exclude nums[i] (backtrack)

    backtrack(0, [])
    return result

print(subsets([1, 2, 3]))
# [[], [1], [1,2], [1,2,3], [1,3], [2], [2,3], [3]]
```

**Decision tree for [1,2,3]:**

```
                    []
          /          |          \
        [1]         [2]         [3]
       /   \          \
    [1,2] [1,3]      [2,3]
      |
   [1,2,3]
```

### Approach 2 — Iterative (bit manipulation):

```python
def subsets_bit(nums):
    n = len(nums)
    result = []
    for mask in range(1 << n):      # 0 to 2^n - 1
        subset = []
        for i in range(n):
            if mask & (1 << i):     # if i-th bit is set
                subset.append(nums[i])
        result.append(subset)
    return result

# mask=0b000 → [], mask=0b001 → [1], mask=0b011 → [1,2], etc.
```

### Subsets II — with duplicates (e.g. `[1,2,2]`):

```python
def subsets_with_dup(nums):
    result = []
    nums.sort()                     # MUST sort first

    def backtrack(start, current):
        result.append(current[:])
        for i in range(start, len(nums)):
            if i > start and nums[i] == nums[i-1]:  # skip duplicate
                continue
            current.append(nums[i])
            backtrack(i + 1, current)
            current.pop()

    backtrack(0, [])
    return result

print(subsets_with_dup([1, 2, 2]))
# [[], [1], [1,2], [1,2,2], [2], [2,2]]
```

> 🎯 **Interview tips:**
> - Time: **O(n × 2ⁿ)** — 2ⁿ subsets, each takes O(n) to copy
> - Space: **O(n)** call stack depth (not counting output)
> - Key insight: *"We add to result at every node, not just leaf nodes"*
> - Duplicate handling: **sort first**, then skip `nums[i] == nums[i-1]` when `i > start`

---

## 2. 🔀 Generate Permutations

Given `nums = [1,2,3]`, return **all orderings** — every element used exactly once.

Total permutations of n elements = **n!**

### Approach 1 — Backtracking with `used[]` array:

```python
def permutations(nums):
    result = []
    used = [False] * len(nums)

    def backtrack(current):
        if len(current) == len(nums):   # base case: full permutation
            result.append(current[:])
            return
        for i in range(len(nums)):
            if used[i]:
                continue
            used[i] = True
            current.append(nums[i])
            backtrack(current)
            current.pop()               # backtrack
            used[i] = False             # backtrack

    backtrack([])
    return result

print(permutations([1, 2, 3]))
# [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

### Approach 2 — Swap-based (in-place):

```python
def permutations_swap(nums):
    result = []

    def backtrack(start):
        if start == len(nums):
            result.append(nums[:])
            return
        for i in range(start, len(nums)):
            nums[start], nums[i] = nums[i], nums[start]     # swap in
            backtrack(start + 1)
            nums[start], nums[i] = nums[i], nums[start]     # swap back

    backtrack(0)
    return result
```

### Permutations II — with duplicates (e.g. `[1,1,2]`):

```python
def perms_unique(nums):
    result = []
    nums.sort()
    used = [False] * len(nums)

    def backtrack(current):
        if len(current) == len(nums):
            result.append(current[:])
            return
        for i in range(len(nums)):
            if used[i]:
                continue
            # Skip duplicate: same value, previous not used in this level
            if i > 0 and nums[i] == nums[i-1] and not used[i-1]:
                continue
            used[i] = True
            current.append(nums[i])
            backtrack(current)
            current.pop()
            used[i] = False

    backtrack([])
    return result

print(perms_unique([1, 1, 2]))
# [[1,1,2],[1,2,1],[2,1,1]]
```

> 🎯 **Interview tips:**
> - Time: **O(n × n!)** — n! permutations, O(n) to copy each
> - Space: **O(n)** for the `used` array + call stack
> - Subsets vs Permutations: subsets use `start` index to avoid reuse, permutations use `used[]`
> - Duplicate pruning condition: `nums[i] == nums[i-1] and not used[i-1]`

---

## 3. 📱 Phone Letter Mapping (Letter Combinations)

Given a string of digits `"23"`, return all letter combinations (T9 keyboard).

```
2→"abc", 3→"def", 4→"ghi", 5→"jkl"
6→"mno", 7→"pqrs", 8→"tuv", 9→"wxyz"
```

```python
def letter_combinations(digits):
    if not digits:
        return []

    phone_map = {
        '2': 'abc', '3': 'def', '4': 'ghi', '5': 'jkl',
        '6': 'mno', '7': 'pqrs', '8': 'tuv', '9': 'wxyz'
    }
    result = []

    def backtrack(index, current):
        if index == len(digits):        # base case: used all digits
            result.append("".join(current))
            return
        for ch in phone_map[digits[index]]:
            current.append(ch)
            backtrack(index + 1, current)
            current.pop()               # backtrack

    backtrack(0, [])
    return result

print(letter_combinations("23"))
# ['ad','ae','af','bd','be','bf','cd','ce','cf']
```

**Decision tree for "23":**

```
            ""
      /      |      \
     a        b        c          ← letters for '2'
   / | \    / | \    / | \
  d  e  f  d  e  f  d  e  f      ← letters for '3'
```

**Count combinations without generating:**

```python
from math import prod

def count_combinations(digits):
    phone_map = {
        '2':3,'3':3,'4':3,'5':3,
        '6':3,'7':4,'8':3,'9':4   # 7,9 have 4 letters
    }
    return prod(phone_map[d] for d in digits)

print(count_combinations("79"))  # 4*4 = 16
```

**Pythonic alternative using itertools:**

```python
from itertools import product

def letter_combinations_iter(digits):
    if not digits: return []
    phone_map = {'2':'abc','3':'def','4':'ghi','5':'jkl',
                 '6':'mno','7':'pqrs','8':'tuv','9':'wxyz'}
    return [''.join(combo) for combo in product(*[phone_map[d] for d in digits])]
```

> 🎯 **Interview tips:**
> - Time: **O(4ⁿ)** worst case, typically **O(3ⁿ)**
> - Space: **O(n)** call stack where n = len(digits)
> - Edge cases: empty string `""` → return `[]`, single digit → return its letters

---

## 4. 🔣 Generate Parentheses

Given `n`, generate all **valid** combinations of `n` pairs of parentheses.

**Key invariant (pruning rules):**
- Can add `(` only if `open < n`
- Can add `)` only if `close < open`

```python
def generate_parentheses(n):
    result = []

    def backtrack(current, open_count, close_count):
        if len(current) == 2 * n:           # base case: full string built
            result.append("".join(current))
            return
        if open_count < n:                  # can add open paren
            current.append('(')
            backtrack(current, open_count + 1, close_count)
            current.pop()
        if close_count < open_count:        # can add close paren
            current.append(')')
            backtrack(current, open_count, close_count + 1)
            current.pop()

    backtrack([], 0, 0)
    return result

print(generate_parentheses(3))
# ['((()))', '(()())', '(())()', '()(())', '()()()']
print(len(generate_parentheses(3)))  # 5 = Catalan number C(3)
```

**Decision tree for n=2:**

```
              ""
              |
             "("           ← open=1, close=0
            /     \
         "(("      "()"    ← open=2 or add ')'
          |          |
        "(()"      "()()"
          |
        "(())"
```

**Verify a parentheses string (bonus):**

```python
def is_valid(s):
    balance = 0
    for ch in s:
        balance += 1 if ch == '(' else -1
        if balance < 0:     # closed before opened
            return False
    return balance == 0

print(is_valid("(()())"))   # True
print(is_valid(")("))       # False
```

> 🎯 **Interview tips:**
> - Total valid combos for n pairs = **Catalan number**: `C(n) = (2n)! / ((n+1)! × n!)`
> - C(1)=1, C(2)=2, C(3)=5, C(4)=14
> - Time: **O(4ⁿ / √n)**, Space: **O(n)** call stack
> - The pruning `close < open` keeps all generated strings valid — **no post-filtering needed**

---

## 5. 🎯 Combination Sum

### Combination Sum I — unlimited reuse:

```python
def combination_sum(candidates, target):
    result = []
    candidates.sort()

    def backtrack(start, current, remaining):
        if remaining == 0:                  # base case: exact match
            result.append(current[:])
            return
        for i in range(start, len(candidates)):
            if candidates[i] > remaining:   # prune: no point continuing
                break
            current.append(candidates[i])
            backtrack(i, current, remaining - candidates[i])  # i not i+1 (reuse allowed)
            current.pop()

    backtrack(0, [], target)
    return result

print(combination_sum([2, 3, 6, 7], 7))
# [[2,2,3],[7]]
```

### Combination Sum II — each number used once, may have duplicates:

```python
def combination_sum2(candidates, target):
    result = []
    candidates.sort()                   # MUST sort for dedup

    def backtrack(start, current, remaining):
        if remaining == 0:
            result.append(current[:])
            return
        for i in range(start, len(candidates)):
            if candidates[i] > remaining:
                break
            if i > start and candidates[i] == candidates[i-1]:  # skip dup
                continue
            current.append(candidates[i])
            backtrack(i + 1, current, remaining - candidates[i])  # i+1 (no reuse)
            current.pop()

    backtrack(0, [], target)
    return result

print(combination_sum2([10,1,2,7,6,1,5], 8))
# [[1,1,6],[1,2,5],[1,7],[2,6]]
```

### Combination Sum III — exactly k numbers from 1–9:

```python
def combination_sum3(k, n):
    """Find all combos of k numbers from 1-9 that sum to n"""
    result = []

    def backtrack(start, current, remaining):
        if len(current) == k and remaining == 0:    # base case
            result.append(current[:])
            return
        if len(current) == k or remaining <= 0:     # prune
            return
        for i in range(start, 10):
            current.append(i)
            backtrack(i + 1, current, remaining - i)
            current.pop()

    backtrack(1, [], n)
    return result

print(combination_sum3(3, 7))   # [[1,2,4]]
print(combination_sum3(3, 9))   # [[1,2,6],[1,3,5],[2,3,4]]
```

**Side-by-side differences:**

| | Reuse | Duplicates in input | Skip condition |
|---|---|---|---|
| **Sum I** | ✅ Yes | ❌ No | `if candidates[i] > remaining: break` |
| **Sum II** | ❌ No | ✅ Yes | `if i > start and nums[i]==nums[i-1]` |
| **Sum III** | ❌ No | ❌ No | `len(current)==k or remaining<=0` |

> 🎯 **Interview tips:**
> - **Reuse allowed → pass `i`** to next call; **no reuse → pass `i+1`**
> - Always **sort first** — enables the `break` pruning when `candidates[i] > remaining`
> - Time: **O(n^(T/M))** where T=target, M=min candidate

---

## ⚡ Combinatorics: Pattern Cheat Sheet

| Problem | Adds to result | Key parameter | Reuse | Dedup trick |
|---|---|---|---|---|
| Subsets | Every node | `start` index | ❌ | sort + skip `nums[i]==nums[i-1]` |
| Permutations | Leaf only | `used[]` array | ❌ | sort + skip if `not used[i-1]` |
| Phone Mapping | Leaf only | digit `index` | N/A | N/A |
| Parentheses | Leaf only | `open`, `close` counts | N/A | pruning rules |
| Combo Sum | Leaf only | `start` + `remaining` | configurable | sort + skip |

## 🧠 Combinatorics: Key Interview Talking Points

- **Backtracking = DFS + Undo** — always explain the undo step explicitly
- **Always copy before appending** — `result.append(current[:])` not `result.append(current)`
- **Sort before deduplicating** — identical elements must be adjacent to skip them
- **Reuse decision = index passed to next call** — `i` means reuse, `i+1` means no reuse
- **Pruning is key** — mention `break` when `candidates[i] > remaining`
- **Counting vs generating** — if they only need the count, dynamic programming is often faster

---

---

# ♟️ Backtracking

## 📌 What is Backtracking?

Backtracking is a **systematic trial-and-error** algorithm that explores all possibilities by building solutions incrementally and **pruning paths** that can't lead to valid solutions.

**Difference from pure recursion:** Backtracking actively *undoes* decisions when a path fails.

**The Universal Backtracking Template:**

```python
def backtrack(state, choices):
    if is_complete(state):          # base case: valid solution found
        result.append(copy(state))
        return True

    for choice in choices:
        if is_valid(state, choice): # prune invalid paths EARLY
            apply(state, choice)    # make move
            if backtrack(state):    # recurse
                return True         # propagate success (if one solution needed)
            undo(state, choice)     # backtrack ← undo the move

    return False                    # no valid choice worked
```

> ⚠️ **Critical interview distinction:**
> - **Find ALL solutions** → never return early, always backtrack fully
> - **Find ONE solution** → return `True` on first success, propagate it up

---

## 1. 👑 N-Queens

Place `n` queens on an `n×n` board so no two queens attack each other.

**Three attack conditions:**
- Same column: `col in cols`
- Same diagonal (↘): `row - col` is constant
- Same anti-diagonal (↙): `row + col` is constant

```python
def solve_n_queens(n):
    result = []
    cols = set()
    diag1 = set()       # row - col
    diag2 = set()       # row + col
    board = [['.' ] * n for _ in range(n)]

    def backtrack(row):
        if row == n:                            # all rows filled
            result.append(["".join(r) for r in board])
            return
        for col in range(n):
            if col in cols or (row-col) in diag1 or (row+col) in diag2:
                continue                        # prune: queen attacks here
            cols.add(col); diag1.add(row - col); diag2.add(row + col)
            board[row][col] = 'Q'
            backtrack(row + 1)
            cols.remove(col); diag1.remove(row - col); diag2.remove(row + col)
            board[row][col] = '.'

    backtrack(0)
    return result
```

**Count-only version (optimized):**

```python
def total_n_queens(n):
    count = [0]
    cols, d1, d2 = set(), set(), set()

    def backtrack(row):
        if row == n:
            count[0] += 1
            return
        for col in range(n):
            if col in cols or (row-col) in d1 or (row+col) in d2:
                continue
            cols.add(col); d1.add(row-col); d2.add(row+col)
            backtrack(row + 1)
            cols.remove(col); d1.remove(row-col); d2.remove(row+col)

    backtrack(0)
    return count[0]

print(total_n_queens(4))   # 2
print(total_n_queens(8))   # 92
```

**Diagonal sets explanation:**

```
Board (4x4):          row-col values:    row+col values:
. . . .               -0-1-2-3          0 1 2 3
. . . .               0 -1-2            1 2 3 4
. . . .               1  0 -1           2 3 4 5
. . . .               2  1  0           3 4 5 6
```

> 🎯 **Interview tips:**
> - Time: **O(n!)**, Space: **O(n)** for sets + call stack
> - Known solutions: n=1→1, n=4→2, n=8→92
> - Using sets for O(1) lookup — mention vs O(n) linear scan

---

## 2. 🔢 Sudoku Solver

Fill a 9×9 grid so every row, column, and 3×3 box contains digits 1–9 exactly once.

```python
def solve_sudoku(board):
    rows = [set() for _ in range(9)]
    cols = [set() for _ in range(9)]
    boxes = [[set() for _ in range(3)] for _ in range(3)]
    empty = []

    for r in range(9):
        for c in range(9):
            if board[r][c] != '.':
                d = board[r][c]
                rows[r].add(d); cols[c].add(d); boxes[r//3][c//3].add(d)
            else:
                empty.append((r, c))

    def backtrack(idx):
        if idx == len(empty):       # all empty cells filled
            return True
        r, c = empty[idx]
        for d in '123456789':
            if d in rows[r] or d in cols[c] or d in boxes[r//3][c//3]:
                continue            # prune
            rows[r].add(d); cols[c].add(d); boxes[r//3][c//3].add(d)
            board[r][c] = d
            if backtrack(idx + 1):
                return True
            rows[r].remove(d); cols[c].remove(d); boxes[r//3][c//3].remove(d)
            board[r][c] = '.'
        return False

    backtrack(0)
    return board
```

**Box index visualization:**

```
(0,0)(0,1)(0,2) | (0,3)(0,4)(0,5) | (0,6)(0,7)(0,8)
  box[0][0]     |    box[0][1]     |    box[0][2]
-------------------------------------------------------
  box[1][0]     |    box[1][1]     |    box[1][2]
-------------------------------------------------------
  box[2][0]     |    box[2][1]     |    box[2][2]

Key: box index = (row//3, col//3)
```

**MRV heuristic (Most Constrained Cell First):**

```python
def find_best_empty(board, rows, cols, boxes):
    """Pick empty cell with fewest valid candidates"""
    best, best_pos = 10, None
    for r in range(9):
        for c in range(9):
            if board[r][c] == '.':
                count = sum(
                    1 for d in '123456789'
                    if d not in rows[r] and d not in cols[c]
                    and d not in boxes[r//3][c//3]
                )
                if count < best:
                    best, best_pos = count, (r, c)
    return best_pos
```

> 🎯 **Interview tips:**
> - Time: Worst case **O(9^m)** where m = number of empty cells
> - **MRV heuristic**: fill cells with fewest options first — dramatically reduces search space
> - Box index formula `r//3, c//3` — be ready to derive this on a whiteboard

---

## 3. 🔤 Word Search

Given a 2D grid and a word, return `True` if the word exists (no cell reused).

```python
def word_search(board, word):
    rows, cols = len(board), len(board[0])
    directions = [(0,1),(0,-1),(1,0),(-1,0)]

    def backtrack(r, c, idx):
        if idx == len(word):            # base case: matched all chars
            return True
        if r < 0 or r >= rows or c < 0 or c >= cols:
            return False
        if board[r][c] != word[idx]:
            return False

        temp = board[r][c]
        board[r][c] = '#'               # mark visited (in-place)

        for dr, dc in directions:
            if backtrack(r+dr, c+dc, idx+1):
                board[r][c] = temp
                return True

        board[r][c] = temp              # restore (backtrack)
        return False

    for r in range(rows):
        for c in range(cols):
            if backtrack(r, c, 0):
                return True
    return False

board = [
    ['A','B','C','E'],
    ['S','F','C','S'],
    ['A','D','E','E']
]
print(word_search(board, "ABCCED"))   # True
print(word_search(board, "ABCB"))     # False
```

**Word Search II — find all words using Trie:**

```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.word = None

def find_words(board, words):
    root = TrieNode()
    for w in words:
        node = root
        for ch in w:
            node = node.children.setdefault(ch, TrieNode())
        node.word = w

    rows, cols = len(board), len(board[0])
    result = []

    def backtrack(r, c, node):
        ch = board[r][c]
        next_node = node.children.get(ch)
        if not next_node:
            return
        if next_node.word:
            result.append(next_node.word)
            next_node.word = None       # avoid duplicates

        board[r][c] = '#'
        for dr, dc in [(0,1),(0,-1),(1,0),(-1,0)]:
            nr, nc = r+dr, c+dc
            if 0 <= nr < rows and 0 <= nc < cols and board[nr][nc] != '#':
                backtrack(nr, nc, next_node)
        board[r][c] = ch

    for r in range(rows):
        for c in range(cols):
            backtrack(r, c, root)
    return result
```

> 🎯 **Interview tips:**
> - Time: **O(m × n × 4 × 3^(L-1))** where L = word length
> - **In-place marking** with `'#'` avoids extra `visited` array
> - Word Search II with Trie avoids redundant search per word

---

## 4. 🐀 Rat in a Maze

Find all paths from `(0,0)` to `(n-1,n-1)` — `1` = open, `0` = blocked.

```python
def rat_maze_directions(maze):
    n = len(maze)
    result = []
    visited = [[False]*n for _ in range(n)]
    dirs = {'D':(1,0), 'U':(-1,0), 'R':(0,1), 'L':(0,-1)}

    def backtrack(r, c, path_str):
        if r == n-1 and c == n-1:
            result.append(path_str)
            return
        for direction, (dr, dc) in dirs.items():
            nr, nc = r+dr, c+dc
            if 0<=nr<n and 0<=nc<n and maze[nr][nc]==1 and not visited[nr][nc]:
                visited[nr][nc] = True
                backtrack(nr, nc, path_str + direction)
                visited[nr][nc] = False     # backtrack

    visited[0][0] = True
    if maze[0][0] == 1:
        backtrack(0, 0, "")
    return result

maze = [
    [1, 0, 0, 0],
    [1, 1, 0, 1],
    [0, 1, 0, 0],
    [0, 1, 1, 1]
]
print(rat_maze_directions(maze))
```

> 🎯 **Interview tips:**
> - Time: **O(4^(n²))** worst case
> - Space: **O(n²)** for visited matrix
> - **Minimum path length** → use BFS instead (BFS guarantees shortest path)

---

## 5. ♞ Knight's Tour

Move a chess knight across an `n×n` board, visiting every cell exactly once.

```python
def knights_tour(n):
    board = [[-1]*n for _ in range(n)]
    moves = [(2,1),(2,-1),(-2,1),(-2,-1),(1,2),(1,-2),(-1,2),(-1,-2)]

    def is_valid(r, c):
        return 0 <= r < n and 0 <= c < n and board[r][c] == -1

    def backtrack(r, c, move_num):
        if move_num == n*n:
            return True
        for dr, dc in moves:
            nr, nc = r+dr, c+dc
            if is_valid(nr, nc):
                board[nr][nc] = move_num
                if backtrack(nr, nc, move_num+1):
                    return True
                board[nr][nc] = -1      # backtrack
        return False

    board[0][0] = 0
    if backtrack(0, 0, 1):
        return board
    return None
```

**Warnsdorff's Heuristic — always move to most constrained cell:**

```python
def knights_tour_warnsdorff(n):
    board = [[-1]*n for _ in range(n)]
    moves = [(2,1),(2,-1),(-2,1),(-2,-1),(1,2),(1,-2),(-1,2),(-1,-2)]

    def is_valid(r, c):
        return 0 <= r < n and 0 <= c < n and board[r][c] == -1

    def degree(r, c):
        return sum(1 for dr,dc in moves if is_valid(r+dr, c+dc))

    def backtrack(r, c, move_num):
        if move_num == n*n:
            return True
        neighbors = [(r+dr, c+dc) for dr,dc in moves if is_valid(r+dr, c+dc)]
        neighbors.sort(key=lambda pos: degree(pos[0], pos[1]))  # Warnsdorff sort

        for nr, nc in neighbors:
            board[nr][nc] = move_num
            if backtrack(nr, nc, move_num+1):
                return True
            board[nr][nc] = -1

        return False

    board[0][0] = 0
    return backtrack(0, 0, 1)
```

**Knight move visualization:**

```
  . X . X .
  X . . . X
  . . K . .     K = Knight at center
  X . . . X     X = valid move destinations
  . X . X .
```

> 🎯 **Interview tips:**
> - Naive time: **O(8^(n²))** — impractical without heuristics
> - With Warnsdorff: nearly **O(n²)** in practice
> - Interviewers usually want the approach + Warnsdorff explanation, not full code

---

## 6. 🎯 Subset Sum

### Variant 1 — Does any subset exist? (Decision):

```python
def subset_sum_exists(nums, target):
    def backtrack(idx, remaining):
        if remaining == 0:
            return True
        if idx == len(nums) or remaining < 0:
            return False
        if backtrack(idx+1, remaining - nums[idx]):
            return True
        return backtrack(idx+1, remaining)

    return backtrack(0, target)

print(subset_sum_exists([3,1,5,9,12], 8))   # True  (3+5)
```

### Variant 2 — Find ALL subsets that sum to target:

```python
def subset_sum_all(nums, target):
    result = []
    nums.sort()

    def backtrack(start, current, remaining):
        if remaining == 0:
            result.append(current[:])
            return
        for i in range(start, len(nums)):
            if nums[i] > remaining:
                break
            current.append(nums[i])
            backtrack(i+1, current, remaining - nums[i])
            current.pop()

    backtrack(0, [], target)
    return result

print(subset_sum_all([1,2,3,4,5], 5))
# [[1,4],[2,3],[5]]
```

### Variant 3 — DP Memoization (Top-Down):

```python
def subset_sum_memo(nums, target):
    from functools import lru_cache

    @lru_cache(maxsize=None)
    def dp(idx, remaining):
        if remaining == 0:
            return True
        if idx == len(nums) or remaining < 0:
            return False
        return dp(idx+1, remaining - nums[idx]) or dp(idx+1, remaining)

    return dp(0, target)
```

### Variant 4 — Classic DP (Bottom-Up):

```python
def subset_sum_dp(nums, target):
    n = len(nums)
    dp = [[False]*(target+1) for _ in range(n+1)]
    for i in range(n+1):
        dp[i][0] = True     # empty subset always sums to 0

    for i in range(1, n+1):
        for j in range(target+1):
            dp[i][j] = dp[i-1][j]
            if j >= nums[i-1]:
                dp[i][j] = dp[i][j] or dp[i-1][j-nums[i-1]]

    return dp[n][target]

# Space-optimized O(target):
def subset_sum_dp_opt(nums, target):
    dp = [False] * (target+1)
    dp[0] = True
    for num in nums:
        for j in range(target, num-1, -1):  # iterate backwards!
            dp[j] = dp[j] or dp[j-num]
    return dp[target]
```

**Why iterate backwards in optimized DP:**

```
Forward: uses updated values from current row → each item used multiple times (unbounded knapsack)
Backward: uses values from previous row     → each item used at most once (0/1 knapsack)
```

> 🎯 **Interview tips:**
> - Backtracking finds actual subsets; DP answers yes/no faster
> - **Space-optimized DP is the gold standard** for decision/count version
> - Backward iteration in 1D DP is a key insight interviewers love to ask about
> - Subset Sum ↔ **0/1 Knapsack**: same structure, different objectives

---

## ⚡ Backtracking: Complexity Cheat Sheet

| Problem | Time | Space | Key Pruning |
|---|---|---|---|
| N-Queens | O(n!) | O(n) | col/diag sets for O(1) check |
| Sudoku | O(9^m) | O(m) | constraint sets per row/col/box |
| Word Search | O(mn × 3^L) | O(L) | in-place `#` marking |
| Rat in Maze | O(4^(n²)) | O(n²) | bounds + visited check |
| Knight's Tour | O(8^(n²)) | O(n²) | Warnsdorff heuristic |
| Subset Sum (BT) | O(2^n) | O(n) | sort + break early |
| Subset Sum (DP) | O(n×T) | O(T) | 1D DP, reverse iteration |

## 🧠 Backtracking: Key Interview Talking Points

- **"Backtracking = DFS + pruning + undo"** — always name all three components
- **Pruning separates a good answer from a great one** — name what you're pruning and why
- **In-place state modification** avoids extra memory — always mention the tradeoff
- **Return `True` immediately on first solution** when only one is needed
- **Heuristics turn exponential into practical**: Warnsdorff for Knight's Tour, MRV for Sudoku
- **Backtracking vs DP**: need actual solution path → backtracking; count/existence → DP

---

---

# ⚡ Optimization — Memoization & Pruning

## 📌 Why Optimization Matters in Interviews

Raw recursive/backtracking solutions are often **correct but too slow**. Interviewers expect you to recognize inefficiency and apply targeted fixes.

> 🎯 **Interview meta-tip:** Always code the naive solution first, state its complexity, *then* optimize.

---

## 1. 🧠 Memoization

### What it is

Cache the result of a function call the first time it's computed, return the cached value on subsequent calls.

**Core idea:** If `f(x)` was already computed, look it up in O(1).

### The Memoization Template:

```python
# Manual memo dict
def f(n, memo=None):
    if memo is None: memo = {}
    if n in memo:
        return memo[n]          # cache hit
    if n == 0:                  # base case
        return 0
    memo[n] = f(n-1, memo) + f(n-2, memo)
    return memo[n]

# Pythonic: @lru_cache (preferred in interviews)
from functools import lru_cache

@lru_cache(maxsize=None)
def f(n):
    if n <= 1:
        return n
    return f(n-1) + f(n-2)
```

> ⚠️ **`memo={}` as default arg is a Python gotcha** — mutable default args are shared across calls. Use `memo=None` + `if memo is None: memo = {}` for production code.

---

### 📊 Problem 1: Fibonacci

**Redundant call tree for fib(6):**

```
                    fib(6)
                /          \
           fib(5)           fib(4)
          /      \         /      \
       fib(4)  fib(3)  fib(3)  fib(2)
       /    \
    fib(3) fib(2)
    ← fib(4) computed TWICE, fib(3) THREE times
```

**Memoized — O(n):**

```python
from functools import lru_cache

@lru_cache(maxsize=None)
def fib(n):
    if n <= 1: return n
    return fib(n-1) + fib(n-2)

# Each unique n computed exactly once → n+1 unique calls total
```

---

### 📊 Problem 2: Longest Common Subsequence (LCS)

**Memoized — O(m×n):**

```python
from functools import lru_cache

def lcs(s1, s2):
    @lru_cache(maxsize=None)
    def dp(i, j):
        if i == len(s1) or j == len(s2):
            return 0
        if s1[i] == s2[j]:
            return 1 + dp(i+1, j+1)
        return max(dp(i+1, j), dp(i, j+1))
    return dp(0, 0)

print(lcs("abcde", "ace"))   # 3  (a,c,e)
print(lcs("abc", "def"))     # 0
```

**State space visualization for "ace" vs "abcde":**

```
    ""  a  b  c  d  e
""   0  0  0  0  0  0
a    0  1  1  1  1  1
c    0  1  1  2  2  2
e    0  1  1  2  2  3   ← answer at dp(m,n)

Each cell computed once → O(m×n) total
```

---

### 📊 Problem 3: 0/1 Knapsack

```python
from functools import lru_cache

def knapsack(weights, values, capacity):
    n = len(weights)

    @lru_cache(maxsize=None)
    def dp(idx, remaining_cap):
        if idx == n or remaining_cap == 0:
            return 0
        if weights[idx] > remaining_cap:
            return dp(idx+1, remaining_cap)
        return max(
            dp(idx+1, remaining_cap),                               # skip
            values[idx] + dp(idx+1, remaining_cap - weights[idx])  # take
        )

    return dp(0, capacity)

weights = [1, 3, 4, 5]
values  = [1, 4, 5, 7]
print(knapsack(weights, values, 7))     # 9
```

**Cache analysis:**

```
Without memo: O(2^n) — many states recomputed
With memo:    O(n × W) — each (idx, cap) pair computed once
```

---

### 📊 Problem 4: Word Break

```python
from functools import lru_cache

def word_break(s, word_dict):
    word_set = set(word_dict)

    @lru_cache(maxsize=None)
    def dp(start):
        if start == len(s):
            return True
        for end in range(start+1, len(s)+1):
            if s[start:end] in word_set and dp(end):
                return True
        return False

    return dp(0)

print(word_break("leetcode", ["leet","code"]))          # True
print(word_break("catsandog", ["cats","dog","sand"]))   # False
```

---

### 📊 Problem 5: Coin Change (Min Coins)

```python
from functools import lru_cache

def coin_change(coins, amount):
    @lru_cache(maxsize=None)
    def dp(remaining):
        if remaining == 0: return 0
        if remaining < 0: return float('inf')

        min_coins = float('inf')
        for coin in coins:
            result = dp(remaining - coin)
            if result != float('inf'):
                min_coins = min(min_coins, 1 + result)
        return min_coins

    result = dp(amount)
    return result if result != float('inf') else -1

print(coin_change([1,5,6,9], 11))   # 2  (5+6)
print(coin_change([2], 3))          # -1 (impossible)
```

---

### 🔑 Memoization: Key Patterns & Pitfalls

**Two conditions for memoization to apply:**

```
1. OVERLAPPING SUBPROBLEMS — same subproblem solved multiple times
2. OPTIMAL SUBSTRUCTURE — solution built from optimal subsolutions
```

**Cache key rules:**

```python
# Single parameter → int key
@lru_cache(maxsize=None)
def dp(n): ...

# Multiple parameters → tuple key (lru_cache handles automatically)
@lru_cache(maxsize=None)
def dp(i, j, remaining): ...

# List/set → must convert to hashable tuple
@lru_cache(maxsize=None)
def dp(i, visited_tuple): ...      # tuple, not list

# Manual dict with tuple key
memo = {}
def dp(i, j):
    if (i,j) in memo: return memo[(i,j)]
    ...
    memo[(i,j)] = result
    return result
```

**`lru_cache` — what to know for interviews:**

```python
from functools import lru_cache, cache

@lru_cache(maxsize=None)    # unlimited — use for interviews
@lru_cache(maxsize=128)     # bounded LRU — use for memory constraints
@cache                      # Python 3.9+ shorthand for lru_cache(maxsize=None)

# Inspect cache
print(dp.cache_info())      # CacheInfo(hits=..., misses=..., currsize=...)

# Clear between test cases
dp.cache_clear()
```

---

## 2. ✂️ Pruning Techniques

### What it is

Pruning **eliminates branches of the search tree** before fully exploring them.

**Types:**
- **Feasibility pruning** — branch violates a hard constraint (can never be valid)
- **Optimality pruning** — branch cannot improve on the best solution found so far
- **Symmetry pruning** — branch is equivalent to one already explored

---

### 🌿 Technique 1: Constraint-Based Pruning (Feasibility)

```python
def n_queens(n):
    result = []
    cols, d1, d2 = set(), set(), set()

    def backtrack(row):
        if row == n:
            result.append(list(cols))
            return
        for col in range(n):
            # PRUNE: check all 3 attack conditions before recursing
            if col in cols or (row-col) in d1 or (row+col) in d2:
                continue                # ← prune entire subtree
            cols.add(col); d1.add(row-col); d2.add(row+col)
            backtrack(row + 1)
            cols.remove(col); d1.remove(row-col); d2.remove(row+col)

    backtrack(0)
    return result
```

---

### 🌿 Technique 2: Bound-Based Pruning (Optimality)

Track best solution so far — prune branches that can't beat it.

```python
def knapsack_bnb(weights, values, capacity):
    n = len(weights)
    items = sorted(zip(values, weights), key=lambda x: x[0]/x[1], reverse=True)
    best = [0]

    def upper_bound(idx, current_val, current_weight):
        val = current_val
        cap = capacity - current_weight
        for i in range(idx, n):
            v, w = items[i]
            if w <= cap:
                val += v; cap -= w
            else:
                val += v * (cap / w)    # fractional
                break
        return val

    def backtrack(idx, current_val, current_weight):
        if current_weight > capacity:
            return
        if current_val > best[0]:
            best[0] = current_val
        if idx == n:
            return
        # PRUNE: upper bound can't beat current best
        if upper_bound(idx, current_val, current_weight) <= best[0]:
            return
        v, w = items[idx]
        backtrack(idx+1, current_val+v, current_weight+w)  # take
        backtrack(idx+1, current_val, current_weight)      # skip

    backtrack(0, 0, 0)
    return best[0]
```

---

### 🌿 Technique 3: Sorted Input + Early Break

```python
def combination_sum(candidates, target):
    result = []
    candidates.sort()               # ← CRITICAL: enables early break

    def backtrack(start, current, remaining):
        if remaining == 0:
            result.append(current[:])
            return
        for i in range(start, len(candidates)):
            if candidates[i] > remaining:
                break               # ← all subsequent also too big (sorted)
            current.append(candidates[i])
            backtrack(i, current, remaining - candidates[i])
            current.pop()

    backtrack(0, [], target)
    return result
```

---

### 🌿 Technique 4: Duplicate Skipping

```python
def subsets_no_dup(nums):
    result = []
    nums.sort()

    def backtrack(start, current):
        result.append(current[:])
        for i in range(start, len(nums)):
            # PRUNE: skip duplicate at same level
            if i > start and nums[i] == nums[i-1]:
                continue
            current.append(nums[i])
            backtrack(i+1, current)
            current.pop()

    backtrack(0, [])
    return result

# WHY i > start and not i > 0:
# i > start: skip second '2' only when '2' already explored at THIS level ✓
# i > 0:     would skip even when '2' is first occurrence at this level   ✗
```

---

### 🌿 Technique 5: Visited / Path Tracking

```python
def word_search(board, word):
    rows, cols = len(board), len(board[0])

    def backtrack(r, c, idx):
        if idx == len(word): return True
        if not (0<=r<rows and 0<=c<cols): return False
        if board[r][c] != word[idx]: return False

        temp = board[r][c]
        board[r][c] = '#'           # mark visited IN THIS PATH

        found = any(
            backtrack(r+dr, c+dc, idx+1)
            for dr,dc in [(0,1),(0,-1),(1,0),(-1,0)]
        )

        board[r][c] = temp          # unmark on backtrack
        return found

    return any(backtrack(r,c,0) for r in range(rows) for c in range(cols))
```

---

### 🌿 Technique 6: Forward Checking (Constraint Propagation)

Proactively check that remaining choices are still viable after each assignment.

```python
def sudoku_with_forward_check(board):
    rows = [set() for _ in range(9)]
    cols = [set() for _ in range(9)]
    boxes = [[set() for _ in range(3)] for _ in range(3)]
    empty = []

    for r in range(9):
        for c in range(9):
            if board[r][c] != '.':
                d = board[r][c]
                rows[r].add(d); cols[c].add(d); boxes[r//3][c//3].add(d)
            else:
                empty.append((r,c))

    def get_candidates(r, c):
        used = rows[r] | cols[c] | boxes[r//3][c//3]
        return set('123456789') - used  # only valid digits

    def backtrack(idx):
        if idx == len(empty): return True
        r, c = empty[idx]
        for d in get_candidates(r, c):  # only try valid candidates
            rows[r].add(d); cols[c].add(d); boxes[r//3][c//3].add(d)
            board[r][c] = d
            if backtrack(idx+1): return True
            rows[r].remove(d); cols[c].remove(d); boxes[r//3][c//3].remove(d)
            board[r][c] = '.'
        return False

    backtrack(0)
    return board
```

---

### 🌿 Technique 7: Memoization + Pruning Combined

```python
from functools import lru_cache

def count_paths_obstacles(grid):
    rows, cols = len(grid), len(grid[0])

    @lru_cache(maxsize=None)
    def dp(r, c):
        # PRUNE: out of bounds or obstacle
        if r >= rows or c >= cols or grid[r][c] == 1:
            return 0
        # BASE CASE
        if r == rows-1 and c == cols-1:
            return 1
        # MEMO: result cached after first computation
        return dp(r+1, c) + dp(r, c+1)

    return dp(0, 0)

grid = [
    [0, 0, 0],
    [0, 1, 0],
    [0, 0, 0]
]
print(count_paths_obstacles(grid))  # 2
```

---

### 🌿 Technique 8: Iterative Deepening (IDDFS)

Combines DFS memory efficiency with BFS's optimal depth-first exploration.

```python
def iddfs(root, target):
    def dfs_limited(node, target, depth_limit, current_depth):
        if node == target:
            return current_depth
        if current_depth == depth_limit:    # PRUNE: depth exceeded
            return None
        for neighbor in get_neighbors(node):
            result = dfs_limited(neighbor, target, depth_limit, current_depth+1)
            if result is not None:
                return result
        return None

    for max_depth in range(1, 100):
        result = dfs_limited(root, target, max_depth, 0)
        if result is not None:
            return result
    return None

# Memory: O(depth) — much better than BFS's O(branching^depth)
# Time:   O(b^d) where b=branching factor, d=solution depth
```

---

## ⚡ Optimization: Which Technique for Which Problem

| Scenario | Technique | Mechanism |
|---|---|---|
| Same subproblem called repeatedly | Memoization | Cache `(args) → result` |
| Remaining elements too large | Sort + Early Break | `if val > limit: break` |
| Duplicate results from equal elements | Duplicate Skipping | `if i>start and nums[i]==nums[i-1]` |
| Invalid state (wrong char, OOB) | Constraint Pruning | Check before recursing |
| Can't beat current best answer | Bound Pruning | `if bound <= best: return` |
| Can't reuse current path cell | Visited Marking | Mark `'#'`, unmark on backtrack |
| Need only valid candidates | Forward Checking | Compute valid set before loop |
| Unknown solution depth | IDDFS | Increase depth limit iteratively |

## 🧠 Optimization: Key Interview Talking Points

- **Memoization requires two conditions:** overlapping subproblems + optimal substructure
- **Cache key must be hashable** — convert lists/sets to tuples
- **Pruning acts at the call site** — the check happens *before* the recursive call
- **Sort enables break** — O(n log n) cost, but exponential savings from pruning
- **`i > start` not `i > 0`** — the duplicate skip condition is a common interview error
- **Memo + Prune compound** — pruning reduces branches, memoization eliminates recomputation
- **Always state the complexity improvement** — *"naive O(2ⁿ) becomes O(n²) with memoization because there are only n² unique subproblems"*



# 🧠 DSA Interview Notes — Python

> Topics: Searching · Sorting · Two Pointer Techniques

---

# 🔍 Searching

---

## 1. Linear Search

### Concept
- Traverse every element one by one until the target is found or the array ends.
- **Time:** O(n) | **Space:** O(1)
- Use when: array is unsorted, small datasets, or you need the first occurrence.

### Interview Points
- Simplest search — always mention it as a baseline comparison.
- Works on **any** data structure (linked list, array, string).
- Returns `-1` (or `None`) by convention when not found.

```python
def linear_search(arr, target):
    for i, val in enumerate(arr):
        if val == target:
            return i
    return -1

# Example
print(linear_search([4, 2, 7, 1, 9], 7))  # → 2
print(linear_search([4, 2, 7, 1, 9], 5))  # → -1
```

---

## 2. Binary Search

### Concept
- Repeatedly halve the search space on a **sorted** array.
- **Time:** O(log n) | **Space:** O(1) iterative, O(log n) recursive

### Interview Points
- **CRITICAL:** Array must be sorted. Say this immediately in the interview.
- Three templates interviewers love to see:
  - `left <= right` (standard, finds exact match)
  - `left < right` (used for boundary/first-last occurrence variants)
- Always be careful with **integer overflow**: use `mid = left + (right - left) // 2` (same in Python, but good habit).
- Know both iterative and recursive.

```python
# Iterative (preferred in interviews)
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = left + (right - left) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1

# Recursive
def binary_search_rec(arr, target, left=0, right=None):
    if right is None:
        right = len(arr) - 1
    if left > right:
        return -1
    mid = left + (right - left) // 2
    if arr[mid] == target:
        return mid
    elif arr[mid] < target:
        return binary_search_rec(arr, target, mid + 1, right)
    else:
        return binary_search_rec(arr, target, left, mid - 1)

# Using Python's bisect
import bisect
arr = [1, 3, 5, 7, 9]
idx = bisect.bisect_left(arr, 5)  # → 2
```

---

## 3. Rotated Array Search

### Concept
- A sorted array has been rotated at some pivot. Find a target.
- **Time:** O(log n) | **Space:** O(1)
- Key insight: **one half is always sorted** after a rotation.

### Interview Points
- Two main scenarios: no duplicates (clean solution), duplicates (worst case O(n)).
- The core trick: check which half is **monotonically sorted**, then determine if target lies in that half.
- Common follow-up: "Find the pivot/minimum element."

```python
def search_rotated(arr, target):
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = left + (right - left) // 2
        if arr[mid] == target:
            return mid

        # Left half is sorted
        if arr[left] <= arr[mid]:
            if arr[left] <= target < arr[mid]:
                right = mid - 1
            else:
                left = mid + 1
        # Right half is sorted
        else:
            if arr[mid] < target <= arr[right]:
                left = mid + 1
            else:
                right = mid - 1
    return -1

# Examples
print(search_rotated([4, 5, 6, 7, 0, 1, 2], 0))  # → 4
print(search_rotated([4, 5, 6, 7, 0, 1, 2], 3))  # → -1
print(search_rotated([1], 0))                      # → -1

# With duplicates (LeetCode 81) — worst case O(n)
def search_rotated_dups(arr, target):
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = left + (right - left) // 2
        if arr[mid] == target:
            return True
        # Can't determine which side is sorted → shrink both
        if arr[left] == arr[mid] == arr[right]:
            left += 1
            right -= 1
        elif arr[left] <= arr[mid]:
            if arr[left] <= target < arr[mid]:
                right = mid - 1
            else:
                left = mid + 1
        else:
            if arr[mid] < target <= arr[right]:
                left = mid + 1
            else:
                right = mid - 1
    return False
```

---

## 4. First / Last Occurrence

### Concept
- In a sorted array with duplicates, find the **first** or **last** index of a target.
- **Time:** O(log n) | **Space:** O(1)

### Interview Points
- This is a **modified binary search** — don't stop at first match, keep narrowing.
- For **first**: when found, keep going left (`right = mid - 1`), store result.
- For **last**: when found, keep going right (`left = mid + 1`), store result.
- Can also use to count occurrences: `last - first + 1`.

```python
def find_first(arr, target):
    left, right = 0, len(arr) - 1
    result = -1
    while left <= right:
        mid = left + (right - left) // 2
        if arr[mid] == target:
            result = mid       # record, then keep searching left
            right = mid - 1
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return result

def find_last(arr, target):
    left, right = 0, len(arr) - 1
    result = -1
    while left <= right:
        mid = left + (right - left) // 2
        if arr[mid] == target:
            result = mid       # record, then keep searching right
            left = mid + 1
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return result

def search_range(arr, target):
    return [find_first(arr, target), find_last(arr, target)]

# Count occurrences
def count_occurrences(arr, target):
    first = find_first(arr, target)
    if first == -1:
        return 0
    return find_last(arr, target) - first + 1

# Examples
arr = [5, 7, 7, 8, 8, 10]
print(search_range(arr, 8))          # → [3, 4]
print(search_range(arr, 6))          # → [-1, -1]
print(count_occurrences(arr, 7))     # → 2

# Using bisect
import bisect
first = bisect.bisect_left(arr, 8)   # 3
last  = bisect.bisect_right(arr, 8) - 1  # 4
```

---

## 5. Peak Element

### Concept
- Find an index where `arr[i] > arr[i-1]` and `arr[i] > arr[i+1]`. Ends are treated as `-∞`.
- **Time:** O(log n) | **Space:** O(1)

### Interview Points
- **Multiple peaks may exist** — any valid peak is acceptable.
- Key insight: if `arr[mid] < arr[mid+1]`, a peak **must exist** on the right. If `arr[mid] < arr[mid-1]`, peak is on the left.
- This is a non-obvious binary search — explain the invariant clearly.
- Common variant: find the **maximum** in a bitonic (mountain) array.

```python
def find_peak(arr):
    left, right = 0, len(arr) - 1
    while left < right:
        mid = left + (right - left) // 2
        if arr[mid] < arr[mid + 1]:
            # Ascending slope → peak is to the right
            left = mid + 1
        else:
            # Descending slope → peak is at mid or to the left
            right = mid
    return left  # left == right == peak index

# Examples
print(find_peak([1, 2, 3, 1]))            # → 2 (value 3)
print(find_peak([1, 2, 1, 3, 5, 6, 4]))  # → 5 (value 6) or 1 (value 2)

# Mountain Array Maximum (strict single peak)
def mountain_peak(arr):
    left, right = 0, len(arr) - 1
    while left < right:
        mid = left + (right - left) // 2
        if arr[mid] < arr[mid + 1]:
            left = mid + 1
        else:
            right = mid
    return left
```

---

## 6. Square Root (Binary Search)

### Concept
- Compute `floor(√n)` without using `math.sqrt`.
- **Time:** O(log n) | **Space:** O(1)

### Interview Points
- Great example of binary search on **answer space**, not array index.
- Search space: `[1, n]`, condition: `mid*mid <= n`.
- Beware of **integer overflow** — in Python it's not a problem, but mention it for Java/C++.
- Precision variant: find sqrt to `k` decimal places using fractional binary search.

```python
def my_sqrt(n):
    if n < 2:
        return n
    left, right = 1, n // 2
    result = 1
    while left <= right:
        mid = left + (right - left) // 2
        if mid * mid == n:
            return mid
        elif mid * mid < n:
            result = mid       # best candidate so far
            left = mid + 1
        else:
            right = mid - 1
    return result

# Examples
print(my_sqrt(4))   # → 2
print(my_sqrt(8))   # → 2 (floor)
print(my_sqrt(9))   # → 3
print(my_sqrt(0))   # → 0

# Precision variant (to p decimal places)
def sqrt_precision(n, precision=5):
    left, right = 0.0, float(n)
    for _ in range(200):  # enough iterations for any precision
        mid = (left + right) / 2
        if mid * mid > n:
            right = mid
        else:
            left = mid
    return round(left, precision)

print(sqrt_precision(2))  # → 1.41421
```

---

## 7. 2D Matrix Search

### Concept

Two common variants:
- **Variant 1 (LeetCode 74):** Rows and columns sorted, and last element of row i < first element of row i+1. Treat as a flat 1D sorted array.
- **Variant 2 (LeetCode 240):** Each row and column sorted independently. Use staircase search.

### Interview Points
- Immediately clarify which variant — they have different time complexities.
- Variant 1: **O(log m\*n)** — classic binary search with index math.
- Variant 2: **O(m + n)** — start from top-right, move left if too big, down if too small.
- Index conversion for Variant 1: `row = mid // cols`, `col = mid % cols`.

```python
# Variant 1: Fully sorted matrix (treat as 1D)
def search_matrix_v1(matrix, target):
    if not matrix or not matrix[0]:
        return False
    rows, cols = len(matrix), len(matrix[0])
    left, right = 0, rows * cols - 1
    while left <= right:
        mid = left + (right - left) // 2
        val = matrix[mid // cols][mid % cols]  # ← key index conversion
        if val == target:
            return True
        elif val < target:
            left = mid + 1
        else:
            right = mid - 1
    return False

# Variant 2: Row and column sorted (staircase search)
def search_matrix_v2(matrix, target):
    if not matrix or not matrix[0]:
        return False
    row, col = 0, len(matrix[0]) - 1  # start top-right
    while row < len(matrix) and col >= 0:
        val = matrix[row][col]
        if val == target:
            return True
        elif val > target:
            col -= 1   # too big → go left
        else:
            row += 1   # too small → go down
    return False

# Examples
m1 = [[1,3,5,7],[10,11,16,20],[23,30,34,60]]
print(search_matrix_v1(m1, 3))    # → True
print(search_matrix_v1(m1, 13))   # → False

m2 = [[1,4,7,11],[2,5,8,12],[3,6,9,16],[10,13,14,17]]
print(search_matrix_v2(m2, 5))    # → True
print(search_matrix_v2(m2, 20))   # → False
```

---

## 8. Search Insert Position

### Concept
- Given a sorted array and a target, return the index to insert it (maintaining sort order). If target exists, return its index.
- **Time:** O(log n) | **Space:** O(1)
- This is exactly `bisect_left`.

### Interview Points
- Clean, elegant binary search — `left` pointer naturally lands at the insert position.
- When the loop ends, `left` is always the answer — explain **why**: `left` is where the target would go to keep the array sorted.
- Relate to `bisect.bisect_left` — shows Python library awareness.

```python
def search_insert(arr, target):
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = left + (right - left) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return left  # left == insertion point

# Examples
print(search_insert([1, 3, 5, 6], 5))  # → 2 (found)
print(search_insert([1, 3, 5, 6], 2))  # → 1 (insert between 1 and 3)
print(search_insert([1, 3, 5, 6], 7))  # → 4 (after last)
print(search_insert([1, 3, 5, 6], 0))  # → 0 (before first)

# Equivalent using bisect
import bisect
print(bisect.bisect_left([1, 3, 5, 6], 2))  # → 1
```

---

## 🧠 Searching — Cheat Sheet

| Problem | Algorithm | Time | Space | Key Trick |
|---|---|---|---|---|
| Linear Search | Sequential scan | O(n) | O(1) | Works unsorted |
| Binary Search | Halve search space | O(log n) | O(1) | Must be sorted |
| Rotated Array | Check which half sorted | O(log n) | O(1) | One half always sorted |
| First/Last Occurrence | Binary + keep searching | O(log n) | O(1) | Don't stop on match |
| Peak Element | Compare mid vs mid+1 | O(log n) | O(1) | One peak guaranteed |
| Square Root | Binary on answer space | O(log n) | O(1) | Search `[1, n//2]` |
| 2D Matrix (fully sorted) | Flat binary search | O(log mn) | O(1) | `mid // cols`, `mid % cols` |
| 2D Matrix (row/col sorted) | Staircase from top-right | O(m+n) | O(1) | Eliminate row or column |
| Search Insert Position | Binary, return `left` | O(log n) | O(1) | `left` = insert point |

---

## ⚡ Common Binary Search Bugs to Avoid

```python
# ❌ Wrong: can cause infinite loop when left+1 == right
mid = (left + right) // 2

# ✅ Correct: always safe
mid = left + (right - left) // 2

# ❌ Off-by-one: misses last element
while left < right  →  use left <= right for exact match problems

# ❌ Forgetting that left IS the answer in insert-position style problems

# ❌ Not handling empty array
if not arr: return -1  # always add this guard
```

---

## 🔗 Binary Search Template Summary

```python
# Template 1: Exact match
left, right = 0, len(arr) - 1
while left <= right:
    mid = left + (right - left) // 2
    if arr[mid] == target: return mid
    elif arr[mid] < target: left = mid + 1
    else: right = mid - 1
return -1

# Template 2: Left boundary (first occurrence / insert position)
left, right = 0, len(arr)        # note: right = len(arr)
while left < right:               # note: strict <
    mid = left + (right - left) // 2
    if arr[mid] < target: left = mid + 1
    else: right = mid
return left

# Template 3: Right boundary (last occurrence)
left, right = 0, len(arr)
while left < right:
    mid = left + (right - left) // 2
    if arr[mid] <= target: left = mid + 1
    else: right = mid
return left - 1
```

---
---

# 🔃 Sorting

---

## 1. Bubble Sort

### Concept
- Repeatedly swap adjacent elements if they're in the wrong order. Largest element "bubbles up" to the end each pass.
- **Time:** O(n²) avg/worst | O(n) best (already sorted) | **Space:** O(1)
- **Stable:** ✅

### Interview Points
- Almost never used in practice — interviewers ask it to test fundamentals.
- Key optimization: track if **any swap occurred** in a pass — if not, array is already sorted → early exit.
- Each full pass guarantees the largest unsorted element is in its final position → inner loop shrinks each iteration.

```python
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):
        swapped = False                        # ← optimization flag
        for j in range(0, n - i - 1):         # ← shrinks by i each pass
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
                swapped = True
        if not swapped:                        # ← early exit if sorted
            break
    return arr

# Examples
print(bubble_sort([64, 34, 25, 12, 22, 11, 90]))
# → [11, 12, 22, 25, 34, 64, 90]

print(bubble_sort([1, 2, 3, 4, 5]))  # O(n) — exits after 1 pass
# → [1, 2, 3, 4, 5]
```

---

## 2. Selection Sort

### Concept
- Find the **minimum** element in the unsorted portion, swap it to the front. Repeat.
- **Time:** O(n²) always (no early exit) | **Space:** O(1)
- **Stable:** ❌ (can be made stable but not by default)

### Interview Points
- Makes exactly **n-1 swaps** — best algorithm when swap cost is very high (e.g., writing to flash memory).
- Unlike bubble sort, **no early exit** possible — always does O(n²) comparisons regardless of input.
- Not stable: swapping minimum into place can disrupt order of equal elements.

```python
def selection_sort(arr):
    n = len(arr)
    for i in range(n):
        min_idx = i                            # assume current position is min
        for j in range(i + 1, n):
            if arr[j] < arr[min_idx]:
                min_idx = j                    # find actual minimum
        arr[i], arr[min_idx] = arr[min_idx], arr[i]  # swap into place
    return arr

# Examples
print(selection_sort([64, 25, 12, 22, 11]))
# → [11, 12, 22, 25, 64]

# Demonstrating instability: equal keys can swap
# [(3,a), (3,b), (1,c)] → after pass 1: [(1,c), (3,b), (3,a)]
# relative order of (3,a) and (3,b) is lost
```

---

## 3. Insertion Sort

### Concept
- Build a sorted subarray one element at a time. Pick next element, shift larger elements right, insert in correct position.
- **Time:** O(n²) avg/worst | O(n) best (nearly sorted) | **Space:** O(1)
- **Stable:** ✅

### Interview Points
- **Best choice for small arrays (n < ~20)** — low constant factor, cache-friendly.
- Used as a subroutine in **TimSort** (Python's built-in sort) for small runs.
- Excellent for **nearly sorted** data — nearly O(n) in that case.
- Think of it like sorting playing cards in your hand.

```python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        key = arr[i]                           # element to be inserted
        j = i - 1
        while j >= 0 and arr[j] > key:        # shift larger elements right
            arr[j + 1] = arr[j]
            j -= 1
        arr[j + 1] = key                       # insert in correct position
    return arr

# Examples
print(insertion_sort([12, 11, 13, 5, 6]))
# → [5, 6, 11, 12, 13]

# Nearly sorted — almost O(n)
print(insertion_sort([1, 2, 4, 3, 5]))
# → [1, 2, 3, 4, 5]

# Binary Insertion Sort variant — fewer comparisons but same shifts
import bisect
def binary_insertion_sort(arr):
    for i in range(1, len(arr)):
        key = arr[i]
        pos = bisect.bisect_left(arr, key, 0, i)  # O(log i) comparisons
        arr[pos+1:i+1] = arr[pos:i]               # O(i) shifts still
        arr[pos] = key
    return arr
```

---

## 4. Merge Sort

### Concept
- Divide array in half recursively until size 1, then **merge** sorted halves back together.
- **Time:** O(n log n) always | **Space:** O(n) auxiliary
- **Stable:** ✅

### Interview Points
- **Guaranteed O(n log n)** — unlike quicksort, no bad worst case.
- Preferred for **linked lists** (no random access needed, O(1) extra space possible).
- Preferred for **external sorting** (data too large for RAM — merge from disk).
- The `merge` step is the key — know it cold. Two pointers, compare, append remainder.
- Common follow-up: **count inversions** (modification of merge sort).

```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr

    mid = len(arr) // 2
    left  = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    return merge(left, right)

def merge(left, right):
    result = []
    i = j = 0
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:               # ← <= ensures stability
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    result.extend(left[i:])                   # append remaining
    result.extend(right[j:])
    return result

# In-place merge (avoids slicing overhead)
def merge_sort_inplace(arr, left=0, right=None):
    if right is None:
        right = len(arr) - 1
    if left >= right:
        return
    mid = left + (right - left) // 2
    merge_sort_inplace(arr, left, mid)
    merge_sort_inplace(arr, mid + 1, right)
    merge_inplace(arr, left, mid, right)

def merge_inplace(arr, left, mid, right):
    L = arr[left:mid+1]
    R = arr[mid+1:right+1]
    i = j = 0
    k = left
    while i < len(L) and j < len(R):
        if L[i] <= R[j]:
            arr[k] = L[i]; i += 1
        else:
            arr[k] = R[j]; j += 1
        k += 1
    while i < len(L): arr[k] = L[i]; i += 1; k += 1
    while j < len(R): arr[k] = R[j]; j += 1; k += 1

# Count Inversions (classic merge sort variant)
def count_inversions(arr):
    if len(arr) <= 1:
        return arr, 0
    mid = len(arr) // 2
    left,  l_inv = count_inversions(arr[:mid])
    right, r_inv = count_inversions(arr[mid:])
    merged, split_inv = merge_count(left, right)
    return merged, l_inv + r_inv + split_inv

def merge_count(left, right):
    result, count, i, j = [], 0, 0, 0
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i]); i += 1
        else:
            count += len(left) - i    # ← all remaining left elements form inversions
            result.append(right[j]); j += 1
    result.extend(left[i:]); result.extend(right[j:])
    return result, count

print(merge_sort([38, 27, 43, 3, 9, 82, 10]))
# → [3, 9, 10, 27, 38, 43, 82]
_, inv = count_inversions([2, 4, 1, 3, 5])
print(inv)  # → 3
```

---

## 5. Quick Sort

### Concept
- Pick a **pivot**, partition array so elements < pivot are left, > pivot are right. Recurse.
- **Time:** O(n log n) avg | O(n²) worst (bad pivot) | **Space:** O(log n) avg stack
- **Stable:** ❌

### Interview Points
- **Fastest in practice** for in-memory sorting due to cache efficiency and low constant factor.
- Worst case O(n²) happens when pivot is always min/max (sorted/reverse-sorted input).
- **Three pivot strategies:**
  - Last element (naive, bad on sorted input)
  - Random pivot (avoids adversarial inputs)
  - Median-of-three (robust, used in practice)
- **Lomuto vs Hoare partition** — know both, Hoare does fewer swaps.
- **3-way partition** (Dutch National Flag) — handles duplicates efficiently → O(n) when all elements equal.

```python
import random

# Lomuto partition scheme
def quick_sort_lomuto(arr, low=0, high=None):
    if high is None: high = len(arr) - 1
    if low < high:
        pi = partition_lomuto(arr, low, high)
        quick_sort_lomuto(arr, low, pi - 1)
        quick_sort_lomuto(arr, pi + 1, high)

def partition_lomuto(arr, low, high):
    pivot = arr[high]                          # pivot = last element
    i = low - 1                               # i tracks smaller region
    for j in range(low, high):
        if arr[j] <= pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]
    arr[i+1], arr[high] = arr[high], arr[i+1]
    return i + 1

# Hoare partition scheme (more efficient — fewer swaps)
def quick_sort_hoare(arr, low=0, high=None):
    if high is None: high = len(arr) - 1
    if low < high:
        pi = partition_hoare(arr, low, high)
        quick_sort_hoare(arr, low, pi)
        quick_sort_hoare(arr, pi + 1, high)

def partition_hoare(arr, low, high):
    pivot = arr[low + (high - low) // 2]      # middle element as pivot
    i, j = low - 1, high + 1
    while True:
        i += 1
        while arr[i] < pivot: i += 1
        j -= 1
        while arr[j] > pivot: j -= 1
        if i >= j: return j
        arr[i], arr[j] = arr[j], arr[i]

# Randomized quicksort (best for interviews)
def quick_sort_random(arr, low=0, high=None):
    if high is None: high = len(arr) - 1
    if low < high:
        rand_idx = random.randint(low, high)
        arr[rand_idx], arr[high] = arr[high], arr[rand_idx]  # swap random to end
        pi = partition_lomuto(arr, low, high)
        quick_sort_random(arr, low, pi - 1)
        quick_sort_random(arr, pi + 1, high)

# 3-way partition — Dutch National Flag (handles duplicates perfectly)
def quick_sort_3way(arr, low=0, high=None):
    if high is None: high = len(arr) - 1
    if low >= high: return
    pivot = arr[low + (high - low) // 2]
    lt, gt = low, high
    i = low
    while i <= gt:
        if arr[i] < pivot:
            arr[lt], arr[i] = arr[i], arr[lt]
            lt += 1; i += 1
        elif arr[i] > pivot:
            arr[gt], arr[i] = arr[i], arr[gt]
            gt -= 1                             # don't increment i — recheck
        else:
            i += 1
    # arr[low..lt-1] < pivot, arr[lt..gt] == pivot, arr[gt+1..high] > pivot
    quick_sort_3way(arr, low, lt - 1)
    quick_sort_3way(arr, gt + 1, high)

arr = [10, 7, 8, 9, 1, 5]
quick_sort_lomuto(arr)
print(arr)  # → [1, 5, 7, 8, 9, 10]

arr = [3, 3, 3, 3, 1, 2]
quick_sort_3way(arr)
print(arr)  # → [1, 2, 3, 3, 3, 3]
```

---

## 6. Heap Sort

### Concept
- Build a **max-heap**, then repeatedly extract the maximum (swap root with last, heapify down).
- **Time:** O(n log n) always | **Space:** O(1) — in-place
- **Stable:** ❌

### Interview Points
- Only comparison sort that is **both O(n log n) worst case AND O(1) space**.
- Two phases: **Build heap** O(n) + **Extract** n times × O(log n) = O(n log n).
- Build heap starts from `n//2 - 1` (last non-leaf) — not from root. This is O(n) not O(n log n).
- Not cache-friendly → slower than quicksort/mergesort in practice despite same big-O.
- Know `heapify` (sift down) cold — it's the core operation.

```python
def heap_sort(arr):
    n = len(arr)

    # Phase 1: Build max-heap — O(n)
    # Start from last non-leaf node and sift down
    for i in range(n // 2 - 1, -1, -1):
        heapify(arr, n, i)

    # Phase 2: Extract elements — O(n log n)
    for i in range(n - 1, 0, -1):
        arr[0], arr[i] = arr[i], arr[0]        # move current max to end
        heapify(arr, i, 0)                     # restore heap on reduced size

    return arr

def heapify(arr, n, i):
    """Sift down: ensure subtree rooted at i is a max-heap."""
    largest = i
    left    = 2 * i + 1
    right   = 2 * i + 2

    if left  < n and arr[left]  > arr[largest]: largest = left
    if right < n and arr[right] > arr[largest]: largest = right

    if largest != i:
        arr[i], arr[largest] = arr[largest], arr[i]
        heapify(arr, n, largest)               # recursively fix the affected subtree

# Examples
print(heap_sort([12, 11, 13, 5, 6, 7]))
# → [5, 6, 7, 11, 12, 13]

# Using Python's heapq (min-heap) for reference
import heapq
def heap_sort_builtin(arr):
    heapq.heapify(arr)                         # O(n) build min-heap
    return [heapq.heappop(arr) for _ in arr]   # O(n log n) extract

# Heap relationships (0-indexed):
# parent(i)      = (i - 1) // 2
# left_child(i)  = 2 * i + 1
# right_child(i) = 2 * i + 2
```

---

## 7. Counting Sort

### Concept
- Count the frequency of each element, compute cumulative counts, then place elements in output.
- **Time:** O(n + k) where k = range of values | **Space:** O(n + k)
- **Stable:** ✅ (when implemented correctly with cumulative counts)

### Interview Points
- **Non-comparison sort** — breaks the O(n log n) lower bound.
- Only works on **integers** (or items mappable to integers) within a known bounded range.
- If k >> n, it's worse than comparison sorts — always state this tradeoff.
- Foundation for **Radix Sort**.
- Stable version: use cumulative count and fill from **right to left**.

```python
def counting_sort(arr):
    if not arr: return arr
    max_val = max(arr)
    min_val = min(arr)
    range_val = max_val - min_val + 1

    # Count frequencies
    count = [0] * range_val
    for val in arr:
        count[val - min_val] += 1

    # Reconstruct (simple version)
    result = []
    for i, freq in enumerate(count):
        result.extend([i + min_val] * freq)
    return result

# Stable version (preserves order of equal elements)
def counting_sort_stable(arr, max_val=None):
    if not arr: return arr
    if max_val is None: max_val = max(arr)

    count = [0] * (max_val + 1)
    for val in arr:
        count[val] += 1

    # Cumulative count — count[i] = number of elements ≤ i
    for i in range(1, len(count)):
        count[i] += count[i - 1]

    output = [0] * len(arr)
    for val in reversed(arr):                  # ← right to left ensures stability
        output[count[val] - 1] = val
        count[val] -= 1
    return output

# Examples
print(counting_sort([4, 2, 2, 8, 3, 3, 1]))
# → [1, 2, 2, 3, 3, 4, 8]

print(counting_sort([-2, -3, 0, 1, -1]))      # handles negatives via min offset
# → [-3, -2, -1, 0, 1]
```

---

## 8. Radix Sort

### Concept
- Sort integers digit by digit from **least significant** to most significant digit (LSD radix sort), using counting sort as a stable subroutine.
- **Time:** O(d × (n + k)) where d = digits, k = base (usually 10) | **Space:** O(n + k)
- **Stable:** ✅

### Interview Points
- Another **non-comparison sort** — O(n) when d and k are constants.
- **LSD (Least Significant Digit):** Sort from rightmost digit — standard, stable.
- **MSD (Most Significant Digit):** Sort from leftmost — used for strings, recursive.
- Works on integers, fixed-length strings, dates — **not** floating point directly.
- Key insight: **must use a stable sort** at each digit position (counting sort is used).
- For 32-bit integers: d = 10 digits, so effectively O(10n) = O(n).

```python
def radix_sort(arr):
    if not arr: return arr

    max_val = max(abs(x) for x in arr)

    exp = 1
    while max_val // exp > 0:
        counting_sort_by_digit(arr, exp)
        exp *= 10
    return arr

def counting_sort_by_digit(arr, exp):
    n = len(arr)
    output = [0] * n
    count  = [0] * 10                          # digits 0-9

    for val in arr:
        digit = (val // exp) % 10
        count[digit] += 1

    # Cumulative count
    for i in range(1, 10):
        count[i] += count[i - 1]

    # Fill output right to left (stability)
    for i in range(n - 1, -1, -1):
        digit = (arr[i] // exp) % 10
        output[count[digit] - 1] = arr[i]
        count[digit] -= 1

    arr[:] = output                            # in-place update

# Full version handling negatives
def radix_sort_full(arr):
    if not arr: return arr
    negatives = sorted([-x for x in arr if x < 0])  # treat as positives
    positives = [x for x in arr if x >= 0]

    def _radix(a):
        if not a: return a
        max_val = max(a)
        exp = 1
        while max_val // exp > 0:
            counting_sort_by_digit(a, exp)
            exp *= 10
        return a

    _radix(negatives)
    _radix(positives)
    return [-x for x in reversed(negatives)] + positives

# Examples
arr = [170, 45, 75, 90, 802, 24, 2, 66]
print(radix_sort(arr))
# → [2, 24, 45, 66, 75, 90, 170, 802]
```

---

## 9. Bucket Sort

### Concept
- Distribute elements into **buckets** based on value range, sort each bucket individually (usually insertion sort), then concatenate.
- **Time:** O(n + k) avg | O(n²) worst (all elements in one bucket) | **Space:** O(n + k)
- **Stable:** ✅ (if stable sort used within buckets)

### Interview Points
- Best for **uniformly distributed** floating-point numbers in `[0, 1)`.
- Works great when input is drawn from a **known, bounded, roughly uniform** distribution.
- Number of buckets is a tunable parameter — typically `n` buckets for `n` elements.
- Generalizes to integers by mapping to bucket index: `bucket_idx = int(val / max_val * n)`.
- Worst case still O(n²) if distribution is skewed — always mention this.

```python
def bucket_sort(arr):
    if not arr: return arr
    n = len(arr)

    # Create n empty buckets
    buckets = [[] for _ in range(n)]

    max_val = max(arr)
    min_val = min(arr)
    range_val = max_val - min_val

    # Distribute elements into buckets
    for val in arr:
        if range_val == 0:
            idx = 0
        else:
            idx = int((val - min_val) / range_val * (n - 1))
        buckets[idx].append(val)

    # Sort each bucket and concatenate
    result = []
    for bucket in buckets:
        insertion_sort_bucket(bucket)          # insertion sort works well for small buckets
        result.extend(bucket)
    return result

def insertion_sort_bucket(arr):
    for i in range(1, len(arr)):
        key = arr[i]
        j = i - 1
        while j >= 0 and arr[j] > key:
            arr[j + 1] = arr[j]
            j -= 1
        arr[j + 1] = key

# Classic use case: floats in [0, 1)
def bucket_sort_floats(arr):
    n = len(arr)
    buckets = [[] for _ in range(n)]
    for val in arr:
        idx = int(val * n)                     # maps [0,1) to [0, n-1]
        idx = min(idx, n - 1)                  # clamp for val == 1.0
        buckets[idx].append(val)
    result = []
    for bucket in buckets:
        result.extend(sorted(bucket))
    return result

# Examples
print(bucket_sort([64, 34, 25, 12, 22, 11, 90]))
# → [11, 12, 22, 25, 34, 64, 90]

floats = [0.78, 0.17, 0.39, 0.26, 0.72, 0.94, 0.21, 0.12]
print(bucket_sort_floats(floats))
# → [0.12, 0.17, 0.21, 0.26, 0.39, 0.72, 0.78, 0.94]
```

---

## 🧠 Sorting — Cheat Sheet

| Algorithm | Best | Average | Worst | Space | Stable | Best Use Case |
|---|---|---|---|---|---|---|
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) | ✅ | Teaching only |
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) | ❌ | Minimum swaps needed |
| Insertion Sort | O(n) | O(n²) | O(n²) | O(1) | ✅ | Small / nearly sorted |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) | ✅ | Linked lists, external sort |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | O(log n) | ❌ | General-purpose (fastest avg) |
| Heap Sort | O(n log n) | O(n log n) | O(n log n) | O(1) | ❌ | O(n log n) + O(1) space |
| Counting Sort | O(n+k) | O(n+k) | O(n+k) | O(n+k) | ✅ | Small integer range |
| Radix Sort | O(dn) | O(dn) | O(dn) | O(n+k) | ✅ | Fixed-width integers/strings |
| Bucket Sort | O(n+k) | O(n+k) | O(n²) | O(n+k) | ✅ | Uniform float distribution |

---

## ⚡ Sorting — Algorithm Decision Framework

```
Input is integers with small range (k << n²)?
  └─ Yes → Counting Sort or Radix Sort

Input is floats uniformly in [0,1)?
  └─ Yes → Bucket Sort

Need guaranteed O(n log n) worst case?
  ├─ Space is no concern → Merge Sort (stable)
  └─ Must be in-place → Heap Sort

General purpose, in-memory, speed matters most?
  └─ Quick Sort (randomized pivot)

Linked list?
  └─ Merge Sort (no random access needed)

Small array (n < 20) or nearly sorted?
  └─ Insertion Sort

External sorting (data on disk)?
  └─ Merge Sort
```

---

## 🔗 Python Built-in Sort

```python
# Python uses TimSort — hybrid of Merge Sort + Insertion Sort
# O(n log n) worst case, O(n) best case, stable, O(n) space

arr = [3, 1, 4, 1, 5, 9, 2, 6]
arr.sort()                          # in-place, returns None
new = sorted(arr)                   # returns new list

# Custom key — sort by second element of tuple
pairs = [(1, 'b'), (2, 'a'), (3, 'c')]
pairs.sort(key=lambda x: x[1])

# Reverse sort
arr.sort(reverse=True)

# Sort objects by attribute
from functools import cmp_to_key
def compare(a, b): return a - b
arr.sort(key=cmp_to_key(compare))

# TimSort details — worth mentioning in interviews:
# - Finds natural runs (already sorted subsequences)
# - Merges runs using merge sort
# - Uses insertion sort for runs shorter than ~64 elements
# - This is why it's O(n) on already-sorted input
```

---
---

# 👉👈 Two Pointer Techniques

---

## 1. Two Sum / Three Sum

### Concept
- **Two Sum (sorted):** Place one pointer at start, one at end. If sum too small → move left right. If too large → move right left.
- **Three Sum:** Fix one element, run two-pointer two-sum on the remainder.
- **Time:** Two Sum O(n log n) sort + O(n) scan = O(n log n) | Three Sum O(n²) | **Space:** O(1) extra (excluding output)

### Interview Points
- **Always clarify:** return indices or values? unique pairs only? sorted input assumed?
- Two Sum on **unsorted** array → use a hashmap for O(n) — two pointer only works on sorted.
- Three Sum: sort first, then for each fixed element skip duplicates **at two levels**: the outer loop and inner pointers.
- Key duplicate-skip pattern: `while left < right and arr[left] == arr[left-1]: left += 1`
- Common follow-ups: **Four Sum** (two nested loops + two pointer), **Two Sum closest to target**, **count pairs**.

```python
# ── Two Sum (sorted array) ─────────────────────────────────────────
def two_sum_sorted(arr, target):
    left, right = 0, len(arr) - 1
    while left < right:
        s = arr[left] + arr[right]
        if s == target:
            return [left, right]
        elif s < target:
            left += 1
        else:
            right -= 1
    return []

# Two Sum (unsorted) — hashmap O(n), return original indices
def two_sum_unsorted(arr, target):
    seen = {}                                  # val → index
    for i, val in enumerate(arr):
        complement = target - val
        if complement in seen:
            return [seen[complement], i]
        seen[val] = i
    return []

# ── Two Sum — all unique pairs ─────────────────────────────────────
def two_sum_all_pairs(arr, target):
    arr.sort()
    result = []
    left, right = 0, len(arr) - 1
    while left < right:
        s = arr[left] + arr[right]
        if s == target:
            result.append([arr[left], arr[right]])
            while left < right and arr[left]  == arr[left + 1]:  left  += 1
            while left < right and arr[right] == arr[right - 1]: right -= 1
            left += 1; right -= 1
        elif s < target:
            left += 1
        else:
            right -= 1
    return result

# ── Three Sum ──────────────────────────────────────────────────────
def three_sum(arr):
    arr.sort()
    result = []
    for i in range(len(arr) - 2):
        # Skip duplicate fixed elements
        if i > 0 and arr[i] == arr[i - 1]:
            continue
        left, right = i + 1, len(arr) - 1
        while left < right:
            s = arr[i] + arr[left] + arr[right]
            if s == 0:
                result.append([arr[i], arr[left], arr[right]])
                while left < right and arr[left]  == arr[left + 1]:  left  += 1
                while left < right and arr[right] == arr[right - 1]: right -= 1
                left += 1; right -= 1
            elif s < 0:
                left += 1
            else:
                right -= 1
    return result

# ── Three Sum Closest ──────────────────────────────────────────────
def three_sum_closest(arr, target):
    arr.sort()
    closest = float('inf')
    for i in range(len(arr) - 2):
        left, right = i + 1, len(arr) - 1
        while left < right:
            s = arr[i] + arr[left] + arr[right]
            if abs(s - target) < abs(closest - target):
                closest = s
            if s < target:   left += 1
            elif s > target: right -= 1
            else:            return s           # exact match
    return closest

# ── Four Sum ──────────────────────────────────────────────────────
def four_sum(arr, target):
    arr.sort()
    result = []
    n = len(arr)
    for i in range(n - 3):
        if i > 0 and arr[i] == arr[i-1]: continue
        for j in range(i + 1, n - 2):
            if j > i + 1 and arr[j] == arr[j-1]: continue
            left, right = j + 1, n - 1
            while left < right:
                s = arr[i] + arr[j] + arr[left] + arr[right]
                if s == target:
                    result.append([arr[i], arr[j], arr[left], arr[right]])
                    while left < right and arr[left]  == arr[left + 1]:  left  += 1
                    while left < right and arr[right] == arr[right - 1]: right -= 1
                    left += 1; right -= 1
                elif s < target: left += 1
                else:            right -= 1
    return result

# Examples
print(two_sum_sorted([2, 7, 11, 15], 9))         # → [0, 1]
print(two_sum_unsorted([3, 2, 4], 6))             # → [1, 2]
print(three_sum([-1, 0, 1, 2, -1, -4]))           # → [[-1,-1,2],[-1,0,1]]
print(three_sum_closest([-1, 2, 1, -4], 1))       # → 2
print(four_sum([1, 0, -1, 0, -2, 2], 0))
# → [[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]
```

---

## 2. Container With Most Water

### Concept
- Given heights array, find two lines that together with the x-axis form a container holding the most water.
- Area = `min(height[left], height[right]) × (right - left)`
- **Time:** O(n) | **Space:** O(1)

### Interview Points
- Classic greedy two-pointer proof: always move the **shorter** side inward.
- Reasoning: if you move the taller side, width decreases AND height is still bounded by the shorter — you can only lose. Moving the shorter side at least gives a chance at a taller boundary.
- Interviewers love asking: **"why move the shorter pointer?"** — be ready to explain the invariant.
- Common mistake: moving both pointers, or moving the taller one.
- Variant: **Trapping Rain Water** is different — it accounts for water trapped between bars (uses stack or two-pointer with running max).

```python
# ── Container With Most Water ─────────────────────────────────────
def max_area(height):
    left, right = 0, len(height) - 1
    max_water = 0
    while left < right:
        h = min(height[left], height[right])
        w = right - left
        max_water = max(max_water, h * w)
        # Move the shorter side — only way to potentially increase area
        if height[left] < height[right]:
            left += 1
        else:
            right -= 1
    return max_water

# ── Trapping Rain Water (classic variant) ─────────────────────────
# Water at i = min(max_left, max_right) - height[i]
def trap_rain_water(height):
    if not height: return 0
    left, right = 0, len(height) - 1
    left_max = right_max = 0
    water = 0
    while left < right:
        if height[left] < height[right]:
            if height[left] >= left_max:
                left_max = height[left]
            else:
                water += left_max - height[left]   # trapped water at left
            left += 1
        else:
            if height[right] >= right_max:
                right_max = height[right]
            else:
                water += right_max - height[right] # trapped water at right
            right -= 1
    return water

# Trap Rain Water — prefix/suffix max approach (clearer, O(n) space)
def trap_rain_water_v2(height):
    n = len(height)
    left_max  = [0] * n
    right_max = [0] * n
    left_max[0]   = height[0]
    right_max[-1] = height[-1]
    for i in range(1, n):
        left_max[i] = max(left_max[i-1], height[i])
    for i in range(n-2, -1, -1):
        right_max[i] = max(right_max[i+1], height[i])
    return sum(min(left_max[i], right_max[i]) - height[i] for i in range(n))

# Examples
print(max_area([1, 8, 6, 2, 5, 4, 8, 3, 7]))        # → 49
print(max_area([1, 1]))                               # → 1
print(trap_rain_water([0,1,0,2,1,0,1,3,2,1,2,1]))    # → 6
print(trap_rain_water_v2([4,2,0,3,2,5]))              # → 9
```

---

## 3. Dutch National Flag Algorithm

### Concept
- Sort an array of 0s, 1s, and 2s **in-place in a single pass**.
- Three pointers: `low` (boundary of 0s), `mid` (current), `high` (boundary of 2s).
- **Time:** O(n) | **Space:** O(1)

### Interview Points
- Invented by **Dijkstra** — named after the Dutch flag (3 color bands).
- Core invariant at all times:
  - `arr[0..low-1]` → all 0s
  - `arr[low..mid-1]` → all 1s
  - `arr[mid..high]` → unknown
  - `arr[high+1..n-1]` → all 2s
- When swapping with `high`, **don't increment `mid`** — the swapped element is unknown.
- When swapping with `low`, **do increment `mid`** — swapped element is guaranteed to be 1.
- This is the foundation of **3-way quicksort partition**.
- Follow-up: sort array with k distinct values → generalize with counting sort or recursive DNF.

```python
# ── Dutch National Flag ───────────────────────────────────────────
def sort_colors(arr):
    low = 0                                    # next position for 0
    mid = 0                                    # current element
    high = len(arr) - 1                        # next position for 2

    while mid <= high:
        if arr[mid] == 0:
            arr[low], arr[mid] = arr[mid], arr[low]
            low += 1
            mid += 1                           # ← safe: arr[low] was 1
        elif arr[mid] == 1:
            mid += 1                           # 1 is already in place
        else:                                  # arr[mid] == 2
            arr[mid], arr[high] = arr[high], arr[mid]
            high -= 1                          # ← don't increment mid: recheck

    return arr

# Step-by-step trace for [2,0,2,1,1,0]:
# low=0 mid=0 high=5: arr[0]=2 → swap(0,5) → [0,0,2,1,1,2], high=4
# low=0 mid=0 high=4: arr[0]=0 → swap(0,0) → [0,0,2,1,1,2], low=1,mid=1
# low=1 mid=1 high=4: arr[1]=0 → swap(1,1) → [0,0,2,1,1,2], low=2,mid=2
# low=2 mid=2 high=4: arr[2]=2 → swap(2,4) → [0,0,1,1,2,2], high=3
# low=2 mid=2 high=3: arr[2]=1 → mid=3
# low=2 mid=3 high=3: arr[3]=1 → mid=4
# mid > high → done: [0,0,1,1,2,2] ✓

# ── Generalized: sort array with values {0..k} ────────────────────
def sort_k_colors(arr, k):
    count = [0] * k
    for val in arr: count[val] += 1
    idx = 0
    for val in range(k):
        for _ in range(count[val]):
            arr[idx] = val
            idx += 1
    return arr

# ── Variant: Move all zeros to end ───────────────────────────────
def move_zeros(arr):
    low = 0
    for mid in range(len(arr)):
        if arr[mid] != 0:
            arr[low], arr[mid] = arr[mid], arr[low]
            low += 1
    return arr

# ── Variant: Separate negatives and positives ────────────────────
def separate_neg_pos(arr):
    low, high = 0, len(arr) - 1
    while low < high:
        while low < high and arr[low] < 0:   low  += 1
        while low < high and arr[high] >= 0: high -= 1
        if low < high:
            arr[low], arr[high] = arr[high], arr[low]
    return arr

# Examples
print(sort_colors([2, 0, 2, 1, 1, 0]))       # → [0, 0, 1, 1, 2, 2]
print(sort_colors([2, 0, 1]))                 # → [0, 1, 2]
print(move_zeros([0, 1, 0, 3, 12]))           # → [1, 3, 12, 0, 0]
print(separate_neg_pos([-1, 2, -3, 4, -5]))   # → [-1, -5, -3, 2, 4]
```

---

## 4. Remove Duplicates

### Concept
- Use a **slow pointer** (`write`) to track the position to write next unique element, and a **fast pointer** (`read`) to scan ahead.
- **Time:** O(n) | **Space:** O(1) in-place

### Interview Points
- Classic **read/write two-pointer** pattern — different from opposite-end pattern.
- Return the new length `k`; first `k` elements of array are the answer.
- Interviewers often say "don't allocate extra space" — this is O(1) space.
- **Variants** in order of increasing complexity:
  1. Remove duplicates (each element appears **once**)
  2. Allow duplicates up to **k times** (generalizes elegantly)
  3. Remove a **specific value** in-place
  4. Remove duplicates from **sorted linked list**
- The `k=2` generalization trick: compare `arr[write]` with `arr[write - k]` instead of `arr[write - 1]`.

```python
# ── Remove Duplicates (each element once, sorted array) ───────────
def remove_duplicates(arr):
    if not arr: return 0
    write = 1                                  # next write position
    for read in range(1, len(arr)):
        if arr[read] != arr[read - 1]:         # new unique element found
            arr[write] = arr[read]
            write += 1
    return write                               # new length

# ── Allow at most k=2 duplicates ─────────────────────────────────
def remove_duplicates_k(arr, k=2):
    if len(arr) <= k: return len(arr)
    write = k                                  # first k elements always kept
    for read in range(k, len(arr)):
        if arr[read] != arr[write - k]:        # compare with k positions back
            arr[write] = arr[read]
            write += 1
    return write

# Why does arr[read] != arr[write - k] work?
# arr[write-k..write-1] are the last k written elements.
# If arr[read] == arr[write-k], we already have k copies → skip.
# If different → safe to include.

# ── Remove specific value in-place ───────────────────────────────
def remove_element(arr, val):
    write = 0
    for read in range(len(arr)):
        if arr[read] != val:
            arr[write] = arr[read]
            write += 1
    return write

# ── Remove duplicates from unsorted array ────────────────────────
def remove_duplicates_unsorted(arr):
    seen = set()
    write = 0
    for read in range(len(arr)):
        if arr[read] not in seen:
            seen.add(arr[read])
            arr[write] = arr[read]
            write += 1
    return write

# ── Remove duplicates from sorted linked list ────────────────────
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def remove_duplicates_linked(head):
    curr = head
    while curr and curr.next:
        if curr.val == curr.next.val:
            curr.next = curr.next.next         # skip duplicate
        else:
            curr = curr.next                   # only advance if no duplicate
    return head

# Remove ALL nodes with duplicate values (keep only unique)
def remove_all_duplicates_linked(head):
    dummy = ListNode(0, head)
    prev = dummy
    curr = head
    while curr:
        if curr.next and curr.val == curr.next.val:
            while curr.next and curr.val == curr.next.val:
                curr = curr.next               # skip all dupes
            prev.next = curr.next              # skip the last dupe too
        else:
            prev = prev.next
        curr = curr.next
    return dummy.next

# Examples
arr = [1, 1, 2, 3, 3, 4]
k = remove_duplicates(arr)
print(arr[:k])                                 # → [1, 2, 3, 4]

arr = [1, 1, 1, 2, 2, 3]
k = remove_duplicates_k(arr, k=2)
print(arr[:k])                                 # → [1, 1, 2, 2, 3]

arr = [3, 2, 2, 3]
k = remove_element(arr, 3)
print(arr[:k])                                 # → [2, 2]
```

---

## 🧠 Two Pointer — Cheat Sheet

| Problem | Pointer Setup | Move Condition | Key Insight |
|---|---|---|---|
| Two Sum (sorted) | Opposite ends | Sum < target → left++, Sum > target → right-- | Sorted + opposite ends |
| Three Sum | Fix i, opposite ends on rest | Same as two sum for inner | Skip duplicates at both levels |
| Container With Water | Opposite ends | Move the **shorter** side | Width shrinks, height can only improve on shorter side |
| Trapping Rain Water | Opposite ends | Move smaller max side | Water = min(left_max, right_max) - height |
| Dutch National Flag | low / mid / high | 0→swap low, 1→skip, 2→swap high | Don't advance mid after swap with high |
| Remove Duplicates | Read / Write (same dir) | Write only on new unique value | Write pointer = next insertion point |
| Remove Element | Read / Write (same dir) | Write only when val ≠ target | Overwrite targets with non-targets |

---

## ⚡ Two Pointer Patterns At a Glance

```python
# ── Pattern 1: Opposite ends (sorted array, target sum) ──────────
left, right = 0, len(arr) - 1
while left < right:
    if condition_met:      return result
    elif need_larger:      left += 1
    else:                  right -= 1

# ── Pattern 2: Read / Write (in-place modification) ──────────────
write = 0
for read in range(len(arr)):
    if should_keep(arr[read]):
        arr[write] = arr[read]
        write += 1
return write                               # new length

# ── Pattern 3: Fast / Slow (cycle detection, middle of list) ─────
slow = fast = head
while fast and fast.next:
    slow = slow.next
    fast = fast.next.next
# slow is now at the middle

# ── Pattern 4: Three pointers (DNF, 3-way partition) ─────────────
low, mid, high = 0, 0, len(arr) - 1
while mid <= high:
    if arr[mid] == LOW:    swap(low, mid); low += 1; mid += 1
    elif arr[mid] == MID:  mid += 1
    else:                  swap(mid, high); high -= 1
```

---

## 🔗 Duplicate Skipping Template

```python
# After recording a valid result, skip duplicates on both sides:
while left < right and arr[left]  == arr[left  + 1]: left  += 1
while left < right and arr[right] == arr[right - 1]: right -= 1
left  += 1
right -= 1

# In outer loop of 3Sum:
if i > 0 and arr[i] == arr[i - 1]: continue   # skip duplicate fixed element
```

---













