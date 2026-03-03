Given an array of integers `heights` representing the histogram's bar height where the width of each bar is `1`, return _the area of the largest rectangle in the histogram_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/04/histogram.jpg)

**Input:** heights =` [2,1,5,6,2,3]`

**Output:** 10

**Explanation:** The above is a histogram where width of each bar is 1.
The largest rectangle is shown in the red area, which has an area = 10 units.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/04/histogram-1.jpg)

**Input:** heights = `[2,4]`

**Output:** 4


---
---
# Optimal Solution : [[85. Maximal Rectangle]]
# Average Solution: 
### **Step 1: Find Nearest Smaller to Left**

- Create an array `left[]`
    
- Use a stack to store indices
    
- Move from **left to right**
    
- While stack top bar height ≥ current bar → pop it
    
- If stack becomes empty → no smaller bar → store `-1`
    
- Else → store stack top index
    
- Push current index into stack
    

This gives:

> Left boundary for every bar

---

### **Step 2: Find Nearest Smaller to Right**

- Clear the stack
    
- Create array `right[]`
    
- Move from **right to left**
    
- While stack top bar height ≥ current bar → pop it
    
- If stack becomes empty → no smaller bar → store `heights.length`
    
- Else → store stack top index
    
- Push current index into stack
    

This gives:

> Right boundary for every bar

---

### **Step 3: Calculate Maximum Area**

For every bar `i`:

- Width = `right[i] - left[i] - 1`
    
- Area = `width * heights[i]`
    
- Keep updating the maximum area
    

---

## **Why `-1` and `heights.length` are used**

- `-1` means bar can extend fully to the **left end**
    
- `heights.length` means bar can extend fully to the **right end**
    
- This makes width calculation easy and correct
    

---

## **Final Formula Used**

`Area = (right[i] - left[i] - 1) * heights[i]`

---

## **Time & Space Complexity**

|Type|Complexity|Reason|
|---|---|---|
|**TC**|`O(n)`|Each bar is pushed & popped once|
|**SC**|`O(n)`|Stack + left & right arrays|
``` java
class Solution {
    public int largestRectangleArea(int[] heights) {

        // left[i] will store index of nearest smaller bar on the left of i
        int left[] = new int[heights.length];   

        // right[i] will store index of nearest smaller bar on the right of i
        int right[] = new int[heights.length];   

        // stack is used to keep indices of bars
        Stack<Integer> st = new Stack<>();

        // -------- finding nearest smaller element on the left --------
        for(int i = 0; i < heights.length; i++){

            // remove all bars from stack which are taller or equal to current bar
            // because they cannot be the nearest smaller
            while(!st.isEmpty() && heights[st.peek()] >= heights[i]){
                st.pop();
            }

            // if stack is empty, no smaller element on left
            // so store -1
            // otherwise top of stack is nearest smaller index
            left[i] = st.isEmpty() ? -1 : st.peek();

            // push current index for future elements
            st.push(i);
        }

        // clear stack to reuse it
        st.clear();

        // -------- finding nearest smaller element on the right --------
        for(int i = heights.length - 1; i >= 0; i--){

            // remove all bars from stack which are taller or equal to current bar
            while(!st.isEmpty() && heights[st.peek()] >= heights[i]){
                st.pop();
            }

            // if stack is empty, no smaller element on right
            // so store heights.length
            // otherwise top of stack is nearest smaller index
            right[i] = st.isEmpty() ? heights.length : st.peek();

            // push current index for future elements
            st.push(i);
        }

        int ans = 0;

        // -------- calculate maximum rectangle area --------
        for(int i = 0; i < heights.length; i++){

            // width = (right boundary - left boundary - 1)
            // area = height * width
            ans = Math.max(ans, (right[i] - left[i] - 1) * heights[i]);
        }

        // return maximum area found
        return ans;
    }
}

```