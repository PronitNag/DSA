# 3Sum Problem Solution with ASCII Art Visualization

## Problem Statement

Given an integer array nums, return all the triplets `[nums[i], nums[j], nums[k]]` where:
- `nums[i] + nums[j] + nums[k] == 0`
- The indices i, j and k are all distinct
- Output should not contain duplicate triplets

## ASCII Art Visualization of Approaches

### Common Starting Point 📊
```
Input Array: [nums] = [-4, -1, -1, 0, 1, 2]
                         ^   ^   ^  ^  ^  ^
                     Find triplets that sum to zero 🎯
```

### 1. Brute Force Solution 🔨🔨🔨

```
                            [-4, -1, -1, 0, 1, 2]
                             /    |     \
                      Choose i    j      k
                       /          |       \
           Try All    /           |        \    Combinations
                     /            |         \
              [-4,-1,-1]       [-4,-1,0]    ... and so on
                 ↓                ↓
         -4 + -1 + -1 = -6      -4 + -1 + 0 = -5    (Not zero)
              (Not zero)         (Not zero)

                  ... eventually finding ...

              [-4, 1, 3]         [-1, -1, 2]       [0, -1, 1]
                 ↓                   ↓                 ↓
         -4 + 1 + 3 = 0         -1 + -1 + 2 = 0    0 + -1 + 1 = 0
              ✓ Found!             ✓ Found!           ✓ Found!

         But extremely slow! O(n³) time complexity 😱
```

### 2. Hash Map Solution 🗺️

```
                     [-4, -1, -1, 0, 1, 2]
                       ↓
         1. For each num (i), try to find pairs (j,k) that sum to -num
                       ↓
         2. Use hash table to find pairs in O(n) time
                       ↓
     For num = -4:     For num = -1:        For num = 0:
          ┌─────┐         ┌─────┐             ┌─────┐
          │ -4  │         │ -1  │             │  0  │
          └─────┘         └─────┘             └─────┘
             ↓               ↓                   ↓
     Need sum = 4      Need sum = 1        Need sum = 0
      ┌─────────┐       ┌─────────┐        ┌─────────┐
      │ Hash    │       │ Hash    │        │ Hash    │
      │ Table   │       │ Table   │        │ Table   │
      └─────────┘       └─────────┘        └─────────┘
          ↓                 ↓                  ↓
  Looking for pairs    Found (0,1)!         Found (-1,1)!
  None found...                   
         
  O(n²) time complexity - Better! 😊
```

### 3. Two Pointers Solution ⬅️➡️

```
                         Sort the array
                              ↓
                     [-4, -1, -1, 0, 1, 2]
                       ^              ^
                      left          right

For each element as "anchor":
    ┌──────────────────────────────────────────────┐
    │  Anchor                                      │
    │    ↓                                         │
    │ [-4, -1, -1,  0,  1,  2]                     │
    │       ^                ^                     │
    │      left             right                  │
    │                                              │
    │  Sum = -4 + (-1) + 2 = -3 < 0                │
    │     Move left pointer right →                │
    └──────────────────────────────────────────────┘

    ┌──────────────────────────────────────────────┐
    │  Anchor                                      │
    │    ↓                                         │
    │ [-4, -1, -1,  0,  1,  2]                     │
    │           ^           ^                      │
    │          left        right                   │
    │                                              │
    │  Sum = -4 + (-1) + 2 = -3 < 0                │
    │     Move left pointer right →                │
    └──────────────────────────────────────────────┘

    ┌──────────────────────────────────────────────┐
    │  Anchor                                      │
    │    ↓                                         │
    │ [-4, -1, -1,  0,  1,  2]                     │
    │               ^       ^                      │
    │              left    right                   │
    │                                              │
    │  Sum = -4 + 0 + 2 = -2 < 0                   │
    │     Move left pointer right →                │
    └──────────────────────────────────────────────┘

    ┌──────────────────────────────────────────────┐
    │  Anchor                                      │
    │    ↓                                         │
    │ [-4, -1, -1,  0,  1,  2]                     │
    │                   ^   ^                      │
    │                  left right                  │
    │                                              │
    │  Sum = -4 + 1 + 2 = -1 < 0                   │
    │  No more moves possible with this anchor     │
    └──────────────────────────────────────────────┘

    ... And so on for each anchor element ...

    Until we find all triplets: [[-4,1,3], [-1,-1,2], [0,-1,1]]

    O(n²) time complexity - Efficient! 🚀
```

## 1. Brute Force Solution

### Pseudo Code (Brute Force) 🧠
```
function threeSum(nums):
    sort the array nums
    result = empty set to store unique triplets
    
    for i from 0 to length(nums)-3:
        if i > 0 and nums[i] == nums[i-1]: continue  // skip duplicates
            
        for j from i+1 to length(nums)-2:
            if j > i+1 and nums[j] == nums[j-1]: continue  // skip duplicates
            
            for k from j+1 to length(nums)-1:
                if k > j+1 and nums[k] == nums[k-1]: continue  // skip duplicates
                
                if nums[i] + nums[j] + nums[k] == 0:
                    add [nums[i], nums[j], nums[k]] to result
    
    return result
```

### Python Implementation (Brute Force) 🐍
```python
def threeSum_bruteforce(nums):
    """
    Solves the 3Sum problem using brute force approach.
    
    Args:
        nums (List[int]): Input array of integers
        
    Returns:
        List[List[int]]: List of triplets that sum to zero
        
    Time Complexity: O(n³) where n is the length of nums
    Space Complexity: O(1) excluding the output array
    """
    nums.sort()  # Sort array to handle duplicates easily
    result = []
    n = len(nums)
    
    for i in range(n - 2):
        # Skip duplicates for first position
        if i > 0 and nums[i] == nums[i - 1]:
            continue
            
        for j in range(i + 1, n - 1):
            # Skip duplicates for second position
            if j > i + 1 and nums[j] == nums[j - 1]:
                continue
                
            for k in range(j + 1, n):
                # Skip duplicates for third position
                if k > j + 1 and nums[k] == nums[k - 1]:
                    continue
                    
                if nums[i] + nums[j] + nums[k] == 0:
                    result.append([nums[i], nums[j], nums[k]])
                    
    return result
```

## 2. Hash Map Solution

### Pseudo Code (Hash Map) 🧠
```
function threeSum(nums):
    sort the array nums
    result = empty set to store unique triplets
    
    for i from 0 to length(nums)-3:
        if i > 0 and nums[i] == nums[i-1]: continue  // skip duplicates
        
        target = -nums[i]
        seen = empty hash map
        
        for j from i+1 to length(nums)-1:
            complement = target - nums[j]
            
            if complement in seen:
                add [nums[i], complement, nums[j]] to result
                while j+1 < length(nums) and nums[j] == nums[j+1]: j++  // skip duplicates
            
            seen[nums[j]] = j
    
    return result
```

### Python Implementation (Hash Map) 🐍
```python
def threeSum_hashmap(nums):
    """
    Solves the 3Sum problem using hash map approach.
    
    Args:
        nums (List[int]): Input array of integers
        
    Returns:
        List[List[int]]: List of triplets that sum to zero
        
    Time Complexity: O(n²) where n is the length of nums
    Space Complexity: O(n) for the hash map
    """
    nums.sort()  # Sort array to handle duplicates easily
    result = []
    n = len(nums)
    
    for i in range(n - 2):
        # Skip duplicates for first position
        if i > 0 and nums[i] == nums[i - 1]:
            continue
            
        # Find pairs that sum to -nums[i]
        target = -nums[i]
        seen = {}
        
        for j in range(i + 1, n):
            complement = target - nums[j]
            
            if complement in seen:
                result.append([nums[i], complement, nums[j]])
                # Skip duplicates for third position
                while j + 1 < n and nums[j] == nums[j + 1]:
                    j += 1
                    
            seen[nums[j]] = j
            
    return result
```

## 3. Two Pointers Solution (Most Efficient)

### Pseudo Code (Two Pointers) 🧠
```
function threeSum(nums):
    sort the array nums
    result = empty array to store unique triplets
    n = length of nums
    
    for i from 0 to n-3:
        if i > 0 and nums[i] == nums[i-1]: continue  // skip duplicates
        
        left = i + 1
        right = n - 1
        
        while left < right:
            sum = nums[i] + nums[left] + nums[right]
            
            if sum < 0:
                left++
            else if sum > 0:
                right--
            else:  // sum == 0
                add [nums[i], nums[left], nums[right]] to result
                
                // Skip duplicates
                while left < right and nums[left] == nums[left+1]: left++
                while left < right and nums[right] == nums[right-1]: right--
                
                left++
                right--
    
    return result
```

### Python Implementation (Two Pointers) 🐍
```python
def threeSum_twopointers(nums):
    """
    Solves the 3Sum problem using two pointers approach.
    
    Args:
        nums (List[int]): Input array of integers
        
    Returns:
        List[List[int]]: List of triplets that sum to zero
        
    Time Complexity: O(n²) where n is the length of nums
    Space Complexity: O(1) excluding the output array
    
    Yeh solution sabse efficient hai! (This solution is the most efficient!)
    """
    nums.sort()  # Sort array to easily handle duplicates
    result = []
    n = len(nums)
    
    for i in range(n - 2):
        # Agar pehle element ko already check kar chuke hain to skip karo
        if i > 0 and nums[i] == nums[i - 1]:
            continue
            
        # Left aur right pointers set karo
        left, right = i + 1, n - 1
        
        while left < right:
            current_sum = nums[i] + nums[left] + nums[right]
            
            if current_sum < 0:
                # Sum bahut chota hai, left pointer ko aage badhao
                left += 1
            elif current_sum > 0:
                # Sum bahut bada hai, right pointer ko peeche karo
                right -= 1
            else:
                # Mil gaya triplet! Result mein add karo
                result.append([nums[i], nums[left], nums[right]])
                
                # Duplicate elements ko skip karo
                while left < right and nums[left] == nums[left + 1]:
                    left += 1
                while left < right and nums[right] == nums[right - 1]:
                    right -= 1
                    
                # Pointers ko move karo
                left += 1
                right -= 1
                
    return result
```

## Full Solution with All Three Approaches 🚀

```python
def threeSum_complete(nums):
    """
    Main function that provides full solution using two pointers approach
    (the most efficient approach).
    
    Args:
        nums (List[int]): Input array of integers
        
    Returns:
        List[List[int]]: List of triplets that sum to zero
    """
    return threeSum_twopointers(nums)  # Using the most efficient approach

# Example usage:
nums = [-1, 0, 1, 2, -1, -4]
result = threeSum_complete(nums)
print("Triplets that sum to zero:", result)
# Output: [[-1, -1, 2], [-1, 0, 1]]
```

## Comparison of Solution Approaches 📊

| Approach        | Time Complexity | Space Complexity | Pros                      | Cons                          |
|-----------------|----------------|------------------|---------------------------|-------------------------------|
| Brute Force     | O(n³)          | O(1)             | Simple to understand      | Very slow for large inputs    |
| Hash Map        | O(n²)          | O(n)             | Faster than brute force   | Uses extra space              |
| Two Pointers    | O(n²)          | O(1)             | Best overall performance  | Requires sorting (O(n log n)) |

## Conclusion

The two pointers approach is the most efficient way to solve this problem with O(n²) time complexity and O(1) extra space (not counting the output array).

"Sabse achcha solution do pointers wala hai, kyunki isme hum array ko ek baar sort karke, O(n²) time mein saare triplets dhund sakte hain!" 🎯

