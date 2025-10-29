You are given 2 numbers **n and m,** the task is to find **n√m** (nth root of m). If the root is not integer then returns **-1**.

**Examples :**

**Input:** n = 3, m = 27
**Output:** 3
**Explanation:** 33 = 27

**Input:** n = 3, m = 9
**Output:** -1
**Explanation:** 3rd root of 9 is not integer.

**Input:** n = 4, m = 625
**Output:** 5
**Explanation:** 54 = 625

---
# Approach

• **Function:** `multiply(int x, int p, int m)`  
 → Calculates `x^p`.  
 → Stops early and returns if the result exceeds `m` → avoids unnecessary large computations.

• **Function:** `nthRoot(int n, int m)`  
 → Uses binary search to find the integer `x` such that `x^n = m`.  
 → Initialize search range: `start = 1`, `end = m`.  
 → While `start <= end`:  
  – `mid = start + (end - start)/2` → current candidate.  
  – `cal = multiply(mid, n, m)` → calculate `mid^n`.  
  – If `cal == m` → return `mid` (exact root found).  
  – If `cal > m` → root must be smaller → `end = mid - 1`.  
  – If `cal < m` → root must be larger → `start = mid + 1`.

• **Return:**  
 → `-1` if no integer root exists.

```java
class Solution {

    // Function to calculate x^p and stop early if it exceeds m
    public long multiply(int x, int p, int m) {
        long ans = 1; 
        for (int i = 0; i < p; i++) {
            ans *= x;       // Multiply x, p times
            if (ans > m)    // Early exit if result exceeds m
                return ans;
        }
        return ans;
    }

    // Function to find the nth root of m using binary search
    public int nthRoot(int n, int m) {
        int start = 1, end = m;

        while (start <= end) {
            int mid = start + (end - start) / 2;  // Middle value
            long cal = multiply(mid, n, m);       // Calculate mid^n

            if (cal == m) {
                return mid;                       // Found exact nth root
            } else if (cal > m) {
                end = mid - 1;                    // Mid is too big, search left
            } else {
                start = mid + 1;                  // Mid is too small, search right
            }
        }

        return -1; // No exact nth root found
    }
}


```