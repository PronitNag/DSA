# Two Sum - Leetcode Easy Problem Solutions

## Problem Statement
Given an array of integers `nums` and an integer `target`, return the indices `i` and `j` such that:
- `nums[i] + nums[j] == target`
- `i != j`
- Every input has exactly one solution
- Return the answer with the smaller index first

## Solution Approaches with ASCII Art Visualizations

### 1. Brute Force Solution

```
Start: [2, 7, 11, 15], target = 9
       🔍 🔍 🔍  🔍
       |  |
       |  ↓
       |  Check: 2+7=9 ✓
       ↓  
       Check each pair

End: [0, 1] (indices where 2+7=9)
```

#### Pseudocode:
```
function twoSum(nums, target)
    for i from 0 to length(nums)-1
        for j from i+1 to length(nums)-1
            if nums[i] + nums[j] == target
                return [i, j]
    return [] // No solution found (though problem guarantees a solution)
```

#### Python Code:
```python
def twoSum_brute_force(nums, target):
    """
    Brute force solution for Two Sum problem.
    
    Args:
        nums: List[int] - Array of integers
        target: int - Target sum
    
    Returns:
        List[int] - Indices of the two numbers that add up to target
    
    Time Complexity: O(n²) - Checking all pairs
    Space Complexity: O(1) - Constant extra space
    """
    n = len(nums)
    # Bahar wala loop har number ko check karta hai
    for i in range(n):
        # Andar wala loop uske baad wale sabhi numbers ko check karta hai
        for j in range(i + 1, n):
            # Agar dono ka sum target ke barabar hai
            if nums[i] + nums[j] == target:
                return [i, j]  # Indices return karo
    
    # Problem guarantees a solution, so we shouldn't reach here
    return []
```

### 2. Sorting Solution

```
Start: [2, 7, 11, 15], target = 9
        ↓  ↓  ↓   ↓
        
Sort & track indices: [(0,2), (1,7), (2,11), (3,15)]
                        ↑                       ↑
                        left                    right
                        
Check: 2+15=17 (too large) → move right pointer left
       ↑     ↑
       left  right
       
Check: 2+11=13 (too large) → move right pointer left
       ↑    ↑
       left right
       
Check: 2+7=9 (equal to target!) ✓
       ↑ ↑
       L R

End: [0, 1] (original indices where 2+7=9)
```

#### Pseudocode:
```
function twoSum(nums, target)
    // Create pairs of (index, value)
    pairs = []
    for i from 0 to length(nums)-1
        append (i, nums[i]) to pairs
    
    // Sort by value
    sort pairs by second element (value)
    
    // Two pointers approach
    left = 0
    right = length(pairs)-1
    
    while left < right
        currentSum = pairs[left].value + pairs[right].value
        
        if currentSum == target
            return [pairs[left].index, pairs[right].index] sorted by index
        else if currentSum < target
            left++
        else
            right--
    
    return [] // No solution found
```

#### Python Code:
```python
def twoSum_sorting(nums, target):
    """
    Sorting-based solution for Two Sum problem.
    
    Args:
        nums: List[int] - Array of integers
        target: int - Target sum
    
    Returns:
        List[int] - Indices of the two numbers that add up to target
    
    Time Complexity: O(n log n) - Due to sorting
    Space Complexity: O(n) - For storing the pairs
    """
    # Har number ke saath uska original index track karenge
    pairs = [(i, num) for i, num in enumerate(nums)]
    
    # Value ke basis par sort karenge
    pairs.sort(key=lambda x: x[1])
    
    left, right = 0, len(pairs) - 1
    
    # Two pointer approach
    while left < right:
        current_sum = pairs[left][1] + pairs[right][1]
        
        if current_sum == target:
            # Original indices return karenge, chote index ko pehle
            indices = [pairs[left][0], pairs[right][0]]
            return sorted(indices)
        
        # Agar sum chota hai to left pointer ko aage badhao
        elif current_sum < target:
            left += 1
        
        # Agar sum bada hai to right pointer ko peeche lao
        else:
            right -= 1
    
    # Problem guarantees a solution
    return []
```

### 3. Hash Map (Two Pass)

```
Start: [2, 7, 11, 15], target = 9
        ↓  ↓  ↓   ↓

First pass: Build hash map of value->index
{
  2 → 0,
  7 → 1,
  11 → 2,
  15 → 3
}

Second pass: For each number, check if (target-number) exists in map
[2, 7, 11, 15]
 ↓
 2: Check if (9-2)=7 exists in map? Yes at index 1! ✓

End: [0, 1] (indices where 2+7=9)
```

#### Pseudocode:
```
function twoSum(nums, target)
    // First pass: Build hash map
    hash_map = empty map
    for i from 0 to length(nums)-1
        hash_map[nums[i]] = i
    
    // Second pass: Check for complements
    for i from 0 to length(nums)-1
        complement = target - nums[i]
        if complement in hash_map AND hash_map[complement] != i
            return [i, hash_map[complement]] sorted by index
    
    return [] // No solution found
```

#### Python Code:
```python
def twoSum_hash_two_pass(nums, target):
    """
    Two-pass hash map solution for Two Sum problem.
    
    Args:
        nums: List[int] - Array of integers
        target: int - Target sum
    
    Returns:
        List[int] - Indices of the two numbers that add up to target
    
    Time Complexity: O(n) - Two passes through the array
    Space Complexity: O(n) - For storing the hash map
    """
    # Pehla pass: Hash map banao (value -> index)
    num_to_index = {}
    for i, num in enumerate(nums):
        num_to_index[num] = i
    
    # Dusra pass: Har number ke liye complement check karo
    for i, num in enumerate(nums):
        complement = target - num
        
        # Check if complement exists and is not the same element
        if complement in num_to_index and num_to_index[complement] != i:
            return [i, num_to_index[complement]] if i < num_to_index[complement] else [num_to_index[complement], i]
    
    # Problem guarantees a solution
    return []
```

### 4. Hash Map (One Pass)

```
Start: [2, 7, 11, 15], target = 9
        ↓  ↓  ↓   ↓
        
Hash map: {} (empty)

Process 2: Check if (9-2)=7 exists in map? No
           Add 2→0 to map
           Map = {2→0}
           
Process 7: Check if (9-7)=2 exists in map? Yes at index 0! ✓
           Found solution [0, 1]

End: [0, 1] (indices where 2+7=9)
```

#### Pseudocode:
```
function twoSum(nums, target)
    hash_map = empty map
    
    for i from 0 to length(nums)-1
        complement = target - nums[i]
        
        if complement in hash_map
            return [hash_map[complement], i]
        
        hash_map[nums[i]] = i
    
    return [] // No solution found
```

#### Python Code:
```python
def twoSum_hash_one_pass(nums, target):
    """
    One-pass hash map solution for Two Sum problem.
    
    Args:
        nums: List[int] - Array of integers
        target: int - Target sum
    
    Returns:
        List[int] - Indices of the two numbers that add up to target
    
    Time Complexity: O(n) - Single pass through the array
    Space Complexity: O(n) - For storing the hash map
    """
    # Ek hi pass mein solution dhoondhte hain
    num_to_index = {}
    
    for i, num in enumerate(nums):
        complement = target - num
        
        # Pehle check karo ki complement map mein hai ya nahi
        if complement in num_to_index:
            # Mil gaya solution!
            return [num_to_index[complement], i]
        
        # Nahi mila to current number ko map mein add karo
        num_to_index[num] = i
    
    # Problem guarantees a solution
    return []
```

## Sequence of ASCII Art Evolution

### Initial Array
```
[2, 7, 11, 15], target = 9
 0  1   2   3
```

### Brute Force Evolution
```
Step 1: Check 2+7
[2, 7, 11, 15]
 ↑  ↑
 i  j
 2+7=9 ✓ Solution found!
```

### Sorting Evolution
```
Step 1: Track indices and sort
[(0,2), (1,7), (2,11), (3,15)]
   ↑                    ↑
  left                right

Step 2: Compare sum with target
2+15=17 > 9, move right pointer
[(0,2), (1,7), (2,11), (3,15)]
   ↑               ↑
  left            right

Step 3: Compare again
2+11=13 > 9, move right pointer
[(0,2), (1,7), (2,11), (3,15)]
   ↑        ↑
  left     right

Step 4: Final check
2+7=9 == 9 ✓ Solution found!
[(0,2), (1,7), (2,11), (3,15)]
   ↑     ↑
  left  right
```

### Hash Map Two-Pass Evolution
```
Step 1: Build hash map
{2→0, 7→1, 11→2, 15→3}

Step 2: For each number, check complement
[2, 7, 11, 15]
 ↑
For 2, complement=7 exists at index 1 ✓ Solution found!
```

### Hash Map One-Pass Evolution
```
Step 1: Start with empty map {}

Step 2: Process 2
Check if (9-2)=7 exists? No
Add {2→0} to map

Step 3: Process 7
Check if (9-7)=2 exists? Yes at index 0!
Solution: [0, 1] ✓
```

## Comparison of All Solutions

| Solution Approach | Time Complexity | Space Complexity | Advantages | Disadvantages |
|-------------------|----------------|-----------------|------------|---------------|
| Brute Force | O(n²) | O(1) | Simple to implement, no extra space | Inefficient for large arrays |
| Sorting | O(n log n) | O(n) | Better than brute force | Requires tracking original indices |
| Hash Map (Two Pass) | O(n) | O(n) | Linear time | Requires extra space |
| Hash Map (One Pass) | O(n) | O(n) | Most efficient, single pass | Requires extra space |

## Emoji Summary
👉 Brute Force: 🔍🔍 (two loops checking all pairs)
👉 Sorting: 📊↔️ (sort and use two pointers)
👉 Hash Map (Two Pass): 🗺️✅ (build map, then check)
👉 Hash Map (One Pass): 🗺️🏃 (build map while checking)

For most use cases, the One-Pass Hash Map solution is optimal as it has O(n) time complexity and requires only one pass through the array! 🚀
