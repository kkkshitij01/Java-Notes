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

# Approach : TLE (ERROR)

### Time complexity : O   2^(m+n)
### **• Step 1 — Base Case**

- If either string length becomes `0`, no subsequence is possible:
    

`if (first == 0 || second == 0) return 0;`

---

### **• Step 2 — Characters Match**

- If the last characters match:
    

`text1[first - 1] == text2[second - 1]`

- Then this character **must be part of LCS**, so:
    

`return 1 + helper(text1, text2, first - 1, second - 1);`

- Move both pointers left.
    

---

### **• Step 3 — Characters Do Not Match**

- If last characters differ, we have two choices:
    

**Option 1:**  
Exclude last char of `text2` → recurse with `second - 1`

**Option 2:**  
Exclude last char of `text1` → recurse with `first - 1`

- Take the best (maximum) of the two:
    

`return Math.max(     helper(text1, text2, first, second - 1),     helper(text1, text2, first - 1, second) );`


```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        return helper(text1, text2, text1.length(), text2.length());
    }
    public int helper(String text1, String text2, int first, int second) {

        // If either string is empty, LCS length is 0
        if (first == 0 || second == 0) {
            return 0;
        }

        // If last characters match, add 1 and move both pointers
        if (text1.charAt(first - 1) == text2.charAt(second - 1)) {
            return 1 + helper(text1, text2, first - 1, second - 1);
        }

        // Otherwise, take the maximum LCS by removing one char at a time
        return Math.max(
            helper(text1, text2, first, second - 1),
            helper(text1, text2, first - 1, second)
        );
    }
}

```



# Optimal DP Approach: [[LCS]]