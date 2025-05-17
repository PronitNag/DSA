# LeetCode: Encode and Decode Strings

## Problem Statement
Design an algorithm to encode a list of strings to a single string. The encoded string is then decoded back to the original list of strings.

## ASCII Art Visualization

```
Input List of Strings:
┌─────────┐  ┌─────────┐  ┌─────────┐
│ "Hello" │  │ "World" │  │ "!"     │
└─────────┘  └─────────┘  └─────────┘
      │           │            │
      ▼           ▼            ▼
┌───────────────────────────────────┐
│           ENCODE PROCESS          │
└───────────────────────────────────┘
      │                             
      ▼                             
┌───────────────────────────────────┐
│         ENCODED STRING            │
└───────────────────────────────────┘
      │                             
      ▼                             
┌───────────────────────────────────┐
│           DECODE PROCESS          │
└───────────────────────────────────┘
      │                             
      ▼                             
┌─────────┐  ┌─────────┐  ┌─────────┐
│ "Hello" │  │ "World" │  │ "!"     │
└─────────┘  └─────────┘  └─────────┘
Output List of Strings (same as input)
```

### Solution 1: Length List + Separator

```
Encoding:
["Hello", "World", "!"]
  ↓       ↓       ↓
  5       5       1    (length of each string)
  ↓       ↓       ↓
"5,5,1#HelloWorld!"    (lengths + separator + concatenated strings)

Decoding:
"5,5,1#HelloWorld!"
  ↓       ↓
"5,5,1"   "HelloWorld!"  (split by '#')
  ↓          ↓
[5,5,1]    Extract substrings of specified lengths
  ↓          ↓
["Hello", "World", "!"]
```

### Solution 2: Length Prefixing

```
Encoding:
["Hello", "World", "!"]
  ↓       ↓       ↓
"5#Hello" + "5#World" + "1#!"  (each string prefixed with its length)
  ↓
"5#Hello5#World1#!"

Decoding:
"5#Hello5#World1#!"
  ↓      ↓      ↓
"5#Hello" "5#World" "1#!"  (parse length, then extract substring)
  ↓        ↓        ↓
"Hello"   "World"   "!"
  ↓        ↓        ↓
["Hello", "World", "!"]
```

## Pseudo Code

### Solution 1: Length List + Separator

```
ENCODE:
1. Create empty list 'sizes' for string lengths
2. Create empty result string 'res'
3. For each string in input list:
   a. Add its length to 'sizes'
4. For each size in 'sizes':
   a. Add size and ',' to 'res'
5. Add '#' separator to 'res'
6. For each string in input list:
   a. Append string to 'res'
7. Return 'res'

DECODE:
1. If input string is empty, return empty list
2. Create empty list 'sizes' for string lengths
3. Create empty list 'result' for output strings
4. Initialize index i = 0
5. Parse sizes until '#' separator:
   a. Parse number until ','
   b. Add number to 'sizes'
   c. Increment i
6. Skip '#' separator
7. For each size in 'sizes':
   a. Extract substring of that size
   b. Add substring to 'result'
8. Return 'result'
```

### Solution 2: Length Prefixing

```
ENCODE:
1. Create empty result string 'res'
2. For each string in input list:
   a. Append (length + '#' + string) to 'res'
3. Return 'res'

DECODE:
1. Create empty result list
2. Initialize index i = 0
3. While i < length of input string:
   a. Find '#' to get length
   b. Parse length value
   c. Extract substring of that length after '#'
   d. Add substring to result list
   e. Move index forward
4. Return result list
```

## Python Implementation

### Solution 1: Length List + Separator

```python
class Solution:
    """
    A solution to encode and decode strings using a length list and separator approach.
    """
    
    def encode(self, strs):
        """
        Encodes a list of strings to a single string.
        
        Args:
            strs: List[str] - Input list of strings
            
        Returns:
            str - Encoded string
        
        Working:
            1. Store lengths of all strings, separated by commas
            2. Add '#' separator
            3. Concatenate all original strings
        """
        if not strs:
            return ""
        
        # Store the sizes of each string
        sizes = []
        for s in strs:
            sizes.append(str(len(s)))
        
        # Create result string with sizes first
        res = ""
        for sz in sizes:
            res += sz + ","
        
        # Add separator
        res += "#"
        
        # Append all strings
        for s in strs:
            res += s
            
        return res
    
    def decode(self, s):
        """
        Decodes a single string to a list of strings.
        
        Args:
            s: str - Input encoded string
            
        Returns:
            List[str] - Decoded list of strings
            
        Working:
            1. Parse sizes before '#' separator
            2. Extract substrings based on sizes after separator
        """
        if not s:
            return []
        
        # Parse sizes
        sizes = []
        i = 0
        while s[i] != '#':
            curr = ""
            while s[i] != ',':
                curr += s[i]
                i += 1
            sizes.append(int(curr))
            i += 1  # Skip the comma
        
        i += 1  # Skip the '#'
        
        # Extract strings
        result = []
        for sz in sizes:
            result.append(s[i:i+sz])
            i += sz
            
        return result
```

### Solution 2: Length Prefixing

```python
class Solution:
    """
    A solution to encode and decode strings using length prefixing.
    """
    
    def encode(self, strs):
        """
        Encodes a list of strings to a single string.
        
        Args:
            strs: List[str] - Input list of strings
            
        Returns:
            str - Encoded string
            
        Working:
            For each string, prefix it with its length followed by '#'
        """
        result = ""
        for s in strs:
            result += str(len(s)) + "#" + s
        return result
    
    def decode(self, s):
        """
        Decodes a single string to a list of strings.
        
        Args:
            s: str - Input encoded string
            
        Returns:
            List[str] - Decoded list of strings
            
        Working:
            Parse each length prefix, then extract substring of that length
        """
        result = []
        i = 0
        
        while i < len(s):
            # Find position of delimiter
            j = i
            while s[j] != '#':
                j += 1
            
            # Parse length
            length = int(s[i:j])
            
            # Extract string
            i = j + 1
            result.append(s[i:i+length])
            
            # Move to next string
            i += length
            
        return result
```

## Hinglish Explanation

Solution 1 mein, hum pehle saare strings ki lengths ko comma-separated format mein store karte hain, phir ek '#' separator add karte hain, aur uske baad saare original strings ko jod dete hain. 

Decoding karte time, hum pehle '#' se pehle wale part se lengths parse karte hain, phir un lengths ke according strings ko extract karte hain.

Solution 2 mein, har string ke saamne uski length aur '#' add karte hain. Example: "Hello" ko "5#Hello" mein encode karte hain.

Decoding ke liye, hum string ko iterate karte hue har baar pehle '#' tak length read karte hain, phir us length ki substring extract karte hain.

## Time and Space Complexity

### Solution 1:
- Time Complexity: O(n), where n is the total length of all strings
- Space Complexity: O(n) for storing the encoded string

### Solution 2:
- Time Complexity: O(n), where n is the total length of all strings
- Space Complexity: O(n) for storing the encoded string

Solution 2 is generally more elegant and simpler to implement than Solution 1.


