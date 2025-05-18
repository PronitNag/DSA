# Container With Most Water - Solution

## Problem Statement

**Medium**

You are given an integer array `heights` where `heights[i]` represents the height of the ith bar.
You may choose any two bars to form a container. Return the **maximum** amount of water a container can store.

## Understanding the Problem

To visualize this problem, imagine each height as a vertical bar and we need to find two bars that can hold the maximum amount of water between them.

```
                    |                                
                    |                                
                    |        |                       
                    |        |                       
        |           |        |                       
        |           |        |           |           
        |           |        |           |           
        |   |       |        |           |           
        |   |       |        |   |       |           
        |   |       |        |   |       |           
      --|---|-------|--------|---|-------|------
        1   2       3        4   5       6       
```

The water is contained between two bars, and the amount of water is calculated as:
- **Width** = Distance between the two bars (indices)
- **Height** = Minimum height of the two chosen bars
- **Area (Water)** = Width × Height

## Solution Approaches

### 1. Brute Force Approach

#### Visualization

```
Step 1: Initialize maxWater = 0
Step 2: Consider every possible pair of bars
```

```
For example with heights [1,8,6,2,5,4,8,3,7]:

First iteration:
                    |                 |          
                    |                 |          
                    |        |        |        | 
                    |        |        |        | 
        |           |        |        |        | 
        |           |        |        |        | 
        |           |        |        |        | 
        |   |       |        |   |    |        | 
        |   |       |        |   |    |        | 
        |   |       |        |   |    |        | 
      --|---|-------|--------|---|----|-|------|-
        1   8       6        2   5    4 8      3 7
        ^                                       ^
        i=0                                     j=8
        
        width = 8-0 = 8
        height = min(1,7) = 1
        area = 8*1 = 8
        maxWater = 8
```

```
Another iteration (with significant area):
                    |                 |          
                    |                 |          
                    |        |        |        | 
                    |        |        |        | 
        |           |        |        |        | 
        |           |        |        |        | 
        |           |        |        |        | 
        |   |       |        |   |    |        | 
        |   |       |        |   |    |        | 
        |   |       |        |   |    |        | 
      --|---|-------|--------|---|----|-|------|-
        1   8       6        2   5    4 8      3 7
            ^                                 ^
            i=1                               j=8
            
        width = 8-1 = 7
        height = min(8,7) = 7
        area = 7*7 = 49
        maxWater = 49
```

#### Pseudo Code

```
function maxArea(heights):
    maxWater = 0
    n = length of heights
    
    for i = 0 to n-2:
        for j = i+1 to n-1:
            width = j - i
            height = min(heights[i], heights[j])
            currentWater = width * height
            maxWater = max(maxWater, currentWater)
            
    return maxWater
```

#### Python Implementation

```python
def maxArea_brute_force(heights):
    """
    Brute Force solution for Container With Most Water problem.
    Time Complexity: O(n²) where n is the length of heights array
    Space Complexity: O(1) as we only use constant extra space
    
    Args:
        heights: List of integers representing heights of bars
        
    Returns:
        Maximum water that can be stored between any two bars
    """
    n = len(heights)
    max_water = 0
    
    # Consider every possible pair of bars
    for i in range(n):
        for j in range(i+1, n):
            # Calculate width between bars
            width = j - i
            # Height is limited by the shorter bar
            height = min(heights[i], heights[j])
            # Calculate area (water) and update maximum if needed
            current_water = width * height
            max_water = max(max_water, current_water)
    
    return max_water
```

### 2. Two Pointers Approach

#### Visualization

```
Using two pointers at both ends and moving inward:

Initial state with heights [1,8,6,2,5,4,8,3,7]:
                    |                 |          
                    |                 |          
                    |        |        |        | 
                    |        |        |        | 
        |           |        |        |        | 
        |           |        |        |        | 
        |           |        |        |        | 
        |   |       |        |   |    |        | 
        |   |       |        |   |    |        | 
        |   |       |        |   |    |        | 
      --|---|-------|--------|---|----|-|------|-
        1   8       6        2   5    4 8      3 7
        ^                                       ^
       left                                   right
        
        width = 8-0 = 8
        height = min(1,7) = 1
        area = 8*1 = 8
        maxWater = 8
```

```
Next step (move left pointer since heights[left] < heights[right]):
                    |                 |          
                    |                 |          
                    |        |        |        | 
                    |        |        |        | 
        |           |        |        |        | 
        |           |        |        |        | 
        |           |        |        |        | 
        |   |       |        |   |    |        | 
        |   |       |        |   |    |        | 
        |   |       |        |   |    |        | 
      --|---|-------|--------|---|----|-|------|-
        1   8       6        2   5    4 8      3 7
            ^                                   ^
           left                               right
           
        width = 8-1 = 7
        height = min(8,7) = 7
        area = 7*7 = 49
        maxWater = 49
```

```
Next step (move right pointer since heights[left] > heights[right]):
                    |                 |          
                    |                 |          
                    |        |        |        | 
                    |        |        |        | 
        |           |        |        |        | 
        |           |        |        |        | 
        |           |        |        |        | 
        |   |       |        |   |    |        | 
        |   |       |        |   |    |        | 
        |   |       |        |   |    |        | 
      --|---|-------|--------|---|----|-|------|-
        1   8       6        2   5    4 8      3 
            ^                               ^
           left                           right
           
        width = 7-1 = 6
        height = min(8,3) = 3
        area = 6*3 = 18
        maxWater = 49 (no change)
```

```
After more iterations, we find that the maximum area is 49 (between heights[1]=8 and heights[8]=7)
```

#### Pseudo Code

```
function maxArea(heights):
    maxWater = 0
    left = 0
    right = length of heights - 1
    
    while left < right:
        width = right - left
        height = min(heights[left], heights[right])
        maxWater = max(maxWater, width * height)
        
        if heights[left] < heights[right]:
            left = left + 1
        else:
            right = right - 1
            
    return maxWater
```

#### Python Implementation

```python
def maxArea_two_pointers(heights):
    """
    Two Pointers solution for Container With Most Water problem.
    Time Complexity: O(n) where n is the length of heights array
    Space Complexity: O(1) as we only use constant extra space
    
    Args:
        heights: List of integers representing heights of bars
    
    Returns:
        Maximum water that can be stored between any two bars
    """
    max_water = 0
    left = 0
    right = len(heights) - 1
    
    while left < right:
        # Calculate width between bars
        width = right - left
        # Height is limited by the shorter bar
        height = min(heights[left], heights[right])
        # Calculate area (water) and update maximum if needed
        current_water = width * height
        max_water = max(max_water, current_water)
        
        # Move pointers strategically
        # We always move the pointer with smaller height because:
        # - Moving the taller bar can only decrease the height or keep it the same
        # - While decreasing the width
        # - So area can never increase by moving the taller bar
        if heights[left] < heights[right]:
            left += 1
        else:
            right -= 1
    
    return max_water
```

## Hindi-English Explanation (Hinglish)

### Brute Force Approach Explanation

Is problem mein humein do bars choose karne hain jisse maximum pani store ho sake.
Brute force approach mein hum har possible pair of bars ko consider karte hain
aur unke beech kitna pani store ho sakta hai, calculate karte hain.

Har pair ke liye:
- Width = bars ke beech ka distance (j - i)
- Height = dono bars mein se minimum height (min(heights[i], heights[j]))
- Area = Width × Height

Phir maximum area track karte hain. Iska time complexity O(n²) hota hai kyunki
hum do nested loops use karte hain aur har possible pair check karte hain.

### Two Pointers Approach Explanation

Two pointers approach mein hum do pointers ka use karte hain - ek left end pe
aur ek right end pe. Phir dono pointers ko center ki taraf move karte hain,
is strategy ke saath ki hum hamesha chote height wale pointer ko move karenge.

Kyon? Kyunki:
- Agar hum bade height wale pointer ko move karein, to width kam hoga
- Aur height ya to same rahega ya kam hoga (kyunki height = min of both bars)
- Isliye area kabhi increase nahi ho sakta bade height wale pointer ko move karne se

Is approach se hum O(n) time complexity mein problem solve kar sakte hain, jo
brute force se bahut better hai.

## Complexity Analysis

### Brute Force:
- **Time Complexity**: O(n²) where n is the number of bars
- **Space Complexity**: O(1) constant extra space

### Two Pointers:
- **Time Complexity**: O(n) where n is the number of bars
- **Space Complexity**: O(1) constant extra space

## Conclusion

The Two Pointers approach is much more efficient than the Brute Force approach for
this problem. While the Brute Force approach compares all possible pairs of bars,
the Two Pointers approach strategically moves pointers to find the optimal solution
in a single pass through the array.

