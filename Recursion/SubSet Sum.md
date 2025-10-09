**Problem Statement:** Given an array print all the sum of the subset generated from it, in the increasing order.

**Examples:**

**Example 1:**

**Input:** N = 3, arr[] = {5,2,1}

**Output:** 0,1,2,3,5,6,7,8

**Explanation:** We have to find all the subset’s sum and print them.in this case the generated subsets are` [ [], [1], [2], [2,1], [5], [5,1], [5,2]. [5,2,1],so the sums we get will be  0,1,2,3,5,6,7,8`

**Input:** N=3,arr[]= {3,1,2}

**Output:** 0,1,2,3,3,4,5,6

**Explanation:** We have to find all the subset’s sum and print them.in this case the generated subsets are `[ [], [1], [2], [2,1], [3], [3,1], [3,2]. [3,2,1],so the sums we get will be  0,1,2,3,3,4,5,6`



## SAME AS [[PRINT ALL SUBSET]]
#   Approach : 
• We have an input array **`arr[]`.**  
• Create an **`ArrayList<Integer> list`** to store subset sums.  
• Initialize **`idx = 0` and `sum = 0`.**  
• Call the recursive **function `solve(arr, list, sum, idx)**` to start processing.

• In `solve(arr, list, sum, idx)`:  
 • If `idx == arr.length` → all elements are processed → add `sum` to `list`.  
 • Otherwise, for the current element at `arr[idx]`:  
  – Include it in the sum → call `solve(arr, list, sum + arr[idx], idx + 1)`.  
  – Exclude it from the sum → call `solve(arr, list, sum, idx + 1)`.

• After all recursive calls, `list` contains sums of all possible subsets of the array.


```java
// SUBSET SUM 
class Solution {
    public ArrayList<Integer> subsetSums(int[] arr) {
        ArrayList<Integer> list = new ArrayList<>();
        int idx = 0; 
        int sum = 0; 
        solve(arr, list, sum, idx );
        Collections.sort(list, (a,b)-> Integer.compare(a.size(), b.size()))
        return list;
    }
    public void solve( int arr[] , List<Integer> list , int sum, int idx){
        if( arr.length == idx){
            list.add(sum);
        }
            solve(arr, list, sum +arr[idx], idx+1);
            solve(arr, list, sum , idx+1);
    }
}
```