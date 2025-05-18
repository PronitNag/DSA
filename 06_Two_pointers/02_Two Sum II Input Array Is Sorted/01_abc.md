# Two Integer Sum II - Solutions

## Problem Statement 

Given an array of integers `numbers` that is sorted in non-decreasing order, return the indices (1-indexed) of two numbers, `[index1, index2]`, such that they add up to a specific `target` number and `index1 < index2`. Note that `index1` and `index2` cannot be equal. There will always be exactly one valid solution.

## ASCII Art Visualization of Solutions

### Starting Point for All Solutions:
```
Sorted Array:  [2, 3, 6, 8, 11, 15]
Indices:        1  2  3  4   5   6
Target: 14
```

## 1. Brute Force Solution

### Visual Representation:

```
Step 1: Check (2,3)   -> 2+3=5   ❌
        [2, 3, 6, 8, 11, 15]
         ↑  ↑
         
Step 2: Check (2,6)   -> 2+6=8   ❌
        [2, 3, 6, 8, 11, 15]
         ↑     ↑
         
Step 3: Check (2,8)   -> 2+8=10  ❌
        [2, 3, 6, 8, 11, 15]
         ↑        ↑
         
Step 4: Check (2,11)  -> 2+11=13 ❌
        [2, 3, 6, 8, 11, 15]
         ↑           ↑
         
Step 5: Check (2,15)  -> 2+15=17 ❌
        [2, 3, 6, 8, 11, 15]
         ↑              ↑
         
...continuing checks...

Step 9: Check (3,11)  -> 3+11=14 ✅ Found solution!
        [2, 3, 6, 8, 11, 15]
            ↑        ↑
         index1=2, index2=5
```

### Pseudo Code for Brute Force:
```
function twoSum(numbers, target):
    n = length of numbers
    for i = 0 to n-2:
        for j = i+1 to n-1:
            if numbers[i] + numbers[j] == target:
                return [i+1, j+1]  # Converting to 1-indexed
    return null  # No solution found (though problem guarantees a solution)
```

### Python Code for Brute Force:
```python
def twoSum_brute_force(numbers, target):
    """
    Brute force approach to find two numbers in a sorted array that add up to
    the target value.
    
    Args:
        numbers: A list of integers sorted in non-decreasing order
        target: An integer representing the target sum
        
    Returns:
        A list containing two 1-indexed integers representing the positions of
        the two numbers that add up to the target
    
    Time Complexity: O(n²) - we check each pair of elements in the worst case
    Space Complexity: O(1) - we use constant extra space
    """
    n = len(numbers)
    for i in range(n - 1):
        for j in range(i + 1, n):
            if numbers[i] + numbers[j] == target:
                return [i + 1, j + 1]  # Convert to 1-indexed as required
    
    # Problem statement guarantees exactly one solution
    return None
```

## 2. Binary Search Solution

### Visual Representation:

```
For each index i, we search for (target - numbers[i]) using binary search
in the rest of the array

Step 1: For i=0 (value=2), search for (14-2)=12 in remaining array
        [2, 3, 6, 8, 11, 15]
         ↑  |     |      |
         i  L     M      R
        Binary search for 12 in [3,6,8,11,15] -> Not found

Step 2: For i=1 (value=3), search for (14-3)=11 in remaining array
        [2, 3, 6, 8, 11, 15]
            ↑  |  |   |
            i  L  M   R
        Binary search for 11 in [6,8,11,15] -> Found at index 4!
        
        Return [2, 5] (1-indexed answers for indices 1 and 4)
```

### Pseudo Code for Binary Search:
```
function twoSum(numbers, target):
    n = length of numbers
    for i = 0 to n-2:
        complement = target - numbers[i]
        
        # Binary search for complement in numbers[i+1...n-1]
        left = i + 1
        right = n - 1
        
        while left <= right:
            mid = left + (right - left) / 2
            if numbers[mid] == complement:
                return [i+1, mid+1]  # Convert to 1-indexed
            elif numbers[mid] < complement:
                left = mid + 1
            else:
                right = mid - 1
                
    return null  # No solution found
```

### Python Code for Binary Search:
```python
def twoSum_binary_search(numbers, target):
    """
    Binary search approach to find two numbers in a sorted array that add up to
    the target value.
    
    Args:
        numbers: A list of integers sorted in non-decreasing order
        target: An integer representing the target sum
        
    Returns:
        A list containing two 1-indexed integers representing the positions of
        the two numbers that add up to the target
    
    Time Complexity: O(n log n) - for each element, we do a binary search
    Space Complexity: O(1) - we use constant extra space
    """
    n = len(numbers)
    
    for i in range(n - 1):
        complement = target - numbers[i]
        
        # Binary search for complement in the remaining array
        left, right = i + 1, n - 1
        
        while left <= right:
            mid = left + (right - left) // 2
            if numbers[mid] == complement:
                return [i + 1, mid + 1]  # Convert to 1-indexed
            elif numbers[mid] < complement:
                left = mid + 1
            else:
                right = mid - 1
    
    # Problem statement guarantees exactly one solution
    return None
```

## 3. Hash Map Solution

### Visual Representation:

```
Create a hash map to store {value: index} as we iterate through the array

Step 1: Check if (target-2)=12 exists in hash map → No, add {2:1} to hash map
        [2, 3, 6, 8, 11, 15]
         ↑
        Hash Map: {2:1}

Step 2: Check if (target-3)=11 exists in hash map → No, add {3:2} to hash map
        [2, 3, 6, 8, 11, 15]
            ↑
        Hash Map: {2:1, 3:2}

Step 3: Check if (target-6)=8 exists in hash map → No, add {6:3} to hash map
        [2, 3, 6, 8, 11, 15]
               ↑
        Hash Map: {2:1, 3:2, 6:3}

Step 4: Check if (target-8)=6 exists in hash map → Yes! Found 6 at index 3
        [2, 3, 6, 8, 11, 15]
                  ↑
        Return [3, 4] (indices of 6 and 8)
```

### Pseudo Code for Hash Map:
```
function twoSum(numbers, target):
    hash_map = empty hash map
    
    for i = 0 to n-1:
        complement = target - numbers[i]
        
        if complement exists in hash_map:
            return [hash_map[complement], i+1]  # Convert to 1-indexed
            
        hash_map[numbers[i]] = i+1  # Store 1-indexed position
        
    return null  # No solution found
```

### Python Code for Hash Map:
```python
def twoSum_hash_map(numbers, target):
    """
    Hash map approach to find two numbers in a sorted array that add up to the
    target value.
    
    Args:
        numbers: A list of integers sorted in non-decreasing order
        target: An integer representing the target sum
        
    Returns:
        A list containing two 1-indexed integers representing the positions of
        the two numbers that add up to the target
    
    Time Complexity: O(n) - we go through the array once
    Space Complexity: O(n) - we use a hash map that could store up to n elements
    
    Note: This solution doesn't take advantage of the array being sorted!
    """
    num_to_index = {}  # Dictionary to store value -> index mapping
    
    for i, num in enumerate(numbers):
        complement = target - num
        
        if complement in num_to_index:
            return [num_to_index[complement], i + 1]  # Convert to 1-indexed
            
        num_to_index[num] = i + 1  # Store 1-indexed positions
    
    # Problem statement guarantees exactly one solution
    return None
```

## 4. Two Pointers Solution 

### Visual Representation:

```
Start with two pointers: left at beginning, right at end

Step 1: left=0, right=5 → numbers[0]+numbers[5] = 2+15 = 17 > 14
        [2, 3, 6, 8, 11, 15]
         ↑              ↑
         L              R
        Sum = 17 > target, so move R left

Step 2: left=0, right=4 → numbers[0]+numbers[4] = 2+11 = 13 < 14
        [2, 3, 6, 8, 11, 15]
         ↑           ↑
         L           R
        Sum = 13 < target, so move L right

Step 3: left=1, right=4 → numbers[1]+numbers[4] = 3+11 = 14 = 14
        [2, 3, 6, 8, 11, 15]
            ↑        ↑
            L        R
        Sum = 14 = target, solution found! Return [2, 5]
```

### Pseudo Code for Two Pointers:
```
function twoSum(numbers, target):
    left = 0
    right = length of numbers - 1
    
    while left < right:
        sum = numbers[left] + numbers[right]
        
        if sum == target:
            return [left+1, right+1]  # Convert to 1-indexed
        else if sum < target:
            left += 1  # Need a larger sum, move left pointer right
        else:
            right -= 1  # Need a smaller sum, move right pointer left
            
    return null  # No solution found
```

### Python Code for Two Pointers:
```python
def twoSum_two_pointers(numbers, target):
    """
    Two pointers approach to find two numbers in a sorted array that add up to
    the target value.
    
    Args:
        numbers: A list of integers sorted in non-decreasing order
        target: An integer representing the target sum
        
    Returns:
        A list containing two 1-indexed integers representing the positions of
        the two numbers that add up to the target
    
    Time Complexity: O(n) - in the worst case we traverse the array once
    Space Complexity: O(1) - we use constant extra space
    
    This approach takes advantage of the array being sorted!
    """
    left, right = 0, len(numbers) - 1
    
    while left < right:
        current_sum = numbers[left] + numbers[right]
        
        if current_sum == target:
            return [left + 1, right + 1]  # Convert to 1-indexed
        elif current_sum < target:
            left += 1  # Need a larger sum, move left pointer right
        else:
            right -= 1  # Need a smaller sum, move right pointer left
    
    # Problem statement guarantees exactly one solution
    return None
```

## Emoji Summary of Approaches

```
Brute Force:  🐢 O(n²) time, O(1) space
              👉 Check all possible pairs - simplest but slowest

Binary Search: 🔍 O(n log n) time, O(1) space
              👉 For each element, search for its complement

Hash Map:      📝 O(n) time, O(n) space  
              👉 Store visited elements to find complements quickly

Two Pointers:  ⚡ O(n) time, O(1) space
              👉 Best solution! Uses sorted property of array
```

## Hinglish Explanation

Jab hum "Two Sum II" problem ko solve karte hain, toh humein ek sorted array mein
se do aise numbers dhoondhne hote hain jinki value ka sum ek given target ke
barabar ho. Hume yeh bhi ensure karna hota hai ki indices 1-indexed hon, aur
index1 < index2 ho.

Is problem ko solve karne ke liye main 4 tareeke hai:

1. **Brute Force (O(n²))**: Har possible pair check karo. Simple but slow hai.

2. **Binary Search (O(n log n))**: Har element ke liye, uska complement (target - 
   element) array mein binary search se dhoondhte hain.

3. **Hash Map (O(n))**: Hash map mein elements store karke unke complements fast
   dhoondhte hain. Space complexity O(n) hai.

4. **Two Pointers (O(n))**: Best solution! Sorted array ka faayda uthake, 2 pointers
   (left & right) se array traverse karte hain aur sum ke hisaab se pointers move 
   karte hain. Is approach mein time complexity O(n) aur space complexity O(1) hai.

Sorted array hone ka sabse bada faayda Two Pointers approach mein milta hai!

