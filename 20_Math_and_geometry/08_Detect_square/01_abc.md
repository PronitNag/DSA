# Detect Squares Solution

## Problem Visualization with ASCII Art

Let's understand the problem visually:

```
                y
                ^
                |
                |
  p2 o---------o p3      When we have a query point 'q', we need to find
     |         |         all possible 'squares' that can be formed with 'q' 
     |         |         as one corner and 3 other points from our collection.
     |         |         
  q  o---------o p1      
     |         
     |         
  ---+-------> x
```

When we have a query point 'q', we want to find all combinations of 3 other points that form a square with 'q'. For example, points q, p1, p2, p3 form a square.

## Key Insight Animation

```
Step 1: Query point q = (x, y)        Step 2: Find diagonal point         Step 3: Find other corners
    y                                     y                                   y
    ^                                     ^                                   ^
    |                                     |                                   |
    |                                     |                                   |
    o   Other points                      o   Other points                    o   p2 o---------o p3
    |   in our collection                 |   in our collection               |      |         |
    |                                     |   o p3 (diagonal)                 |      |         |
    |                                     |                                   |      |         |
    o q (query point)                     o q                                 o q o---------o p1
    |                                     |                                   |
    |                                     |                                   |
    +-------> x                           +-------> x                         +-------> x
```

## Solution Approach

### Hash Map - I Solution

We'll use a HashMap to store counts of points by their coordinates:

1. For each query point (x, y), we need to find diagonal points (x+d, y+d), (x+d, y-d), etc.
2. Then we check if all corners exist to form a square.

```python
from collections import defaultdict
import math

class CountSquares:
    def __init__(self):
        # Stores count of points at each coordinate as {(x,y): count}
        self.points_count = defaultdict(int)
        # Also keep a list of all points for easy iteration
        self.points_list = []
        
    def add(self, point):
        # Add the point to our data structure
        x, y = point
        self.points_count[(x, y)] += 1
        self.points_list.append((x, y))
        
    def count(self, point):
        query_x, query_y = point
        result = 0
        
        # For each point in our collection
        for x, y in self.points_list:
            # Skip if the point is on the same vertical or horizontal line as query point
            # (can't form a square with such points)
            if x == query_x or y == query_y:
                continue
                
            # Calculate the number of valid squares
            # For a square, we need to check if the other two corners exist
            # Corner 1: (query_x, y)
            # Corner 2: (x, query_y)
            result += self.points_count[(query_x, y)] * self.points_count[(x, query_y)]
            
        return result
```

Wait, the above solution is incorrect! Let me fix it:

```python
from collections import defaultdict

class CountSquares:
    def __init__(self):
        # Stores count of points at each coordinate as {(x,y): count}
        self.points_count = defaultdict(int)
        
    def add(self, point):
        # Add the point to our data structure
        x, y = point
        self.points_count[(x, y)] += 1
        
    def count(self, point):
        query_x, query_y = point
        result = 0
        
        # For each point in our collection
        for (x, y), count in self.points_count.items():
            # Skip if the point is on the same vertical or horizontal line as query point
            # (can't form a valid square with such points)
            if x == query_x or y == query_y:
                continue
                
            # Calculate the side length of the potential square
            # Check if the other two corners exist
            corner1 = (query_x, y)
            corner2 = (x, query_y)
            
            # If both corners exist, we can form a square
            if corner1 in self.points_count and corner2 in self.points_count:
                # Multiply by the count of each corner to account for duplicates
                result += count * self.points_count[corner1] * self.points_count[corner2]
            
        return result
```

Wait, there's still an issue. Let me correct my thinking and adjust the solution:

### Hash Map - I (Corrected Solution)

```python
from collections import defaultdict

class CountSquares:
    def __init__(self):
        # Stores count of points at each coordinate
        self.points_count = defaultdict(int)
        
    def add(self, point):
        # Add the point to our data structure
        x, y = point
        self.points_count[(x, y)] += 1
        
    def count(self, point):
        query_x, query_y = point
        result = 0
        
        # Iterate through all points in our collection
        for (x, y), count in self.points_count.items():
            # Skip points that are on the same vertical or horizontal line as query point
            if x == query_x or y == query_y:
                continue
                
            # Calculate the diagonal distance
            dx = abs(x - query_x)
            dy = abs(y - query_y)
            
            # For a valid square, dx must equal dy (for all sides to be equal)
            if dx != dy:
                continue
            
            # Check if the other two corners exist
            corner1 = (query_x, y)  # Same x as query, same y as current point
            corner2 = (x, query_y)  # Same x as current point, same y as query
            
            # If both corners exist, we can form a square
            # Multiply counts to account for all possible combinations
            result += count * self.points_count[corner1] * self.points_count[corner2]
            
        return result
```

### Hash Map - II Solution

This approach organizes points by their x-coordinates for more efficient lookup:

```python
from collections import defaultdict

class CountSquares:
    def __init__(self):
        # Store points by x-coordinate: {x: [(y, count), ...]}
        self.points_by_x = defaultdict(list)
        # Also store points by coordinates for easy lookup
        self.point_counts = defaultdict(int)
        
    def add(self, point):
        x, y = point
        
        # Update count for this coordinate
        self.point_counts[(x, y)] += 1
        
        # If this is first occurrence of this (x,y), add to points_by_x
        if self.point_counts[(x, y)] == 1:
            self.points_by_x[x].append(y)
        
    def count(self, point):
        query_x, query_y = point
        result = 0
        
        # For each x-coordinate in our collection
        for x in self.points_by_x:
            # Skip if it's the same x-coordinate as query point
            if x == query_x:
                continue
                
            # Calculate the distance between x-coordinates
            d = x - query_x
            
            # For each y-coordinate at this x
            for y in self.points_by_x[x]:
                # Skip if it's the same y-coordinate as query point
                if y == query_y:
                    continue
                    
                # Check if the square can be formed
                # We need points at (query_x, y) and (x, query_y)
                corner1_count = self.point_counts.get((query_x, y), 0)
                corner2_count = self.point_counts.get((x, query_y), 0)
                
                if corner1_count > 0 and corner2_count > 0:
                    # Get count of current diagonal point
                    diagonal_count = self.point_counts[(x, y)]
                    
                    # Add to result
                    result += diagonal_count * corner1_count * corner2_count
        
        return result
```

## Hinglish Explanation

```
Yeh problem mein humein ek data structure banana hai jo points store kar sake 
aur phir "squares" ko count kare. Kya hai yeh "squares"? Yeh woh squares hain 
jinke sides x aur y axis ke parallel hain.

Hum kaise solve kar sakte hain?

1. Ek HashMap banayenge jo har point ka count store karega.
2. Query point (x,y) diya gaya hai, toh hume dekhna hai kitne squares ban 
   sakte hain.
3. Iske liye, hum har point (p_x, p_y) ke liye check karenge:
   - Diagonal points: (p_x, p_y) aur (x, y)
   - Baaki do corners: (x, p_y) aur (p_x, y)
4. Agar ye dono corners exist karte hain, toh hum unke counts ko multiply 
   karke result mein add kar denge.

Is approach se hum O(n) time complexity mein problem solve kar sakte hain,
jahan 'n' total points ka count hai.
```

## Time and Space Complexity

**Hash Map - I:**
- Time Complexity: 
  - add: O(1)
  - count: O(n), where n is the number of unique points
- Space Complexity: O(n)

**Hash Map - II:**
- Time Complexity:
  - add: O(1)
  - count: O(n), where n is the number of unique points
- Space Complexity: O(n)

## Visual Example

```
Example:
Points added: (0,0), (1,0), (0,1), (1,1), (2,0), (2,2)
Query point: (0,0)

        y
        ^
        |
    (0,2)   (1,2)   (2,2)
        |       
        |       
    (0,1)   (1,1)        
        |       
        |       
    (0,0)----(1,0)----(2,0)--> x
    
For query (0,0):
- Square 1: (0,0), (1,0), (0,1), (1,1) ✓
- Square 2: (0,0), (2,0), (0,2), (2,2) ✓

Answer: 2
```
