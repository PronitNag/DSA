# Number of One Bits: Solution with ASCII Visualization

## Problem Statement

You are given an unsigned integer n. Return the number of 1 bits in its binary representation.

You may assume n is a non-negative integer which fits within 32-bits.

## ASCII Art Visualization of the Solution Approaches

```
INPUT INTEGER: n = 13 (example)
                    |
                    v
BINARY REPRESENTATION: 1 1 0 1
                        ↑ ↑   ↑
                        | |   |
                        1 1   1  ← BITS TO COUNT = 3
                        |
                        v
OUTPUT: 3 (count of 1 bits)
```

## Solution 1: Bit Mask - I (Shift n by 1 bit at a time)

```
n = 13 (1101 in binary)
                              ┌─────┐
                              │Count│
                              │  0  │
                              └─────┘
                                 |
                                 v
┌───┬───┬───┬───┐      ┌───┐
│ 1 │ 1 │ 0 │ 1 │  AND │ 1 │    → Result = 1   → Count = 1
└───┴───┴───┴───┘      └───┘
      n = 13          mask = 1
                                 |
                                 v
┌───┬───┬───┬───┐      ┌───┐
│ 0 │ 1 │ 1 │ 0 │  AND │ 1 │    → Result = 0   → Count = 1
└───┴───┴───┴───┘      └───┘
    n>>1 = 6          mask = 1
                                 |
                                 v
┌───┬───┬───┬───┐      ┌───┐
│ 0 │ 0 │ 1 │ 1 │  AND │ 1 │    → Result = 1   → Count = 2
└───┴───┴───┴───┘      └───┘
    n>>2 = 3          mask = 1
                                 |
                                 v
┌───┬───┬───┬───┐      ┌───┐
│ 0 │ 0 │ 0 │ 1 │  AND │ 1 │    → Result = 1   → Count = 3
└───┴───┴───┴───┘      └───┘ 
    n>>3 = 0          mask = 1
                                 |
                                 v
                              ┌─────┐
                              │Count│
                              │  3  │
                              └─────┘
```

## Solution 2: Bit Mask - II (Shift mask by 1 bit at a time)

```
n = 13 (1101 in binary)
                              ┌─────┐
                              │Count│
                              │  0  │
                              └─────┘
                                 |
                                 v
┌───┬───┬───┬───┐      ┌───┬───┬───┬───┐
│ 1 │ 1 │ 0 │ 1 │  AND │ 0 │ 0 │ 0 │ 1 │  → Result = 1  → Count = 1
└───┴───┴───┴───┘      └───┴───┴───┴───┘
      n = 13            mask = 1
                                 |
                                 v
┌───┬───┬───┬───┐      ┌───┬───┬───┬───┐
│ 1 │ 1 │ 0 │ 1 │  AND │ 0 │ 0 │ 1 │ 0 │  → Result = 0  → Count = 1
└───┴───┴───┴───┘      └───┴───┴───┴───┘
      n = 13           mask<<1 = 2
                                 |
                                 v
┌───┬───┬───┬───┐      ┌───┬───┬───┬───┐
│ 1 │ 1 │ 0 │ 1 │  AND │ 0 │ 1 │ 0 │ 0 │  → Result ≠ 0  → Count = 2
└───┴───┴───┴───┘      └───┴───┴───┴───┘
      n = 13           mask<<2 = 4
                                 |
                                 v
┌───┬───┬───┬───┐      ┌───┬───┬───┬───┐
│ 1 │ 1 │ 0 │ 1 │  AND │ 1 │ 0 │ 0 │ 0 │  → Result ≠ 0  → Count = 3
└───┴───┴───┴───┘      └───┴───┴───┴───┘
      n = 13           mask<<3 = 8
                                 |
                                 v
                              ┌─────┐
                              │Count│
                              │  3  │
                              └─────┘
```

## Solution 3: Bit Mask (Optimal) - Brian Kernighan's Algorithm

```
n = 13 (1101 in binary)
                              ┌─────┐
                              │Count│
                              │  0  │
                              └─────┘
                                 |
                                 v
┌───┬───┬───┬───┐
│ 1 │ 1 │ 0 │ 1 │  Initial n = 13
└───┴───┴───┴───┘
        |
        v
┌───┬───┬───┬───┐      ┌───┬───┬───┬───┐
│ 1 │ 1 │ 0 │ 1 │  AND │ 1 │ 1 │ 0 │ 0 │  → n & (n-1) = 12
└───┴───┴───┴───┘      └───┴───┴───┴───┘
    n = 13            n-1 = 12
                                 |
                                 v
┌───┬───┬───┬───┐             (Removed rightmost 1)
│ 1 │ 1 │ 0 │ 0 │  n = 12     Count = 1
└───┴───┴───┴───┘
        |
        v
┌───┬───┬───┬───┐      ┌───┬───┬───┬───┐
│ 1 │ 1 │ 0 │ 0 │  AND │ 1 │ 0 │ 1 │ 1 │  → n & (n-1) = 8
└───┴───┴───┴───┘      └───┴───┴───┴───┘
    n = 12            n-1 = 11
                                 |
                                 v
┌───┬───┬───┬───┐             (Removed rightmost 1)
│ 1 │ 0 │ 0 │ 0 │  n = 8      Count = 2
└───┴───┴───┴───┘
        |
        v
┌───┬───┬───┬───┐      ┌───┬───┬───┬───┐
│ 1 │ 0 │ 0 │ 0 │  AND │ 0 │ 1 │ 1 │ 1 │  → n & (n-1) = 0
└───┴───┴───┴───┘      └───┴───┴───┴───┘
    n = 8             n-1 = 7
                                 |
                                 v
┌───┬───┬───┬───┐             (Removed rightmost 1)
│ 0 │ 0 │ 0 │ 0 │  n = 0      Count = 3
└───┴───┴───┴───┘
                                 |
                                 v
                              ┌─────┐
                              │Count│
                              │  3  │
                              └─────┘
```

## Solution 4: Built-In Function

```
n = 13 (1101 in binary)
        |
        v
┌───────────────────┐
│ bin(n) = "0b1101" │  Convert to binary string 
└───────────────────┘
        |
        v
┌────────────────┐
│ count('1') = 3 │  Count the '1' characters in the string
└────────────────┘
        |
        v
      ┌─────┐
      │Count│
      │  3  │
      └─────┘
```

## Emoji Art Visualization 🎨

```
INPUT: 🔢 (13)
   |
   v
BINARY: 1️⃣1️⃣0️⃣1️⃣
   |
   v
COUNT: 3️⃣ ones!
```

## Pseudocode and Solution Implementations

### Approach 1: Bit Mask - I (Shifting n)

#### Pseudocode:
```
function hammingWeight(n):
    count = 0
    for i from 0 to 31:
        if (n & 1) == 1:
            count += 1
        n = n >> 1
    return count
```

#### Python Code Solution:
```python
def hammingWeight_method1(n: int) -> int:
    """
    Solution Method 1: Bit Mask - I (Shifting n)
    
    Hum n ko right shift kar kar ke har bit ko check karenge.
    Har iteration me n ka rightmost bit AND operation se check hoga.
    
    Time Complexity: O(32) = O(1) since n is a 32-bit integer
    Space Complexity: O(1)
    """
    count = 0
    # 32-bit integer me max 32 bits hote hain, isliye 32 times loop chalega
    for i in range(32):
        # 1 ke saath AND operation se rightmost bit check hoti hai
        if (n & 1) == 1:
            count += 1
        # Right shift karne se next bit check karne ke liye ready ho jaata hai
        n = n >> 1
        # Agar n already 0 ho gaya hai to loop se bahar nikal jao
        if n == 0:
            break
    return count
```

### Approach 2: Bit Mask - II (Shifting Mask)

#### Pseudocode:
```
function hammingWeight(n):
    count = 0
    mask = 1
    for i from 0 to 31:
        if (n & mask) != 0:
            count += 1
        mask = mask << 1
    return count
```

#### Python Code Solution:
```python
def hammingWeight_method2(n: int) -> int:
    """
    Solution Method 2: Bit Mask - II (Shifting Mask)
    
    Is approach me hum mask ko left shift karte hain aur n ke har bit ko
    check karte hain.
    
    Time Complexity: O(32) = O(1) since n is a 32-bit integer
    Space Complexity: O(1)
    """
    count = 0
    mask = 1
    # 32-bit integer me max 32 bits hote hain, isliye 32 times loop chalega
    for i in range(32):
        # Mask ke saath AND operation se particular position pe bit check hoti hai
        if (n & mask) != 0:
            count += 1
        # Left shift karne se mask agle bit position ko check karne ke liye ready
        mask = mask << 1
    return count
```

### Approach 3: Brian Kernighan's Algorithm (Optimal)

#### Pseudocode:
```
function hammingWeight(n):
    count = 0
    while n != 0:
        n = n & (n - 1)  # Removes the rightmost set bit
        count += 1
    return count
```

#### Python Code Solution:
```python
def hammingWeight_method3(n: int) -> int:
    """
    Solution Method 3: Brian Kernighan's Algorithm (Optimal)
    
    Ye approach sabse efficient hai kyunki sirf utne iterations hote hain
    jitne 1-bits hain.
    
    n & (n-1) operation n ke rightmost set bit (1) ko hata deta hai.
    
    Time Complexity: O(number of 1-bits) which is ≤ 32 for 32-bit integers
    Space Complexity: O(1)
    """
    count = 0
    while n:
        # n & (n-1) rightmost set bit ko 0 kar deta hai
        n = n & (n - 1)
        count += 1
    return count
```

### Approach 4: Built-In Function

#### Pseudocode:
```
function hammingWeight(n):
    return count_of_1s_in_binary_representation(n)
```

#### Python Code Solution:
```python
def hammingWeight_method4(n: int) -> int:
    """
    Solution Method 4: Built-In Function
    
    Python me binary conversion aur counting built-in functions se bhi 
    ho sakti hai.
    
    Time Complexity: O(log n) for binary conversion
    Space Complexity: O(log n) for storing the binary string
    """
    # bin(n) function n ko binary string me convert karta hai '0b1101' format me
    # [2:] se starting ke '0b' ko hata dete hain, phir '1' ko count karte hain
    return bin(n)[2:].count('1')
```

## Performance Comparison

| Method               | Time Complexity                | Space Complexity | Iterations  |
|----------------------|--------------------------------|------------------|-------------|
| Bit Mask - I         | O(32) = O(1)                   | O(1)             | Up to 32    |
| Bit Mask - II        | O(32) = O(1)                   | O(1)             | Always 32   |
| Brian Kernighan's    | O(number of 1-bits) ≤ O(32)    | O(1)             | # of 1-bits |
| Built-In Function    | O(log n)                       | O(log n)         | -           |

## Conclusion

Brian Kernighan's Algorithm (Solution 3) is the most efficient for this problem:
- It only iterates once per set bit (1-bit) in the number
- For sparse numbers with few 1s, it performs significantly better
- No need for language-specific functions or string operations

The beauty of this algorithm is that `n & (n-1)` always flips the rightmost set bit to 0, allowing us to count exactly how many 1-bits exist in the number.

