# Anagram Solution with Visual Representation

## Problem Statement
> Given two strings s and t, return true if t is an anagram of s, and false otherwise.

## Kya Hai Anagram? (What is Anagram?)
Anagram tab hota hai jab do strings me same characters hote hain, bas unka order alag ho sakta hai.
Example: "listen" and "silent" ek anagram pair hai.

## Visual Representation of Solutions

```
Input: s = "listen", t = "silent"
```

### Solution 1: HashMap Approach
```
 "listen"                  "silent"
  ┌─────┐                  ┌─────┐
  │ l:1 │                  │ s:1 │
  │ i:1 │                  │ i:1 │
  │ s:1 │  Compare Maps    │ l:1 │
  │ t:1 │  ==========>     │ e:1 │
  │ e:1 │                  │ n:1 │
  │ n:1 │                  │ t:1 │
  └─────┘                  └─────┘
    Map 1                    Map 2
     |                         |
     |                         |
     v                         v
     Same keys and values? --> TRUE
```

### Solution 2: Sorting Approach
```
"listen"    -->    Sort    -->    "eilnst"
                                     ||
                                   Compare
                                     ||
"silent"    -->    Sort    -->    "eilnst"
                                     ||
                                     v
                                   EQUAL
                                     ||
                                     v
                                    TRUE
```

### Solution 3: ASCII Count Approach
```
      "listen"                      "silent"
        |                              |
        v                              v
┌──────────────────┐       ┌──────────────────┐
│ [0,0,0,...,0,0]  │       │ [0,0,0,...,0,0]  │
└──────────────────┘       └──────────────────┘
    ASCII array               ASCII array
 (26 zeros initially)       (26 zeros initially)
        |                              |
        v                              v
┌──────────────────┐       ┌──────────────────┐
│ l -> +1          │       │ s -> +1          │
│ i -> +1          │       │ i -> +1          │
│ s -> +1          │       │ l -> +1          │
│ t -> +1          │       │ e -> +1          │
│ e -> +1          │       │ n -> +1          │
│ n -> +1          │       │ t -> +1          │
└──────────────────┘       └──────────────────┘
        |                              |
        v                              v
┌──────────────────┐       ┌──────────────────┐
│[0,0,0,0,1,1,...] │       │[0,0,0,0,1,1,...] │
└──────────────────┘       └──────────────────┘
        |                              |
        +------------------------------+
                      |
                      v
                 Arrays match?
                      |
                      v
                    TRUE
```

## Pseudocode

### Method 1: HashMap Approach
```
function isAnagram(s, t):
    if length(s) != length(t):
        return False
    
    create map1 to store characters and counts of s
    create map2 to store characters and counts of t
    
    for each character c in s:
        increment count of c in map1
    
    for each character c in t:
        increment count of c in map2
    
    return map1 == map2
```

### Method 2: Sorting Approach
```
function isAnagram(s, t):
    if length(s) != length(t):
        return False
    
    sort s
    sort t
    
    return s == t
```

### Method 3: ASCII Count Approach
```
function isAnagram(s, t):
    if length(s) != length(t):
        return False
    
    create count_array[26] with all zeros
    
    for each character c in s:
        increment count_array[ascii(c) - ascii('a')]
    
    for each character c in t:
        decrement count_array[ascii(c) - ascii('a')]
    
    for each count in count_array:
        if count != 0:
            return False
    
    return True
```

## Python Implementation

### Method 1: HashMap Approach (Using Dictionary)
```python
def isAnagram_hashmap(s, t):
    """
    Anagram checker using hashmap approach
    
    Parameters:
    s (str): First string
    t (str): Second string
    
    Returns:
    bool: True if t is an anagram of s, False otherwise
    
    Time Complexity: O(n) - where n is the length of strings
    Space Complexity: O(k) - where k is the unique characters in strings
    """
    # Agar dono strings ki length alag hai, to anagram nahi ho sakte
    if len(s) != len(t):
        return False
    
    # Dono strings ke liye hashmap banate hain
    count_s = {}
    count_t = {}
    
    # Pehli string ke har character ko count karte hain
    for char in s:
        if char in count_s:
            count_s[char] += 1
        else:
            count_s[char] = 1
    
    # Dusri string ke har character ko count karte hain
    for char in t:
        if char in count_t:
            count_t[char] += 1
        else:
            count_t[char] = 1
    
    # Dono hashmaps ko compare karte hain
    return count_s == count_t
```

### Method 2: Sorting Approach
```python
def isAnagram_sorting(s, t):
    """
    Anagram checker using sorting approach
    
    Parameters:
    s (str): First string
    t (str): Second string
    
    Returns:
    bool: True if t is an anagram of s, False otherwise
    
    Time Complexity: O(n log n) - due to sorting
    Space Complexity: O(n) - for storing sorted strings
    """
    # Agar dono strings ki length alag hai, to anagram nahi ho sakte
    if len(s) != len(t):
        return False
    
    # Dono strings ko sort karte hain
    sorted_s = sorted(s)
    sorted_t = sorted(t)
    
    # Sorted strings ko compare karte hain
    return sorted_s == sorted_t
```

### Method 3: ASCII Count Approach
```python
def isAnagram_ascii(s, t):
    """
    Anagram checker using ASCII count approach
    
    Parameters:
    s (str): First string
    t (str): Second string
    
    Returns:
    bool: True if t is an anagram of s, False otherwise
    
    Time Complexity: O(n) - where n is the length of strings
    Space Complexity: O(1) - constant space for array of size 26
    """
    # Agar dono strings ki length alag hai, to anagram nahi ho sakte
    if len(s) != len(t):
        return False
    
    # 26 lowercase letters ke liye ek array banate hain
    count = [0] * 26
    
    # Pehli string ke har character ke liye count badhate hain
    for char in s:
        count[ord(char) - ord('a')] += 1
    
    # Dusri string ke har character ke liye count ghatate hain
    for char in t:
        count[ord(char) - ord('a')] -= 1
    
    # Agar koi bhi count 0 nahi hai, to anagram nahi hai
    for val in count:
        if val != 0:
            return False
    
    # Sab counts 0 hain, to anagram hai
    return True
```

## Emoji Art Representation 🎭

```
Input: s = "listen", t = "silent"
```

### Solution Flow with Emoji Art:

```
                                 🔍 START 🔍
                                      |
                                      v
                    🧵 Check if lengths are equal? 🧵
                                      |
                          +-----------+-----------+
                          |                       |
                      Yes |                       | No
                          v                       v
      +-----------------------------------+     ❌ FALSE ❌
      |         Choose a method:          |
      +-----------------------------------+
          |               |              |
          v               v              v
   🗺️ HashMap        📊 Sorting       🔢 ASCII
      |               |                |
      v               v                v
 Count chars      Sort both      Create array[26]
      |               |                |
      v               v                v
 Compare maps    Compare sorted   Check if array
      |               |           all zeros
      v               v                |
    Result          Result             v
      |               |              Result
      |               |                |
      +---------------+----------------+
                      |
                      v
                 🎯 ANSWER 🎯
                (TRUE/FALSE)
```

## Example Test Cases:

1. `isAnagram("anagram", "nagaram")` → `True` ✅
2. `isAnagram("rat", "car")` → `False` ❌
3. `isAnagram("a", "ab")` → `False` ❌ (different lengths)

## Summary of All Methods:

| Method           | Time Complexity | Space Complexity | Comments                        |
|------------------|----------------|------------------|--------------------------------|
| HashMap Approach | O(n)           | O(k)             | k = unique chars, good general solution |
| Sorting Approach | O(n log n)     | O(n)             | Simplest to implement          |
| ASCII Count      | O(n)           | O(1)             | Most efficient for lowercase letters only |

## Conclusion:
* Hashmap approach is versatile and works for any character set
* Sorting approach is simpler but slower
* ASCII count approach is fastest but limited to known character sets

Saare methods apne-apne tarike se sahi hain, bas aapke requirements ke hisaab se choose karna hai! (All methods are correct in their own way, just choose according to your requirements!)
