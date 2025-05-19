# Daily Temperatures Problem Solution

## Problem Statement
Given an array of integers `temperatures` representing daily temperatures, return an array `answer` such that `answer[i]` is the number of days you have to wait after the ith day to get a warmer temperature. If there is no future day for which this is possible, keep `answer[i] = 0`.

## Visual Representation of the Problem

```
Input temperatures: [73, 74, 75, 71, 69, 72, 76, 73]
```

### ASCII Art Visualization

```
Temperature Timeline:
Day:     [0]    [1]    [2]    [3]    [4]    [5]    [6]    [7]
Temp:    73°    74°    75°    71°    69°    72°    76°    73°
         ▆▆▆    ▆▆▆▆   ▆▆▆▆▆  ▆▆▆    ▆▆     ▆▆▆▆   ▆▆▆▆▆▆ ▆▆▆
         
Answer:   1      1      4      2      1      1      0      0
         ↓      ↓      ↓      ↓      ↓      ↓      ↓      ↓
         74°    75°    76°    72°    72°    76°    None   None
         
Days to Wait: [1, 1, 4, 2, 1, 1, 0, 0]
```

### Emoji Visualization of Solution Process

```
Day 0: 🌡️(73°) → Looking ahead → 🌡️(74°) found at day 1 → Wait 1 day
Day 1: 🌡️(74°) → Looking ahead → 🌡️(75°) found at day 2 → Wait 1 day
Day 2: 🌡️(75°) → Looking ahead → 🌡️(76°) found at day 6 → Wait 4 days
Day 3: 🌡️(71°) → Looking ahead → 🌡️(72°) found at day 5 → Wait 2 days
Day 4: 🌡️(69°) → Looking ahead → 🌡️(72°) found at day 5 → Wait 1 day
Day 5: 🌡️(72°) → Looking ahead → 🌡️(76°) found at day 6 → Wait 1 day
Day 6: 🌡️(76°) → Looking ahead → No higher temp found → Wait 0 days
Day 7: 🌡️(73°) → Looking ahead → No higher temp found → Wait 0 days
```

## Solution 1: Brute Force Approach

### Visual Representation

```
For each day i, we check every future day j until we find a warmer day:

Day 0 (73°): Check day 1 (74°) ✓ → Answer[0] = 1
             │
             └── Found warmer temperature right away!

Day 1 (74°): Check day 2 (75°) ✓ → Answer[1] = 1
             │
             └── Found warmer temperature right away!

Day 2 (75°): Check day 3 (71°) ✗ → Keep searching
             Check day 4 (69°) ✗ → Keep searching
             Check day 5 (72°) ✗ → Keep searching
             Check day 6 (76°) ✓ → Answer[2] = 4
             │
             └── Had to check 4 days ahead to find warmer temperature!

Day 3 (71°): Check day 4 (69°) ✗ → Keep searching
             Check day 5 (72°) ✓ → Answer[3] = 2
             │
             └── Found warmer temperature 2 days ahead!

... and so on
```

### Pseudo Code (Brute Force)

```
function dailyTemperaturesBruteForce(temperatures):
    n = length of temperatures
    answer = array of size n filled with 0s
    
    for i from 0 to n-1:
        for j from i+1 to n-1:
            if temperatures[j] > temperatures[i]:
                answer[i] = j - i
                break
                
    return answer
```

### Python Code (Brute Force)

```python
def dailyTemperaturesBruteForce(temperatures):
    """
    Brute force approach to find the number of days to wait for warmer temperature.
    
    Iss approach mein, har din ke liye, hum aage ke sabhi dino ko check karte hain
    jab tak ki hume ek aisa din nahi milta jiska temperature current din se zyada
    ho. Jab mil jata hai to wait karne wale dino ki sankhya calculate kar lete hain.
    
    Args:
        temperatures: List of daily temperatures
        
    Returns:
        List where each element represents days to wait for a warmer temperature
    """
    n = len(temperatures)
    answer = [0] * n  # Initialize answer array with zeros
    
    for i in range(n):
        for j in range(i + 1, n):
            if temperatures[j] > temperatures[i]:
                answer[i] = j - i  # Store the number of days to wait
                break  # Found the first warmer day, move to next day
                
    return answer
```

## Solution 2: Stack Approach

### Visual Representation

```
We use a stack to keep track of indices of days with temperatures that we haven't 
found a warmer day for yet.

Stack: [Day Index, Temperature]

Day 0 (73°): Push (0, 73°) to stack
             Stack: [(0, 73°)]

Day 1 (74°): 74° > 73° ✓
             Pop (0, 73°) from stack, Answer[0] = 1 - 0 = 1
             Push (1, 74°) to stack
             Stack: [(1, 74°)]

Day 2 (75°): 75° > 74° ✓
             Pop (1, 74°) from stack, Answer[1] = 2 - 1 = 1
             Push (2, 75°) to stack
             Stack: [(2, 75°)]

Day 3 (71°): 71° < 75° ✗
             Push (3, 71°) to stack
             Stack: [(2, 75°), (3, 71°)]

Day 4 (69°): 69° < 71° ✗
             Push (4, 69°) to stack
             Stack: [(2, 75°), (3, 71°), (4, 69°)]

Day 5 (72°): 72° > 69° ✓
             Pop (4, 69°) from stack, Answer[4] = 5 - 4 = 1
             72° > 71° ✓
             Pop (3, 71°) from stack, Answer[3] = 5 - 3 = 2
             72° < 75° ✗
             Push (5, 72°) to stack
             Stack: [(2, 75°), (5, 72°)]

... and so on
```

### Pseudo Code (Stack)

```
function dailyTemperaturesStack(temperatures):
    n = length of temperatures
    answer = array of size n filled with 0s
    stack = empty stack (will store indices)
    
    for i from 0 to n-1:
        while stack is not empty AND temperatures[i] > temperatures[stack.top()]:
            prevIndex = stack.pop()
            answer[prevIndex] = i - prevIndex
        stack.push(i)
                
    return answer
```

### Python Code (Stack)

```python
def dailyTemperaturesStack(temperatures):
    """
    Stack approach to find the number of days to wait for warmer temperature.
    
    Iss approach mein, hum ek stack ka use karte hain jisme un dino ke indices
    store karte hain jinke liye abhi tak koi warmer temperature nahi mila hai.
    Jab bhi current temperature stack ke top par stored temperature se bada hota
    hai, hum stack se pop karke answer update karte hain.
    
    Args:
        temperatures: List of daily temperatures
        
    Returns:
        List where each element represents days to wait for a warmer temperature
    """
    n = len(temperatures)
    answer = [0] * n
    stack = []  # Stack to store indices of temperatures
    
    for i in range(n):
        # While stack is not empty and current temp is greater than temp at stack top
        while stack and temperatures[i] > temperatures[stack[-1]]:
            prev_idx = stack.pop()  # Get the index of the cooler temperature
            answer[prev_idx] = i - prev_idx  # Calculate days to wait
        
        stack.append(i)  # Push current index to stack
    
    return answer
```

## Solution 3: Dynamic Programming Approach

### Visual Representation

```
We start from the right and work our way to the left. For each day, we either find
a warmer day immediately to the right, or we jump using our previous calculations.

Processing from right to left:

Day 7 (73°): No days to the right → Answer[7] = 0
Day 6 (76°): No warmer day to the right → Answer[6] = 0
Day 5 (72°): Day 6 (76°) is warmer → Answer[5] = 1
Day 4 (69°): Day 5 (72°) is warmer → Answer[4] = 1
Day 3 (71°): Jump pattern:
             │
             └── Check Day 4 (69°) ✗ Not warmer
                 Jump using Answer[4] = 1 to Day 5 (72°) ✓ Warmer!
                 Answer[3] = 2

Day 2 (75°): Jump pattern:
             │
             └── Check Day 3 (71°) ✗ Not warmer
                 Jump using Answer[3] = 2 to Day 5 (72°) ✗ Not warmer
                 Jump using Answer[5] = 1 to Day 6 (76°) ✓ Warmer!
                 Answer[2] = 4

... and so on
```

### Pseudo Code (Dynamic Programming)

```
function dailyTemperaturesDP(temperatures):
    n = length of temperatures
    answer = array of size n filled with 0s
    
    for i from n-2 down to 0:
        next_idx = i + 1
        while next_idx < n and temperatures[next_idx] <= temperatures[i]:
            if answer[next_idx] == 0:
                next_idx = n  # No warmer day found for next_idx
            else:
                next_idx = next_idx + answer[next_idx]  # Jump using previous result
        
        if next_idx < n:
            answer[i] = next_idx - i
                
    return answer
```

### Python Code (Dynamic Programming)

```python
def dailyTemperaturesDP(temperatures):
    """
    Dynamic Programming approach to find days to wait for warmer temperature.
    
    Iss approach mein, hum right se left ki taraf process karte hain. Har din
    ke liye, ya to turant agle din warmer temperature milta hai, ya phir hum
    pehle se calculate kiye gaye results ka use karke jump karte hain.
    
    Args:
        temperatures: List of daily temperatures
        
    Returns:
        List where each element represents days to wait for a warmer temperature
    """
    n = len(temperatures)
    answer = [0] * n
    
    # Process backwards from the second last day
    for i in range(n - 2, -1, -1):
        next_idx = i + 1
        
        # Keep jumping to the right until a warmer temperature is found
        while next_idx < n and temperatures[next_idx] <= temperatures[i]:
            if answer[next_idx] == 0:  # No warmer day found for next_idx
                next_idx = n  # Exit the loop
                break
            next_idx += answer[next_idx]  # Jump using previous result
        
        # If a warmer temperature was found, calculate days to wait
        if next_idx < n:
            answer[i] = next_idx - i
    
    return answer
```

## Time and Space Complexity Analysis

| Approach | Time Complexity | Space Complexity |
|----------|----------------|-----------------|
| Brute Force | O(n²) | O(1) extra space |
| Stack | O(n) | O(n) for the stack |
| Dynamic Programming | O(n) | O(1) extra space |

## Solution Evolution Visualization

```
Approach Evolution:
Brute Force → Stack → Dynamic Programming
O(n²)       → O(n)  → O(n) with less space

     ┌───────────┐
     │Brute Force│
     └─────┬─────┘
           │  Check all days for each day
           ▼
     ┌───────────┐
     │   Stack   │
     └─────┬─────┘
           │  Use stack to track unresolved days
           ▼
┌────────────────────┐
│Dynamic Programming │
└────────────────────┘
     Use previous results to avoid recalculation
```

## Conclusion

The stack approach is generally the most recommended solution for this problem because:
1. It has optimal time complexity O(n)
2. It processes the array in a single forward pass
3. It's intuitive once you understand the pattern

The dynamic programming approach is also O(n) but requires more complex logic and might be 
harder to understand initially.

The brute force approach is simpler to understand but becomes inefficient for large inputs
due to its O(n²) time complexity.

