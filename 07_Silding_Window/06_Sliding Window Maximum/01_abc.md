# Sliding Window Maximum Solution (Leetcode 239)

## Problem Statement

Array mein integers ka ek list diya gaya hai `nums`, aur ek sliding window size `k` diya 
gaya hai jo array ke left se right tak move karta hai. Aap window mein sirf k numbers 
dekh sakte hain. Har bar window ek position right move karta hai.

Hamein har position per max sliding window return karna hai.

## Visualization with ASCII Art

Initial array with window size k=3:
```
[1, 3, -1, -3, 5, 3, 6, 7]
 ↑  ↑  ↑
 └──┴──┘ <- Window 1: max = 3
    ↑  ↑  ↑
    └──┴──┘ <- Window 2: max = 3
       ↑  ↑  ↑
       └──┴──┘ <- Window 3: max = -1
          ↑  ↑  ↑
          └──┴──┘ <- Window 4: max = 5
             ↑  ↑  ↑
             └──┴──┘ <- Window 5: max = 5
                ↑  ↑  ↑
                └──┴──┘ <- Window 6: max = 6
```

Output: [3, 3, -1, 5, 5, 6, 7]

## Visualization of Solutions

### 1. Brute Force Solution (Time Complexity: O(n*k))

```
Input array: [1, 3, -1, -3, 5, 3, 6, 7], k=3

Window 1: [1, 3, -1]   -> max = 3   (Checking each element in window)
           ↑  ↑  ↑
           max

Window 2: [3, -1, -3]  -> max = 3   (Checking each element in window)
           ↑  ×  ×
           max

Window 3: [-1, -3, 5]  -> max = 5   (Checking each element in window)
            ×  ×  ↑
                  max

... and so on
```

### 2. Heap Solution (Time Complexity: O(n log k))

```
Input array: [1, 3, -1, -3, 5, 3, 6, 7], k=3

Heap: [(value, index)]

Window 1: [1, 3, -1]
Heap: [(-3,1), (-1,2), (-3,3)]  // Max heap using negative values
      -3 is max, but at index 1 = 3 is our max

Window 2: [3, -1, -3]
Remove 1 from heap, add -3 (index 3)
Heap: [(-3,1), (-3,3), (-1,2)]
      -3 at index 1 = 3 is our max
      
... and so on
```

### 3. Deque Solution (Time Complexity: O(n))

```
Input array: [1, 3, -1, -3, 5, 3, 6, 7], k=3

Deque stores indices

Window 1: [1, 3, -1]
         
Initial:
Deque: []
Process 1: Deque: [0]      // Index of 1
Process 3: Deque: [1]      // 3 > 1, so 1 gets removed
Process -1: Deque: [1, 2]  // -1 < 3, so append
Result: Deque[0] = index 1 = 3

Window 2: [3, -1, -3]
Remove outdated elements (index < i-k+1)
Deque: [1, 2]  // 1 is still valid
Process -3: Deque: [1, 2, 3]  // -3 < -1, so append
Result: Deque[0] = index 1 = 3

... and so on
```

## Solution 1: Brute Force Approach

```python
def maxSlidingWindow_brute(nums, k):
    """
    Brute force solution for finding maximum in each sliding window.
    
    Yeh solution har window ko check karta hai aur max element dhoondhta hai.
    
    Parameters:
        nums (List[int]): Input array jisme sliding window apply karna hai
        k (int): Sliding window ka size
    
    Returns:
        List[int]: Har sliding window ka maximum element
    
    Time Complexity: O(n*k) - n iterations, each finding max in window of size k
    Space Complexity: O(n-k+1) - for result array
    """
    n = len(nums)
    if n == 0 or k == 0:
        return []
    if k == 1:
        return nums
    
    result = []
    
    # Har possible window ke liye loop
    for i in range(n - k + 1):
        # Window ke andar maximum element find karo
        max_val = max(nums[i:i+k])
        result.append(max_val)
    
    return result
```

## Solution 2: Heap Approach

```python
import heapq

def maxSlidingWindow_heap(nums, k):
    """
    Heap based solution for finding maximum in each sliding window.
    
    Is approach mein hum ek max heap maintain karte hain jisme tuples (value, index)
    store hote hain. Har step pe valid elements se maximum nikal lete hain.
    
    Parameters:
        nums (List[int]): Input array jisme sliding window apply karna hai
        k (int): Sliding window ka size
    
    Returns:
        List[int]: Har sliding window ka maximum element
    
    Time Complexity: O(n log k) - heap operations take log k time
    Space Complexity: O(k) - for the heap
    """
    n = len(nums)
    if n == 0 or k == 0:
        return []
    if k == 1:
        return nums
    
    result = []
    heap = []  # Max heap banane ke liye negative values use karenge
    
    # Pehli window process karo
    for i in range(k):
        heapq.heappush(heap, (-nums[i], i))  # (negative value, index)
    
    # Pehli window ka max result mein add karo
    result.append(-heap[0][0])
    
    # Aage ke windows process karo
    for i in range(k, n):
        heapq.heappush(heap, (-nums[i], i))
        
        # Remove elements that are no longer in the current window
        while heap and heap[0][1] <= i - k:
            heapq.heappop(heap)
        
        # Current window ka max add karo
        result.append(-heap[0][0])
    
    return result
```

## Solution 3: Segment Tree Approach

```python
class SegmentTree:
    """
    Segment Tree data structure for range query operations.
    
    Yeh data structure array ke segments ka maximum efficiently find karne ke liye
    use hota hai. Isse hum O(log n) time mein range maximum query kar sakte hain.
    """
    
    def __init__(self, nums):
        """Initialize segment tree with given array."""
        self.n = len(nums)
        self.tree = [0] * (4 * self.n)  # 4n is sufficient for segment tree size
        self.build_tree(nums, 0, 0, self.n - 1)
    
    def build_tree(self, nums, node, start, end):
        """Build segment tree recursively."""
        if start == end:
            self.tree[node] = nums[start]
            return
        
        mid = (start + end) // 2
        # Left child
        self.build_tree(nums, 2 * node + 1, start, mid)
        # Right child
        self.build_tree(nums, 2 * node + 2, mid + 1, end)
        # Current node stores maximum of children
        self.tree[node] = max(self.tree[2 * node + 1], self.tree[2 * node + 2])
    
    def query(self, node, start, end, left, right):
        """Query for maximum in range [left, right]."""
        # No overlap
        if start > right or end < left:
            return float('-inf')
        
        # Complete overlap
        if start >= left and end <= right:
            return self.tree[node]
        
        # Partial overlap - check both children
        mid = (start + end) // 2
        left_max = self.query(2 * node + 1, start, mid, left, right)
        right_max = self.query(2 * node + 2, mid + 1, end, left, right)
        
        return max(left_max, right_max)

def maxSlidingWindow_segment_tree(nums, k):
    """
    Segment Tree solution for finding maximum in each sliding window.
    
    Is approach mein hum segment tree use karke har window ka maximum efficiently
    find karte hain.
    
    Parameters:
        nums (List[int]): Input array jisme sliding window apply karna hai
        k (int): Sliding window ka size
    
    Returns:
        List[int]: Har sliding window ka maximum element
    
    Time Complexity: O(n log n) - building tree O(n), n-k+1 queries each O(log n)
    Space Complexity: O(n) - for the segment tree
    """
    n = len(nums)
    if n == 0 or k == 0:
        return []
    if k == 1:
        return nums
    
    # Segment tree build karo
    segment_tree = SegmentTree(nums)
    
    result = []
    
    # Har window ka maximum find karo
    for i in range(n - k + 1):
        max_val = segment_tree.query(0, 0, n - 1, i, i + k - 1)
        result.append(max_val)
    
    return result
```

## Solution 4: Dynamic Programming Approach

```python
def maxSlidingWindow_dp(nums, k):
    """
    Dynamic Programming solution for finding maximum in each sliding window.
    
    Is approach mein hum array ko left aur right se process karke maximum values
    calculate karte hain, phir dono ko combine karke window maximums nikalte hain.
    
    Parameters:
        nums (List[int]): Input array jisme sliding window apply karna hai
        k (int): Sliding window ka size
    
    Returns:
        List[int]: Har sliding window ka maximum element
    
    Time Complexity: O(n) - two passes through the array
    Space Complexity: O(n) - for the left and right arrays
    """
    n = len(nums)
    if n == 0 or k == 0:
        return []
    if k == 1:
        return nums
    
    # Left aur right arrays banao
    left = [0] * n
    right = [0] * n
    
    # Left array mein har k-size block ka left se right max calculate karo
    for i in range(n):
        # Block ke start mein direct value assign karo
        if i % k == 0:
            left[i] = nums[i]
        else:
            # Previous max se compare karke update karo
            left[i] = max(left[i-1], nums[i])
    
    # Right array mein har k-size block ka right se left max calculate karo
    for i in range(n-1, -1, -1):
        # Block ke end mein direct value assign karo
        if i % k == k-1 or i == n-1:
            right[i] = nums[i]
        else:
            # Previous max se compare karke update karo
            right[i] = max(right[i+1], nums[i])
    
    result = []
    
    # Har window ka maximum compute karo
    for i in range(n - k + 1):
        # Window ka maximum left end ke right value aur right end ke left value
        # ka maximum hoga
        result.append(max(right[i], left[i+k-1]))
    
    return result
```

## Solution 5: Deque Approach (Most Efficient)

```python
from collections import deque

def maxSlidingWindow_deque(nums, k):
    """
    Deque based solution for finding maximum in each sliding window.
    
    Is approach mein hum ek monotonic decreasing deque maintain karte hain jo
    potential maximum values ke indices store karta hai.
    
    Parameters:
        nums (List[int]): Input array jisme sliding window apply karna hai
        k (int): Sliding window ka size
    
    Returns:
        List[int]: Har sliding window ka maximum element
    
    Time Complexity: O(n) - each element is pushed and popped at most once
    Space Complexity: O(k) - for the deque
    """
    n = len(nums)
    if n == 0 or k == 0:
        return []
    if k == 1:
        return nums
    
    result = []
    dq = deque()  # Deque will store indices
    
    # Process first window
    for i in range(k):
        # Remove smaller elements from deque as they won't be maximum
        while dq and nums[i] >= nums[dq[-1]]:
            dq.pop()
        
        # Add current element index to deque
        dq.append(i)
    
    # Process rest of the windows
    for i in range(k, n):
        # Add maximum of previous window to result
        result.append(nums[dq[0]])
        
        # Remove elements that are out of current window
        while dq and dq[0] <= i - k:
            dq.popleft()
        
        # Remove smaller elements from deque as they won't be maximum
        while dq and nums[i] >= nums[dq[-1]]:
            dq.pop()
        
        # Add current element index to deque
        dq.append(i)
    
    # Add maximum of last window to result
    result.append(nums[dq[0]])
    
    return result
```

## Comparison and Analysis

| Approach | Time Complexity | Space Complexity | Comments |
|---------|----------------|-----------------|---------|
| Brute Force | O(n*k) | O(1) | Simple but inefficient for large k |
| Heap | O(n log k) | O(k) | Better than brute force, good for large elements |
| Segment Tree | O(n log n) | O(n) | Works well for general range query problems |
| Dynamic Programming | O(n) | O(n) | Very efficient, two pass algorithm |
| Deque | O(n) | O(k) | Most efficient solution for this problem |

## Emoji Art Solution Progress

```
Initial array: [1, 3, -1, -3, 5, 3, 6, 7], k=3

📊 📊 📊 📊 📊 📊 📊 📊  <- Original array
1️⃣ 3️⃣ -1️⃣ -3️⃣ 5️⃣ 3️⃣ 6️⃣ 7️⃣  <- Values

🔍 🔍 🔍 ⬜ ⬜ ⬜ ⬜ ⬜  <- Window 1: max = 3
⬜ 🔍 🔍 🔍 ⬜ ⬜ ⬜ ⬜  <- Window 2: max = 3
⬜ ⬜ 🔍 🔍 🔍 ⬜ ⬜ ⬜  <- Window 3: max = -1
⬜ ⬜ ⬜ 🔍 🔍 🔍 ⬜ ⬜  <- Window 4: max = 5
⬜ ⬜ ⬜ ⬜ 🔍 🔍 🔍 ⬜  <- Window 5: max = 5
⬜ ⬜ ⬜ ⬜ ⬜ 🔍 🔍 🔍  <- Window 6: max = 7

✅ Output: [3️⃣, 3️⃣, -1️⃣, 5️⃣, 5️⃣, 6️⃣, 7️⃣]
```

## Conclusion

Sliding Window Maximum problem can be solved with multiple approaches, each with their
own trade-offs. The Deque-based solution provides the most efficient approach with O(n)
time complexity and O(k) space complexity. It maintains a monotonic decreasing queue
that keeps potential maximum elements in order.

For interviews or competitive programming, understanding all these approaches gives you
flexibility based on the constraints of the problem. If you're dealing with very large
arrays with small window sizes, the Deque approach is clearly the best choice.

