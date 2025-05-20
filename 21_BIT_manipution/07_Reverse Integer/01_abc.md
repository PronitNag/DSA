# Reverse Integer Solutions with ASCII Art Visualization

## Problem Statement

**Reverse Integer**
You are given a signed 32-bit integer x.
Return x after reversing each of its digits. After reversing, if x goes outside the signed 32-bit integer range [-2^31, 2^31 - 1], then return 0 instead.
Solve the problem without using integers that are outside the signed 32-bit integer range.

## ASCII Art Visualization of the Solution Process

```
            INPUT INTEGER (x)
                   ↓
      ┌─────────────────────────┐
      │         12345           │  Starting Point: Original Integer
      └─────────────────────────┘
                   ↓
                   
      ┌─────────────────────────┐
      │ DIGIT EXTRACTION PROCESS│  Extraction Process
      └─────────────────────────┘
                   ↓
                   
 ┌───────┐     ┌───────┐     ┌───────┐     ┌───────┐     ┌───────┐
 │   5   │◄────│   4   │◄────│   3   │◄────│   2   │◄────│   1   │  Digits in reverse order
 └───────┘     └───────┘     └───────┘     └───────┘     └───────┘
     ↓             ↓             ↓             ↓             ↓
 ┌───────┐     ┌───────┐     ┌───────┐     ┌───────┐     ┌───────┐
 │5*10⁰  │     │4*10¹  │     │3*10²  │     │2*10³  │     │1*10⁴  │  Position value calculation
 └───────┘     └───────┘     └───────┘     └───────┘     └───────┘
     ↓             ↓             ↓             ↓             ↓
     5    +       40    +      300    +     2000    +    10000           Addition
                   ↓
      ┌─────────────────────────┐
      │         54321           │  Result: Reversed Integer
      └─────────────────────────┘
                   ↓
         CHECK 32-BIT RANGE
    [-2147483648, 2147483647]
                   ↓
      ┌─────────────────────────┐
      │ Return reversed number  │  Final Check and Return
      │ or 0 if out of range    │
      └─────────────────────────┘
```

## Three Solution Approaches

### 1. Brute Force Solution

#### Pseudocode:
```
Function reverseInteger(x):
    1. Convert the integer to a string
    2. Reverse the string
    3. Convert back to integer with the original sign
    4. Check if result is within 32-bit integer range
    5. Return result if within range, otherwise return 0
```

#### Python Implementation:

```python
def reverse_integer_brute_force(x):
    """
    Reverses the digits of a 32-bit signed integer using string conversion approach.
    
    This function takes a signed integer x and returns its digit-reversed value. If
    the result exceeds the 32-bit signed integer range, it returns 0 instead as per
    the problem requirements.
    
    Parameters:
        x (int): The input integer to be reversed
        
    Returns:
        int: The digit-reversed integer, or 0 if out of 32-bit range
        
    Example:
        >>> reverse_integer_brute_force(123)
        321
        >>> reverse_integer_brute_force(-123)
        -321
    """
    # Handle edge case of zero separately
    if x == 0:
        return 0
    
    # Determine if the number is negative and store the sign
    is_negative = x < 0
    
    # Convert to positive number for easier handling
    x_absolute = abs(x)
    
    # Convert to string, reverse it, and convert back to integer
    reversed_str = str(x_absolute)[::-1]
    result = int(reversed_str)
    
    # Reapply the original sign
    if is_negative:
        result = -result
    
    # Check if the result is within the 32-bit signed integer range
    if result < -2**31 or result > 2**31 - 1:
        return 0
    
    return result
```

### 2. Recursive Solution

#### Pseudocode:
```
Function reverseIntegerRecursive(x, accumulated_result):
    1. Base case: If x is 0, return the accumulated result
    2. Extract the last digit of x: digit = x % 10
    3. Add digit to accumulated result: accumulated_result = accumulated_result * 10 + digit
    4. Check if result within 32-bit range, return 0 if not
    5. Recursively call with remaining digits: x // 10
```

#### Python Implementation:

```python
def reverse_integer_recursive(x):
    """
    Reverses the digits of a 32-bit signed integer using recursive approach.
    
    This function implements a recursive solution that extracts digits one by one
    and builds the reversed number. If the result exceeds the 32-bit signed integer
    range, it returns 0 as specified in the problem statement.
    
    Parameters:
        x (int): The input integer to be reversed
        
    Returns:
        int: The digit-reversed integer, or 0 if out of 32-bit range
        
    Example:
        >>> reverse_integer_recursive(123)
        321
        >>> reverse_integer_recursive(-123)
        -321
    """
    # Define helper function for recursive implementation
    def reverse_helper(remaining, accumulated_result):
        # Base case: no more digits to process
        if remaining == 0:
            return accumulated_result
        
        # Extract the last digit
        last_digit = remaining % 10
        
        # Add the digit to our accumulated result
        next_result = accumulated_result * 10 + last_digit
        
        # Check if we're still within 32-bit integer range
        if next_result < -2**31 or next_result > 2**31 - 1:
            return 0
        
        # Recursive call with the remaining digits
        return reverse_helper(remaining // 10, next_result)
    
    # Handle edge case of zero separately
    if x == 0:
        return 0
    
    # Handle negative numbers
    if x < 0:
        return -reverse_helper(-x, 0)
    else:
        return reverse_helper(x, 0)
```

### 3. Iterative Solution

#### Pseudocode:
```
Function reverseIntegerIterative(x):
    1. Initialize result = 0
    2. While x is not 0:
       a. Extract last digit: digit = x % 10
       b. Add digit to result: result = result * 10 + digit
       c. Check if result within 32-bit range, return 0 if not
       d. Remove last digit from x: x = x // 10
    3. Return result
```

#### Python Implementation:

```python
def reverse_integer_iterative(x):
    """
    Reverses the digits of a 32-bit signed integer using iterative approach.
    
    This function implements an iterative solution that extracts the last digit
    in each iteration and builds the reversed number. It handles negative numbers
    and checks if the result is within the 32-bit integer range.
    
    Parameters:
        x (int): The input integer to be reversed
        
    Returns:
        int: The digit-reversed integer, or 0 if out of 32-bit range
        
    Example:
        >>> reverse_integer_iterative(123)
        321
        >>> reverse_integer_iterative(-123)
        -321
    """
    # Store the sign of the input number
    sign = -1 if x < 0 else 1
    
    # Work with positive number for simplicity
    x = abs(x)
    
    # Initialize the result
    result = 0
    
    # Process each digit
    while x > 0:
        # Extract the last digit
        last_digit = x % 10
        
        # Before adding the digit, check if this might cause overflow
        if result > (2**31 - 1) // 10:
            return 0
        
        # Update the result by shifting left and adding the digit
        result = result * 10 + last_digit
        
        # Remove the last digit from original number
        x //= 10
    
    # Apply the original sign
    final_result = sign * result
    
    # Final check for 32-bit integer range
    if final_result < -2**31 or final_result > 2**31 - 1:
        return 0
    
    return final_result
```

## Emoji Art Representation of the Algorithm

```
🔢 1️⃣2️⃣3️⃣4️⃣5️⃣  ➡️  🔄  ➡️  5️⃣4️⃣3️⃣2️⃣1️⃣ 🔢
    ⬇️                              ⬇️
    📊 Check if within range: [-2³¹, 2³¹-1] 📊
    ⬇️                              ⬇️
    ✅ Return result if valid       ❌ Return 0 if invalid

🧠 Three approaches to solve:
🔨 Brute Force: Convert to string, reverse, convert back
🔄 Recursion: Extract digits and build result recursively
🔁 Iteration: Process each digit in a loop, building result
```

## Hindi-English (Hinglish) Summary

```
Is problem mein humko ek integer ko reverse karna hai. Jaise ki 12345 ko 54321 banana
hai. Lekin dhyan rahe ki agar reversed number 32-bit integer range se bahar jata hai, 
to humko 0 return karna hai.

Humne teen different tarike se solution provide kiya hai:

1. Brute Force approach mein, hum number ko string mein convert karte hain, phir reverse
   karte hain, aur phir se integer mein convert karte hain.

2. Recursive approach mein, hum har digit ko extract karke ek naya number build
   karte hain recursively.

3. Iterative approach mein, hum loop chalate hain jab tak saare digits process
   na ho jayen, aur har iteration mein result update karte hain.

Teeno approaches mein, hum check karte hain ki kya final result 32-bit range
ke andar hai ya nahi.
```

## Time and Space Complexity

### Brute Force Solution:
- **Time Complexity**: O(log(x)) - Converting to string and back takes time proportional to the number of digits
- **Space Complexity**: O(log(x)) - For storing the string representation

### Recursive Solution:
- **Time Complexity**: O(log(x)) - We process each digit once
- **Space Complexity**: O(log(x)) - Due to the recursion stack depth

### Iterative Solution:
- **Time Complexity**: O(log(x)) - We process each digit once
- **Space Complexity**: O(1) - We only use a constant amount of extra space

## Example Test Cases

```
Input: x = 123
Output: 321

Input: x = -123
Output: -321

Input: x = 120
Output: 21

Input: x = 1534236469 (Out of 32-bit range when reversed)
Output: 0
```
