Given an array `arr[]` containing **non-negative** integers, the task is to divide it into two sets **set1** and **set2** such that the absolute difference between their sums is minimum and find the **minimum** difference.

**Examples:**

**Input**: arr`[] = [1, 6, 11, 5]`
**Output:** 1
**Explanation**: 
Subset1 = {1, 5, 6}, sum of Subset1 = 12 
Subset2 = {11}, sum of Subset2 = 11   
Hence, minimum difference is 1.  

**Input:** arr`[] = [1, 4]`
**Output:** 3
**Explanation**: 
Subset1 = {1}, sum of Subset1 = 1
Subset2 = {4}, sum of Subset2 = 4  
Hence, minimum difference is 3.  

**Input:** arr`[] = [1]`
**Output:** 1
**Explanation**: 
Subset1 = {1}, sum of Subset1 = 1
Subset2 = {}, sum of Subset2 = 0  
Hence, minimum difference is 1.

**Constraints:**  
1 ≤ arr.size()*|sum of array elements| ≤ 105  
`1 <= arr[i] <= 105`

---
---

# Optimal 

```java
class Solution {
    boolean dp[][];

    public void subSetSum(int arr[] , int target ){

        // dp[i][j] = can we form sum j using first i elements

        // sum 0 is always possible
        for (int i = 0; i < arr.length + 1; i++) {
            dp[i][0] = true;
        }

        // fill dp table
        for (int i = 1; i < arr.length + 1; i++) {

            // try to form all sums from 0 to target
            for (int j = 0; j < target + 1; j++) {

                // if current element can be included
                if (j >= arr[i - 1]) {

                    // include OR exclude current element
                    dp[i][j] = dp[i - 1][j] || dp[i - 1][j - arr[i - 1]];

                } else {

                    // cannot include, so skip
                    dp[i][j] = dp[i - 1][j];
                }
            }
        }
    }

    public int minDifference(int arr[]) {

        // calculate total sum of array
        int sum = 0; 
        for(int ar : arr){
            sum+=ar;
        }

        // create dp table for subset sum
        dp = new boolean[arr.length+1][sum+1];

        // fill dp table
        subSetSum(arr, sum);
        
        int min = Integer.MAX_VALUE; 

        // check only half of possible sums
        // because other half will give same difference
        for(int i = 0 ; i< (sum+2)/2; i++){

            // if sum i is possible
            if(dp[arr.length][i]){

                // second subset sum
                int s2 = sum - i;

                // update minimum difference
                if(s2-i < min){
                    min = s2-i;
                }
            }
        }

        // return minimum difference
        return min;
        
    }
}
```


# Minimum Subset Sum Difference

## Goal

Split the array into **two subsets** such that the **difference between their sums is minimum**.

---

## Core Idea

If total sum of array = `S`, and one subset has sum `i`,  
then the other subset has sum `S - i`.  
Difference = `(S - i) - i = S - 2i`

So the problem becomes → **find all achievable subset sums**, then pick the one closest to `S/2` that minimizes `S - 2i`.

---

## Step 1 — `subSetSum()` : Build the DP Table

Same as the classic **Subset Sum DP** — fill a boolean table where:

```
dp[i][j] = true → "can we form sum j using first i elements?"
```

**Base case:** `dp[i][0] = true` for all `i` → sum 0 is always possible (empty subset)

**Recurrence:**

```
if j >= arr[i-1]:
    dp[i][j] = dp[i-1][j]            // skip arr[i-1]
             || dp[i-1][j - arr[i-1]] // include arr[i-1]
else:
    dp[i][j] = dp[i-1][j]            // can only skip
```

---

## Step 2 — `minDifference()` : Find the Answer

After filling the table, look at the **last row** `dp[arr.length][i]` — these are all subset sums actually achievable.

We only check sums from `0` to `S/2` because:

- If one subset has sum `i ≤ S/2`, the other has `S - i ≥ S/2`
- Difference is always `S - 2i` → minimized when `i` is as close to `S/2` as possible

```
for i from 0 to S/2:
    if dp[n][i] is true:
        difference = (S - i) - i = S - 2i
        track minimum
```

---

## Example

```
arr = [1, 6, 11, 5],  S = 23
```

Achievable sums near S/2 (~11): `11` is reachable  
→ Subset 1 sum = 11, Subset 2 sum = 12  
→ **Min difference = 1** ✅

