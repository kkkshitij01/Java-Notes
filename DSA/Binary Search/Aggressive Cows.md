
You are given an array with unique elements of **stalls[]**, which denote the positions of **stalls**. You are also given an integer **k** which denotes the number of aggressive cows. The task is to assign **stalls** to **k** cows such that the **minimum distance** between any two of them is the **maximum** possible.

**Examples:**

**Input:** `stalls[] = [1, 2, 4, 8, 9], k = 3`
**Output:** 3
**Explanation:** The first cow can be placed at stalls`[0],   `
the second cow can be placed at stalls`[2] `and 
the third cow can be placed at stalls`[3]`. 
The minimum distance between cows in this case is 3, which is the largest among all possible ways.

**Input:** stalls`[] = [10, 1, 2, 7, 5],` k = 3
**Output:** 4
**Explanation:** The first cow can be placed at stalls`[0]`,
the second cow can be placed at stalls`[1] `and
the third cow can be placed at stalls`[4]`.
The minimum distance between cows in this case is 4, which is the largest among all possible ways.

**Input:** stalls[] =` [2, 12, 11, 3, 26, 7], k = 5`
**Output:** 1
**Explanation:** Each cow can be placed in any of the stalls, as the no. of stalls are exactly equal to the number of cows.
The minimum distance between cows in this case is 1, which is the largest among all possible ways.

**Constraints:**  
2 ≤ stalls.size() ≤ 106  
0 ≤ stalls`[i]` ≤ 108  
2 ≤ k ≤ stalls.size()


# Optimal Solution : 

- **Function Name:** `aggressiveCows()`
    
    - This function finds the **largest minimum distance** possible between any two cows when placing `k` cows in given stall positions.
        
    - **Step 1:** Sort the `stalls` array to arrange positions in increasing order.
        
    - **Step 2:** Define the search range —
        
        - `start = 1` → minimum possible gap
            
        - `end = stalls[last] - stalls[0]` → maximum possible gap
            
    - **Step 3:** Apply **binary search** on the possible distance (`mid` = potential minimum gap).
        
    - **Step 4:** For each `mid`, call `canWePlace()` to check if `k` cows can be placed with at least `mid` distance apart.
        
        - If yes → store `ans = mid` and try for a larger gap (`start = mid + 1`).
            
        - Else → reduce the gap (`end = mid - 1`).
            
    - **Step 5:** Return the largest distance (`ans`) found.
        

---

- **Function Name:** `canWePlace()`
    
    - Checks whether it’s possible to place `k` cows in the stalls with a minimum distance of `gap`.
        
    - **Step 1:** Place the first cow at the first stall (`prev = stalls[0]`).
        
    - **Step 2:** Loop through remaining stalls, and whenever `stalls[i] - prev >= gap`, place another cow and update `prev`.
        
    - **Step 3:** If all `k` cows are placed, return `true`; otherwise, return `false`.

## - **Time Complexity:** `O(N log(M))`
    
    - N = number of stalls,
        
    - M = search range (`max distance - min distance`)
        
- **Space Complexity:** `O(1)`
```java
class Solution {
    public int aggressiveCows(int[] stalls, int k) {
        // Step 1: Sort the stall positions in ascending order
        Arrays.sort(stalls);

        // Minimum possible distance between cows = 1 (closest possible)
        int start = 1;

        // Maximum possible distance = farthest stall - closest stall
        int end = stalls[stalls.length - 1] - stalls[0];

        int ans = 0; // will store the largest minimum distance found

        // Step 2: Apply binary search on the "distance" (gap between cows)
        while (start <= end) {
            int mid = (start + end) / 2; // mid represents the minimum distance to test

            // Check if we can place all cows with at least 'mid' distance between them
            if (canWePlace(stalls, k, mid)) {
                ans = mid;       // store this as a valid answer
                start = mid + 1; // try for a larger minimum distance
            } else {
                end = mid - 1;   // reduce distance, as cows can’t be placed with this gap
            }
        }

        return ans; // final maximum minimum distance
    }

    // Helper function to check if we can place 'k' cows with at least 'gap' distance apart
    public boolean canWePlace(int[] stalls, int k, int gap) {
        int prev = stalls[0]; // place first cow in the first stall
        int cow = 1;          // count of cows placed

        // Try to place the remaining cows
        for (int i = 1; i < stalls.length; i++) {
            // If the current stall is far enough from the previous cow
            if (stalls[i] - prev >= gap) {
                cow++;           // place the cow here
                prev = stalls[i]; // update last cow position

                // If all cows are placed successfully, stop checking
                if (cow == k) {
                    break;
                }
            }
        }

        // Return true if all cows are placed
        return cow == k;
    }
}

```