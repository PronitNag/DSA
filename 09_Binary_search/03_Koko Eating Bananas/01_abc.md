# Koko Eating Bananas - LeetCode 875 Solution

## Problem Statement

Koko loves to eat bananas. There are n piles of bananas, the ith pile has piles[i] bananas. The guards have gone and will come back in h hours.

Koko can decide her bananas-per-hour eating speed of k. Each hour, she chooses some pile of bananas and eats k bananas from that pile. If the pile has less than k bananas, she eats all of them instead and will not eat any more bananas during this hour.

Koko likes to eat slowly but still wants to finish eating all the bananas before the guards return.

Return the minimum integer k such that she can eat all the bananas within h hours.

## ASCII Art Visualization

### Starting Point - The Problem

```
                      Guards Gone for 'h' Hours
                              ↓
                 .-.
     .-.__      /   \      Guards will return after 'h' hours!
    (     |    /     \     Koko needs to find minimum k to eat all bananas
     \    |   /       \    k = eating speed (bananas per hour)
      '-.-'  /         \
            /           \
           /             \
          '               '

    Pile 1    Pile 2    Pile 3        ...      Pile n
   [🍌🍌🍌]  [🍌🍌🍌🍌]  [🍌🍌]                [🍌🍌🍌🍌🍌]
  piles[0]   piles[1]   piles[2]     ...     piles[n-1]

What is the minimum 'k' (eating speed) to finish all bananas in 'h' hours?
```

### Approach Evolution - Finding Minimum Eating Speed 'k'

```
Brute Force Approach:                        Binary Search Approach:
                                             
k=1:  Too Slow!                              Search Space: [1...max(piles)]
  │                                             │
k=2:  Too Slow!                              mid=(1+max)÷2
  │                                           /     \
k=3:  Too Slow!                             /        \
  │                                        /          \
  ↓                                       /            \
  .                                      /              \
  .                              Can eat all         Cannot eat all
  .                              in h hours?         in h hours?
k=i:  Just Right! ← Minimum!            /                \
                                       /                  \
                                   Reduce                Increase
                                upper bound            lower bound
                                      
                                      Solution: First k that works!
```

### Ending Point - Solution Found

```
  Final Minimum Speed k = ?
         .-'~~~~~`-.
       .'          '.
      /              \
     |  Koko can eat  |
     |   all bananas  |
     |   in h hours   |
      \     at k     /
       '.         .'
         '-.....-'
       
  Time:  O(n * max(piles))  →  O(n * log(max(piles)))
  Space: O(1)                  O(1)
```

## Solving Approaches

### 1. Brute Force Solution

In brute force approach, we try every possible speed starting from k=1 and check 
if Koko can finish all bananas within h hours at that speed.

#### Pseudo Code (Brute Force)
```
function minEatingSpeed(piles, h):
    k = 1
    while true:
        if canEatAll(piles, k, h):
            return k
        k = k + 1

function canEatAll(piles, speed, h):
    totalHours = 0
    for pile in piles:
        totalHours += ceiling(pile / speed)
    return totalHours <= h
```

#### Python Code (Brute Force Solution)

```python
import math

def minEatingSpeed(piles, h):
    """
    Finds minimum eating speed using brute force approach.
    
    Args:
        piles: List of banana piles where piles[i] is number of bananas in ith pile
        h: Available hours before guards return
    
    Returns:
        Minimum eating speed (k) needed to eat all bananas within h hours
        
    Time Complexity: O(n * max(piles)) where n is the number of piles
    Space Complexity: O(1)
    """
    # Yeh brute force approach hai, har possible speed k ko check karenge
    k = 1
    while True:
        # Is speed par, kya Koko h hours mein sab bananas kha sakti hai?
        if canEatAll(piles, k, h):
            return k
        k += 1
        
def canEatAll(piles, speed, h):
    """
    Checks if Koko can eat all bananas at given speed within h hours.
    
    Args:
        piles: List of banana piles
        speed: Current eating speed to check
        h: Available hours
        
    Returns:
        True if Koko can eat all bananas within h hours, False otherwise
    """
    total_hours = 0
    # Har ek pile ke liye, kitne hours lagenge check karo
    for pile in piles:
        # Math.ceil use karte hai kyunki partial pile bhi pura hour leta hai
        total_hours += math.ceil(pile / speed)
    return total_hours <= h
```

### 2. Binary Search Solution

Binary search is more efficient. Since we know the minimum speed is 1 and max speed 
can't be more than the largest pile, we can use binary search to find minimum k.

#### Pseudo Code (Binary Search)
```
function minEatingSpeed(piles, h):
    left = 1
    right = max(piles)
    
    while left < right:
        mid = left + (right - left) // 2
        
        if canEatAll(piles, mid, h):
            right = mid
        else:
            left = mid + 1
            
    return left
```

#### Python Code (Binary Search Solution)

```python
import math

def minEatingSpeed(piles, h):
    """
    Finds minimum eating speed using binary search approach.
    
    Args:
        piles: List of banana piles where piles[i] is number of bananas in ith pile
        h: Available hours before guards return
    
    Returns:
        Minimum eating speed (k) needed to eat all bananas within h hours
        
    Time Complexity: O(n * log(max(piles))) where n is the number of piles
    Space Complexity: O(1)
    """
    # Binary search ka use karke optimized solution banayenge
    # Minimum speed 1 ho sakta hai, maximum speed largest pile ke barabar hoga
    left = 1
    right = max(piles)
    
    # Binary search to find minimum valid speed
    while left < right:
        mid = left + (right - left) // 2  # Overflow se bachne ke liye
        
        # Check if Koko can eat all bananas at 'mid' speed within h hours
        if canEatAll(piles, mid, h):
            # Agar mid speed par possible hai, to chota speed bhi check karo
            right = mid
        else:
            # Agar mid speed par possible nahi hai, to speed badhana padega
            left = mid + 1
    
    # Left pointer will be at the minimum valid speed
    return left
        
def canEatAll(piles, speed, h):
    """
    Checks if Koko can eat all bananas at given speed within h hours.
    
    Args:
        piles: List of banana piles
        speed: Current eating speed to check
        h: Available hours
        
    Returns:
        True if Koko can eat all bananas within h hours, False otherwise
    """
    total_hours = 0
    for pile in piles:
        # Math.ceil use karte hai kyunki partial pile bhi pura hour leta hai
        total_hours += math.ceil(pile / speed)
    return total_hours <= h
```

## Example to Visualize the Solution

Let's consider an example:
- piles = [3, 6, 7, 11]
- h = 8

### Brute Force Approach Visualization

```
Testing k=1:
  Pile 1 (3 bananas): Needs ceil(3/1) = 3 hours
  Pile 2 (6 bananas): Needs ceil(6/1) = 6 hours
  Pile 3 (7 bananas): Needs ceil(7/1) = 7 hours
  Pile 4 (11 bananas): Needs ceil(11/1) = 11 hours
  Total hours needed: 3 + 6 + 7 + 11 = 27 hours > 8 hours ❌
  
Testing k=2:
  Pile 1 (3 bananas): Needs ceil(3/2) = 2 hours
  Pile 2 (6 bananas): Needs ceil(6/2) = 3 hours
  Pile 3 (7 bananas): Needs ceil(7/2) = 4 hours
  Pile 4 (11 bananas): Needs ceil(11/2) = 6 hours
  Total hours needed: 2 + 3 + 4 + 6 = 15 hours > 8 hours ❌
  
...continues testing...

Testing k=4:
  Pile 1 (3 bananas): Needs ceil(3/4) = 1 hour
  Pile 2 (6 bananas): Needs ceil(6/4) = 2 hours
  Pile 3 (7 bananas): Needs ceil(7/4) = 2 hours
  Pile 4 (11 bananas): Needs ceil(11/4) = 3 hours
  Total hours needed: 1 + 2 + 2 + 3 = 8 hours = 8 hours ✓
  
Answer: k = 4
```

### Binary Search Approach Visualization
```
Initial range: [1, 11]

Iteration 1:
  mid = (1 + 11) // 2 = 6
  Check k=6:
  Total hours needed: ceil(3/6) + ceil(6/6) + ceil(7/6) + ceil(11/6) = 1 + 1 + 2 + 2 = 6 hours
  6 hours ≤ 8 hours, so we can try smaller speeds: new range [1, 6]
  
Iteration 2:
  mid = (1 + 6) // 2 = 3
  Check k=3:
  Total hours needed: ceil(3/3) + ceil(6/3) + ceil(7/3) + ceil(11/3) = 1 + 2 + 3 + 4 = 10 hours
  10 hours > 8 hours, so k=3 is too slow: new range [4, 6]
  
Iteration 3:
  mid = (4 + 6) // 2 = 5
  Check k=5:
  Total hours needed: ceil(3/5) + ceil(6/5) + ceil(7/5) + ceil(11/5) = 1 + 2 + 2 + 3 = 8 hours
  8 hours = 8 hours, so we can try smaller speeds: new range [4, 5]
  
Iteration 4:
  mid = (4 + 5) // 2 = 4
  Check k=4:
  Total hours needed: ceil(3/4) + ceil(6/4) + ceil(7/4) + ceil(11/4) = 1 + 2 + 2 + 3 = 8 hours
  8 hours = 8 hours, so we can try smaller speeds: new range [4, 4]
  
Range converges to 4, so answer is k = 4
```

## Conclusion

Both approaches find that Koko needs to eat at a minimum speed of 4 bananas per 
hour to finish all bananas before the guards return. However, the binary search 
approach is much more efficient:

- Brute Force Time Complexity: O(n * max(piles))
- Binary Search Time Complexity: O(n * log(max(piles)))

For large inputs, binary search provides a significant performance improvement.

