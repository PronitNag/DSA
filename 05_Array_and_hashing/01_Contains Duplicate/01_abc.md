# Duplicate Number Detection - ASCII/Emoji Art Solution
## (Hinglish Explanation with Python Code)

### Problem Statement 🧩
> Given ek integer array `nums`, return `true` agar koi value kam-se-kam do baar array mein appear hoti hai, aur return `false` agar har element distinct hai.

It is solved using three methods: 

1)	Brute-force= compare every single number to other
2)	
3)	Sorting= and see if duplicates are adjacent
4)	
5)	Hash set to check if an element are present in hash set


### Sample Input Array:
```
[4, 2, 7, 2, 9]
```

## Three Solutions with Visualization 👇

### 1️⃣ Brute Force Method 🔨

```python
def containsDuplicate_bruteForce(nums):
    n = len(nums)
    for i in range(n):
        for j in range(i+1, n):
            if nums[i] == nums[j]:
                return True
    return False
```

#### Step-by-Step ASCII Visualization:

```
Input: [4, 2, 7, 2, 9]

Iteration 1: Compare 4 with all elements ahead
[4, 2, 7, 2, 9]
 ↓  ↓
 4  2  ➡️ No match

[4, 2, 7, 2, 9]
 ↓     ↓
 4     7  ➡️ No match

[4, 2, 7, 2, 9]
 ↓        ↓
 4        2  ➡️ No match

[4, 2, 7, 2, 9]
 ↓           ↓
 4           9  ➡️ No match

Iteration 2: Compare 2 with all elements ahead
[4, 2, 7, 2, 9]
    ↓  ↓
    2  7  ➡️ No match

[4, 2, 7, 2, 9]
    ↓     ↓
    2     2  ➡️ MATCH FOUND! ✅

Return: TRUE
```

### 2️⃣ Sorting Method 📊

```python
def containsDuplicate_sorting(nums):
    # Sorting array
    sorted_nums = sorted(nums)
    
    # Check adjacent elements
    for i in range(len(sorted_nums) - 1):
        if sorted_nums[i] == sorted_nums[i+1]:
            return True
    
    return False
```

#### Step-by-Step ASCII Visualization:

```
Original: [4, 2, 7, 2, 9]

Step 1: Sort the array
[4, 2, 7, 2, 9] ➡️ [2, 2, 4, 7, 9]

Step 2: Check adjacent elements
[2, 2, 4, 7, 9]
 ↓  ↓
 2  2  ➡️ MATCH FOUND! ✅

Return: TRUE
```

### 3️⃣ HashSet Method 🧰

```python
def containsDuplicate_hashSet(nums):
    seen = set()
    
    for num in nums:
        if num in seen:
            return True
        seen.add(num)
    
    return False
```

#### Step-by-Step ASCII Visualization:

```
Input: [4, 2, 7, 2, 9]

HashSet: {}

Step 1: Check 4
4 in HashSet? No ➡️ Add to HashSet
HashSet: {4}

Step 2: Check 2
2 in HashSet? No ➡️ Add to HashSet
HashSet: {4, 2}

Step 3: Check 7
7 in HashSet? No ➡️ Add to HashSet
HashSet: {4, 2, 7}

Step 4: Check 2
2 in HashSet? YES! ✅ ➡️ Duplicate Found!

Return: TRUE
```

## Time & Space Complexity Comparison 📈

```
+----------------+----------------+------------------+
|     Method     | Time Complexity| Space Complexity |
+----------------+----------------+------------------+
| Brute Force    |     O(n²)      |      O(1)        |
| Sorting        |   O(n log n)   |    O(1)~O(n)*    |
| HashSet        |     O(n)       |      O(n)        |
+----------------+----------------+------------------+
* Depends on sorting algorithm implementation
```

## Visual Comparison ⏱️

```
Time Complexity:

Brute Force:  O(n²)      
█████████████████████████████  Slowest! 🐌

Sorting:      O(n log n)  
█████████████████          Medium 🚶

HashSet:      O(n)        
███████                    Fastest! 🚀
```

## Evolving Solution Journey 🗺️

```
START
  │
  ▼
[4, 2, 7, 2, 9]
  │
  ├──────────────┬──────────────┐
  │              │              │
  ▼              ▼              ▼
BRUTE FORCE     SORTING        HASHSET
  │              │              │
  ▼              ▼              ▼
Compare each    Sort array      Add to set
element pair    [2,2,4,7,9]     {4,2,7}
  │              │              │
  ▼              ▼              ▼
Found match at   Check adjacent Find duplicate
i=1, j=3        elements        when adding 2 again
  │              │              │
  ▼              ▼              ▼
Return TRUE     Return TRUE     Return TRUE
  │              │              │
  └──────────────┴──────────────┘
  │
  ▼
 END
```

## Code Saare Teeno Methods Ke Saath Pura Solution 💻

```python
def containsDuplicate_bruteForce(nums):
    """
    Brute force method - har element ko baaki elements se compare karta hai
    Time: O(n²), Space: O(1)
    """
    n = len(nums)
    for i in range(n):
        for j in range(i+1, n):
            if nums[i] == nums[j]:
                return True
    return False


def containsDuplicate_sorting(nums):
    """
    Sorting method - array ko sort karke adjacent elements check karta hai
    Time: O(n log n), Space: O(1) or O(n) depending on sorting implementation
    """
    # Original array modify na karne ke liye sorted() use kiya
    sorted_nums = sorted(nums)
    
    # Check adjacent elements
    for i in range(len(sorted_nums) - 1):
        if sorted_nums[i] == sorted_nums[i+1]:
            return True
    
    return False


def containsDuplicate_hashSet(nums):
    """
    HashSet method - set ka use karke duplicates check karta hai
    Time: O(n), Space: O(n)
    """
    seen = set()
    
    for num in nums:
        # Agar number pehle se set mein hai, toh duplicate mil gaya
        if num in seen:
            return True
        # Nahi toh set mein add kar do
        seen.add(num)
    
    return False


# Test cases
test_cases = [
    [4, 2, 7, 2, 9],       # True (2 appears twice)
    [1, 2, 3, 4, 5],       # False (all distinct)
    [1, 1, 1, 3, 3, 4, 3], # True (multiple duplicates)
    [],                    # False (empty array)
]

# Test sare methods
for i, test in enumerate(test_cases):
    print(f"Test Case {i+1}: {test}")
    print(f"Brute Force: {containsDuplicate_bruteForce(test)}")
    print(f"Sorting: {containsDuplicate_sorting(test)}")
    print(f"HashSet: {containsDuplicate_hashSet(test)}")
    print("-" * 40)
```

## Kaun Sa Method Best Hai? 🤔

```
Agar array size chota hai:
  └── Koi bhi method chalega, difference negligible hoga

Agar array size medium/large hai:
  └── HashSet method sabse fast hai (O(n) time)

Agar memory constraint tight hai:
  └── Brute Force method best hai (O(1) space)

Agar array already sorted hai:
  └── Sorting method best hai (bas ek pass mein O(n) time)
```

## Final Recommendation 🌟

```
      👑
      │
 HashSet Method
      │
O(n) Time & Space
```

HashSet approach is usually the best choice for most practical scenarios jab tak aapko extra space use karne mein koi problem na ho. Iska time complexity sabse kam hai - O(n), aur code bhi simple aur readable hai. Yehi solution most interviews mein "optimal solution" maana jaata hai.
