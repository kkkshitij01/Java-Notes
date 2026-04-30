Given an array **`arr`** of non-negative integers and an integer **`target`**, the task is to count all subsets of the array whose sum is equal to the given `target`.

**Examples:**

**Input**: arr`[] = [5, 2, 3, 10, 6, 8]`, target = 10

**Output:** 3

**Explanation**: The subsets {5, 2, 3}, {2, 8}, and {10} sum up to the target 10.

**Input**: arr`[] = [2, 5, 1, 4, 3]`, target = 10

**Output:** 3
**Explanation**: The subsets {2, 1, 4, 3}, {5, 1, 4}, and {2, 5, 3} sum up to the target 10.

**Input**: arr`[] = [5, 7, 8]`, target = 3  

**Output:** 0

**Explanation**: There are no subsets of the array that sum up to the target 3.

**Input**: arr`[] = [35, 2, 8, 22]`, target = 0  

**Output:** 1

**Explanation**: The empty subset is the only subset with a sum of 0.

**Constraints:**  
1 ≤ arr.size() ≤ 103  
0 ≤ arr`[i]` ≤ 103  
0 ≤ target ≤ 103

---
---


# Brute Force 
```java

class Solution {
    public int perfectSum(int[] nums, int target) {

        // ans array is used to store the final count of valid subsets
        // array is used so that recursion can modify the value
        int ans[] = new int[1];

        // start recursion from index 0 with the full target sum
        solve( nums , target , 0  , ans);

        // return total number of subsets that form the target sum
        return ans[0];
    }

    public void solve(int[] nums ,  int currentSum , int idx , int ans[]){

        // if all elements are processed
        if(nums.length == idx){

            // if remaining sum becomes 0,
            // it means a valid subset was found
          if (currentSum == 0) {
                ans[0]++;
            }

            // stop further recursion
            return;
        }

        // if current element can be included in the subset
        if(currentSum >= nums[idx] ){

            // include the current element
            // reduce the remaining sum
            solve(nums ,  currentSum-nums[idx], idx+1 , ans);
        }

        // exclude the current element
        // move to next index without changing sum
            solve(nums, currentSum, idx+1, ans);
        
    }
}
```



---
---

# Recursive solution


```java
class Solution {
    public int perfectSum(int[] nums, int target) {

        // ans[0] stores total count of valid subsets
        int ans[] = new int[1];

        // start recursion from index 0
        solve(nums , target , 0 , ans);

        // return total count
        return ans[0];
    }

    public void solve(int nums[] , int target , int i , int ans[]){

        // if all elements are checked
        if(i == nums.length){

            // if remaining target becomes 0, valid subset found
            if(target ==0){
                ans[0]++;
            }

            return;
        }
       
        // if current element can be taken
        if(nums[i] <= target){

            // include current element and reduce target
            solve(nums, target-nums[i], i+1 , ans);
        }

        // skip current element
        solve(nums , target , i+1 , ans);
    }
}
```


## How does it work? 

Imagine you have a bag of numbered balls, and you want to pick some of them so their total equals a magic number.

For **each ball**, you have **2 choices**:

1. 🟢 **Pick it** → subtract its value from the target
2. 🔴 **Skip it** → leave the target as it is

You keep doing this for every ball one by one.

When you've gone through **all the balls**:

- If the remaining target is **0** → you found a valid subset! Count it ✅
- If not → that combination didn't work ❌

---

## The Code in Simple Steps

```
solve(nums, target, i, ans)
```

- `i` = which ball (index) you're currently deciding on
- `target` = how much sum you still need
- `ans[0]` = your running count of valid subsets

```
Base Case → if i == nums.length
    if target == 0 → ans[0]++   ✅ found one!
    return

Pick it   → if nums[i] <= target → solve(..., target - nums[i], i+1, ...)
Skip it   →                        solve(..., target,            i+1, ...)
```

> We only "pick" a number if it's **≤ remaining target** (no point picking something too big).

---

## Key Idea

This is a classic **Recursion / Subset / Pick-or-Skip** pattern.  
Every element has 2 choices → total paths = 2ⁿ, but we prune early when the number is too big.

**Tags:** `#recursion` `#subsets` `#pick-or-skip` `#DSA`


---
---


# Tabulation : 
```java
class Solution {
    // Function to calculate the number of subsets with a given sum
    public int perfectSum(int[] nums, int target) {

        // dp[i][j] = number of ways to form sum j using first i elements
        int dp[][] = new int[nums.length+1][target+1];
        
        // there is always 1 way to form sum 0 (take no elements)
        for(int i = 0 ; i<=nums.length ; i++){
            dp[i][0] = 1;
        }

        // fill dp table
        for(int i = 1 ; i<=nums.length; i++){

            // try to form all sums from 0 to target
            for(int j= 0 ; j<= target; j++){

                // if current element can be included
                if(j>= nums[i-1]){

                    // ways = include current element + skip it
                    dp[i][j] = dp[i-1][j-nums[i-1]] + dp[i-1][j];

                }else{

                    // cannot include current element, so skip it
                    dp[i][j] = dp[i-1][j];
                }
                
            }
        }

        // final answer: number of subsets forming target sum
        return dp[nums.length][target];
    }
}
```

---


- **Rows** = considering one more number from the array
- **Columns** = every possible sum from `0` to `target`
- **Each cell** `dp[i][j]` = _"how many subsets using first `i` numbers add up to `j`?"_

---

## Setting Up the Table

```
dp[i][0] = 1  for all i
```

> Sum of **0** can always be made with **empty subset** → so every row's first column = 1 ✅

---

## Filling the Table (The Core Rule)

For each number `nums[i-1]`, for each sum `j`:

```
Can I include nums[i-1]?
  YES (j >= nums[i-1]) → dp[i][j] = dp[i-1][j - nums[i-1]]  ← include it
                                   + dp[i-1][j]              ← skip it

  NO  (j < nums[i-1])  → dp[i][j] = dp[i-1][j]              ← can only skip it
```

> 🧠 Just like the recursive pick-or-skip, but stored in a table instead of call stack!

---

## Visual Example

```
nums = [1, 2, 3],  target = 3
```

||sum=0|sum=1|sum=2|sum=3|
|---|---|---|---|---|
|0 items|1|0|0|0|
|+1|1|1|0|0|
|+2|1|1|1|1|
|+3|1|1|1|**2**|

**Answer = `dp[3][3]` = 2** → subsets `{3}` and `{1,2}` ✅

---

## Recursive vs DP

||Recursive|DP|
|---|---|---|
|Approach|Top-down, explore all paths|Bottom-up, fill a table|
|Repeated work|Yes ❌|No, reuses results ✅|
|Space|Call stack|2D array|

---

**Tags:** `#dp` `#subsets` `#bottom-up` `#tabulation` `#DSA`