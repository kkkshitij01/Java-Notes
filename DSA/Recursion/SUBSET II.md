Given an integer array `nums` that may contain duplicates, return _all possible_ _subsets_ _(the power set)_.

The solution set **must not** contain duplicate subsets. Return the solution in **any order**.

**Example 1:**

**Input:** nums = `[1,2,2]`
**Output:**` [[],[1],[1,2],[1,2,2],[2],[2,2]]`

**Example 2:**

**Input:** nums = `[0]`
**Output:** `[[],[0]]`

---

# Approach : Same AS [[Combination Sum II]] Optimal Approch

• We have an input array `nums[]` that may contain duplicate elements.  
• First, we **sort** `nums` to group duplicates together.  
• Create a main list `list` to store all unique subsets.  
• Create a temporary list `temp` (or `ans`) to build each subset during recursion.  
• Call the recursive function `solve(nums, list, temp, 0)` starting from index `0`.

• In `solve(nums, list, ans, idx)`:  
 • If `idx == nums.length` → all elements processed → add a **copy** of `ans` to `list` and return.  
 • Otherwise:  
  – **Include** the current element `nums[idx]` → add it to `ans`.  
  – Call `solve(nums, list, ans, idx + 1)` to move to the next element.  
  – **Backtrack** by removing the last added element.  
  – **Skip duplicates:** use a `while` loop to move `idx` forward while consecutive elements are equal (`nums[idx] == nums[idx+1] && idx<nums.length-1`) .  
  – **Exclude** the current element → call `solve(nums, list, ans, idx + 1)` again to generate subsets without it.

• After recursion completes, `list` contains **all unique subsets** without duplicates.



```java
class Solution {

    public List<List<Integer>> subsetsWithDup(int[] nums) {

        Arrays.sort(nums);

        List<List<Integer>> list = new ArrayList<>();

        List<Integer> temp = new ArrayList<>();

        solve(nums, list , temp , 0);

        return list;

    }

    public void solve(int nums[] , List<List<Integer>> list , List<Integer> ans , int idx){

        if(nums.length == idx){

            list.add(new ArrayList<>(ans));

            return;

        }

        ans.add(nums[idx]);

        solve(nums, list, ans , idx+1);

        ans.remove(ans.size()-1);

        while( idx<nums.length-1 &&  nums[idx] == nums[idx+1]){ // increment idx while we have consecutive same number
            idx++;
        }

        solve(nums, list, ans , idx+1);

    }

}
```