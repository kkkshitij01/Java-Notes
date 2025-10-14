Given an array `nums` of distinct integers, return all the possible permutations. You can return the answer in **any order**.

**Example 1:**

**Input:** nums = `[1,2,3]`
**Output:** `[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]`

**Example 2:**

**Input:** nums = `[0,1]`
**Output:** `[[0,1],[1,0]]`

**Example 3:**

**Input:** nums = `[1]`
**Output:** `[[1]]`

# Approach

• We have an input array `nums[]`.  
• Create a list `list` to store all possible permutations.  
• Call the recursive function `solve(nums, 0, list)` to start building permutations from index `0`.

• In `solve(nums, idx, list)`:  
 • **Base case:** If `idx == nums.length` → all positions in the array are fixed →  
  – Create a new list `temp` and copy all elements from `nums` into it.  
  – Add `temp` to `list` and return.  
 • **Recursive case:** Loop from `i = idx` to `nums.length - 1`:  
  – **Swap** `nums[i]` and `nums[idx]` → put a new number at the current position `idx`.  
  – **Recursive call:** `solve(nums, idx + 1, list)` → build permutations for the next positions.  
  – **Backtrack:** Swap back `nums[i]` and `nums[idx]` → restore the original array for the next iteration.

• `swap(nums, a, b)` → a helper function to exchange two elements in the array.

• After all recursion finishes, `list` will contain **all possible arrangements** (permutations) of `nums[]`.

### Time complexity : - n* n!


```java
class Solution {

    public List<List<Integer>> permute(int[] nums) {

        List<List<Integer>> list = new ArrayList<>();

        solve(nums, 0 , list );

        return list;

    }
    
// Thought Process: we consider the input array as an empty array , and at each index "IDX" we put all other element at the "IDX"
// then call recursion to next index "IDX+1 " after that , make the array as it was 

    public void solve(int nums[] , int idx  ,  List<List<Integer>> list ){

        if(idx == nums.length){

            List<Integer> temp = new ArrayList<>();

            for(int n : nums){

                temp.add(n);

            }

            list.add(temp);

            return;

        }

        for(int i = idx ; i<nums.length ; i++){ // for idx i fix each element one by one 

            swap(nums, i, idx); 

            solve( nums, idx+1 , list); 

            swap(nums,i, idx);

        }

  

    }

    public void swap(int nums[] , int a , int b){

        int temp = nums[a];

        nums[a] = nums[b];

        nums[b] = temp;

    }

}
```

