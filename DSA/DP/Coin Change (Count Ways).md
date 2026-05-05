Given an integer array **coins`[ ]`** representing different denominations of currency and an integer **sum**, find the number of ways you can make **sum** by using different combinations from coins`[ ]`.   
**Note:** Assume that you have an **infinite** supply of each type of coin. Therefore, you can use any coin as many times as you want.  
Answers are guaranteed to fit into a 32-bit integer. 

**Examples:**

**Input:** coins`[] = [1, 2, 3],` sum = 4
**Output:** 4
**Explanation**: Four Possible ways are:` [1, 1, 1, 1], [1, 1, 2], [2, 2], [1, 3].`

**Input**: coin`s[] = [2, 5, 3, 6]`, sum = 10
**Output:** 5
**Explanation**: Five Possible ways are: `[2, 2, 2, 2, 2], [2, 2, 3, 3], [2, 2, 6], [2, 3, 5] and [5, 5].  `

**Input**: coins`[] = [5, 10], `sum = 3
**Output:** 0  
**Explanation:** Since all coin denominations are greater than sum, no combination can make the target sum.

**Constraints:**  
1 <= sum <= 103  
1 <= coins`[i] `<= 104  
1 <= coins.size() <= 103

---
---

# Memorization : 

```java 
class Solution {
    static int dp[][];

    public int count(int coins[], int sum) {

        // dp[i][j] = number of ways to form sum j starting from index i
        dp = new int[coins.length+1][sum+1];

        // initialize dp with -1 (means not calculated yet)
        for(int d[] : dp){
            Arrays.fill(d, -1);
        }

        // start recursion from index 0 with full sum
        return solve(coins ,  sum ,  0 );
    }

    public int solve(int coins[] , int sum , int idx ){

        // if sum becomes 0, one valid way found
        if(sum == 0 ){
            return 1;
        }

        // if no coins left but sum is not 0, no way possible
        if(idx == coins.length){
            return 0 ;
        }

        // if already calculated, return stored result
        if(dp[idx][sum] != -1){
            return dp[idx][sum];
        }

        int include = 0; 

        // if current coin can be used
        if (coins[idx] <= sum) {

            // include current coin (stay at same index because unlimited coins allowed)
            include = solve(coins, sum - coins[idx], idx);
        }

        // skip current coin and move to next
        int exclude = solve(coins, sum, idx + 1); 

        // total ways = include + exclude
        return dp[idx][sum] = include + exclude;
    }
}
```


## Problem

Given a `coins[]` array and a target `sum`, find **how many ways** you can pick coins (with unlimited reuse) to reach the sum.

---

## Approach – Recursion + Memoization (Top-Down DP)

### The Core Idea

At every step, the function `solve(coins, sum, idx)` stands at coin index `idx` and asks:

> _"Should I take this coin or skip it?"_

- **Include** → pick `coins[idx]`, reduce sum by it, and stay on the **same index** (because we can reuse it)
    
    ```
    solve(coins, sum - coins[idx], idx)
    ```
    
- **Exclude** → skip this coin, move to the **next index**
    
    ```
    solve(coins, sum, idx + 1)
    ```
    

Answer at each node = `include + exclude`

---

## Base Cases

|Condition|Return|Why|
|---|---|---|
|`sum == 0`|`1`|We hit the target exactly — valid way found ✅|
|`idx == coins.length`|`0`|No coins left but sum still > 0 — dead end ❌|

---

## Why Memoization?

Without memo, the same `(idx, sum)` pair gets computed **multiple times** across different recursion branches.

For example, `solve(coins, 2, 1)` might be reached via:

- include coin`[0] → include coin[0] `→ then arrive here
- exclude coin`[0] `→ then arrive here directly

So we store the result in `dp[idx][sum]` the **first time** it's computed, and return it instantly on future visits.

This brings time complexity from **exponential → O(n × sum)**

---

## Recursion Tree (Mental Model)

```
solve(coins, 4, 0)
 ├── INCLUDE coin[0] → solve(coins, 3, 0)   ← same index, reduced sum
 │         ├── INCLUDE coin[0] → solve(coins, 2, 0)
 │         │         └── ... keeps going until sum = 0 ✅
 │         └── EXCLUDE coin[0] → solve(coins, 3, 1)  ← move to next coin
 │
 └── EXCLUDE coin[0] → solve(coins, 4, 1)   ← try from coin[1] onwards
```

Every branch either:

- Reaches `sum = 0` → count it as **1 valid way**
- Runs out of coins with sum still left → count as **0**

---

## Key Insight – Why `idx` stays same on Include?

This is what makes it **unbounded** (unlimited use).

In a 0/1 knapsack, after picking an item you'd move to `idx + 1`.  
Here, we stay at `idx` → same coin can be picked again and again.

---

## Complexity

|Time|`O(n × sum)`|
|Space|`O(n × sum)` for dp table + recursion stack|



----
---
# Tabulation : 
``` java 
class Solution {
    public int count(int coins[], int sum) {

        // dp[i][j] = number of ways to form sum j using first i coins
        int dp[][] = new int[coins.length+1][sum+1];

        // there is always 1 way to form sum 0 (take no coins)
        for(int i = 0; i< coins.length+1;i++){
            dp[i][0] = 1; 
        }

        // fill dp table
        for(int i = 1 ; i< coins.length+1 ; i++){

            // try to form all sums from 1 to target sum
            for(int j = 1 ; j< sum+1; j++){

                // if current coin can be used
                if(j >= coins[i-1]){

                    // two choices:
                    // 1. skip current coin
                    // 2. take current coin again (unbounded use)
                    dp[i][j] = dp[i-1][j] + dp[i][j-coins[i-1]];

                }else{

                    // cannot take coin, so skip
                    dp[i][j] = dp[i-1][j];
                }
            }
        }

        // final answer: number of ways to form target sum
        return dp[coins.length][sum];
    }
}
```