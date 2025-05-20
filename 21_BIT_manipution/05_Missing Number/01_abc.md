# LeetCode Missing Number Solutions with ASCII/Emoji Art 🎮

## Problem Statement 📝

**Missing Number**: Given an array `nums` containing `n` integers in the range `[0, n]` without any duplicates, return the single number in the range that is missing from `nums`.

Follow-up: Implement a solution using only O(1) extra space complexity and O(n) runtime complexity.

## ASCII Art Visualization of the Problem 🖼️

```
Input Array: [3, 0, 1]
Range: [0, 1, 2, 3]

┌───┬───┬───┐       ┌───┬───┬───┬───┐
│ 3 │ 0 │ 1 │  vs.  │ 0 │ 1 │ 2 │ 3 │
└───┴───┴───┘       └───┴───┴───┴───┘
                            ⬆️
                     Missing Number: 2
```

## Solution 1: Sorting Approach 📊

### Visualization

```
Step 1: Start with unsorted array
┌───┬───┬───┐
│ 3 │ 0 │ 1 │
└───┴───┴───┘

Step 2: Sort the array
┌───┬───┬───┐
│ 0 │ 1 │ 3 │ ← Notice gap between 1 and 3! Number 2 is missing!
└───┴───┴───┘
          ⬆️
          Missing Number: 2
```

### Pseudo Code 💡
```
function findMissingNumber(nums):
    sort nums in ascending order
    
    for i from 0 to length(nums)-1:
        if nums[i] != i:
            return i
    
    return length(nums)  // If no number is missing in between, the last number is missing
```

### Python Code 🐍
```python
def find_missing_number_sorting(nums):
    """
    Finds missing number in range [0,n] using sorting approach.
    
    Args:
        nums: List of integers containing n distinct numbers in range [0,n] with one
              missing number.
              
    Returns:
        The missing number from the range [0,n].
        
    Time Complexity: O(n log n) due to sorting
    Space Complexity: O(1) as we sort in-place without extra space
    """
    nums.sort()  # Sort array in ascending order to identify the gap
    
    for i in range(len(nums)):
        if nums[i] != i:  # If index doesn't match value, we found our missing number
            return i
            
    # If we didn't find a mismatch, the missing number is n
    return len(nums)
```

## Solution 2: Hash Set Approach 🔍

### Visualization

```
Step 1: Create a hash set with all numbers in the array
┌─────────────────┐
│ Hash Set: {3,0,1} │
└─────────────────┘

Step 2: Check each number in range [0,n]
┌───┬───┬───┬───┐
│ 0 │ 1 │ 2 │ 3 │  Check if each exists in hash set
└───┴───┴───┴───┘
        ❌        Number 2 is not in hash set! It's missing!
```

### Pseudo Code 💡
```
function findMissingNumber(nums):
    numSet = create a hash set from nums
    
    for i from 0 to length(nums):
        if i is not in numSet:
            return i
    
    return -1  // This should never happen given the problem constraints
```

### Python Code 🐍
```python
def find_missing_number_hashset(nums):
    """
    Finds missing number in range [0,n] using hash set approach.
    
    Args:
        nums: List of integers containing n distinct numbers in range [0,n] with one
              missing number.
              
    Returns:
        The missing number from the range [0,n].
        
    Time Complexity: O(n) as we iterate through array once
    Space Complexity: O(n) for storing the hash set
    """
    num_set = set(nums)  # Create a set with all numbers in the array
    
    # Iterate through range [0,n] and find the first number not in our set
    for i in range(len(nums) + 1):
        if i not in num_set:
            return i
            
    # This line should never be reached with valid input based on problem description
    return -1
```

## Solution 3: Bitwise XOR Approach ⚡

### Visualization

```
Step 1: XOR all numbers from 0 to n
┌───┬───┬───┬───┐
│ 0 │ 1 │ 2 │ 3 │  XOR all = 0⊕1⊕2⊕3
└───┴───┴───┴───┘

Step 2: XOR all numbers in array
┌───┬───┬───┐
│ 3 │ 0 │ 1 │  XOR all = 3⊕0⊕1
└───┴───┴───┘

Step 3: XOR the results (missing number appears once)
Result = (0⊕1⊕2⊕3) ⊕ (3⊕0⊕1) = 2
                                ⬆️
                        Missing Number: 2
```

### Pseudo Code 💡
```
function findMissingNumber(nums):
    result = length(nums)
    
    for i from 0 to length(nums)-1:
        result = result XOR i XOR nums[i]
    
    return result
```

### Python Code 🐍
```python
def find_missing_number_xor(nums):
    """
    Finds missing number in range [0,n] using bitwise XOR approach.
    
    Args:
        nums: List of integers containing n distinct numbers in range [0,n] with one
              missing number.
              
    Returns:
        The missing number from the range [0,n].
        
    Time Complexity: O(n) as we iterate through array once
    Space Complexity: O(1) as we only use a single variable
    
    Logic: XOR has two important properties:
    1. XOR of a number with itself is 0
    2. XOR is commutative and associative
    
    By XORing all numbers from 0 to n and all numbers in array, duplicates cancel out,
    leaving only the missing number.
    """
    # Start with n (length of nums) as initial value
    missing = len(nums)
    
    # XOR with each index and value in the array
    for i, num in enumerate(nums):
        # XOR with current index and current value
        missing ^= i ^ num
        
    return missing
```

## Solution 4: Math Approach 🧮

### Visualization

```
Step 1: Calculate expected sum of numbers from 0 to n 
┌───┬───┬───┬───┐
│ 0 │ 1 │ 2 │ 3 │  Sum = 0+1+2+3 = 6
└───┴───┴───┴───┘

Step 2: Calculate actual sum of array
┌───┬───┬───┐
│ 3 │ 0 │ 1 │  Sum = 3+0+1 = 4
└───┴───┴───┘

Step 3: Find difference (missing number)
Missing = 6 - 4 = 2
                    ⬆️
            Missing Number: 2
```

### Pseudo Code 💡
```
function findMissingNumber(nums):
    n = length(nums)
    expectedSum = n * (n + 1) / 2
    actualSum = sum of all elements in nums
    
    return expectedSum - actualSum
```

### Python Code 🐍
```python
def find_missing_number_math(nums):
    """
    Finds missing number in range [0,n] using mathematical approach.
    
    Args:
        nums: List of integers containing n distinct numbers in range [0,n] with one
              missing number.
              
    Returns:
        The missing number from the range [0,n].
        
    Time Complexity: O(n) for calculating sum of array
    Space Complexity: O(1) as we only use a few variables
    
    Logic: Use arithmetic sum formula n*(n+1)/2 to find expected sum of all numbers
    from 0 to n, then subtract actual sum of array to find missing number.
    """
    n = len(nums)
    
    # Calculate expected sum of all numbers from 0 to n
    expected_sum = n * (n + 1) // 2
    
    # Calculate actual sum of numbers in array
    actual_sum = sum(nums)
    
    # The difference is our missing number
    return expected_sum - actual_sum
```

## Solution Comparison and Emoji Summary 📊

| Approach  | Time Complexity | Space Complexity | Visual Summary |
|-----------|----------------|-----------------|----------------|
| Sorting   | O(n log n)     | O(1)            | 🔄➡️🔍         |
| Hash Set  | O(n)           | O(n)            | 🧩➡️🔍         |
| XOR       | O(n)           | O(1)            | ⚡➡️🔍         |
| Math      | O(n)           | O(1)            | ➕➡️➖➡️🔍     |

## Hinglish Summary (Hindi+English) 🇮🇳

Iss problem mein hamein ek array mein se missing number find karna hai. Hamne char
different tarike se solution implement kiya hai. Sabse pehle sorting approach hai,
jismein hum array ko sort karke gap dhoondhte hain. Dusra approach hash set ka use
karta hai jismein hum har number ko check karte hain. Teesra solution XOR operator
ka use karta hai, jo sabse efficient hai space complexity ke hisab se. Aur aakhiri
approach mathematical formula ka use karke expected sum aur actual sum ke difference
se missing number find karta hai.

## Conclusion 🎯

Best solution depends on requirements:
- For best runtime: Use XOR or Math approach (O(n) time, O(1) space)
- For simplicity: Use Math approach 
- For interview: Discuss all four approaches and their trade-offs

The XOR and Math approaches both meet the follow-up challenge of O(n) time complexity
and O(1) space complexity.

