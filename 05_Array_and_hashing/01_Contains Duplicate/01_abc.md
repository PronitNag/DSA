# Duplicate Number Detection - ASCII/Emoji Art Solution
## (Hinglish Explanation with Python Code)

### Problem Statement 🧩
> Given ek integer array `nums`, return `true` agar koi value kam-se-kam do baar array mein appear hoti hai, aur return `false` agar har element distinct hai.

It is solved using three methods: 

1)	Brute-force= compare every single number to other
3)	Sorting= and see if duplicates are adjacent
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
        # Initialize an empty set to track elements we've already seen
    seen = set()
    
    # Iterate through each element in the input array
    for num in nums:
        # Check if we've already seen this element
        if num in seen:
            # If so, we found a duplicate
            return True
        # Add the current element to our set of seen elements
        seen.add(num)
    
    # If we've checked all elements without finding duplicates
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




``` text

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
```
