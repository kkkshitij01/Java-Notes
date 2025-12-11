**Problem Statement:** There is **one** meeting room in a firm. You are given two arrays, start and end each of size N.For an index ‘i’, start[i] denotes the starting time of the ith meeting while end[i]  will denote the ending time of the ith meeting. Find the maximum number of meetings that can be accommodated if only one meeting can happen in the room at a  particular time. Print the order in which these meetings will be performed.

**Example:**

**Input:**  N = 6,  start[] = {1,3,0,5,8,5}, end[] =  {2,4,5,7,9,9}

**Output:** 1 2 4 5

**Explanation:** See the figure for a better understanding.

---

# Approach: 

### **1. Create a 2D array to store activity info**

Each row stores:

- `arr[i][0]` → original index
    
- `arr[i][1]` → start time
    
- `arr[i][2]` → end time
    

This helps pair each activity’s start and end times together.

---

### **2. Sort all activities by end time**

`Arrays.sort(arr, (a, b) -> a[2] - b[2]);`

Sorting by earlier end times ensures we always choose the activity that finishes the soonest →  
leaves maximum space for future activities.

---

### **3. Greedy selection of activities**

- Initialize `count = 0`
    
- Set `prevEnd = Integer.MIN_VALUE` (so the first activity is always considered)
    

Loop over all activities:

#### **Condition to select an activity**

`if (prevEnd < arr[i][1])`

Meaning:

- The previous selected activity ends **before** the next activity starts.
    
- So there is **no overlap** → We can select it.
    

#### **If selected**

- Update `prevEnd = arr[i][2]`
    
- Increase `count++`
    

---

### **4. Print the final count**

`count` = maximum number of non-overlapping activities.


```java
public static void solve(int start[], int end[]) {

    // Create a 2D array to store:
    // [0] → original index
    // [1] → start time
    // [2] → end time
    int arr[][] = new int[start.length][3];

    for (int i = 0; i < start.length; i++) {
        arr[i][0] = i;
        arr[i][1] = start[i];
        arr[i][2] = end[i];
    }

    // Sort activities based on their end times (increasing order)
    Arrays.sort(arr, (a, b) -> (a[2] - b[2]));

    int count = 0;
    int prevEnd = Integer.MIN_VALUE; // keeps track of the last selected activity's end time

    // Greedily pick activities
    for (int i = 0; i < arr.length; i++) {

        // If current activity starts after the previous selected one ended → select it
        if (prevEnd < arr[i][1]) {
            prevEnd = arr[i][2]; // update end time to current activity's end
            count++;             // increase selected activity count
        }
    }

    // Final result: maximum number of non-overlapping activities
    System.out.println(count);
}


```