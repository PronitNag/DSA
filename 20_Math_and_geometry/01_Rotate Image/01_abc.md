# Rotate Image Solution

## Problem Statement

**48. Rotate Image**

You are given an n x n 2D matrix representing an image, rotate the image by 90 degrees (clockwise).

You have to rotate the image in-place, which means you have to modify the input 2D matrix directly. DO NOT allocate another 2D matrix and do the rotation.

## Visualization of 90-Degree Clockwise Rotation

Original Matrix:
```
┌───┬───┬───┐
│ 1 │ 2 │ 3 │
├───┼───┼───┤
│ 4 │ 5 │ 6 │
├───┼───┼───┤
│ 7 │ 8 │ 9 │
└───┴───┴───┘
```

After 90° Clockwise Rotation:
```
┌───┬───┬───┐
│ 7 │ 4 │ 1 │
├───┼───┼───┤
│ 8 │ 5 │ 2 │
├───┼───┼───┤
│ 9 │ 6 │ 3 │
└───┴───┴───┘
```

## Solution 1: Brute Force Approach

### ASCII Art Visualization

```
Original:                           Creating New Matrix:              Final Result:
                                    
┌───┬───┬───┐                      ┌───┬───┬───┐                    ┌───┬───┬───┐
│ 1 │ 2 │ 3 │                      │   │   │   │                    │ 7 │ 4 │ 1 │
├───┼───┼───┤                      ├───┼───┼───┤                    ├───┼───┼───┤
│ 4 │ 5 │ 6 │  ===Transform===>    │   │   │   │  ===Copy Back===>  │ 8 │ 5 │ 2 │
├───┼───┼───┤                      ├───┼───┼───┤                    ├───┼───┼───┤
│ 7 │ 8 │ 9 │                      │   │   │   │                    │ 9 │ 6 │ 3 │
└───┴───┴───┘                      └───┴───┴───┘                    └───┴───┴───┘

Transformation Relation: new_matrix[j][n-1-i] = matrix[i][j]

Example: matrix[0][0]=1 → new_matrix[0][2]=1
         matrix[0][1]=2 → new_matrix[1][2]=2
         matrix[0][2]=3 → new_matrix[2][2]=3
```

### Pseudo Code
```
function rotate_brute_force(matrix):
    n = length of matrix
    Create a new_matrix of size n×n
    
    for i from 0 to n-1:
        for j from 0 to n-1:
            new_matrix[j][n-1-i] = matrix[i][j]
    
    Copy new_matrix back to original matrix
```

### Python Implementation
```python
def rotate_brute_force(matrix):
    """
    Rotates the given n x n matrix by 90 degrees clockwise using brute force.
    
    Args:
        matrix: A square matrix represented as a list of lists.
        
    Time Complexity: O(n²) where n is the dimension of the matrix.
    Space Complexity: O(n²) for the temporary matrix.
    
    Note: This solution uses extra space so it doesn't fulfill the in-place
    requirement, but it's included for comparison.
    """
    n = len(matrix)
    # Create a new matrix to store the rotated values temporarily
    new_matrix = [[0] * n for _ in range(n)]
    
    # Fill new matrix with the rotated values according to the transformation
    for i in range(n):
        for j in range(n):
            new_matrix[j][n-1-i] = matrix[i][j]
    
    # Copy back values from new_matrix to original matrix
    for i in range(n):
        for j in range(n):
            matrix[i][j] = new_matrix[i][j]
```

## Solution 2: Rotate Four Cells at Once

### ASCII Art Visualization

```
┌───┬───┬───┐
│ 1 │ 2 │ 3 │   For each group of 4 cells, we perform a 4-way swap:
├───┼───┼───┤
│ 4 │ 5 │ 6 │     1───→──3      7───→──1
├───┼───┼───┤     │     │       ↑     ↓
│ 7 │ 8 │ 9 │     ↓     ↑       │     │
└───┴───┴───┘     7───←──9      9───←──3


Rotating groups of 4 elements in one cycle:
(0,0)→(0,2)→(2,2)→(2,0)→(0,0) : [1→3→9→7→1]
(0,1)→(1,2)→(2,1)→(1,0)→(0,1) : [2→6→8→4→2]

Final result after all rotations:
┌───┬───┬───┐
│ 7 │ 4 │ 1 │
├───┼───┼───┤
│ 8 │ 5 │ 2 │
├───┼───┼───┤
│ 9 │ 6 │ 3 │
└───┴───┴───┘
```

### Pseudo Code
```
function rotate_four_cells(matrix):
    n = length of matrix
    
    for i from 0 to (n/2)-1:
        for j from i to n-i-2:
            # Save top left
            temp = matrix[i][j]
            
            # Move bottom left to top left
            matrix[i][j] = matrix[n-1-j][i]
            
            # Move bottom right to bottom left
            matrix[n-1-j][i] = matrix[n-1-i][n-1-j]
            
            # Move top right to bottom right
            matrix[n-1-i][n-1-j] = matrix[j][n-1-i]
            
            # Place saved top left to top right
            matrix[j][n-1-i] = temp
```

### Python Implementation
```python
def rotate_four_cells(matrix):
    """
    Rotates the given n x n matrix by 90 degrees clockwise by rotating
    four cells at once.
    
    Args:
        matrix: A square matrix represented as a list of lists.
        
    Time Complexity: O(n²) where n is the dimension of the matrix.
    Space Complexity: O(1) as we only use one temp variable.
    
    How it works:
    - For each position (i,j) in the first quadrant, we perform a 4-way swap
    - We rotate the elements at (i,j), (j,n-1-i), (n-1-i,n-1-j), (n-1-j,i)
    """
    n = len(matrix)
    
    # We iterate through each layer of the matrix
    for i in range(n // 2):
        # And then through each element in the current layer
        for j in range(i, n - i - 1):
            # Save the top left value
            temp = matrix[i][j]
            
            # Move bottom left to top left
            matrix[i][j] = matrix[n-1-j][i]
            
            # Move bottom right to bottom left
            matrix[n-1-j][i] = matrix[n-1-i][n-1-j]
            
            # Move top right to bottom right
            matrix[n-1-i][n-1-j] = matrix[j][n-1-i]
            
            # Place saved top left value to top right
            matrix[j][n-1-i] = temp
```

## Solution 3: Transpose and Reverse

### ASCII Art Visualization

```
Original:          After Transpose:     After Reversing Rows:
┌───┬───┬───┐      ┌───┬───┬───┐        ┌───┬───┬───┐
│ 1 │ 2 │ 3 │      │ 1 │ 4 │ 7 │        │ 7 │ 4 │ 1 │
├───┼───┼───┤      ├───┼───┼───┤        ├───┼───┼───┤
│ 4 │ 5 │ 6 │  =>  │ 2 │ 5 │ 8 │   =>   │ 8 │ 5 │ 2 │
├───┼───┼───┤      ├───┼───┼───┤        ├───┼───┼───┤
│ 7 │ 8 │ 9 │      │ 3 │ 6 │ 9 │        │ 9 │ 6 │ 3 │
└───┴───┴───┘      └───┴───┴───┘        └───┴───┴───┘

Transpose: swap matrix[i][j] with matrix[j][i] for all i < j
Reverse rows: for each row, reverse the elements
```

### Pseudo Code
```
function rotate_transpose_reverse(matrix):
    n = length of matrix
    
    # Transpose the matrix
    for i from 0 to n-1:
        for j from i+1 to n-1:
            swap matrix[i][j] and matrix[j][i]
    
    # Reverse each row
    for i from 0 to n-1:
        reverse the row matrix[i]
```

### Python Implementation
```python
def rotate_transpose_reverse(matrix):
    """
    Rotates the given n x n matrix by 90 degrees clockwise using the transpose
    and reverse method.
    
    Args:
        matrix: A square matrix represented as a list of lists.
        
    Time Complexity: O(n²) where n is the dimension of the matrix.
    Space Complexity: O(1) as we modify the matrix in-place.
    
    How it works:
    - First, we transpose the matrix (swap rows with columns)
    - Then, we reverse each row of the transposed matrix
    """
    n = len(matrix)
    
    # Step 1: Transpose the matrix
    for i in range(n):
        for j in range(i + 1, n):
            # Swap matrix[i][j] with matrix[j][i]
            matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
    
    # Step 2: Reverse each row
    for i in range(n):
        # Python's way to reverse a list or part of a list in-place
        matrix[i].reverse()
```

## Comparison of Solutions

### Brute Force
- **Pros**: Simple to understand and implement
- **Cons**: Uses O(n²) extra space, not truly in-place
- **Time Complexity**: O(n²)
- **Space Complexity**: O(n²)

### Rotate Four Cells
- **Pros**: Truly in-place solution with O(1) extra space
- **Cons**: More complex logic and harder to understand
- **Time Complexity**: O(n²)
- **Space Complexity**: O(1)

### Transpose and Reverse
- **Pros**: In-place solution with clearer logic than "rotate four cells"
- **Cons**: Still requires stepping through the entire matrix
- **Time Complexity**: O(n²)
- **Space Complexity**: O(1)

## Hinglish Note

Iss problem mein hume ek square matrix ko 90 degrees clockwise rotate karna hai.
Maine teen different tarike se solution provide kiya hai:

Pehla tarika brute force hai jisme hum ek naya matrix banate hain aur rotation
ka formula apply karte hain. Lekin ismein extra space lagta hai, jo question ki
requirement ke against hai.

Dusra tarika "four cells rotation" hai jahan hum matrix ke har layer ke har
element ko consider karte hain aur 4 positions ko ek saath swap karte hain.
Yeh solution completely in-place hai.

Teesra tarika "transpose and reverse" hai jismein pehle hum matrix ko transpose
karte hain (rows ko columns aur columns ko rows banate hain) aur phir har row ko
reverse kar dete hain. Yeh bhi in-place hai aur second solution se samajhne mein
aasan hai.

Teeno solutions ka time complexity O(n²) hai, lekin space complexity mein
difference hai. Brute force O(n²) space leta hai, jabki baaki dono O(1) space
lete hain.

