# Group Anagrams Solution

## Problem Statement
Given an array of strings `strs`, group all anagrams together into sublists. You may return the output in any order.

An anagram is a string that contains the exact same characters as another string, but the order of the characters can be different.

## Visual Solution Process

### Starting Point
```
Input: ["eat", "tea", "tan", "ate", "nat", "bat"]
```

```
+---------+     +---------+     +---------+     +---------+     +---------+     +---------+
|  "eat"  |     |  "tea"  |     |  "tan"  |     |  "ate"  |     |  "nat"  |     |  "bat"  |
+---------+     +---------+     +---------+     +---------+     +---------+     +---------+
     |               |               |               |               |               |
     v               v               v               v               v               v
    😕              😕              😕              😕              😕              😕
Kya karna hai? Hame anagrams ko group karna hai!
```

### Solution 1: Using Sorting (Time Evolution)

```
Step 1: Sort each string
+---------+     +---------+     +---------+     +---------+     +---------+     +---------+
|  "eat"  |     |  "tea"  |     |  "tan"  |     |  "ate"  |     |  "nat"  |     |  "bat"  |
+---------+     +---------+     +---------+     +---------+     +---------+     +---------+
     |               |               |               |               |               |
     v               v               v               v               v               v
+---------+     +---------+     +---------+     +---------+     +---------+     +---------+
|  "aet"  |     |  "aet"  |     |  "ant"  |     |  "aet"  |     |  "ant"  |     |  "abt"  |
+---------+     +---------+     +---------+     +---------+     +---------+     +---------+
```

```
Step 2: Group by sorted string
                 +---------------------+
                 |     Dictionary      |
                 +---------------------+
                 |  "aet" → ["eat",    |
                 |          "tea",     |
                 |          "ate"]     |
                 |                     |
                 |  "ant" → ["tan",    |
                 |          "nat"]     |
                 |                     |
                 |  "abt" → ["bat"]    |
                 +---------------------+
                           |
                           v
                 +---------------------+
                 |      Output         |
                 +---------------------+
                 | [["eat","tea","ate"],|
                 |  ["tan","nat"],     |
                 |  ["bat"]]           |
                 +---------------------+
```

### Solution 2: Using Counting (Time Evolution)

```
Step 1: Count characters in each string
+---------+     +---------+     +---------+     +---------+     +---------+     +---------+
|  "eat"  |     |  "tea"  |     |  "tan"  |     |  "ate"  |     |  "nat"  |     |  "bat"  |
+---------+     +---------+     +---------+     +---------+     +---------+     +---------+
     |               |               |               |               |               |
     v               v               v               v               v               v
+-----------+   +-----------+   +-----------+   +-----------+   +-----------+   +-----------+
| a:1,e:1,  |   | a:1,e:1,  |   | a:1,n:1,  |   | a:1,e:1,  |   | a:1,n:1,  |   | a:1,b:1,  |
| t:1       |   | t:1       |   | t:1       |   | t:1       |   | t:1       |   | t:1       |
+-----------+   +-----------+   +-----------+   +-----------+   +-----------+   +-----------+
```

```
Step 2: Create a hash key for each count
+---------+     +---------+     +---------+     +---------+     +---------+     +---------+
|  "eat"  |     |  "tea"  |     |  "tan"  |     |  "ate"  |     |  "nat"  |     |  "bat"  |
+---------+     +---------+     +---------+     +---------+     +---------+     +---------+
     |               |               |               |               |               |
     v               v               v               v               v               v
+-----------+   +-----------+   +-----------+   +-----------+   +-----------+   +-----------+
| "a1e1t1"  |   | "a1e1t1"  |   | "a1n1t1"  |   | "a1e1t1"  |   | "a1n1t1"  |   | "a1b1t1"  |
+-----------+   +-----------+   +-----------+   +-----------+   +-----------+   +-----------+
```

```
Step 3: Group by character count key
                 +---------------------+
                 |     Dictionary      |
                 +---------------------+
                 | "a1e1t1" → ["eat",  |
                 |            "tea",   |
                 |            "ate"]   |
                 |                     |
                 | "a1n1t1" → ["tan",  |
                 |            "nat"]   |
                 |                     |
                 | "a1b1t1" → ["bat"]  |
                 +---------------------+
                           |
                           v
                 +---------------------+
                 |      Output         |
                 +---------------------+
                 | [["eat","tea","ate"],|
                 |  ["tan","nat"],     |
                 |  ["bat"]]           |
                 +---------------------+
```

## Pseudo Code

### Solution 1: Using Sorting
```
function groupAnagramsBySorting(strs):
    result = empty dictionary
    
    for each string in strs:
        sorted_string = sort characters of string
        
        if sorted_string is not in result:
            result[sorted_string] = empty list
            
        append string to result[sorted_string]
    
    return values of result as list
```

2. **Har string ko process karo**:
   Input list mein di gayi har string ke liye:
   
   a. **String ke characters ko sort karo**:
      Jaise "eat" ko sort karne par "aet" milta hai
      "tea" ko sort karne par bhi "aet" milta hai
      
   b. **Dictionary mein check karo**:
      Agar sorted string (jaise "aet") already dictionary mein hai, toh kuch nahi karna.
      Agar nahi hai, toh dictionary mein ek new key banao (sorted string) aur uske value ke roop mein ek khali list assign karo.
      
   c. **Original string ko add karo**:
      Original string (jaise "eat" ya "tea") ko sorted string key ke corresponding list mein add karo.

3. **Result return karo**:
   Dictionary ke saare values (jo ki lists hain) ko ek list mein convert karke return karo.

## Example:

Input: ["eat", "tea", "tan", "ate", "nat", "bat"]

Processing:
- "eat" → sort → "aet" → dictionary: {"aet": ["eat"]}
- "tea" → sort → "aet" → dictionary: {"aet": ["eat", "tea"]}
- "tan" → sort → "ant" → dictionary: {"aet": ["eat", "tea"], "ant": ["tan"]}
- "ate" → sort → "aet" → dictionary: {"aet": ["eat", "tea", "ate"], "ant": ["tan"]}
- "nat" → sort → "ant" → dictionary: {"aet": ["eat", "tea", "ate"], "ant": ["tan", "nat"]}
- "bat" → sort → "abt" → dictionary: {"aet": ["eat", "tea", "ate"], "ant": ["tan", "nat"], "abt": ["bat"]}

Output: [["eat", "tea", "ate"], ["tan", "nat"], ["bat"]]

### Solution 2: Using Character Counting
```
function groupAnagramsByCounting(strs):
    result = empty dictionary
    
    for each string in strs:
        char_count = count occurrences of each character in string
        
        create a key from char_count
        
        if key is not in result:
            result[key] = empty list
            
        append string to result[key]
    
    return values of result as list
```

## Python Implementation

### Solution 1: Using Sorting

```python
def groupAnagramsBySorting(strs):
    """
    Group anagrams together using sorting method.
    
    Args:
        strs (List[str]): List of input strings
        
    Returns:
        List[List[str]]: List of anagram groups
        
    Time Complexity: O(N * K * log K) where N is length of input array, K is max length of strings
    Space Complexity: O(N * K)
    """
    # Dictionary to store groups of anagrams
    anagram_groups = {}
    
    # Process each string in the input list
    for s in strs:
        # Sort the characters of the string to get a unique key for anagrams
        # "eat" -> "aet", "tea" -> "aet", etc.
        sorted_str = ''.join(sorted(s))
        
        # If this sorted form is not in our dictionary yet, create a new list
        if sorted_str not in anagram_groups:
            anagram_groups[sorted_str] = []
            
        # Add the original string to its anagram group
        anagram_groups[sorted_str].append(s)
    
    # Convert dictionary values to a list of lists and return
    return list(anagram_groups.values())
```

### Solution 2: Using Character Counting

```python
def groupAnagramsByCounting(strs):
    """
    Group anagrams together using character counting method.
    
    Args:
        strs (List[str]): List of input strings
        
    Returns:
        List[List[str]]: List of anagram groups
        
    Time Complexity: O(N * K) where N is length of input array, K is max length of strings
    Space Complexity: O(N * K)
    """
    # Dictionary to store groups of anagrams
    anagram_groups = {}
    
    # Process each string in the input list
    for s in strs:
        # Initialize a count array for lowercase English letters (a-z)
        # We'll use ASCII values: 'a' is 97, so we'll use indices 0-25
        count = [0] * 26
        
        # Count the occurrences of each character
        for c in s:
            count[ord(c) - ord('a')] += 1
        
        # Convert the count array to a tuple so it can be used as a dictionary key
        # We can't use a list as it's not hashable
        key = tuple(count)
        
        # If this character count pattern is not in our dictionary yet, create a new list
        if key not in anagram_groups:
            anagram_groups[key] = []
            
        # Add the original string to its anagram group
        anagram_groups[key].append(s)
    
    # Convert dictionary values to a list of lists and return
    return list(anagram_groups.values())
```

## Hinglish Explanation

```
# Anagram solution ka explanation

# Pehla solution - Sorting wala tarika
# Is tarike mein hum har string ko sort kar dete hain
# Jaise "eat" ko sort karke "aet" ban jata hai
# Phir hum ek dictionary banate hain jahan sorted string key hoti hai
# Agar "eat" aur "tea" dono sort honge to dono "aet" ban jayenge
# Isi tarah se same anagrams ek hi group mein aa jayenge

# Dusra solution - Character counting wala tarika
# Is tarike mein hum har string ke characters ko count karte hain
# Jaise "eat" mein e=1, a=1, t=1 hai
# Phir is count ko key bana dete hain
# Agar do strings same characters se bane hain, to unka count bhi same hoga
# Yeh tarika sorting se jyada efficient hai agar strings badi hain

# Dono solutions ka time complexity O(N*K) hai, jahan N strings ki count hai 
# aur K maximum string length hai
# Lekin counting wala solution thoda faster hai kyunki sorting log(K) factor add karta hai
```

## Conclusion

Both solutions solve the problem efficiently but have different strengths:

1. **Sorting Solution**: Simpler to implement but slightly less efficient with O(N * K * log K) time complexity
2. **Counting Solution**: More efficient with O(N * K) time complexity but requires more code

For most practical purposes, the sorting solution is recommended due to its simplicity, unless you're dealing with very long strings where the log K factor becomes significant.

The best approach depends on:
- Input size (number of strings)
- Length of strings
- Character set size

For LeetCode specifically, both solutions should pass all test cases efficiently.

