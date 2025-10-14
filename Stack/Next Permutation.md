A **permutation** of an array of integers is an arrangement of its members into a sequence or linear order.

- For example, for `arr = [1,2,3]`, the following are all the permutations of `arr`: `[1,2,3], [1,3,2], [2, 1, 3], [2, 3, 1], [3,1,2], [3,2,1]`.

The **next permutation** of an array of integers is the next lexicographically greater permutation of its integer. More formally, if all the permutations of the array are sorted in one container according to their lexicographical order, then the **next permutation** of that array is the permutation that follows it in the sorted container. If such arrangement is not possible, the array must be rearranged as the lowest possible order (i.e., sorted in ascending order).

- For example, the next permutation of `arr = [1,2,3]` is `[1,3,2]`.
- Similarly, the next permutation of `arr = [2,3,1]` is `[3,1,2]`.
- While the next permutation of `arr = [3,2,1]` is `[1,2,3]` because `[3,2,1]` does not have a lexicographical larger rearrangement.

Given an array of integers `nums`, _find the next permutation of_ `nums`.


**Example 1:**
**Input:**` nums = [1,2,3]`
**Output:** `[1,3,2]`

**Example 2:**
**Input:** nums = `[3,2,1]`
**Output:** `[1,2,3]`

---
# Approach :

• We have an input array `nums[]`.  
• Create a variable `piv = -1` to store the position where change will happen.

• Step 1: **Find the pivot**  
 • Start from the second last element and move left.  
 • Find the first index `i` where `nums[i] < nums[i + 1]`.  
 • Store that index in `piv` and stop the loop.

• Step 2: **If no pivot found**  
 • If `piv == -1`, the array is already the largest permutation.  
 • Reverse the whole array to get the smallest permutation and return.

• Step 3: **Find the next greater element to swap with pivot**  
 • Start from the end of the array and move left.  
 • Find the first number that is greater than `nums[piv]`.  
 • Swap both numbers.

• Step 4: **Reverse the part after the pivot**  
 • Reverse all elements from `piv + 1` to the end.  
 • This makes the sequence after pivot the smallest possible (sorted order).

• The array `nums[]` now becomes the **next permutation** in order.

```java

class Solution {

    public void nextPermutation(int[] nums) {

        int piv = -1;

        for (int i = nums.length - 2; i >= 0; i--) { 

            if (nums[i] < nums[i + 1]) {
                piv = i;
                break;

            }

        }

        if (piv == -1 ) { // if piv is not updated means nums[] contains the largest permutation so we just reverse the array & return
            reverseArray(nums, 0, nums.length - 1);
            return;
        }

        for (int i = nums.length - 1; i > piv; i--) {

            if (nums[piv] < nums[i]) { // when we find the smallest greater element than piv we swap it with piv 

                int temp = nums[piv];

                nums[piv] = nums[i];

                nums[i] = temp;

                break;

            }

        }

        reverseArray(nums, piv + 1, nums.length - 1); // to form the smallest next permutation we sort the subarray starting from piv+1

    }

// Function to reverse the array 
    public void reverseArray(int nums[], int start, int end) {

        while (start < end) {

            int temp = nums[start];

            nums[start] = nums[end];

            nums[end] = temp;

            start++;

            end--;

        }

    }

}
```