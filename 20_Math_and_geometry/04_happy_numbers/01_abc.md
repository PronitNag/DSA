# Non-Cyclical Number Solutions

## Problem Definition

A non-cyclical number is an integer defined by the following algorithm:
1. Given a positive integer, replace it with the sum of the squares of its digits.
2. Repeat the above step until the number equals 1, or it loops infinitely in a cycle which does not include 1.
3. If it stops at 1, then the number is a non-cyclical number.

Given a positive integer n, return true if it is a non-cyclical number, otherwise return false.

## Visual Representation of the Problem

```
                 +--------+
                 | START  |
                 +--------+
                     |
                     v
              +--------------+
              | Input: n > 0 |
              +--------------+
                     |
                     v
          +--------------------+
          | seen = empty set   |
          +--------------------+
                     |
                     v
          +--------------------+
          | Is n == 1?         |---------> YES -------> +------------------+
          +--------------------+                        | Return True       |
                     | NO                               | (Non-cyclical)    |
                     v                                  +------------------+
          +--------------------+
          | Is n in seen?      |---------> YES -------> +------------------+
          +--------------------+                        | Return False      |
                     | NO                               | (Cyclical)        |
                     v                                  +------------------+
          +--------------------+
          | Add n to seen      |
          +--------------------+
                     |
                     v
          +--------------------+
          | Calculate new n:   |
          | Sum of squares of  |
          | its digits         |
          +--------------------+
                     |
                     +----> (Loop back to "Is n == 1?")
```

### Emoji Art Representation

```
🚀 Start with n
   |
   v
🔍 Check: Is n = 1? ➡️ Yes ➡️ 🎉 True (Non-cyclical)
   |
   ❌ No
   |
   v
🔄 Has n been seen? ➡️ Yes ➡️ 🔄 False (Cyclical)
   |
   ❌ No
   |
   v
📝 Record n in seen set
   |
   v
🧮 n = sum of squares of digits
   |
   +---> (Loop back)
```

## Evolution of the Process for Example n = 19

```
Step 1: Start with n = 19
        seen = { }
        
Step 2: Is 19 = 1? No
        Is 19 in seen? No
        seen = { 19 }
        Calculate: 1² + 9² = 1 + 81 = 82
        
Step 3: Is 82 = 1? No
        Is 82 in seen? No
        seen = { 19, 82 }
        Calculate: 8² + 2² = 64 + 4 = 68
        
Step 4: Is 68 = 1? No
        Is 68 in seen? No
        seen = { 19, 82, 68 }
        Calculate: 6² + 8² = 36 + 64 = 100
        
Step 5: Is 100 = 1? No
        Is 100 in seen? No
        seen = { 19, 82, 68, 100 }
        Calculate: 1² + 0² + 0² = 1 + 0 + 0 = 1
        
Step 6: Is 1 = 1? Yes!
        Return True (19 is a non-cyclical number)
```

## Solution 1: Using Hash Set

### Pseudo Code

```
function isNonCyclical(n):
    seen = empty set
    
    while n != 1 and n not in seen:
        seen.add(n)
        n = sumOfSquaresOfDigits(n)
    
    return n == 1
    
function sumOfSquaresOfDigits(n):
    sum = 0
    while n > 0:
        digit = n % 10
        sum += digit * digit
        n = n / 10
    return sum
```

### Python Implementation

```python
def isNonCyclical(n: int) -> bool:
    """
    Determines if a number is non-cyclical using the hash set approach.
    
    Args:
        n: A positive integer to check
        
    Returns:
        True if n is a non-cyclical number, False otherwise
        
    Time Complexity: O(log n) - We need at most 9²*log(n) iterations
    Space Complexity: O(log n) - For storing visited numbers in the set
    """
    seen = set()  # Yeh set hai dekhne ke liye ki kya number repeat ho raha hai
    
    while n != 1 and n not in seen:
        seen.add(n)  # Number ko seen set mein add karo
        n = sum_of_squares_of_digits(n)  # Naya number banao
        
    return n == 1  # Agar process 1 par ruka hai to non-cyclical hai

def sum_of_squares_of_digits(n: int) -> int:
    """
    Calculate the sum of squares of all digits in a number.
    
    Args:
        n: Input integer
        
    Returns:
        Sum of squares of all digits
    """
    total = 0
    while n > 0:
        digit = n % 10  # Last digit nikalo
        total += digit * digit  # Square karke total mein add karo
        n //= 10  # Number ko chota karo
    return total
```

## Solution 2: Fast And Slow Pointers - I

### Pseudo Code

```
function isNonCyclical(n):
    slow = n
    fast = getNext(n)
    
    while fast != 1 and slow != fast:
        slow = getNext(slow)
        fast = getNext(getNext(fast))
    
    return fast == 1
    
function getNext(n):
    sum = 0
    while n > 0:
        digit = n % 10
        sum += digit * digit
        n = n / 10
    return sum
```

### Python Implementation

```python
def isNonCyclical(n: int) -> bool:
    """
    Determines if a number is non-cyclical using the fast and slow pointers I
    approach (Floyd's Tortoise and Hare algorithm).
    
    Args:
        n: A positive integer to check
        
    Returns:
        True if n is a non-cyclical number, False otherwise
        
    Time Complexity: O(log n) - Similar to solution 1
    Space Complexity: O(1) - Only two pointers are used, constant space
    """
    slow = n  # Yeh hai hamara "kachhua" pointer jo dheere chalta hai
    fast = get_next(n)  # Yeh hai hamara "khargosh" pointer jo tez chalta hai
    
    while fast != 1 and slow != fast:
        slow = get_next(slow)  # Kachhua ek step lega
        fast = get_next(get_next(fast))  # Khargosh do step lega
    
    return fast == 1  # Agar fast 1 tak pahunch gaya to non-cyclical hai

def get_next(n: int) -> int:
    """
    Calculate the next number in the sequence (sum of squares of digits).
    
    Args:
        n: Current number
        
    Returns:
        Next number in the sequence
    """
    total = 0
    while n > 0:
        digit = n % 10  # Last digit nikalo
        total += digit * digit  # Square karke total mein add karo
        n //= 10  # Number ko chota karo
    return total
```

## Solution 3: Fast And Slow Pointers - II

### Pseudo Code

```
function isNonCyclical(n):
    slow = n
    fast = n
    
    do:
        slow = getNext(slow)
        fast = getNext(getNext(fast))
    while slow != fast and fast != 1
    
    return fast == 1
    
function getNext(n):
    sum = 0
    while n > 0:
        digit = n % 10
        sum += digit * digit
        n = n / 10
    return sum
```

### Python Implementation

```python
def isNonCyclical(n: int) -> bool:
    """
    Determines if a number is non-cyclical using the fast and slow pointers II
    approach (modified Floyd's algorithm starting both pointers at n).
    
    Args:
        n: A positive integer to check
        
    Returns:
        True if n is a non-cyclical number, False otherwise
        
    Time Complexity: O(log n) - Similar to previous solutions
    Space Complexity: O(1) - Only two pointers are used, constant space
    """
    # Dono pointers same position se start karte hain
    slow = n  # Yeh hamara dheere chalne wala pointer hai
    fast = n  # Yeh hamara tez chalne wala pointer hai
    
    while True:
        slow = get_next(slow)  # Kachhua ek step lega
        fast = get_next(get_next(fast))  # Khargosh do step lega
        
        if slow == fast or fast == 1:
            break
    
    return fast == 1  # Agar fast 1 tak pahunch gaya to non-cyclical hai

def get_next(n: int) -> int:
    """
    Calculate the next number in the sequence (sum of squares of digits).
    
    Args:
        n: Current number
        
    Returns:
        Next number in the sequence
    """
    total = 0
    while n > 0:
        digit = n % 10  # Last digit nikalo
        total += digit * digit  # Square karke total mein add karo
        n //= 10  # Number ko chota karo
    return total
```

## Time and Space Complexity Comparison

| Solution                 | Time Complexity | Space Complexity |
|--------------------------|----------------|------------------|
| Hash Set                 | O(log n)       | O(log n)         |
| Fast And Slow Pointers I | O(log n)       | O(1)             |
| Fast And Slow Pointers II| O(log n)       | O(1)             |

## Conclusion

All three solutions correctly determine if a number is non-cyclical:

1. **Hash Set Solution**: Uses additional space to store visited numbers, making it easy to 
   detect cycles, but requires O(log n) space.

2. **Fast And Slow Pointers I**: Uses Floyd's Tortoise and Hare algorithm with slow 
   starting at n and fast starting at next(n). Constant space complexity.

3. **Fast And Slow Pointers II**: Slightly modified version where both pointers start at n.
   Also uses constant space.

The pointer-based solutions are more space-efficient as they detect cycles without 
storing all visited numbers.

For LeetCode problems where space efficiency is important, the pointer-based solutions 
would be preferred, though all three will pass the time and space requirements.

