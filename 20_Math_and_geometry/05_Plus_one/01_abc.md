# Plus One LeetCode Solution with ASCII Art Visualization

## Problem Statement

> You are given an integer array digits, where each digits[i] is the ith digit 
> of a large integer. It is ordered from most significant to least significant 
> digit, and it will not contain any leading zero.
>
> Return the digits of the given integer after incrementing it by one.
>
> You must do it in place.

## Visualization of the Problem

```ascii
Input Array:  [1, 2, 9]
                     ↑
              Add 1 here
                     ↓
Output Array: [1, 3, 0]  (129 + 1 = 130)

Special Case:
Input Array:  [9, 9, 9]
                     ↑
              Add 1 here
                     ↓
Output Array: [1, 0, 0, 0]  (999 + 1 = 1000)
```

## The Journey of Plus One

```ascii
                     +-------------------+
                     |  Start: digits[]  |  (jaise [1,2,9] ya [9,9,9])
                     +-------------------+
                               |
                               v
          +---------------------------------------------+
          | Hume last digit se start karna hai (index n-1) |
          +---------------------------------------------+
                               |
                               v
       +-----------------------------------------------------+
       |                  Kya karna hai?                     |
       | Har digit ko plus 1 karna hai lekin dhyan se:       |
       | Agar digit 9 hai, to 0 banana hai aur carry forward |
       | Agar digit 9 nahi hai, bas +1 karna hai aur khatam  |
       +-----------------------------------------------------+
                               |
              +---------------------------------+
              |                                 |
              v                                 v
 +-------------------------+      +---------------------------+
 | Recursion Approach      |      | Iteration Approaches      |
 | (end se start tak jao)  |      | (end se start tak loop)   |
 +-------------------------+      +---------------------------+
              |                                 |
              v                                 v
+---------------------------+    +------------------------------+
| Agar digit 9 hai:         |    | Iteration-I: simple loop     |
| - 0 set karo              |    | Iteration-II: pythonic trick |
| - Next digit par recursion |    +------------------------------+
| Agar 9 nahi:              |
| - digit+1 karo aur return |
+---------------------------+
              |
              v
   +----------------------+
   | Final Check:         |
   | Agar sab digits 9    |
   | the to carry forward |
   | karke [1] + [0,...0] |
   +----------------------+
              |
              v
     +------------------+
     | Return: digits[] |  (jaise [1,3,0] ya [1,0,0,0])
     +------------------+
```

## Solution Approaches

### 1. Recursion Solution

```python
def plusOne(digits):
    """
    Function jo recursion ka use karke digits array mein +1 karta hai.
    
    Args:
        digits (List[int]): Array of integers representing a large number
                           (jaise [1,2,9] ya [9,9,9])
    
    Returns:
        List[int]: Incremented array (jaise [1,3,0] ya [1,0,0,0])
    """
    
    def add_one_recursive(index):
        # Base case: agar index invalid hai to hum naya digit [1] add karenge
        if index < 0:
            digits.insert(0, 1)
            return
        
        # Agar current digit 9 hai
        if digits[index] == 9:
            digits[index] = 0   # 9 ko 0 bana do
            add_one_recursive(index - 1)  # Aur pichle digit par +1 karne jao
        else:
            digits[index] += 1  # Agar 9 nahi hai, to bas +1 karo aur khatam
    
    # Recursion ko last digit se start karo
    add_one_recursive(len(digits) - 1)
    return digits
```

### 2. Iteration Solution - I (Classic Loop)

```python
def plusOne(digits):
    """
    Function jo simple iteration ka use karke digits array mein +1 karta hai.
    
    Args:
        digits (List[int]): Array of integers representing a large number
                           (jaise [1,2,9] ya [9,9,9])
    
    Returns:
        List[int]: Incremented array (jaise [1,3,0] ya [1,0,0,0])
    """
    
    n = len(digits)
    
    # Last digit se start karke peeche ki taraf jaate hain
    for i in range(n-1, -1, -1):
        # Agar digit 9 nahi hai, to sirf +1 karo aur return
        if digits[i] < 9:
            digits[i] += 1
            return digits
        
        # Agar digit 9 hai, to use 0 bana do aur loop continue karo
        digits[i] = 0
    
    # Agar code yahan tak pahunch gaya hai, matlab saare digits 9 the
    # To hume array ke shuru mein 1 add karna padega (jaise [9,9,9] -> [1,0,0,0])
    digits.insert(0, 1)
    return digits
```

### 3. Iteration Solution - II (Pythonic Approach)

```python
def plusOne(digits):
    """
    Function jo pythonic tarike se digits array mein +1 karta hai.
    
    Args:
        digits (List[int]): Array of integers representing a large number
                           (jaise [1,2,9] ya [9,9,9])
    
    Returns:
        List[int]: Incremented array (jaise [1,3,0] ya [1,0,0,0])
    """
    
    # Digits ko integer mein convert karo, +1 karo, fir wapas digits mein convert
    num = 0
    for digit in digits:
        num = num * 10 + digit
    
    num += 1  # +1 karo
    
    # Number ko wapas array mein convert karo
    result = []
    while num > 0:
        result.insert(0, num % 10)  # Right-most digit ko extract karo
        num //= 10  # Number ko divide karo
    
    # Edge case: agar input [0] hai to result empty ho jayega, isliye check karo
    return result if result else [1]
```

## Time and Space Complexity Comparison

### ASCII Art Complexity Chart

```ascii
+------------------+----------------------+--------------------+
| Solution Type    | Time Complexity      | Space Complexity   |
+------------------+----------------------+--------------------+
| Recursion        | O(n)                 | O(n) [call stack]  |
|                  | ┌─────┐              | ┌─────┐            |
|                  | │     │              | │     │            |
|                  | │     v              | │     v            |
|                  | └──────────────────→ | └───────────────→  |
+------------------+----------------------+--------------------+
| Iteration - I    | O(n)                 | O(1)               |
|                  | ┌─────┐              | ┌─────┐            |
|                  | │     │              | │     │            |
|                  | │     v              | │     v            |
|                  | └──────────────────→ | └→                 |
+------------------+----------------------+--------------------+
| Iteration - II   | O(n)                 | O(n)               |
|                  | ┌─────┐              | ┌─────┐            |
|                  | │     │              | │     │            |
|                  | │     v              | │     v            |
|                  | └──────────────────→ | └───────────────→  |
+------------------+----------------------+--------------------+
```

## Example Evolution of the Solution (ASCII Art)

Let's visualize how our solution would handle the input `[1, 9, 9]`:

### Example Evolution for Recursive Approach:

```ascii
Step 1: Start with [1, 9, 9]
              0  1  2
             [1, 9, 9]
                    ^
                    |
                 add_one_recursive(2)
                    |
                    v
              0  1  2
             [1, 9, 0]  # 9 becomes 0, recursive call to index 1
                 ^
                 |
              add_one_recursive(1)
                 |
                 v
              0  1  2
             [1, 0, 0]  # 9 becomes 0, recursive call to index 0
              ^
              |
           add_one_recursive(0)
              |
              v
              0  1  2
             [2, 0, 0]  # 1+1=2, we're done!
```

### Example Evolution for Iterative Approach I:

```ascii
Step 1: Start with [1, 9, 9]
              0  1  2
             [1, 9, 9]
                    ^
                    |
                 i = 2, digits[2] = 9
                    |
                    v
              0  1  2
             [1, 9, 0]  # 9 becomes 0, continue loop
                 ^
                 |
              i = 1, digits[1] = 9
                 |
                 v
              0  1  2
             [1, 0, 0]  # 9 becomes 0, continue loop
              ^
              |
           i = 0, digits[0] = 1
              |
              v
              0  1  2
             [2, 0, 0]  # 1+1=2, return result
```

### Example Evolution for Iterative Approach II:

```ascii
Step 1: Start with [1, 9, 9]
        Convert to integer: 1*100 + 9*10 + 9*1 = 199
        
Step 2: Add 1
        199 + 1 = 200
        
Step 3: Convert back to array
        200 % 10 = 0, push to front: [0]
        200 // 10 = 20
        20 % 10 = 0, push to front: [0,0]
        20 // 10 = 2
        2 % 10 = 2, push to front: [2,0,0]
        2 // 10 = 0, end loop
        
Final result: [2, 0, 0]
```

## Edge Cases Test

Special case handling when all digits are 9:

```ascii
Input: [9, 9, 9]
                                  
+-------+      +--------+      +---------+      +---------+
| [9,9,9] ---> | [9,9,0] ---> | [9,0,0] ---> | [0,0,0] |
+-------+      +--------+      +---------+      +---------+
     |                                              |
     |                                              |
     +---------------------------------------------+
                         |
                         v
                    +----------+
                    | [1,0,0,0] |  Final Answer!
                    +----------+
```

## Conclusion

Iteration-I is generally the most efficient solution because:
1. It has O(n) time complexity like others but O(1) space complexity
2. No extra conversion steps like Iteration-II
3. No recursive stack overhead like Recursion approach

Happy coding! 😊
