# 🔤 Valid Anagram Problem - Complete Solution Guide

## 🎯 Problem Understanding

**Input:** Two strings `s` and `t`  
**Output:** `true` if `t` is anagram of `s`, `false` otherwise  
**Anagram:** Same characters, same frequency, different arrangement  

---

## 🎨 ASCII Art Visualization - Problem Flow

```
                    🚀 STARTING POINT
                         |
                    Input: s, t
                         |
                    ┌─────────┐
                    │ s="rat" │
                    │ t="car" │
                    └─────────┘
                         |
            ┌─────────────┼─────────────┐
            │             │             │
        METHOD 1      METHOD 2      METHOD 3
        SORTING      HASH MAP     ARRAY COUNT
            │             │             │
    ┌───────────────┐ ┌───────────┐ ┌───────────┐
    │ r-a-t → a-r-t │ │ r:1,a:1.. │ │ [0,1,0..] │
    │ c-a-r → a-c-r │ │ c:1,a:1.. │ │ [0,1,1..] │
    └───────────────┘ └───────────┘ └───────────┘
            │             │             │
         Compare       Compare       Compare
            │             │             │
    ┌───────────────┐ ┌───────────┐ ┌───────────┐
    │ a-r-t ≠ a-c-r │ │ Maps ≠    │ │ Arrays ≠  │
    │ FALSE ❌       │ │ FALSE ❌   │ │ FALSE ❌   │
    └───────────────┘ └───────────┘ └───────────┘
                         |
                    🏁 ENDING POINT
                      Result: FALSE
```

---

## 📊 Method Evolution - Time Complexity Visualization

```
┌─────────────────────────────────────────────────────────────────────┐
│                    COMPLEXITY COMPARISON                            │
├─────────────────────────────────────────────────────────────────────┤
│ METHOD 1: SORTING                                                   │
│ ⏰ Time: O(n log n)    💾 Space: O(1)                              │
│ [▓▓▓▓▓▓▓▓░░] Performance: Good for small inputs                     │
│                                                                     │
│ METHOD 2: HASH MAP                                                  │
│ ⏰ Time: O(n)          💾 Space: O(n)                              │
│ [▓▓▓▓▓▓▓▓▓▓] Performance: Best for general case                     │
│                                                                     │
│ METHOD 3: ARRAY COUNT                                               │
│ ⏰ Time: O(n)          💾 Space: O(1)                              │
│ [▓▓▓▓▓▓▓▓▓▓] Performance: Best for lowercase letters only           │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 🔄 Step-by-Step Evolution Example

### Example: s="anagram", t="nagaram"

```
STEP 1: Initial State
┌─────────────────────────────────────────────────────────────────────┐
│ s = "anagram"    │ a │ n │ a │ g │ r │ a │ m │                      │
│ t = "nagaram"    │ n │ a │ g │ a │ r │ a │ m │                      │
└─────────────────────────────────────────────────────────────────────┘

STEP 2: Method 1 - Sorting Process
┌─────────────────────────────────────────────────────────────────────┐
│ s sorted: "aaagmnr"  │ a │ a │ a │ g │ m │ n │ r │                  │
│ t sorted: "aaagmnr"  │ a │ a │ a │ g │ m │ n │ r │                  │
│ Result: MATCH ✅                                                    │
└─────────────────────────────────────────────────────────────────────┘

STEP 3: Method 2 - Hash Map Counting
┌─────────────────────────────────────────────────────────────────────┐
│ s_count = {a:3, n:1, g:1, r:1, m:1}                                │
│ t_count = {n:1, a:3, g:1, r:1, m:1}                                │
│ Result: MAPS EQUAL ✅                                               │
└─────────────────────────────────────────────────────────────────────┘

STEP 4: Method 3 - Array Counting
┌─────────────────────────────────────────────────────────────────────┐
│ Index: 0 1 2 3 4 5 6 ... 25 (a-z)                                  │
│ s_arr: [3,0,0,0,0,0,1,0,0,0,0,0,1,1,0,0,0,1,0,0,0,0,0,0,0,0]       │
│ t_arr: [3,0,0,0,0,0,1,0,0,0,0,0,1,1,0,0,0,1,0,0,0,0,0,0,0,0]       │
│ Result: ARRAYS EQUAL ✅                                             │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 💡 Pseudo Code

### Method 1: Sorting Approach
```
FUNCTION isAnagram_sorting(s, t):
    IF length(s) != length(t):
        RETURN FALSE
    
    s_sorted = sort(s)
    t_sorted = sort(t)
    
    RETURN s_sorted == t_sorted
```

### Method 2: Hash Map Approach
```
FUNCTION isAnagram_hashmap(s, t):
    IF length(s) != length(t):
        RETURN FALSE
    
    char_count = {}
    
    FOR each char in s:
        char_count[char] = char_count[char] + 1
    
    FOR each char in t:
        IF char not in char_count:
            RETURN FALSE
        char_count[char] = char_count[char] - 1
        IF char_count[char] < 0:
            RETURN FALSE
    
    RETURN TRUE
```

### Method 3: Array Counting Approach
```
FUNCTION isAnagram_array(s, t):
    IF length(s) != length(t):
        RETURN FALSE
    
    count = array[26] filled with 0
    
    FOR i from 0 to length(s)-1:
        count[s[i] - 'a'] += 1
        count[t[i] - 'a'] -= 1
    
    FOR each value in count:
        IF value != 0:
            RETURN FALSE
    
    RETURN TRUE
```

---

## 💻 Complete Python Implementation

```python
def isAnagram_sorting(s: str, t: str) -> bool:
    """
    Method 1: Sorting approach
    Time Complexity: O(n log n) where n is length of string
    Space Complexity: O(1) excluding space used by sorting algorithm
    
    Approach: Sort both strings and compare them
    Agar dono strings sort karne ke baad same hai, toh anagram hai
    """
    # Length check - agar lengths different hai toh anagram nahi ho sakta
    if len(s) != len(t):
        return False
    
    # Dono strings ko sort karte hai
    s_sorted = sorted(s)
    t_sorted = sorted(t)
    
    # Sorted strings compare karte hai
    return s_sorted == t_sorted


def isAnagram_hashmap(s: str, t: str) -> bool:
    """
    Method 2: Hash Map approach
    Time Complexity: O(n) where n is length of string
    Space Complexity: O(n) for storing character counts
    
    Approach: Count characters in both strings using hash map
    Har character ki frequency count karte hai
    """
    # Length check - basic optimization
    if len(s) != len(t):
        return False
    
    # Character count ke liye dictionary banate hai
    char_count = {}
    
    # Pehle string s ke characters count karte hai
    for char in s:
        char_count[char] = char_count.get(char, 0) + 1
    
    # Phir string t ke characters subtract karte hai
    for char in t:
        if char not in char_count:
            return False  # Agar char exist nahi karta
        char_count[char] -= 1
        if char_count[char] < 0:
            return False  # Agar count negative ho gaya
    
    # Sab characters ka count zero hona chahiye
    return all(count == 0 for count in char_count.values())


def isAnagram_array(s: str, t: str) -> bool:
    """
    Method 3: Array counting approach (Only for lowercase letters)
    Time Complexity: O(n) where n is length of string
    Space Complexity: O(1) - fixed size array of 26
    
    Approach: Use array to count characters (a-z only)
    Array indexing use karte hai character count ke liye
    """
    # Length check
    if len(s) != len(t):
        return False
    
    # 26 letters ke liye array banate hai (a-z)
    count = [0] * 26
    
    # Ek saath dono strings process karte hai
    for i in range(len(s)):
        # String s ke character ka count increase karte hai
        count[ord(s[i]) - ord('a')] += 1
        # String t ke character ka count decrease karte hai
        count[ord(t[i]) - ord('a')] -= 1
    
    # Sabhi counts zero hone chahiye
    return all(c == 0 for c in count)


# Alternative implementation using Counter from collections
from collections import Counter

def isAnagram_counter(s: str, t: str) -> bool:
    """
    Method 4: Using Python's Counter (Built-in optimization)
    Time Complexity: O(n)
    Space Complexity: O(n)
    
    Approach: Use Counter to count characters
    Python ka built-in Counter use karte hai
    """
    # Length check
    if len(s) != len(t):
        return False
    
    # Counter se character frequency count karte hai
    return Counter(s) == Counter(t)


# Test cases with visualization
def test_all_methods():
    """
    Test function - sabhi methods ko test karte hai
    """
    test_cases = [
        ("anagram", "nagaram", True),
        ("rat", "car", False),
        ("", "", True),
        ("a", "ab", False),
        ("listen", "silent", True)
    ]
    
    methods = [
        ("Sorting", isAnagram_sorting),
        ("Hash Map", isAnagram_hashmap),
        ("Array Count", isAnagram_array),
        ("Counter", isAnagram_counter)
    ]
    
    print("🧪 Testing All Methods:")
    print("─" * 70)
    
    for s, t, expected in test_cases:
        print(f"Input: s='{s}', t='{t}' | Expected: {expected}")
        
        for method_name, method_func in methods:
            result = method_func(s, t)
            status = "✅" if result == expected else "❌"
            print(f"  {method_name:12}: {result} {status}")
        
        print("─" * 70)


if __name__ == "__main__":
    # Run all tests
    test_all_methods()
    
    # Individual method testing
    print("\n🔍 Individual Method Analysis:")
    s, t = "anagram", "nagaram"
    
    print(f"Testing with s='{s}', t='{t}':")
    print(f"Method 1 (Sorting): {isAnagram_sorting(s, t)}")
    print(f"Method 2 (Hash Map): {isAnagram_hashmap(s, t)}")
    print(f"Method 3 (Array): {isAnagram_array(s, t)}")
    print(f"Method 4 (Counter): {isAnagram_counter(s, t)}")
```

---

## 🎯 Algorithm Comparison

```
┌─────────────────────────────────────────────────────────────────────┐
│                        TRADE-OFFS ANALYSIS                         │
├─────────────────────────────────────────────────────────────────────┤
│ 📈 SORTING METHOD                                                   │
│ ✅ Simple to understand and implement                               │
│ ✅ Works with any characters (Unicode support)                      │
│ ❌ Slower due to sorting overhead                                   │
│ ❌ O(n log n) time complexity                                       │
│                                                                     │
│ 🗺️ HASH MAP METHOD                                                  │
│ ✅ Optimal time complexity O(n)                                     │
│ ✅ Works with any characters                                        │
│ ❌ Extra space for hash map                                         │
│ ❌ Slightly complex implementation                                  │
│                                                                     │
│ 🔢 ARRAY COUNT METHOD                                               │
│ ✅ Optimal time and space complexity                                │
│ ✅ Most efficient for lowercase letters                             │
│ ❌ Limited to specific character set                                │
│ ❌ Not flexible for Unicode                                         │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 🚀 Performance Benchmarks

```
Input Size Analysis:
┌─────────────────────────────────────────────────────────────────────┐
│ Size   │ Sorting    │ Hash Map   │ Array Count │ Best Choice        │
├─────────────────────────────────────────────────────────────────────┤
│ Small  │ Fast ⚡     │ Fast ⚡     │ Fastest 🚀   │ Any method works   │
│ Medium │ Slow 🐌     │ Fast ⚡     │ Fastest 🚀   │ Hash Map/Array     │
│ Large  │ Slower 🐌   │ Fast ⚡     │ Fastest 🚀   │ Array Count        │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 🎨 Visual Memory Aid

```
                    🧠 REMEMBER THIS!
                         
    S O R T I N G          H A S H   M A P          A R R A Y
    ┌─────────────┐        ┌─────────────┐        ┌─────────────┐
    │ "abc" "bac" │        │ a:1  a:1    │        │ [1,1,1,0..] │
    │     ↓       │        │ b:1  b:1    │        │ [1,1,1,0..] │
    │ "abc" "abc" │        │ c:1  c:1    │        │ [0,0,0,0..] │
    │   MATCH!    │        │   MATCH!    │        │ ALL ZEROS!  │
    └─────────────┘        └─────────────┘        └─────────────┘
       O(n log n)             O(n)                   O(n)
```

---

## 💼 When to Use Which Method?

1. **Sorting Method**: Jab simplicity chahiye aur performance critical nahi hai
2. **Hash Map Method**: General purpose solution, sabse versatile approach
3. **Array Count Method**: Jab sirf lowercase letters handle karne hai
4. **Counter Method**: Jab Python built-ins ka use karna ho

---

## 🔚 Conclusion

Anagram problem solve karne ke multiple ways hai, har method ka apna fayda hai:
- **Sorting**: Simple but slower
- **Hash Map**: Balanced approach
- **Array Count**: Most efficient for specific case
- **Counter**: Pythonic way

Choose according to your constraints aur requirements! 🎯
