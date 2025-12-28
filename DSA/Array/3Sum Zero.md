Given an integer array nums, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

Notice that the solution set must not contain duplicate triplets.

**Example 1:**

**Input:** nums = `[-1,0,1,2,-1,-4]`
**Output:**` [[-1,-1,2],[-1,0,1]]`
**Explanation:** 
nums`[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0.`
nums`[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0.
nums`[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0.`
The distinct triplets are `[-1,0,1] and [-1,-1,2].`
Notice that the order of the output and the order of the triplets does not matter.

**Example 2:**

**Input:** nums = `[0,1,1]`
**Output:** `[]`
**Explanation:** The only possible triplet does not sum up to 0.

**Example 3:**

**Input:** nums = `[0,0,0]`
**Output:** `[[0,0,0]]`
**Explanation:** The only possible triplet sums up to 0.

**Constraints:**

- `3 <= nums.length <= 3000`
- `-105 <= nums[i] <= 105`


---
---

# Optimal 
**Function: `threeSum(nums[])`**

• We sort the array so we can use two-pointer search and group duplicates together.  
• Create an empty list `list` to store valid triplets.  
• Loop `i` from `0` to `n-1` to pick the first number.  
• **At each i**, check:  
 `if(i > 0 && nums[i-1] == nums[i]) continue;`
 – This skips repeated `i` values so we don't process the same number again.  
 – Because sorted array keeps equal numbers side-by-side, `nums[i-1] == nums[i]` means this value was already used as a first number, so we ignore it.  
• Set two pointers:  
 `j = i + 1` (next element)  
 `k = last index`  
• While `j < k`:  
 – Compute `sum = nums[i] + nums[j] + nums[k]`  
 – If `sum == 0`:  
  • Add `[nums[i], nums[j], nums[k]]` to list  
  • Move both pointers → `j++`, `k--`  
  • Skip duplicates on left → `while(j < k && nums[j] == nums[j-1]) j++`  
  • Skip duplicates on right → `while(k > j && nums[k] == nums[k+1]) k--`  
 – If `sum < 0` → move `j++` to increase sum  
 – If `sum > 0` → move `k--` to decrease sum  
• Stop when pointers cross, continue for next `i`.  
• Return `list` with all unique triplets.

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {

        // list stores all triplets that sum to 0
        List<List<Integer>> list = new ArrayList<>();

        // sort array so we can use two pointers
        Arrays.sort(nums);

        // n stores last index of array
        int n = nums.length - 1;

        // i picks first number
        for(int i = 0; i < n; i++){

            // skip duplicate first number to avoid same triplet
            if(i > 0 && nums[i-1] == nums[i]) continue;

            // j = next number after i
            int j = i + 1;

            // k = last number
            int k = n;

            // two pointers move to find valid triplets
            while(j < k){

                // calculate sum of 3 numbers
                int sum = nums[i] + nums[j] + nums[k];

                // if sum is 0, we found a valid triplet
                if(sum == 0){

                    // convert 3 numbers into a list and add to answer
                    List<Integer> dummy = Arrays.asList(nums[i], nums[j], nums[k]);
                    list.add(dummy);

                    // move both pointers
                    j++;
                    k--;

                    // skip duplicates on j side
                    while(j < k && nums[j] == nums[j-1]) j++;

                    // skip duplicates on k side
                    while(k > j && nums[k] == nums[k+1]) k--;
                }
                // if sum is too small, move j forward to increase sum
                else if(sum < 0){
                    j++;
                }
                // if sum is too big, move k backward to decrease sum
                else{
                    k--;
                }
            }
        }

        // return all found triplets
        return list;
    }
}

```