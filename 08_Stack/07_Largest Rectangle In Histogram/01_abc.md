# Largest Rectangle in Histogram Solutions

## Problem Statement
You are given an array of integers `heights` where `heights[i]` represents the height of a bar. The width of each bar is 1. Return the area of the largest rectangle that can be formed among the bars.

## Problem Visualization

Let's say we have the following histogram:
```
            █   
            █   
        █   █   
        █   █   █
    █   █   █   █
█   █   █   █   █
2   1   5   6   2
0   1   2   3   4  (indices)
```

The largest area would be 3 × 2 = 6, formed by the rectangle spanning indices 2-4 with height 2.

## Approach 1: Brute Force

### Visualization:
For each position, we consider all possible widths and find the maximum area.

```
Step 1: Position 0, height = 2
        
        For width = 1: Area = 2 × 1 = 2
        Largest area so far = 2

Step 2: Position 1, height = 1
        
        For width = 1: Area = 1 × 1 = 1
        For width = 2: min(1,2) × 2 = 1 × 2 = 2
        Largest area so far = 2

Step 3: Position 2, height = 5
        
        For width = 1: Area = 5 × 1 = 5
        For width = 2: min(5,1) × 2 = 1 × 2 = 2
        For width = 3: min(5,1,2) × 3 = 1 × 3 = 3
        Largest area so far = 5

... and so on
```

### Python Code:
```python
def largestRectangleArea_bruteForce(heights):
    """
    Bruteforce approach to find the largest rectangle area in a histogram.
    Time Complexity: O(n^2) where n is the number of bars
    Space Complexity: O(1)
    
    :param heights: List of integers representing heights of bars
    :return: Largest rectangle area
    """
    n = len(heights)
    max_area = 0
    
    # Iterate through all bars
    for i in range(n):
        # Find minimum height in the range [i, j]
        min_height = float('inf')
        
        # Consider all possible widths starting from position i
        for j in range(i, n):
            min_height = min(min_height, heights[j])
            current_area = min_height * (j - i + 1)
            max_area = max(max_area, current_area)
    
    return max_area
```

## Approach 2: Divide and Conquer

### Visualization:
We divide the histogram into two halves and recursively find the largest rectangle in both halves. We also find the largest rectangle that crosses the middle.

```
            █   
            █   
        █   █   
        █   █   █
    █   █   █   █
█   █   █   █   █
2   1   5   6   2
0   1   2   3   4

Divide into [2,1] and [5,6,2]

Left half max area = 2 (height 2, width 1)
Right half max area = 6 (height 2, width 3)

Cross-middle rectangle:
min_height = min(1,5) = 1
area = 1 × 2 = 2

... continue recursively
```

### Python Code:
```python
def largestRectangleArea_divideConquer(heights):
    """
    Divide and conquer approach to find the largest rectangle area in histogram.
    Time Complexity: O(n log n) in average case, O(n^2) in worst case
    Space Complexity: O(log n) for recursion stack
    
    :param heights: List of integers representing heights of bars
    :return: Largest rectangle area
    """
    return divideAndConquer(heights, 0, len(heights) - 1)

def divideAndConquer(heights, start, end):
    # Base case: single bar
    if start > end:
        return 0
    if start == end:
        return heights[start]
    
    # Find the bar with minimum height
    min_index = start
    for i in range(start, end + 1):
        if heights[i] < heights[min_index]:
            min_index = i
    
    # Calculate area with minimum height bar as the limiting factor
    area_with_min = heights[min_index] * (end - start + 1)
    
    # Calculate area of left half
    area_left = divideAndConquer(heights, start, min_index - 1)
    
    # Calculate area of right half
    area_right = divideAndConquer(heights, min_index + 1, end)
    
    # Return maximum of three areas
    return max(area_with_min, area_left, area_right)
```

## Approach 3: Stack

### Visualization:
We use a stack to keep track of bars in ascending order of their heights. When we find a bar shorter than the one on top of the stack, we calculate the area.

```
Process histogram: [2, 1, 5, 6, 2]

Step 1: Push 0 (index of height 2), stack = [0]
Step 2: Current height 1 < heights[stack.top()] (2)
        Pop 0, calculate area = 2 × 1 = 2
        Push 1 (index of height 1), stack = [1]
Step 3: Current height 5 > heights[stack.top()] (1)
        Push 2 (index of height 5), stack = [1, 2]
Step 4: Current height 6 > heights[stack.top()] (5)
        Push 3 (index of height 6), stack = [1, 2, 3]
Step 5: Current height 2 < heights[stack.top()] (6)
        Pop 3, calculate area = 6 × 1 = 6
        Current height 2 < heights[stack.top()] (5)
        Pop 2, calculate area = 5 × 2 = 10
        Push 4 (index of height 2), stack = [1, 4]
Step 6: End of array, pop remaining elements
        Pop 4, calculate area = 2 × 4 = 8
        Pop 1, calculate area = 1 × 5 = 5
        
Maximum area = 10
```

### Python Code:
```python
def largestRectangleArea_stack(heights):
    """
    Stack-based approach to find the largest rectangle area in histogram.
    Time Complexity: O(n) where n is the number of bars
    Space Complexity: O(n) for the stack
    
    :param heights: List of integers representing heights of bars
    :return: Largest rectangle area
    """
    stack = []  # Store indices of bars
    max_area = 0
    i = 0
    n = len(heights)
    
    while i < n:
        # If stack is empty or height is increasing, push to stack
        if not stack or heights[stack[-1]] <= heights[i]:
            stack.append(i)
            i += 1
        # If height is decreasing, calculate area with stack top as smallest bar
        else:
            top = stack.pop()
            
            # Calculate area with the top of the stack as smallest bar
            area = heights[top] * (i - stack[-1] - 1 if stack else i)
            max_area = max(max_area, area)
    
    # Process remaining bars in the stack
    while stack:
        top = stack.pop()
        area = heights[top] * (n - stack[-1] - 1 if stack else n)
        max_area = max(max_area, area)
    
    return max_area
```

## Approach 4: Stack (One Pass)

### Visualization:
A more concise version of the stack-based approach, processing elements in a single traversal.

```
Process histogram: [2, 1, 5, 6, 2]

Stack: [] (we'll use -1 as a sentinel value), max_area = 0
i=0: Push 0, stack = [-1, 0]
i=1: heights[1] = 1 < heights[0] = 2, so pop 0
     Calculate area = 2 × (1 - (-1) - 1) = 2 × 1 = 2
     max_area = 2, push 1, stack = [-1, 1]
i=2: Push 2, stack = [-1, 1, 2]
i=3: Push 3, stack = [-1, 1, 2, 3]
i=4: heights[4] = 2 < heights[3] = 6, so pop 3
     Calculate area = 6 × (4 - 2 - 1) = 6 × 1 = 6
     max_area = 6
     
     heights[4] = 2 < heights[2] = 5, so pop 2
     Calculate area = 5 × (4 - 1 - 1) = 5 × 2 = 10
     max_area = 10, push 4, stack = [-1, 1, 4]

After loop, pop remaining elements:
Pop 4: area = 2 × (5 - 1 - 1) = 2 × 3 = 6
Pop 1: area = 1 × (5 - (-1) - 1) = 1 × 5 = 5

Maximum area = 10
```

### Python Code:
```python
def largestRectangleArea_stackOnePass(heights):
    """
    Optimized one-pass stack approach to find largest rectangle area.
    Time Complexity: O(n) where n is the number of bars
    Space Complexity: O(n) for the stack
    
    :param heights: List of integers representing heights of bars
    :return: Largest rectangle area
    """
    n = len(heights)
    stack = [-1]  # Use -1 as a sentinel value to simplify calculation
    max_area = 0
    
    for i in range(n):
        # While stack is not empty and current height is less than height at stack top
        while stack[-1] != -1 and heights[stack[-1]] > heights[i]:
            h = heights[stack.pop()]
            w = i - stack[-1] - 1
            max_area = max(max_area, h * w)
        stack.append(i)
    
    # Process remaining bars in the stack
    while stack[-1] != -1:
        h = heights[stack.pop()]
        w = n - stack[-1] - 1
        max_area = max(max_area, h * w)
    
    return max_area
```

## Approach 5: Stack (Optimal)

### Visualization:
This is a more optimized version of the stack approach. We add 0 to the beginning and end of the heights array to handle edge cases elegantly.

```
Append 0 at beginning and end: [0, 2, 1, 5, 6, 2, 0]

Stack: [], max_area = 0
i=0: Push 0, stack = [0]
i=1: Push 1, stack = [0, 1]
i=2: heights[2] = 1 < heights[1] = 2, so pop 1
     area = 2 × (2 - 0 - 1) = 2 × 1 = 2
     max_area = 2, push 2, stack = [0, 2]
i=3: Push 3, stack = [0, 2, 3]
i=4: Push 4, stack = [0, 2, 3, 4]
i=5: heights[5] = 2 < heights[4] = 6, so pop 4
     area = 6 × (5 - 3 - 1) = 6 × 1 = 6
     max_area = 6
     
     heights[5] = 2 < heights[3] = 5, so pop 3
     area = 5 × (5 - 2 - 1) = 5 × 2 = 10
     max_area = 10, push 5, stack = [0, 2, 5]
i=6: heights[6] = 0 < heights[5] = 2, so pop 5
     area = 2 × (6 - 2 - 1) = 2 × 3 = 6
     
     heights[6] = 0 < heights[2] = 1, so pop 2
     area = 1 × (6 - 0 - 1) = 1 × 5 = 5
     
Maximum area = 10
```

### Python Code:
```python
def largestRectangleArea_stackOptimal(heights):
    """
    Most optimal stack approach to find largest rectangle area in histogram.
    Time Complexity: O(n) where n is the number of bars
    Space Complexity: O(n) for the stack
    
    :param heights: List of integers representing heights of bars
    :return: Largest rectangle area
    """
    # Add 0 at the beginning and end to simplify the algorithm
    heights = [0] + heights + [0]
    n = len(heights)
    stack = [0]  # Stack contains indices
    max_area = 0
    
    for i in range(1, n):
        while heights[stack[-1]] > heights[i]:
            h = heights[stack.pop()]
            w = i - stack[-1] - 1
            max_area = max(max_area, h * w)
        stack.append(i)
    
    return max_area
```

## Comparison of Approaches

| Approach | Time Complexity | Space Complexity | Description |
|----------|----------------|------------------|-------------|
| Brute Force | O(n²) | O(1) | For each position, try all possible widths. Simple but inefficient. |
| Divide and Conquer | O(n log n) average, O(n²) worst | O(log n) | Recursively divide the problem. Elegant but can degenerate to O(n²). |
| Basic Stack | O(n) | O(n) | Use stack to track heights in increasing order. |
| One Pass Stack | O(n) | O(n) | More concise stack implementation with single traversal. |
| Optimal Stack | O(n) | O(n) | Most efficient approach, handling edge cases elegantly. |

## Hinglish Explanation

Ye problem histogram mein largest rectangle dhoondhne ke baare mein hai. Histogram ek chart hai jisme vertical bars hote hain, aur humein unme se sabse bada rectangle nikalna hai.

Humne 5 tarike se solve kiya hai:

1. Brute Force: Har position se shuru karke sare possible widths check karte hain. O(n²) time complexity hai.

2. Divide and Conquer: Histogram ko do hisson mein divide karke recursively solve karte hain. Average case mein O(n log n) time complexity hai.

3. Stack: Stack ka use karke heights ko ascending order mein track karte hain. Jab koi chota height milta hai, tab rectangle area calculate karte hain. O(n) time complexity hai.

4. Stack (One Pass): Stack approach ka optimized version hai, jisme ek hi traversal mein problem solve ho jata hai.

5. Stack (Optimal): Stack approach ka sabse efficient version hai, jisme edge cases ko handle karne ke liye array ke start aur end mein 0 add kiya jata hai.

In approach mein se, Stack (Optimal) approach sabse efficient hai kyunki iska time complexity O(n) hai aur ye edge cases ko bhi achhe se handle karta hai.

