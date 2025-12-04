Given two **version strings**, `version1` and `version2`, compare them. A version string consists of **revisions** separated by dots `'.'`. The **value of the revision** is its **integer conversion** ignoring leading zeros.

To compare version strings, compare their revision values in **left-to-right order**. If one of the version strings has fewer revisions, treat the missing revision values as `0`.

Return the following:

- If `version1 < version2`, return -1.
- If `version1 > version2`, return 1.
- Otherwise, return 0.

**Example 1:**

**Input:** version1 = "1.2", version2 = "1.10"

**Output:** -1

**Explanation:**

version1's second revision is "2" and version2's second revision is "10": 2 < 10, so version1 < version2.

**Example 2:**

**Input:** version1 = "1.01", version2 = "1.001"

**Output:** 0

**Explanation:**

Ignoring leading zeroes, both "01" and "001" represent the same integer "1".

**Example 3:**

**Input:** version1 = "1.0", version2 = "1.0.0.0"

**Output:** 0

**Explanation:**

version1 has less revisions, which means every missing revision are treated as "0".


---
# Approach : 

**• Step 1 — Split both version strings**

- Use `split("\\.")` to break versions into numeric parts.
    
- Example: `"1.02.3"` → `["1", "02", "3"]`.
    

---

**• Step 2 — Determine the number of iterations**

- Compare up to the **maximum length** of the two arrays.
    
- Missing parts are treated as `0`.
    
- Example: `"1.0"` vs `"1"` → treat `"1"` as `"1.0"`.
    

---

**• Step 3 — Loop through each version segment**

- For index `i`:
    
    - Get number from `version1`:
        
        `numb1 = i < first.length ? Integer.parseInt(first[i]) : 0`
        
    - Get number from `version2`:
        
        `numb2 = i < second.length ? Integer.parseInt(second[i]) : 0`
        

---

**• Step 4 — Compare numeric parts**

- If `numb1 > numb2` → return `1`.
    
- If `numb1 < numb2` → return `-1`.
    
- If equal → continue checking next segment.
    

---

**• Step 5 — If all segments match**

- All version parts are equal → return `0`.
    

---

## **Summary**

- Versions are compared part by part (split by `"."`).
    
- Missing version parts are considered `0`.
    
- Numeric comparison ensures `"1.10"` is correctly treated as greater than `"1.2"`.



```java
class Solution {
    public int compareVersion(String version1, String version2) {
        
        // Split both version strings into parts using '.'
        String[] first = version1.split("\\.");
        String[] second = version2.split("\\.");
        
        // We compare up to the longer length (missing parts count as 0)
        int max = Math.max(first.length, second.length);
        
        for (int i = 0; i < max; i++) {
            
            // Get current version number part, or 0 if this version ran out of parts
            int numb1 = i < first.length ? Integer.parseInt(first[i]) : 0;
            int numb2 = i < second.length ? Integer.parseInt(second[i]) : 0;
            
            // Compare the two numbers
            if (numb1 > numb2) {
                return 1;   // version1 is greater
            }
            if (numb1 < numb2) {
                return -1;  // version2 is greater
            }
        }
        
        // All compared parts are equal
        return 0;
    }
}

```