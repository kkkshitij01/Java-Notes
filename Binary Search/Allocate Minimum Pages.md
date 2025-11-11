Given an array **arr[]** of integers, where each element **arr`[i]`** represents the number of pages in the i-th book. You also have an integer **k** representing the number of students. The task is to allocate books to each student such that:

- Each student receives atleast one book.
- Each student is assigned a contiguous sequence of books.
- No book is assigned to more than one student.

The objective is to **minimize the maximum number of pages** assigned to any student. In other words, out of all possible allocations, find the arrangement where the student who receives the most pages still has the **smallest possible maximum**.

**Note:** If it is not possible to allocate books to all students, return **-1**.

**Examples:**

**Input:**` arr[] = [12, 34, 67, 90], k = 2`
**Output:** 113
**Explanation:** Allocation can be done in following ways:  
=>` [12] and [34, 67, 90] `Maximum Pages = 191  
=> `[12, 34] and [67, 90]` Maximum Pages = 157  
=> `[12, 34, 67]` and `[90]` Maximum Pages = 113.  
The third combination has the minimum pages assigned to a student which is 113.

**Input:** arr`[] = [15, 17, 20],` k = 5  
**Output:** -1  
**Explanation:** Since there are more students than total books, it's impossible to allocate a book to each student.

**Constraints:**  
1 ≤ arr.size() ≤ 106  
`1 ≤ arr[i], k ≤ 103`


---

# BruteForce Approach
###  **Function:** `findPages(int[] arr, int k)`
• **Step 1:**  
 → Initialize two boundaries for possible answers:  
  – `start` = maximum pages in a single book (no student can get less than this).  
  – `end` = total pages of all books (if only one student gets all books).

• **Step 2:**  
 → Loop through all possible maximum page limits (`limit`) from `start` to `end`.  
 → For each limit, call `check(arr, limit)` to find how many students are needed if no student gets more than `limit` pages.  
 → If the number of students `ans <= k`, it means books can be distributed within that limit → return `limit`.

###  **Function:** `check(int[] arr, int max)`

• **Purpose:**  
 → Determines how many students are required if each student can get at most `max` pages.

• **Logic:**  
 → Initialize `student = 1`, `pages = 0`.  
 → Loop through all books:  
  – If adding `arr[i]` to current student’s total doesn’t exceed `max`, add it (`pages += arr[i]`).  
  – Otherwise, assign the next student (`student++`) and reset `pages = arr[i]`.  
 → Return the total number of students required.
```java
class Solution {

    // Main function to find the minimum number of pages each student can get
    public int findPages(int[] arr, int k) {
        int start = Integer.MIN_VALUE; // minimum possible pages (max single book pages)
        int end = 0;                   // maximum possible pages (sum of all books)

        // Step 1: Determine the search space
        for (int i = 0; i < arr.length; i++) {
            // The lower limit = the largest single book
            if (arr[i] > start) {
                start = arr[i];
            }
            // The upper limit = sum of all pages
            end += arr[i];
        }

        // Step 2: Try all possible limits from start to end
        // (Brute force approach)
        for (int limit = start; limit <= end; limit++) {
            int studentsNeeded = check(arr, limit);

            // If we can allocate books within this limit using <= k students
            // it means this limit works, so return it
            if (studentsNeeded <= k) {
                return limit;
            }
        }

        return -1; // Should never reach here for valid inputs
    }

    // Helper function to calculate how many students are needed
    // if no student can read more than 'max' pages
    public int check(int[] arr, int max) {
        int student = 1; // start with 1 student
        int pages = 0;   // pages assigned to the current student

        for (int i = 0; i < arr.length; i++) {
            // If adding this book doesn't exceed the max limit
            if (pages + arr[i] <= max) {
                pages += arr[i];
            } else {
                // Assign books to next student
                student++;
                pages = arr[i]; // start counting for new student
            }
        }

        return student; // total students needed for this max limit
    }
}

```

---

# Optimal Approach

### ⚙️ **Function:** `findPages(int[] arr, int k)`

• **Goal:**  
 → Allocate books to `k` students such that the **maximum pages assigned to a student is minimized**.  
 → Each student must get **consecutive books**.

• **Step 1:**  
 → Handle edge case → if `k > arr.length`, return `-1` (not enough books for each student).

• **Step 2:**  
 → Initialize binary search range:  
  – `start` = maximum value in `arr` (minimum possible answer).  
  – `end` = sum of all pages (maximum possible answer if one student takes all books).

• **Step 3:**  
 → Perform **binary search** while `start <= end`:  
  – Calculate `mid = (start + end) / 2` → the current maximum pages limit.  
  – Use `check(arr, mid)` to find how many students are needed if no one reads more than `mid` pages.  
  – If students required `<= k`, we can reduce the limit → `end = mid - 1`.  
  – Otherwise, increase the limit → `start = mid + 1`.

• **Step 4:**  
 → When the loop ends, `start` will be at the **minimum possible maximum pages** — return `start`.

### ⚙️ **Function:** `check(int[] arr, int max)`

• **Purpose:**  
 → Count how many students are needed if each can read at most `max` pages.

• **Logic:**  
 → Initialize `student = 1`, `pages = 0`.  
 → Traverse each book:  
  – If `pages + arr[i] <= max`, assign the book to the same student.  
  – Otherwise, assign a new student (`student++`) and reset `pages = arr[i]`.  
 → Return the total number of students used.
```java
class Solution {

    public int findPages(int[] arr, int k) {
        // If there are fewer books than students, it's impossible to allocate
        if (k > arr.length) return -1;

        int start = Integer.MIN_VALUE; // minimum possible limit = largest single book
        int end = 0;                   // maximum possible limit = sum of all books

        // Step 1: find search space (min and max number of pages)
        for (int i = 0; i < arr.length; i++) {
            start = Math.max(start, arr[i]); // biggest single book
            end += arr[i];                   // total pages (max possible)
        }

        int ans = 0;

        // Step 2: Binary search to find the smallest possible max pages
        while (start <= end) {
            int mid = (start + end) / 2; // mid = guess for max pages per student

            // Check how many students are needed if no one reads more than 'mid' pages
            ans = check(arr, mid);

            // If the number of students needed is less than or equal to k,
            // it means we can try to lower the max pages further
            if (ans <= k) {
                end = mid - 1;
            } 
            // Otherwise, we need more students, so increase allowed pages
            else {
                start = mid + 1;
            }
        }

        // 'start' will point to the smallest maximum pages each student can get
        return start;
    }

    // Helper function to count how many students are required for a given max page limit
    public int check(int[] arr, int max) {
        int student = 1; // start with first student
        int pages = 0;   // track pages assigned to current student

        for (int i = 0; i < arr.length; i++) {
            // If adding current book doesn't exceed the max limit
            if (pages + arr[i] <= max) {
                pages += arr[i];
            } 
            // Otherwise, allocate to a new student
            else {
                student++;
                pages = arr[i];
            }
        }

        return student; // total students needed for this limit
    }
}


```