# Median of Two Sorted Arrays - LeetCode Problem 4

## Problem Statement
Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return the median of the two sorted arrays.
The overall run time complexity should be O(log (m+n)).

## ASCII Art Visualization of Approaches

### 1. Brute Force Approach: Merge and Find Median

```
Input:      nums1 = [1, 3, 8, 9, 15]     nums2 = [2, 4, 7, 10, 12]
            |                             |
            v                             v
            +-----------------------------+
            |                             |
Merge:      | [1, 2, 3, 4, 7, 8, 9, 10, 12, 15]
            |                             |
            +-------------+---------------+
                          |
                          v
Find Middle:             7
                        / \
                If Even    If Odd
                  |          |
                  v          v
              (7 + 8)/2      7
                  |          
                  v          
                 7.5         
```

### 2. Two Pointers Approach

```
nums1 = [1, 3, 8, 9, 15]     nums2 = [2, 4, 7, 10, 12]
         ^                             ^
      pointer1                      pointer2
         |                             |
         +-----------------------------+
                        |
                        v
                 +-------------+
                 | Compare and |
                 | move forward|
                 +-------------+
                        |
                        v
                 +--------------+
                 | Count until  |
                 | median index |
                 +--------------+
                        |
                        v
                 +-------------+
                 | Return when |
                 | reached pos |
                 +-------------+
```

### 3. Binary Search Approach

```
nums1 = [1, 3, 8, 9, 15]     nums2 = [2, 4, 7, 10, 12]
         |                             |
         v                             v
   +-------------+              +-------------+
   | Binary      |              | Binary      |
   | Partition   |<------------>| Partition   |
   +-------------+              +-------------+
         |                             |
         v                             v
   left_max1 = 3                  left_max2 = 4
   right_min1 = 8                 right_min2 = 7
         |                             |
         +---------------+-------------+
                         |
                         v
                 +---------------+
                 |   Balance     |
                 | partitions    |
                 +---------------+
                         |
                         v
                 +---------------+
                 |  Calculate    |
                 |    median     |
                 +---------------+
```

### 4. Binary Search (Optimal) Approach

```
nums1 = [1, 3, 8, 9, 15]    nums2 = [2, 4, 7, 10, 12]
    ^           ^                ^           ^
    |           |                |           |
  start      middle            start       middle
    |           |                |           |
    v           v                v           v
    +-----------+                +-----------+
    |  Binary   |                |  Binary   |
    |  Search   |                |  Search   |
    +-----------+                +-----------+
         |                             |
         v                             v
   Find partition that satisfies: max(left1,left2) <= min(right1,right2)
         |
         v
  +----------------+
  | Adjust search  |
  | based on       |
  | comparison     |
  +----------------+
         |
         v
  +----------------+
  | Find median    |
  | from partition |
  +----------------+
```

## Solution 1: Brute Force Approach

### Pseudocode
```
function findMedianSortedArrays(nums1, nums2):
    merged = merge(nums1, nums2)
    n = length of merged
    if n is odd:
        return merged[n/2]
    else:
        return (merged[n/2 - 1] + merged[n/2]) / 2
```

### Python Code
```python
def findMedianSortedArrays_brute_force(nums1, nums2):
    """
    Brute force approach to find median of two sorted arrays.
    Time Complexity: O(m+n) - where m and n are lengths of input arrays
    Space Complexity: O(m+n) - for storing merged array
    
    Yeh function dono sorted arrays ko merge karke median nikaalta hai.
    Sabse simple approach hai but memory usage zyada hai aur O(log(m+n)) 
    requirement ko satisfy nahi karta.
    """
    # Dono arrays ko merge karo
    merged = []
    i, j = 0, 0
    
    # Jab tak dono arrays mein elements bache hain
    while i < len(nums1) and j < len(nums2):
        if nums1[i] <= nums2[j]:
            merged.append(nums1[i])
            i += 1
        else:
            merged.append(nums2[j])
            j += 1
    
    # Agar pehle array mein kuch elements bach gaye
    while i < len(nums1):
        merged.append(nums1[i])
        i += 1
    
    # Agar dusre array mein kuch elements bach gaye
    while j < len(nums2):
        merged.append(nums2[j])
        j += 1
    
    # Ab median find karo
    n = len(merged)
    if n % 2 == 1:  # Odd length
        return merged[n // 2]
    else:  # Even length
        return (merged[n // 2 - 1] + merged[n // 2]) / 2.0
```

## Solution 2: Two Pointers Approach

### Pseudocode
```
function findMedianSortedArrays(nums1, nums2):
    total_length = length of nums1 + length of nums2
    median_index = total_length / 2
    
    count = 0
    previous = current = 0
    i = j = 0
    
    while count <= median_index:
        previous = current
        
        if i < length of nums1 and (j >= length of nums2 or nums1[i] <= nums2[j]):
            current = nums1[i]
            i += 1
        else:
            current = nums2[j]
            j += 1
            
        count += 1
    
    if total_length is odd:
        return current
    else:
        return (previous + current) / 2
```

### Python Code
```python
def findMedianSortedArrays_two_pointers(nums1, nums2):
    """
    Two pointers approach to find median of two sorted arrays.
    Time Complexity: O(m+n) - we might need to traverse half of both arrays
    Space Complexity: O(1) - constant extra space
    
    Yeh approach dono arrays ko merge kiye bina median find karta hai.
    Hum sirf median position tak count karte hain aur values track karte hain.
    Isse memory usage kam ho jata hai lekin time complexity abhi bhi O(m+n) hai.
    """
    # Total length nikalo
    total_length = len(nums1) + len(nums2)
    # Median index nikalo
    median_index = total_length // 2
    
    # Pointers dono arrays ke liye
    i, j = 0, 0
    # Previous aur current values track karne ke liye
    previous, current = 0, 0
    # Count kitne elements process ho gaye
    count = 0
    
    # Median index tak traverse karo
    while count <= median_index:
        # Previous value ko update karo
        previous = current
        
        # Decide karo kis array se next element lena hai
        if i < len(nums1) and (j >= len(nums2) or nums1[i] <= nums2[j]):
            current = nums1[i]
            i += 1
        else:
            current = nums2[j]
            j += 1
            
        count += 1
    
    # Median calculate karo
    if total_length % 2 == 1:  # Odd length
        return current
    else:  # Even length
        return (previous + current) / 2.0
```

## Solution 3: Binary Search Approach

### Pseudocode
```
function findMedianSortedArrays(nums1, nums2):
    if length of nums1 > length of nums2:
        swap nums1 and nums2  # Ensure nums1 is smaller

    m = length of nums1
    n = length of nums2
    total = m + n
    half = (total + 1) / 2
    
    low = 0
    high = m
    
    while low <= high:
        partition1 = (low + high) / 2
        partition2 = half - partition1
        
        maxLeft1 = -infinity if partition1 == 0 else nums1[partition1 - 1]
        minRight1 = infinity if partition1 == m else nums1[partition1]
        
        maxLeft2 = -infinity if partition2 == 0 else nums2[partition2 - 1]
        minRight2 = infinity if partition2 == n else nums2[partition2]
        
        if maxLeft1 <= minRight2 and maxLeft2 <= minRight1:
            # Found correct partition
            if total is odd:
                return max(maxLeft1, maxLeft2)
            else:
                return (max(maxLeft1, maxLeft2) + min(minRight1, minRight2)) / 2
        elif maxLeft1 > minRight2:
            high = partition1 - 1
        else:
            low = partition1 + 1
```

### Python Code
```python
def findMedianSortedArrays_binary_search(nums1, nums2):
    """
    Binary search approach to find median of two sorted arrays.
    Time Complexity: O(log(min(m,n))) - binary search on smaller array
    Space Complexity: O(1) - constant extra space
    
    Is approach mein hum chote array par binary search karte hain aur dono
    arrays ko virtually do parts mein divide karte hain aise ki left parts
    ke saare elements right parts se chote hon. Isse time complexity
    O(log(min(m,n))) ho jati hai jo required O(log(m+n)) se better hai.
    """
    # Ensure nums1 is the smaller array for efficient binary search
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1
        
    m, n = len(nums1), len(nums2)
    total = m + n
    half = (total + 1) // 2  # Ceiling for odd case handling
    
    # Binary search range
    low, high = 0, m
    
    while low <= high:
        # Partition points in both arrays
        partition1 = (low + high) // 2
        partition2 = half - partition1
        
        # Edge cases: if partition at start/end, use -inf/inf for comparison
        maxLeft1 = float('-inf') if partition1 == 0 else nums1[partition1 - 1]
        minRight1 = float('inf') if partition1 == m else nums1[partition1]
        
        maxLeft2 = float('-inf') if partition2 == 0 else nums2[partition2 - 1]
        minRight2 = float('inf') if partition2 == n else nums2[partition2]
        
        # Check if we found correct partition
        if maxLeft1 <= minRight2 and maxLeft2 <= minRight1:
            # Found the correct partition, calculate median
            if total % 2 == 1:  # Odd total length
                return max(maxLeft1, maxLeft2)
            else:  # Even total length
                return (max(maxLeft1, maxLeft2) + min(minRight1, minRight2)) / 2.0
        elif maxLeft1 > minRight2:  # Need to move partition1 to left
            high = partition1 - 1
        else:  # Need to move partition1 to right
            low = partition1 + 1
```

## Solution 4: Binary Search (Optimal) Approach

### Pseudocode
```
function findMedianSortedArrays(nums1, nums2):
    if length of nums1 > length of nums2:
        swap nums1 and nums2  # Ensure nums1 is smaller
    
    m = length of nums1
    n = length of nums2
    
    low = 0
    high = m
    
    while low <= high:
        cut1 = (low + high) / 2
        cut2 = (m + n + 1) / 2 - cut1
        
        l1 = -infinity if cut1 == 0 else nums1[cut1 - 1]
        l2 = -infinity if cut2 == 0 else nums2[cut2 - 1]
        r1 = infinity if cut1 == m else nums1[cut1]
        r2 = infinity if cut2 == n else nums2[cut2]
        
        if l1 <= r2 and l2 <= r1:
            if (m + n) is odd:
                return max(l1, l2)
            else:
                return (max(l1, l2) + min(r1, r2)) / 2
        elif l1 > r2:
            high = cut1 - 1
        else:
            low = cut1 + 1
```

### Python Code
```python
def findMedianSortedArrays_binary_search_optimal(nums1, nums2):
    """
    Optimal binary search approach to find median of two sorted arrays.
    Time Complexity: O(log(min(m,n))) - binary search on smaller array
    Space Complexity: O(1) - constant extra space
    
    Yeh approach binary search ka optimized version hai jismein hum chote array
    par binary search karke dono arrays ko aise partition karte hain ki:
    1. Left side mein (m+n+1)/2 elements hon
    2. max(left1, left2) <= min(right1, right2) ho
    Isse median easily calculate ho jata hai aur complexity O(log(min(m,n)))
    rehti hai.
    """
    # Ensure we do binary search on the smaller array
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1
        
    m, n = len(nums1), len(nums2)
    
    # Binary search range
    low, high = 0, m
    
    while low <= high:
        # Calculate cut positions
        cut1 = (low + high) // 2
        cut2 = (m + n + 1) // 2 - cut1
        
        # Handle edge cases with -inf and inf
        l1 = float('-inf') if cut1 == 0 else nums1[cut1 - 1]
        l2 = float('-inf') if cut2 == 0 else nums2[cut2 - 1]
        r1 = float('inf') if cut1 == m else nums1[cut1]
        r2 = float('inf') if cut2 == n else nums2[cut2]
        
        # Check if we found correct partition
        if l1 <= r2 and l2 <= r1:
            # Correct partition found, calculate median
            if (m + n) % 2 != 0:  # Odd length
                return max(l1, l2)
            else:  # Even length
                return (max(l1, l2) + min(r1, r2)) / 2.0
        elif l1 > r2:  # Move cut1 to left
            high = cut1 - 1
        else:  # Move cut1 to right
            low = cut1 + 1

    # This should never be reached if arrays are sorted
    return 0.0
```

## Time and Space Complexity Analysis

| Approach | Time Complexity | Space Complexity |
|----------|----------------|-----------------|
| Brute Force | O(m+n) | O(m+n) |
| Two Pointers | O(m+n) | O(1) |
| Binary Search | O(log(min(m,n))) | O(1) |
| Binary Search (Optimal) | O(log(min(m,n))) | O(1) |

## Conclusion

Binary search approaches (3 and 4) are the most efficient, achieving 
O(log(min(m,n))) time complexity, which satisfies the required O(log(m+n)) 
complexity since log(min(m,n)) <= log(m+n). The optimal binary search approach 
is particularly elegant as it directly partitions the arrays to find the median 
without merging them.

