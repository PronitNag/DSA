# Pow(x, n) Solutions with ASCII Art Visualization

## Problem Statement

**50. Pow(x, n)**: Implement pow(x, n), which calculates `x` raised to the power `n` (i.e., `xn`).

## ASCII Art Visualization of Binary Exponentiation

```
                  START: Calculate x^n
                         /    \
                        /      \
                       /        \
                      /          \
            Special Cases        Main Logic
             /      \             /     \
            /        \           /       \
           /          \         /         \
       n = 0         n < 0     +ve n    Recursive/Iterative
         |             |        |        Approach
     Return 1    Return 1/x^|n| |        /       \
                              \  |       /         \
                               \ |      /           \
                          Binary Exponentiation     Brute Force
                           /            \                |
                          /              \               |
                         /                \              |
                   Recursive            Iterative    Multiply x
                      |                    |          n times
                      |                    |             |
                 x^n = (x^2)^(n/2)     Use bits      O(n) time
                 if n is even          of n to
                      |                multiply        
                 x^n = x * (x^2)^((n-1)/2)            
                 if n is odd               |
                      |                    |
                 O(log n) time        O(log n) time
                      \                   /
                       \                 /
                        \               /
                     END: Return Result x^n
```

## Evolution of Calculation: x^10

```
BINARY EXPONENTIATION VISUALIZATION (n = 10 = 1010₂)

Step 1:                    Step 2:                    Step 3:
   x^10                       x^10                       x^10
   /   \                      /   \                      /   \
  /     \                    /     \                    /     \
x^5     x^5                x^5     x^5                x^5     x^5
 |       |                  |       |                 / \      / \
 |       |                 / \     / \              x^2 x^1  x^2 x^1
result = x^10             x^2 x^1 x^2 x^1           |  |    |   |
                           |   |   |   |            x² · x  x² · x
                          x² · x x² · x             |       |
                           |       |              x^5 = x^4 · x
                         x^5 = x^4 · x              |       |
                           |       |              result = x^10
                        result = x^10
```

## 1. Brute Force Solution

```python
def myPow_brute(x, n):
    """
    Brute force approach for calculating x^n
    
    Args:
        x: base (float)
        n: exponent (int)
    
    Returns:
        x raised to power n (float)
    
    # Hinglish comment:
    # Ye function brute force tarike se x^n calculate karta hai,
    # Isme hum simply n baar x ko multiply karte hain, lekin ye approach
    # O(n) time complexity leta hai jo ki efficient nahi hai bade n ke liye
    """
    if n == 0:
        return 1
        
    if n < 0:
        x = 1 / x
        n = -n
    
    result = 1
    for _ in range(n):
        result *= x
        
    return result
```

## 2. Binary Exponentiation (Recursive)

```python
def myPow_recursive(x, n):
    """
    Binary exponentiation using recursive approach for calculating x^n
    
    Args:
        x: base (float)
        n: exponent (int)
    
    Returns:
        x raised to power n (float)
    
    # Hinglish comment:
    # Is function mein hum binary exponentiation ka recursive tarika use karte hain
    # Agar n even hai, toh x^n = (x^2)^(n/2)
    # Agar n odd hai, toh x^n = x * (x^2)^((n-1)/2)
    # Ye approach O(log n) time complexity provide karta hai jo ki bahut efficient hai
    """
    def fast_pow(base, power):
        if power == 0:
            return 1
            
        # Calculate half of the exponentiation recursively
        half = fast_pow(base, power // 2)
        
        # If power is even, return half * half
        if power % 2 == 0:
            return half * half
        # If power is odd, return base * half * half
        else:
            return base * half * half
    
    # Handle negative exponent
    if n < 0:
        return 1 / fast_pow(x, -n)
    else:
        return fast_pow(x, n)
```

## 3. Binary Exponentiation (Iterative)

```python
def myPow_iterative(x, n):
    """
    Binary exponentiation using iterative approach for calculating x^n
    
    Args:
        x: base (float)
        n: exponent (int)
    
    Returns:
        x raised to power n (float)
    
    # Hinglish comment:
    # Ye function binary exponentiation ka iterative version use karta hai
    # Isme hum n ko binary representation mein convert karte hain aur har bit
    # ke liye appropriate calculation karte hain. Ye bhi O(log n) time complexity
    # deta hai aur stack overflow se bhi bachata hai jo recursive approach mein
    # ho sakta hai agar n bahut bada ho toh
    """
    # Handle edge case
    if n == 0:
        return 1
        
    # Handle negative exponent
    if n < 0:
        x = 1 / x
        n = -n
    
    result = 1
    current_product = x
    
    # Process each bit of n
    while n > 0:
        # If current bit is 1, multiply result by current_product
        if n & 1:
            result *= current_product
            
        # Square the current_product for next bit
        current_product *= current_product
        
        # Shift n to right by 1 bit (equivalent to n //= 2)
        n >>= 1
        
    return result
```

## Comparison of Approaches

```
+------------------------+---------------------+---------------------+
|                        | Time Complexity     | Space Complexity    |
+------------------------+---------------------+---------------------+
| Brute Force            | O(n)                | O(1)                |
| Binary Exp (Recursive) | O(log n)            | O(log n) - stack    |
| Binary Exp (Iterative) | O(log n)            | O(1)                |
+------------------------+---------------------+---------------------+
```

## Example Execution for x = 2, n = 10

```
BRUTE FORCE:
2^10 = 2 × 2 × 2 × 2 × 2 × 2 × 2 × 2 × 2 × 2 = 1024
(Requires 10 multiplications)

BINARY EXPONENTIATION:
n = 10 = 1010₂

Step 1: Initialize result = 1, current_product = 2

Step 2: Check least significant bit of n (0)
   - Skip multiplication (bit is 0)
   - Square current_product: 2² = 4
   - n = 5 (101₂)

Step 3: Check least significant bit of n (1)
   - Multiply result by current_product: 1 × 4 = 4
   - Square current_product: 4² = 16
   - n = 2 (10₂)

Step 4: Check least significant bit of n (0)
   - Skip multiplication (bit is 0)
   - Square current_product: 16² = 256
   - n = 1 (1₂)

Step 5: Check least significant bit of n (1)
   - Multiply result by current_product: 4 × 256 = 1024
   - Square current_product: 256² = 65536
   - n = 0 (0₂)

Final result: 1024
(Requires only 4 multiplications)
```

## Closing Thoughts

Binary exponentiation is a powerful technique that reduces the time complexity from 
O(n) to O(log n), which is a significant improvement for large values of n. The 
iterative approach is generally preferred in production code as it avoids potential
stack overflow issues with the recursive approach for very large values of n.

