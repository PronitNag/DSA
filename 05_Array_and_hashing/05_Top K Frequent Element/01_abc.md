# Top K Frequent Elements - Solution with ASCII Art

## Problem Statement

Given an integer array `nums` and an integer `k`, return the `k` most frequent elements within the array.

### Example:
```
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
```

## Solution 1: Using Sorting Approach

### ASCII Art Visualization:

```
Input: [1,1,1,2,2,3], k = 2

Step 1: Count frequencies
┌───────────────────┐
│ Count Frequencies │
└───────────────────┘
       ↓
┌───┬───┐
│ 1 │ 3 │  <- Element 1 appears 3 times
├───┼───┤
│ 2 │ 2 │  <- Element 2 appears 2 times
├───┼───┤
│ 3 │ 1 │  <- Element 3 appears 1 time
└───┴───┘

Step 2: Sort by frequency (descending)
┌──────────────────────┐
│ Sort by Frequencies  │
└──────────────────────┘
       ↓
┌───┬───┐  ┌───┬───┐  ┌───┬───┐
│ 1 │ 3 │  │ 2 │ 2 │  │ 3 │ 1 │
└───┴───┘  └───┴───┘  └───┴───┘
    ↓          ↓          ↓
┌───────────────────────────┐
│      Sort Descending      │
└───────────────────────────┘
       ↓
┌───┬───┐
│ 1 │ 3 │  <- Highest frequency
├───┼───┤
│ 2 │ 2 │  <- Second highest
├───┼───┤
│ 3 │ 1 │
└───┴───┘

Step 3: Take top k elements (k=2)
┌───────────────────┐
│ Take top k items  │
└───────────────────┘
       ↓
┌───┬───┐
│ 1 │ 3 │  ← Include
├───┼───┤
│ 2 │ 2 │  ← Include
└───┴───┘

Result: [1, 2]  ✓
```

### Pseudocode:
```
function topKFrequent(nums, k):
    // Step 1: Count frequencies of each element
    frequency = {}
    for each num in nums:
        frequency[num] = frequency.getOrDefault(num, 0) + 1
    
    // Step 2: Sort elements by frequency (descending)
    sortedElements = sort elements in frequency based on their values in descending order
    
    // Step 3: Return top k elements
    return first k elements from sortedElements
```

### Python Solution:
```python
def topKFrequent(nums, k):
    """
    Sorting approach to find top k frequent elements.
    
    Args:
        nums: Input array of integers
        k: Number of most frequent elements to return
        
    Returns:
        List of k most frequent elements
    """
    # Step 1: Count frequencies of each element (frequency map banao)
    freq_map = {}
    for num in nums:
        freq_map[num] = freq_map.get(num, 0) + 1
    
    # Step 2: Sort elements by frequency in descending order (frequency ke hisaab se sort karo)
    sorted_elements = sorted(freq_map.keys(), key=lambda x: freq_map[x], reverse=True)
    
    # Step 3: Return top k elements (top k elements return karo)
    return sorted_elements[:k]
```

## Solution 2: Using Min-Heap Approach

### ASCII Art Visualization:

```
Input: [1,1,1,2,2,3], k = 2

Step 1: Count frequencies
┌───────────────────┐
│ Count Frequencies │
└───────────────────┘
       ↓
┌───┬───┐
│ 1 │ 3 │
├───┼───┤
│ 2 │ 2 │
├───┼───┤
│ 3 │ 1 │
└───┴───┘

Step 2: Build min-heap of size k
┌───────────────────────┐
│ Build Min-Heap (k=2)  │
└───────────────────────┘
       ↓
       Initially empty
       ↓
       Add (1,3)
    ┌───┬───┐
    │ 1 │ 3 │
    └───┴───┘
       ↓
       Add (2,2)
    ┌───┬───┐
    │ 2 │ 2 │
    └───┴───┘
    ┌───┬───┐
    │ 1 │ 3 │
    └───┴───┘
       ↓
       Add (3,1) and pop smallest freq
    ┌───┬───┐          ┌───┬───┐
    │ 3 │ 1 │   ===>   │ 1 │ 3 │
    └───┴───┘          └───┴───┘
    ┌───┬───┐          ┌───┬───┐
    │ 2 │ 2 │          │ 2 │ 2 │
    └───┴───┘          └───┴───┘
       ↓
Final heap has elements: (2,2) and (1,3)

Result: [1, 2]  ✓
```

### Pseudocode:
```
function topKFrequent(nums, k):
    // Step 1: Count frequencies of each element
    frequency = {}
    for each num in nums:
        frequency[num] = frequency.getOrDefault(num, 0) + 1
    
    // Step 2: Use min-heap of size k
    minHeap = new MinHeap based on frequencies
    
    for each (element, freq) in frequency:
        if minHeap.size < k:
            minHeap.add((element, freq))
        else if freq > minHeap.top.freq:
            minHeap.pop()
            minHeap.add((element, freq))
    
    // Step 3: Extract elements from heap
    result = []
    while not minHeap.isEmpty():
        result.add(minHeap.pop().element)
    
    return result
```

### Python Solution:
```python
import heapq

def topKFrequent(nums, k):
    """
    Min-heap approach to find top k frequent elements.
    
    Args:
        nums: Input array of integers
        k: Number of most frequent elements to return
        
    Returns:
        List of k most frequent elements
    """
    # Step 1: Count frequencies (frequency map banao)
    freq_map = {}
    for num in nums:
        freq_map[num] = freq_map.get(num, 0) + 1
    
    # Step 2: Use a min heap of size k (k size ka min heap banao)
    # We store (frequency, element) pairs for proper min heap ordering
    min_heap = []
    
    for num, freq in freq_map.items():
        # Agar heap ka size k se chota hai, toh direct push karo
        if len(min_heap) < k:
            heapq.heappush(min_heap, (freq, num))
        # Agar current frequency heap ke top se badi hai, toh replace karo
        elif freq > min_heap[0][0]:
            heapq.heappop(min_heap)
            heapq.heappush(min_heap, (freq, num))
    
    # Step 3: Extract elements from heap (heap se elements nikalo)
    result = [num for freq, num in min_heap]
    
    return result
```

## Solution 3: Using Bucket Sort Approach

### ASCII Art Visualization:
```
Input: [1,1,1,2,2,3], k = 2

Step 1: Count frequencies
┌───────────────────┐
│ Count Frequencies │
└───────────────────┘
       ↓
┌───┬───┐
│ 1 │ 3 │
├───┼───┤
│ 2 │ 2 │
├───┼───┤
│ 3 │ 1 │
└───┴───┘

Step 2: Create buckets (indexed by frequency)
┌────────────────────┐
│ Create Frequency   │
│ Buckets (0 to n)   │
└────────────────────┘
       ↓
┌───┐  ┌───┐  ┌───┐  ┌───┐
│ 0 │  │ 1 │  │ 2 │  │ 3 │  <- Frequencies
└───┘  └───┘  └───┘  └───┘
Empty   [3]    [2]    [1]   <- Elements with that frequency

Step 3: Scan buckets from highest frequency
┌────────────────────────┐
│ Collect from highest   │
│ frequency buckets      │
└────────────────────────┘
       ↓
       Start at highest frequency
       ↓
┌───┐  ┌───┐  ┌───┐  ┌───┐
│ 0 │  │ 1 │  │ 2 │  │ 3 │
└───┘  └───┘  └───┘  └───┘
Empty   [3]    [2]    [1]
                       ↑
                    Start here
       ↓
       Take element 1 (k=1 left)
       ↓
       Go to next bucket
       ↓
┌───┐  ┌───┐  ┌───┐  ┌───┐
│ 0 │  │ 1 │  │ 2 │  │ 3 │
└───┘  └───┘  └───┘  └───┘
Empty   [3]    [2]    [1]
               ↑
         Take element 2
         (k=0, done)

Result: [1, 2]  ✓
```

### Pseudocode:
```
function topKFrequent(nums, k):
    // Step 1: Count frequencies of each element
    frequency = {}
    for each num in nums:
        frequency[num] = frequency.getOrDefault(num, 0) + 1
    
    // Step 2: Create buckets (lists) where index is frequency
    buckets = array of empty lists with size (n+1)
    for each (element, freq) in frequency:
        buckets[freq].add(element)
    
    // Step 3: Scan buckets from highest frequency
    result = []
    for i from n down to 0:
        for each element in buckets[i]:
            result.add(element)
            if result.size == k:
                return result
```

### Python Solution:
```python
def topKFrequent(nums, k):
    """
    Bucket sort approach to find top k frequent elements.
    
    Args:
        nums: Input array of integers
        k: Number of most frequent elements to return
        
    Returns:
        List of k most frequent elements
    """
    # Step 1: Count frequencies (frequency map banao)
    freq_map = {}
    for num in nums:
        freq_map[num] = freq_map.get(num, 0) + 1
    
    # Step 2: Create buckets where index = frequency (frequency ke hisaab se buckets banao)
    # Max frequency can be the length of the array
    buckets = [[] for _ in range(len(nums) + 1)]
    
    # Put elements in their frequency buckets
    for num, freq in freq_map.items():
        buckets[freq].append(num)
    
    # Step 3: Scan buckets from highest frequency to lowest (highest frequency se elements collect karo)
    result = []
    for i in range(len(buckets) - 1, 0, -1):
        for num in buckets[i]:
            result.append(num)
            if len(result) == k:
                return result
    
    return result  # This should never be reached if k is valid
```

## Time & Space Complexity Comparison

```
           | Time Complexity | Space Complexity |
-----------------------------------------------
Sorting    | O(n log n)     | O(n)             |
-----------------------------------------------
Min-Heap   | O(n log k)     | O(n + k)         |
-----------------------------------------------
Bucket Sort| O(n)           | O(n)             |
-----------------------------------------------
```

## 💡 Conclusion:

The bucket sort approach is the most efficient with O(n) time complexity, making it the best solution for large datasets. Min-heap is a good compromise when memory is a concern, and sorting is the most straightforward implementation but least efficient for large inputs.

🏆 **Winner: Bucket Sort Approach**

```
🎯 Bucket Sort (Best Solution) 🎯
💨 Linear Time - O(n)
🧠 Linear Space - O(n)
⚡ No sorting required!
```
