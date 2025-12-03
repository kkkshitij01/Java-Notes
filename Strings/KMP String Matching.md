Also Knows as Longest Proper Prefix Suffix (LPS) && 1392. Longest Happy Prefix on leetcode

---
A string is called a **happy prefix** if is a **non-empty** prefix which is also a suffix (excluding itself).

Given a string `s`, return _the **longest happy prefix** of_ `s`. Return an empty string `""` if no such prefix exists.

**Example 1:**

**Input:** s = "level"
**Output:** "l"
**Explanation:** s contains 4 prefix excluding itself ("l", "le", "lev", "leve"), and suffix ("l", "el", "vel", "evel"). The largest prefix which is also suffix is given by "l".

**Example 2:**

**Input:** s = "ababab"
**Output:** "abab"
**Explanation:** "abab" is the largest prefix which is also suffix. They can overlap in the original string.


---


# BruteForce : [[1392. Longest Happy Prefix]]

# Approach: 

**• Step 1 — Setup:**

- Let `n = s.length()`.
    
- Create an integer array `pi[n]` to store prefix-function values.
    
- Initialize `pre = 0`, which represents the current matched prefix length.
    

---

**• Step 2 — Build the prefix array:**

- Loop `suf` from `1` to `n - 1`:
    
    - `suf` represents the current character in the suffix we are trying to match.
        

---

**• Step 3 — Handle mismatches:**

- While `pre > 0` and characters do not match:
    
    - Move `pre` back to `pi[pre - 1]`.
        
- This step ensures we reuse the longest previous prefix match.
    

---

**• Step 4 — Handle matches:**

- If `s.charAt(pre) == s.charAt(suf)`:
    
    - Increase `pre++` because the matching prefix extended.
        

---

**• Step 5 — Save prefix length:**

- Assign `pi[suf] = pre`.
    
- This value represents the length of the longest prefix that is also a suffix ending at index `suf`.
    

---

### **• Step 6 — Extract the answer**

- The value `pi[n - 1]` gives the length of the **longest prefix that is also a suffix**.
    
- Return that prefix using:
    
    `s.substring(0, pi[n - 1])`

```java
class Solution {
    public String longestPrefix(String s) {

        int n = s.length();
        int pi[] = new int[n]; // pi[i] = length of longest prefix that is also suffix for substring ending at i
        int pre = 0;           // length of current matching prefix

        // Build the prefix array
        for (int suf = 1; suf < n; suf++) {

            // If characters don't match, move prefix pointer back
            while (pre > 0 && s.charAt(pre) != s.charAt(suf)) {
                pre = pi[pre - 1];  // fallback to previous longest prefix
            }

            // If characters match, extend the current prefix
            if (s.charAt(pre) == s.charAt(suf)) {
                pre++;
            }

            // Store the prefix length for this index
            pi[suf] = pre;
        }

        // The last value of pi[] gives the length of the longest prefix
        // which is also a suffix of the entire string
        int longestPrefixLength = pi[n - 1];

        // Return that prefix
        return s.substring(0, longestPrefixLength);
    }
}

```