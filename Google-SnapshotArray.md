Here’s how you can structure a markdown file for the SnapshotArray problem with approaches and solutions in different languages (Java, C++, C, Python).

---

# Snapshot Array Problem (LeetCode 1146)

## Problem Statement

Implement a `SnapshotArray` that supports the following interface:

1. `SnapshotArray(int length)`: Initializes an array-like data structure with the given length. Initially, each element equals `0`.
2. `void set(index, val)`: Sets the element at the given index to be equal to `val`.
3. `int snap()`: Takes a snapshot of the array and returns the `snap_id`: the total number of times we called `snap()` minus 1.
4. `int get(index, snap_id)`: Returns the value at the given index, at the time we took the snapshot with the given `snap_id`.

### Example

```plaintext
Input: ["SnapshotArray","set","snap","set","get"]
[[3],[0,5],[],[0,6],[0,0]]
Output: [null,null,0,null,5]
```

```plaintext
Explanation: 
SnapshotArray snapshotArr = new SnapshotArray(3); // set the length to be 3
snapshotArr.set(0,5);  // Set array[0] = 5
snapshotArr.snap();  // Take a snapshot, return snap_id = 0
snapshotArr.set(0,6);
snapshotArr.get(0,0);  // Get the value of array[0] with snap_id = 0, return 5
```

## Constraints

- `1 <= length <= 5 * 10^4`
- `0 <= index < length`
- `0 <= val <= 10^9`
- `0 <= snap_id < (the total number of times we call snap())`
- At most `5 * 10^4` calls will be made to `set()`, `snap()`, and `get()`.

---

## Approaches

### 1. Time Complexity Analysis

**Challenges**:
- We need to efficiently store the state of the array at each `snap()` call.
- Directly storing a full copy of the array for each snapshot would be inefficient in both time and space.

**Optimized Approach**:
- Use a dictionary (or map) to store changes only.
- For each index, store the values that change over time.
- Use a dictionary with a list of tuples `(snap_id, value)` for each index, keeping track of when each index was last updated.
- Use binary search or linear search to efficiently retrieve the last known value for a given `snap_id`.

### 2. Solution Outline

- **Data Structure**: A list of dictionaries, where each dictionary keeps track of changes for that index over time.
- **Snapshot Id**: Every time we call `snap()`, we increment the snapshot id and store the current changes.

---

## Solution

### Java

```java
import java.util.*;

class SnapshotArray {
    private List<TreeMap<Integer, Integer>> arr;
    private int snapId;

    public SnapshotArray(int length) {
        snapId = 0;
        arr = new ArrayList<>();
        for (int i = 0; i < length; i++) {
            arr.add(new TreeMap<>());
            arr.get(i).put(0, 0); // Initialize each index with snapId 0 and value 0
        }
    }

    public void set(int index, int val) {
        arr.get(index).put(snapId, val); // Update the value for the current snapId
    }

    public int snap() {
        return snapId++; // Return current snapId and increment for next snap
    }

    public int get(int index, int snap_id) {
        return arr.get(index).floorEntry(snap_id).getValue(); // Find the value at or before snap_id
    }
}
```

### C++

```cpp
#include <vector>
#include <map>
using namespace std;

class SnapshotArray {
private:
    vector<map<int, int>> arr;
    int snapId;

public:
    SnapshotArray(int length) {
        snapId = 0;
        arr.resize(length);
        for (int i = 0; i < length; i++) {
            arr[i][0] = 0; // Initialize with snapId 0 and value 0
        }
    }

    void set(int index, int val) {
        arr[index][snapId] = val; // Update value for the current snapId
    }

    int snap() {
        return snapId++; // Return current snapId and increment for next snap
    }

    int get(int index, int snap_id) {
        auto it = arr[index].upper_bound(snap_id);
        if (it == arr[index].begin()) {
            return 0;
        } else {
            return prev(it)->second; // Find the value at or before snap_id
        }
    }
};
```

### C

```c
#include <stdlib.h>

typedef struct {
    int snap_id;
    int value;
} Snapshot;

typedef struct {
    Snapshot** changes;
    int length;
    int current_snap;
    int* change_size;
} SnapshotArray;

SnapshotArray* snapshotArrayCreate(int length) {
    SnapshotArray* obj = (SnapshotArray*)malloc(sizeof(SnapshotArray));
    obj->length = length;
    obj->current_snap = 0;
    obj->changes = (Snapshot**)malloc(length * sizeof(Snapshot*));
    obj->change_size = (int*)calloc(length, sizeof(int));
    for (int i = 0; i < length; i++) {
        obj->changes[i] = (Snapshot*)malloc(1 * sizeof(Snapshot));
        obj->changes[i][0].snap_id = 0;
        obj->changes[i][0].value = 0;
        obj->change_size[i] = 1;
    }
    return obj;
}

void snapshotArraySet(SnapshotArray* obj, int index, int val) {
    int size = obj->change_size[index];
    obj->changes[index] = (Snapshot*)realloc(obj->changes[index], (size + 1) * sizeof(Snapshot));
    obj->changes[index][size].snap_id = obj->current_snap;
    obj->changes[index][size].value = val;
    obj->change_size[index]++;
}

int snapshotArraySnap(SnapshotArray* obj) {
    return obj->current_snap++;
}

int snapshotArrayGet(SnapshotArray* obj, int index, int snap_id) {
    int size = obj->change_size[index];
    for (int i = size - 1; i >= 0; i--) {
        if (obj->changes[index][i].snap_id <= snap_id) {
            return obj->changes[index][i].value;
        }
    }
    return 0;
}

void snapshotArrayFree(SnapshotArray* obj) {
    for (int i = 0; i < obj->length; i++) {
        free(obj->changes[i]);
    }
    free(obj->changes);
    free(obj->change_size);
    free(obj);
}
```

### Python

```python
class SnapshotArray:

    def __init__(self, length: int):
        self.snap_id = 0
        self.arr = [{} for _ in range(length)]
        for i in range(length):
            self.arr[i][0] = 0

    def set(self, index: int, val: int) -> None:
        self.arr[index][self.snap_id] = val

    def snap(self) -> int:
        self.snap_id += 1
        return self.snap_id - 1

    def get(self, index: int, snap_id: int) -> int:
        if snap_id in self.arr[index]:
            return self.arr[index][snap_id]
        else:
            # Find the nearest lower snapshot
            for i in range(snap_id, -1, -1):
                if i in self.arr[index]:
                    return self.arr[index][i]
        return 0
```

---

## Conclusion

This problem is optimized using dictionaries or maps to track changes to indices in the array at specific snapshots. The solutions make use of efficient lookup methods such as binary search or floor queries to retrieve the correct value based on the `snap_id`. The space complexity is reduced by only storing the changes, not the entire array for each snapshot.