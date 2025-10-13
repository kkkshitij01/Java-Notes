Given a string `s`, return _the number of **palindromic substrings** in it_.

A string is a **palindrome** when it reads the same backward as forward.

A **substring** is a contiguous sequence of characters within the string.

**Example 1:**

**Input:** s = "abc"
**Output:** 3
**Explanation:** Three palindromic strings: "a", "b", "c".

**Example 2:**

**Input:** s = "aaa"
**Output:** 6
**Explanation:** Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".


***

# Approach

• We have an input string `s`.  
• The function `countSubstrings(s)` calls `solve(s, 0, 0)` to start counting from index `0` with count `0`.

• In `solve(s, idx, res)`:  
 • If `idx == s.length()` → the whole string is processed → return the total count `res`.  
 • Otherwise, run a loop from `i = idx` to `s.length() - 1`:  
  – For each substring `s[idx...i]`, check if it’s a palindrome using `isPalindrome(s, idx, i)`.  
  – If true → increase `res` by 1.  
 • After checking all substrings starting at `idx`,  
  call `solve(s, idx + 1, res)` to move to the next starting index.

• In `isPalindrome(s, start, end)`:  
 • Compare characters from both ends towards the center.  
 • If any mismatch occurs → return `false`.  
 • If all match → return `true`.

• After all recursive calls, the function returns the total number of **palindromic substrings** in the string `s`.

```java
class Solution {

    public int countSubstrings(String s) {

        return solve( s,  0, 0);

    }

    public int solve(String s, int idx , int count){

        if(idx == s.length()){

            return count;

        }   

        for(int i = idx ; i< s.length(); i++){ // check for all substring starting from idx ;
            if(isPalindrome(s, idx , i)){ //if palindrome increase count
                count++;  
            }

        }

        return solve( s, idx+1 , count); // 

    }

    public boolean isPalindrome(String s , int start , int end){

        while(start< end){

            if(s.charAt(start)!= s.charAt(end)){

                return false;

            }

            start++;

            end--;

        }

        return true;

    }

}
```