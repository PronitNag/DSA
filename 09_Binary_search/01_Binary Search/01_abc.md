# Binary Search Solutions with Visualizations

## Problem Statement

**704. Binary Search**

Given an array of integers `nums` which is sorted in ascending order, and an integer `target`, write a function to search `target` in `nums`. If `target` exists, then return its index. Otherwise, return `-1`.
You must write an algorithm with `O(log n)` runtime complexity.

## Binary Search Visualization with ASCII Art

```
◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️
         Binary Search Visualization - Finding target = 15
◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️◼️

Initial Array:  [1, 3, 5, 7, 9, 11, 13, 15, 17, 19, 21]
                 0  1  2  3  4   5   6   7   8   9  10   (indices)

Step 1: Set left = 0, right = 10
        Find mid = (0 + 10) // 2 = 5
        mid element = nums[5] = 11

        [1, 3, 5, 7, 9, 11, 13, 15, 17, 19, 21]
         L        ↑   M             ↑        R
                 11 < 15, so search right half

Step 2: Set left = 6, right = 10
        Find mid = (6 + 10) // 2 = 8
        mid element = nums[8] = 17

        [1, 3, 5, 7, 9, 11, 13, 15, 17, 19, 21]
                         L    ↑    M    ↑    R
                             15 < 17, so search left half

Step 3: Set left = 6, right = 7
        Find mid = (6 + 7) // 2 = 6
        mid element = nums[6] = 13

        [1, 3, 5, 7, 9, 11, 13, 15, 17, 19, 21]
                         L↑   M R
                        13 < 15, so search right half

Step 4: Set left = 7, right = 7
        Find mid = (7 + 7) // 2 = 7
        mid element = nums[7] = 15

        [1, 3, 5, 7, 9, 11, 13, 15, 17, 19, 21]
                             L↑   
                             M↑   
                             R↑   
                        15 == 15, Target found!

Result: Return index 7
```

## Binary Search Visualization with Emoji Art

```
🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍 BINARY SEARCH EMOJI VISUALIZATION 🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍🔍

Initial Array: 
📊 [1️⃣, 3️⃣, 5️⃣, 7️⃣, 9️⃣, 1️⃣1️⃣, 1️⃣3️⃣, 1️⃣5️⃣, 1️⃣7️⃣, 1️⃣9️⃣, 2️⃣1️⃣]
    0️⃣  1️⃣  2️⃣  3️⃣  4️⃣   5️⃣    6️⃣    7️⃣    8️⃣    9️⃣   1️⃣0️⃣

🎯 Target = 1️⃣5️⃣

Step 1: 👇  👇                      🔎                        👇
       [ 1️⃣, 3️⃣, 5️⃣, 7️⃣, 9️⃣, 1️⃣1️⃣, 1️⃣3️⃣, 1️⃣5️⃣, 1️⃣7️⃣, 1️⃣9️⃣, 2️⃣1️⃣]
         L                   M                           R
       ❓ 1️⃣1️⃣ < 1️⃣5️⃣ (mid < target)
       ✅ Move left pointer to mid + 1 (search right half)

Step 2:                         👇   🔎             👇
       [ 1️⃣, 3️⃣, 5️⃣, 7️⃣, 9️⃣, 1️⃣1️⃣, 1️⃣3️⃣, 1️⃣5️⃣, 1️⃣7️⃣, 1️⃣9️⃣, 2️⃣1️⃣]
                                 L          M               R
       ❓ 1️⃣7️⃣ > 1️⃣5️⃣ (mid > target)
       ✅ Move right pointer to mid - 1 (search left half)

Step 3:                         👇   🔎  👇
       [ 1️⃣, 3️⃣, 5️⃣, 7️⃣, 9️⃣, 1️⃣1️⃣, 1️⃣3️⃣, 1️⃣5️⃣, 1️⃣7️⃣, 1️⃣9️⃣, 2️⃣1️⃣]
                                 L    M    R
       ❓ 1️⃣3️⃣ < 1️⃣5️⃣ (mid < target)
       ✅ Move left pointer to mid + 1 (search right half)

Step 4:                                👇
       [ 1️⃣, 3️⃣, 5️⃣, 7️⃣, 9️⃣, 1️⃣1️⃣, 1️⃣3️⃣, 1️⃣5️⃣, 1️⃣7️⃣, 1️⃣9️⃣, 2️⃣1️⃣]
                                        L
                                        M
                                        R
       ❓ 1️⃣5️⃣ == 1️⃣5️⃣ (mid == target)
       ✅ Target found! 🎉

Result: 7️⃣ ✅
```

## Pseudo Code for Binary Search

### Iterative Binary Search Pseudo Code

```
function binarySearch(nums, target):
    left = 0
    right = length of nums - 1
    
    while left <= right:
        mid = (left + right) // 2
        
        if nums[mid] == target:
            return mid  # target found
        else if nums[mid] < target:
            left = mid + 1  # search right half
        else:
            right = mid - 1  # search left half
    
    return -1  # target not found
```

### Recursive Binary Search Pseudo Code

```
function binarySearchRecursive(nums, target, left, right):
    if left > right:
        return -1  # base case: target not found
    
    mid = (left + right) // 2
    
    if nums[mid] == target:
        return mid  # target found
    else if nums[mid] < target:
        return binarySearchRecursive(nums, target, mid + 1, right)  # search right
    else:
        return binarySearchRecursive(nums, target, left, mid - 1)  # search left
```

## Python Implementation

### Iterative Solution

```python
def binary_search_iterative(nums, target):
    """
    Iterative binary search implementation to find target in sorted array.
    
    Args:
        nums: A sorted array of integers in ascending order
        target: The integer value we're searching for
        
    Returns:
        Index of target if found, otherwise -1
        
    Time Complexity: O(log n) - we divide search space in half each time
    Space Complexity: O(1) - we only use a constant amount of extra space
    """
    left, right = 0, len(nums) - 1  # Initialize left and right pointers
    
    while left <= right:  # Continue searching as long as search space exists
        mid = (left + right) // 2  # Find the middle element index
        
        if nums[mid] == target:  # Target found at mid position
            return mid
        elif nums[mid] < target:  # Target is in right half
            left = mid + 1  # Discard left half including mid
        else:  # Target is in left half
            right = mid - 1  # Discard right half including mid
    
    return -1  # Target not found in the array
```

### Recursive Solution

```python
def binary_search_recursive(nums, target, left=None, right=None):
    """
    Recursive binary search implementation to find target in sorted array.
    
    Args:
        nums: A sorted array of integers in ascending order
        target: The integer value we're searching for
        left: Starting index of the search space (default: 0)
        right: Ending index of the search space (default: len(nums)-1)
        
    Returns:
        Index of target if found, otherwise -1
        
    Time Complexity: O(log n) - we divide search space in half each time
    Space Complexity: O(log n) - recursion stack uses space proportional to depth
    """
    # Set default values for first call
    if left is None:
        left = 0
    if right is None:
        right = len(nums) - 1
    
    # Base case: empty search space
    if left > right:
        return -1
    
    # Find middle element
    mid = (left + right) // 2
    
    if nums[mid] == target:  # Found the target
        return mid
    elif nums[mid] < target:  # Target is in right half
        return binary_search_recursive(nums, target, mid + 1, right)
    else:  # Target is in left half
        return binary_search_recursive(nums, target, left, mid - 1)
```

## Hinglish Explanation (Using English Letters)

```
Binary search ek bahut hi efficient searching algorithm hai jo sorted arrays par 
kaam karta hai. Isme hum har step par search space ko half kar dete hain, jisse 
search O(log n) time mai complete ho jata hai.

Kaise kaam karta hai binary search:
1. Sabse pehle, hum left aur right pointers ko array ke start aur end par set 
   karte hain.
2. Phir hum middle element ko check karte hain - agar woh target hai, toh mil 
   gaya!
3. Agar target middle element se chota hai, toh hum right half ko discard kar 
   dete hain.
4. Agar target middle element se bada hai, toh hum left half ko discard kar 
   dete hain.
5. Yeh process tab tak repeat karte hain jab tak humein element mil na jaye, ya 
   phir search space khatam na ho jaye.

Binary search do tarike se implement kar sakte hain:
1. Iterative approach - isme hum while loop ka use karte hain
2. Recursive approach - isme function khud ko call karta hai with smaller inputs

Dono approaches same principle par work karte hain, bas implementation alag hai.
Time complexity dono ki O(log n) hai, lekin space complexity iterative approach 
mein O(1) hai aur recursive approach mein O(log n) hai stack ke karan.
```

## Comparing Both Approaches:

| Feature                | Iterative Approach                    | Recursive Approach                       |
|------------------------|---------------------------------------|------------------------------------------|
| Implementation         | Uses while loop                       | Function calls itself                    |
| Time Complexity        | O(log n)                              | O(log n)                                 |
| Space Complexity       | O(1) - constant space                 | O(log n) - recursive call stack          |
| Code Readability       | Sometimes more complex                | Often cleaner and more elegant           |
| Performance            | Slightly better (no function calls)   | Slight overhead due to function calls    |
| Best For               | Memory-constrained environments       | Clear understanding of the algorithm     |

## Real-World Applications of Binary Search

1. 📚 Dictionary lookup - finding words efficiently
2. 🔍 Database searching - quick lookup in sorted data
3. 📱 Phone contacts - finding contacts in alphabetical order
4. 🎮 Game development - efficiently finding elements in sorted structures
5. 📊 Machine learning - algorithms like k-d trees for nearest neighbor search

## Conclusion

Binary search is an elegant algorithm that demonstrates the power of divide and 
conquer approach. By repeatedly dividing the search space in half, we achieve 
logarithmic time complexity, which is extremely efficient for large datasets.

Both the iterative and recursive approaches have their merits, and the choice 
between them often comes down to specific requirements and constraints of your 
application.


