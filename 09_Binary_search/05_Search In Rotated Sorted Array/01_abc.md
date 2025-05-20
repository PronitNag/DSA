# Search in Rotated Sorted Array - Solutions with Visualizations

## Problem Statement

There is an integer array nums sorted in ascending order (with distinct values).

Prior to being passed to your function, nums is possibly rotated at an unknown pivot index k (1 <= k < nums.length) such that the resulting array is [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]] (0-indexed). For example, [0,1,2,4,5,6,7] might be rotated at pivot index 3 and become [4,5,6,7,0,1,2].

Given the array nums after the possible rotation and an integer target, return the index of target if it is in nums, or -1 if it is not in nums.

You must write an algorithm with O(log n) runtime complexity.

## ASCII Art Visualization of Rotated Array

```
Original Array (Sorted):
    0   1   2   3   4   5   6
  +---+---+---+---+---+---+---+
  | 0 | 1 | 2 | 4 | 5 | 6 | 7 |  <-- Sorted in ascending order
  +---+---+---+---+---+---+---+
    ^                       ^
    |                       |
  Start                    End

After Rotation (k=3):
    0   1   2   3   4   5   6
  +---+---+---+---+---+---+---+
  | 4 | 5 | 6 | 7 | 0 | 1 | 2 |  <-- Rotated at pivot k=3
  +---+---+---+---+---+---+---+
    ^       ^   ^       ^
    |       |   |       |
  Start   Pivot|      End
              Largest

The special property: One half is always sorted!
```

## Solution 1: Brute Force (O(n))

This is the simplest approach - just scan the entire array for the target element.

### Visualization

```
Input Array: [4, 5, 6, 7, 0, 1, 2], Target = 0

Step 1: Check nums[0] = 4 ≠ 0 ❌
    [4, 5, 6, 7, 0, 1, 2]
     ↑
     Currently checking

Step 2: Check nums[1] = 5 ≠ 0 ❌
    [4, 5, 6, 7, 0, 1, 2]
        ↑
        Currently checking

Step 3: Check nums[2] = 6 ≠ 0 ❌
    [4, 5, 6, 7, 0, 1, 2]
           ↑
           Currently checking

Step 4: Check nums[3] = 7 ≠ 0 ❌
    [4, 5, 6, 7, 0, 1, 2]
              ↑
              Currently checking

Step 5: Check nums[4] = 0 = 0 ✅
    [4, 5, 6, 7, 0, 1, 2]
                 ↑
                 Found! Return index 4
```

### Pseudo Code

```
function search(nums, target):
    for i from 0 to length(nums)-1:
        if nums[i] equals target:
            return i
    return -1
```

### Python Code - Brute Force

```python
def search_brute_force(nums, target):
    """
    Search for target in rotated sorted array using brute force approach.
    
    Yeh function target ko dhoondhne ke liye array ko ek-ek element check karta hai.
    Time Complexity: O(n) - kyunki worst case mein pura array check karna pad
    sakta hai.
    Space Complexity: O(1) - koi extra space nahi use karta.
    
    Args:
        nums: Rotated sorted array with distinct values
        target: Element to search for
        
    Returns:
        Index of target if found, -1 otherwise
    """
    for i in range(len(nums)):
        if nums[i] == target:
            return i
    return -1
```

## Solution 2: Binary Search (One Pass)

This is the most efficient approach - use binary search by determining which half of the array is sorted and if the target could be in that half.

### Visualization

```
Input Array: [4, 5, 6, 7, 0, 1, 2], Target = 0

Step 1: Initialize left=0, right=6, mid=3
    [4, 5, 6, 7, 0, 1, 2]
     ↑       ↑       ↑
     L       M       R
    
    nums[mid] = 7
    Is 7 > 4 (nums[left])? Yes, so left half [4,5,6,7] is sorted
    Is target (0) in range [4,7]? No, so search in right half

Step 2: Update left=mid+1=4, right=6, mid=5
    [4, 5, 6, 7, 0, 1, 2]
                 ↑   ↑   ↑
                 L   M   R
    
    nums[mid] = 1
    Is 1 > 0 (nums[left])? Yes, so left half [0,1] is sorted
    Is target (0) in range [0,1]? Yes, so search in left half

Step 3: Update left=4, right=mid-1=4, mid=4
    [4, 5, 6, 7, 0, 1, 2]
                 ↑
                L,M,R
    
    nums[mid] = 0 = target ✅
    Return mid = 4
```

### Pseudo Code

```
function search(nums, target):
    left = 0
    right = length(nums) - 1
    
    while left <= right:
        mid = (left + right) / 2
        
        if nums[mid] equals target:
            return mid
            
        # Check if left half is sorted
        if nums[left] <= nums[mid]:
            # Check if target is in left half
            if nums[left] <= target < nums[mid]:
                right = mid - 1
            else:
                left = mid + 1
        # Right half is sorted
        else:
            # Check if target is in right half
            if nums[mid] < target <= nums[right]:
                left = mid + 1
            else:
                right = mid - 1
                
    return -1
```

### Python Code - Binary Search (One Pass)

```python
def search_binary_one_pass(nums, target):
    """
    Search for target in rotated sorted array using one-pass binary search.
    
    Yeh function ek hi baar mein binary search ka use karke target ko dhoondhta hai.
    Hamesha check karta hai ki array ka kaun sa half sorted hai, aur target us part 
    mein ho sakta hai ya nahi.
    Time Complexity: O(log n) - binary search ki wajah se
    Space Complexity: O(1) - koi extra space nahi use karta
    
    Args:
        nums: Rotated sorted array with distinct values
        target: Element to search for
        
    Returns:
        Index of target if found, -1 otherwise
    """
    left, right = 0, len(nums) - 1
    
    while left <= right:
        mid = (left + right) // 2
        
        # Target mil gaya
        if nums[mid] == target:
            return mid
        
        # Check karo left half sorted hai ya nahi
        if nums[left] <= nums[mid]:
            # Agar target left sorted half mein hai
            if nums[left] <= target < nums[mid]:
                right = mid - 1
            else:
                left = mid + 1
        # Right half sorted hoga
        else:
            # Agar target right sorted half mein hai
            if nums[mid] < target <= nums[right]:
                left = mid + 1
            else:
                right = mid - 1
                
    return -1
```

## Solution 3: Binary Search (Two Pass)

First find the pivot (smallest element) using binary search, then perform a regular binary search in the appropriate section.

### Visualization

```
Input Array: [4, 5, 6, 7, 0, 1, 2], Target = 0

PASS 1: Find the pivot (smallest element) index

Step 1: Initialize left=0, right=6, mid=3
    [4, 5, 6, 7, 0, 1, 2]
     ↑       ↑       ↑
     L       M       R
    
    nums[mid] = 7 > nums[right] = 2
    So pivot is in right half

Step 2: Update left=mid+1=4, right=6, mid=5
    [4, 5, 6, 7, 0, 1, 2]
                 ↑   ↑   ↑
                 L   M   R
    
    nums[mid] = 1 < nums[right] = 2
    But nums[mid-1] = 0 < nums[mid] = 1
    Need to check more

Step 3: Update left=4, right=mid=5, mid=4
    [4, 5, 6, 7, 0, 1, 2]
                 ↑   ↑
                L,M  R
    
    nums[mid] = 0 < nums[mid-1] = 7 (out of order!)
    Pivot found at index 4!

PASS 2: Perform regular binary search with adjusted indices

Step 4: Target = 0, now we have pivot = 4
    Since target = nums[pivot], return pivot = 4 ✅
```

### Pseudo Code

```
function findPivot(nums):
    left = 0
    right = length(nums) - 1
    
    # Edge case: already sorted array
    if nums[left] < nums[right]:
        return 0
        
    while left <= right:
        mid = (left + right) / 2
        
        # Found pivot if next element is smaller
        if mid < length(nums)-1 and nums[mid] > nums[mid+1]:
            return mid + 1
        # Found pivot if current element is smaller than previous
        if mid > 0 and nums[mid] < nums[mid-1]:
            return mid
            
        # Decide which half to search
        if nums[mid] > nums[0]:
            left = mid + 1
        else:
            right = mid - 1
            
    return 0

function search(nums, target):
    pivot = findPivot(nums)
    
    # If target is the pivot element
    if nums[pivot] == target:
        return pivot
        
    n = length(nums)
    left = 0
    right = n - 1
    
    # Decide which half to search based on target and pivot
    if pivot > 0 and target >= nums[0] and target <= nums[pivot-1]:
        # Search in left half (before pivot)
        right = pivot - 1
    else:
        # Search in right half (after pivot)
        left = pivot
        
    # Standard binary search
    while left <= right:
        mid = (left + right) / 2
        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
            
    return -1
```

### Python Code - Binary Search (Two Pass)

```python
def search_binary_two_pass(nums, target):
    """
    Search for target in rotated sorted array using two-pass binary search.
    
    Yeh function do steps mein kaam karta hai:
    1. Pehle pivot element (smallest element) ko find karta hai
    2. Fir standard binary search apply karta hai appropriate section mein
    
    Time Complexity: O(log n) - dono passes binary search use karte hain
    Space Complexity: O(1) - koi extra space nahi use karta
    
    Args:
        nums: Rotated sorted array with distinct values
        target: Element to search for
        
    Returns:
        Index of target if found, -1 otherwise
    """
    # Edge case: empty array
    if not nums:
        return -1
        
    # First pass: find the pivot index (smallest element)
    def find_pivot():
        left, right = 0, len(nums) - 1
        
        # If array is not rotated
        if nums[left] <= nums[right]:
            return 0
            
        while left <= right:
            mid = (left + right) // 2
            
            # Mid element is greater than next element
            if mid < len(nums) - 1 and nums[mid] > nums[mid + 1]:
                return mid + 1
            # Mid element is less than previous element
            if mid > 0 and nums[mid] < nums[mid - 1]:
                return mid
                
            # Decide which half to search
            if nums[mid] >= nums[0]:
                left = mid + 1  # Pivot is in right half
            else:
                right = mid - 1  # Pivot is in left half
                
        return 0  # Should not reach here
    
    pivot = find_pivot()
    
    # Second pass: regular binary search in the correct portion
    def binary_search(left, right):
        while left <= right:
            mid = (left + right) // 2
            if nums[mid] == target:
                return mid
            elif nums[mid] < target:
                left = mid + 1
            else:
                right = mid - 1
        return -1
    
    # Decide which portion to search
    if pivot == 0 or target < nums[0]:
        # Search in right portion (after pivot)
        return binary_search(pivot, len(nums) - 1)
    else:
        # Search in left portion (before pivot)
        return binary_search(0, pivot - 1)
```

## Solution 4: Modified Binary Search (Alternative Approach)

This approach uses binary search but with a different condition for determining which half to search.

### Visualization

```
Input Array: [4, 5, 6, 7, 0, 1, 2], Target = 0

Step 1: Initialize left=0, right=6, mid=3
    [4, 5, 6, 7, 0, 1, 2]
     ↑       ↑       ↑
     L       M       R
    
    nums[mid] = 7, target = 0
    nums[mid] > nums[right]: Array is rotated, pivot is in right half
    target <= nums[right]: Target is in the right half (after rotation)
    Search in right half
    
Step 2: Update left=mid+1=4, right=6, mid=5
    [4, 5, 6, 7, 0, 1, 2]
                 ↑   ↑   ↑
                 L   M   R
    
    nums[mid] = 1, target = 0
    nums[mid] <= nums[right]: Right half is sorted
    target < nums[mid]: Target is to the left of mid
    Search in left half
    
Step 3: Update left=4, right=mid-1=4, mid=4
    [4, 5, 6, 7, 0, 1, 2]
                 ↑
                L,M,R
    
    nums[mid] = 0 = target ✅
    Return mid = 4
```

### Pseudo Code

```
function search(nums, target):
    left = 0
    right = length(nums) - 1
    
    while left <= right:
        mid = (left + right) / 2
        
        if nums[mid] equals target:
            return mid
            
        # Check if right half is sorted
        if nums[mid] <= nums[right]:
            # Target is in the sorted right half
            if nums[mid] < target <= nums[right]:
                left = mid + 1
            else:
                right = mid - 1
        # Left half is sorted
        else:
            # Target is in the sorted left half
            if nums[left] <= target < nums[mid]:
                right = mid - 1
            else:
                left = mid + 1
                
    return -1
```

### Python Code - Modified Binary Search

```python
def search_modified_binary(nums, target):
    """
    Search for target in rotated sorted array using modified binary search.
    
    Yeh approach binary search ka variation hai, jisme hum compare karte hain ki
    right half sorted hai ya left half, aur uske basis par decision lete hain.
    
    Time Complexity: O(log n) - binary search ki wajah se
    Space Complexity: O(1) - koi extra space nahi use karta
    
    Args:
        nums: Rotated sorted array with distinct values
        target: Element to search for
        
    Returns:
        Index of target if found, -1 otherwise
    """
    left, right = 0, len(nums) - 1
    
    while left <= right:
        mid = (left + right) // 2
        
        if nums[mid] == target:
            return mid
        
        # Check if right half is sorted
        if nums[mid] <= nums[right]:
            # Target is in sorted right half
            if nums[mid] < target <= nums[right]:
                left = mid + 1
            else:
                right = mid - 1
        # Left half must be sorted
        else:
            # Target is in sorted left half
            if nums[left] <= target < nums[mid]:
                right = mid - 1
            else:
                left = mid + 1
                
    return -1
```

## Comparison of Solutions with Emoji Visualization

```
🔍 Brute Force: O(n)
   [4, 5, 6, 7, 0, 1, 2]
    👀 → → → → 🎯 
    Linear scan until found
    
🔍🔍 Binary Search (One Pass): O(log n)
   [4, 5, 6, 7, 0, 1, 2]
    ⬅️  🔎  ➡️   ✂️
    Checks which half is sorted and makes decision
    
🔄🔍 Binary Search (Two Pass): O(log n)
   [4, 5, 6, 7, 0, 1, 2]
    🔄 Find pivot first: 0 at index 4
    Then 🔍 regular binary search
    
🔀🔍 Modified Binary Search: O(log n)
   [4, 5, 6, 7, 0, 1, 2]
    ⬅️  🔎  ➡️   ✂️
    Alternative condition to determine search half
```

## Time and Space Complexity Analysis

| Solution              | Time Complexity | Space Complexity | Comments                                   |
|-----------------------|----------------|-----------------|-------------------------------------------|
| Brute Force           | O(n)           | O(1)            | Simple but inefficient for large arrays    |
| Binary Search (One Pass) | O(log n)    | O(1)            | Most efficient, single pass               |
| Binary Search (Two Pass) | O(log n)    | O(1)            | Two binary searches, still O(log n)        |
| Modified Binary Search   | O(log n)    | O(1)            | Alternative approach, same complexity     |

## Conclusion

The binary search one-pass solution is the most elegant and efficient for this problem, achieving the required O(log n) time complexity. It works by identifying which half of the array is sorted and using that information to determine if the target could be in that half.
