Given an array ****arr`[]***`* and an integer ****diff****, count the ****number of ways**** to partition the array into two subsets such that the difference between their sums is equal to ****diff****.

****Note:**** A partition in the array means dividing an array into two subsets say S1 and S2 such that the union of S1 and S2 is equal to the original array and each element is present in only one of the subsets.

**Examples :**

**Input:** arr`[] = [5, 2, 6, 4],` diff = 3
**Output:** 1
**Explanation:** There is only one possible partition of this array. Partition : `[6, 4], [5, 2].` The subset difference between subset sum is: (6 + 4) - (5 + 2) = 3.

**Input:** arr`[] = [1, 1, 1, 1],` diff = 0   
**Output:** 6   
**Explanation:** We can choose two 1's from indices` [0,1], [0,2], [0,3], [1,2], [1,3], [2,3] `and put them in sum1 and remaning two 1's in sum2.  
Thus there are total 6 ways for partition the array arr. 

**Input:** arr`[] = [3, 2, 7, 1]`, diff = 4    
**Output:** 0  
**Explanation:** There is no possible partition of the array that satisfy the given difference. 

**Constraint:**  
1 ≤ arr.size() ≤ 50  
0 ≤ diff ≤ 50  
0 ≤ arr`[i]` ≤ 6

---
---


# Optimal : 

```java

```
# Count Partitions with Given Difference

## Goal

Count the number of ways to split the array into **two subsets S1 and S2** such that `S2 - S1 = diff`.

---

## Key Math First

If total sum = `S` and one subset has sum `i`:

- Other subset = `S - i`
- Condition: `(S - i) - i = diff` → `S - 2i = diff`

So instead of thinking about two subsets, we just need to find **how many subsets have sum `i`** where `S - 2i == diff`.

---

## Step 1 — `subSetSum()` : Count-based DP Table

Same structure as before, but now `dp[i][j]` stores a **count** instead of true/false:

```
dp[i][j] = number of ways to form sum j using first i elements
```

**Base case:** `dp[i][0] = 1` → there's always 1 way to make sum 0 (empty subset)

**Recurrence:**

```
if j >= arr[i-1]:
    dp[i][j] = dp[i-1][j]               // skip arr[i-1]
             + dp[i-1][j - arr[i-1]]    // include arr[i-1]
else:
    dp[i][j] = dp[i-1][j]               // can only skip
```

---

## Step 2 — `countPartitions()` : Find Matching Splits

Scan all subset sums `i` from `0` to `S/2`.  
For each reachable sum `i`, check if it satisfies `S - 2i == diff`.  
If yes → add `dp[n][i]` (number of ways to form that subset) to the answer.

```
for i from 0 to S/2:
    if dp[n][i] != 0 and S - 2*i == diff:
        count += dp[n][i]
```

