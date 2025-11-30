Given two strings, one is a text string **txt** and the other is a pattern string **pat**. The task is to print the **indexes** of **all the occurrences** of the pattern string in the text string. Use **0-based** indexing while returning the indices.  
**Note:** Return an empty list in case of no occurrences of pattern.  

**Examples :**

**Input:** txt = "abcab", pat = "ab"
**Output:** `[0, 3]`
**Explanation**: The string "ab" occurs twice in txt, one starts at index 0 and the other at index 3. 

**Input**: txt = "abesdu", pat = "edu"
**Output:** []
**Explanation**: There's no substring "edu" present in txt.  

**Input**: txt = "aabaacaadaabaaba", pat = "aaba"
**Output:** `[0, 9, 12]`
**Explanation**:  
![](https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/703119/Web/Other/blobid0_1731391225.png)  


---

# BruteForce :
### **Function:** `search(String pat, String txt)`

• **Goal:**  
 - Find all starting indices in `txt` where the pattern `pat` occurs.  
 - Return all matching positions in an `ArrayList`.

• **Step 1:**  
 - Create an empty list `ans` to store all valid starting indices.

• **Step 2:**  
 - Loop through each possible starting position `i` in `txt`.  
 - For every `i`, try to match the pattern starting from that index.

• **Step 3:**  
 - Set `patIdx = 0` to track characters in `pat`.  
 - Run an inner loop with index `j` starting from `i` through `txt`.

• **Step 4:**  
 - Compare characters:  
  • If `txt.charAt(j) != pat.charAt(patIdx)`, break (pattern can't match here).  
  • Else, increase `patIdx` (pattern matched so far).

• **Step 5:**  
 - If `patIdx == pat.length()`, it means the entire pattern matched starting from index `i`.  
  • Add `i` to the answer list.  
  • Break the inner loop since this starting point is processed.

• **Step 6:**  
 - After checking all positions, return `ans` containing all match indices.
```java
class Solution {
    ArrayList<Integer> search(String pat, String txt) {

        ArrayList<Integer> ans = new ArrayList<>(); // store starting indices where pattern is found

        // Loop over every possible starting position in the text
        for (int i = 0; i < txt.length(); i++) {

            int patIdx = 0;  // pointer for pattern characters

            // Try to match pattern starting at index i in text
            for (int j = i; j < txt.length(); j++) {

                // If characters do not match, stop checking from this position
                if (txt.charAt(j) != pat.charAt(patIdx)) {
                    break;
                }

                patIdx++; // move to next character in the pattern

                // If entire pattern matched, record the starting index
                if (patIdx == pat.length()) {
                    ans.add(i);
                    break; // stop inner loop for this starting point
                }
            }
        }

        return ans; // return list of indices where pattern occurs
    }
}

```