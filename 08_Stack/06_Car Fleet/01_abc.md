# Car Fleet Solution

## Problem Visualization

Let's visualize the car fleet problem using ASCII art:

```
Starting point                                                Target
0                                                              12
|--------------------------------------------------------------|
                                                  
Car 1 (pos=10, speed=2)  🚗                        →           |
Car 2 (pos=8, speed=4)      🚙                     →→          |
Car 3 (pos=0, speed=1)                      🚚      →           |
Car 4 (pos=5, speed=1)               🚛            →           |
Car 5 (pos=3, speed=3)                  🚐         →→→         |
```

### Time Evolution:

```
t=0
|--------------------------------------------------------------|
Car 1 (pos=10, speed=2)  🚗                                    |
Car 2 (pos=8, speed=4)      🚙                                 |
Car 3 (pos=0, speed=1)                      🚚                 |
Car 4 (pos=5, speed=1)               🚛                        |
Car 5 (pos=3, speed=3)                  🚐                     |

t=1
|--------------------------------------------------------------|
Car 1 (pos=10+2=12, speed=2)                                  🚗
Car 2 (pos=8+4=12, speed=4)                                   🚙
Car 3 (pos=0+1=1, speed=1)                    🚚               |
Car 4 (pos=5+1=6, speed=1)              🚛                     |
Car 5 (pos=3+3=6, speed=3)              🚐                     |

t=2
|--------------------------------------------------------------|
Car 1 (REACHED TARGET)                                        🚗
Car 2 (REACHED TARGET)                                        🚙
Car 3 (pos=1+1=2, speed=1)                   🚚                |
Car 4 (pos=6+1=7, speed=1)             🚛                      |
Car 5 (pos=6+3=9, speed=3)        🚐                           |

t=3
|--------------------------------------------------------------|
Car 1 (REACHED TARGET)                                        🚗
Car 2 (REACHED TARGET)                                        🚙
Car 3 (pos=2+1=3, speed=1)                  🚚                 |
Car 4 (pos=7+1=8, speed=1)            🚛                       |
Car 5 (pos=9+3=12, speed=3)                                  🚐

... and so on
```

In this problem, car fleets form when faster cars catch up to slower ones. Since cars can't overtake, they merge into a fleet moving at the slower car's speed.

## Key Insights

1. Cars that start further ahead and are slower will cause faster cars behind them to slow down and form fleets.
2. We need to determine how many distinct fleets will arrive at the target.
3. For each car, we calculate the time it takes to reach the target.
4. Cars that can't catch up to the car ahead before reaching the target will form their own fleet.

## Approach 1: Using Stack

### Pseudocode:
```
1. Create array of (position, time-to-target) pairs for each car
2. Sort array by position in descending order (cars closest to target first)
3. Initialize a stack to track fleet leaders
4. For each car:
   a. Calculate time to reach target
   b. If stack is empty or current car time > stack top time (can't catch up):
      - Push current car's time onto stack
   c. Else:
      - This car joins the fleet ahead (don't push to stack)
5. Return stack size (number of fleets)
```

### Python Implementation:

```python
def carFleet(target: int, position: list[int], speed: list[int]) -> int:
    # Create cars with their positions and calculate time to reach target
    cars = [(pos, (target - pos) / spd) for pos, spd in zip(position, speed)]
    
    # Sort cars by position in descending order (starting with cars closest to target)
    cars.sort(reverse=True)
    
    # Stack to store arrival times of fleet leaders
    stack = []
    
    # Process each car and determine fleets
    for pos, time in cars:
        # If stack is empty or current car is slower than stack top (can't catch up)
        if not stack or time > stack[-1]:
            stack.append(time)  # This car becomes leader of a new fleet
    
    # Return the number of fleets
    return len(stack)
```

## Approach 2: Using Iteration (without explicit stack)

### Pseudocode:
```
1. Create array of (position, time-to-target) pairs for each car
2. Sort array by position in descending order (cars closest to target first)
3. Initialize fleets_count = 0
4. Initialize prev_time = 0
5. For each car in sorted order:
   a. Calculate time to reach target
   b. If current car time > prev_time (can't catch up):
      - Increment fleets_count
      - Set prev_time = current car time
6. Return fleets_count
```

### Python Implementation:

```python
def carFleet(target: int, position: list[int], speed: list[int]) -> int:
    # Make sure we have cars to process
    if not position or not speed:
        return 0
    
    # Create cars with their positions and calculate time to reach target
    cars = [(pos, (target - pos) / spd) for pos, spd in zip(position, speed)]
    
    # Sort cars by position in descending order (starting with cars closest to target)
    cars.sort(reverse=True)
    
    fleets_count = 0
    prev_time = 0  # Time of the previous fleet leader
    
    # Process each car and determine fleets
    for pos, time in cars:
        if time > prev_time:
            fleets_count += 1
            prev_time = time
    
    return fleets_count
```

## Hinglish Explanation

```
Car fleet problem mein humein yeh dekhna hai ki target tak kitne alag-alag
fleets pahunch rahe hain. Jab bhi koi fast car slow car ko pakad leta hai, 
toh woh slow car ki speed se chalne lagta hai aur ek fleet ban jata hai.

Main approach kuch aisa hai:
1. Sabse pehle har car ke liye time calculate karo ki woh target tak 
   pahunchne mein kitna time legi
2. Cars ko position ke hisaab se descending order mein sort karo (target ke
   paas waali pehle)
3. Ab dekhte hai ki kaun si car kisi doosri car ke peeche phase gayi aur
   kaun si car apni fleet lead karegi

Stack approach mein:
- Har car ko dekhte hain aur agar woh previous car se slower hai (time zyada
  leta hai), toh woh nayi fleet banaayegi
- Stack mein sirf fleet leaders ke arrival times store karte hain
- Final answer stack ka size hota hai

Iteration approach mein:
- Hum same logic follow karte hain lekin explicit stack ki jagah bas ek
  counter aur previous time track karte hain
- Jab bhi current car ka time previous se zyada ho, toh naya fleet count
  karte hain
```

## Complexity Analysis

### Time Complexity: 
- Both approaches: O(n log n) due to sorting
- The actual processing is O(n) for both approaches

### Space Complexity:
- Stack approach: O(n) in worst case all cars form separate fleets
- Iteration approach: O(n) for storing the car time pairs

## Detailed Code with Documentation

### Stack-based Solution:

```python
def carFleet(target: int, position: list[int], speed: list[int]) -> int:
    """
    Determine the number of car fleets that will arrive at the target.
    
    Args:
        target: The target distance
        position: List of starting positions of each car
        speed: List of speeds of each car
        
    Returns:
        Number of car fleets that will arrive at the target
        
    Approach:
        We use a stack to keep track of fleet leaders. Cars that can't catch up
        to the car ahead before reaching the target will form their own fleet.
    """
    # Edge case: no cars
    if not position or not speed:
        return 0
        
    # Combine position and time-to-target for each car
    cars = [(pos, (target - pos) / spd) for pos, spd in zip(position, speed)]
    
    # Sort cars by position in descending order (cars closest to target first)
    cars.sort(reverse=True)
    
    # Stack to track fleet leaders (by their arrival times)
    stack = []
    
    # Process each car to determine fleets
    for pos, time in cars:
        # If stack is empty or current car is slower than the last fleet leader
        if not stack or time > stack[-1]:
            stack.append(time)  # This car becomes a fleet leader
            
    # The number of elements in the stack equals the number of fleets
    return len(stack)
```

### Iteration-based Solution:

```python
def carFleet(target: int, position: list[int], speed: list[int]) -> int:
    """
    Determine the number of car fleets that will arrive at the target.
    
    Args:
        target: The target distance
        position: List of starting positions of each car
        speed: List of speeds of each car
        
    Returns:
        Number of car fleets that will arrive at the target
        
    Approach:
        We iterate through cars sorted by position and count fleet leaders.
        A car becomes a fleet leader if it takes longer to reach the target
        than any car ahead of it.
    """
    # Edge case: no cars
    if not position or not speed:
        return 0
        
    # Combine position and time-to-target for each car
    cars = [(pos, (target - pos) / spd) for pos, spd in zip(position, speed)]
    
    # Sort cars by position in descending order (cars closest to target first)
    cars.sort(reverse=True)
    
    fleets_count = 0  # Count of fleet leaders
    prev_time = 0     # Time of the previous fleet leader
    
    # Process each car to determine fleets
    for pos, time in cars:
        # If current car is slower than the previous fleet leader
        if time > prev_time:
            fleets_count += 1  # This car becomes a fleet leader
            prev_time = time   # Update the time of the latest fleet leader
            
    return fleets_count
```

