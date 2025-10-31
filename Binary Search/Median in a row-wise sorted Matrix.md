Given a **row-wise sorted** matrix **mat[][]** of size n*m, where the number of rows and columns is always **odd**. Return the **median** of the matrix.

**Examples:**

**Input**: mat[][] =         `[[1, 3, 5], `
                `[2, 6, 9],   `
                `[3, 6, 9]]`
**Output:** 5
**Explanation**: Sorting matrix elements gives us` [1, 2, 3, 3, 5, 6, 6, 9, 9].` Hence, 5 is median.

**Input:** mat`[][]` = ` [[2, 4, 9],`
                 `[3, 6, 7],`
                `[4, 7, 10]]`
**Output:** 6
**Explanation**: Sorting matrix elements gives us `[2, 3, 4, 4, 6, 7, 7, 9, 10].` Hence, 6 is median.

**Input:** mat = `[[3], [4], [8]]`
**Output:** 4
**Explanation**: Sorting matrix elements gives us` [3, 4, 8]. Hence, 4 is median.

---

# BruteForce :

• **Goal:** Find the median of a row-wise sorted matrix.  
• Let `n = mat.length` (rows) and `m = mat[0].length` (columns).  
• Create a 1D array `arr` of size `n * m` to store all elements.  
• Use a pointer `ptr` to keep track of the next index in `arr`.  
• Loop through every element of the 2D matrix → copy each element into `arr`.  
• After flattening, sort the array using `Arrays.sort(arr)`.  
• The median is the element at index `arr.length / 2`.  
• Return that element as the result.  
• **Time Complexity:** O(n_m log(n_m)) → due to sorting.  
• **Space Complexity:** O(n*m) → extra space for flattened array.

```java
class Solution {
    public int median(int[][] mat) {
        int n = mat.length;          // Number of rows
        int m = mat[0].length;       // Number of columns

        int arr[] = new int[n * m];  // Array to store all elements from the matrix
        int ptr = 0;

        // Flatten the 2D matrix into a 1D array
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                arr[ptr++] = mat[i][j];
            }
        }

        // Sort the array to find the median
        Arrays.sort(arr);

        // Return the middle element (median)
        return arr[arr.length / 2];
    }
}

```


---

# Optimal Approach: 

• Let `row = mat.length` and `col = mat[0].length`.

• **Step 1:** Find the smallest and largest elements in the matrix.  
 → Set `low` = minimum of all first elements of each row (since rows are sorted).  
 → Set `high` = maximum of all last elements of each row.

• **Step 2:** Apply **binary search** between `low` and `high`.  
 → Find `mid = (low + high) / 2`.  
 → Count how many numbers in the matrix are ≤ `mid` using `lessThanEqualTo()`.  
 → If this count is less than half of total elements → median lies in the **right half** → `low = mid + 1`.  
 → Otherwise → median lies in the **left half or at mid** → `high = mid`.

• **Step 3:** When `low == high`, this value is the **median** of the matrix.

### ⚙️ **lessThanEqualTo(int[] arr, int numb)**

• **Goal:** Count how many elements in a sorted row are ≤ `numb`.  
• Use **binary search** to move `low` until it points to the first element > `numb`.  
• Return `low` → represents the count of elements ≤ `numb`.

```java
class Solution {
    public int median(int[][] mat) {
        int row = mat.length; 
        int col = mat[0].length;

        // Step 1: Find the global minimum and maximum in the matrix
        int low = Integer.MAX_VALUE; 
        int high = Integer.MIN_VALUE; 
        for (int i = 0; i < row; i++) {
            low = Math.min(mat[i][0], low);         // smallest element in each row
            high = Math.max(mat[i][col - 1], high); // largest element in each row
        }

        // Step 2: Binary search between low and high
        while (low < high) {
            int mid = (low + high) / 2;
            int count = 0; 

            // Count how many numbers are <= mid in all rows
            for (int i = 0; i < row; i++) {
                count += lessThanEqualTo(mat[i], mid);
            }

            // If count is less than half of total elements, move right
            if (count < (row * col) / 2 + 1) {
                low = mid + 1;
            } 
            // Otherwise, move left (median might be smaller or equal to mid)
            else {
                high = mid;
            }
        }

        // Step 3: When low == high, that's the median value
        return low;
    }

    // Helper function: counts how many elements in the row are ≤ numb
    public int lessThanEqualTo(int arr[], int numb) {
        int low = 0; 
        int high = arr.length;

        // Binary search inside a single sorted row
        while (low < high) {
            int mid = (low + high) / 2;

            if (arr[mid] <= numb) {
                low = mid + 1;   // move right to include elements ≤ numb
            } else {
                high = mid;      // move left if arr[mid] > numb
            }
        }

        // 'low' ends up being the count of elements ≤ numb
        return low;
    }
}


```