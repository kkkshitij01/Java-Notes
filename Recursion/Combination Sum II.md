
Given a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sum to `target`.

Each number in `candidates` may only be used **once** in the combination.

**Note:** The solution set must not contain duplicate combinations.

**Example 1:**

**Input:** candidates = `[10,1,2,7,6,1,5]`, target = 8
**Output:** 
`[
`[1,1,6],`
`[1,2,5],`
`[1,7],`
`[2,6]`
]`

**Example 2:**

**Input:** candidates = `[2,5,2,1,2]`, target = 5
**Output:** 
`[`
`[1,2,2],`
`[5]`
]



# BruteForce Approach 

• We have an input array `arr[]` and a number `target`.  
• **Sort** `arr[]` to group duplicates together.  
• Create a list `list` to store all unique combinations.  
• Create a temporary list `ans` to build the current combination.  
• Create a `Set<List<Integer>> set` to automatically remove duplicate combinations.  
• Call the recursive function `solve(arr, target, 0, set, ans)` starting from index `0`.

• In `solve(arr, target, idx, set, ans)`:  
 • If `target == 0` → a valid combination is found → add a **copy** of `ans` to `set` and return.  
 • If `idx == arr.length` or `target < 0` → stop this path and return.  
 • Otherwise:  
  – **Include** `arr[idx]` in `ans` → call `solve(arr, target - arr[idx], idx + 1, set, ans)` (move to next index).  
  – **Backtrack** → remove the last element from `ans`.  
  – **Exclude** `arr[idx]` → call `solve(arr, target, idx + 1, set, ans)` to try combinations without it.

• After recursion, convert `set` to `list` → contains **all unique combinations** that sum to `target`.

```java
class Solution {

    public List<List<Integer>> combinationSum2(int[] arr, int target) {

        Arrays.sort(arr);

        List<List<Integer>> list = new ArrayList<>();

        List<Integer> ans = new ArrayList<>();

        Set<List<Integer>> set = new HashSet<>();

        solve(arr, target, 0 ,set , ans );

        list.addAll(set);

        return list ; 

    }

public void solve( int arr[] , int target ,  int idx, Set<List<Integer>>  set , List<Integer> ans){

    if( target == 0){

        set.add(new ArrayList<>(ans));

        return;

    } 

    if(arr.length == idx|| target< 0 ){

        return;

    }

    ans.add(arr[idx]);

    solve( arr, target-arr[idx] , idx+1,  set , ans );

    ans.remove(ans.size()-1);

    solve( arr, target , idx+1,  set , ans );

}

}
```