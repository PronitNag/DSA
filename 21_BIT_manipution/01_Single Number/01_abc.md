# Single Number Problem Solutions

## Problem Statement

Given a non-empty array of integers nums, every element appears twice except for one. Find that single one.

You must implement a solution with a linear runtime complexity and use only constant extra space.

## Visual Representation Using ASCII Art

### Initial Problem Visualization
```
Input Array:
┌─────┬─────┬─────┬─────┬─────┬─────┬─────┐
│  4  │  1  │  2  │  1  │  2  │  4  │  7  │
└─────┴─────┴─────┴─────┴─────┴─────┴─────┘
                        Find the single element (7)
```

### Solution Evolution

#### 1. Brute Force Approach
```
┌─────┐     ┌─────┐     ┌─────┐     ┌─────┐     ┌─────┐     ┌─────┐     ┌─────┐
│  4  │     │  1  │     │  2  │     │  1  │     │  2  │     │  4  │     │  7  │
└──┬──┘     └──┬──┘     └──┬──┘     └──┬──┘     └──┬──┘     └──┬──┘     └──┬──┘
   │           │           │           │           │           │           │
   ▼           ▼           ▼           ▼           ▼           ▼           ▼
┌──────┐    ┌──────┐    ┌──────┐    ┌──────┐    ┌──────┐    ┌──────┐    ┌──────┐
│Count:│    │Count:│    │Count:│    │Count:│    │Count:│    │Count:│    │Count:│
│  2   │    │  2   │    │  2   │    │  2   │    │  2   │    │  2   │    │  1   │ ← Found!
└──────┘    └──────┘    └──────┘    └──────┘    └──────┘    └──────┘    └──────┘
```

#### 2. Hash Set Approach
```
┌──────────────────┐
│  Temporary Set   │
└────────┬─────────┘
         │
         ▼
┌─────┬─────┬─────┬─────┬─────┬─────┬─────┐  Input Array
│  4  │  1  │  2  │  1  │  2  │  4  │  7  │
└─────┴─────┴─────┴─────┴─────┴─────┴─────┘
  │       │     │     │     │     │     │
  ▼       ▼     ▼     ▼     ▼     ▼     ▼
┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐
│Add 4│ │Add 1│ │Add 2│ │Rem 1│ │Rem 2│ │Rem 4│ → Set contains 7
└─────┘ └─────┘ └─────┘ └─────┘ └─────┘ └─────┘
```

#### 3. Sorting Approach
```
Original:  [4, 1, 2, 1, 2, 4, 7]
           ↓    ↓    ↓
Sorted:    [1, 1, 2, 2, 4, 4, 7]
            ─┘    ─┘    ─┘    ↑
           Pairs      Single Element
```

#### 4. Bit Manipulation (XOR) Approach
```
Array:     [  4  ,   1  ,   2  ,   1  ,   2  ,   4  ,   7  ]
Binary:    [ 100 ,  001 ,  010 ,  001 ,  010 ,  100 ,  111 ]
                ↓      ↓      ↓      ↓      ↓      ↓      ↓
XOR Process:
100 ⊕ 001 = 101
      101 ⊕ 010 = 111
            111 ⊕ 001 = 110
                  110 ⊕ 010 = 100
                        100 ⊕ 100 = 000
                              000 ⊕ 111 = 111 (Binary for 7)
                                           ↓
                                      Answer = 7
```

## Solutions in Python

### 1. Brute Force Approach

```python
def singleNumber_bruteForce(nums):
    """
    Brute force solution jismein hum ek-ek element ko array se compare karte hain.
    Time Complexity: O(n²) - Har element ke liye puri array check karte hain
    Space Complexity: O(1) - Humne koi additional data structure use nahi kiya
    
    :param nums: Array of integers where all elements appear twice except one
    :return: The element that appears only once
    """
    for i in range(len(nums)):
        count = 0
        for j in range(len(nums)):
            if nums[i] == nums[j]:
                count += 1
        
        # Agar koi element exact ek baar aaya hai, to wahi hamara answer hai
        if count == 1:
            return nums[i]
    
    return -1  # Agar koi element nahi mila (this should not happen per problem)
```

### 2. Hash Set Approach

```python
def singleNumber_hashSet(nums):
    """
    Hash Set approach jismein hum element ko set mein add aur remove karte hain.
    Time Complexity: O(n) - Har element ke liye constant time operations
    Space Complexity: O(n) - Worst case mein n/2+1 elements store honge
    
    :param nums: Array of integers where all elements appear twice except one
    :return: The element that appears only once
    """
    unique_elements = set()
    
    for num in nums:
        # Agar number set mein pehle se hai to use remove karo, warna add karo
        if num in unique_elements:
            unique_elements.remove(num)
        else:
            unique_elements.add(num)
    
    # Ant mein, set mein sirf ek hi element bachega - wohi single number hai
    return unique_elements.pop()  # Return the only remaining element
```

### 3. Sorting Approach

```python
def singleNumber_sorting(nums):
    """
    Sorting approach jismein elements ko sort karke adjacent pairs check karte hain.
    Time Complexity: O(n log n) - Sorting ke liye
    Space Complexity: O(1) or O(n) - Sorting algorithm ke according vary karega
    
    :param nums: Array of integers where all elements appear twice except one
    :return: The element that appears only once
    """
    # Pehle array ko sort karo - complexity O(n log n)
    nums.sort()
    
    # Ab pairs mein check karo - har dusre index se shuru karo
    i = 0
    while i < len(nums) - 1:
        # Agar adjacent elements same nahi hain, to current element single hai
        if nums[i] != nums[i + 1]:
            return nums[i]
        
        # Pair mil gaya, next pair check karne ke liye 2 positions aage jao
        i += 2
    
    # Agar last element single hai (ye tab hoga jab array ka size odd ho)
    return nums[-1]
```

### 4. Bit Manipulation (XOR) Approach

```python
def singleNumber_xor(nums):
    """
    XOR-based approach jo sabse efficient hai is problem ke liye.
    Time Complexity: O(n) - Single pass through array
    Space Complexity: O(1) - Sirf ek variable use karte hain
    
    :param nums: Array of integers where all elements appear twice except one
    :return: The element that appears only once
    
    Ye approach XOR ke properties par based hai:
    1. a ⊕ 0 = a (kisi bhi number ka XOR with 0 is the number itself)
    2. a ⊕ a = 0 (kisi bhi number ka XOR with itself is 0)
    3. (a ⊕ b ⊕ a) = (a ⊕ a) ⊕ b = 0 ⊕ b = b (XOR operation is associative)
    """
    result = 0
    
    # Har element ka XOR karte jao
    for num in nums:
        result ^= num
        
    # Double elements XOR karke zero ho jayenge, sirf single element bachega
    return result
```

## Performance Comparison

| Approach | Time Complexity | Space Complexity | Best For |
|----------|----------------|------------------|----------|
| Brute Force | O(n²) | O(1) | Small arrays |
| Hash Set | O(n) | O(n) | General purpose |
| Sorting | O(n log n) | O(1) or O(n) | When memory is limited |
| XOR | O(n) | O(1) | Optimal solution |

## Conclusion

Bit manipulation (XOR) provides the optimal solution for this problem, meeting both requirements:
- Linear runtime complexity O(n)
- Constant extra space O(1)

Sorting is a good approach when we want to avoid using extra data structures but doesn't meet the linear time requirement. Hash Set approach works well but uses extra space. Brute force is straightforward but inefficient for large inputs.

