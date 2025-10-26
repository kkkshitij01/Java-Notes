Consider a rat placed at position (0, 0) in an **n x n** square matrix `**maze[][]**`. The rat's goal is to reach the destination at position (n-1, n-1). The rat can move in four possible directions: **'U'(up)**, **'D'(down)**, **'L' (left)**, **'R' (right)**.

The matrix contains only two possible values:

- `0`: A blocked cell through which the rat cannot travel.
- `1`: A free cell that the rat can pass through.

Your task is to find **all possible paths** the rat can take to reach the destination, starting from (0, 0) and ending at (n-1, n-1), under the condition that the rat cannot **revisit** any cell along the same path. Furthermore, the rat can only move to adjacent cells that are within the bounds of the matrix and not blocked.  
If no path exists, return an **empty list****.**

**Note:** Return the final result vector in **lexicographically smallest order**.

**Examples:**

**Input**: maze`[][] = [[1, 0, 0, 0], [1, 1, 0, 1], [1, 1, 0, 0], [0, 1, 1, 1]]`
**Output:** `["DDRDRR", "DRDDRR"]`
**Explanation**: The rat can reach the destination at (3, 3) from (0, 0) by two paths - DRDDRR and DDRDRR, when printed in sorted order we get DDRDRR DRDDRR.

**Input**: maze`[][] = [[1, 0], [1, 0]]`
**Output:** []
**Explanation**: No path exists as the destination cell (1, 1) is blocked.

`**Input**: maze[][] = [[1, 1, 1], [1, 0, 1], [1, 1, 1]]`
`**Output:** ["DDRR", "RRDD"]`
**Explanation**: The rat has two possible paths to reach the destination: DDRR and RRDD.


---

## BruteForce Approach : 

• Input: `maze[][]` → n x n grid with cells `1` (open) and `0` (blocked).  
• Goal → find all paths from top-left `(0,0)` to bottom-right `(n-1,n-1)` moving only to open cells.

• **Function:** `ratInMaze(int[][] maze)`  
 – Input: `maze[][]` → n x n grid with cells `1` (open) and `0` (blocked).  
 – Goal → find all paths from top-left `(0,0)` to bottom-right `(n-1,n-1)` moving only to open cells.  
 – Create `track[][]` → boolean array to mark visited cells.  
 – Create `ans` → list to store all possible paths as strings.  
 – Call `solve(maze, track, ans, "", 0, 0)` → start backtracking from `(0,0)` with empty path.  
 – Return `ans` → list of all paths.

• **Function:** `solve(int[][] maze, boolean[][] track, List<String> ans, String str, int row, int col)`  
 – Base case: if `(row, col) == (n-1, n-1)` → add current path `str` to `ans`.  
 – Mark current cell as visited → `track[row][col] = true`.  
 – Explore all 4 directions from current cell:  
  • **Down** → `(row+1, col)` → if `isSafe` → append `"D"` and recurse.  
  • **Left** → `(row, col-1)` → if `isSafe` → append `"L"` and recurse.  
  • **Right** → `(row, col+1)` → if `isSafe` → append `"R"` and recurse.  
  • **Up** → `(row-1, col)` → if `isSafe` → append `"U"` and recurse.  
 – After exploring all directions → backtrack → unmark current cell → `track[row][col] = false`.

• **Function:** `isSafe(int[][] maze, boolean[][] track, int row, int col, int n)`  
 – Check if `(row, col)` is within grid bounds.  
 – Check if the cell is open (`maze[row][col] == 1`).  
 – Check if the cell is not visited (`!track[row][col]`).  
 – Return `true` if all conditions are satisfied → safe to move, else `false`.

• Key points:  
 – `ratInMaze()` → initializes structures and starts the backtracking.  
 – `solve()` → recursively explores all paths and backtracks when needed.  
 – `isSafe()` → ensures we only move to valid, unvisited, open cells.  
 – `ans` → contains all possible paths from start to finish represented as strings of `"D"`, `"L"`, `"R"`, `"U"`.

```java
class Solution {

    // Main function to find all paths
    public ArrayList<String> ratInMaze(int[][] maze) {
        int n = maze.length;
        boolean track[][] = new boolean[n][n]; // Tracks visited cells
        ArrayList<String> ans = new ArrayList<>(); // Stores all possible paths

        // Start solving from top-left corner (0,0) with empty path
        solve(maze, track, ans, "", 0, 0);
        return ans;
    }

    // Backtracking function to explore all paths
    public void solve(int[][] maze, boolean track[][], List<String> ans, String str, int row, int col) {
        int n = maze.length;

        // Base case: reached bottom-right corner
        if (row == n - 1 && col == n - 1) {
            ans.add(str); // Add current path to answer
            return;
        }

        // Mark current cell as visited
        track[row][col] = true;

        // Move Down
        if (isSafe(maze, track, row + 1, col, n)) {
            solve(maze, track, ans, str + "D", row + 1, col);
        }
        // Move Left
        if (isSafe(maze, track, row, col - 1, n)) {
            solve(maze, track, ans, str + "L", row, col - 1);
        }
        // Move Right
        if (isSafe(maze, track, row, col + 1, n)) {
            solve(maze, track, ans, str + "R", row, col + 1);
        }
        // Move Up
        if (isSafe(maze, track, row - 1, col, n)) {
            solve(maze, track, ans, str + "U", row - 1, col);
        }

        // Backtrack: unmark current cell
        track[row][col] = false;
    }

    // Check if the cell is within bounds, open (1), and not visited
    public boolean isSafe(int[][] maze, boolean track[][], int row, int col, int n) {
        return (row >= 0 && row < n && col >= 0 && col < n 
                && maze[row][col] == 1 && (!track[row][col]));
    }
}


```



---

# Optimal Solution: 

• **Function:** `ratInMaze()`  
 → Creates the answer list and starts solving from (0,0).  
 → Doesn’t use any extra visited array — saves space.

• **Function:** `solve()`  
 → Uses recursion and backtracking to find all paths.  
 → Marks the current cell as visited by setting it to `-1` instead of using a separate boolean array.  
 → Tries all four directions using arrays (`D, L, R, U`) — this replaces four long if-conditions.  
 → When a valid move is found, it continues exploring recursively.  
 → When the destination `(n-1, n-1)` is reached, adds the path to the answer list.  
 → After exploring, resets the cell back to `1` (backtracking).
 
• **Function:** `isSafe()`  
 → Checks that the next cell is inside the maze, open (`1`), and not visited (`-1`).
 
• **Why it’s optimized:**  
 → Removes the need for a separate `track[][]` array → less memory.  
 → Direction arrays make code shorter and faster.  
 → Fewer parameters → simpler recursive calls.  
 → Same result, cleaner and more efficient code.

```java
class Solution {

    // Main function to find all possible paths for the rat
    public ArrayList<String> ratInMaze(int[][] maze) {
        int n = maze.length;
        ArrayList<String> ans = new ArrayList<>(); // Stores all valid paths
        solve(maze, ans, "", 0, 0); // Start from top-left corner (0,0)
        return ans;
    }

    // Recursive function to explore all possible moves
    public void solve(int[][] maze, List<String> ans, String str, int row, int col) {
        int n = maze.length;

        // Base case: reached destination (bottom-right corner)
        if (row == n - 1 && col == n - 1) {
            ans.add(str); // Add the successful path to the answer list
            return;
        }

        // Directions: Down, Left, Right, Up
        char nextMove[] = {'D', 'L', 'R', 'U'};
        int rowMove[] = {1, 0, 0, -1};
        int colMove[] = {0, -1, +1, 0};

        // Mark current cell as visited (to avoid loops)
        maze[row][col] = -1;

        // Try all four directions
        for (int i = 0; i < 4; i++) {
            int newRow = row + rowMove[i];
            int newCol = col + colMove[i];

            // If the next move is safe, continue the path
            if (isSafe(maze, newRow, newCol, n)) {
                solve(maze, ans, str + nextMove[i], newRow, newCol);
            }
        }

        // Backtrack: mark current cell as open again
        maze[row][col] = 1;
    }

    // Check if the cell is valid (inside maze, not blocked, and not visited)
    public boolean isSafe(int[][] maze, int row, int col, int n) {
        return (row >= 0 && row < n && col >= 0 && col < n && maze[row][col] == 1);
    }
}

```