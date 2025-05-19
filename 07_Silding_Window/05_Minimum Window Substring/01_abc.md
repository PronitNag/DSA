# Minimum Window Substring Solution

## Problem Statement
Given two strings s and t of lengths m and n respectively, return the minimum window substring of s such that every character in t (including duplicates) is included in the window. If there is no such substring, return the empty string "".

## Visual Explanation with ASCII Art

### Initial Setup
```
String s: "ADOBECODEBANC"
String t: "ABC"

We need to find smallest substring in s containing all characters of t.
```

### Visualizing Sliding Window Approach

```
Step 1: Initialize window
          ↓
s = [A] D O B E C O D E B A N C    
    ↑
    window = "A" (missing B,C so not valid)

Step 2: Expand window
          ↓     ↓
s = [A D O B E C] O D E B A N C    
    ↑
    window = "ADOBEC" (valid! contains A,B,C)

Step 3: Try to shrink window from left
            ↓     ↓
s = A [D O B E C] O D E B A N C    
      ↑
    window = "DOBEC" (invalid, missing A)

Step 4: Move back to valid window
          ↓     ↓
s = [A D O B E C] O D E B A N C    
    ↑
    window = "ADOBEC" (current minimum)

Step 5: Move right boundary to find better window
          ↓           ↓
s = [A D O B E C O D E B A N C]    
    ↑
    Valid windows found during sliding:
    "ADOBEC" (length 6)
    "BECODEBA" (length 8) - rejected, longer
    "CODEBA" (length 6) - tied, keep track
    "ODEBANC" (length 7) - rejected, longer
    "DEBANC" (length 6) - tied, keep track
    "EBANC" (length 5) - better! new minimum
    "BANC" (length 4) - better! new minimum

Final Answer: "BANC" (minimum window containing A,B,C)
```

### Emoji Art Visualization

```
String s: 🅰️🔤🔅🅱️📧🅲️🔅🔤📧🅱️🅰️🔑🅲️
String t: 🅰️🅱️🅲️

✅ = valid window
❌ = invalid window

Initial:         ❌ 🅰️ (missing 🅱️,🅲️)
Expand:          ✅ 🅰️🔤🔅🅱️📧🅲️ (valid but try to minimize)
Shrink attempt:  ❌ 🔤🔅🅱️📧🅲️ (missing 🅰️)
Move window:     ✅ 🔅🅱️📧🅲️🔅🔤📧 (too long)
Continue...      ✅ 🅱️🅰️🔑🅲️ (final minimum window with 🅰️,🅱️,🅲️)
```

## Approach 1: Brute Force Approach

### Pseudocode
```
function minWindow(s, t):
    if t is empty or length(t) > length(s):
        return ""
    
    Initialize hashmap need to store frequency of characters in t
    Initialize minLen to infinity
    Initialize result to empty string
    
    For each possible starting position 'start' in s:
        For each possible ending position 'end' starting from 'start':
            Initialize window substring as s[start:end+1]
            If window contains all characters of t with required frequency:
                If length of window < minLen:
                    Update minLen and result
                Break inner loop (found valid window for this start)
    
    Return result
```

### Python Implementation (Brute Force)

```python
def minWindow(s, t):
    """
    Brute force solution to find minimum window substring.
    
    Args:
        s (str): Source string to search within
        t (str): Target string with characters to find
        
    Returns:
        str: Minimum window substring containing all characters from t
        
    Time Complexity: O(n^3), where n is length of string s
                    (n^2 for all substrings, n for checking if valid)
    Space Complexity: O(m + n), where m is length of string t
    """
    if not t or len(t) > len(s):
        return ""
        
    # Create frequency counter for characters in t
    need = {}
    for char in t:
        need[char] = need.get(char, 0) + 1
        
    min_len = float('inf')
    result = ""
    
    # Check all possible substrings (Brute Force)
    for start in range(len(s)):
        for end in range(start, len(s)):
            # Get current substring
            window = s[start:end+1]
            
            # Check if window is valid
            is_valid = True
            window_count = {}
            for char in window:
                window_count[char] = window_count.get(char, 0) + 1
                
            for char, count in need.items():
                if char not in window_count or window_count[char] < count:
                    is_valid = False
                    break
                    
            if is_valid and end - start + 1 < min_len:
                min_len = end - start + 1
                result = window
                
    return result
```

## Approach 2: Sliding Window Approach (Optimized)

### Pseudocode
```
function minWindow(s, t):
    if t is empty or length(t) > length(s):
        return ""
    
    Initialize hashmaps need and window to track character frequencies
    Initialize left pointer to 0
    Initialize valid_count to 0 (number of characters matched correctly)
    Initialize result parameters (min_len and result)
    
    For right pointer from 0 to length(s)-1:
        Add s[right] to window
        
        If s[right] is in need and window frequency matches need frequency:
            Increment valid_count
            
        While valid_count equals the number of unique characters in t:
            Update result if current window is smaller
            Remove s[left] from window
            If s[left] is in need and removing it makes window invalid:
                Decrement valid_count
            Increment left
    
    Return result
```

### Python Implementation (Sliding Window)

```python
def minWindow(s, t):
    """
    Sliding window solution to find minimum window substring.
    
    Args:
        s (str): Source string to search within
        t (str): Target string with characters to find
        
    Returns:
        str: Minimum window substring containing all characters from t
        
    Time Complexity: O(n), where n is length of string s
    Space Complexity: O(m + n), where m is length of string t
    """
    if not t or len(t) > len(s):
        return ""
        
    # Dictionary to keep count of characters needed
    need = {}
    for char in t:
        need[char] = need.get(char, 0) + 1
    
    # Number of unique characters in t
    required = len(need)
    
    # Sliding window variables
    window = {}
    left = 0
    formed = 0  # Count of characters with matching frequency
    
    # Result tracking
    min_len = float('inf')
    result_start = 0
    
    # Move right pointer for each character in s
    for right in range(len(s)):
        # Add current character to window count
        char = s[right]
        window[char] = window.get(char, 0) + 1
        
        # Check if current character helps form a valid window
        if char in need and window[char] == need[char]:
            formed += 1
        
        # Try to minimize window by moving left pointer
        while left <= right and formed == required:
            char = s[left]
            
            # Update result if current window is smaller
            if right - left + 1 < min_len:
                min_len = right - left + 1
                result_start = left
            
            # Remove leftmost character from window
            window[char] -= 1
            
            # Check if removing this character makes window invalid
            if char in need and window[char] < need[char]:
                formed -= 1
                
            # Move left pointer
            left += 1
    
    # Return result
    return s[result_start:result_start + min_len] if min_len != float('inf') else ""
```

## Solution in Hinglish

```python
def minimum_window_substring(s, t):
    """
    Ye function minimum window substring dhoondhta hai jo saare characters t mein
    se contain karta hai (duplicates sameth).
    
    Args:
        s (str): Main string jisme search karna hai
        t (str): Target string jiske saare characters dhoondhne hai
        
    Returns:
        str: Sabse chota substring jo t ke saare characters contain karta hai
    """
    # Agar t empty hai ya t ka size s se bada hai to empty return karo
    if not t or len(t) > len(s):
        return ""
        
    # Dictionary banao t ke characters count karne ke liye
    zaroorat = {}
    for char in t:
        zaroorat[char] = zaroorat.get(char, 0) + 1
    
    # Kitne unique characters chahiye
    required_chars = len(zaroorat)
    
    # Sliding window ke variables
    window_chars = {}
    left = 0
    matched_chars = 0  # Kitne characters ki frequency match ho gayi
    
    # Result track karne ke liye variables
    min_length = float('inf')
    result_start = 0
    
    # Right pointer ko aage badhao
    for right in range(len(s)):
        # Current character ko window mein add karo
        char = s[right]
        window_chars[char] = window_chars.get(char, 0) + 1
        
        # Check karo ki kya current character ne help ki valid window banane mein
        if char in zaroorat and window_chars[char] == zaroorat[char]:
            matched_chars += 1
        
        # Try karo window ko minimize karne ka by moving left pointer
        while left <= right and matched_chars == required_chars:
            char = s[left]
            
            # Update result agar current window chota hai
            if right - left + 1 < min_length:
                min_length = right - left + 1
                result_start = left
            
            # Leftmost character ko window se hatao
            window_chars[char] -= 1
            
            # Check karo ki character hatane se window invalid to nahi ho gaya
            if char in zaroorat and window_chars[char] < zaroorat[char]:
                matched_chars -= 1
                
            # Left pointer ko aage badhao
            left += 1
    
    # Final result return karo
    return s[result_start:result_start + min_length] if min_length != float('inf') else ""
```

## Time and Space Complexity Analysis

### Brute Force Approach:
- **Time Complexity**: O(n³) where n is the length of string s
  - O(n²) for generating all possible substrings
  - Additional O(n) for checking if each substring is valid
- **Space Complexity**: O(m + n) where m is the length of string t
  - Storage for character frequency maps

### Sliding Window Approach:
- **Time Complexity**: O(n) where n is the length of string s
  - Each character is processed at most twice (once by right pointer, once by left)
- **Space Complexity**: O(m + n) where m is the length of string t
  - Storage for character frequency maps

## Conclusion

The sliding window approach significantly optimizes the solution compared to the brute
force approach. By intelligently moving the left and right boundaries of our window,
we avoid redundant checks of substrings and achieve a linear time complexity solution.
The ASCII art and emoji visualizations help understand how the window expands and
contracts to find the optimal solution.

