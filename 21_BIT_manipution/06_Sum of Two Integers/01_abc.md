# Sum of Two Integers Without Using + and - Operators

## Problem Statement
Given two integers a and b, return the sum of the two integers without using the + and - operators.

## ASCII Art Solution Journey

```
       Input: a, b
          |
          v
+-------------------+
| Kya karna hai?    |    Translation: What needs to be done?
| a + b without +/- |    We need to calculate a + b without using + and -
+-------------------+
          |
          v
+-------------------+          +-------------------+          +-------------------+
| Brute Force       | -------> | Bit Manipulation  | -------> | Bit Manipulation  |
| (Basic Approach)  |          | (Standard)        |          | (Optimal)         |
+-------------------+          +-------------------+          +-------------------+
          |                             |                             |
          v                             v                             v
    +-----------+                +-----------+                +-----------+
    | Solution 1 |                | Solution 2 |                | Solution 3 |
    +-----------+                +-----------+                +-----------+
```

## 1. Brute Force Approach

### ASCII Art Visualization

```
           a = 5, b = 3
           
 +---+---+---+---+---+      +---+---+---+      
 | 1 | 2 | 3 | 4 | 5 |      | 1 | 2 | 3 |      
 +---+---+---+---+---+      +---+---+---+      
                                               
 a = 5                       b = 3             
                                               
 Step 1: Iterate b times     Step 2: Return a  
 and add 1 to a each time    a = 5+3 = 8       
                                               
 Loop 1: a = 6                                 
 Loop 2: a = 7                                 
 Loop 3: a = 8                                 
                                               
 Result: 8                                     
```

### Pseudo Code (Brute Force)
```
function getSum(a, b):
    if b > 0:
        for i from 1 to b:
            a = a + 1  // Ye nahi kar sakte (can't use +)
    else if b < 0:
        for i from 1 to -b:
            a = a - 1  // Ye bhi nahi kar sakte (can't use -)
    return a
```

### Python Code (Brute Force)
```python
def getSum_brute_force(a, b):
    """
    Hum brute force approach mein a aur b ka sum nikaalne ke liye loops use
    karenge, lekin kyunki + aur - operators allowed nahi hain, hum increment
    aur decrement ke liye alternative methods use karenge.
    
    Args:
        a (int): Pehla integer number
        b (int): Doosra integer number
    
    Returns:
        int: a aur b ka sum
    """
    # Practically ye solution theoretical hai kyunki Python mein hum loop
    # ke liye increment bhi + operator use karte hain
    # Lekin conceptually, ham b times loop chalate aur a ko increment karte
    
    # NOTE: Ye sirf samajhne ke liye hai, actual mein hum + operator use kar rahe
    # hain jo problem ke rules ke against hai
    
    if b > 0:
        for _ in range(b):
            a += 1  # Not allowed as per problem constraints
    elif b < 0:
        for _ in range(-b):
            a -= 1  # Not allowed as per problem constraints
    
    return a
```

## 2. Bit Manipulation (Standard Approach)

### ASCII Art Visualization

```
  a = 5 (101₂), b = 3 (011₂)
  
  Step 1: Calculate XOR (a ^ b) = 110₂      Step 2: Calculate Carry (a & b) << 1 = 010₂
  
  a = 1 0 1                                 a = 1 0 1
  b = 0 1 1                                 b = 0 1 1
      -----                                     -----
  XOR = 1 1 0                               AND = 0 0 1
                                          Carry = 0 1 0 (after left shift)
  
  Step 3: If carry = 0, return XOR          Step 4: Otherwise, repeat with XOR as a and carry as b
  
  XOR = 1 1 0                               a = 1 1 0
  Carry = 0 1 0                             b = 0 1 0
  Carry != 0, so continue                    
                                            And so on until carry becomes 0...
  
  Final result: 8 (1000₂)
```

### Pseudo Code (Bit Manipulation - Standard)
```
function getSum(a, b):
    while b != 0:
        carry = a & b        // Find common set bits (carriers)
        a = a ^ b            // XOR gives sum without carry
        b = carry << 1       // Shift carry by 1 as it'll be added to next position
    return a
```

### Python Code (Bit Manipulation - Standard)
```python
def getSum_bit_manipulation(a, b):
    """
    Hum bit manipulation approach mein XOR aur AND operators ke saath work
    karenge. XOR humein bina carry ke addition deta hai, aur AND with left
    shift humein carry provide karta hai.
    
    Args:
        a (int): Pehla integer number
        b (int): Doosra integer number
    
    Returns:
        int: a aur b ka sum
    """
    # Python mein integer ki range unlimited hai, isliye hum 32-bit constraint
    # ko handle karna padega
    mask = 0xFFFFFFFF  # 32 bit mask
    
    # Loop tab tak chalega jab tak b (carry) zero nahi ho jata
    while b != 0:
        # Carry nikalne ke liye AND operation
        carry = (a & b) & mask
        
        # Sum without carry nikalna (XOR operation)
        a = (a ^ b) & mask
        
        # Carry ko left shift karna kyunki ye next position par add hoga
        b = (carry << 1) & mask
    
    # Handle negative numbers in 32-bit representation
    if a > 0x7FFFFFFF:
        a = ~(a ^ mask)
    
    return a
```

## 3. Bit Manipulation (Optimal Approach)

### ASCII Art Visualization

```
  a = 5 (101₂), b = 3 (011₂)
  
  Step 1: Calculate XOR (a ^ b) = 110₂      Step 2: Calculate Carry (a & b) << 1 = 010₂
  
  🔢 a = 1 0 1                              🔢 a = 1 0 1
  🔢 b = 0 1 1                              🔢 b = 0 1 1
      -----                                     -----
  🔀 XOR = 1 1 0                            🔄 AND = 0 0 1
                                          🚚 Carry = 0 1 0 (after left shift)
  
  Step 3: Recursive call with a = XOR, b = carry
  
  🔁 getSum(110₂, 010₂)
  
  🔢 a = 1 1 0
  🔢 b = 0 1 0
      -----
  🔀 XOR = 1 0 0
  🔄 AND = 0 1 0
  🚚 Carry = 1 0 0 (after left shift)
  
  🔁 getSum(100₂, 100₂)
  
  🔢 a = 1 0 0
  🔢 b = 1 0 0
      -----
  🔀 XOR = 0 0 0
  🔄 AND = 1 0 0
  🚚 Carry = 1 0 0 0 (after left shift)
  
  🔁 getSum(000₂, 1000₂)
  
  Result: 8 (1000₂)
```

### Pseudo Code (Bit Manipulation - Optimal/Recursive)
```
function getSum(a, b):
    if b == 0:
        return a
    else:
        return getSum(a ^ b, (a & b) << 1)
```

### Python Code (Bit Manipulation - Optimal/Recursive)
```python
def getSum_optimal(a, b):
    """
    Hum recursive bit manipulation approach use karenge jahan XOR operation
    addition without carrying aur AND operation with left shift carry provide
    karega.
    
    Args:
        a (int): Pehla integer number
        b (int): Doosra integer number
    
    Returns:
        int: a aur b ka sum
    """
    # Base case: agar b (carry) zero ho gaya to a hi final sum hai
    if b == 0:
        return a
    
    # Recursive case: XOR gives sum without carry, AND with shift gives carry
    # Agar Python mein recursion limit ki problem ho, to iterative solution use karo
    
    # Handle negative numbers for 32-bit integers
    mask = 0xFFFFFFFF
    
    # Sum without carry (XOR)
    sum_without_carry = (a ^ b) & mask
    
    # Calculate carry
    carry = ((a & b) << 1) & mask
    
    # Recursive call
    return getSum_optimal(sum_without_carry, carry)
```

## All Solutions Together

Let's compare all three solutions:

```
+--------------------------------------------------------+
|               |  Brute Force  |  Bit Manipulation  |  Optimal  |
+--------------------------------------------------------+
| Time          |    O(|b|)     |      O(log n)      |  O(log n) |
| Complexity    |               |                    |           |
+--------------------------------------------------------+
| Space         |     O(1)      |       O(1)         |   O(log n) |
| Complexity    |               |                    | (recursion)|
+--------------------------------------------------------+
| Key           | Conceptual    | Iterative bit      | Recursive |
| Approach      | approach only | manipulation       | bit       |
|               | Cannot use    | using XOR & AND    | manipulatio|
|               | +/- directly  | with shifting      | n          |
+--------------------------------------------------------+
```

## Final Solution Implementation

```python
def getSum(a, b):
    """
    Calculate the sum of two integers without using + or - operators.
    
    Args:
        a (int): Pehla integer number
        b (int): Doosra integer number
    
    Returns:
        int: a aur b ka sum
    """
    # 32-bit mask for handling integer overflow
    mask = 0xFFFFFFFF
    
    # Handle negatives
    while b & mask:
        carry = (a & b) & mask
        a = (a ^ b) & mask
        b = (carry << 1) & mask
    
    # If negative result in 32-bit representation
    return a if a <= 0x7FFFFFFF else ~(a ^ mask)
```

