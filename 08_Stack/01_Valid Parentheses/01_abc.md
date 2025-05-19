# 🔄 Valid Parentheses Solution with Enhanced ASCII Art Visualization 🔄

## 📋 Problem Statement

Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:
1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.

## 🎯 Solution Approach with Interactive ASCII Art Visualization

### 💡 Kaise Kaam Karta Hai? (How does it work?)

Stack data structure ka use karke hum brackets ko track kar sakte hain. Jab bhi 
opening bracket milta hai, use stack mein push karte hain. Jab closing bracket 
milta hai, hum stack se top element ko pop karke check karte hain ki kya woh 
matching opening bracket hai. Agar match nahi karta, to string valid nahi hai.

### 🔍 Initial State: The Problem Solver's Starting Point

```
+------------------------------------------------------+
|                     STARTING POINT                   |
+------------------------------------------------------+
|                                                      |
|  INPUT: String of brackets - e.g., "({[]})"          |
|                                                      |
|  +------------------------------------------+        |
|  |                                          |        |
|  |               EMPTY STACK                |        |
|  |                                          |        |
|  +------------------------------------------+        |
|                                                      |
|  BRACKET PAIRS:  ( )    { }    [ ]                   |
|                  └─┘    └─┘    └─┘                   |
|                 Must match in correct order!         |
|                                                      |
+------------------------------------------------------+
```

### 🎬 Example Visualization: Processing Valid Input "({[]})"

```
+------------------------------------------------------+
|                 PROCESSING: "({[]})"                 |
+------------------------------------------------------+

1️⃣ Character: '('  👉 Opening bracket - PUSH to stack
+------------------+        +------------------+
|                  |        |        (         |
|                  |   →    |                  |
|      STACK       |        |      STACK       |
|     (empty)      |        |                  |
+------------------+        +------------------+
   Input: ({[]})                 Remaining: {[]}

2️⃣ Character: '{'  👉 Opening bracket - PUSH to stack  
+------------------+        +------------------+
|        (         |        |        {         |
|                  |   →    |        (         |
|      STACK       |        |      STACK       |
|                  |        |                  |
+------------------+        +------------------+
   Remaining: {[]}               Remaining: []}

3️⃣ Character: '['  👉 Opening bracket - PUSH to stack
+------------------+        +------------------+
|        {         |        |        [         |
|        (         |   →    |        {         |
|      STACK       |        |      STACK       |
|                  |        |        (         |
+------------------+        +------------------+
   Remaining: []}                Remaining: ]}

4️⃣ Character: ']'  👉 Closing bracket - POP & MATCH
+------------------+        +------------------+
|        [         |        |        {         |
|        {         |   →    |        (         |
|        (         |        |      STACK       |
|      STACK       |        |                  |
+------------------+        +------------------+
   Remaining: ]}                 Remaining: }
   
   👍 MATCH CHECK: '[' matches ']' ✓ 
   
5️⃣ Character: '}'  👉 Closing bracket - POP & MATCH
+------------------+        +------------------+
|        {         |        |        (         |
|        (         |   →    |                  |
|      STACK       |        |      STACK       |
|                  |        |                  |
+------------------+        +------------------+
   Remaining: }                  Remaining: )
   
   👍 MATCH CHECK: '{' matches '}' ✓
   
6️⃣ Character: ')'  👉 Closing bracket - POP & MATCH
+------------------+        +------------------+
|        (         |        |                  |
|                  |   →    |                  |
|      STACK       |        |      STACK       |
|                  |        |     (empty)      |
+------------------+        +------------------+
   Remaining: )                  Remaining: ""
   
   👍 MATCH CHECK: '(' matches ')' ✓

+------------------------------------------------------+
|                 🎉 FINAL RESULT 🎉                   |
+------------------------------------------------------+
|                                                      |
|  ✅ VALID! Stack is empty and all brackets matched   |
|                                                      |
+------------------------------------------------------+
```

### 🚫 Example for Invalid Input: "([)]"

```
+------------------------------------------------------+
|                 PROCESSING: "([)]"                   |
+------------------------------------------------------+

1️⃣ Character: '('  👉 Opening bracket - PUSH to stack
+------------------+        +------------------+
|                  |        |        (         |
|                  |   →    |                  |
|      STACK       |        |      STACK       |
|     (empty)      |        |                  |
+------------------+        +------------------+
   Input: ([)]                   Remaining: [)]

2️⃣ Character: '['  👉 Opening bracket - PUSH to stack  
+------------------+        +------------------+
|        (         |        |        [         |
|                  |   →    |        (         |
|      STACK       |        |      STACK       |
|                  |        |                  |
+------------------+        +------------------+
   Remaining: [)]                Remaining: )]

3️⃣ Character: ')'  👉 Closing bracket - POP & MATCH
+------------------+        +------------------+
|        [         |        |                  |
|        (         |   →    |      ERROR!      |
|      STACK       |        |                  |
|                  |        |                  |
+------------------+        +------------------+
   Remaining: )]                 Remaining: ]
   
   ❌ MATCH CHECK: Popped '[' doesn't match ')' 
   Expected '(' but found '[' - Brackets don't match!

+------------------------------------------------------+
|                 ❌ FINAL RESULT ❌                   |
+------------------------------------------------------+
|                                                      |
|  INVALID! Brackets don't match in correct order      |
|                                                      |
+------------------------------------------------------+
```

### 🚫 Another Invalid Example: "((("

```
+------------------------------------------------------+
|                 PROCESSING: "((("                    |
+------------------------------------------------------+

1️⃣ Character: '('  👉 Opening bracket - PUSH to stack
+------------------+        +------------------+
|                  |        |        (         |
|                  |   →    |                  |
|      STACK       |        |      STACK       |
|     (empty)      |        |                  |
+------------------+        +------------------+
   Input: (((                    Remaining: ((

2️⃣ Character: '('  👉 Opening bracket - PUSH to stack  
+------------------+        +------------------+
|        (         |        |        (         |
|                  |   →    |        (         |
|      STACK       |        |      STACK       |
|                  |        |                  |
+------------------+        +------------------+
   Remaining: ((                  Remaining: (

3️⃣ Character: '('  👉 Opening bracket - PUSH to stack
+------------------+        +------------------+
|        (         |        |        (         |
|        (         |   →    |        (         |
|      STACK       |        |      STACK       |
|                  |        |        (         |
+------------------+        +------------------+
   Remaining: (                   Remaining: ""

+------------------------------------------------------+
|                 ❌ FINAL RESULT ❌                   |
+------------------------------------------------------+
|                                                      |
|  INVALID! Stack still contains unmatched brackets    |
|                                                      |
+------------------------------------------------------+
```

## 📝 Pseudocode with Detailed Annotation

```
function isValid(s):
    // Initialize an empty stack to store opening brackets
    stack = empty_stack()
    
    // Create a mapping of closing brackets to their corresponding opening brackets
    bracket_mapping = {
        ')': '(',
        '}': '{',
        ']': '['
    }
    
    // Process each character in the input string one by one
    for each character 'char' in s:
        
        // Check if current character is a closing bracket
        if char is in bracket_mapping keys:
            
            // If stack is empty, there's no matching opening bracket
            if stack is empty:
                return false
                
            // Pop the top element from stack (most recent opening bracket)
            top_element = stack.pop()
            
            // Check if popped element matches the expected opening bracket
            if bracket_mapping[char] != top_element:
                return false    // Brackets don't match - invalid!
        else:
            // Current character is an opening bracket, push onto stack
            stack.push(char)
    
    // After processing all characters, check if stack is empty
    // If empty, all brackets were properly matched; otherwise, unmatched brackets remain
    return stack is empty
```

## 💻 Python Implementation with Enhanced Documentation

```python
def isValid(s: str) -> bool:
    """
    Determines if a string containing only brackets is valid according to bracket matching rules.
    
    Time Complexity: O(n) where n is the length of the input string
    Space Complexity: O(n) in worst case when all characters are opening brackets
    
    Args:
        s: A string containing just the characters '(', ')', '{', '}', '[', ']'
        
    Returns:
        bool: True if all brackets are properly matched and nested, False otherwise
        
    Examples:
        >>> isValid("()")          # Simple matching pair
        True
        >>> isValid("()[]{}")      # Multiple matching pairs
        True
        >>> isValid("(]")          # Mismatched bracket types
        False
        >>> isValid("([)]")        # Correct types but wrong nesting order
        False
        >>> isValid("{[]}")        # Properly nested brackets
        True
        >>> isValid("")            # Empty string is valid
        True
        >>> isValid("((")          # Unmatched opening brackets
        False
    """
    # Initialize an empty stack for storing opening brackets
    stack = []
    
    # Create a mapping of closing brackets to their corresponding opening brackets
    bracket_map = {
        ')': '(',    # Round brackets pair
        '}': '{',    # Curly brackets pair
        ']': '['     # Square brackets pair
    }
    
    # Process each character in the string
    for char in s:
        # Check if current character is a closing bracket
        if char in bracket_map:
            # If stack is empty, there's no matching opening bracket
            if not stack:
                return False
                
            # Pop the top element from the stack
            top_element = stack.pop()
            
            # Check if the popped element matches the corresponding opening bracket
            if bracket_map[char] != top_element:
                return False  # Mismatch found - string is invalid
        else:
            # Current character is an opening bracket, push it onto the stack
            stack.append(char)
    
    # After processing all characters, the stack should be empty for a valid string
    # If stack has remaining elements, there are unmatched opening brackets
    return len(stack) == 0
```

## 🗣️ Hinglish Explanation with Detailed Comments

```python
def kya_valid_brackets_hai(s: str) -> bool:
    """
    Is function ka kaam ye check karna hai ki brackets sahi tarike se arranged hain ya nahi.
    
    Time Complexity: O(n) jahan 'n' input string ki length hai
    Space Complexity: O(n) worst case mein jab saare characters opening brackets hain
    
    Parameters:
        s: Ek string jisme sirf '(', ')', '{', '}', '[', ']' characters hain
        
    Returns:
        bool: True agar saare brackets sahi tarike se match karte hain, False agar nahi
        
    Examples:
        >>> kya_valid_brackets_hai("()")         # Simple matching pair
        True
        >>> kya_valid_brackets_hai("()[]{}")     # Multiple matching pairs
        True
        >>> kya_valid_brackets_hai("(]")         # Mismatched bracket types
        False
    """
    # Ek khali stack banayenge jisme opening brackets store karenge
    stack = []
    
    # Ek dictionary banayenge jo closing brackets ko unke matching opening brackets se map karega
    bracket_jodi = {
        ')': '(',    # Round brackets ki jodi
        '}': '{',    # Curly brackets ki jodi
        ']': '['     # Square brackets ki jodi
    }
    
    # String ke har character ko ek-ek karke check karenge
    for char in s:
        # Agar current character closing bracket hai
        if char in bracket_jodi:
            # Agar stack khali hai, matlab koi matching opening bracket nahi hai
            if not stack:
                return False
                
            # Stack se top element nikalenge (most recent opening bracket)
            upar_wala_element = stack.pop()
            
            # Check karenge ki nikala gaya element expected opening bracket hai ya nahi
            if bracket_jodi[char] != upar_wala_element:
                return False  # Brackets match nahi karte - invalid hai!
        else:
            # Agar opening bracket hai, to use stack mein push karenge
            stack.append(char)
    
    # Saare characters process karne ke baad, check karenge ki stack khali hai ya nahi
    # Agar khali hai, to saare brackets properly match ho gaye hain
    # Agar khali nahi hai, to kuch unmatched opening brackets reh gaye hain
    return len(stack) == 0
```

## 🧠 Understanding the Algorithm Using Stack Visualization

```
+------------------------------------------------------+
|           🔄 ALGORITHM MENTAL MODEL 🔄               |
+------------------------------------------------------+
|                                                      |
|  STACK DATA STRUCTURE:                               |
|  +------------------------------------------+        |
|  |                                          |        |
|  |  Think of stack like a stack of plates   |        |
|  |                                          |        |
|  |  - You can only add/remove from the top  |        |
|  |  - Last In, First Out (LIFO) principle   |        |
|  |                                          |        |
|  +------------------------------------------+        |
|                                                      |
|  WHY STACK WORKS FOR BRACKET MATCHING:               |
|                                                      |
|  1. Opening brackets get stacked in order            |
|     they appear                                      |
|                                                      |
|  2. When a closing bracket appears, it must match    |
|     the MOST RECENT opening bracket (top of stack)   |
|                                                      |
|  3. This naturally enforces the nested structure     |
|     of properly matched brackets                     |
|                                                      |
+------------------------------------------------------+
```

## 🔄 Another Visualization Approach with Emoji Brackets

```
Input: ({[]})

Processing step by step:

1. See ( → Push to stack → Stack: (
   👉 (⬜⬜⬜) → Current Position

2. See { → Push to stack → Stack: ({
   👉 (👉{⬜⬜) → Current Position

3. See [ → Push to stack → Stack: ({[
   👉 ({👉[⬜) → Current Position

4. See ] → Pop [ from stack → Stack: ({
   👉 ({[👉]) → Current Position → Match! ✓

5. See } → Pop { from stack → Stack: (
   👉 ({[]👉}) → Current Position → Match! ✓

6. See ) → Pop ( from stack → Stack: empty
   👉 ({[]})👉 → Current Position → Match! ✓

Stack is empty after processing all characters: VALID! ✅
```

## 🏋️ Edge Cases to Consider

```
+------------------------------------------------------+
|               🚨 IMPORTANT EDGE CASES 🚨             |
+------------------------------------------------------+
|                                                      |
|  1. EMPTY STRING: ""                                 |
|     → Result: Valid (stack starts and ends empty)    |
|                                                      |
|  2. ONLY CLOSING BRACKETS: ")]}"                     |
|     → Result: Invalid (nothing to match with)        |
|                                                      |
|  3. ONLY OPENING BRACKETS: "([{"                     |
|     → Result: Invalid (unmatched brackets remain)    |
|                                                      |
|  4. MIXED BRACKETS BUT WRONG ORDER: "([)]"           |
|     → Result: Invalid (nesting order violated)       |
|                                                      |
|  5. CORRECT BRACKETS BUT EXTRA CHARACTERS: "(a)"     |
|     → This problem assumes only bracket characters   |
|                                                      |
+------------------------------------------------------+
```

## 🔍 Time and Space Complexity Analysis

```
+------------------------------------------------------+
|               🧮 COMPLEXITY ANALYSIS 🧮              |
+------------------------------------------------------+
|                                                      |
|  TIME COMPLEXITY: O(n)                               |
|  ✓ We process each character in the string exactly   |
|    once                                              |
|  ✓ Each stack operation (push/pop) takes O(1) time   |
|  ✓ Dictionary lookups for bracket matching take O(1) |
|                                                      |
|  SPACE COMPLEXITY: O(n)                              |
|  ✓ Worst case: All characters are opening brackets   |
|    and get pushed onto the stack                     |
|  ✓ Best case: Alternating opening/closing brackets   |
|    keeps stack size around O(n/2), which is O(n)     |
|                                                      |
+------------------------------------------------------+
```

## 💪 Alternate Solution: Brute Force Approach

```python
def isValidBruteForce(s: str) -> bool:
    """
    Solves the valid parentheses problem using a brute force approach.
    
    This approach repeatedly finds and removes the innermost valid pairs of brackets
    until no more valid pairs can be removed or the string becomes empty.
    
    Time Complexity: O(n²) where n is the length of the string
    Space Complexity: O(n) for string manipulations
    
    Args:
        s: A string containing just bracket characters
        
    Returns:
        bool: True if the string contains valid brackets, False otherwise
    """
    # If the length is odd, it cannot be valid
    if len(s) % 2 != 0:
        return False
        
    # Define valid bracket pairs
    valid_pairs = ["()", "[]", "{}"]
    
    # Keep removing valid pairs until no changes can be made
    previous_length = -1
    current_length = len(s)
    
    # Continue until no more valid pairs can be removed
    while previous_length != current_length:
        previous_length = current_length
        
        # Remove all valid pairs
        for pair in valid_pairs:
            s = s.replace(pair, "")
            
        current_length = len(s)
    
    # If the string is empty, all brackets were valid and matched
    return len(s) == 0
```

## 🏁 Conclusion

Is problem ko solve karne ke liye stack approach sabse efficient hai kyunki:

1. Stack data structure naturally implements the LIFO (Last In, First Out) property,
   jo bracket matching ke liye perfect hai - last opened bracket must be first closed.

2. Time complexity O(n) hai, jo linear hai aur efficient hai.

3. Code simple aur readable hai, making it easy to understand and implement.

4. Edge cases jaise empty string, unbalanced brackets, aur wrong nesting order
   ko bhi handle kar leta hai.

Stack data structure ka use karke, hum is problem ko efficiently aur intuitively
solve kar sakte hain, jisse code maintainable aur performant rehta hai. 🎯
