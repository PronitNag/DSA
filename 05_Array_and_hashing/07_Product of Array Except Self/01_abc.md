# Product of Array Except Self - Visual Solution

## Problem Statement

Given an integer array `nums`, return an array `output` where `output[i]` is the product of all elements of `nums` except `nums[i]`.

*Each product is guaranteed to fit in a 32-bit integer.*

*Follow-up: Solve it in O(n) time without using division.*

## Visual Explanation with ASCII Art

Let's visualize the solution with a sample array: `[1, 2, 3, 4]`

### Initial Understanding

```
Input Array: [1, 2, 3, 4]
                ↓  ↓  ↓  ↓
Expected Output: [24, 12, 8, 6]  (Output[i] = product of all except nums[i])
```

### The Key Insight: Prefix and Postfix Products

We'll use two passes through the array:
1. First pass (left to right): Calculate prefix products
2. Second pass (right to left): Multiply with postfix products

### First Pass: Computing Prefix Products

```
Array:    [1,    2,    3,    4]
           ↓     ↓     ↓     ↓
           ┌─────┬─────┬─────┬─────┐
           │     │     │     │     │
Prefix:    │  1  │  1  │  2  │  6  │  ← Prefix[i] = Product of all elements 
           │     │     │     │     │    before index i
           └─────┴─────┴─────┴─────┘
```

### Second Pass: Multiplying with Postfix Products

```
           ┌─────┬─────┬─────┬─────┐
           │     │     │     │     │
Prefix:    │  1  │  1  │  2  │  6  │
           │     │     │     │     │
           └─────┴─────┴─────┴─────┘
             ↓     ↓     ↓     ↓
           ┌─────┬─────┬─────┬─────┐
           │     │     │     │     │
Postfix:   │ 24  │ 12  │  4  │  1  │  ← Postfix values calculated on-the-fly
           │     │     │     │     │
           └─────┴─────┴─────┴─────┘
             ↓     ↓     ↓     ↓
           ┌─────┬─────┬─────┬─────┐
           │     │     │     │     │
Result:    │ 24  │ 12  │  8  │  6  │  ← Result[i] = Prefix[i] * Postfix[i]
           │     │     │     │     │
           └─────┴─────┴─────┴─────┘
```

### Step-by-Step Animation

```
Starting with empty result array:
[1, 1, 1, 1]  (Initialize with 1's)

After prefix calculations (left to right):
[1, 1, 2, 6]

Postfix = 1 (Initial value)

After index 3 (right to left):
[1, 1, 2, 6] (no change) → res[3] = 6 * 1 = 6
Postfix = 1 * 4 = 4

After index 2:
[1, 1, 8, 6] → res[2] = 2 * 4 = 8
Postfix = 4 * 3 = 12

After index 1:
[1, 12, 8, 6] → res[1] = 1 * 12 = 12
Postfix = 12 * 2 = 24

After index 0:
[24, 12, 8, 6] → res[0] = 1 * 24 = 24

Final result: [24, 12, 8, 6]  ✓
```

## Time and Space Complexity Analysis

```
Time Complexity: O(n) - We do two passes through the array (n + n = 2n → O(n))
Space Complexity: O(1) - Not counting the output array (required by problem)
```

## Solution Code

### Pseudocode
```
function productExceptSelf(nums[]):
    n = length of nums
    result = array of n 1's
    
    # Calculate prefix products
    for i from 1 to n-1:
        result[i] = result[i-1] * nums[i-1]
    
    # Calculate postfix products on the fly
    postfix = 1
    for i from n-1 down to 0:
        result[i] = result[i] * postfix
        postfix = postfix * nums[i]
    
    return result
```

### Python Solution with Comments

```python
class Solution:
    def productExceptSelf(self, nums: list[int]) -> list[int]:
        """
        Calculate product of all elements except self without using division.
        
        Args:
            nums: Input array of integers
            
        Returns:
            Array where each element is product of all nums except current element
            
        Time Complexity: O(n) where n is the length of the input array
        Space Complexity: O(1) excluding the output array
        """
        n = len(nums)
        result = [1] * n  # Initialize result array with 1's
        
        # First pass: Calculate prefix products (left to right)
        # result[i] will contain product of all elements to the left of i
        for i in range(1, n):
            result[i] = result[i-1] * nums[i-1]
        
        # Second pass: Multiply with postfix products (right to left)
        # Use a variable to keep track of the running product from right side
        postfix = 1
        for i in range(n-1, -1, -1):
            result[i] *= postfix  # Multiply prefix result with postfix product
            postfix *= nums[i]    # Update postfix for next iteration
            
        return result
```

### Solution in Hinglish with Comments

```python
class Solution:
    def productExceptSelf(self, nums: list[int]) -> list[int]:
        """
        Division ka use kiye bina, har element ke liye baaki sab elements ka
        product calculate karta hai.
        
        Args:
            nums: Input array of integers
            
        Returns:
            Array jisme har element ke liye baaki sab ka product hai
            
        Time Complexity: O(n) jahan n input array ki length hai
        Space Complexity: O(1) output array ko chhod kar
        """
        n = len(nums)
        result = [1] * n  # Result array ko 1's se initialize karo
        
        # Pehla pass: Prefix products calculate karo (left to right)
        # result[i] mein i ke left side ke sare elements ka product hoga
        for i in range(1, n):
            result[i] = result[i-1] * nums[i-1]
        
        # Doosra pass: Postfix products ko multiply karo (right to left)
        # Ek variable mein right side se running product ko track karo
        postfix = 1
        for i in range(n-1, -1, -1):
            result[i] *= postfix  # Prefix result ko postfix product se multiply karo
            postfix *= nums[i]    # Agli iteration ke liye postfix ko update karo
            
        return result
```

## Alternative Approaches

### Using Division (Not Meeting Follow-up Requirement)

```python
def productExceptSelfWithDivision(nums: list[int]) -> list[int]:
    """
    Simple solution using division (doesn't meet follow-up requirement).
    Works only when there are no zeros in the array.
    
    Time Complexity: O(n)
    Space Complexity: O(1)
    """
    product = 1
    for num in nums:
        product *= num
        
    return [product // num for num in nums]  # Division approach (not allowed)
    
# Note: This approach fails if there are zeros in the array!
```

### Handling Zeros in Division Approach

```python
def productExceptSelfHandlingZeros(nums: list[int]) -> list[int]:
    """
    Division approach that handles zeros, but still doesn't meet follow-up.
    
    Time Complexity: O(n)
    Space Complexity: O(1)
    """
    zero_count = nums.count(0)
    
    if zero_count > 1:
        return [0] * len(nums)  # More than one zero means all results are zero
        
    if zero_count == 1:
        product = 1
        for num in nums:
            if num != 0:
                product *= num
                
        return [product if num == 0 else 0 for num in nums]
        
    # No zeros case
    product = 1
    for num in nums:
        product *= num
        
    return [product // num for num in nums]
```

## Conclusion

The optimal O(n) solution without division uses prefix and postfix products in two passes through the array. This approach is elegant and handles all cases correctly, including arrays with zeros.

