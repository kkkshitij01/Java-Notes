Given a string `s`, partition `s` such that every substring of the partition is a **palindrome**. Return _all possible palindrome partitioning of_ `s`.

**Example 1:**

**Input:** s = "aab"
**Output:** `[["a","a","b"],["aa","b"]]`

**Example 2:**

**Input:** s = "a"
**Output:**` [["a"]]`

**Constraints:**

- `1 <= s.length <= 16`
- `s` contains only lowercase English letters.

---

# Approach

• We have an input string `s`.  
• Create a list `list` to store all valid palindrome partitions.  
• Create a temporary list `ans` to store the current partition.  
• Call `solve(s, list, ans, 0)` to start checking from index `0`.

• In `solve(s, list, ans, idx)`:  
 • If `idx == s.length()` → the whole string is processed → add a **copy** of `ans` to `list` and return.  
 • Run a loop from `i = idx` to `s.length() - 1`:  
  – Check if substring `s[idx...i]` is a palindrome using `isPalindrome(s, idx, i)`.  
  – If it is:  
   • Add this substring (`s.substring(idx, i + 1)`) to `ans`.  
   • Recursively call `solve(s, list, ans, i + 1)` to partition the remaining string.  
   • After returning, **backtrack** by removing the last added substring.

• In `isPalindrome(s, start, end)`:  
 • Check characters from both ends towards the center.  
 • If any mismatch is found, return `false`.  
 • If the loop completes, return `true` (the substring is a palindrome).

• After recursion, `list` contains **all possible palindrome partitions** of the string `s`.

```java
class Solution {

    public List<List<String>> partition(String s) {

        List<List<String>> list = new ArrayList<>();

        List<String> ans = new ArrayList<>();

        solve(s, list, ans, 0);

        return list;

    }

  

    public void solve(String s, List<List<String>> list, List<String> ans, int idx) {

        if( idx == s.length()) {

            list.add(new ArrayList<>(ans));

            return;

        }

        for (int i = idx; i < s.length(); i++) {

            if(isPalindrome(s, idx , i) ){

                ans.add(s.substring(idx , i+1 ));

                solve( s, list , ans, i+1);
                


                ans.remove(ans.size()-1);

            }

        }

    }

  

    public boolean isPalindrome(String s, int start, int end) {

        while (start < end) {

            if (s.charAt(start) != s.charAt(end)) {

                return false;

            }

            start++;

            end--;

        }

        return true;

    }

}

```