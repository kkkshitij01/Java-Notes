Given a rod of length **n** inches and an array **price`[]`**, where **price`[i]`** denotes the value of a piece of length **i**. Your task is to determine the **maximum** value obtainable by **cutting up** the rod and selling the **pieces**.

**Note:** n = size of price, and price`[]` is **1-indexed** array.

**Example:**

**Input:** price`[] = [1, 5, 8, 9, 10, 17, 17, 20]  `
**Output:** 22  
**Explanation:** The maximum obtainable value is 22 by cutting in two pieces of lengths 2 and 6, i.e., 5 + 17 = 22.

**Input:** price`[] = [3, 5, 8, 9, 10, 17, 17, 20]  `
**Output:** 24  
**Explanation:** The maximum obtainable value is 24 by cutting the rod into 8 pieces of length 1, i.e, 8*price`[1] `= 8*3 = 24.  

**Input:** price`[] = [3]  `
**Output:** 3  
**Explanation:** There is only 1 way to pick a piece of length 1.

**Constraints:**  
1 ≤ price.size() ≤ 103  
1 ≤ price`[i] `≤ 106

---
---


# Optimal : completely same as unbounded knapsack

```java
class Solution {
    public int cutRod(int[] price) {

        // n = length of rod
        int n = price.length; 

        // dp[i][j] = max profit using rod pieces of length up to i
        // to make total rod length j
        int dp[][] = new int[n+1][n+1];

        // fill dp table
        for(int i = 1 ; i< n+1; i++){

            // try all rod lengths from 1 to n
            for(int j = 1; j< n+1 ; j++){

                // if current piece length i can be used
                if(j >= i){

                    // two choices:
                    // 1. skip this piece length
                    // 2. take this piece and stay on same i (unbounded use)
                    dp[i][j] = Math.max(dp[i-1][j] , dp[i][j-i] + price[i-1]);

                }else{

                    // cannot use this piece length, skip it
                    dp[i][j] = dp[i-1][j];
                }
            }
        }

        // final answer: max profit for rod length n
        return dp[n][n];
        
    }
}
```