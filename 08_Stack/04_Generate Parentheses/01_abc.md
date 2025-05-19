# Generate Parentheses Solutions

## Problem Statement
Given an integer n, generate all well-formed parentheses strings with n pairs of parentheses.

## ASCII Art Visualization of the Problem

```
       START
         |
         v
    n = 2 pairs
         |
         v
    +----------+
    | Generate  |
    | all valid |
    | sequences |
    +----------+
         |
         v
    +---------+
    | Results  |
    |  "()()"  |
    |  "(())"  |
    +---------+
         |
         v
        END
```

## Solution 1: Brute Force Approach

### ASCII Art for Brute Force Approach
```
               START
                 |
                 v
          Generate ALL possible
          combinations of '(' and ')'
                 |
                 v
             /       \
          /             \
      Valid?           Invalid?
        |                 |
        v                 v
    Keep it            Discard
        |
        v
     Results
```

### Brute Force Pseudo Code
```
function generateParenthesis(n):
    results = []
    generateAll([], 0, 2*n, results)
    return results

function generateAll(current, position, max_length, results):
    if position == max_length:
        if isValid(current):
            results.add(current as string)
        return
    
    current[position] = '('
    generateAll(current, position+1, max_length, results)
    current[position] = ')'
    generateAll(current, position+1, max_length, results)

function isValid(string):
    balance = 0
    for char in string:
        if char == '(':
            balance++
        else:
            balance--
        if balance < 0:
            return false
    return balance == 0
```

### Brute Force Python Code
```python
def generateParenthesis(n):
    """
    Har possible combination banao aur check karo ki valid hai ya nahi.
    Ye brute force approach hai jo saare combinations try karta hai.
    Time complexity O(2^(2n) * n) hai kyunki 2n positions par 2 choices hai.
    
    Args:
        n: Number of pairs of parentheses
        
    Returns:
        List of all valid parentheses combinations
    """
    def generate_all(current, pos, max_len, result):
        # Base case: agar position max length ke equal ho gayi hai
        if pos == max_len:
            # Check karo ki jo combination bana hai wo valid hai ya nahi
            if is_valid(current):
                result.append(''.join(current))
            return
            
        # Recursively try '(' at current position
        current[pos] = '('
        generate_all(current, pos + 1, max_len, result)
        
        # Recursively try ')' at current position
        current[pos] = ')'
        generate_all(current, pos + 1, max_len, result)
    
    def is_valid(string):
        # Check if string is valid parentheses sequence
        balance = 0
        for c in string:
            if c == '(':
                balance += 1
            else:
                balance -= 1
            # Agar kabhi bhi closing brackets zyada ho gaye to invalid hai
            if balance < 0:
                return False
        # End mein balance 0 hona chahiye - matlab saare brackets close hue
        return balance == 0
    
    result = []
    generate_all([''] * (2 * n), 0, 2 * n, result)
    return result
```

## Solution 2: Backtracking Approach

### ASCII Art for Backtracking
```
                          START (n=2)
                             |
                   +---------v---------+
                   | open=0, close=0   |
                   | current=""        |
                   +---------+---------+
                             |
            +----------------+------------------+
            |                                   |
     +------v-------+                   +------v-------+
     | Add '('      |                   | Add ')'      |
     | open < n ?   |                   | close < open?|
     | Yes: open++ --|                  | No: Invalid  |
     +------+-------+                   +-------------+
            |                        
   +--------+---------+                    
   |                  |                  
+--v---+          +---v--+          
|"("   |          |"("   |          
|1,0   |          |1,0   |          
+--+---+          +--+---+          
   |                 |              
   |      +----------+----------+   
   |      |                     |   
+--v---+ |                  +---v--+ 
|"(("  | | Add ')'          |"()"  | 
|2,0   | | close < open?    |1,1   | 
+--+---+ | Yes: close++     +--+---+ 
   |     |                     |     
   |     |                     |     
   |  +--v---+              +--v---+ 
   |  |"(()" |              |"()(" | 
   |  |2,1   |              |2,1   | 
   |  +--+---+              +--+---+ 
   |     |                     |     
   |     |                     |     
   |  +--v---+              +--v---+ 
   |  |"(())"| ✓            |"()()"| ✓
   |  |2,2   |              |2,2   | 
   |  +------+              +------+ 
   |                           
 RESULT: ["(())", "()()"]
```

### Backtracking Pseudo Code
```
function generateParenthesis(n):
    results = []
    backtrack(results, "", 0, 0, n)
    return results

function backtrack(results, current, open, close, max):
    if current.length == max * 2:
        results.add(current)
        return
    
    if open < max:
        backtrack(results, current + "(", open + 1, close, max)
    
    if close < open:
        backtrack(results, current + ")", open, close + 1, max)
```

### Backtracking Python Code
```python
def generateParenthesis(n):
    """
    Backtracking approach se valid parentheses generate karo.
    Is approach mein hum sirf valid combinations ki taraf aage badhte hain.
    Time complexity O(4^n / sqrt(n)) hai jo Catalan number se related hai.
    
    Args:
        n: Number of pairs of parentheses
        
    Returns:
        List of all valid parentheses combinations
    """
    result = []
    
    def backtrack(current_string, open_count, close_count):
        # Base case: agar string ki length 2*n ho gayi to result mein add karo
        if len(current_string) == 2 * n:
            result.append(current_string)
            return
            
        # Agar abhi tak open brackets n se kam hain to add kar sakte hain
        if open_count < n:
            backtrack(current_string + '(', open_count + 1, close_count)
            
        # Agar close brackets open brackets se kam hain to close kar sakte hain
        if close_count < open_count:
            backtrack(current_string + ')', open_count, close_count + 1)
    
    # Initial call with empty string and zero counts
    backtrack('', 0, 0)
    return result
```

## Solution 3: Dynamic Programming Approach

### ASCII Art for Dynamic Programming
```
                    START (n=2)
                       |
                       v
          +-------------------------+
          | Calculate for n=0      |
          | DP[0] = [""]           |
          +-------------------------+
                       |
                       v
          +-------------------------+
          | Calculate for n=1      |
          | DP[1] = ["()"]         |
          +-------------------------+
                       |
                       v
     +------------------------------------+
     | Calculate for n=2                  |
     | For each way to insert () in DP[1] |
     | and for each way to combine DP[0] and DP[1] |
     +------------------------------------+
                       |
                       v
              +----------------+
              | DP[2] = ["()()", "()()"] |
              +----------------+
                       |
                       v
                     RESULT
```

### Dynamic Programming Pseudo Code
```
function generateParenthesis(n):
    dp = [[] for _ in range(n + 1)]
    dp[0] = [""]
    
    for i from 1 to n:
        for j from 0 to i-1:
            for left in dp[j]:
                for right in dp[i-j-1]:
                    dp[i].append("(" + left + ")" + right)
    
    return dp[n]
```

### Dynamic Programming Python Code
```python
def generateParenthesis(n):
    """
    Dynamic Programming approach se valid parentheses generate karo.
    Is approach mein hum smaller subproblems ke solutions ko combine karte hain.
    Time complexity O(4^n / sqrt(n)) hai jo Catalan number se related hai.
    
    Args:
        n: Number of pairs of parentheses
        
    Returns:
        List of all valid parentheses combinations
    """
    # DP array to store results for each subproblem from 0 to n
    dp = [[] for _ in range(n + 1)]
    
    # Base case: empty string for 0 pairs
    dp[0] = [""]
    
    # Fill DP array bottom-up for each number of pairs
    for i in range(1, n + 1):
        # For each pair, consider all ways to insert the new pair
        for j in range(i):
            # Get all valid sequences with j pairs for left part inside new pair
            for left in dp[j]:
                # Get all valid sequences with (i-j-1) pairs for right part
                for right in dp[i - j - 1]:
                    # Construct new sequence: (left) + right
                    dp[i].append("(" + left + ")" + right)
    
    # Return all valid combinations for n pairs
    return dp[n]
```

## Emoji Art Representation of the Solution Process

```
🎮 Generate Parentheses Problem 🎮
       ↓
🔍 Input: n = 2
       ↓
🧠 Processing...
       ↓
🔄 Backtracking Tree:
       ""
      /  \
    "("   ❌
    / \
"(("  "()"
 /      /  \
"(()" "()()"
 /
"(())"
       ↓
🎯 Result: ["(())", "()()"]
```

## Time and Space Complexity Comparison

| Approach | Time Complexity | Space Complexity |
|----------|----------------|-----------------|
| Brute Force | O(2^(2n) * n) | O(2n) |
| Backtracking | O(4^n / sqrt(n)) | O(n) |
| Dynamic Programming | O(4^n / sqrt(n)) | O(4^n / sqrt(n)) |

## Conclusion

The backtracking approach is the most efficient for this problem as it directly builds only valid sequences rather than generating all possibilities. The dynamic programming approach also works well but requires more memory. The brute force approach is the least efficient as it generates all possible combinations before filtering the valid ones.

