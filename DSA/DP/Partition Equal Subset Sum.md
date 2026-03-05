Given an integer array `nums`, return `true` _if you can partition the array into two subsets such that the sum of the elements in both subsets is equal or_ `false` _otherwise_.

**Example 1:**

**Input:** nums = `[1,5,11,5]`
**Output:** true
**Explanation:** The array can be partitioned as `[1, 5, 5] and [11].`

**Example 2:**

**Input:** nums = `[1,2,3,5]`
**Output:** false
**Explanation:** The array cannot be partitioned into equal sum subsets.

**Constraints:**

- `1 <= nums.length <= 200`
- `1 <= nums[i] <= 100`

---
---

## Key Observation

If an array can be split into two equal subsets:

subset1 + subset2 = totalSum  
subset1 = subset2

Therefore:

subset1 = totalSum / 2  
subset2 = totalSum / 2

So the problem reduces to:

> **Can we find a subset whose sum = totalSum / 2 ?**

This becomes the **[[Subset sum equal to target]]*,

```java
class Solution {
    // dp[idx][cap] stores whether we can form sum "cap"
    // starting from index "idx"
    static Boolean dp[][];

    public boolean canPartition(int[] nums) {

        int sum = 0;

        // calculate total sum of array elements
        for(int i = 0; i < nums.length  ; i++){
            sum+= nums[i];
        }

        // if total sum is odd, it cannot be divided into two equal subsets
        if(sum%2 != 0){
            return false;
        }

        // target sum each subset must achieve
        int target = sum/2;

        // initializing dp table for memoization
        // rows represent index, columns represent remaining capacity
        dp = new Boolean[nums.length+1][target+1];

        // start recursion from index 0 with full target capacity
        return solve(nums , target , target , 0 );
    }

    public boolean solve(int nums[] , int target , int cap, int idx){

        // if remaining capacity becomes 0, subset with required sum is found
        if(cap == 0){
            return true;
        }

        // if all elements are used but capacity is not 0,
        // subset cannot be formed
        if(idx == nums.length){
            return dp[idx][cap] = false;
        }

        // if this subproblem was already solved, return stored result
        if(dp[idx][cap] != null){
            return dp[idx][cap];
        }

        // if current element can fit in remaining capacity
        if(cap >= nums[idx]){

            // two choices:
            // include current element OR skip it
            return dp[idx][cap] =
                (solve(nums, target, cap - nums[idx], idx + 1) ||
                 solve(nums, target, cap, idx + 1));

        } else {

            // if element is larger than remaining capacity, skip it
            return dp[idx][cap] = solve(nums, target, cap, idx + 1);
        }
    }
}
```