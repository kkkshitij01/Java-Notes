You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return _the fewest number of coins that you need to make up that amount_. If that amount of money cannot be made up by any combination of the coins, return `-1`.

You may assume that you have an infinite number of each kind of coin.

**Example 1:**

**Input:** coins = `[1,2,5],` amount = 11
**Output:** 3
**Explanation:** 11 = 5 + 5 + 1

**Example 2:**

**Input:** coins =` [2]`, amount = 3
**Output:** -1

**Example 3:`**

**Input:** coins = `[1]`, amount = 0
**Output:** 0

**Constraints:**

- `1 <= coins.length <= 12`
- `1 <= coins[i] <= 231 - 1`
- `0 <= amount <= 104`

---
---
# Recursive Code : 

```java 
class Solution {
    public int coinChange(int[] coins, int amount) {

        // ans[0] stores minimum coins needed
        int ans[] = new int[1];
        ans[0] = Integer.MAX_VALUE;

        // start recursion from index 0 with count = 0
        solve(coins , amount , 0 , ans,0);

        // if no solution found, return -1
        return ans[0]!= Integer.MAX_VALUE ? ans[0] : -1;
    }

    public void solve(int [] coins , int amount , int idx , int ans[]  , int count ){

        // if amount becomes 0, we found a valid combination
        if(amount == 0 ){

            // update minimum coins used
            if(count < ans[0]){
                ans[0] = count;
            }
            return;
        }

        // if all coins are used and amount is not 0, stop
        if( idx == coins.length ){
            return;
        }

        // if current coin can be taken
        if(coins[idx]<= amount){

            // take current coin and stay at same index (unbounded use)
            solve(coins, amount-coins[idx] , idx , ans , count+1);
        }

        // skip current coin and move to next
        solve(coins , amount , idx+1 , ans , count);
    }
}
```

---
---


# Tabulation :

```java
class Solution {
    public int coinChange(int[] coins, int amount) {

        // dp[i][j] = minimum coins needed to make sum j using first i coins
        int dp[][] = new int[coins.length+1][amount+1];

        // if no coins are available, it is impossible to form any sum > 0
        for(int i = 0; i< amount+1 ; i++){
            dp[0][i] = Integer.MAX_VALUE-1;
        }

        // sum 0 always needs 0 coins
        for(int i = 0; i< coins.length +1 ; i++){
            dp[i][0] = 0;
        }

        // initialize for first coin
        // we can only use coins[0] multiple times
        for(int i = 1 ; i< amount+1 ; i++){
            dp[1][i]= i%coins[0]==0 ? i/coins[0] :  Integer.MAX_VALUE-1;
        }

        // fill dp table
        for(int i = 2 ; i< coins.length+1 ; i++){

            // try to form all sums from 1 to amount
            for(int j = 1; j< amount+1 ; j++){

                // if current coin can be used
                if(j>= coins[i-1]){

                    // two choices:
                    // 1. skip current coin
                    // 2. take current coin (unbounded, so stay at same row)
                    dp[i][j] = Math.min(dp[i-1][j] , 1+ dp[i][j-coins[i-1]]);

                }else{

                    // cannot take coin, so skip
                    dp[i][j]= dp[i-1][j];
                }
            }
        }

        // if result is still INF, no solution exists
        return dp[coins.length][amount]== Integer.MAX_VALUE-1 ? -1 : dp[coins.length][amount];
    }
}
```


This is the **unbounded knapsack** style — each coin can be used unlimited times.  
We build a 2D table `dp[i][j]` = minimum coins needed using **first i coins** to make amount **j**.

---

## Table Setup (Initialization)

### Why these base values?

|Condition|Value|Reason|
|---|---|---|
|`dp[0][j]` for all j > 0|`Integer.MAX_VALUE - 1`|0 coins available, can't make any amount → infinity|
|`dp[i][0]` for all i|`0`|Amount = 0, need 0 coins always|
|`dp[1][j]`|`j/coins[0]` if divisible, else `∞`|With only 1st coin, only amounts divisible by it are reachable|

> ⚠️ We use `MAX_VALUE - 1` instead of `MAX_VALUE` to safely do `1 + dp[...]` without integer overflow.

---

## Core Logic (Filling the Table)

For every coin `i` and every amount `j`:

```
if j >= coins[i-1]:                          ← coin fits in current amount
    dp[i][j] = min(
        dp[i-1][j],                          ← EXCLUDE: don't use this coin
        1 + dp[i][j - coins[i-1]]            ← INCLUDE: use this coin, stay same row (reuse allowed)
    )
else:
    dp[i][j] = dp[i-1][j]                   ← coin too big, can't use it → carry forward
```


## Dry Run Example

`coins = [1, 2, 5]`, `amount = 5`

| |0|1|2|3|4|5|
|---|---|---|---|---|---|---|
|0 coins|0|∞|∞|∞|∞|∞|
|coin=1|0|1|2|3|4|5|
|coin=2|0|1|1|2|2|3|
|coin=5|0|1|1|2|2|**1**|

Answer = `dp[3][5]` = **1** (just one coin of 5)

---

## Final Answer

```java
return dp[coins.length][amount] == Integer.MAX_VALUE - 1 ? -1 : dp[coins.length][amount];
```

If the last cell is still infinity → amount was unreachable → return `-1`

---

## Difference from Coin Change 2

| |Coin Change 1|Coin Change 2|
|---|---|---|
|Goal|**Minimum** coins|**Count** of ways|
|DP value|`min(exclude, 1 + include)`|`exclude + include`|
|Impossible case|return `-1`|return `0`|
|Style|Bottom-up tabulation|Top-down memoization|

---

## Complexity

|Time|`O(n × amount)`|
|Space|`O(n × amount)`|
