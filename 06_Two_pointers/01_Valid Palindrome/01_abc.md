# Valid Palindrome Solution

## Problem Statement

**Easy**

Given a string `s`, return `true` if it is a **palindrome**, otherwise return `false`.

A palindrome is a string that reads the same forward and backward. It is also case-insensitive and ignores all non-alphanumeric characters.

## ASCII Art Illustration

### Two-Pointer Approach Visualization

```
Input string: "A man, a plan, a canal: Panama"

Step 1: Initialize two pointers (left and right)
                                    
     left                                                      right
      ↓                                                          ↓
    [ A | m | a | n | , |   | a |   | p | l | a | n | ... | a | m | a ]
                                                                      
    Comparison: 'A' vs 'a' ✓ (case-insensitive match)
    
Step 2: Move pointers (left++ and right--)
                                    
         left                                                right
          ↓                                                    ↓
    [ A | m | a | n | , |   | a |   | p | l | a | n | ... | a | m | a ]
                                                                      
    Comparison: 'm' vs 'm' ✓ (match)
    
Step 3: Continue moving pointers
                                    
             left                                          right
              ↓                                              ↓
    [ A | m | a | n | , |   | a |   | p | l | a | n | ... | a | n | a ]
                                                                      
    Comparison: 'a' vs 'a' ✓ (match)
    
Step 4: Skip non-alphanumeric characters
                                    
                 left                                    right
                  ↓                                        ↓
    [ A | m | a | n | , |   | a |   | p | l | a | n | ... | a | n | a ]
                                                                      
    Character ',' is not alphanumeric, skip left pointer
    
                     left                                right
                      ↓                                    ↓
    [ A | m | a | n | , |   | a |   | p | l | a | n | ... | a | n | a ]
                                                                      
    Comparison: ' ' is not alphanumeric, skip left pointer
    
...and so on until pointers meet or cross each other...

Final state:
                                    
                                 left right
                                   ↓   ↓
    [ A | m | a | n | , |   | a |   | p | l | a | n | ... | a | n | a ]
                                                                      
    Pointers have crossed each other and all comparisons matched ✓
    
Result: True (It is a palindrome)
```

## Pseudocode

### Two-Pointer Approach

```
function isPalindrome(s):
    # Convert string to lowercase
    s = convert s to lowercase
    
    # Initialize two pointers
    left = 0
    right = length of s - 1
    
    # Move pointers towards each other until they meet or cross
    while left < right:
        # Skip non-alphanumeric characters from left
        while left < right AND s[left] is not alphanumeric:
            left = left + 1
        
        # Skip non-alphanumeric characters from right
        while left < right AND s[right] is not alphanumeric:
            right = right - 1
        
        # Compare characters at current pointer positions
        if s[left] != s[right]:
            return False  # Not a palindrome
        
        # Move pointers inward
        left = left + 1
        right = right - 1
    
    # If we've checked all characters and all matched, it's a palindrome
    return True
```

### Reverse String Approach

```
function isPalindrome(s):
    # Extract only alphanumeric characters and convert to lowercase
    cleanStr = ""
    for each character c in s:
        if c is alphanumeric:
            cleanStr = cleanStr + convert c to lowercase
    
    # Check if the string is equal to its reverse
    return cleanStr equals reverse of cleanStr
```

## Python Implementation

### Two-Pointer Approach

```python
def isPalindrome_two_pointers(s):
    """
    Determine if a string is a palindrome using two-pointer approach.
    
    A palindrome is a string that reads the same forward and backward.
    Case-insensitive and ignores all non-alphanumeric characters.
    
    Args:
        s (str): Input string to check
        
    Returns:
        bool: True if the string is a palindrome, False otherwise
        
    Time Complexity: O(n) where n is the length of the string
    Space Complexity: O(1) as we only use two pointers
    """
    # Convert string ke lowercase for case-insensitive comparison
    s = s.lower()
    
    # Initialize left aur right pointers string ke dono ends pe
    left, right = 0, len(s) - 1
    
    # Jab tak dono pointers meet nahi karte ya cross nahi karte
    while left < right:
        # Skip non-alphanumeric characters from left side
        while left < right and not s[left].isalnum():
            left += 1
        
        # Skip non-alphanumeric characters from right side
        while left < right and not s[right].isalnum():
            right -= 1
        
        # Compare characters at current positions
        if s[left] != s[right]:
            return False  # Not a palindrome
        
        # Move pointers inward for next comparison
        left += 1
        right -= 1
    
    # Agar sab characters match kar gaye, to string palindrome hai
    return True
```

### Reverse String Approach

```python
def isPalindrome_reverse(s):
    """
    Determine if a string is a palindrome using reverse string approach.
    
    A palindrome is a string that reads the same forward and backward.
    Case-insensitive and ignores all non-alphanumeric characters.
    
    Args:
        s (str): Input string to check
        
    Returns:
        bool: True if the string is a palindrome, False otherwise
        
    Time Complexity: O(n) where n is the length of the string
    Space Complexity: O(n) as we create a new filtered string
    """
    # Extract only alphanumeric characters aur convert to lowercase
    filtered_chars = [c.lower() for c in s if c.isalnum()]
    
    # Join characters to create a clean string
    filtered_string = ''.join(filtered_chars)
    
    # Check if the string equals its reverse
    return filtered_string == filtered_string[::-1]
```

## Comparison of Approaches

| Approach | Time Complexity | Space Complexity | Advantages | Disadvantages |
|----------|----------------|-----------------|------------|---------------|
| Two Pointers | O(n) | O(1) | Memory efficient, processes string in-place | Slightly more complex logic |
| Reverse String | O(n) | O(n) | Simpler to implement and understand | Requires extra space to store filtered string |

## Example Test Cases

1. "A man, a plan, a canal: Panama" → `True`
2. "race a car" → `False`
3. " " → `True` (A single space is a palindrome as it has no alphanumeric characters)
4. "Madam, I'm Adam" → `True` (Ignoring case and non-alphanumeric characters)

## Conclusion

The two-pointer approach is generally preferred for this problem as it has O(1) space complexity compared to O(n) for the reverse string approach. However, the reverse string approach might be more intuitive for beginners to understand.

Both solutions correctly handle the requirements:
- Case insensitivity
- Ignoring non-alphanumeric characters
- Checking if the string reads the same forward and backward

