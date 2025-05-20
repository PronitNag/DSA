# Counting Bits Problem Solution with ASCII Art Visualization

## Problem Statement:
Given an integer n, count the number of 1's in the binary representation of every number in the range [0, n].
Return an array output where output[i] is the number of 1's in the binary representation of i.

## Visual Representation of the Problem

```
INPUT: n = 5
RANGE: [0, 1, 2, 3, 4, 5]

BINARY REPRESENTATION:
0 → 0       (0 ones)
1 → 1       (1 one)
2 → 10      (1 one)
3 → 11      (2 ones)
4 → 100     (1 one)
5 → 101     (2 ones)

OUTPUT: [0, 1, 1, 2, 1, 2]
```

## Solution 1: Bit Mask - I

### ASCII Art Visualization:

```
                                                     Count
              Binary    Counting Process             of 1's
              ------    ----------------             ------
    0    =    00000     0+0+0+0+0                  =   0
    1    =    00001     0+0+0+0+1                  =   1
    2    =    00010     0+0+0+1+0                  =   1
    3    =    00011     0+0+0+1+1                  =   2
    4    =    00100     0+0+1+0+0                  =   1
    5    =    00101     0+0+1+0+1                  =   2
    
    Process: For each number i from 0 to n
           ↓
    Check each bit position (using mask)
           ↓
    Count the 1's by shifting mask and using bitwise AND
           ↓
    [0,1,1,2,1,2] ← Final result array
```

### Pseudo Code:
```
function countBits(n):
    result = array of size n+1 initialized with 0s
    
    for i from 0 to n:
        count = 0
        num = i
        
        while num > 0:
            if num & 1 == 1:
                count = count + 1
            num = num >> 1
            
        result[i] = count
        
    return result
```

### Python Code:
```python
def countBits_bitmask1(n):
    """
    Solution using Bit Mask approach - Method I
    Time Complexity: O(n * log n) - We check each bit for each number
    Space Complexity: O(n) - For storing the result array
    
    Ye function har number ke liye bit by bit check karta hai aur ones count 
    karta hai using right shift operation.
    """
    result = [0] * (n + 1)  # Initialize result array with n+1 zeros
    
    for i in range(n + 1):
        count = 0  # Initialize count of set bits for current number
        num = i    # Make a copy of current number for bit manipulation
        
        # Count set bits by checking each bit position
        while num > 0:
            count += num & 1  # Add the least significant bit to count
            num >>= 1         # Right shift to check the next bit
            
        result[i] = count  # Store count in result array
        
    return result
```

## Solution 2: Bit Mask - II

### ASCII Art Visualization:

```
              Binary    Checking Process using n&(n-1)    Count
              ------    --------------------------        ------
    0    =    00000     Initial count = 0                =   0
    
    1    =    00001     Initial count = 0
                        1&0=0 → count++ → 1              =   1
                        
    2    =    00010     Initial count = 0
                        2&1=0 → count++ → 1              =   1
                        
    3    =    00011     Initial count = 0
                        3&2=2 → count++ → 1
                        2&1=0 → count++ → 2              =   2
                        
    4    =    00100     Initial count = 0 
                        4&3=0 → count++ → 1              =   1
                        
    5    =    00101     Initial count = 0
                        5&4=4 → count++ → 1
                        4&3=0 → count++ → 2              =   2
                        
    Process: For each number, use n&(n-1) to clear lowest set bit until n=0
           ↓
    Count how many times this operation can be performed
           ↓
    [0,1,1,2,1,2] ← Final result array
```

### Pseudo Code:
```
function countBits(n):
    result = array of size n+1 initialized with 0s
    
    for i from 0 to n:
        count = 0
        num = i
        
        while num > 0:
            num = num & (num - 1)  # Clear the least significant set bit
            count = count + 1
            
        result[i] = count
        
    return result
```

### Python Code:
```python
def countBits_bitmask2(n):
    """
    Solution using Bit Mask approach - Method II
    Time Complexity: O(n * k) where k is avg number of set bits per number
    Space Complexity: O(n) - For storing the result array
    
    Ye approach n&(n-1) technique use karta hai jo har step mein lowest set bit
    ko clear karta hai, jisse calculation faster ho jati hai.
    """
    result = [0] * (n + 1)
    
    for i in range(n + 1):
        count = 0
        num = i
        
        # Count set bits using Brian Kernighan's algorithm
        while num > 0:
            num &= (num - 1)  # Clear the least significant set bit
            count += 1
            
        result[i] = count
        
    return result
```

## Solution 3: Bit Mask (Optimal)

### ASCII Art Visualization:

```
    Number    Binary    Pattern Observation                 Count
    ------    ------    ------------------                  ------
    0    =    00000     base case                         =   0
    
    1    =    00001     1 = 1 more set bit than 0         =   1
    
    2    =    00010     2 = 1 more set bit than 0         =   1
    3    =    00011     3 = 1 more set bit than 2         =   2
    
    4    =    00100     4 = 1 more set bit than 0         =   1
    5    =    00101     5 = 1 more set bit than 4         =   2
    6    =    00110     6 = 1 more set bit than 4         =   2
    7    =    00111     7 = 1 more set bit than 6         =   3
    
    Pattern: result[i] = result[i & (i-1)] + 1
    
    For any number i, we can find bits in i by adding 1 to bits in i with its
    least significant bit removed.
    
    Starting from → [0,0,0,0,0,0] 
                 → [0,1,0,0,0,0] (process 1)
                 → [0,1,1,0,0,0] (process 2)
                 → [0,1,1,2,0,0] (process 3)
                 → [0,1,1,2,1,0] (process 4)
                 → [0,1,1,2,1,2] (process 5)
                 → [0,1,1,2,1,2] ← Final result
```

### Pseudo Code:
```
function countBits(n):
    result = array of size n+1 initialized with 0s
    
    for i from 1 to n:
        result[i] = result[i & (i - 1)] + 1
        
    return result
```

### Python Code:
```python
def countBits_optimal(n):
    """
    Solution using optimal Bit Mask approach
    Time Complexity: O(n) - We process each number exactly once
    Space Complexity: O(n) - For storing the result array
    
    Is approach mein hum dynamic programming use karte hain - har number i ke liye,
    hum uske least significant bit ko hatake jo number banta hai, uske ones count
    mein bas 1 add karte hain.
    """
    result = [0] * (n + 1)
    
    for i in range(1, n + 1):
        # i&(i-1) removes the least significant set bit
        # So result[i] is one more than result of i with its LSB removed
        result[i] = result[i & (i - 1)] + 1
        
    return result
```

## Solution 4: Built-In Function

### ASCII Art Visualization:

```
     Python's bin() → count 1's
            ↓
    0 → '0b0'     → 0 ones
    1 → '0b1'     → 1 ones
    2 → '0b10'    → 1 ones
    3 → '0b11'    → 2 ones
    4 → '0b100'   → 1 ones
    5 → '0b101'   → 2 ones
    
    Process: Convert each number to binary string using bin()
           ↓
    Count the '1' characters in each binary string
           ↓
    [0,1,1,2,1,2] ← Final result array
```

### Pseudo Code:
```
function countBits(n):
    result = array of size n+1 initialized with 0s
    
    for i from 0 to n:
        result[i] = count of '1's in binary representation of i
        
    return result
```

### Python Code:
```python
def countBits_builtin(n):
    """
    Solution using Python's built-in function
    Time Complexity: O(n * log n) - bin() function takes O(log n) time
    Space Complexity: O(n) - For storing the result array
    
    Ye solution Python ke built-in bin() function ka use karta hai, jo number ko
    binary string mein convert karta hai, aur phir hum usme '1' count karte hain.
    """
    result = []
    
    for i in range(n + 1):
        # bin(i) returns binary string with '0b' prefix, so we remove it with [2:]
        # count() counts the number of '1' characters in the binary string
        result.append(bin(i)[2:].count('1'))
        
    return result
```

## Solution 5: Bit Manipulation (DP)

### ASCII Art Visualization:

```
    Number    Binary    DP Pattern                          Count
    ------    ------    ----------                          ------
    0    =    00000     base case                         =   0
    
    1    =    00001     1 = 0 + 1 (1=2^0+0, offset=2^0=1) =   1
    
    2    =    00010     2 = 0 + 1 (2=2^1+0, offset=2^1=2) =   1
    3    =    00011     3 = 1 + 1 (3=2^1+1, offset=2^1=2) =   2
    
    4    =    00100     4 = 0 + 1 (4=2^2+0, offset=2^2=4) =   1
    5    =    00101     5 = 1 + 1 (5=2^2+1, offset=2^2=4) =   2
    
    Pattern: result[i] = result[i - offset] + 1, where offset is the largest
             power of 2 less than or equal to i
    
    Starting from → [0,0,0,0,0,0] 
    Process 1 → [0,1,0,0,0,0] (offset=1)
    Process 2 → [0,1,1,0,0,0] (offset=2)
    Process 3 → [0,1,1,2,0,0] (offset=2)
    Process 4 → [0,1,1,2,1,0] (offset=4)
    Process 5 → [0,1,1,2,1,2] (offset=4)
    → [0,1,1,2,1,2] ← Final result
```

### Pseudo Code:
```
function countBits(n):
    result = array of size n+1 initialized with 0s
    offset = 1
    
    for i from 1 to n:
        if offset * 2 == i:
            offset = i
        result[i] = result[i - offset] + 1
        
    return result
```

### Python Code:
```python
def countBits_dp(n):
    """
    Solution using DP with bit manipulation
    Time Complexity: O(n) - We process each number exactly once
    Space Complexity: O(n) - For storing the result array
    
    Ye dynamic programming approach hai jisme hum observe karte hain ki agar
    i = 2^k + j hai, toh result[i] = result[j] + 1 hoga. Yahan offset 2^k hai.
    """
    result = [0] * (n + 1)
    offset = 1  # Represents the current power of 2
    
    for i in range(1, n + 1):
        # Update offset when we reach a power of 2
        if offset * 2 == i:
            offset = i
        
        # DP relation: result[i] = result[i-offset] + 1
        result[i] = result[i - offset] + 1
        
    return result
```

## Comparing All Solutions

```
           Method              | Time Complexity | Space Complexity
-------------------------------|----------------|------------------
Bit Mask - I                   | O(n * log n)   | O(n)
Bit Mask - II                  | O(n * k)       | O(n)
Bit Mask (Optimal)             | O(n)           | O(n)
Built-In Function              | O(n * log n)   | O(n)
Bit Manipulation (DP)          | O(n)           | O(n)
```

## Summary with Emoji Art

```
🔢 Problem: Count 1's in binary of each number from 0 to n
  
🔍 Example for n=5:
  0 ➡️ 0️⃣0️⃣0️⃣ ➡️ 0 ones
  1 ➡️ 0️⃣0️⃣1️⃣ ➡️ 1 one
  2 ➡️ 0️⃣1️⃣0️⃣ ➡️ 1 one
  3 ➡️ 0️⃣1️⃣1️⃣ ➡️ 2 ones
  4 ➡️ 1️⃣0️⃣0️⃣ ➡️ 1 one
  5 ➡️ 1️⃣0️⃣1️⃣ ➡️ 2 ones
  
⚙️ Solutions:
  1️⃣ Bit Mask I: Check each bit one by one
  2️⃣ Bit Mask II: Use n&(n-1) to clear bits
  3️⃣ Optimal: Use pattern result[i] = result[i&(i-1)] + 1
  4️⃣ Built-in: Use Python's bin().count()
  5️⃣ DP: Use pattern result[i] = result[i-offset] + 1
  
✅ Answer: [0,1,1,2,1,2]
```

## Conclusion

The most efficient solutions are the Optimal Bit Mask approach and the DP approach, both with O(n) time complexity. The key insight is recognizing the pattern between a number and the count of its set bits, which allows us to avoid repeatedly checking each bit for every number.

The beauty of this problem is that it shows how understanding binary representation and bit manipulation can lead to elegant and efficient solutions.

