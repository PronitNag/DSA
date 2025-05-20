# Spiral Matrix Solution

## Problem Statement
Given an m x n matrix of integers matrix, return a list of all elements within the matrix in spiral order.

## Visualization with ASCII Art

Let's visualize the spiral traversal using a 4x4 matrix:

```
START
  ↓
  1 → 2 → 3 → 4
              ↓
  5 → 6 → 7   8
  ↑         ↓
  9 ← 10 ← 11  12
  ↑            ↓
  13 ← 14 ← 15 ← 16
                  ↑
                 END
```

The traversal begins at the top-left element (1) and proceeds in a spiral pattern:
1. Move right: 1 → 2 → 3 → 4
2. Move down: 4 → 8 → 12 → 16
3. Move left: 16 → 15 → 14 → 13
4. Move up: 13 → 9 → 5
5. Move right again: 5 → 6 → 7
6. Move down: 7 → 11
7. Move left: 11 → 10
8. End at 10

The result would be: [1, 2, 3, 4, 8, 12, 16, 15, 14, 13, 9, 5, 6, 7, 11, 10]

### Progressing Through the Algorithm

Let's see how the problem evolves with each step:

**Step 1: Process the top row**
```
1 → 2 → 3 → 4    [Visited: 1,2,3,4]
↓   ↓   ↓   ↓
5   6   7   8
↓   ↓   ↓   ↓
9   10  11  12
↓   ↓   ↓   ↓
13  14  15  16
```

**Step 2: Process the rightmost column**
```
✓   ✓   ✓   ✓    [Visited: 1,2,3,4,8,12,16]
            ↓
5   6   7   8
            ↓
9   10  11  12
            ↓
13  14  15  16 ←
```

**Step 3: Process the bottom row (right to left)**
```
✓   ✓   ✓   ✓    [Visited: 1,2,3,4,8,12,16,15,14,13]
                
5   6   7   ✓
                
9   10  11  ✓
                
13 ← 14 ← 15 ← ✓
```

**Step 4: Process the leftmost column (bottom to top)**
```
✓   ✓   ✓   ✓    [Visited: 1,2,3,4,8,12,16,15,14,13,9,5]
↑               
5   6   7   ✓
↑               
9   10  11  ✓
↑               
✓   ✓   ✓   ✓
```

**Step 5: Process the inner top row (left to right)**
```
✓   ✓   ✓   ✓    [Visited: 1,2,3,4,8,12,16,15,14,13,9,5,6,7]
                
✓ → 6 → 7   ✓
                
✓   10  11  ✓
                
✓   ✓   ✓   ✓
```

**Step 6: Process the inner right column and bottom row**
```
✓   ✓   ✓   ✓    [Visited: 1,2,3,4,8,12,16,15,14,13,9,5,6,7,11,10]
                
✓   ✓   ✓   ✓
        ↓    
✓ ← 10 ← 11  ✓
                
✓   ✓   ✓   ✓    DONE!
```

## Solution 1: Recursion Approach

Recursive approach solves the problem by processing the outer layer of the matrix, then recursively processing the inner matrix.

### Pseudo Code (Recursion)
```
function spiralOrder(matrix):
    if matrix is empty:
        return empty list
    
    result = []
    
    function spiral(row_start, row_end, col_start, col_end):
        if row_start > row_end or col_start > col_end:
            return
        
        # Process top row
        for col from col_start to col_end:
            add matrix[row_start][col] to result
        
        # Process rightmost column
        for row from row_start+1 to row_end:
            add matrix[row][col_end] to result
        
        # Process bottom row (if there's more than one row)
        if row_start < row_end:
            for col from col_end-1 down to col_start:
                add matrix[row_end][col] to result
        
        # Process leftmost column (if there's more than one column)
        if col_start < col_end:
            for row from row_end-1 down to row_start+1:
                add matrix[row][col_start] to result
        
        # Recursively process the inner matrix
        spiral(row_start+1, row_end-1, col_start+1, col_end-1)
    
    # Start the recursion
    spiral(0, len(matrix)-1, 0, len(matrix[0])-1)
    return result
```

### Python Code (Recursion)
```python
def spiralOrder(matrix):
    """
    Recursive solution for spiral matrix traversal.
    Hum matrix ko spiral order mein traverse karenge, starting from outer layer,
    aur gradually inner layers tak jaayenge.
    
    Args:
        matrix: Input matrix containing integers
        
    Returns:
        List of elements in spiral order
    """
    if not matrix:
        return []
    
    result = []
    
    def spiral(r1, r2, c1, c2):
        # Base case: if boundaries cross, we're done
        if r1 > r2 or c1 > c2:
            return
        
        # Process top row - left to right
        for c in range(c1, c2 + 1):
            result.append(matrix[r1][c])
        
        # Process rightmost column - top to bottom
        for r in range(r1 + 1, r2 + 1):
            result.append(matrix[r][c2])
        
        # Process bottom row - right to left (if there are at least 2 rows)
        if r1 < r2:
            for c in range(c2 - 1, c1 - 1, -1):
                result.append(matrix[r2][c])
        
        # Process leftmost column - bottom to top (if there are at least 2 cols)
        if c1 < c2:
            for r in range(r2 - 1, r1, -1):
                result.append(matrix[r][c1])
        
        # Recursively process the inner matrix
        spiral(r1 + 1, r2 - 1, c1 + 1, c2 - 1)
    
    spiral(0, len(matrix) - 1, 0, len(matrix[0]) - 1)
    return result
```

## Solution 2: Iteration Approach

The iterative approach is similar to the recursive approach, but uses a loop instead of recursion.

### Pseudo Code (Iteration)
```
function spiralOrder(matrix):
    if matrix is empty:
        return empty list
    
    result = []
    row_start = 0, row_end = height-1
    col_start = 0, col_end = width-1
    
    while row_start <= row_end and col_start <= col_end:
        # Process top row
        for col from col_start to col_end:
            add matrix[row_start][col] to result
        row_start += 1
        
        # Process rightmost column
        for row from row_start to row_end:
            add matrix[row][col_end] to result
        col_end -= 1
        
        # Process bottom row (if there's still a row to process)
        if row_start <= row_end:
            for col from col_end down to col_start:
                add matrix[row_end][col] to result
            row_end -= 1
        
        # Process leftmost column (if there's still a column to process)
        if col_start <= col_end:
            for row from row_end down to row_start:
                add matrix[row][col_start] to result
            col_start += 1
    
    return result
```

### Python Code (Iteration)
```python
def spiralOrder(matrix):
    """
    Iterative solution for spiral matrix traversal.
    Is approach mein hum loop ka use karke outer layers se inner layers tak
    traverse karenge, boundary variables ko update karte hue.
    
    Args:
        matrix: Input matrix containing integers
        
    Returns:
        List of elements in spiral order
    """
    if not matrix:
        return []
    
    result = []
    r1, r2 = 0, len(matrix) - 1  # Row boundaries (top, bottom)
    c1, c2 = 0, len(matrix[0]) - 1  # Column boundaries (left, right)
    
    while r1 <= r2 and c1 <= c2:
        # Process top row - left to right
        for c in range(c1, c2 + 1):
            result.append(matrix[r1][c])
        r1 += 1
        
        # Process rightmost column - top to bottom
        for r in range(r1, r2 + 1):
            result.append(matrix[r][c2])
        c2 -= 1
        
        # Process bottom row - right to left (if there are rows left)
        if r1 <= r2:
            for c in range(c2, c1 - 1, -1):
                result.append(matrix[r2][c])
            r2 -= 1
        
        # Process leftmost column - bottom to top (if there are columns left)
        if c1 <= c2:
            for r in range(r2, r1 - 1, -1):
                result.append(matrix[r][c1])
            c1 += 1
    
    return result
```

## Solution 3: Iteration (Optimal) - Direction-based Approach

This approach uses direction vectors to simulate the spiral traversal, changing directions when necessary.

### Pseudo Code (Optimal Iteration)
```
function spiralOrder(matrix):
    if matrix is empty:
        return empty list
    
    rows = len(matrix), cols = len(matrix[0])
    result = []
    
    # Define direction vectors: right, down, left, up
    directions = [(0,1), (1,0), (0,-1), (-1,0)]
    direction_idx = 0  # Start by going right
    
    row = 0, col = 0
    visited = create a rows x cols matrix filled with False
    
    for i from 0 to rows*cols-1:
        result.append(matrix[row][col])
        visited[row][col] = True
        
        # Calculate next position
        next_row = row + directions[direction_idx][0]
        next_col = col + directions[direction_idx][1]
        
        # Change direction if next position is out of bounds or already visited
        if (next_row < 0 or next_row >= rows or 
            next_col < 0 or next_col >= cols or 
            visited[next_row][next_col]):
            
            direction_idx = (direction_idx + 1) % 4
            next_row = row + directions[direction_idx][0]
            next_col = col + directions[direction_idx][1]
        
        row, col = next_row, next_col
    
    return result
```

### Python Code (Optimal Iteration)
```python
def spiralOrder(matrix):
    """
    Optimal iterative solution using direction vectors.
    Is approach mein hum direction vectors (right, down, left, up) ka use
    karenge aur jab bhi zaroorat ho direction change karenge.
    
    Args:
        matrix: Input matrix containing integers
        
    Returns:
        List of elements in spiral order
    """
    if not matrix:
        return []
    
    rows, cols = len(matrix), len(matrix[0])
    result = []
    
    # Create a matrix to track visited cells
    visited = [[False for _ in range(cols)] for _ in range(rows)]
    
    # Direction vectors: right, down, left, up
    directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]
    dir_idx = 0  # Start with going right
    
    row, col = 0, 0  # Start at the top-left cell
    
    # We need to visit exactly rows*cols cells
    for _ in range(rows * cols):
        result.append(matrix[row][col])
        visited[row][col] = True
        
        # Calculate the next position
        next_row = row + directions[dir_idx][0]
        next_col = col + directions[dir_idx][1]
        
        # Change direction if we hit a boundary or visited cell
        if (next_row < 0 or next_row >= rows or next_col < 0 or 
            next_col >= cols or visited[next_row][next_col]):
            
            # Change direction (rotate 90 degrees clockwise)
            dir_idx = (dir_idx + 1) % 4
            next_row = row + directions[dir_idx][0]
            next_col = col + directions[dir_idx][1]
        
        # Move to the next cell
        row, col = next_row, next_col
    
    return result
```

## Time and Space Complexity Analysis

For all three solutions:
- **Time Complexity**: O(m×n) where m is the number of rows and n is the number of columns in the matrix. We visit each cell exactly once.
- **Space Complexity**: 
  - For recursion: O(m+n) extra space due to the recursion stack
  - For basic iteration: O(1) extra space (not counting the output array)
  - For optimal iteration: O(m×n) extra space due to the visited matrix

## Comparison of Approaches

1. **Recursion Approach**:
   - Pros: Clean and intuitive code
   - Cons: Potential stack overflow for large matrices, extra space for call stack

2. **Iteration Approach**:
   - Pros: No recursion overhead, straightforward boundary management
   - Cons: Multiple boundary variables to track

3. **Optimal Iteration (Direction-based)**:
   - Pros: Elegant solution using direction vectors, easier to visualize
   - Cons: Requires extra space for tracking visited cells

## Emoji Art Visualization

Let's represent the spiral traversal using emojis:

```
🚀 → 🔄 → 🔄 → 🔄
↑                ↓
🔄     🎯     🔄
↑                ↓
🔄 ← 🔄 ← 🔄 ← 🔄
```

Where:
- 🚀 represents the starting point
- 🎯 represents the center/target
- 🔄 represents the traversal path
- Arrows show the direction of movement

Happy coding! 😊

