# Valid Sudoku Problem Solution

## Problem Statement

Aapko ek 9 x 9 Sudoku board diya gaya hai. Aapko check karna hai ki kya yeh board valid hai based on following rules:

1. Har row mein digits 1-9 hone chahiye without duplicates
2. Har column mein digits 1-9 hone chahiye without duplicates
3. 9 sub-boxes (3x3 grid) mein digits 1-9 hone chahiye without duplicates

Return `True` if board valid hai, otherwise return `False`

Note: Board ko fully filled ya solvable hona zaruri nahi hai valid hone ke liye.

## ASCII Art Visualization

### Initial Sudoku Board Example:
```
┌───────┬───────┬───────┐
│ 5 3 . │ . . . │ . . . │
│ 6 . . │ 1 9 5 │ . . . │
│ . 9 8 │ . . . │ . 6 . │
├───────┼───────┼───────┤
│ 8 . . │ . 6 . │ . . 3 │
│ 4 . . │ 8 . 3 │ . . 1 │
│ 7 . . │ . 2 . │ . . 6 │
├───────┼───────┼───────┤
│ . 6 . │ . . . │ 2 8 . │
│ . . . │ 4 1 9 │ . . 5 │
│ . . . │ . 8 . │ . 7 9 │
└───────┴───────┴───────┘
```

### Checking Validity:

#### 1️⃣ Row Check:
```
Row 1: [5,3,.,.,.,.,.,.,.]  ✅ No duplicates
Row 2: [6,.,.,1,9,5,.,.,.]  ✅ No duplicates
...
```

#### 2️⃣ Column Check:
```
Col 1: [5,6,.,8,4,7,.,.,.]  ✅ No duplicates
Col 2: [3,.,9,.,.,.,6,.,.]  ✅ No duplicates
...
```

#### 3️⃣ 3x3 Sub-grid Check:
```
┌───────┐       ┌───────┐       ┌───────┐
│ 5 3 . │       │ . . . │       │ . . . │
│ 6 . . │  ✅   │ 1 9 5 │  ✅   │ . . . │  ✅
│ . 9 8 │       │ . . . │       │ . 6 . │
└───────┘       └───────┘       └───────┘

┌───────┐       ┌───────┐       ┌───────┐
│ 8 . . │       │ . 6 . │       │ . . 3 │
│ 4 . . │  ✅   │ 8 . 3 │  ✅   │ . . 1 │  ✅
│ 7 . . │       │ . 2 . │       │ . . 6 │
└───────┘       └───────┘       └───────┘

┌───────┐       ┌───────┐       ┌───────┐
│ . 6 . │       │ . . . │       │ 2 8 . │
│ . . . │  ✅   │ 4 1 9 │  ✅   │ . . 5 │  ✅
│ . . . │       │ . 8 . │       │ . 7 9 │
└───────┘       └───────┘       └───────┘
```

## Solution Approaches

### Approach 1: Brute Force

#### Pseudocode:
```
function isValidSudoku(board):
    // Check rows
    for each row in board:
        check for duplicates in non-empty cells
        if duplicates found, return false
    
    // Check columns
    for each column in board:
        check for duplicates in non-empty cells
        if duplicates found, return false
    
    // Check 3x3 sub-boxes
    for each 3x3 sub-box:
        check for duplicates in non-empty cells
        if duplicates found, return false
    
    return true
```

#### Python Implementation:
```python
def isValidSudoku_brute_force(board):
    """
    Brute force solution for checking if a Sudoku board is valid.
    
    Args:
        board: 9x9 list of lists representing the Sudoku board
        
    Returns:
        bool: True if the board is valid, False otherwise
        
    Time Complexity: O(3 * 9²) = O(9²) = O(1) since board size is fixed
    Space Complexity: O(9) = O(1) for the temporary sets
    """
    # Check rows
    for row in range(9):
        seen = set()
        for col in range(9):
            cell_value = board[row][col]
            if cell_value != '.':
                if cell_value in seen:
                    return False  # Duplicate found in row
                seen.add(cell_value)
    
    # Check columns
    for col in range(9):
        seen = set()
        for row in range(9):
            cell_value = board[row][col]
            if cell_value != '.':
                if cell_value in seen:
                    return False  # Duplicate found in column
                seen.add(cell_value)
    
    # Check 3x3 sub-boxes
    for box_row in range(0, 9, 3):
        for box_col in range(0, 9, 3):
            seen = set()
            for row in range(box_row, box_row + 3):
                for col in range(box_col, box_col + 3):
                    cell_value = board[row][col]
                    if cell_value != '.':
                        if cell_value in seen:
                            return False  # Duplicate found in box
                        seen.add(cell_value)
    
    return True  # Board is valid
```

### Approach 2: Hash Set (Three Passes)

#### Pseudocode:
```
function isValidSudoku(board):
    create empty hashset for each row, column, and sub-box
    
    // First pass: Check rows
    for each row:
        create hashset
        for each cell in row:
            if cell is not empty and already in hashset, return false
            add cell to hashset
    
    // Second pass: Check columns
    for each column:
        create hashset
        for each cell in column:
            if cell is not empty and already in hashset, return false
            add cell to hashset
    
    // Third pass: Check 3x3 sub-boxes
    for each sub-box:
        create hashset
        for each cell in sub-box:
            if cell is not empty and already in hashset, return false
            add cell to hashset
    
    return true
```

#### Python Implementation:
```python
def isValidSudoku_hash_set_three_passes(board):
    """
    Hash set solution using three separate passes for checking if Sudoku is valid.
    
    Args:
        board: 9x9 list of lists representing the Sudoku board
        
    Returns:
        bool: True if the board is valid, False otherwise
        
    Time Complexity: O(3 * 9²) = O(9²) = O(1) since board size is fixed
    Space Complexity: O(3 * 9) = O(1) for the hash sets
    """
    # First pass: Check rows
    for row in range(9):
        row_set = set()
        for col in range(9):
            cell_value = board[row][col]
            if cell_value != '.':
                if cell_value in row_set:
                    return False  # Duplicate found in row
                row_set.add(cell_value)
    
    # Second pass: Check columns
    for col in range(9):
        col_set = set()
        for row in range(9):
            cell_value = board[row][col]
            if cell_value != '.':
                if cell_value in col_set:
                    return False  # Duplicate found in column
                col_set.add(cell_value)
    
    # Third pass: Check 3x3 sub-boxes
    for box_row in range(0, 9, 3):
        for box_col in range(0, 9, 3):
            box_set = set()
            for row in range(box_row, box_row + 3):
                for col in range(box_col, box_col + 3):
                    cell_value = board[row][col]
                    if cell_value != '.':
                        if cell_value in box_set:
                            return False  # Duplicate found in box
                        box_set.add(cell_value)
    
    return True  # Board is valid
```

### Approach 3: Hash Set (One Pass)

#### Pseudocode:
```
function isValidSudoku(board):
    initialize empty sets for rows, columns, and boxes
    
    for row from 0 to 8:
        for col from 0 to 8:
            if cell is not empty:
                value = board[row][col]
                box_index = (row // 3) * 3 + (col // 3)
                
                // Check if value exists in current row, column, or box
                if value in rows[row] or value in cols[col] or value in boxes[box_index]:
                    return false
                    
                // Add value to respective sets
                add value to rows[row]
                add value to cols[col]
                add value to boxes[box_index]
    
    return true
```

#### Python Implementation:
```python
def isValidSudoku_hash_set_one_pass(board):
    """
    Hash set solution using one pass for checking if a Sudoku board is valid.
    Isme hum ek hi baar mei rows, columns aur boxes teeno ko check karte hain.
    
    Args:
        board: 9x9 list of lists representing the Sudoku board
        
    Returns:
        bool: True if the board is valid, False otherwise
        
    Time Complexity: O(9²) = O(1) since board size is fixed
    Space Complexity: O(3 * 9²) = O(1) for the hash sets
    """
    # Initialize sets to track digits in each row, column, and 3x3 box
    rows = [set() for _ in range(9)]
    cols = [set() for _ in range(9)]
    boxes = [set() for _ in range(9)]
    
    # One pass through the board
    for row in range(9):
        for col in range(9):
            cell_value = board[row][col]
            
            if cell_value == '.':
                continue  # Skip empty cells
            
            # Calculate which 3x3 box we're in (0-8)
            box_idx = (row // 3) * 3 + (col // 3)
            
            # Check if value exists in current row, column, or box
            if (cell_value in rows[row] or 
                cell_value in cols[col] or 
                cell_value in boxes[box_idx]):
                return False  # Duplicate found
            
            # Add value to respective sets
            rows[row].add(cell_value)
            cols[col].add(cell_value)
            boxes[box_idx].add(cell_value)
    
    return True  # Board is valid
```

## Example Board and Visualization

```
Sample Input:
[
  ["5","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
```

### Visualization of One-Pass Solution:

```
📝 Starting board check...

🔄 Processing cell (0,0): "5"
   ➡️ Add to row 0: {5}
   ➡️ Add to col 0: {5}
   ➡️ Add to box 0: {5}

🔄 Processing cell (0,1): "3"
   ➡️ Add to row 0: {5,3}
   ➡️ Add to col 1: {3}
   ➡️ Add to box 0: {5,3}

... continuing for all cells ...

🔄 Processing cell (8,8): "9"
   ➡️ Add to row 8: {8,7,9}
   ➡️ Add to col 8: {3,1,6,5,9}
   ➡️ Add to box 8: {9,7}

✅ Board is valid! No duplicates found in any row, column, or 3x3 box.
```

## Performance Comparison

| Approach                | Time Complexity | Space Complexity | Pros                    | Cons                            |
|-------------------------|----------------|-----------------|-------------------------|--------------------------------|
| Brute Force             | O(1)           | O(1)            | Simple to understand    | Three separate passes          |
| Hash Set (Three Passes) | O(1)           | O(1)            | Cleaner implementation  | Still requires three passes    |
| Hash Set (One Pass)     | O(1)           | O(1)            | Most efficient, one pass| Slightly more complex logic    |

*Note: Time and space complexities are O(1) because the Sudoku board size is fixed at 9x9.*

## Conclusion

Hash Set (One Pass) solution sabse efficient approach hai kyunki:

1. Sirf ek baar board par iterate karte hain
2. Har cell ko sirf ek baar check karte hain
3. Row, column aur box validity ko simultaneous check karte hain

Is problem ke liye constant time aur space complexity hi possible hai kyunki
Sudoku board ka size fixed hai (9x9), lekin one-pass approach better hai kyunki
kam iterations mein solution mil jata hai.

