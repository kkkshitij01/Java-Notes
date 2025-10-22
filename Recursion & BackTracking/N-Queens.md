The **n-queens** puzzle is the problem of placing `n` queens on an `n x n` chessboard such that no two queens attack each other.

Given an integer `n`, return _all distinct solutions to the **n-queens puzzle**_. You may return the answer in **any order**.

Each solution contains a distinct board configuration of the n-queens' placement, where `'Q'` and `'.'` both indicate a queen and an empty space, respectively.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/13/queens.jpg)

**Input:** n = 4
**Output:** `[[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]`
**Explanation:** There exist two distinct solutions to the 4-queens puzzle as shown above

**Example 2:**

**Input:** n = 1
**Output:** `[["Q"]]`

---
# BruteForce Approach: 
### Step 1: Initialize the board
- Create a 2D character array `board[n][n]`.
- Fill every cell with `'.'` to represent an empty space.
- Prepare a list `ans` to store all valid board arrangements.

### Step 2: Start backtracking (`solve` function)
- Start from **row = 0** (first row).
- Try placing a queen in each column of the current row one by one.
- 
###  Step 3: Check if placing a queen is safe (`isSafe` function)
When checking position `(row, col)` → make sure no queen attacks it:
1. **Vertically above:**  
     – Check all rows above the current one in the same column.  
     – If a queen (`'Q'`) is found → unsafe.
2. **Upper-left diagonal:**  
     – Move diagonally up-left → check if a queen exists.
3. **Upper-right diagonal:**  
     – Move diagonally up-right → check if a queen exists.
4. If none of these have a queen → the position is **safe**.

###  Step 4: Place and backtrack

- If it’s safe, place a queen: `board[row][col] = 'Q'`. 
- Move to the next row: `solve(n, board, ans, row + 1)`.
- After returning, remove the queen (backtrack): `board[row][col] = '.'`.  
     → This helps explore **all possible placements**.
###  Step 5: Base condition
- If `row == n` → all queens are placed safely.
- Convert the `board` into a list of strings and add it to `ans`.

```java
class Solution {
    public List<List<String>> solveNQueens(int n) {
        List<List<String>> ans = new ArrayList<>(); // Stores all possible board arrangements
        char board[][] = new char[n][n];            // Create an empty chessboard
        
        // Fill the board initially with '.'
        for (int i = 0; i < n; i++) {
            Arrays.fill(board[i], '.');
        }
        
        // Start solving from the first row
        solve(n, board, ans, 0);
        return ans;
    }

    // Backtracking function to place queens row by row
    public void solve(int n, char board[][], List<List<String>> ans, int row) {
        // Base case: if all queens are placed
        if (row == n) {
            List<String> temp = new ArrayList<>();
            // Convert board rows into strings for result
            for (int i = 0; i < n; i++) {
                temp.add(new String(board[i]));
            }
            ans.add(temp); // Add valid arrangement to the answer list
            return;
        }

        // Try placing a queen in each column of the current row
        for (int i = 0; i < n; i++) {
            // Check if it's safe to place a queen here
            if (isSafe(n, board, row, i)) {
                board[row][i] = 'Q'; // Place the queen
                solve(n, board, ans, row + 1); // Move to the next row
                board[row][i] = '.'; // Backtrack: remove queen
            }
        }
    }

    // Function to check if placing a queen is safe
    public boolean isSafe(int n, char board[][], int row, int col) {
        // Check vertically above
        for (int i = row; i >= 0; i--) {
            if (board[i][col] == 'Q') {
                return false;
            }
        }

        // Check upper-left diagonal
        for (int i = row, j = col; i >= 0 && j >= 0; i--, j--) {
            if (board[i][j] == 'Q') {
                return false;
            }
        }

        // Check upper-right diagonal
        for (int i = row, j = col; i >= 0 && j < n; i--, j++) {
            if (board[i][j] == 'Q') {
                return false;
            }
        }

        // Safe to place queen
        return true;
    }
}

```


---

# Optimal Approach:
###  Step 1: Initialize board and helper arrays

- Create `board[n][n]` → store current board arrangement.
- Fill every cell with `'.'` → empty space.
- Create three boolean arrays for **quick conflict checks**:  
     – `vertical[n]` → tracks columns already occupied.  
     – `leftDi[2*n - 1]` → tracks left diagonals (`\`) already occupied.  
     – `rightDi[2*n - 1]` → tracks right diagonals (`/`) already occupied.
- Prepare `ans` → list to store all valid board arrangements.

###  Step 2: Start backtracking (`solve` function)

- Begin from **row = 0**.
    
- For each column `col` in current row:  
     – Calculate diagonal indices:  
      • `RD = row + col` → index for right diagonal `/`  
      • `LD = row - col + (n-1)` → index for left diagonal `\`  
     – Skip this column if `vertical[col]` **or** `left[LD]` **or** `right[RD]` is `true`.

###  Step 3: Place queen and recurse
- If safe, place queen → `board[row][col] = 'Q'`.
- Mark column and diagonals as occupied → `vertical[col] = left[LD] = right[RD] = true`.
- Recur for next row → `solve(n, board, ans, row + 1, vertical, left, right)`.

###  Step 4: Backtrack
- After returning from recursion:  
     – Remove queen → `board[row][col] = '.'`.  
     – Mark column and diagonals as free → `vertical[col] = left[LD] = right[RD] = false`.
### Step 5: Base condition

- If `row == n` → all queens are placed safely.
- Convert the current `board` into a list of strings and add it to `ans`.


```java
class Solution {
    public List<List<String>> solveNQueens(int n) {
        List<List<String>> ans = new ArrayList<>(); // Stores all valid board arrangements
        char board[][] = new char[n][n];            // Chessboard

        // Arrays to keep track of columns and diagonals that already have queens
        boolean vertical[] = new boolean[n];        // Tracks columns
        boolean leftDi[] = new boolean[2 * n - 1];  // Tracks left diagonals (\)
        boolean rightDi[] = new boolean[2 * n - 1]; // Tracks right diagonals (/)

        // Initialize board with '.'
        for (int i = 0; i < n; i++) {
            Arrays.fill(board[i], '.');
        }

        // Start backtracking from row 0
        solve(n, board, ans, 0, vertical, leftDi, rightDi);
        return ans;
    }

    // Backtracking function to place queens efficiently using arrays for quick checks
    public void solve(int n, char board[][], List<List<String>> ans, int row,
                      boolean vertical[], boolean left[], boolean right[]) {
        // Base case: all queens are placed
        if (row == n) {
            List<String> temp = new ArrayList<>();
            for (int i = 0; i < n; i++) {
                temp.add(new String(board[i])); // Convert row to string
            }
            ans.add(temp); // Add valid board to answer
            return;
        }

        // Try placing a queen in each column
        for (int col = 0; col < n; col++) {
            int RD = row + col;           // Index for right diagonal (/)
            int LD = (row - col) + (n - 1); // Index for left diagonal (\)

            // Skip if this column or diagonal already has a queen
            if (vertical[col] || left[LD] || right[RD]) continue;

            // Place queen
            board[row][col] = 'Q';
            vertical[col] = left[LD] = right[RD] = true;

            // Recur for next row
            solve(n, board, ans, row + 1, vertical, left, right);

            // Backtrack: remove queen and mark column/diagonals as free
            board[row][col] = '.';
            vertical[col] = left[LD] = right[RD] = false;
        }
    }
}

```