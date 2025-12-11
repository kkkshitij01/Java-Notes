Given an array of integers `nums` and an integer `target`, return _indices of the two numbers such that they add up to `target`_.

You may assume that each input would have **_exactly_ one solution**, and you may not use the _same_ element twice.

You can return the answer in any order.

**Example 1:**

**`Input:** nums = [2,7,11,15], target = 9`
`**Output:** [0,1]`
**`Explanation:** Because nums[0] + nums[1] == 9, we return [0, 1]`.

**Example 2:**

**Input:** nums = `[3,2,4], target = 6`
`Output:[1,2]`

**Example 3:**

**Input:** nums =` [3,3], target = 6`
**Output:** `[0,1]`


---
# BruteForce: 

• **Step 1:**  
 → Get the length of the array `n = nums.length`.

• **Step 2:**  
 → Use two nested loops to check every possible pair:  
  – Outer loop (`i`) → picks the first number.  
  – Inner loop (`j`) → checks all numbers after `i`.

• **Step 3:**  
 → If `nums[i] + nums[j] == target`,  
  ✔ Return `{i, j}` immediately — found the answer.

• **Step 4:**  
 → If no pair matches after checking all possibilities,  
  return a default `{1, 1}` (or could use `{−1, −1}` for no match).

### Complexity
• **Time Complexity:** O(n²)  
 → Every pair is checked once.
• **Space Complexity:** O(1)  
 → No extra data structures used.


```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int n = nums.length; 
        
        // Check every possible pair of numbers
        for (int i = 0; i < n - 1; i++) {
            for (int j = i + 1; j < n; j++) {
                
                // If the pair adds up to the target, return their indices
                if (nums[i] + nums[j] == target) {
                    return new int[]{i, j};
                }
            }
        }

        // If no such pair exists, return a default value
        return new int[]{1, 1};
    }
}

```


---

# Better Approach :

• **Step 1:**  
 → Create a `HashMap<Integer, Integer>` to store each number and its index.  
  – Key → number value.  
  – Value → index of that number.
• **Step 2:**  
 → Loop through the array using index `i`.  
 → For each element `nums[i]`, calculate `toFind = target - nums[i]`.  
  – This is the number we need to reach the target sum.
• **Step 3:**  
 → If `toFind` already exists in the map →  
  ✔ Return the indices `{map.get(toFind), i}` → because we found the pair.
• **Step 4:**  
 → If not found, store the current number in the map → `map.put(nums[i], i)`.
• **Step 5:**  
 → If loop finishes with no pair found → return `[-1, -1]`.
###  Complexity
• **Time Complexity:** O(n)  
 → Each lookup and insertion in HashMap is O(1).
• **Space Complexity:** O(n)  
 → HashMap stores at most all elements once.


```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        // Create a HashMap to store each number and its index
        HashMap<Integer, Integer> map = new HashMap<>();
        
        // Loop through all numbers
        for (int i = 0; i < nums.length; i++) {
            int toFind = target - nums[i];  // The number we need to find to make the sum = target 
               
            // If the required number is already in the map, return the pair of indices
            if (map.containsKey(toFind)) {
                return new int[]{map.get(toFind), i};
            } 
            else {
                // Otherwise, store the current number with its index
                map.put(nums[i], i);
            }
        }
        // If no such pair is found, return [-1, -1]
        return new int[]{-1, -1};
    }
}

```


---

# Optimal Approach : 

• Sort the array first to apply the two-pointer approach.  
• Initialize two pointers: `start = 0`, `end = nums.length - 1`.  
• While `start < end`:  
 → Calculate `sum = nums[start] + nums[end]`.  
 → If `sum == target`, return `{start, end}`.  
 → If `sum < target`, move `start++` (need a larger sum).  
 → If `sum > target`, move `end--` (need a smaller sum).  
• If no valid pair is found, return `{-1, -1}`.  
• **Time Complexity:** O(n log n) → due to sorting.  
• **Space Complexity:** O(1) → uses constant extra space.  
• **Note:** Sorting loses original indices — valid only if indices aren’t required.


```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        // Sort the array first so we can use the two-pointer technique
        Arrays.sort(nums);

        int start = 0;                  // Left pointer
        int end = nums.length - 1;      // Right pointer

        // Move pointers inward until they meet
        while (start < end) {
            int sum = nums[start] + nums[end];  // Current pair sum

            if (sum == target) {
                return new int[]{start, end};   // Found the pair
            } else if (sum < target) {
                start++;                        // Sum too small → move left pointer right
            } else {
                end--;                          // Sum too big → move right pointer left
            }
        }

        // If no valid pair is found
        return new int[]{-1, -1};
    }
}

```