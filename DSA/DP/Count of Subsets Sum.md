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