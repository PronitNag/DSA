# TimeMap: Time Based Key-Value Store Solutions

## Problem Statement

Design a time-based key-value data structure that can store multiple values for the 
same key at different timestamps and retrieve the key's value at a certain timestamp.

**Implement the TimeMap class:**
- `TimeMap()` Initializes the object of the data structure.
- `void set(String key, String value, int timestamp)` Stores the key with the value 
  at the given timestamp.
- `String get(String key, int timestamp)` Returns a value such that set was called 
  previously, with timestamp_prev <= timestamp. If there are multiple such values, 
  it returns the value associated with the largest timestamp_prev. If there are no 
  values, it returns "".

## Visual Representation of the Problem

```
                     ┌─────────────────────────────────────────────┐
                     │               TimeMap                       │
                     └─────────────────────────────────────────────┘
                                       |
              ┌────────────────────────┼────────────────────────┐
              ▼                        ▼                        ▼
┌─────────────────────────┐ ┌─────────────────────────┐ ┌─────────────────────────┐
│     Solution #1         │ │     Solution #2         │ │     Solution #3         │
│    Brute Force          │ │   Binary Search         │ │   Binary Search         │
│    (HashMap + List)     │ │   (TreeMap)             │ │   (HashMap + Array)     │
└─────────────────────────┘ └─────────────────────────┘ └─────────────────────────┘
          │                           │                           │
          ▼                           ▼                           ▼
┌─────────────────────────┐ ┌─────────────────────────┐ ┌─────────────────────────┐
│                         │ │                         │ │                         │
│  key1: [(t1,val1),      │ │  key1: TreeMap<Time,Val>│ │  key1: [(t1,val1),      │
│         (t2,val2),      │ │        ↓                │ │         (t2,val2),      │
│         (t3,val3)]      │ │  {t1:val1, t2:val2,     │ │         (t3,val3)]      │
│                         │ │   t3:val3}              │ │      (sorted by time)   │
│  key2: [(t1,val4),      │ │                         │ │                         │
│         (t4,val5)]      │ │  key2: TreeMap<Time,Val>│ │  key2: [(t1,val4),      │
│                         │ │        ↓                │ │         (t4,val5)]      │
│                         │ │  {t1:val4, t4:val5}     │ │      (sorted by time)   │
└─────────────────────────┘ └─────────────────────────┘ └─────────────────────────┘
          │                           │                           │
          ▼                           ▼                           ▼
┌─────────────────────────┐ ┌─────────────────────────┐ ┌─────────────────────────┐
│  get(key1, t2):         │ │  get(key1, t2):         │ │  get(key1, t2):         │
│    Linear search        │ │    floorEntry(t2)       │ │    Binary search        │
│    O(n) time            │ │    O(log n) time        │ │    O(log n) time        │
└─────────────────────────┘ └─────────────────────────┘ └─────────────────────────┘
```

## Solution Evolution:

```
    Start
      │
      ▼
┌───────────────┐         ┌───────────────┐         ┌───────────────┐
│ Brute Force   │         │ Binary Search │         │ Binary Search │
│ Approach      │ ─────▶  │ with TreeMap  │ ─────▶  │ with Arrays   │
│ O(n) lookup   │         │ O(log n)      │         │ O(log n)      │
└───────────────┘         └───────────────┘         └───────────────┘
      │                          │                          │
      ▼                          ▼                          ▼
┌───────────────┐         ┌───────────────┐         ┌───────────────┐
│HashMap<String, │         │HashMap<String, │         │HashMap<String, │
│List<(t,v)>>   │         │TreeMap<Int,Str>│         │List<(t,v)>>   │
│Linear search  │         │Built-in search │         │Sort + Binary  │
│for timestamp  │         │for floorEntry  │         │search for time│
└───────────────┘         └───────────────┘         └───────────────┘
                                                           │
                                                           ▼
                                                    ┌──────────────┐
                                                    │   Solution   │
                                                    └──────────────┘
```

## Solution 1: Brute Force Approach

### Pseudo Code
```
class TimeMap:
    initialize():
        create empty hashmap store
    
    set(key, value, timestamp):
        if key not in store:
            store[key] = empty list
        append (timestamp, value) to store[key]
    
    get(key, timestamp):
        if key not in store:
            return ""
        
        result = ""
        for (t, v) in store[key]:
            if t <= timestamp and (result == "" or t > prev_t):
                result = v
                prev_t = t
        
        return result
```

### Python Implementation

```python
class TimeMap_BruteForce:
    """
    Time Based Key-Value Store implementation using brute force approach.
    Uses a HashMap with each key mapping to a list of (timestamp, value) pairs.
    Retrieval performs a linear scan through the list to find the appropriate value.
    
    Time Complexity:
        - set: O(1)
        - get: O(n) where n is the number of entries for a given key
    Space Complexity: O(n) overall where n is the total number of entries
    """
    
    def __init__(self):
        """
        Initialize the TimeMap data structure with an empty dictionary to store
        key-timestamp-value mappings.
        """
        self.store = {}  # Dictionary to store key -> list of (timestamp, value) pairs
        
    def set(self, key: str, value: str, timestamp: int) -> None:
        """
        Stores the key with the value at the given timestamp.
        
        Args:
            key: The key to store
            value: The value to store
            timestamp: The timestamp at which the value is set
        """
        if key not in self.store:
            self.store[key] = []
        self.store[key].append((timestamp, value))
        
    def get(self, key: str, timestamp: int) -> str:
        """
        Gets the value associated with the key at the specified timestamp.
        
        Args:
            key: The key to retrieve
            timestamp: The timestamp to query
            
        Returns:
            The value with the largest timestamp <= the query timestamp,
            or "" if no such value exists
        """
        if key not in self.store:
            return ""
        
        result = ""
        for t, v in self.store[key]:
            # Find the value with the largest timestamp <= query timestamp
            if t <= timestamp:
                # Update result if this is a more recent valid timestamp
                if result == "" or t > prev_t:
                    result = v
                    prev_t = t
        
        return result
```

## Solution 2: Binary Search with TreeMap (Sorted Map)

### Pseudo Code
```
class TimeMap:
    initialize():
        create empty hashmap store (each value is a TreeMap)
    
    set(key, value, timestamp):
        if key not in store:
            store[key] = empty TreeMap
        store[key][timestamp] = value
    
    get(key, timestamp):
        if key not in store:
            return ""
            
        // Use TreeMap's floorEntry to find the largest key <= timestamp
        entry = store[key].floorEntry(timestamp)
        if entry exists:
            return entry.value
        else:
            return ""
```

### Python Implementation

```python
from sortedcontainers import SortedDict

class TimeMap_TreeMap:
    """
    Time Based Key-Value Store implementation using a TreeMap approach.
    Uses a HashMap with each key mapping to a SortedDict (Python's equivalent
    of a TreeMap) of timestamp-value pairs. Retrieval uses binary search via
    the SortedDict's bisect_right method.
    
    Time Complexity:
        - set: O(log n) due to TreeMap insertion
        - get: O(log n) for binary search in the TreeMap
    Space Complexity: O(n) overall where n is the total number of entries
    """
    
    def __init__(self):
        """
        Initialize the TimeMap data structure with an empty dictionary to store
        key to SortedDict mappings.
        """
        self.store = {}  # Dictionary to store key -> SortedDict(timestamp -> value)
    
    def set(self, key: str, value: str, timestamp: int) -> None:
        """
        Stores the key with the value at the given timestamp.
        
        Args:
            key: The key to store
            value: The value to store
            timestamp: The timestamp at which the value is set
        """
        if key not in self.store:
            self.store[key] = SortedDict()
        self.store[key][timestamp] = value
    
    def get(self, key: str, timestamp: int) -> str:
        """
        Gets the value associated with the key at the specified timestamp.
        
        Args:
            key: The key to retrieve
            timestamp: The timestamp to query
            
        Returns:
            The value with the largest timestamp <= the query timestamp,
            or "" if no such value exists
        """
        if key not in self.store:
            return ""
        
        # Find the largest timestamp <= query timestamp
        timestamps = self.store[key].keys()
        idx = self.store[key].bisect_right(timestamp)
        
        # If no timestamp is <= query timestamp, return ""
        if idx == 0:
            return ""
        
        # Return the value at the largest timestamp <= query timestamp
        prev_timestamp = timestamps[idx - 1]
        return self.store[key][prev_timestamp]
```

## Solution 3: Binary Search with Arrays

### Pseudo Code
```
class TimeMap:
    initialize():
        create empty hashmap store
    
    set(key, value, timestamp):
        if key not in store:
            store[key] = empty list
        append (timestamp, value) to store[key]
        // No sorting needed as timestamps are added in increasing order by problem constraint
    
    get(key, timestamp):
        if key not in store:
            return ""
            
        values = store[key]
        left = 0
        right = length(values) - 1
        
        result = ""
        while left <= right:
            mid = (left + right) / 2
            if values[mid].timestamp <= timestamp:
                result = values[mid].value
                left = mid + 1
            else:
                right = mid - 1
                
        return result
```

### Python Implementation

```python
class TimeMap_BinarySearch:
    """
    Time Based Key-Value Store implementation using binary search approach.
    Uses a HashMap with each key mapping to a list of (timestamp, value) pairs.
    Since timestamps are guaranteed to be in increasing order when set() is called,
    the list is naturally sorted, allowing for binary search during retrieval.
    
    Time Complexity:
        - set: O(1) for appending to the list
        - get: O(log n) for binary search where n is the number of entries for a key
    Space Complexity: O(n) overall where n is the total number of entries
    """
    
    def __init__(self):
        """
        Initialize the TimeMap data structure with an empty dictionary to store
        key-timestamp-value mappings.
        """
        self.store = {}  # Dictionary to store key -> list of (timestamp, value) pairs
    
    def set(self, key: str, value: str, timestamp: int) -> None:
        """
        Stores the key with the value at the given timestamp.
        
        Args:
            key: The key to store
            value: The value to store
            timestamp: The timestamp at which the value is set
        """
        if key not in self.store:
            self.store[key] = []
        # Problem states that timestamps for set calls are strictly increasing
        # So the list will remain sorted by timestamp automatically
        self.store[key].append((timestamp, value))
    
    def get(self, key: str, timestamp: int) -> str:
        """
        Gets the value associated with the key at the specified timestamp using
        binary search.
        
        Args:
            key: The key to retrieve
            timestamp: The timestamp to query
            
        Returns:
            The value with the largest timestamp <= the query timestamp,
            or "" if no such value exists
        """
        if key not in self.store:
            return ""
        
        values = self.store[key]
        left, right = 0, len(values) - 1
        result = ""
        
        # Binary search to find the largest timestamp <= query timestamp
        while left <= right:
            mid = (left + right) // 2
            if values[mid][0] <= timestamp:
                # This is a valid result, but continue searching to the right
                # to find potentially larger valid timestamps
                result = values[mid][1]
                left = mid + 1
            else:
                # The timestamp is too large, search to the left
                right = mid - 1
                
        return result
```

## Comparison of Solutions in Hinglish

### Brute Force Approach (Samanya Tarika)

Brute force approach mein hum ek simple HashMap ka istemal karte hain jisme key ke 
liye ek list of timestamp-value pairs store karte hain. Get operation ke liye hum
linear search karte hain jo O(n) time complexity deta hai. Ye approach simple hai
lekin bade data ke liye efficient nahi hai.

### TreeMap Approach (Sorted Map Tarika)

Is approach mein hum HashMap ke andar TreeMap (ya Python mein SortedDict) rakhte
hain jo automatically timestamps ko sorted order mein rakhta hai. Get operation
ke liye hum floorEntry method ka istemal karte hain jo binary search algorithm
par based hai, isliye time complexity O(log n) hai. Ye approach second solution
se zada readable hai lekin extra library dependency add karta hai.

### Binary Search Approach (Array ke Saath)

Is approach mein hum HashMap ke andar array rakhte hain jisme timestamp-value 
pairs store karte hain. Problem ye guarantee karta hai ki timestamps increasing
order mein aate hain, isliye array automatically sorted rahega. Get operation
ke liye hum direct binary search algorithm implement karte hain, jisse time
complexity O(log n) ho jati hai. Ye tarika sabse efficient hai aur kisi external
library par depend nahi karta.

## Conclusion

Time Based Key-Value Store ke liye teeno solutions apni jagah theek hain:

1. **Brute Force** approach sabse simple hai implement karne ke liye, lekin large
   datasets ke liye slow ho sakta hai kyunki get operation O(n) time leta hai.

2. **TreeMap** approach retrieval ko O(log n) tak optimize karta hai lekin
   implementation thodi complex ho jati hai aur external library dependency add
   karta hai.

3. **Binary Search with Arrays** approach best balance provide karta hai - O(log n)
   retrieval time, koi external dependency nahi, aur code bhi relatively simple hai.

Agar performance important hai, to solution #3 (Binary Search with Arrays) best choice
hai kyunki ye efficient hai aur maintain karna bhi easy hai.

