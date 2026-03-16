Given an array `nums` with `n` objects colored red, white, or blue, sort them **[in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** so that objects of the same color are adjacent, with the colors in the order red, white, and blue.

We will use the integers `0`, `1`, and `2` to represent the color red, white, and blue, respectively.

You must solve this problem without using the library's sort function.

**Example 1:**

**Input:** nums = `[2,0,2,1,1,0]`
**Output:** `[0,0,1,1,2,2]`

**Example 2:**

**Input:** nums =` [2,0,1]`
**Output:**` [0,1,2]`

---
---

# Dutch National Flag algorithm : 

```java
class Solution {
    public void sortColors(int[] nums) {

        // i → position where next 0 should be placed
        int i = 0; 

        // j → current element being checked
        int j = 0; 

        // k → position where next 2 should be placed
        int k = nums.length-1;

        // continue until current pointer crosses the right boundary
        while(j<=k){

            // if element is 1, it is already in correct middle region
            if(nums[j] == 1){
                j++;

            // if element is 0, swap it with position i
            // so that 0 moves to the left section
            }else if(nums[j]== 0){

                int temp = nums[i];
                nums[i]= nums[j];
                nums[j]= temp;

                // move both pointers forward
                // because left side now has a correct 0
                j++;
                i++;

            // if element is 2, move it to the right section
            }else{

                // replace current element with element from end
                nums[j] = nums[k];

                // put 2 at the correct right position
                nums[k] =2; 

                // shrink right boundary
                k--;
            }
        }
    }
}
```
