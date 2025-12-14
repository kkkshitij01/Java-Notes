## Q: What is the use of  z algorithm 
### Ans: Algorithm used for searching the occurrence of a prefix in that String in Optimal way 

---


```java
public class ZFunction {

    /**
     * Helper function to print the contents of an array.
     */
    public static void printArray(int arr[]) {
        for (int i : arr) {
            System.out.print(i + " ");
        }
        System.out.println(); // Add a newline at the end for clean output
    }

    /**
     * Calculates the Z-array for the given string 'str'.
     * Z[i] is the length of the longest substring starting at index 'i' 
     * that is also a prefix of the entire string.
     */
    public static void solve(String str) {
        // Get the length of the input string.
        int n = str.length();
        
        // If the string is empty, there is nothing to do, so we stop.
        if (n == 0)
            return;
            
        // The Z-array will store the length of the longest prefix match 
        // starting at each index 'i'. It's initialized to zeros.
        int z[] = new int[n];
        
        // 'left' and 'right' define the boundaries of the "Z-box".
        // The Z-box [left, right] is the current largest segment that we've found
        // that matches a prefix of the whole string.
        int left = 0, right = 0;
        
        // We loop through the string, starting from the second character (index 1).
        for (int i = 1; i < n; i++) {
            
            // --- Case 1: The current index 'i' is INSIDE the current Z-box (right >= i) ---
            if (right >= i) {
                
                // 'k' is the corresponding index in the prefix (inside the Z-box).
                // We use the already computed Z-value z[k] to quickly estimate z[i].
                int k = i - left;
                
                // 'remSpace' is the remaining length of the Z-box starting from 'i'.
                // This is the maximum we can copy without going outside the current box.
                int remSpace = right - i + 1;
                
                // Sub-Case 1a: The Z-value z[k] is completely inside the remaining space.
                if (z[k] < remSpace) {
                    // We can safely copy z[k] as z[i]. No character comparison needed.
                    z[i] = z[k];
                } else {
                    // Sub-Case 1b: The prefix Z-value z[k] might go BEYOND the Z-box boundary.
                    // We start by taking the remaining space, as we know this part matches.
                    z[i] = remSpace;
                    
                    // We must use slow character comparison to "expand" the match 
                    // past the 'right' boundary until we find a mismatch or reach the end.
                    while (i + z[i] < n && str.charAt(z[i]) == str.charAt(i + z[i])) {
                        z[i]++;
                    }
                }
            } 
            
            // --- Case 2: The current index 'i' is OUTSIDE the current Z-box (right < i) ---
            else {
                
                // Since 'i' is outside, we have to perform a slow, character-by-character 
                // check from scratch to find the Z-value for z[i].
                while (i + z[i] < n && str.charAt(z[i]) == str.charAt(i + z[i])) {
                    z[i]++;
                }
                
                // After finding the new z[i], check if this new match creates a 
                // Z-box that extends further to the right than the previous one.
                if (i + z[i] > right) {
                    // If it does, update the Z-box boundaries.
                    // 'left' is the new starting index 'i'.
                    left = i;
                    // 'right' is the end of the new match. (i + length - 1)
                    right = z[i] + i - 1;
                }
            }
        }
        
        // Print the final calculated Z-array.
        printArray(z);
    }

    public static void main(String[] args) {
        // Example string. The Z-array for this string should be:
        // [0, 1, 0, 0, 1, 0, 1, 0, 0, 5, 1, 0, 1, 0, 0]
        String str = "aabaacaadaabaaba";
        solve(str);
        
        // Example Output: 0 1 0 0 1 0 1 0 0 5 1 0 1 0 0 
    }
}
```