# LeetCode: Multiply Strings Solution with ASCII Art

## Problem Statement
Given two strings `num1` and `num2` representing non-negative integers, return their product as a string. Neither string contains leading zeros unless the number is 0 itself. You cannot use any built-in library to directly convert the inputs to integers.

## ASCII Art Visualization of the Problem

```
       +--------------+          +--------------+
       |    "123"     |          |    "456"     |
       | (as string)  |          | (as string)  |
       +--------------+          +--------------+
              |                         |
              v                         v
     +----------------+        +----------------+
     | Process digits |        | Process digits |
     | one by one     |        | one by one     |
     +----------------+        +----------------+
              |                         |
              +-----------+-------------+
                          |
                          v
               +--------------------+
               | Multiply & Add     |
               | (like on paper)    |
               +--------------------+
                          |
                          v
                  +--------------+
                  |   "56088"    |
                  | (as string)  |
                  +--------------+
```

## Solution Approach #1: Grade School Multiplication Algorithm

This approach mimics how we multiply numbers by hand on paper.

### ASCII Art of Grade School Multiplication

```
                          1  2  3   (num1)
                       x  4  5  6   (num2)
                    -------------------
                          7  3  8   (6×123)
                       6  1  5  0   (5×123, shifted)
                    4  9  2  0  0   (4×123, shifted twice)
                    -------------------
                    5  6  0  8  8   (final sum)
```

### Step-by-Step Visualization

```
Step 1: Initialize result array with zeros [0,0,0,0,0,0]
        (length of num1 + num2 positions)

Step 2: Multiply each digit of num2 with num1

        For the digit '6' in num2's ones place:
        6×3=18: Put 8, carry 1  [0,0,0,0,0,8]
        6×2=12+1=13: Put 3, carry 1  [0,0,0,0,3,8]
        6×1=6+1=7: Put 7  [0,0,0,7,3,8]

        For the digit '5' in num2's tens place:
        5×3=15: Put 5, carry 1  [0,0,0,7,8,8]
        5×2=10+1=11: Put 1, carry 1  [0,0,1,8,8,8]
        5×1=5+1=6: Put 6  [0,0,6,8,8,8]

        For the digit '4' in num2's hundreds place:
        4×3=12: Put 2, carry 1  [0,0,6,10,8,8]
        4×2=8+1=9: Put 9  [0,0,15,10,8,8]
        4×1=4: Put 4  [0,4,15,10,8,8]

Step 3: Handle all carries from right to left
        [0,4,15,10,8,8] → [0,4,15,10,8,8] → [0,4,16,0,8,8] → 
        [0,5,6,0,8,8] → [5,6,0,8,8]

Step 4: Convert the result array to string
        [5,6,0,8,8] → "56088"
```

### Pseudocode

```
function multiply(num1, num2):
    if num1 = "0" or num2 = "0" return "0"
    
    # Initialize result array with zeros
    result = array of length (len(num1) + len(num2)) filled with zeros
    
    # Perform multiplication
    for i from len(num1)-1 to 0 (decrementing):
        for j from len(num2)-1 to 0 (decrementing):
            # Get current digits and calculate product
            digit1 = num1[i] - '0'
            digit2 = num2[j] - '0'
            product = digit1 * digit2
            
            # Update the corresponding positions in result
            pos1 = i + j
            pos2 = i + j + 1
            sum = product + result[pos2]
            
            result[pos1] += sum / 10  # Add carry
            result[pos2] = sum % 10   # Update current position
    
    # Convert result to string
    result_str = ""
    start_idx = 0
    
    # Skip leading zeros
    while start_idx < len(result) - 1 and result[start_idx] = 0:
        start_idx++
    
    for i from start_idx to len(result)-1:
        result_str += result[i] + '0'
    
    return result_str
```

### Python Solution

```python
def multiply(num1: str, num2: str) -> str:
    """
    Multiplies two string numbers without converting them directly to integers.
    Uses the grade school multiplication algorithm.
    
    Args:
        num1: First number as a string without leading zeros
        num2: Second number as a string without leading zeros
        
    Returns:
        The product of num1 and num2 as a string
        
    Time Complexity: O(m*n) where m and n are lengths of input strings
    Space Complexity: O(m+n) for the result array
    """
    # Edge case: if either number is 0, result is 0
    if num1 == "0" or num2 == "0":
        return "0"
    
    # Initialize an array to store the result, with size m+n
    m, n = len(num1), len(num2)
    result = [0] * (m + n)
    
    # Perform digit-by-digit multiplication like in grade school
    for i in range(m - 1, -1, -1):
        for j in range(n - 1, -1, -1):
            # Get current digits and calculate their product
            digit1 = ord(num1[i]) - ord('0')
            digit2 = ord(num2[j]) - ord('0')
            product = digit1 * digit2
            
            # Calculate positions to update in the result array
            pos1 = i + j
            pos2 = i + j + 1
            total = product + result[pos2]
            
            # Update result with quotient and remainder
            result[pos1] += total // 10
            result[pos2] = total % 10
    
    # Convert the result array to a string, skipping leading zeros
    result_str = ""
    start_idx = 0
    while start_idx < len(result) - 1 and result[start_idx] == 0:
        start_idx += 1
    
    for i in range(start_idx, len(result)):
        result_str += str(result[i])
    
    return result_str
```

## Solution Approach #2: Separate Multiplication and Addition

This approach separately performs multiplication and addition operations.

### ASCII Art for Separate Multiplication and Addition

```
"123" × "456" calculation:

  Step 1: Calculate partial products
  ┌─────────────────────────────────┐
  │ num1="123"   ×   num2="456"     │
  ├─────────────────────────────────┤
  │       ↓            ↓            │
  ├─────────────┬─────────────┬─────┤
  │ "123" × '6' │ "123" × '5' │ "123" × '4' │
  │  = "738"    │  = "615"    │  = "492"    │
  └─────┬───────┴───────┬─────┴───────┬─────┘
        │               │             │
        │ [738]         │ [6150]      │ [49200]
        │               │             │
        └───────────────┼─────────────┘
                        │
  Step 2: Add partial products with appropriate shifting
  ┌─────────────────────┴─────────────────────┐
  │  "738" + "6150" + "49200" = "56088"       │
  └───────────────────────────────────────────┘
```

### Step-by-Step Visualization

```
Step 1: Calculate partial products

For each digit in num2, multiply with num1:
    
    6 × "123":
    6×3=18: store 8, carry 1
    6×2=12+1=13: store 3, carry 1
    6×1=6+1=7: store 7
    Result: "738"
    
    5 × "123":
    5×3=15: store 5, carry 1
    5×2=10+1=11: store 1, carry 1
    5×1=5+1=6: store 6
    Result: "615" + shift = "6150"
    
    4 × "123":
    4×3=12: store 2, carry 1
    4×2=8+1=9: store 9
    4×1=4: store 4
    Result: "492" + shift = "49200"

Step 2: Add all partial products
    "738" + "6150" + "49200" = "56088"
```

### Pseudocode

```
function multiply(num1, num2):
    if num1 = "0" or num2 = "0" return "0"
    
    # Make sure num1 is the longer number for efficiency
    if len(num1) < len(num2):
        swap(num1, num2)
    
    partial_products = []
    
    # Calculate partial products
    for i from len(num2)-1 to 0 (decrementing):
        digit2 = num2[i] - '0'
        current_product = multiply_single_digit(num1, digit2)
        
        # Add zeros based on position
        zeros_to_add = len(num2) - 1 - i
        current_product += "0" * zeros_to_add
        
        partial_products.append(current_product)
    
    # Sum all partial products
    result = "0"
    for product in partial_products:
        result = add_strings(result, product)
    
    return result

function multiply_single_digit(num, digit):
    if digit = 0 return "0"
    
    result = ""
    carry = 0
    
    for i from len(num)-1 to 0 (decrementing):
        current_digit = num[i] - '0'
        product = current_digit * digit + carry
        
        carry = product / 10
        result = (product % 10) + result
    
    if carry > 0:
        result = carry + result
    
    return result

function add_strings(num1, num2):
    result = ""
    carry = 0
    i = len(num1) - 1
    j = len(num2) - 1
    
    while i >= 0 or j >= 0 or carry > 0:
        x = 0 if i < 0 else (num1[i] - '0')
        y = 0 if j < 0 else (num2[j] - '0')
        
        sum = x + y + carry
        result = (sum % 10) + result
        carry = sum / 10
        
        i--
        j--
    
    return result
```

### Python Solution

```python
def multiply(num1: str, num2: str) -> str:
    """
    Multiplies two string numbers by calculating partial products and adding them.
    
    Args:
        num1: First number as a string without leading zeros
        num2: Second number as a string without leading zeros
        
    Returns:
        The product of num1 and num2 as a string
        
    Time Complexity: O(m*n) where m and n are lengths of input strings
    Space Complexity: O(m+n) for storing partial products
    """
    # Edge case: if either number is 0, result is 0
    if num1 == "0" or num2 == "0":
        return "0"
    
    # For better efficiency, make num1 the longer number
    if len(num1) < len(num2):
        num1, num2 = num2, num1
    
    # Function to multiply a string number by a single digit
    def multiply_by_digit(num_str, digit):
        if digit == 0:
            return "0"
        
        result = ""
        carry = 0
        
        for i in range(len(num_str) - 1, -1, -1):
            curr_digit = ord(num_str[i]) - ord('0')
            product = curr_digit * digit + carry
            
            carry = product // 10
            result = str(product % 10) + result
        
        if carry > 0:
            result = str(carry) + result
            
        return result
    
    # Function to add two string numbers
    def add_strings(num1, num2):
        result = ""
        carry = 0
        i, j = len(num1) - 1, len(num2) - 1
        
        while i >= 0 or j >= 0 or carry > 0:
            x = ord(num1[i]) - ord('0') if i >= 0 else 0
            y = ord(num2[j]) - ord('0') if j >= 0 else 0
            
            total = x + y + carry
            result = str(total % 10) + result
            carry = total // 10
            
            i -= 1
            j -= 1
            
        return result
    
    # Calculate partial products and add them
    partial_products = []
    
    for i in range(len(num2) - 1, -1, -1):
        digit = ord(num2[i]) - ord('0')
        product = multiply_by_digit(num1, digit)
        
        # Add zeros based on position
        zeros_to_add = len(num2) - 1 - i
        product += "0" * zeros_to_add
        
        partial_products.append(product)
    
    # Sum all partial products
    result = "0"
    for product in partial_products:
        result = add_strings(result, product)
    
    return result
```

## Hinglish Commentary (English letters)

```
Dosto, is question mein humein do strings (num1 aur num2) ki multiplication 
karni hai bina unko direct integers mein convert kiye. Main aapko dono approach
samjhaunga step by step tarike se.

Pehla approach bilkul school mein sikhaye gaye multiplication ki tarah hai, 
jisme hum har digit ko ek doosre se multiply karte hain aur phir appropriate 
positions par add karte hain. Is approach mein hum ek result array banate hain
jisme har position par calculation hoti hai aur carries ko handle kiya jaata hai.

Doosra approach thoda alag hai jisme hum pehle partial products calculate karte 
hain. Matlab hum num1 ko num2 ke har digit se multiply karte hain aur phir in
sab results ko appropriate positions par add kar dete hain. Yeh approach thoda 
zyada modular hai kyunki multiplication aur addition alag-alag steps mein hote
hain.

Dono approaches ki time complexity O(m*n) hai, jahan m aur n input strings ki 
lengths hain. Space complexity bhi O(m+n) hai result array ke liye.

Important baat yeh hai ki hum leading zeros ko handle karna nahi bhoolte aur 
edge cases ko sahi se handle karte hain, jaise agar koi input "0" ho to output 
bhi "0" hoga.
```

## Comparison of Both Solutions

| Aspect | Approach #1 (Grade School) | Approach #2 (Separate Mult & Add) |
|--------|----------------------------|-----------------------------------|
| Time Complexity | O(m*n) | O(m*n) |
| Space Complexity | O(m+n) | O(m+n) |
| Code Structure | Single pass algorithm | Modular with helper functions |
| Readability | More compact | More readable |
| Edge Cases | Handles leading zeros | Handles leading zeros |

## Time and Space Complexity Analysis

**Time Complexity**: Both solutions have a time complexity of O(m*n) where m and n are the lengths of the input strings. This is because we need to multiply each digit of one number with each digit of the other number.

**Space Complexity**: Both solutions have a space complexity of O(m+n) for storing the result or intermediate calculations.

## Leetcode Submission Results (Mock)

**Approach #1**:
- Runtime: 12 ms (beats 90.15%)
- Memory Usage: 14.1 MB (beats 77.32%)

**Approach #2**:
- Runtime: 16 ms (beats 83.42%)
- Memory Usage: 14.3 MB (beats 65.21%)

## Final Thoughts

The "Multiply Strings" problem is a great exercise in understanding how multiplication works at a fundamental level. Both approaches mimic hand calculations but in different ways:

1. The first approach is more efficient as it performs multiplication and carry handling in a single pass.
2. The second approach is more modular and potentially easier to debug, but involves more string operations.

For very large numbers, these string multiplication methods are crucial since they avoid integer overflow that would occur if we tried to convert the strings to integers directly.

🎯 Remember: When dealing with string calculations, always consider edge cases like zeros and carefully handle the carries!

