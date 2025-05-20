# LeetCode Problem 73: Set Matrix Zeroes

## Problem Statement

Given an m x n integer matrix `matrix`, if an element is 0, set its entire row and column to 0's. You must do it in place.

## Visual Solution with ASCII Art

### Input Matrix Example:
```
┌───┬───┬───┐
│ 1 │ 1 │ 1 │
├───┼───┼───┤
│ 1 │ 0 │ 1 │
├───┼───┼───┤
│ 1 │ 1 │ 1 │
└───┴───┴───┘
```

### Expected Output:
```
┌───┬───┬───┐
│ 1 │ 0 │ 1 │
├───┼───┼───┤
│ 0 │ 0 │ 0 │
├───┼───┼───┤
│ 1 │ 0 │ 1 │
└───┴───┴───┘
```

## Solution 1: Brute Force Approach 💪

### Visual Representation:

1️⃣ Original Matrix:
```
┌───┬───┬───┐
│ 1 │ 1 │ 1 │
├───┼───┼───┤
│ 1 │ 0 │ 1 │ ← Found zero at (1,1)!
├───┼───┼───┤
│ 1 │ 1 │ 1 │
└───┴───┴───┘
```

2️⃣ Mark cells to be zeroed (using special placeholder -999):
```
┌─────┬─────┬─────┐
│  1  │ -999│  1  │ ← Column marked
├─────┼─────┼─────┤
│-999 │  0  │-999 │ ← Row marked
├─────┼─────┼─────┤
│  1  │ -999│  1  │ ← Column marked
└─────┴─────┴─────┘
```

3️⃣ Convert all marked cells to zeroes:
```
┌───┬───┬───┐
│ 1 │ 0 │ 1 │
├───┼───┼───┤
│ 0 │ 0 │ 0 │
├───┼───┼───┤
│ 1 │ 0 │ 1 │
└───┴───┴───┘
```

### Pseudocode:
```
function setZeroes(matrix):
    m = matrix.height
    n = matrix.width
    
    # First pass: mark rows and columns to be zeroed
    for i from 0 to m-1:
        for j from 0 to n-1:
            if matrix[i][j] == 0:
                # Mark row and column with special value (-999)
                for col from 0 to n-1:
                    if matrix[i][col] != 0:
                        matrix[i][col] = -999
                for row from 0 to m-1:
                    if matrix[row][j] != 0:
                        matrix[row][j] = -999
    
    # Second pass: convert all marked cells to 0
    for i from 0 to m-1:
        for j from 0 to n-1:
            if matrix[i][j] == -999:
                matrix[i][j] = 0
```

### Python Code:
```python
def setZeroes_brute_force(matrix):
    """
    Brute force approach to set entire row and column to zeroes if element is zero.
    Time Complexity: O((m*n) * (m+n)) where m is rows and n is columns
    Space Complexity: O(1) - using in-place modification with placeholder
    
    Args:
        matrix: List[List[int]] - Input matrix to be modified in-place
    """
    if not matrix or not matrix[0]:
        return
    
    m, n = len(matrix), len(matrix[0])
    
    # Use a special marker (-999) assuming it's not part of input
    marker = -999
    
    # First pass: Mark rows and columns to be zeroed
    for i in range(m):
        for j in range(n):
            if matrix[i][j] == 0:
                # Mark this row with marker (except for existing zeroes)
                for col in range(n):
                    if matrix[i][col] != 0:
                        matrix[i][col] = marker
                
                # Mark this column with marker (except for existing zeroes)
                for row in range(m):
                    if matrix[row][j] != 0:
                        matrix[row][j] = marker
    
    # Second pass: Convert all marked cells to zero
    for i in range(m):
        for j in range(n):
            if matrix[i][j] == marker:
                matrix[i][j] = 0
```

## Solution 2: Using Additional Space 💾

### Visual Representation:

1️⃣ Original Matrix:
```
┌───┬───┬───┐    row_zeroes = [0, 0, 0]
│ 1 │ 1 │ 1 │    col_zeroes = [0, 0, 0]
├───┼───┼───┤
│ 1 │ 0 │ 1 │ ← Found zero at (1,1)!
├───┼───┼───┤
│ 1 │ 1 │ 1 │
└───┴───┴───┘
```

2️⃣ After scanning entire matrix:
```
┌───┬───┬───┐    row_zeroes = [0, 1, 0] ← Row 1 has zero
│ 1 │ 1 │ 1 │    col_zeroes = [0, 1, 0] ← Column 1 has zero
├───┼───┼───┤
│ 1 │ 0 │ 1 │
├───┼───┼───┤
│ 1 │ 1 │ 1 │
└───┴───┴───┘
```

3️⃣ Apply zeroes based on markers:
```
┌───┬───┬───┐
│ 1 │ 0 │ 1 │ ← Column 1 zeroed
├───┼───┼───┤
│ 0 │ 0 │ 0 │ ← Row 1 zeroed
├───┼───┼───┤
│ 1 │ 0 │ 1 │ ← Column 1 zeroed
└───┴───┴───┘
```

### Pseudocode:
```
function setZeroes(matrix):
    m = matrix.height
    n = matrix.width
    
    rowZeroes = array of size m, initialized with false
    colZeroes = array of size n, initialized with false
    
    # First pass: mark which rows and columns need to be zeroed
    for i from 0 to m-1:
        for j from 0 to n-1:
            if matrix[i][j] == 0:
                rowZeroes[i] = true
                colZeroes[j] = true
    
    # Second pass: zero out rows
    for i from 0 to m-1:
        if rowZeroes[i]:
            for j from 0 to n-1:
                matrix[i][j] = 0
    
    # Third pass: zero out columns
    for j from 0 to n-1:
        if colZeroes[j]:
            for i from 0 to m-1:
                matrix[i][j] = 0
```

### Python Code:
```python
def setZeroes_extra_space(matrix):
    """
    Sets matrix zeroes using additional space to track affected rows and columns.
    Time Complexity: O(m*n) where m is rows and n is columns
    Space Complexity: O(m+n) for tracking zero rows and columns
    
    Args:
        matrix: List[List[int]] - Input matrix to be modified in-place
    """
    if not matrix or not matrix[0]:
        return
    
    m, n = len(matrix), len(matrix[0])
    
    # Arrays to track which rows and columns have zeroes
    row_zeroes = [False] * m
    col_zeroes = [False] * n
    
    # First pass: identify which rows and columns need to be zeroed
    for i in range(m):
        for j in range(n):
            if matrix[i][j] == 0:
                row_zeroes[i] = True  # Mark this row for zeroing
                col_zeroes[j] = True  # Mark this column for zeroing
    
    # Second pass: zero out the rows
    for i in range(m):
        if row_zeroes[i]:
            for j in range(n):
                matrix[i][j] = 0
    
    # Third pass: zero out the columns
    for j in range(n):
        if col_zeroes[j]:
            for i in range(m):
                matrix[i][j] = 0
```

## Solution 3: Space Optimized (O(1) Space) 🚀

### Visual Representation:

1️⃣ Original Matrix:
```
┌───┬───┬───┐
│ 1 │ 1 │ 1 │ ← Use first row as column markers
├───┼───┼───┤
│ 1 │ 0 │ 1 │ ← Found zero at (1,1)!
├───┼───┼───┤
│ 1 │ 1 │ 1 │
└───┴───┴───┘
  ↑
  Use first column as row markers
```

2️⃣ First check if first row/column have zeroes:
```
┌───┬───┬───┐
│ 1*│ 1 │ 1 │ ← firstRowHasZero = false
├───┼───┼───┤  firstColHasZero = false
│ 1 │ 0 │ 1 │
├───┼───┼───┤  * Using these cells as markers
│ 1 │ 1 │ 1 │
└───┴───┴───┘
```

3️⃣ Mark first row/column based on zeroes in rest of matrix:
```
┌───┬───┬───┐
│ 1 │ 0*│ 1 │ ← Column 1 marked with 0
├───┼───┼───┤
│ 0*│ 0 │ 1 │ ← Row 1 marked with 0
├───┼───┼───┤
│ 1 │ 1 │ 1 │
└───┴───┴───┘
```

4️⃣ Update matrix based on markers (except first row/column):
```
┌───┬───┬───┐
│ 1 │ 0 │ 1 │ ← First row/column not updated yet
├───┼───┼───┤
│ 0 │ 0 │ 0 │ ← Updated based on first cell being 0
├───┼───┼───┤
│ 1 │ 0 │ 1 │ ← Updated based on first row cells
└───┴───┴───┘
```

5️⃣ Finally update first row/column if needed:
```
┌───┬───┬───┐
│ 1 │ 0 │ 1 │ ← First row unchanged (no zeros)
├───┼───┼───┤
│ 0 │ 0 │ 0 │ 
├───┼───┼───┤
│ 1 │ 0 │ 1 │
└───┴───┴───┘
```

### Pseudocode:
```
function setZeroes(matrix):
    m = matrix.height
    n = matrix.width
    
    firstRowHasZero = false
    firstColHasZero = false
    
    # Check if first row has zeroes
    for j from 0 to n-1:
        if matrix[0][j] == 0:
            firstRowHasZero = true
            break
    
    # Check if first column has zeroes
    for i from 0 to m-1:
        if matrix[i][0] == 0:
            firstColHasZero = true
            break
    
    # Use first row and column as markers for remaining matrix
    for i from 1 to m-1:
        for j from 1 to n-1:
            if matrix[i][j] == 0:
                matrix[i][0] = 0  # Mark the row
                matrix[0][j] = 0  # Mark the column
    
    # Zero out rows based on first column marker
    for i from 1 to m-1:
        if matrix[i][0] == 0:
            for j from 1 to n-1:
                matrix[i][j] = 0
    
    # Zero out columns based on first row marker
    for j from 1 to n-1:
        if matrix[0][j] == 0:
            for i from 1 to m-1:
                matrix[i][j] = 0
    
    # Zero out first row if needed
    if firstRowHasZero:
        for j from 0 to n-1:
            matrix[0][j] = 0
    
    # Zero out first column if needed
    if firstColHasZero:
        for i from 0 to m-1:
            matrix[i][0] = 0
```

### Python Code:
```python
def setZeroes_optimized(matrix):
    """
    Space optimized approach using first row/column as markers for O(1) space.
    Is tareeka mein hum first row aur column ko markers ke tarah use karte hain,
    jisse extra space ki zaroorat nahi hoti. Hum sabse pehle check karte hain
    ki first row ya column mein zero hai ya nahi, phir unka use markers ke roop
    mein karte hain baki matrix ke liye.
    
    Time Complexity: O(m*n) where m is rows and n is columns
    Space Complexity: O(1) using first row and column as markers
    
    Args:
        matrix: List[List[int]] - Input matrix to be modified in-place
    """
    if not matrix or not matrix[0]:
        return
    
    m, n = len(matrix), len(matrix[0])
    
    # Check if first row has any zeroes
    first_row_has_zero = False
    for j in range(n):
        if matrix[0][j] == 0:
            first_row_has_zero = True
            break
    
    # Check if first column has any zeroes
    first_col_has_zero = False
    for i in range(m):
        if matrix[i][0] == 0:
            first_col_has_zero = True
            break
    
    # Use first row and column as markers for remaining cells
    for i in range(1, m):
        for j in range(1, n):
            if matrix[i][j] == 0:
                # Mark this row and column for zeroing using first row/column
                matrix[i][0] = 0  # Mark in first column (row marker)
                matrix[0][j] = 0  # Mark in first row (column marker)
    
    # Set zeroes based on markers (for all except first row and column)
    # Process rows first (using first column as marker)
    for i in range(1, m):
        if matrix[i][0] == 0:  # If row marker is set
            for j in range(1, n):
                matrix[i][j] = 0
    
    # Process columns (using first row as marker)
    for j in range(1, n):
        if matrix[0][j] == 0:  # If column marker is set
            for i in range(1, m):
                matrix[i][j] = 0
    
    # Finally handle the first row and column if needed
    if first_row_has_zero:
        for j in range(n):
            matrix[0][j] = 0
            
    if first_col_has_zero:
        for i in range(m):
            matrix[i][0] = 0
```

## Performance Comparison

| Solution                    | Time Complexity       | Space Complexity | Visual                         |
|-----------------------------|----------------------|------------------|--------------------------------|
| Brute Force                 | O((m*n) * (m+n))     | O(1)             | 🐢 Slow but straightforward    |
| Using Additional Space      | O(m*n)               | O(m+n)           | 🚶‍♂️ Good balance               |
| Space Optimized             | O(m*n)               | O(1)             | 🚀 Most efficient solution     |

## Emoji Art Visualization of Algorithm Evolution

### Initial Matrix
```
⬜⬜⬜
⬜0️⃣⬜
⬜⬜⬜
```

### Step 1: Identify Zeroes
```
🔍🔍🔍
🔍0️⃣🔍
🔍🔍🔍
```

### Step 2: Mark Rows/Columns (Space Optimized)
```
⬜0️⃣⬜
0️⃣0️⃣⬜
⬜0️⃣⬜
```

### Final Matrix
```
⬜0️⃣⬜
0️⃣0️⃣0️⃣
⬜0️⃣⬜
```

## Summary

Iss problem ko solve karne ke liye hum teen approaches discuss kiye:

1. **Brute Force**: Hum special marker (-999) use karte hain rows aur columns ko mark karne ke liye. Yeh approach simple hai lekin time complexity O((m*n) * (m+n)) hai.

2. **Additional Space**: Hum do arrays (row_zeroes, col_zeroes) use karte hain track rakhne ke liye ki konsi rows aur columns zero hongi. Time complexity O(m*n) hai aur space complexity O(m+n) hai.

3. **Space Optimized**: Iss approach mein hum first row aur first column ko markers ke tarah use karte hain, jisse extra space O(1) ho jati hai. Time complexity O(m*n) hi rehti hai. Yeh sabse efficient solution hai.

Teeno approaches mein, Space Optimized solution best hai kyunki iska time complexity O(m*n) hai aur space complexity sirf O(1) hai.

