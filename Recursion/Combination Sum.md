Given an array of **distinct** integers `candidates` and a target integer `target`, return _a list of all **unique combinations** of_ `candidates` _where the chosen numbers sum to_ `target`_._ You may return the combinations in **any order**.

The **same** number may be chosen from `candidates` an **unlimited number of times**. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

The test cases are generated such that the number of unique combinations that sum up to `target` is less than `150` combinations for the given input.

**Example 1:**

**Input:** candidates = `[2,3,6,7]`, target = 7
**Output:** `[[2,2,3],[7]]`
**Explanation:**
2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.

**Example 2:**

**Input:** candidates = `[2,3,5]`, target = 8
**Output:** `[[2,2,2,2],[2,3,3],[3,5]]`

# Approach 
• We have an input array `candidates[]` and a number `target`.  
• Create a list `list` to store all valid combinations.  
• Create a temporary list `ans` to store the current combination.  
• Call `solve(candidates, target, list, ans, 0, 0)` to start checking from index `0` and sum `0`.

• In `solve(arr, target, list, ans, idx, sum)`:  
 • If `sum == target` → add a copy of `ans` to `list` (valid combination found) and return.  
 • If `idx == arr.length` or `sum > target` → stop that path and return.  
 • Otherwise:  
  – Add `arr[idx]` to `ans` and call `solve` again with the **same index** (because the same number can be used again) and updated sum.  
  – Remove the last added number (backtrack).  
  – Call `solve` again with the **next index** to try other numbers.

• After all recursive calls, `list` will contain all combinations where numbers add up to `target`.

```java
class Solution {

    public List<List<Integer>> combinationSum(int[] candidates, int target) {

        List<List<Integer>> list = new ArrayList<>();

        List<Integer> ans = new ArrayList<>();3

        solve(candidates , target, list , ans , 0 , 0);

        return list;

    }

    public void solve(int arr[] , int target , List<List<Integer>> list , List<Integer> ans , int idx , int sum ){

        if(sum == target ){ // Base case I (when we found n element sum == target)

            list.add(new ArrayList<>(ans));

            return;

        }

        if(idx == arr.length || target < sum ){ // Base Case II ( when sum is greater than target or when array element ends)

            return;

        }

            ans.add(arr[idx]);
			 // for each element we keep on adding until it either exceed or becomes equal to target
            solve(arr, target , list , ans , idx , sum+arr[idx]); 
          
            ans.remove(ans.size()-1);  

            solve(arr, target , list , ans , idx+1 , sum); // moving to next element

  

    }

}
```