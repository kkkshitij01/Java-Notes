You are given an integer array `nums` of `2 * n` integers. You need to partition `nums` into **two** arrays of length `n` to **minimize the absolute difference** of the **sums** of the arrays. To partition `nums`, put each element of `nums` into **one** of the two arrays.

Return _the **minimum** possible absolute difference_.

**Example 1:**

![example-1](https://assets.leetcode.com/uploads/2021/10/02/ex1.png)

**Input:** nums = `[3,9,7,3]`
**Output:** 2
**Explanation:** One optimal partition is: `[3,9] and [7,3]`.
The absolute difference between the sums of the arrays is abs((3 + 9) - (7 + 3)) = 2.

**Example 2:**

**Input:** nums =` [-36,36]`
**Output:** 72
**Explanation:** One optimal partition is: `[-36] and [36].`
The absolute difference between the sums of the arrays is abs((-36) - (36)) = 72.

**Example 3:**

![example-3](https://assets.leetcode.com/uploads/2021/10/02/ex3.png)

**Input:** nums =` [2,-1,0,4,-2,-9]`
**Output:** 0
**Explanation:** One optimal partition is: `[2,4,-9] and [-1,0,-2].`
The absolute difference between the sums of the arrays is abs((2 + 4 + -9) - (-1 + 0 + -2)) = 0.

**Constraints:**

- `1 <= n <= 15`
- `nums.length == 2 * n`
- `-107 <= nums[i] <= 107`


---
---

# Brute Force : 
``` java
class Solution {
    public int minimumDifference(int[] nums) {

        // min[0] stores the minimum difference found
        int min[] = new int[1];
        min[0] = Integer.MAX_VALUE;

        // calculate total sum of array
        int sum = 0; 
        for(int num : nums){
            sum+= num;
        }

        // start recursion from index 0
        solve(nums, 0 , sum ,0 , 0 , min);

        // return minimum difference
        return min[0];
    }

    public void solve(int nums[] , int idx , int totalSum, int count , int aSum, int min[]){

        // if we selected n/2 elements for first subset
        if(count == nums.length/2){

            // remaining elements form second subset
            int bSum = totalSum-aSum;

            // calculate difference between two subsets
            int diff = Math.abs(aSum - bSum);

            // update minimum difference
            if(min[0]> diff){
                min[0] =diff;
            }

            return;
        }

        // if all elements are processed, stop
        if(idx == nums.length ){
            return;
        }  

        // include current element in first subset
        solve(nums, idx+1 , totalSum , count+1 , aSum+nums[idx] , min);

        // skip current element (goes to second subset)
        solve(nums, idx+1 , totalSum , count , aSum , min);
    }
}
```

