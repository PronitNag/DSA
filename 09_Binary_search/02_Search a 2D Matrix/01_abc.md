# Search a 2D Matrix - Visual Solutions

## Problem Statement

You are given an m × n integer matrix with two properties:
1. Each row is sorted in non-decreasing order
2. The first integer of each row is greater than the last integer of the previous row

Given an integer target, return true if the target is in the matrix or false otherwise.
Your solution must have O(log(m * n)) time complexity.

### Sample Matrix Visualization:

```
┌───┬───┬───┬───┐
│ 1 │ 3 │ 5 │ 7 │  ← Row 0: Sorted (1 < 3 < 5 < 7)
├───┼───┼───┼───┤
│ 10│ 11│ 16│ 20│  ← Row 1: Sorted (10 < 11 < 16 < 20)
├───┼───┼───┼───┤
│ 23│ 30│ 34│ 60│  ← Row 2: Sorted (23 < 30 < 34 < 60)
└───┴───┴───┴───┘
  ↑           ↑
  First       Last
  
Note: 7 (last of Row 0) < 10 (first of Row 1) < 20 (last of Row 1) < 23 (first of Row 2)
```

## Solution 1: Brute Force 🔨

### Visual Representation:

```
┌───┬───┬───┬───┐
│🔍1│🔍3│🔍5│🔍7│  Row 0: Check every element one by one
├───┼───┼───┼───┤
│🔍10│🔍11│🔍16│🔍20│  Row 1: Continue checking each element
├───┼───┼───┼───┤
│🔍23│🔍30│🔍34│🔍60│  Row 2: Check remaining elements
└───┴───┴───┴───┘
     ↑
     Found! (if target = 11)
```

### Pseudo Code:
```
for each row in matrix:
    for each element in row:
        if element equals target:
            return True
return False
```

### Python Code:

```python
def searchMatrix_brute_force(matrix, target):
    """
    Ye function matrix mein target ko dhoondne ke liye brute force approach
    use karta hai. Har ek element ko check karta hai.
    
    Time Complexity: O(m * n) - jahan m rows ki sankhya aur n columns ki 
    sankhya hai
    Space Complexity: O(1) - koi additional space use nahi hota
    
    :param matrix: m x n sorted integer matrix
    :param target: dhoondha jaane wala integer
    :return: True agar target mile, False agar na mile
    """
    # Matrix ke har row aur column ko check karo
    for row in matrix:
        for element in row:
            # Agar element target ke barabar hai to True return karo
            if element == target:
                return True
    
    # Agar saare elements check karne ke baad bhi target nahi mila to False
    # return karo
    return False
```

## Solution 2: Staircase Search 🪜

### Visual Representation:

```
┌───┬───┬───┬───┐
│   │   │   │🔍7│ Start at top-right corner
├───┼───┼───┼───┤
│   │   │🔍16│⬇️ │ If target < current, move left
├───┼───┼───┼───┤
│   │🔍30│⬅️ │   │ If target > current, move down
└───┴───┴───┴───┘
      ↑
    Found! (if target = 30)
```

### Pseudo Code:
```
Start from top-right corner (row = 0, col = n-1)
While within bounds:
    if current element equals target:
        return True
    if current element > target:
        move left (col--)
    else:
        move down (row++)
return False
```

### Python Code:

```python
def searchMatrix_staircase(matrix, target):
    """
    Ye function 'staircase search' approach use karta hai, jisme hum matrix
    ke top-right corner se shuru karte hain aur har step par ya to left ya 
    down jaate hain.
    
    Time Complexity: O(m + n) - worst case mein m rows aur n columns travel 
    kar sakte hain
    Space Complexity: O(1) - sirf pointers store karte hain
    
    :param matrix: m x n sorted integer matrix
    :param target: dhoondha jaane wala integer
    :return: True agar target mile, False agar na mile
    """
    if not matrix or not matrix[0]:  # Check if matrix is empty
        return False
    
    rows, cols = len(matrix), len(matrix[0])
    row, col = 0, cols - 1  # Top-right corner se shuruat karo
    
    # Jab tak matrix ke andar hain, search karte raho
    while row < rows and col >= 0:
        current = matrix[row][col]
        
        if current == target:  # Target mil gaya
            return True
        elif current > target:  # Current element target se bada hai
            col -= 1  # Left jao
        else:  # Current element target se chota hai
            row += 1  # Neeche jao
    
    # Target nahi mila
    return False
```

## Solution 3: Binary Search (Two Pass) 🔍🔍

### Visual Representation:

```
Step 1: Find the row using binary search on first column

┌───┬───┬───┬───┐
│🔍1│   │   │   │  
├───┼───┼───┼───┤  Check if target is in range of Row 1
│🔍10│   │   │   │  10 ≤ target=16 ≤ 20? Yes!
├───┼───┼───┼───┤
│🔍23│   │   │   │  
└───┴───┴───┴───┘

Step 2: Binary search within the identified row

┌───┬───┬───┬───┐
│   │   │   │   │  
├───┼───┼───┼───┤
│🔍10│🔍11│🔍16│🔍20│  Binary search in Row 1
├───┼───┼───┼───┤       ↑ Found target!
│   │   │   │   │  
└───┴───┴───┴───┘
```

### Pseudo Code:
```
1. Use binary search to find the row where target might be located
   (Check if target is between first and last element of each row)
2. Use binary search on that row to find the target
```

### Python Code:

```python
def searchMatrix_binary_search(matrix, target):
    """
    Ye function do binary searches use karta hai:
    1. Pehle row ko find karne ke liye
    2. Phir us row mein target ko find karne ke liye
    
    Time Complexity: O(log m + log n) = O(log(m*n))
    Space Complexity: O(1) - koi extra space nahi use karte
    
    :param matrix: m x n sorted integer matrix
    :param target: dhoondha jaane wala integer
    :return: True agar target mile, False agar na mile
    """
    if not matrix or not matrix[0]:  # Empty matrix check
        return False
        
    rows, cols = len(matrix), len(matrix[0])
    
    # Step 1: Binary search to find the correct row
    low, high = 0, rows - 1
    while low <= high:
        mid = (low + high) // 2
        
        # Check if target is in range of this row
        if matrix[mid][0] <= target <= matrix[mid][cols - 1]:
            # Target is in this row, proceed to step 2
            target_row = mid
            break
        elif matrix[mid][0] > target:
            high = mid - 1  # Target might be in upper rows
        else:
            low = mid + 1   # Target might be in lower rows
    else:
        # Target is not in any row's range
        return False
    
    # Step 2: Binary search in the identified row
    low, high = 0, cols - 1
    while low <= high:
        mid = (low + high) // 2
        
        if matrix[target_row][mid] == target:
            return True
        elif matrix[target_row][mid] > target:
            high = mid - 1
        else:
            low = mid + 1
            
    return False
```

## Solution 4: Binary Search (One Pass) 🔍

### Visual Representation:

```
┌───┬───┬───┬───┐   Treat the matrix as a sorted array with virtual
│ 0 │ 1 │ 2 │ 3 │   indices. For example: element at (r,c) has virtual
├───┼───┼───┼───┤   index (r*cols+c)
│ 4 │ 5 │ 6 │ 7 │    
├───┼───┼───┼───┤   For example, with target=16 (at index 6):
│ 8 │ 9 │10 │11 │
└───┴───┴───┴───┘   0...5...11  (Binary search on virtual index)
                      ↑      
                    Found at index 6 (row=1, col=2)
```

### Pseudo Code:
```
1. Treat the 2D matrix as a 1D sorted array with indices 0 to (m*n-1)
2. Perform binary search on this virtual 1D array
3. Convert virtual mid index to actual row,col using:
   - row = mid / cols
   - col = mid % cols
```

### Python Code:

```python
def searchMatrix_binary_search_one_pass(matrix, target):
    """
    Ye function 2D matrix ko virtual 1D array ki tarah treat karke ek hi 
    binary search mein solution provide karta hai.
    
    Time Complexity: O(log(m*n)) - single binary search on entire matrix
    Space Complexity: O(1) - koi extra space nahi use karte
    
    :param matrix: m x n sorted integer matrix
    :param target: dhoondha jaane wala integer
    :return: True agar target mile, False agar na mile
    """
    if not matrix or not matrix[0]:  # Empty matrix check
        return False
        
    rows, cols = len(matrix), len(matrix[0])
    
    # Virtual 1D array par binary search karenge
    left, right = 0, rows * cols - 1
    
    while left <= right:
        mid = (left + right) // 2
        
        # Virtual index ko actual row, col mein convert karna
        r = mid // cols  # Row index
        c = mid % cols   # Column index
        
        # Compare matrix element with target
        if matrix[r][c] == target:
            return True
        elif matrix[r][c] < target:
            left = mid + 1  # Right half mein search karo
        else:
            right = mid - 1  # Left half mein search karo
            
    return False
```

## Comparison of All Solutions 📊

| Solution                | Time Complexity  | Space Complexity | Comments                                   |
|------------------------|------------------|------------------|-------------------------------------------|
| Brute Force            | O(m * n)         | O(1)             | Simple but inefficient                     |
| Staircase Search       | O(m + n)         | O(1)             | Better than brute force                    |
| Binary Search (2-pass) | O(log m + log n) | O(1)             | Meets requirement of O(log(m*n))           |
| Binary Search (1-pass) | O(log(m*n))      | O(1)             | Most elegant solution, meets requirement   |

## Final Recommendation 🌟

Binary Search (One Pass) is the most elegant solution with O(log(m*n)) time complexity, which meets the problem requirement. It treats the 2D matrix as a virtual 1D sorted array and performs a single binary search.

```
┌───┬───┬───┬───┐         ┌───┐
│ 1 │ 3 │ 5 │ 7 │         │ 1 │
├───┼───┼───┼───┤         ├───┤
│ 10│ 11│ 16│ 20│  ====>  │ 3 │  Virtual 1D array
├───┼───┼───┼───┤         ├───┤
│ 23│ 30│ 34│ 60│         │ 5 │
└───┴───┴───┴───┘         │...│
                          └───┘
```

