Given an integer array `nums` of **unique** elements, return _all possible_ _subsets_ _(the power set)_.

The solution set **must not** contain duplicate subsets. Return the solution in **any order**.

**Example 1:**

**Input:** nums = `[1,2,3]`
**Output:** `[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]`

**Example 2:**

**Input:** nums =` [0]`
**Output:** `[[],[0]]`

---

# Approach : SAME AS [[SubSet Sum]]

• We have an input array `nums[]`.  
• Create `List<List<Integer>> list` to store all subsets.  
• Create a temporary `List<Integer> ans` to build individual subsets.  
• Call recursive function `solve(nums, ans, list, 0)` starting at index `0`.

• In `solve(nums, ans, list, idx)`:  
 • If `idx == nums.length` → all elements considered → add a **copy** of `ans` to `list`.  
 • Otherwise, for the current element `nums[idx]`:  
  – **Include** it in `ans` → call `solve(nums, ans, list, idx + 1)`.  
  – **Backtrack** by removing the last element from `ans`.  
  – **Exclude** it → call `solve(nums, ans, list, idx + 1)`.

• After all recursion, `list` contains all possible subsets of `nums[]`.

```java
class Solution {

    public List<List<Integer>> subsets(int[] nums) {

        List<List<Integer>> list = new ArrayList<>(); 

        List<Integer> ans = new ArrayList<>();       

        solve(nums, ans, list, 0);                   

        return list;                                

    }
    public void solve(int[] nums, List<Integer> ans, List<List<Integer>> list, int idx) {

        if (idx == nums.length) {//  when we have traversed all elements
            list.add(new ArrayList<>(ans));  // adding current "ANS" list as a subSET

        } else {
		
            ans.add(nums[idx]);
            solve(nums, ans, list, idx + 1); //Including current nums[idx] 
            ans.remove(ans.size() - 1);       

            solve(nums, ans, list, idx + 1); //Excluding Current nums[idx];

        }

    }

}
```