# Find Minimum in Rotated Sorted Array - Visual Solution

## Problem Statement

Given a sorted array that has been rotated between 1 and n times, find the minimum element in O(log n) time.

## Visual Representation of a Rotated Array

```
Original Sorted Array:  [0, 1, 2, 4, 5, 6, 7]
                         ↑              
                       minimum
                       
After 4 rotations:     [4, 5, 6, 7, 0, 1, 2]
                                  ↑
                               minimum
```

## ASCII Art Visualization: Finding Minimum Element in Rotated Array

### Key Insight: The Inflection Point

```
    Value
    ↑
7   |           *
6   |         *
5   |       *
4   |     *
3   |
2   |                   *
1   |                 *
0   |               *
    +-------------------→ Index
        0 1 2 3 4 5 6

    Inflection point (minimum value) is where the "cliff" appears
```

## Algorithm Evolution 

### 1. Brute Force Approach

```
Iteration 1: [4, 5, 6, 7, 0, 1, 2]
             ↑  Looking at first element (4)

Iteration 2: [4, 5, 6, 7, 0, 1, 2]
                ↑  Looking at second element (5)
                
...

Iteration 5: [4, 5, 6, 7, 0, 1, 2]
                         ↑  Found minimum! (0)
```

### 2. Binary Search Approach

```
          [4, 5, 6, 7, 0, 1, 2]
           L        M        R
           
Is mid > right? No, so minimum is in left half (including mid)

          [4, 5, 6, 7]
           L  M     R
           
Is mid > right? No, so minimum is in left half (including mid)

          [4, 5]
           L  R
           
Is left > right? No, so left is minimum of this subarray (4)

But comparing with overall array's results: 0 < 4, so 0 is minimum
```

### 3. Binary Search (Lower Bound) Approach

```
          [4, 5, 6, 7, 0, 1, 2]
           L        M        R
           
Is mid (7) >= first element (4)? Yes, search right half

          [0, 1, 2]
           L  M  R
           
Is mid (1) >= first element (4)? No, search left half

          [0]
           LMR
           
Single element found: 0 is the minimum!
```

## Solutions in Code

### 1. Brute Force Solution

```python
def findMin_brute_force(nums):
    """
    Brute force solution to find minimum element in rotated sorted array.
    Time Complexity: O(n) - We simply iterate through the entire array once
    Space Complexity: O(1) - We use constant extra space
    
    Kaise kaam karta hai (How it works):
    - Pure array ko ek baar scan karte hain aur minimum element dhundhte hain
    - Simple hai magar O(log n) requirement ko satisfy nahi karta
    """
    min_element = float('inf')  # Initialize with positive infinity
    
    for num in nums:
        if num < min_element:
            min_element = num
    
    return min_element
```

### 2. Binary Search Solution

```python
def findMin_binary_search(nums):
    """
    Binary search solution to find minimum element in rotated sorted array.
    Time Complexity: O(log n) - We divide the search space in half each time
    Space Complexity: O(1) - We use constant extra space
    
    Kaise kaam karta hai (How it works):
    - Binary search ka use karke inflection point ko dhundho
    - Agar mid right se chota hai, to minimum left side mein hai
    - Agar mid right se bada hai, to minimum right side mein hai
    - Edge case: agar array rotate hi nahi hui, to first element minimum hai
    """
    left, right = 0, len(nums) - 1
    
    # Special case: array is not rotated or has single element
    if nums[left] < nums[right] or left == right:
        return nums[left]
        
    while left < right:
        mid = left + (right - left) // 2
        
        # If mid element is greater than its next element, we found the minimum
        if mid < len(nums) - 1 and nums[mid] > nums[mid + 1]:
            return nums[mid + 1]
            
        # If mid element is less than its previous element, mid is the minimum
        if mid > 0 and nums[mid - 1] > nums[mid]:
            return nums[mid]
            
        # If mid element is greater than left element, min is in right half
        if nums[mid] > nums[left]:
            if nums[mid] > nums[right]:
                left = mid + 1
            else:
                right = mid - 1
        # If mid element is less than left element, min is in left half
        else:
            right = mid - 1
            
    return nums[left]
```

### 3. Binary Search (Lower Bound) Solution - Most Elegant

```python
def findMin_binary_search_lower_bound(nums):
    """
    Optimized binary search solution for finding minimum in rotated sorted array.
    Time Complexity: O(log n) - We divide the search space in half each time
    Space Complexity: O(1) - We use constant extra space
    
    Kaise kaam karta hai (How it works):
    - Compare mid with right element instead of left to determine search direction
    - Agar mid right se chota ya equal hai, to minimum mid tak ya left side mein hai
    - Agar mid right se bada hai, to minimum right side mein hai
    - Yeh approach sorted rotated array ke liye specially designed hai
    """
    left, right = 0, len(nums) - 1
    
    while left < right:
        mid = left + (right - left) // 2
        
        # If mid element > right element, minimum is in the right half
        if nums[mid] > nums[right]:
            left = mid + 1
        # If mid element <= right element, minimum is in left half (including mid)
        else:
            right = mid
            
    # When left == right, we've found the minimum element
    return nums[left]
```

## Visual Comparison of All Three Approaches

```
┌────────────────────────────────┬─────────────────────┬─────────────────────┐
│ Approach                       │ Time Complexity     │ Space Complexity    │
├────────────────────────────────┼─────────────────────┼─────────────────────┤
│ 1. Brute Force                 │ O(n)                │ O(1)                │
│                                │                     │                     │
│    [4, 5, 6, 7, 0, 1, 2]       │    n comparisons    │    Simple!          │
│     ↓  ↓  ↓  ↓  ↓  ↓  ↓        │                     │                     │
│    Check each element one      │                     │                     │
│    by one                      │                     │                     │
├────────────────────────────────┼─────────────────────┼─────────────────────┤
│ 2. Binary Search               │ O(log n)            │ O(1)                │
│                                │                     │                     │
│    [4, 5, 6, 7, 0, 1, 2]       │    log n decisions  │    No extra space!  │
│     L     M        R           │                     │                     │
│    Use inflection property     │                     │                     │
│    to narrow search            │                     │                     │
├────────────────────────────────┼─────────────────────┼─────────────────────┤
│ 3. Binary Search (Lower Bound) │ O(log n)            │ O(1)                │
│                                │                     │                     │
│    [4, 5, 6, 7, 0, 1, 2]       │    log n decisions  │    No extra space!  │
│     L     M        R           │                     │                     │
│    Compare mid with right      │                     │                     │
│    More elegant solution       │                     │                     │
└────────────────────────────────┴─────────────────────┴─────────────────────┘
```

## Final Algorithm Decision Tree:

```
                         Start
                           │
                           ▼
             ┌─────────────────────────┐
             │ Initialize left & right │
             └─────────────────────────┘
                           │
                           ▼
                     ┌──────────┐
                     │left<right│───No──┐
                     └──────────┘       │
                           │            │
                          Yes           │
                           │            │
                           ▼            │
             ┌─────────────────────────┐│
             │ Calculate mid point     ││
             └─────────────────────────┘│
                           │            │
                           ▼            │
             ┌─────────────────────────┐│
             │ nums[mid] > nums[right]?││
             └─────────────────────────┘│
                  /            \        │
                 /              \       │
              Yes               No      │
               /                  \     │
              ▼                    ▼    │
    ┌──────────────────┐  ┌──────────────────┐
    │left = mid + 1    │  │right = mid       │
    └──────────────────┘  └──────────────────┘
              │                    │
              └─────────┬──────────┘
                        │
                        │◄────────────┘
                        │
                        ▼
           ┌─────────────────────────┐
           │ Return nums[left]       │
           └─────────────────────────┘
                        │
                        ▼
                       End
```

## Conclusion:

While all three approaches work, the Binary Search (Lower Bound) solution is the most elegant and efficient. It satisfies the O(log n) time complexity requirement while being simpler to implement than the standard binary search approach. The key insight is identifying the "inflection point" where the normally ascending array suddenly drops - this is where our minimum element is located.

