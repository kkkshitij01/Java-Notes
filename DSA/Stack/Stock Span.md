
The stock span problem is a financial problem where we have a series of daily price quotes for a stock and we need to calculate the span of stock price for all days.  
You are given an array **arr[]** representing daily stock prices, the stock span for the **i-th** day is the number of consecutive days up to day i (including day i itself) for which the price of the stock is **less than or equal** to the price on day **i**. Return the span of stock prices for each day in the given sequence.  

**Examples:**

**Input**:` arr[] = [100, 80, 90, 120]`
**Output**: `[1, 1, 2, 4]`
**Explanation**: Traversing the given input span 100 is greater than equal to 100 and there are no more days behind it so the span is 1, 80 is greater than equal to 80 and smaller than 100 so the span is 1, 90 is greater than equal to 90 and 80 so the span is 2, 120 is greater than 90, 80 and 100 so the span is 4. So the output will be `[1, 1, 2, 4].`

**Input**: arr`[] = [10, 4, 5, 90, 120, 80]`
**Output**: `[1, 1, 2, 4, 5, 1]`
**Explanation**: Traversing the given input span 10 is greater than equal to 10 and there are no more days behind it so the span is 1, 4 is greater than equal to 4 and smaller than 10 so the span is 1, 5 is greater than equal to 4 and 5 and smaller than 10 so the span is 2, and so on. Hence the output will be` [1, 1, 2, 4, 5, 1].`

---
# **Brute Force**

### **1. Initialize**

- `len` = number of days
    
- `ans` = output list storing span for each day
    

---

### **2. Loop through each day `i`**

For every price at index `i`:

- Start with `count = 1` (today counts as one day automatically)
    
- Now compare backward with previous days.
    

---

### **3. Compare with all previous days (inner loop)**

Loop `j` from `i-1` down to `0`:

- **If current price ≥ previous price** (`arr[i] >= arr[j]`):
    
    - Increase span: `count++`
        
- **Else:**
    
    - Break the loop because the streak of smaller prices ends here.
        

This ensures span includes only consecutive smaller/equal prices.

---

### **4. Add the span for this day**

Store `count` in `ans`.

---

### **5. After processing all days**

Return the list `ans`.

```java
class Solution {
    public ArrayList<Integer> calculateSpan(int[] arr) {
        int len = arr.length;
        ArrayList<Integer> ans = new ArrayList<>();

        // For each day, compute its stock span
        for (int i = 0; i < len; i++) {

            int count = 1;  // span always includes itself

            // Check previous days until you find a higher price
            for (int j = i - 1; j >= 0; j--) {

                // If current price is >= previous day's price,
                // increase span count
                if (arr[i] >= arr[j]) {
                    count++;
                } 
                // As soon as we find a larger price, stop
                else {
                    break;
                }
            }
            ans.add(count);
        }
        return ans;
    }
}

```

---
---
---

# **Optimal** 
### **1. Use a stack to find Next Greater to the Left (NGTL)**

We store **indices** of previous days in a stack, keeping prices in **strict decreasing order**.

This is exactly the stack behavior used in Next Greater to the Left.

---

### **2. For each index `i`:**

### **A. Remove all prices ≤ current price**

`while(!st.isEmpty() && arr[st.peek()] <= arr[i])     st.pop();`

This is the same step used in Next Greater to the Left (NGTL):

- We remove elements that are "not greater"
    
- We keep searching for the **nearest greater element on the left**
    

---

### **B. Compute the span using NGTL result**

#### **Case 1 — Stack becomes empty**

Means NGTL does **not exist** for current index.

→ There was **no greater price to the left**

Therefore:

`currentSpan = i + 1;`

All previous days had price ≤ today.

---

#### **Case 2 — NGTL exists**

Stack top gives the index of the:

> **Nearest previous greater element (Next Greater to the Left)**

So:

`currentSpan = i - st.peek();`

This tells how many consecutive days we can include before hitting a greater price.

---

### **C. Push the current index**

`st.push(i);`

This index may become the Next Greater to the Left (NGTL) for future days.

---

# **3. Add span to answer list**

We collect all spans in order.

```java
class Solution {
    public ArrayList<Integer> calculateSpan(int[] arr) {
        
        ArrayList<Integer> spanList = new ArrayList<>(); 
        
        Stack<Integer> st = new Stack<>();
        
        for(int i = 0; i < arr.length; i++){
            
            // Remove all previous days whose prices are <= today's price.
            // These days are part of the span and cannot block it.
            while(!st.isEmpty() && arr[st.peek()] <= arr[i]){
                st.pop();
            }
            
            int currentSpan;
            
            // If stack is empty, it means no previous day had a greater price,
            // so the span includes ALL days from index 0 to i.
            if(st.isEmpty()){
                currentSpan = i + 1;
            } 
            else {
                // Otherwise, st.peek() is the nearest previous day with a greater price.
                // Span is the number of days between that index and i.
                currentSpan = i - st.peek();
            }
            
            // Add span result for current day
            spanList.add(currentSpan);
            
            // Push current day index into the stack 
            // so it can be used for future span calculations.
            st.push(i);
        }
        
        return spanList;
    }
}

```
