
You are given two integer arrays `nums1` and `nums2`, sorted in **non-decreasing order**, and two integers `m` and `n`, representing the number of elements in `nums1` and `nums2` respectively.

**Merge** `nums1` and `nums2` into a single array sorted in **non-decreasing order**.

The final sorted array should not be returned by the function, but instead be _stored inside the array_ `nums1`. To accommodate this, `nums1` has a length of `m + n`, where the first `m` elements denote the elements that should be merged, and the last `n` elements are set to `0` and should be ignored. `nums2` has a length of `n`.

**Example 1:**

**Input:** nums1 = `[1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3`
**Output:**` [1,2,2,3,5,6]`
**Explanation:**` The arrays we are merging are [1,2,3] and [2,5,6].`
The result of the merge is `[1,2,2,3,5,6] `with the underlined elements coming from nums1.

**Example 2:**

**Input:** nums1 = `[1], m = 1, nums2 = [], n = 0`
**Output:** `[1]`
**Explanation:** `The arrays we are merging are [1] and [].`
The result of the merge is `[1].`

**Example 3:**

**Input:** nums1 =` [0], m = 0, nums2 = [1], n = 1`
**Output:** `[1]`
**Explanation:** The arrays we are merging are `[] and [1].`
The result of the merge is `[1].`
Note that because m = 0, there are no elements in nums1. The 0 is only there to ensure the merge result can fit in nums1.

---
---
# Approach
## **Idea**

- Both arrays are already sorted.
    
- We start merging **from the end**, so that existing elements in `nums1` are not overwritten.
    
- We compare the **largest remaining elements** first.
    

---

## **Step-by-Step Explanation**

### **Step 1: Initialize pointers**

• `ptr` → points to the **last index** of nums1  
• `first` → points to the **last valid element** in nums1  
• `second` → points to the **last element** in nums2

This helps us fill nums1 from **right to left**.

---

### **Step 2: Compare elements from the back**

• While both arrays still have elements:

- Compare `nums1[first]` and `nums2[second]`.
    

---

### **Step 3: Place the bigger element**

• If `nums2[second]` is bigger or equal:

- Put it at `nums1[ptr]`
    
- Move `second` and `ptr` backward
    

• Else:

- Put `nums1[first]` at `nums1[ptr]`
    
- Move `first` and `ptr` backward
    

This keeps nums1 sorted.

---

### **Step 4: Copy remaining elements of nums2**

• If nums2 still has elements left:

- Copy them directly into nums1
    

No need to copy nums1 leftovers because they are already in place.

---
## **Time Complexity**

`O(m + n)`

Each element is visited once.


```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {

        // ptr points to the last position of nums1 where we should put elements
        int ptr = m + n - 1;

        // first points to the last valid element in nums1
        int first = m - 1;

        // second points to the last element in nums2
        int second = n - 1;

        // Compare elements from the end of both arrays
        // and put the bigger one at the end of nums1
        while (first >= 0 && second >= 0) {

            // If current element of nums2 is bigger or equal
            if (nums2[second] >= nums1[first]) {

                // Put nums2 element at correct position
                nums1[ptr--] = nums2[second--];
            } 
            else {

                // Put nums1 element at correct position
                nums1[ptr--] = nums1[first--];
            }
        }

        // If nums2 still has elements left,
        // copy them into nums1
        while (second >= 0) {
            nums1[ptr--] = nums2[second--];
        }
    }
}

```