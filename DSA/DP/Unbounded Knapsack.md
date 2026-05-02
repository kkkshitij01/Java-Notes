Given a set of items, each with a weight and a value, represented by the array **wt[]** and **val[]** respectively. Also, a knapsack with a weight limit **capacity**.  
Your task is to fill the knapsack in such a way that we can get the maximum profit. Return the **maximum profit**.

**Note:** Each item can be taken any number of times.

**Examples:**

**Input:** `val[] = [1, 1], wt[] = [2, 1],` capacity = 3
**Output:** 3
**Explanation:** The optimal choice is to pick the 2nd element 3 times.

**Input:** `val[] = [10, 40, 50, 70], wt[] = [1, 3, 4, 5],` capacity = 8
**Output:** 110
**Explanation:** The optimal choice is to pick the 2nd element and the 4th element.  

**Input:** `val[] = [6, 8, 7, 100], wt[] = [2, 3, 4, 5],` capacity = 1
**Output:** 0
**Explanation:** We can't pick any element. Hence, total profit is 0.

**Constraints:**  
1 ≤ val.size() = wt.size() ≤ 1000  
1 ≤ capacity ≤ 1000  
1 ≤` val[i], wt[i] ≤` 100

---
---

# Optimal : 

```java
class Solution {
    public int knapSack(int val[], int wt[], int capacity) {

        // dp[i][j] = maximum value we can get
        // using first i items with capacity j
        int dp[][] = new int[val.length+1][capacity+1];
        
        // fill dp table
        for(int i = 1; i< val.length+1 ; i++){

            // try all capacities from 0 to max capacity
            for(int j= 0 ; j< capacity+1 ; j++){

                // if current item can fit
                if( j>= wt[i-1]){

                    // two choices:
                    // 1. skip current item
                    // 2. take current item again (unbounded, so use same row i)
                    dp[i][j] = Math.max(dp[i-1][j] , val[i-1] + dp[i][j-wt[i-1]]);

                }else{

                    // cannot take item, so skip
                    dp[i][j] = dp[i-1][j];
                }
            }
        }
        
        // final answer: max value with all items and given capacity
        return dp[val.length][capacity];
    }
}
```