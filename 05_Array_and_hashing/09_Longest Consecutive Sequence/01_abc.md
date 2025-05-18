# Longest Consecutive Sequence Problem - Solutions

## Problem Statement

Given an array of integers `nums`, return the length of the longest consecutive
sequence of elements that can be formed.

A consecutive sequence is a sequence of elements in which each element is exactly
1 greater than the previous element. The elements do not have to be consecutive
in the original array.

You must write an algorithm that runs in O(n) time.

## Visual Explanation

### Input Example: [100, 4, 200, 1, 3, 2]

```
         Longest Consecutive Sequence = [1, 2, 3, 4]
         Length = 4
         
         Visualization:
         
         Input Array:  [100,  4, 200,  1,  3,  2]
                         |    |   |    |   |   |
                         v    v   v    v   v   v
         
         Jumbled:      100    4  200   1   3   2   (Unordered elements)
                                      
                                    ┌───┐
                         ┌──────────┤ 1 ├──────────┐
                         │          └───┘          │
                         │                         │
                         ▼                         ▼
                       ┌───┐                     ┌───┐
                       │ 2 │◄────────────────────┤ 3 │
                       └───┘                     └───┘
                         │                         │
                         │                         │
                         ▼                         │
                       ┌───┐                       │
                       │ 4 │◄─────────────────────┘
                       └───┘
                    
         Isolated:   ┌─────┐      ┌─────┐
                     │ 100 │      │ 200 │ 
                     └─────┘      └─────┘
         
         Sequences:  [1,2,3,4]   [100]   [200]
         Lengths:        4          1       1
         
         Result: 4
```

### Approach Evolution

```
        📌 START
          │
          ▼
┌──────────────────────────┐
│      Input Array         │ [100, 4, 200, 1, 3, 2]
└──────────────────────────┘
          │
          ▼
┌──────────────────────────┐
│  Approach Selection      │
└──────────────────────────┘
          │
          ├────────────────┬────────────────┬─────────────────┐
          │                │                │                 │
          ▼                ▼                ▼                 ▼
┌─────────────────┐ ┌─────────────┐ ┌─────────────┐ ┌──────────────┐
│  Brute Force    │ │   Sorting   │ │  Hash Set   │ │   Hash Map   │
│  O(n³) Time     │ │ O(n log n)  │ │    O(n)     │ │     O(n)     │
└─────────────────┘ └─────────────┘ └─────────────┘ └──────────────┘
          │                │                │                 │
          ▼                ▼                ▼                 ▼
┌─────────────────┐ ┌─────────────┐ ┌─────────────┐ ┌──────────────┐
│ Check all       │ │ Sort first  │ │ Add to set  │ │ Find sequence│
│ possibilities   │ │ Then iterate│ │ Find starts │ │ with HashMap │
└─────────────────┘ └─────────────┘ └─────────────┘ └──────────────┘
          │                │                │                 │
          └────────────────┴────────────────┴─────────────────┘
                                  │
                                  ▼
                         ┌──────────────────┐
                         │   BEST SOLUTION  │
                         │   (Hash Set)     │
                         └──────────────────┘
                                  │
                                  ▼
                               📍 END
```

## Solution 1: Brute Force Approach

### Pseudocode
```
function longestConsecutiveBruteForce(nums):
    if nums is empty:
        return 0
    
    max_length = 1
    
    for each num in nums:
        current_num = num
        current_length = 1
        
        while current_num + 1 exists in nums:
            current_num = current_num + 1
            current_length = current_length + 1
        
        max_length = maximum of max_length and current_length
    
    return max_length
```

### Python Implementation
```python
def longest_consecutive_brute_force(nums):
    """
    Brute force approach for finding the longest consecutive sequence.
    
    Args:
        nums (List[int]): Input array of integers
        
    Returns:
        int: Length of the longest consecutive sequence
        
    Time Complexity: O(n³) - For each element, we check if next element exists
    Space Complexity: O(1) - No extra space used except variables
    """
    if not nums:
        return 0
    
    max_length = 1
    
    for num in nums:
        current_num = num
        current_length = 1
        
        # Keep checking if the next consecutive number exists
        while current_num + 1 in nums:  # This 'in' operation is O(n) in worst case
            current_num += 1
            current_length += 1
        
        max_length = max(max_length, current_length)
    
    return max_length
```

## Solution 2: Sorting Approach

### Pseudocode
```
function longestConsecutiveSorting(nums):
    if nums is empty:
        return 0
    
    sort nums in ascending order
    
    max_length = 1
    current_length = 1
    
    for i from 1 to length of nums - 1:
        if nums[i] != nums[i-1]:  # Avoid duplicates
            if nums[i] == nums[i-1] + 1:  # Consecutive element
                current_length = current_length + 1
            else:  # Sequence break
                max_length = maximum of max_length and current_length
                current_length = 1
    
    return maximum of max_length and current_length
```

### Python Implementation
```python
def longest_consecutive_sorting(nums):
    """
    Sorting approach for finding the longest consecutive sequence.
    
    Args:
        nums (List[int]): Input array of integers
        
    Returns:
        int: Length of the longest consecutive sequence
        
    Time Complexity: O(n log n) - Dominated by the sorting operation
    Space Complexity: O(1) or O(n) depending on sorting implementation
    """
    if not nums:
        return 0
    
    # Sort the array - O(n log n)
    nums.sort()
    
    max_length = 1
    current_length = 1
    
    # Iterate through sorted array - O(n)
    for i in range(1, len(nums)):
        # Skip duplicates
        if nums[i] == nums[i-1]:
            continue
        
        # Check if current element is consecutive to previous
        if nums[i] == nums[i-1] + 1:
            current_length += 1
        else:
            # Reset current length if sequence breaks
            max_length = max(max_length, current_length)
            current_length = 1
    
    # Update max_length one last time before returning
    return max(max_length, current_length)
```

## Solution 3: Hash Set Approach (Optimal)

### Pseudocode
```
function longestConsecutiveHashSet(nums):
    if nums is empty:
        return 0
    
    create a set from nums
    max_length = 0
    
    for each num in set:
        # Check if num is the start of a sequence
        if num - 1 is not in set:
            current_num = num
            current_length = 1
            
            # Count sequence length
            while current_num + 1 is in set:
                current_num = current_num + 1
                current_length = current_length + 1
            
            max_length = maximum of max_length and current_length
    
    return max_length
```

### Python Implementation
```python
def longest_consecutive_hash_set(nums):
    """
    Hash Set approach for finding the longest consecutive sequence.
    
    Args:
        nums (List[int]): Input array of integers
        
    Returns:
        int: Length of the longest consecutive sequence
        
    Time Complexity: O(n) - Each element is visited at most twice
    Space Complexity: O(n) - Extra space for the hash set
    """
    if not nums:
        return 0
    
    # Convert input array to a set for O(1) lookups
    num_set = set(nums)
    max_length = 0
    
    # Check each possible sequence start
    for num in num_set:
        # Only consider numbers that could be the start of a sequence
        if num - 1 not in num_set:
            current_num = num
            current_length = 1
            
            # Count consecutive elements
            while current_num + 1 in num_set:
                current_num += 1
                current_length += 1
            
            # Update max_length if necessary
            max_length = max(max_length, current_length)
    
    return max_length
```

## Solution 4: Hash Map Approach

### Pseudocode
```
function longestConsecutiveHashMap(nums):
    if nums is empty:
        return 0
    
    create an empty hash map
    max_length = 0
    
    for each num in nums:
        if num is already in hash map:
            continue
        
        left_length = length of sequence ending at num-1 (0 if not in map)
        right_length = length of sequence starting at num+1 (0 if not in map)
        
        total_length = left_length + 1 + right_length
        
        # Update boundaries of the sequence
        update hash map at (num - left_length) to total_length
        update hash map at (num + right_length) to total_length
        update hash map at num to total_length
        
        max_length = maximum of max_length and total_length
    
    return max_length
```

### Python Implementation
```python
def longest_consecutive_hash_map(nums):
    """
    Hash Map approach for finding the longest consecutive sequence.
    
    Args:
        nums (List[int]): Input array of integers
        
    Returns:
        int: Length of the longest consecutive sequence
        
    Time Complexity: O(n) - Each element is processed once
    Space Complexity: O(n) - Extra space for the hash map
    """
    if not nums:
        return 0
    
    # Hash map to store sequence lengths at boundaries
    length_map = {}
    max_length = 0
    
    for num in nums:
        # Skip if already processed
        if num in length_map:
            continue
        
        # Find lengths of adjacent sequences
        left_length = length_map.get(num - 1, 0)
        right_length = length_map.get(num + 1, 0)
        
        # Calculate total sequence length
        total_length = left_length + 1 + right_length
        
        # Update max length
        max_length = max(max_length, total_length)
        
        # Update boundaries in the map
        length_map[num] = total_length
        
        # Update the boundaries of the extended sequence
        if left_length > 0:
            length_map[num - left_length] = total_length
        if right_length > 0:
            length_map[num + right_length] = total_length
    
    return max_length
```

## Comparison of Approaches

| Approach    | Time Complexity | Space Complexity | Comments                                   |
|-------------|----------------|------------------|-------------------------------------------|
| Brute Force | O(n³)          | O(1)             | Simple but inefficient                     |
| Sorting     | O(n log n)     | O(1) or O(n)     | Better but still not optimal              |
| Hash Set    | O(n)           | O(n)             | Optimal time complexity, intuitive         |
| Hash Map    | O(n)           | O(n)             | Optimal time complexity, more complex      |

## Hindi-English Explanation (Hinglish)

**Brute Force Approach:**
```
Is problem mein hume longest consecutive sequence dhundhna hai. Brute force 
approach mein, hum har number ke liye check karte hain ki kya uske baad wala 
number array mein hai ya nahi. Agar hai, toh sequence continue karte hain, 
nahi toh new sequence start karte hain. Har sequence ki length calculate karke
maximum length track karte hain. Lekin iska time complexity O(n³) hai jo bahut
slow hai.
```

**Sorting Approach:**
```
Sorting approach mein, pehle array ko sort kar lete hain (O(n log n) time). 
Phir ek baar iterate karke consecutive elements ko count karte hain. Duplicates
ko handle karke, maximum sequence length nikal lete hain. Iska time complexity
O(n log n) hai jo brute force se better hai, lekin optimal nahi hai.
```

**Hash Set Approach:**
```
Hash Set approach optimal hai aur O(n) time mein solution deta hai. Is approach
mein, pehle saare elements ko ek set mein daal dete hain. Phir har possible 
sequence ke start point ko dhundhte hain (woh number jiske peeche wala number
set mein nahi hai). Phir har start point se sequence ki length count karte hain.
Maximum length return karte hain. Yeh approach bahut efficient hai.
```

**Hash Map Approach:**
```
Hash Map approach bhi O(n) time complexity deta hai. Is approach mein, hum har
number ke left aur right mein existing sequences ki length track karte hain.
Jab koi naya number milta hai, toh hum check karte hain ki kya uske aas-paas
koi sequence already exist karti hai. Agar haan, toh sequences ko merge karte
hain aur length update karte hain. Yeh approach thoda complex hai lekin optimal
time complexity provide karta hai.
```

## Conclusion

The Hash Set approach is the most efficient solution for this problem with O(n) time
complexity. It avoids the expensive sorting operation and uses a smart way to
identify the start of sequences and count their lengths. The Hash Map approach is
also O(n) but is slightly more complex to implement.

For interviews, the Hash Set approach is recommended as it is the optimal solution
and is easier to explain and implement correctly.

