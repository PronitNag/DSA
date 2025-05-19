# Min Stack Solution (LeetCode #155)

## Problem Statement

Design a stack that supports `push`, `pop`, `top`, and retrieving the minimum element in constant time.

Implement the `MinStack` class:
- `MinStack()` initializes the stack object.
- `void push(int val)` pushes the element `val` onto the stack.
- `void pop()` removes the element on the top of the stack.
- `int top()` gets the top element of the stack.
- `int getMin()` retrieves the minimum element in the stack.

You must implement a solution with O(1) time complexity for each function.

## Approach 1: Brute Force

### Visualization

```
Min Stack: Brute Force Approach
                                                                            
     +-----+       +-----+       +-----+       +-----+       +-----+        
     | -2  |       | -2  |       | -2  |       | -3  |       | -3  |        
     +-----+       +-----+       +-----+       +-----+       +-----+        
     | 0   |       | 0   |       | 0   |       | 0   |       | 0   |        
     +-----+       +-----+       +-----+       +-----+       +-----+        
     | -3  |  Pop  | -3  |  Top  | -3  | Push  | -3  | GetMin| -3  |        
     +-----+ ----> +-----+ ----> +-----+ ----> +-----+ ----> +-----+        
                                          |                                 
                        Returns: -2   +-----+           Returns: -3         
                                      | -2  |                               
                                      +-----+                               
                                                                            
        Regular stack operations      Find min by traversing entire stack   
```

### Pseudocode
```
class MinStack:
    initialize stack as empty list
    
    function push(val):
        add val to end of stack
    
    function pop():
        remove and return last element from stack
    
    function top():
        return last element of stack
    
    function getMin():
        min_val = infinity
        for each element in stack:
            if element < min_val:
                min_val = element
        return min_val
```

### Python Code (Brute Force)

```python
class MinStack:
    """
    Brute force implementation of Min Stack with O(n) time complexity for getMin().
    
    Isme hum ek simple stack use karenge push, pop, aur top operations ke liye,
    lekin getMin() operation mein hume poora stack traverse karna padega.
    """
    
    def __init__(self):
        """
        Ek khali stack initialize karta hai jismein elements store honge.
        """
        self.stack = []
        
    def push(self, val: int) -> None:
        """
        Stack mein ek nya element add karta hai.
        Time Complexity: O(1)
        """
        self.stack.append(val)
        
    def pop(self) -> None:
        """
        Stack ke top element ko remove karta hai.
        Time Complexity: O(1)
        """
        if self.stack:
            self.stack.pop()
        
    def top(self) -> int:
        """
        Stack ke top element ko return karta hai bina remove kiye.
        Time Complexity: O(1)
        """
        if self.stack:
            return self.stack[-1]
        return None
        
    def getMin(self) -> int:
        """
        Stack mein minimum element ko find karke return karta hai.
        Time Complexity: O(n) - Kyunki hume saare elements check karne padte hai.
        """
        if not self.stack:
            return None
        return min(self.stack)
```

## Approach 2: Two Stacks

### Visualization

```
Min Stack: Two Stacks Approach
                                                                            
Main Stack:                                                                 
     +-----+       +-----+       +-----+       +-----+       +-----+        
     | -2  |       |     |       |     |       | -2  |       | -2  |        
     +-----+       +-----+       +-----+       +-----+       +-----+        
     | 0   |       | 0   |       | 0   |       | 0   |       | 0   |        
     +-----+  Pop  +-----+  Top  +-----+ Push  +-----+ GetMin+-----+        
     | -3  | ----> | -3  | ----> | -3  | ----> | -3  | ----> | -3  |        
     +-----+       +-----+       +-----+       +-----+       +-----+        
                                          ↑                                 
                      Returns: 0     +-----+                                
Min Stack:                       Add | -2  |                                
     +-----+       +-----+       +-----+       +-----+       +-----+        
     | -3  |       | -3  |       | -3  |       | -3  |       | -3  |        
     +-----+  Pop  +-----+  Top  +-----+ Push  +-----+ GetMin+-----+        
     | -3  | ----> | -3  | ----> | -3  | ----> | -3  | ----> | -3  |        
     +-----+       +-----+       +-----+       +-----+       +-----+        
                                          ↑                                 
                                     +-----+           Returns: -3          
                                Add  | -3  | (min value so far)             
                                     +-----+                                
```

### Pseudocode
```
class MinStack:
    initialize main_stack as empty list
    initialize min_stack as empty list
    
    function push(val):
        add val to end of main_stack
        if min_stack is empty OR val <= min_stack.top():
            add val to end of min_stack
    
    function pop():
        if main_stack.top() == min_stack.top():
            remove last element from min_stack
        remove last element from main_stack
    
    function top():
        return last element of main_stack
    
    function getMin():
        return last element of min_stack
```

### Python Code (Two Stacks)

```python
class MinStack:
    """
    Two stacks implementation of Min Stack with O(1) time complexity for all operations.
    
    Isme hum do stacks use karenge - ek normal elements ke liye aur doosra
    minimum elements ke liye. Is approach mein getMin() constant time mein possible hai.
    """
    
    def __init__(self):
        """
        Do empty stacks initialize karta hai - ek regular elements ke liye
        aur doosra har point par minimum element track karne ke liye.
        """
        self.stack = []        # Main stack to store all elements
        self.min_stack = []    # Auxiliary stack to track minimum elements
        
    def push(self, val: int) -> None:
        """
        Main stack mein element add karta hai aur min_stack ko update karta hai.
        Time Complexity: O(1)
        """
        self.stack.append(val)
        
        # Min stack mein sirf tab add karo jab:
        # 1. Min stack khali ho, ya
        # 2. New value current minimum se chota ya equal hai
        if not self.min_stack or val <= self.min_stack[-1]:
            self.min_stack.append(val)
        
    def pop(self) -> None:
        """
        Main stack se element remove karta hai aur zaroorat padne par min_stack
        ko bhi update karta hai.
        Time Complexity: O(1)
        """
        if not self.stack:
            return
            
        # Agar hum current minimum ko pop kar rahe hain, to min_stack se bhi pop karo
        if self.stack[-1] == self.min_stack[-1]:
            self.min_stack.pop()
            
        self.stack.pop()
        
    def top(self) -> int:
        """
        Main stack ke top element ko return karta hai.
        Time Complexity: O(1)
        """
        if self.stack:
            return self.stack[-1]
        return None
        
    def getMin(self) -> int:
        """
        Current minimum element ko return karta hai.
        Time Complexity: O(1)
        """
        if self.min_stack:
            return self.min_stack[-1]
        return None
```

## Approach 3: One Stack with Pairs

### Visualization

```
Min Stack: One Stack with Pairs Approach
                                                                            
     +----------+    +----------+    +----------+    +----------+    +------+
     | val: -2  |    |          |    |          |    | val: -2  |    |      |
     | min: -3  |    |          |    |          |    | min: -3  |    |      |
     +----------+    +----------+    +----------+    +----------+    +------+
     | val: 0   |    | val: 0   |    | val: 0   |    | val: 0   |    |      |
     | min: -3  | Pop| min: -3  | Top| min: -3  |Push| min: -3  |GetMin     |
     +----------+ -->+----------+ -->+----------+ -->+----------+ -->+------+
     | val: -3  |    | val: -3  |    | val: -3  |    | val: -3  |    |      |
     | min: -3  |    | min: -3  |    | min: -3  |    | min: -3  |    |      |
     +----------+    +----------+    +----------+    +----------+    +------+
                                                ↑                           
                    Returns: 0     +----------+      Returns: -3            
                                   | val: -2  |                             
                                   | min: -3  |                             
                                   +----------+                             
                                                                            
      Each node stores both value and current minimum                       
```

### Pseudocode
```
class MinStack:
    initialize stack as empty list
    
    function push(val):
        if stack is empty:
            min_val = val
        else:
            min_val = minimum(val, stack.top()[1])
        add (val, min_val) pair to end of stack
    
    function pop():
        remove last element from stack
    
    function top():
        return first element of the last pair in stack
    
    function getMin():
        return second element of the last pair in stack
```

### Python Code (One Stack with Pairs)

```python
class MinStack:
    """
    One stack implementation of Min Stack using pairs with O(1) time complexity for all ops.
    
    Isme hum sirf ek stack use karenge lekin har element ke saath current minimum
    value bhi store karenge. Har node (val, min_so_far) ke format mein hoga.
    """
    
    def __init__(self):
        """
        Ek khali stack initialize karta hai jisme tuples store honge.
        Har tuple mein: (actual_value, current_minimum_value)
        """
        self.stack = []
        
    def push(self, val: int) -> None:
        """
        Stack mein (value, current_min) format mein new element add karta hai.
        Time Complexity: O(1)
        """
        # Agar stack khali hai to current minimum wahi value hai
        if not self.stack:
            self.stack.append((val, val))
        else:
            # Otherwise, current minimum is the minimum of new value and previous minimum
            current_min = min(val, self.stack[-1][1])
            self.stack.append((val, current_min))
        
    def pop(self) -> None:
        """
        Stack ke top element ko remove karta hai.
        Time Complexity: O(1)
        """
        if self.stack:
            self.stack.pop()
        
    def top(self) -> int:
        """
        Stack ke top element ki actual value ko return karta hai.
        Time Complexity: O(1)
        """
        if self.stack:
            return self.stack[-1][0]
        return None
        
    def getMin(self) -> int:
        """
        Current minimum element ko return karta hai.
        Time Complexity: O(1)
        """
        if self.stack:
            return self.stack[-1][1]
        return None
```

## Another Approach: One Stack with Value Encoding

### Visualization

```
Min Stack: One Stack with Value Encoding
                                                                            
     +-----+       +-----+       +-----+       +-----+       +-----+        
     | -2  |       |     |       |     |       | -2  |       | -2  |        
     +-----+       +-----+       +-----+       +-----+       +-----+        
     | 0   |  Pop  | 0   |  Top  | 0   | Push  | 0   | GetMin| 0   |        
     +-----+ ----> +-----+ ----> +-----+ ----> +-----+ ----> +-----+        
     | -3  |       | -3  |       | -3  |       | -3  |       | -3  |        
     +-----+       +-----+       +-----+       +-----+       +-----+        
     | MIN=-3|     | MIN=-3|     | MIN=-3|     | MIN=-3|     | MIN=-3|      
     +-----+       +-----+       +-----+       +-----+       +-----+        
                                                                            
    When a new minimum is pushed,     Special encoding helps maintain       
    we store the previous minimum     the minimum across pop operations     
    beforehand                                                              
```

### Python Code (One Stack with Value Encoding)

```python
class MinStack:
    """
    Advanced One stack implementation with value encoding for O(1) operations.
    
    Isme hum current minimum ko track karte hain aur jab bhi naya minimum
    push hota hai, hum pehle purane minimum ko stack mein push karte hain.
    """
    
    def __init__(self):
        """
        Ek khali stack aur current minimum value ko infinity se initialize karta hai.
        """
        self.stack = []
        self.min_val = float('inf')
        
    def push(self, val: int) -> None:
        """
        Stack mein new element add karta hai, special encoding ke saath agar ye 
        naya minimum hai.
        Time Complexity: O(1)
        """
        # Agar new value current minimum se kam hai, to special processing
        if val <= self.min_val:
            # Pehle purane minimum ko push karo
            self.stack.append(self.min_val)
            self.min_val = val
            
        # Ab actual value ko push karo
        self.stack.append(val)
        
    def pop(self) -> None:
        """
        Stack ke top element ko remove karta hai aur special handling karta hai
        agar minimum value pop hui hai.
        Time Complexity: O(1)
        """
        if not self.stack:
            return
            
        # Pop the top value
        val = self.stack.pop()
        
        # Agar ye current minimum tha, to previous minimum ko restore karo
        if val == self.min_val and self.stack:
            self.min_val = self.stack.pop()
        
    def top(self) -> int:
        """
        Stack ke top element ko return karta hai.
        Time Complexity: O(1)
        """
        if self.stack:
            return self.stack[-1]
        return None
        
    def getMin(self) -> int:
        """
        Current minimum element ko return karta hai.
        Time Complexity: O(1)
        """
        return self.min_val if self.min_val != float('inf') else None
```

## Time and Space Complexity Comparison

| Approach | push() | pop() | top() | getMin() | Space |
|----------|--------|-------|-------|----------|-------|
| Brute Force | O(1) | O(1) | O(1) | O(n) | O(n) |
| Two Stacks | O(1) | O(1) | O(1) | O(1) | O(n) |
| One Stack (Pairs) | O(1) | O(1) | O(1) | O(1) | O(n) |
| One Stack (Encoding) | O(1) | O(1) | O(1) | O(1) | O(n) worst case, but typically better |

## Conclusion

The Two Stacks approach and the One Stack with Pairs approach are the most straightforward and efficient solutions for this problem. Both meet the requirement of O(1) time complexity for all operations.

The One Stack with Value Encoding approach is more space-efficient in some scenarios but requires careful implementation to handle edge cases correctly.

Based on simplicity and reliability, the Two Stacks approach is often recommended for interview settings.

```

def test_min_stack():
    """
    Function to test our MinStack implementation
    """
    min_stack = MinStack()
    min_stack.push(-3)
    min_stack.push(0)
    min_stack.push(-2)
    
    print("Current stack:", min_stack.stack)
    print("Current minimum:", min_stack.getMin())  # Should return -3
    print("Top element:", min_stack.top())        # Should return -2
    
    min_stack.pop()
    print("After pop, top is:", min_stack.top())  # Should return 0
    print("After pop, min is:", min_stack.getMin())  # Should still be -3
```

