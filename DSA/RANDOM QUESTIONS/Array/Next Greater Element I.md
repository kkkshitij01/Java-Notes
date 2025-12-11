
The **next greater element** of some element `x` in an array is the **first greater** element that is **to the right** of `x` in the same array.

You are given two **distinct 0-indexed** integer arrays `nums1` and `nums2`, where `nums1` is a subset of `nums2`.

For each `0 <= i < nums1.length`, find the index `j` such that `nums1[i] == nums2[j]` and determine the **next greater element** of `nums2[j]` in `nums2`. If there is no next greater element, then the answer for this query is `-1`.

Return _an array_ `ans` _of length_ `nums1.length` _such that_ `ans[i]` _is the **next greater element** as described above._

**Example 1:**

**Input:** nums1 =` [4,1,2], nums2 = [1,3,4,2]`
**Output:** `[-1,3,-1]`
**Explanation:** The next greater element for each value of nums1 is as follows:
- 4 is underlined in nums2 =` [1,3,4,2]. There is no next greater element, so the answer is -1.`
- 1 is underlined in nums2 = `[1,3,4,2]. The next greater element is 3.`
- 2 is underlined in nums2 = `[1,3,4,2]. There is no next greater element, so the answer is -1.`

**Example 2:**

**Input:** nums1 = `[2,4], nums2 = [1,2,3,4]`
**Output:** `[3,-1]`
**Explanation:** The next greater element for each value of nums1 is as follows:
- 2 is underlined in nums2 = `[1,2,3,4].` The next greater element is 3.
- 4 is underlined in nums2 = `[1,2,3,4]`. There is no next greater element, so the answer is -1.

---

# **BruteForce**

```java
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {

        int first = nums1.length;
        int second = nums2.length;
        int ans[] = new int[first];
        for (int i = 0; i < first; i++) {

            // For each element in nums1, search for it inside nums2
            for (int j = 0; j < second; j++) {

                // Found the position of nums1[i] inside nums2
                if (nums1[i] == nums2[j]) {

                    // Look for the next element greater than nums1[i] on the right side of nums2
                    for (int k = j + 1; k < second; k++) {

                        // First greater element found
                        if (nums2[k] > nums1[i]) {
                            ans[i] = nums2[k];
                            break;
                        }
                    }

                    // If no greater element was found, ans[i] is still 0 → so mark it as -1
                    // (0 cannot be a valid next greater element here)
                    if (ans[i] == 0) {
                        ans[i] = -1;
                        break;
                    }
                }
            }
        }
        return ans;
    }
}

```


---
# **Optimal** 
### 1. Use a stack + map to precompute results for nums2

We scan `nums2` once and compute the next greater element for every number.

- `st` → a stack holding numbers in decreasing order
    
- `map` → stores:
    
    `number → next greater number`
    

---

### 2. Traverse nums2

For each `num` in nums2:

#### A. While stack is not empty AND top < num

- That means `num` is the **next greater** for `st.peek()`
    
- Pop from stack, and assign in map:
    
    `map.put(poppedValue, num)`
    

#### B. Push current number

- Because it might have a next greater value later.
    

This creates a **monotonic decreasing stack**.

---

### 3. Build the answer for nums1

Create array `arr` of same size as `nums1`.

For each element in `nums1`:

- If it exists in `map` → store mapped value
    
- Else → store `-1`
    

`arr[i] = map.getOrDefault(nums1[i], -1)`

---

### 4. Return the final array

This array contains next greater elements for each number in nums1.

```java
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {

        // Map stores: number → its next greater number
        Map<Integer, Integer> map = new HashMap<>();

        // Stack keeps decreasing sequence while scanning nums2
        Stack<Integer> st = new Stack<>();

        // Build next greater map for all elements in nums2
        for (int num : nums2) {

            // If current number is greater than stack top,
            // it is the "next greater" for the stack top element.
            while (!st.isEmpty() && st.peek() < num) {
                int val = st.pop();
                map.put(val, num);
            }

            // Push current number to stack
            st.push(num);
        }

        // For all remaining stack elements, no next greater exists → they stay -1 by default.

        int arr[] = new int[nums1.length];

        // Construct answer for nums1 using the map
        for (int i = 0; i < nums1.length; i++) {
            // Get next greater from map, else default to -1
            arr[i] = map.getOrDefault(nums1[i], -1);
        }

        return arr;
    }
}


```