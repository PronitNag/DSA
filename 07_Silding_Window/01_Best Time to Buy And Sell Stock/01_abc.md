# Best Time to Buy and Sell Stock - Solution with ASCII Art

## Problem Statement

**NeetCoin ka price har din badalta rehta hai. Aapko ek array `prices` diya gaya hai jisme `prices[i]` NeetCoin ka price `i`th din par hai.**

**Aapko ek din chun-na hai NeetCoin khareedne ke liye aur future mein kisi din bechne ke liye, taki maximum profit mile.**

**Agar koi transaction faydemand na ho, to aap koi transaction na karein aur profit `0` ho.**

## Visualization of the Problem

Maan lijiye humara price array hai: `[7, 1, 5, 3, 6, 4]`

```
Stock Price Over Time:
                                         
                                        +---+
                             +---+      |   |
      +---+                  |   |      |   |      +---+
      |   |                  |   |      |   |      |   |
      |   |                  |   |      |   |      |   |
      |   |                  |   | +---+|   |      |   |
      |   |                  |   | |   ||   |      |   |
      |   |                  |   | |   ||   |      |   |
      |   |        +---+     |   | |   ||   |      |   |
      |   |        |   |     |   | |   ||   |      |   |
------+---+--------+---+-----+---+-+---++---+------+---+----> Time
Day:    0          1         2     3     4         5
Price:  7          1         5     3     6         4
        ^          ^                     ^
        |          |                     |
        |          +---------------------+
        |            Best Buy & Sell     |
        |            Profit: 6-1 = 5     |
        Not optimal to buy               
```

## Solution 1: Brute Force Approach

### ASCII Art Visualization

```
 For each possible buy day (i), check each possible sell day (j) after it
 
 +---+---+---+---+---+---+   Start: Check all possible buy-sell combinations
 | 7 | 1 | 5 | 3 | 6 | 4 |   
 +---+---+---+---+---+---+
   ^
   | Buy?
   
 +---+---+---+---+---+---+   Then check profit for each future day
 | 7 | 1 | 5 | 3 | 6 | 4 |   Profit = Future Price - Buy Price
 +---+---+---+---+---+---+
   ^   ^
   |   | Sell on day 1? Profit = 1-7 = -6 (No profit)
   |
   Buy
   
 +---+---+---+---+---+---+   
 | 7 | 1 | 5 | 3 | 6 | 4 |   
 +---+---+---+---+---+---+
   ^       ^
   |       | Sell on day 2? Profit = 5-7 = -2 (No profit)
   |
   Buy
   
   ...and so on for all combinations...
   
 +---+---+---+---+---+---+   Eventually find the best combination:
 | 7 | 1 | 5 | 3 | 6 | 4 |   Buy at price 1, sell at price 6
 +---+---+---+---+---+---+
       ^           ^
       |           | 
       |           Sell (price = 6)
       |
       Buy (price = 1)
       
 Maximum Profit = 6 - 1 = 5
```

### Pseudo Code (Brute Force)
```
function maxProfit(prices):
    max_profit = 0
    
    for i from 0 to length(prices)-1:  # Buy day
        for j from i+1 to length(prices)-1:  # Sell day
            current_profit = prices[j] - prices[i]
            max_profit = maximum(max_profit, current_profit)
    
    return max_profit
```

### Python Code (Brute Force)
```python
def maxProfit_bruteforce(prices):
    """
    Brute Force approach mein hum har possible buy day ke liye, uske baad ke
    har possible sell day ko check karte hain aur maximum profit dhoondte hain.
    
    Time Complexity: O(n²) - Kyunki humne har buy day ke liye saare aage ke 
    din check kiye
    Space Complexity: O(1) - Sirf fixed variables use kiye
    
    Args:
        prices (list): Array jisme har index par uss din ka stock price hai
        
    Returns:
        int: Maximum possible profit, ya 0 agar profit possible na ho
    """
    max_profit = 0
    n = len(prices)
    
    for buy_day in range(n):
        for sell_day in range(buy_day + 1, n):
            current_profit = prices[sell_day] - prices[buy_day]
            max_profit = max(max_profit, current_profit)
    
    return max_profit
```

## Solution 2: Two Pointers / One Pass Approach

### ASCII Art Visualization

```
Keep track of minimum price so far and maximum profit possible
 
 +---+---+---+---+---+---+   Start: Initialize min_price = first element
 | 7 | 1 | 5 | 3 | 6 | 4 |          max_profit = 0
 +---+---+---+---+---+---+
   ^
   | min_price = 7
   
 +---+---+---+---+---+---+   Step 1: Check price[1] = 1
 | 7 | 1 | 5 | 3 | 6 | 4 |   1 < min_price, so min_price = 1
 +---+---+---+---+---+---+   profit = 1-1 = 0, max_profit still 0
       ^
       | min_price = 1
   
 +---+---+---+---+---+---+   Step 2: Check price[2] = 5
 | 7 | 1 | 5 | 3 | 6 | 4 |   profit = 5-1 = 4 > max_profit
 +---+---+---+---+---+---+   max_profit = 4
       ^   ^
       |   | Current price
       |
       min_price still 1
   
 +---+---+---+---+---+---+   Step 3: Check price[3] = 3
 | 7 | 1 | 5 | 3 | 6 | 4 |   profit = 3-1 = 2 < max_profit
 +---+---+---+---+---+---+   max_profit still 4
       ^       ^
       |       | Current price
       |
       min_price still 1
       
 +---+---+---+---+---+---+   Step 4: Check price[4] = 6
 | 7 | 1 | 5 | 3 | 6 | 4 |   profit = 6-1 = 5 > max_profit
 +---+---+---+---+---+---+   max_profit = 5
       ^           ^
       |           | Current price
       |
       min_price still 1
       
 +---+---+---+---+---+---+   Step 5: Check price[5] = 4
 | 7 | 1 | 5 | 3 | 6 | 4 |   profit = 4-1 = 3 < max_profit
 +---+---+---+---+---+---+   max_profit still 5
       ^               ^
       |               | Current price
       |
       min_price still 1
       
 Final answer: max_profit = 5
```

### Pseudo Code (Two Pointers)
```
function maxProfit(prices):
    if prices is empty:
        return 0
        
    min_price = prices[0]
    max_profit = 0
    
    for price in prices:
        min_price = minimum(min_price, price)
        current_profit = price - min_price
        max_profit = maximum(max_profit, current_profit)
    
    return max_profit
```

### Python Code (Two Pointers)
```python
def maxProfit_twopointers(prices):
    """
    Two Pointers approach mein hum ek hi baar array traverse karte hain. Hum
    abhi tak dekhe gaye minimum price ko track karte hain aur har step par
    check karte hain ki current price se kitna profit ho sakta hai.
    
    Time Complexity: O(n) - Array ko sirf ek baar traverse kiya
    Space Complexity: O(1) - Sirf fixed variables use kiye
    
    Args:
        prices (list): Array jisme har index par uss din ka stock price hai
        
    Returns:
        int: Maximum possible profit, ya 0 agar profit possible na ho
    """
    if not prices:  # Agar array khaali hai
        return 0
        
    min_price = float('inf')  # Shuru mein minimum price infinity maan lete hain
    max_profit = 0
    
    for price in prices:
        # Update karo minimum price ko
        min_price = min(min_price, price)
        
        # Check karo current price se kitna profit ho sakta hai
        current_profit = price - min_price
        
        # Update karo maximum profit ko
        max_profit = max(max_profit, current_profit)
    
    return max_profit
```

## Solution 3: Dynamic Programming Approach

### ASCII Art Visualization

```
DP approach - Calculate and store the maximum profit up to each day
 
 +---+---+---+---+---+---+   Initial state: First day has 0 profit
 | 7 | 1 | 5 | 3 | 6 | 4 |   dp[0] = 0
 +---+---+---+---+---+---+   min_price = 7
   ^
   |
   
 +---+---+---+---+---+---+   Day 1: Min price = 1
 | 7 | 1 | 5 | 3 | 6 | 4 |   Profit = 0 (can't sell yet)
 +---+---+---+---+---+---+   dp[1] = 0
       ^
       |
   
 +---+---+---+---+---+---+   Day 2: Check if selling gives profit
 | 7 | 1 | 5 | 3 | 6 | 4 |   Profit = 5-1 = 4
 +---+---+---+---+---+---+   dp[2] = 4
       |-->^
           |
           
 +---+---+---+---+---+---+   Day 3: Compare with previous max profit
 | 7 | 1 | 5 | 3 | 6 | 4 |   Profit = 3-1 = 2 < dp[2]
 +---+---+---+---+---+---+   dp[3] = 4 (unchanged)
       |---->^
               
 +---+---+---+---+---+---+   Day 4: Better profit found
 | 7 | 1 | 5 | 3 | 6 | 4 |   Profit = 6-1 = 5 > dp[3]
 +---+---+---+---+---+---+   dp[4] = 5
       |------>^
                   
 +---+---+---+---+---+---+   Day 5: Compare with previous max profit
 | 7 | 1 | 5 | 3 | 6 | 4 |   Profit = 4-1 = 3 < dp[4]
 +---+---+---+---+---+---+   dp[5] = 5 (unchanged)
       |-------->^
                      
 Final answer: dp[5] = 5
```

### Pseudo Code (Dynamic Programming)
```
function maxProfit(prices):
    if prices is empty:
        return 0
        
    dp = array of size length(prices), initialized with 0
    min_price = prices[0]
    
    for i from 1 to length(prices)-1:
        dp[i] = maximum(dp[i-1], prices[i] - min_price)
        min_price = minimum(min_price, prices[i])
    
    return dp[length(prices)-1]
```

### Python Code (Dynamic Programming)
```python
def maxProfit_dp(prices):
    """
    Dynamic Programming approach mein hum har din ke liye maximum possible 
    profit calculate karte hain, pichle din ke maximum profit ko use karke.
    
    Time Complexity: O(n) - Array ko sirf ek baar traverse kiya
    Space Complexity: O(n) - DP array ke liye extra space use kiya
    
    Args:
        prices (list): Array jisme har index par uss din ka stock price hai
        
    Returns:
        int: Maximum possible profit, ya 0 agar profit possible na ho
    """
    if not prices or len(prices) < 2:  # Agar array empty hai ya sirf ek element hai
        return 0
        
    n = len(prices)
    dp = [0] * n  # dp[i] = maximum profit possible till day i
    min_price = prices[0]
    
    for i in range(1, n):
        # Do options hain: ya to pichle din ka profit use karo, ya aaj becho
        current_profit = prices[i] - min_price
        dp[i] = max(dp[i-1], current_profit)
        
        # Update karo minimum price
        min_price = min(min_price, prices[i])
    
    return dp[n-1]
```

## Visual Comparison of All Three Approaches

```
💰 Time and Space Complexity Comparison 💰

+------------------+------------------------+------------------------+
|     Approach     |    Time Complexity     |    Space Complexity    |
+------------------+------------------------+------------------------+
| 🐢 Brute Force   |     O(n²)              |      O(1)              |
| 🐇 Two Pointers  |     O(n)               |      O(1)              |
| 🧠 DP Solution   |     O(n)               |      O(n)              |
+------------------+------------------------+------------------------+

👉 Best Solution: Two Pointers approach is optimal, kyunki time complexity O(n)
   aur space complexity O(1) hai.
```

## Emoji-Based Process Flow

```
🏁 Starting Problem: [7, 1, 5, 3, 6, 4]

Brute Force Process:
[7, 1, 5, 3, 6, 4]
 ⬇️  ⬇️
 Buy Sell  ➡️ Compare all combinations ➡️ 📊 Max Profit = 5

Two Pointers Process:
[7,  1,  5,  3,  6,  4]
 🔍   ⬇️   ⬇️   ⬇️   ⬇️   ⬇️
minP=7  1   1   1   1   1
maxP=0  0   4   4   5   5  ➡️ 📈 Max Profit = 5

DP Process:
[7,  1,  5,  3,  6,  4]
 🧮   ⬇️   ⬇️   ⬇️   ⬇️   ⬇️
dp=[0,  0,  4,  4,  5,  5] ➡️ 🎯 Max Profit = 5

🏆 Final Answer: 5 (Buy at price 1, sell at price 6)
```

## Comparison of All Three Solutions

| **Solution**    | **Pros**                         | **Cons**                           |
|-----------------|----------------------------------|----------------------------------- |
| Brute Force     | Simple to understand             | Very slow for large inputs (O(n²)) |
|                 | Works for all inputs             | Not efficient                      |
| Two Pointers    | Fast - O(n)                      | Slightly harder to understand      |
|                 | Space efficient - O(1)           |                                    |
| DP              | Easy to extend for more complex  | Uses more memory - O(n)            |
|                 | variations of the problem        |                                    |

## Optimized Solution (Two Pointers)

Based on our analysis, the Two Pointers approach is the best solution for this problem, with O(n) time complexity and O(1) space complexity.

```python
def maxProfit(prices):
    """
    Maximum profit find karne ka optimum solution.
    
    Args:
        prices (list): Array jisme har index par uss din ka stock price hai
        
    Returns:
        int: Maximum possible profit, ya 0 agar profit possible na ho
    """
    if not prices:
        return 0
        
    min_price = float('inf')
    max_profit = 0
    
    for price in prices:
        min_price = min(min_price, price)
        current_profit = price - min_price
        max_profit = max(max_profit, current_profit)
    
    return max_profit
```

## Test Case Example

```
Input: prices = [7, 1, 5, 3, 6, 4]
Output: 5
Explanation: Day 1 par buy karo (price = 1) aur day 4 par becho (price = 6),
             profit = 6-1 = 5.
```

Happy Trading! 📊 📈 💹

