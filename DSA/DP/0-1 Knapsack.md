Given two arrays, **val`[]`** and **wt`[]`**, where each element represents the value and weight of an item respectively, and an integer **W** representing the maximum capacity of the knapsack (the total weight it can hold).

The task is to put the items into the knapsack such that the total value obtained is **maximum** without exceeding the capacity W.

**Note:** You can either include an item completely or exclude it entirely — fractional selection of items is not allowed. Each item is available only once.

**Examples :**

**Input:** W = 4, `val`[]` = [1, 2, 3], `wt[]` = [4, 5, 1]  ``
**Output:** 3  
**Explanation:** Choose the last item, which weighs 1 unit and has a value of 3.

**Input:**` W = 3, `val[]` = [1, 2, 3], `wt[]` = [4, 5, 6]  ` 
**Output:** 0  
**Explanation:** Every item has a weight exceeding the knapsack's capacity (3).

**Input:** W =` 5, `val[]` = [10, 40, 30, 50], `wt[]` = [5, 4, 2, 3]   `
**Output:** 80  
**Explanation:** Choose the third item (value 30, weight 2) and the last item (value 50, weight 3) for a total value of 80.

**Constraints:**  
1 ≤ val.size() = wt.size() ≤ 103  
1 ≤ W ≤ 103  
1 ≤ val`[i] ≤ 103  `
1 ≤ wt`[i] ≤ 103`

---
---

# Recursive Approach:

```java
class Solution {

    public int knapsack(int W, int val[], int wt[]) {

        // Start recursion from index 0
        return solve(W, val, wt, 0);
    }

    public int solve(int W, int val[], int wt[], int index){

        // BASE CONDITION:
        // If weight capacity becomes 0 OR we checked all items,
        // then no more profit can be added
        if(W == 0 || index == val.length){
            return 0;
        }

        // If current item's weight is less than or equal to remaining capacity
        if(W >= wt[index]){

            // Two choices:
            // 1. Take the item → add its value and reduce weight
            // 2. Skip the item → move to next index
            // Take the maximum of both choices
            return Math.max(
                val[index] + solve(W - wt[index], val, wt, index + 1),
                solve(W, val, wt, index + 1)
            );

        } else {

            // If current item's weight is more than remaining capacity,
            // we cannot take it, so just skip it
            return solve(W, val, wt, index + 1);
        }
    }
}
```

- Start checking from index `0`.
    
- For every item, we have **2 choices**:
    
    1. Take the item
        
    2. Skip the item
        

We choose the option that gives **maximum profit**.

---

## **Base Condition**

- If `W == 0` → bag is full → return `0`
    
- If `index == val.length` → no items left → return `0`
    

---

## **Main Logic**

### Case 1: If item can fit (`W >= wt[index]`)

We have two options:

- **Take the item**
    
    - Add its value
        
    - Reduce weight: `W - wt[index]`
        
    - Move to next index
        
- **Skip the item**
    
    - Keep same weight
        
    - Move to next index
        

Return:

max(take, skip)

---

### Case 2: If item cannot fit (`W < wt[index]`)

- We cannot take it
    
- So only option → skip
    

solve(W, val, wt, index + 1)

---
## **Time & Space Complexity**

| Type   | Complexity             |
| ------ | ---------------------- |
| **TC** | `O(2^n)`               |
| **SC** | `O(n)` recursion stack |


---
---
---

# DP Approach : 

```java
class Solution {

    int[][] dp; // dp[index][W] stores maximum value from index with capacity W

    public void dpInit(int W, int val[], int wt[]) {

        int n = val.length;

        // Create DP table with n rows and W+1 columns
        dp = new int[n][W + 1];

        // Fill all values with -1 to mark "not calculated yet"
        for (int[] row : dp) {
            Arrays.fill(row, -1);
        }
    }
    
    public int knapsack(int W, int val[], int wt[]) {

        // Initialize dp table
        dpInit(W, val , wt);

        // Start solving from index 0
        return solve(W, val, wt, 0);
    }

    public int solve(int W, int val[], int wt[], int index){

        // BASE CONDITION:
        // If capacity becomes 0 OR all items checked, no value can be added
        if(W == 0 || index == val.length){
            return 0; 
        }

        // If already computed, return stored value (avoid recomputation)
        if(dp[index][W] != -1){
            return dp[index][W];
        }

        // If current item weight is within remaining capacity
        if(W >= wt[index]){

            // Two choices:
            // 1. Take item → add value and reduce capacity
            // 2. Skip item → move to next index
            // Store maximum of both in dp table
            return dp[index][W] = Math.max(
                val[index] + solve(W - wt[index], val, wt, index + 1),
                solve(W, val, wt, index + 1)
            );

        } else {

            // If item weight exceeds capacity, cannot take it
            // So skip it and store result
            return dp[index][W] = solve(W, val, wt, index + 1);
        }
    }
}
```

## **Step 1: DP Table Creation**

dp`[n][W+1]`

- `n` → number of items
    
- `W` → capacity
    
- `dp[index][W]` stores:
    
     Maximum profit from `index` to end with capacity `W`
    

Initialize all values with `-1`  
meaning → not calculated yet.

---

## **Step 2: Base Condition**

If:

- `W == 0` → no capacity left
    
- `index == val.length` → no items left
    

Return `0`.

---

## **Step 3: Check DP Before Solving**

If:

`dp[index][W] != -1
`
Return stored answer immediately.

This avoids recomputation.

---

## **Step 4: Two Choices**

### Case 1: Item fits (`W >= wt[index]`)

We choose maximum of:

- **Take**
    
    `val[index] + solve(W - wt[index], index + 1)`
    
- **Skip**
    
    solve(W, index + 1)
    

Store result in:

dp`[index][W]`

---

### Case 2: Item doesn’t fit

Only option:

skip

Store in:

dp`[index][W]`

---
# **Time & Space Complexity**

| Type                | Complexity |
| ------------------- | ---------- |
| **Time**            | `O(n × W)` |
| **Space**           | `O(n × W)` |
| **Recursion Stack** | `O(n)`     |