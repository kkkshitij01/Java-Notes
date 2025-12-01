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
### STEP 1


```
if (pat.length() > txt.length()) return ans;
```

- **What:** Physical impossibility check.
    
- **Why:** A longer string cannot be a substring of a shorter string.
    
- **Optimization:** $O(1)$ immediate rejection.
    
---
### STEP 2

```
for (int i = 0; i <= txt.length() - pat.length(); i++)
```

- **What:** The "Driver" Loop. Moves the starting position $i$.
    
- **Limit:** `txt.length() - pat.length()`
    
    - **Why:** If `txt` has 10 chars and `pat` has 3, we stop at index 7. Indices 8 and 9 don't have enough room for 3 characters.
        
- **Visualization:**
    
    Plaintext
    
    ```
    Text:  [ A  B  C  D  E ]
    Pat:   [ C  D ]
             ^ Last valid start index for 'i'
    ```
    

---

### PART 2

#### THE MATCHING PHASE (Helper Function)

### STEP 3

```
for (int i = idx; i < txt.length() && j < pat.length(); i++, j++) {
    if (txt.charAt(i) != pat.charAt(j)) return false;
}
```

- **What:** Character-by-Character Verification.
    
- **Logic:**
    
    - Pointer `i`: Scans `txt` starting from `idx`.
        
    - Pointer `j`: Scans `pat` starting from `0`.
        
- **Mismatch Rule:** If `txt[i] \neq pat[j]`, the overlay fails immediately.
    
- **Time Complexity:** Worst case $O(M)$ where $M$ is pattern length.
    

---

### STEP 4

```
return j == pat.length();
```

- **What:** Success Validation.
    
- **Why:** The loop can end for two reasons:
    
    1. **Mismatch:** We broke out early (Failure).
        
    2. **End of Pattern:** `j` incremented until it equaled length (Success).
        
- **Math:** If $j = |pat|$, we successfully matched all characters.
    

### Final Complexity Analysis

- **Time:** $O((N - M + 1) \times M)$ $\approx O(N \times M)$.
    
    - Where $N$ is text length, $M$ is pattern length.
        
- **Space:** $O(1)$ (No extra data structures used).
```java
class Solution {
    /**
     * This function searches for all starting positions (indices) 
     * where the 'pat' (pattern) string appears inside the 'txt' (text) string.
     * It uses a simple, 'brute-force' approach.
     */
    ArrayList<Integer> search(String pat, String txt) {
        
        // This list will store the starting index (position) in the text 
        // where we find a match for the pattern.
        ArrayList<Integer> ans = new ArrayList<>();
        
        // --- Step 1: Handle Edge Case ---
        
        // If the pattern is longer than the text, it can't possibly fit inside.
        // We return the empty list immediately.
        if (pat.length() > txt.length()) {
            return ans;
        }
        
        // --- Step 2: Slide the Window (Outer Loop) ---
        
        // This loop tries every possible starting position 'i' in the 'txt'.
        // The loop runs from index 0 up to the last possible starting point.
        // The last valid start index is (txt.length() - pat.length()).
        for (int i = 0; i <= txt.length() - pat.length(); i++) {
            
            // At the current starting position 'i', we call a helper function 
            // to check if the pattern 'pat' matches the text 'txt' starting there.
            if (maching(pat, txt, i)) {
                // If the helper function says it's a match, we add the starting index 'i' 
                // to our list of answers.
                ans.add(i);
            }
        }
        
        // After checking all possibilities, we return the list of starting indices.
        return ans;
    }
    
    /**
     * This helper function checks for an exact match between 'pat' 
     * and the substring of 'txt' that starts at 'idx'.
     */
    public boolean maching(String pat, String txt, int idx) {
        
        // 'j' is a pointer (index) that keeps track of the current position in the PATTERN.
        int j = 0; // pattern ptr
        
        // --- Step 3: Character-by-Character Comparison (Inner Loop) ---
        
        // The loop runs as long as:
        // 1. We haven't gone past the end of the text (i < txt.length()).
        // 2. We haven't finished checking all characters in the pattern (j < pat.length()).
        for (int i = idx; i < txt.length() && j < pat.length(); i++, j++) {
            
            // Compare the character in the text (at index 'i') with the 
            // corresponding character in the pattern (at index 'j').
            if (txt.charAt(i) != pat.charAt(j)) {
                // If they don't match, this specific starting position 'idx' is NOT a match.
                // We stop immediately and return 'false'.
                return false;
            }
        }
        
        // --- Step 4: Final Success Check ---
        
        // If the loop finished without returning 'false', we must check WHY it finished.
        // If 'j' equals the pattern's full length, it means we compared ALL characters 
        // in the pattern successfully. This is a full match!
        return j == pat.length();
    }
}

```