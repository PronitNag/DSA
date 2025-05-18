# Longest Substring Without Repeating Characters

## Problem Statement
Given a string s, find the length of the longest substring without duplicate characters.

## Understanding the Problem
- We need to find a substring (contiguous sequence of characters) with no repeating characters
- We need to return the length of the longest such substring
- Example: In "abcabcbb", the longest substring without repeating characters is "abc" with length 3

## Solution Approaches
Let's solve this problem using three methods with ASCII art visualization:

### 1. Brute Force Approach

```ascii
Input String: "abcabcbb"
                                                                                
╔════════════════════════════════════════════════════════════════════════════╗
║ Check every possible substring:                                             ║
╠════════════════════════════════════════════════════════════════════════════╣
║ a -> Unique                       ✓  Length = 1                             ║
║ ab -> Unique                      ✓  Length = 2                             ║
║ abc -> Unique                     ✓  Length = 3                             ║
║ abca -> Has duplicate 'a'         ✗                                         ║
║ ...                                                                         ║
║ ...checking all possible substrings...                                      ║
║ ...                                                                         ║
║ Maximum Length Found = 3                                                    ║
╚════════════════════════════════════════════════════════════════════════════╝
```

#### Pseudo Code - Brute Force
```
function lengthOfLongestSubstring(s):
    n = length of s
    max_length = 0
    
    for i from 0 to n-1:
        char_set = empty set
        for j from i to n-1:
            if s[j] is in char_set:
                break
            add s[j] to char_set
            max_length = max(max_length, j-i+1)
            
    return max_length
```

#### Python Code - Brute Force
```python
def lengthOfLongestSubstring_bruteForce(s):
    """
    Brute force approach to find the length of longest substring without repeating chars
    
    Args:
        s (str): Input string
        
    Returns:
        int: Length of longest substring without repeating characters
    
    Time Complexity: O(n³) - We check every substring which is O(n²) and for each 
                    substring we check if it has duplicates which is O(n)
    Space Complexity: O(min(n, m)) where m is size of the charset/alphabet
    """
    n = len(s)
    max_length = 0
    
    # Bahar waala loop har possible starting position ke liye
    for i in range(n):
        char_set = set()  # Set to track dekhe hue characters
        
        # Andar waala loop har possible ending position ke liye
        for j in range(i, n):
            # Agar character already set mein hai, to yeh valid substring nahi hai
            if s[j] in char_set:
                break
                
            # Character ko set mein add karo aur length update karo
            char_set.add(s[j])
            current_length = j - i + 1
            max_length = max(max_length, current_length)
            
    return max_length
```

### 2. Sliding Window Approach

```ascii
Input String: "abcabcbb"

╔═══════════════════════════════════════════════════════════════════════════════╗
║ Start with window [i,j] where both i and j point to first character           ║
║                                                                               ║
║ Initial:    [a] b c a b c b b                                                 ║
║              i                                                                ║
║              j                                                                ║
║                                                                               ║
║ Expand window by moving j:                                                    ║
║              [a b c] a b c b b                                                ║
║               i     j                                                         ║
║                                                                               ║
║ When we see 'a' again, move i after the first 'a':                            ║
║                [b c a] b c b b                                                ║
║                 i     j                                                       ║
║                                                                               ║
║ Continue this process...                                                      ║
║                                                                               ║
║ Longest window found: "abc" with length 3                                     ║
╚═══════════════════════════════════════════════════════════════════════════════╝
```

#### Pseudo Code - Sliding Window
```
function lengthOfLongestSubstring(s):
    n = length of s
    char_set = empty set
    max_length = 0
    i = 0
    
    for j from 0 to n-1:
        while s[j] is in char_set:
            remove s[i] from char_set
            i = i + 1
        add s[j] to char_set
        max_length = max(max_length, j-i+1)
        
    return max_length
```

#### Python Code - Sliding Window
```python
def lengthOfLongestSubstring_slidingWindow(s):
    """
    Sliding window approach to find length of longest substring without repeating chars
    
    Args:
        s (str): Input string
        
    Returns:
        int: Length of longest substring without repeating characters
    
    Time Complexity: O(2n) which simplifies to O(n) because each character will be 
                    processed at most twice (once when added to the window and once
                    when removed)
    Space Complexity: O(min(n, m)) where m is size of the charset/alphabet
    """
    n = len(s)
    char_set = set()  # Set to track window mein maujood characters
    max_length = 0
    i = 0  # Window ka left pointer
    
    # j is window ka right pointer
    for j in range(n):
        # Jab tak duplicate character milta hai, window ko left se shrink karo
        while s[j] in char_set:
            char_set.remove(s[i])
            i += 1
            
        # Current character ko window mein add karo
        char_set.add(s[j])
        
        # Update maximum length
        max_length = max(max_length, j - i + 1)
        
    return max_length
```

### 3. Sliding Window Optimized Approach

```ascii
Input String: "abcabcbb"

╔═════════════════════════════════════════════════════════════════════════════╗
║ Use a dictionary to store character positions                                ║
║                                                                             ║
║ Initial:    [a] b c a b c b b                                               ║
║              i                                                              ║
║              j                                                              ║
║ dict = {a:0}                                                                ║
║                                                                             ║
║ Move j forward, updating dict:                                              ║
║              [a b c] a b c b b                                              ║
║               i     j                                                       ║
║ dict = {a:0, b:1, c:2}                                                      ║
║                                                                             ║
║ When we see 'a' again, directly jump i to pos+1:                            ║
║                  [a b c] b b                                                ║
║                   i     j                                                   ║
║ dict = {a:3, b:1, c:2}                                                      ║
║                                                                             ║
║ Optimize by directly jumping instead of removing one by one                 ║
║                                                                             ║
║ Longest window found: "abc" with length 3                                   ║
╚═════════════════════════════════════════════════════════════════════════════╝
```

#### Pseudo Code - Sliding Window Optimized
```
function lengthOfLongestSubstring(s):
    n = length of s
    char_map = empty dictionary
    max_length = 0
    i = 0
    
    for j from 0 to n-1:
        if s[j] is in char_map and char_map[s[j]] >= i:
            i = char_map[s[j]] + 1
        char_map[s[j]] = j
        max_length = max(max_length, j-i+1)
        
    return max_length
```

#### Python Code - Sliding Window Optimized
```python
def lengthOfLongestSubstring_slidingWindowOptimized(s):
    """
    Optimized sliding window approach using a dictionary to track character positions
    
    Args:
        s (str): Input string
        
    Returns:
        int: Length of longest substring without repeating characters
    
    Time Complexity: O(n) - Each character is processed exactly once
    Space Complexity: O(min(n, m)) where m is size of the charset/alphabet
    """
    n = len(s)
    char_dict = {}  # Dictionary to track har character ka last dekha position
    max_length = 0
    start = 0  # Current window ka start position
    
    for end in range(n):
        # Agar character pehle dikha hai aur window ke andar hai
        if s[end] in char_dict and char_dict[s[end]] >= start:
            # Window ka start direct uss character ke baad se kardo
            start = char_dict[s[end]] + 1
        
        # Character ka current position store karo
        char_dict[s[end]] = end
        
        # Update maximum length
        current_length = end - start + 1
        max_length = max(max_length, current_length)
        
    return max_length
```

## Emoji Art Visualization 📝

```
Input String: "abcabcbb"

🔍 Brute Force: Check ALL possible substrings!
   🧩 a    ✓
   🧩 ab   ✓
   🧩 abc  ✓    
   🧩 abca ✗ (duplicate 'a')
   ... and so on
   
🪟 Sliding Window:
   👉 [a]bcabcbb       (length 1)
   👉 [ab]cabcbb       (length 2) 
   👉 [abc]abcbb       (length 3)
   👉 a[bca]bcbb       (length 3, move start after seeing duplicate 'a')
   👉 ab[cab]cbb       (length 3, move start after seeing duplicate 'b')
   👉 abc[abc]bb       (length 3, move start after seeing duplicate 'c')
   👉 abca[bc]bb       (length 2, move start after seeing duplicate 'b')
   👉 abcab[cb]b       (length 2, move start after seeing duplicate 'c')
   👉 abcabc[b]b       (length 1, move start after seeing duplicate 'b')
   
⚡ Sliding Window Optimized:
   🗺️ {a:0} [a]bcabcbb           (length 1)
   🗺️ {a:0, b:1} [ab]cabcbb      (length 2)
   🗺️ {a:0, b:1, c:2} [abc]abcbb (length 3)
   🗺️ {a:3, b:1, c:2} a[bca]bcbb (length 3, jump start after first 'a')
   ... and so on
```

## Performance Comparison

| Algorithm            | Time Complexity | Space Complexity     | Pros | Cons |
|----------------------|----------------|----------------------|------|------|
| Brute Force          | O(n³)          | O(min(n, m))         | Simple to understand | Very inefficient for large inputs |
| Sliding Window       | O(n)           | O(min(n, m))         | Much more efficient | Requires careful implementation |
| Sliding Window Opt.  | O(n)           | O(min(n, m))         | Most efficient | Slightly more complex logic |

## Complete Solution

```python
class Solution:
    def lengthOfLongestSubstring_bruteForce(self, s):
        """
        Brute force approach to find the length of longest substring without repeating chars
        
        Args:
            s (str): Input string
            
        Returns:
            int: Length of longest substring without repeating characters
        
        Time Complexity: O(n³)
        Space Complexity: O(min(n, m)) where m is size of the charset/alphabet
        """
        n = len(s)
        max_length = 0
        
        # Bahar waala loop har possible starting position ke liye
        for i in range(n):
            char_set = set()  # Set to track dekhe hue characters
            
            # Andar waala loop har possible ending position ke liye
            for j in range(i, n):
                # Agar character already set mein hai, to yeh valid substring nahi hai
                if s[j] in char_set:
                    break
                    
                # Character ko set mein add karo aur length update karo
                char_set.add(s[j])
                current_length = j - i + 1
                max_length = max(max_length, current_length)
                
        return max_length
    
    def lengthOfLongestSubstring_slidingWindow(self, s):
        """
        Sliding window approach to find length of longest substring without repeating chars
        
        Args:
            s (str): Input string
            
        Returns:
            int: Length of longest substring without repeating characters
        
        Time Complexity: O(n)
        Space Complexity: O(min(n, m)) where m is size of the charset/alphabet
        """
        n = len(s)
        char_set = set()  # Set to track window mein maujood characters
        max_length = 0
        i = 0  # Window ka left pointer
        
        # j is window ka right pointer
        for j in range(n):
            # Jab tak duplicate character milta hai, window ko left se shrink karo
            while s[j] in char_set:
                char_set.remove(s[i])
                i += 1
                
            # Current character ko window mein add karo
            char_set.add(s[j])
            
            # Update maximum length
            max_length = max(max_length, j - i + 1)
            
        return max_length
    
    def lengthOfLongestSubstring_slidingWindowOptimized(self, s):
        """
        Optimized sliding window approach using a dictionary to track character positions
        
        Args:
            s (str): Input string
            
        Returns:
            int: Length of longest substring without repeating characters
        
        Time Complexity: O(n)
        Space Complexity: O(min(n, m)) where m is size of the charset/alphabet
        """
        n = len(s)
        char_dict = {}  # Dictionary to track har character ka last dekha position
        max_length = 0
        start = 0  # Current window ka start position
        
        for end in range(n):
            # Agar character pehle dikha hai aur window ke andar hai
            if s[end] in char_dict and char_dict[s[end]] >= start:
                # Window ka start direct uss character ke baad se kardo
                start = char_dict[s[end]] + 1
            
            # Character ka current position store karo
            char_dict[s[end]] = end
            
            # Update maximum length
            current_length = end - start + 1
            max_length = max(max_length, current_length)
            
        return max_length
```

