Given an array of positive integers **arr[]** and a value **sum**, determine if there is a subset of **arr[]** with sum equal to given **sum**. 

**Examples:**

**Input**: arr`[] = [3, 34, 4, 12, 5, 2`], sum = 9  
**Output:** true 
**Explanation**: Here there exists a subset with target sum = 9, 4+3+2 = 9.

**Input**: ar`r[] = [3, 34, 4, 12, 5, 2`], sum = 30
**Output:** false
**Explanation**: There is no subset with target sum 30.

**Input**: ar`r[] = [1, 2, 3]`, sum = 6
**Output:** true  
**Explanation**: The entire array can be taken as a subset, giving 1 + 2 + 3 = 6.

**Constraints:**  
1 <= arr.size() <= 200  
1<= arr`[i] `<= 200  
1<= sum <= 104

---

# DP : (Memo)

```java
class Solution {

    static Boolean dp[][]; // dp[idx][cap] stores result of whether sum "cap" can be formed starting from index idx

    static Boolean isSubsetSum(int arr[], int sum) {

        // Create DP table with rows = number of elements + 1
        // and columns = possible sums from 0 to target sum
        dp = new Boolean[arr.length + 1][sum + 1];

        // Start recursion with full capacity = sum and index = 0
        return solve(arr, sum, sum, 0);
    }

    static Boolean solve(int arr[], int sum, int cap, int idx) {

        // If remaining sum becomes 0, subset exists
        if(cap == 0){
            return true;
        }

        // If we checked all elements but still sum not formed
        if(idx == arr.length){
            return dp[idx][cap] = false;
        }

        // If this state already computed, reuse it
        if(dp[idx][cap] != null){
            return dp[idx][cap];
        }

        // If current element can fit in remaining sum
        if(cap >= arr[idx]){

            // Two choices:
            // 1. Include current element
            // 2. Skip current element
            return dp[idx][cap] =
                (solve(arr, sum, cap - arr[idx], idx + 1) ||
                 solve(arr, sum, cap, idx + 1));

        } else {

            // If element is bigger than remaining sum, skip it
            return dp[idx][cap] = solve(arr, sum, cap, idx + 1);
        }
    }
}
```

## **Step 1: DP Initialization**

In `isSubsetSum()`:

- Create DP table of size:
    

(arr.length + 1) × (sum + 1)

- Fill with `null` → means state not calculated yet.
    

Then start recursion:

solve(arr, sum, sum, 0)

Meaning:

- Start from index `0`
    
- Remaining sum needed = `sum`
    

---

# **Recursive Function: `solve()`**

---

## **Step 2: Base Conditions**

### Case 1

If remaining sum becomes `0`:

cap == 0

Return `true` because subset is found.

---

### Case 2

If we checked all elements but still sum not formed:

idx == arr.length

Return `false`.

---

## **Step 3: DP Check**

Before computing:

if(dp[idx][cap] != null)  
    return dp[idx][cap]

Reuse stored answer.

---

## **Step 4: Two Choices**

If current element fits:

cap >= arr[idx]

We try two options:

### Include element

solve(arr, cap - arr[idx], idx + 1)

### Skip element

solve(arr, cap, idx + 1)

Return **true if either works**.

include OR skip

---

## **Step 5: If Element Doesn't Fit**

If:

arr[idx] > cap

We cannot include it.

Only option:

skip element

---

# **Why Memoization Helps**

Without DP:

- Same `(idx, cap)` gets solved many times.
    
- Time complexity becomes exponential.
    

With DP:

- Each state `(idx, cap)` is solved only once.
    

---

# **Time & Space Complexity**

|Type|Complexity|
|---|---|
|**Time Complexity**|`O(n × sum)`|
|**Space Complexity**|`O(n × sum)`|
|**Recursion Stack**|`O(n)`|