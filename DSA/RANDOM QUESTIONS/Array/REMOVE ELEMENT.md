
Given an integer array `nums` and an integer `val`, remove all occurrences of `val` in `nums` .The order of the elements may be changed. Then return _the number of elements in_ `nums` _which are not equal to_ `val`.

Consider the number of elements in `nums` which are not equal to `val` be `k`, to get accepted, you need to do the following things:

- Change the array `nums` such that the first `k` elements of `nums` contain the elements which are not equal to `val`. The remaining elements of `nums` are not important as well as the size of `nums`.
- Return `k`.

**Custom Judge:**

The judge will test your solution with the following code:

`int[] nums = [...]; // Input array`
`int val = ...; // Value to remove`
`int[] expectedNums = [...]; // The expected answer with correct length.// It is sorted with no values equaling val.`
int k = removeElement(nums, val); // Calls your implementation

assert k == expectedNums.length;
`sort(nums, 0, k); // Sort the first k elements of nums
for (int i = 0; i < actualLength; i++) {
    `assert nums[i] == expectedNums[i];`
}

If all assertions pass, then your solution will be **accepted**.

**Example 1:**

**Input:** nums =` [3,2,2,3], val = 3`
**Output:** 2, nums = `[2,2,_,_]`
**Explanation:** Your function should return k = 2, with the first two elements of nums being 2.
It does not matter what you leave beyond the returned k (hence they are underscores).

**Example 2:**

**Input:** nums = `[0,1,2,2,3,0,4,2], val = 2`
**Output:** `5, nums = [0,1,4,0,3,_,_,_]`
**Explanation:** Your function should return k = 5, with the first five elements of nums containing 0, 0, 1, 3, and 4.
Note that the five elements can be returned in any order.
It does not matter what you leave beyond the returned k (hence they are underscores).

---
---
# Optimal : 
### **Part 1: Base / Condition to Accept**

• Start pointer `k = 0` → this will store the next valid position.  
• Loop `i` from `0` to end of array.

---

### **Part 2: Work Done at Each Step**

• If `nums[i]` is **not equal** to `val` → keep it.  
• Copy `nums[i]` into `nums[k]`.  
• Increase `k++` so next valid number goes ahead.

---

### **Part 3: Breaking into Smaller Task**

For every element:  
→ Check if it should stay  
→ If yes, move it to front using `k`

---

## **Complexity**

`Time  : O(n) Space : O(1)`

```java
public int removeElement(int[] nums, int val) {

    // k keeps track of where the next valid (not equal to val) number should go
    int k = 0;

    for (int i = 0; i < nums.length; i++) {

        // If the current number is NOT the value we want to remove
        if (nums[i] != val) {

            // Put that valid number at index k
            // This shifts all good numbers to the front
            nums[k] = nums[i];

            // Increase k to point to next empty spot for valid number
            k++;
        }
        // If nums[i] == val, we skip it and do nothing (this removes it logically)
    }

    // k is now the count of all valid numbers
    return k;
}

```