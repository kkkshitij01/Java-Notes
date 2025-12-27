Given an integer array `nums` sorted in **non-decreasing order**, remove the duplicates [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) such that each unique element appears only **once**. The **relative order** of the elements should be kept the **same**.

Consider the number of _unique elements_ in `nums` to be `k**​​​​​​​**`​​​​​​​. After removing duplicates, return the number of unique elements `k`.

The first `k` elements of `nums` should contain the unique numbers in **sorted order**. The remaining elements beyond index `k - 1` can be ignored.

**Custom Judge:**

The judge will test your solution with the following code:

i`nt[] nums = [...]; // Input array`
`int[] expectedNums = [...]; // The expected answer with correct length`

`int k = removeDuplicates(nums); // Calls your implementation`

`assert k == expectedNums.length;`
for (int i = 0; i < k; i++) {
    `assert nums[i] == expectedNums[i];`
}

If all assertions pass, then your solution will be **accepted**.

**Example 1:**

**Input:** nums = `[1,1,2]`
**Output:** 2`, nums = [1,2,_]`
**Explanation:** Your function should return k = 2, with the first two elements of nums being 1 and 2 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).

**Example 2:**

**Input:** nums =` [0,0,1,1,1,2,2,3,3,4]`
**Output:** 5, nums = `[0,1,2,3,4,_,_,_,_,_]`
**Explanation:** Your function should return k = 5, with the first five elements of nums being 0, 1, 2, 3, and 4 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).


---
---

# Optimal :
### **Step 1: Initialize pointer `i`**

• `i` points to the **last unique element** found so far.  
• Initially, `i = 0` because the first element is always unique.

---

### **Step 2: Use pointer `j` to scan the array**

• Start `j` from index `1`.  
• `j` moves forward to check every element.

---

### **Step 3: Compare elements**

• If `nums[i] != nums[j]`  
→ It means a **new unique element** is found.

---

### **Step 4: Store the unique element**

• Increase `i` by 1.  
• Copy `nums[j]` to `nums[i]`.

This keeps all unique elements **grouped at the front** of the array.

---

### **Step 5: Continue till the end**

• `j` keeps moving until the array ends.  
• `i` only moves when a new unique value is found.

---

### **Step 6: Return the answer**

• Total number of unique elements = `i + 1`.

```java
class Solution {
    public int removeDuplicates(int[] nums) {

        // i points to the position of last unique element
        int i = 0; 

        // j moves through the array to check each element
        for(int j = 1; j < nums.length; j++){

            // If current element is different from last unique element
            if(nums[i] != nums[j]){

                // Move i forward and store the new unique element there
                nums[++i] = nums[j];
            }
        }

        // i + 1 gives the total number of unique elements
        return i + 1;
    }
}

```