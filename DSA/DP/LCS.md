Given two strings `text1` and `text2`, return _the length of their longest **common subsequence**._ If there is no **common subsequence**, return `0`.

A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

- For example, `"ace"` is a subsequence of `"abcde"`.

A **common subsequence** of two strings is a subsequence that is common to both strings.

**Example 1:**

**Input:** text1 = "abcde", text2 = "ace" 
**Output:** 3  
**Explanation:** The longest common subsequence is "ace" and its length is 3.

**Example 2:**

**Input:** text1 = "abc", text2 = "abc"
**Output:** 3
**Explanation:** The longest common subsequence is "abc" and its length is 3.

**Example 3:**

**Input:** text1 = "abc", text2 = "def"
**Output:** 0
**Explanation:** There is no such common subsequence, so the result is 0.



---
# Recursive Approach : [[LONGEST COMMON SUBSEQUENCE]]

# Optimal Approach: 


## **Function: `longestCommonSubsequence(String text1, String text2)`**

- Create a 2D memo array `dp[][]` to store already computed LCS values.
    
- Call the helper function:  
    `h(text1, text2, text1.length(), text2.length(), dp)`.
    

---

## **Function: `h(String text1, String text2, int m, int n, Integer dp[][])`**

### **Base Case**

- If `m == 0` or `n == 0`, return `0`.
    

---

### **Memo Check**

- If `dp[m-1][n-1]` is already filled, return that value.
    

---

### **Case 1: Last characters match**

- If `text1.charAt(m-1) == text2.charAt(n-1)`  
    Then the LCS includes this character.
    
- Store and return:  
    `dp[m-1][n-1] = 1 + h(text1, text2, m-1, n-1, dp)`.
    

---

### **Case 2: Last characters do not match**

- Compute both possibilities:
    
    - LCS removing last char of text1 → `h(m-1, n)`
        
    - LCS removing last char of text2 → `h(m, n-1)`
        
- Take the maximum of the two.
    
- Store and return:  
    `dp[m-1][n-1] = max(...)`.
    

---

```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        
        // Memo table: dp[i][j] will store LCS of text1[0..i] and text2[0..j]
        Integer dp[][] = new Integer[text1.length()][text2.length()];
        
        // Start recursion using full lengths
        return h(text1, text2, text1.length(), text2.length(), dp);
    }

    public int h(String text1, String text2, int m, int n, Integer dp[][]) {

        // If any string is empty → LCS = 0
        if (m == 0 || n == 0) {
            return 0;
        }

        // If already computed, return memoized answer
        if (dp[m-1][n-1] != null) return dp[m-1][n-1];

        // If characters match → include 1 + LCS of previous parts
        if (text1.charAt(m-1) == text2.charAt(n-1)) {
            return dp[m-1][n-1] =
                1 + h(text1, text2, m-1, n-1, dp);
        }

        // If characters don't match → take max of:
        // - removing last char from text1
        // - removing last char from text2
        return dp[m-1][n-1] = Math.max(
            h(text1, text2, m-1, n, dp),
            h(text1, text2, m, n-1, dp)
        );
    }
}



```