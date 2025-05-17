# Valid Anagram Solutions

## Problem Statement

**Easy**  
**Given two strings `s` and `t`, return `true` if the two strings are anagrams of each other, otherwise return `false`.**  
**An anagram is a string that contains the exact same characters as another string, but the order of the characters can be different.**

## ASCII Art Visualization of the Problem

```
Input Strings:
"listen" and "silent"

Are these anagrams?

   l    +--+    s
   i    |  |    i
   s    |  |    l
   t    |  |    e
   e    |  |    n
   n    +--+    t
   
   Same characters? YES!
   Same count? YES!
   
   RESULT: TRUE ✓
```

## Solution Approaches

### 1. Sorting Approach

```
                 +--------+
                 | START  |
                 +--------+
                     |
                     v
             +----------------+
             | Input: s and t |
             +----------------+
                     |
                     v
            +------------------+
            | Check if lengths |
            | are equal        |
            +------------------+
                     |
                     v
                 +------+
                 | No   |-------+
                 +------+       |
                     |          v
                     | Yes     +-------+
                     v         | False |
            +------------------+ +-----+
            | Sort both strings|
            +------------------+
                     |
                     v
            +------------------+
            | Compare if sorted|
            | strings are equal|
            +------------------+
                     |
                     v
                 +------+
                 | Yes  |-------+
                 +------+       |
                     |          v
                     | No      +------+
                     v         | True |
                 +-------+     +------+
                 | False |
                 +-------+
```

#### Pseudo Code (Sorting):
```
function isAnagram(s, t):
    if length(s) != length(t):
        return False
    
    return sorted(s) == sorted(t)
```

#### Python Code (Sorting):
```python
def isAnagram_sorting(s, t):
    """
    Soln 1: Sorting approach
    Time Complexity: O(n log n) - due to sorting
    Space Complexity: O(n) - for creating sorted strings
    
    Parameters:
    s (str): First input string
    t (str): Second input string
    
    Returns:
    bool: True if s and t are anagrams, False otherwise
    """
    # Agar lengths alag hain, toh anagram nahi ho sakte
    if len(s) != len(t):
        return False
    
    # Dono strings ko sort karke compare karo
    return sorted(s) == sorted(t)
```

### 2. Hash Map Approach

```
                 +--------+
                 | START  |
                 +--------+
                     |
                     v
             +----------------+
             | Input: s and t |
             +----------------+
                     |
                     v
            +------------------+
            | Check if lengths |
            | are equal        |
            +------------------+
                     |
                     v
                 +------+
                 | No   |-------+
                 +------+       |
                     |          v
                     | Yes     +-------+
                     v         | False |
            +------------------+ +-----+
            | Create HashMap   |
            | for string s     |
            +------------------+
                     |
                     v
            +------------------+
            | For each char in |
            | s: count++ in map|
            +------------------+
                     |
                     v
            +------------------+
            | For each char in |
            | t: count-- in map|
            +------------------+
                     |
                     v
            +------------------+
            | Check if all map |
            | values are zero  |
            +------------------+
                     |
                     v
                 +------+
                 | Yes  |-------+
                 +------+       |
                     |          v
                     | No      +------+
                     v         | True |
                 +-------+     +------+
                 | False |
                 +-------+
```

#### Pseudo Code (Hash Map):
```
function isAnagram(s, t):
    if length(s) != length(t):
        return False
    
    counts = empty hash map
    
    for char in s:
        increment counts[char]
    
    for char in t:
        decrement counts[char]
        if counts[char] < 0:
            return False
    
    return all values in counts are zero
```

#### Python Code (Hash Map):
```python
def isAnagram_hashmap(s, t):
    """
    Soln 2: Hash Map approach
    Time Complexity: O(n) - where n is the length of strings
    Space Complexity: O(k) - where k is the size of character set
    
    Parameters:
    s (str): First input string
    t (str): Second input string
    
    Returns:
    bool: True if s and t are anagrams, False otherwise
    """
    # Agar lengths alag hain, toh anagram nahi ho sakte
    if len(s) != len(t):
        return False
    
    # Character count ke liye HashMap banao
    char_count = {}
    
    # Pehle string ke har character ko count karo
    for char in s:
        if char in char_count:
            char_count[char] += 1
        else:
            char_count[char] = 1
    
    # Doosre string ke har character ko subtract karo
    for char in t:
        if char not in char_count:
            return False
        
        char_count[char] -= 1
        if char_count[char] < 0:
            return False
    
    # Check karo ki saare counts zero hain
    for count in char_count.values():
        if count != 0:
            return False
    
    return True
```

### 3. Hash Table (Using Array) Approach

```
                 +--------+
                 | START  |
                 +--------+
                     |
                     v
             +----------------+
             | Input: s and t |
             +----------------+
                     |
                     v
            +------------------+
            | Check if lengths |
            | are equal        |
            +------------------+
                     |
                     v
                 +------+
                 | No   |-------+
                 +------+       |
                     |          v
                     | Yes     +-------+
                     v         | False |
            +------------------+ +-----+
            | Create array of  |
            | size 26 (for a-z)|
            +------------------+
                     |
                     v
            +------------------+
            | For each char in |
            | s: array[i]++    |
            +------------------+
                     |
                     v
            +------------------+
            | For each char in |
            | t: array[i]--    |
            +------------------+
                     |
                     v
            +------------------+
            | Check if all     |
            | array values = 0 |
            +------------------+
                     |
                     v
                 +------+
                 | Yes  |-------+
                 +------+       |
                     |          v
                     | No      +------+
                     v         | True |
                 +-------+     +------+
                 | False |
                 +-------+
```

#### Pseudo Code (Hash Table Using Array):
```
function isAnagram(s, t):
    if length(s) != length(t):
        return False
    
    counter = array of 26 zeros (for a-z)
    
    for char in s:
        counter[char - 'a']++
    
    for char in t:
        counter[char - 'a']--
        if counter[char - 'a'] < 0:
            return False
    
    return all values in counter are zero
```

#### Python Code (Hash Table Using Array):
```python
def isAnagram_array(s, t):
    """
    Soln 3: Hash Table using Array approach
    Time Complexity: O(n) - where n is the length of strings
    Space Complexity: O(1) - fixed size array for character counts
    
    Parameters:
    s (str): First input string
    t (str): Second input string
    
    Returns:
    bool: True if s and t are anagrams, False otherwise
    """
    # Agar lengths alag hain, toh anagram nahi ho sakte
    if len(s) != len(t):
        return False
    
    # 26 characters (a-z) ke liye array banao
    # Unicode code point se index calculate karenge
    char_count = [0] * 26
    
    # Pehle string ke characters ko count karo
    for char in s:
        char_count[ord(char) - ord('a')] += 1
    
    # Doosre string ke characters ko subtract karo
    for char in t:
        char_count[ord(char) - ord('a')] -= 1
        if char_count[ord(char) - ord('a')] < 0:
            return False
    
    # Check karo ki saare counts zero hain
    for count in char_count:
        if count != 0:
            return False
    
    return True
```

## Step-by-Step Evolution of Solution Process

### Example with inputs "listen" and "silent"

#### Initial State:
```
Input: s = "listen", t = "silent"

Check lengths:
len(s) = 6, len(t) = 6 ✓
```

#### Solution 1: Sorting
```
s = "listen" → sorted(s) = "eilnst"
t = "silent" → sorted(t) = "eilnst"

Compare: "eilnst" == "eilnst" ✓
Result: True
```

#### Solution 2: Hash Map
```
Step 1: Create HashMap for s = "listen"
{
   'l': 1,
   'i': 1,
   's': 1,
   't': 1,
   'e': 1,
   'n': 1
}

Step 2: Process t = "silent"
- For 's': map['s'] = 0
- For 'i': map['i'] = 0
- For 'l': map['l'] = 0
- For 'e': map['e'] = 0
- For 'n': map['n'] = 0
- For 't': map['t'] = 0

Step 3: All counts are 0 ✓
Result: True
```

#### Solution 3: Hash Table (Array)
```
Step 1: Initialize array [0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]
          Indexes:         a b c d e f g h i j k l m n o p q r s t u v w x y z

Step 2: Process s = "listen"
- 'l' → array[11] = 1
- 'i' → array[8] = 1
- 's' → array[18] = 1
- 't' → array[19] = 1
- 'e' → array[4] = 1
- 'n' → array[13] = 1

Array: [0,0,0,0,1,0,0,0,1,0,0,1,0,1,0,0,0,0,1,1,0,0,0,0,0,0]

Step 3: Process t = "silent"
- 's' → array[18] = 0
- 'i' → array[8] = 0
- 'l' → array[11] = 0
- 'e' → array[4] = 0
- 'n' → array[13] = 0
- 't' → array[19] = 0

Array: [0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]

Step 4: All values are 0 ✓
Result: True
```

### Counter Example with inputs "rat" and "car"

```
Input: s = "rat", t = "car"

Check lengths:
len(s) = 3, len(t) = 3 ✓

Sorting:
s = "rat" → sorted(s) = "art"
t = "car" → sorted(t) = "acr"

Compare: "art" != "acr" ✗
Result: False

Hash Map:
HashMap after processing 'rat': {'r':1, 'a':1, 't':1}
HashMap after processing 'car': {'r':0, 'a':0, 't':1, 'c':-1}

Check if all values are 0: No ✗
Result: False

Hash Table (Array):
Array after processing 'rat':
[1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1,0,1,0,0,0,0,0,0]
 a b c d e f g h i j k l m n o p q r s t u v w x y z

Array after processing 'car':
[0,0,1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1,0,0,0,0,0,0]
 a b c d e f g h i j k l m n o p q r s t u v w x y z

Check if all values are 0: No ✗
Result: False
```

## Emoji Art Summary

```
🧩 Valid Anagram 🧩

Input:  🔤 s 🔤   🔄   🔤 t 🔤
        
        🔍 Check if these are anagrams! 🔍

Solution 1: Sorting 📊
        s → 🔡 → sort → 🔠
        t → 🔡 → sort → 🔠
        🔠 == 🔠 ❓

Solution 2: HashMap 📝
        s → 🔡 → count++ 📈
        t → 🔡 → count-- 📉
        All counts = 0 ❓

Solution 3: Array 📊
        s → 🔡 → array++ 📈
        t → 🔡 → array-- 📉
        All array[i] = 0 ❓

📌 All solutions lead to:
        ✅ True if anagram
        ❌ False if not anagram
```

## Time and Space Complexity Comparison

| Solution          | Time Complexity | Space Complexity | Best For                          |
|-------------------|----------------|------------------|-----------------------------------|
| Sorting           | O(n log n)     | O(n)             | Simplicity, clean code            |
| Hash Map          | O(n)           | O(k)             | General purpose, any character set|
| Array (Hash Table)| O(n)           | O(1)             | Small, known character set (a-z)  |

Where:
- n = length of input strings
- k = size of the character set
