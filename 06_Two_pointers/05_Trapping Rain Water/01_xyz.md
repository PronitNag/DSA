# Trapping Rain Water Problem Solutions

## Problem Statement

Aapko ek array of non-negative integers `height` diya gaya hai jo elevation map represent karta hai. Har value `height[i]` ek bar ki height ko represent karti hai, jiska width 1 hai.

Return the maximum area of water that can be trapped between the bars.

## Problem Visualization Using ASCII Art

### Initial Elevation Map:
```
                  █   
      █           █   
      █ █         █   
      █ █ █   █   █   
    █ █ █ █   █   █ █ 
  0 1 2 3 4 5 6 7 8 9    <- Indices
```

### Water Trapped (Shaded with `.`):
```
                  █   
      █.....      █   
      █ █...      █   
      █ █ █...█...█   
    █ █ █ █...█...█ █ 
  0 1 2 3 4 5 6 7 8 9    <- Indices
```

### Explanation:
- Blue dots (`.`) represent trapped water
- Total trapped water = 12 units

## Solution Approaches

### 1. Brute Force Approach

#### Visualization:

For each position `i`, we find:
```
               Max height to the left of i
               |
               v
      █ . . . . █   
      █ █ . . . █   
      █ █ █ . . █          Max height to the right of i
    █ █ █ █ . . █ █        ^
  0 1 2 3 4 5 6 7 8 9      |
               ^
               |
           Current position i = 5
```

Water trapped at position `i` = min(leftMax, rightMax) - height[i]

#### Pseudo Code:
```
function trap(height):
    totalWater = 0
    n = length(height)
    
    for i from 1 to n-2:
        leftMax = maximum height to the left of i (including i)
        rightMax = maximum height to the right of i (including i)
        
        waterLevel = min(leftMax, rightMax)
        
        if waterLevel > height[i]:
            totalWater += waterLevel - height[i]
    
    return totalWater
```

#### Python Code:
```python
def trap_brute_force(height):
    """
    Brute force solution for the trapping rain water problem.
    Har position i par ham left aur right ka maximum height find karte hain,
    phir water level ko calculate karte hain. Time complexity O(n²).
    
    Args:
        height: List of integers representing heights of bars
        
    Returns:
        Total amount of water trapped between bars
    """
    if not height or len(height) < 3:  # At least 3 bars needed to trap water
        return 0
        
    total_water = 0
    n = len(height)
    
    # Skip first and last bars as they can't trap water
    for i in range(1, n-1):
        # Find maximum height to the left
        left_max = max(height[:i+1])
        
        # Find maximum height to the right
        right_max = max(height[i:])
        
        # Calculate water level at current position
        water_level = min(left_max, right_max)
        
        # Add trapped water if water level is higher than current bar
        if water_level > height[i]:
            total_water += water_level - height[i]
    
    return total_water
```

### 2. Prefix & Suffix Arrays Approach

#### Visualization:

Precompute max heights to avoid repeated calculations:
```
Left max array:  [0, 1, 2, 2, 2, 2, 2, 2, 2, 2]
                  0  1  2  3  4  5  6  7  8  9

Right max array: [3, 3, 3, 3, 3, 3, 3, 3, 3, 1]
                  0  1  2  3  4  5  6  7  8  9

Height array:    [0, 1, 2, 1, 0, 1, 3, 2, 1, 0]
                  0  1  2  3  4  5  6  7  8  9
```

For each position `i`, water = min(left_max[i], right_max[i]) - height[i]

#### Pseudo Code:
```
function trap(height):
    n = length(height)
    leftMax = array of size n
    rightMax = array of size n
    totalWater = 0
    
    # Compute left max array
    leftMax[0] = height[0]
    for i from 1 to n-1:
        leftMax[i] = max(leftMax[i-1], height[i])
    
    # Compute right max array
    rightMax[n-1] = height[n-1]
    for i from n-2 down to 0:
        rightMax[i] = max(rightMax[i+1], height[i])
    
    # Compute trapped water
    for i from 0 to n-1:
        totalWater += min(leftMax[i], rightMax[i]) - height[i]
    
    return totalWater
```

#### Python Code:
```python
def trap_prefix_suffix(height):
    """
    Prefix & Suffix arrays solution for the trapping rain water problem.
    Ham do arrays precompute karte hain: ek left se maximum height ke liye,
    aur ek right se maximum height ke liye. Time complexity O(n).
    
    Args:
        height: List of integers representing heights of bars
        
    Returns:
        Total amount of water trapped between bars
    """
    if not height or len(height) < 3:
        return 0
        
    n = len(height)
    left_max = [0] * n
    right_max = [0] * n
    total_water = 0
    
    # Precompute maximum heights from left to right
    left_max[0] = height[0]
    for i in range(1, n):
        left_max[i] = max(left_max[i-1], height[i])
    
    # Precompute maximum heights from right to left
    right_max[n-1] = height[n-1]
    for i in range(n-2, -1, -1):
        right_max[i] = max(right_max[i+1], height[i])
    
    # Calculate trapped water at each position
    for i in range(n):
        water_at_i = min(left_max[i], right_max[i]) - height[i]
        total_water += max(0, water_at_i)  # In case water is negative
    
    return total_water
```

### 3. Stack-Based Approach

#### Visualization:

Using a stack to track potential water traps:
```
Step 1: Processing index 0           Step 2: Processing index 1
Stack: []                            Stack: [0]
                                                █
                                              █ |
                                          0 1 2 3 4 5 6 7 8 9

Step 3: Processing index 2           Step 4: Processing index 3
Stack: [0, 1]                        Stack: [0, 2]
        █                                    █
      █ |                                  █ █ 
  0 1 2 3 4 5 6 7 8 9                  0 1 2 3 4 5 6 7 8 9

Step 5: Calculating water at index 3:
Water trapped between bars at index 2 and 3 = width * height
                                    = (3-2-1) * min(height[2], height[3]) - height[3]
```

#### Pseudo Code:
```
function trap(height):
    stack = empty stack
    totalWater = 0
    
    for i from 0 to n-1:
        while stack is not empty AND height[i] > height[stack.top()]:
            top = stack.pop()
            
            if stack is empty:
                break
                
            distance = i - stack.top() - 1
            boundedHeight = min(height[i], height[stack.top()]) - height[top]
            totalWater += distance * boundedHeight
        
        stack.push(i)
    
    return totalWater
```

#### Python Code:
```python
def trap_stack(height):
    """
    Stack-based solution for the trapping rain water problem.
    Ham ek stack use karke potential "water traps" ko track karte hain.
    Jab bhi current height stack ke top element se bada hota hai,
    tab ham water calculate karte hain. Time complexity O(n).
    
    Args:
        height: List of integers representing heights of bars
        
    Returns:
        Total amount of water trapped between bars
    """
    if not height or len(height) < 3:
        return 0
        
    stack = []  # Stack will store indices of bars
    total_water = 0
    
    for i in range(len(height)):
        # While current bar can form a water trap with bars in stack
        while stack and height[i] > height[stack[-1]]:
            # Pop the top bar that will form the bottom of the trap
            top = stack.pop()
            
            # If stack is empty, no left boundary for water trap
            if not stack:
                break
                
            # Calculate width of water trap
            distance = i - stack[-1] - 1
            
            # Calculate bounded height (height of water above bar at 'top')
            bounded_height = min(height[i], height[stack[-1]]) - height[top]
            
            # Add water volume for this section
            total_water += distance * bounded_height
        
        # Push current index to stack
        stack.append(i)
    
    return total_water
```

### 4. Two Pointers Approach

#### Visualization:

Using two pointers to track water from both ends:
```
Initial state:
left = 0, right = 9, left_max = 0, right_max = 0

As we process:
           left_max                           right_max
              |                                   |
              v                                   v
              █ . . . █                           █ 
              █ █ . . █                           █ 
              █ █ █ . █                           █ 
            █ █ █ █ . █ █                         █ 
          0 1 2 3 4 5 6 7 8 9                   0 1 2 3 4 5 6 7 8 9
          ^                 ^                   ^                 ^
          |                 |                   |                 |
        left              right               left              right
```

We only need to know if there's a higher bar somewhere to the right or left.

#### Pseudo Code:
```
function trap(height):
    left = 0
    right = length(height) - 1
    leftMax = 0
    rightMax = 0
    totalWater = 0
    
    while left < right:
        if height[left] < height[right]:
            if height[left] >= leftMax:
                leftMax = height[left]
            else:
                totalWater += leftMax - height[left]
            left++
        else:
            if height[right] >= rightMax:
                rightMax = height[right]
            else:
                totalWater += rightMax - height[right]
            right--
    
    return totalWater
```

#### Python Code:
```python
def trap_two_pointers(height):
    """
    Two pointers solution for the trapping rain water problem.
    Ham left aur right se simultaneously process karte hain, jisse ek hi
    pass mein solution milta hai. Time complexity O(n).
    
    Args:
        height: List of integers representing heights of bars
        
    Returns:
        Total amount of water trapped between bars
    """
    if not height or len(height) < 3:
        return 0
        
    left = 0
    right = len(height) - 1
    left_max = 0
    right_max = 0
    total_water = 0
    
    while left < right:
        # Process from whichever side has the smaller bar
        if height[left] < height[right]:
            # If current bar is higher than or equal to left_max
            if height[left] >= left_max:
                # Update left_max
                left_max = height[left]
            else:
                # Current bar is lower, so it will trap water
                total_water += left_max - height[left]
            left += 1
        else:
            # If current bar is higher than or equal to right_max
            if height[right] >= right_max:
                # Update right_max
                right_max = height[right]
            else:
                # Current bar is lower, so it will trap water
                total_water += right_max - height[right]
            right -= 1
    
    return total_water
```

## Comparison of Approaches

| Approach             | Time Complexity | Space Complexity | Comments                                      |
|----------------------|-----------------|------------------|-----------------------------------------------|
| Brute Force          | O(n²)           | O(1)             | Simple but inefficient for large inputs       |
| Prefix & Suffix      | O(n)            | O(n)             | Uses extra space for precomputation           |
| Stack                | O(n)            | O(n)             | Good for visualizing the solution step by step|
| Two Pointers         | O(n)            | O(1)             | Optimal in both time and space                |

## Conclusion

Is problem mein, har approach apne tarah se efficient hai:

1. Brute Force approach simple hai lekin O(n²) time complexity ke karan bade inputs ke liye slow hai.

2. Prefix & Suffix approach O(n) time mein solution provide karta hai lekin extra space use karta hai.

3. Stack-based approach bhi O(n) time mein solution deta hai aur visually samajhne mein helpful hai.

4. Two Pointers approach sabse optimal hai, kyunki yeh O(n) time aur O(1) space mein solution provide karta hai.

Two Pointers approach is particularly elegant as it handles the problem in a single pass through the array and doesn't require any additional data structures beyond a few variables.
