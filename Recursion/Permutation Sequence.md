The set `[1, 2, 3, ..., n]` contains a total of `n!` unique permutations.

By listing and labeling all of the permutations in order, we get the following sequence for `n = 3`:

1. `"123"`
2. `"132"`
3. `"213"`
4. `"231"`
5. `"312"`
6. `"321"`

Given `n` and `k`, return the `kth` permutation sequence.

**Example 1:**

**Input:** n = 3, k = 3
**Output:** "213"

**Example 2:**

**Input:** n = 4, k = 9
**Output:** "2314"

**Example 3:**

**Input:** n = 3, k = 1
**Output:** "123"


--- 
# Approach

• We are given two numbers:  
 → `n` → numbers from 1 to n.  
 → `k` → the kth permutation to find.

• Create a list `list` to store numbers from 1 to n.  
• Create a `StringBuilder str` to build the final permutation.  
• Create a variable `fact = 1` to store factorial values.  
• Convert `k` to **0-based index** by doing `k = k - 1`.

• Multiply `fact` by i in a loop from 1 to n-1 → this calculates `(n-1)!`.  
 – We stop at n-1 because the first digit fixes the block of `(n-1)!` permutations.  
 – Add numbers 1 to n-1 to the list in the same loop.  
• Add the last number n to the list → now list = `[1, 2, …, n]`.

• While the list is not empty:  
 – Find index = `k / fact` → tells which number to pick next.  
 – Append that number from the list to `str`.  
 – Remove that number from the list.  
 – Update `k = k % fact` → position within the new block.  
 – Update `fact = fact / list.size()` → remaining numbers have fewer permutations.

• Stop when the list is empty → all digits are chosen.  
• Return `str.toString()` → this is the **kth permutation**.

```java
class Solution {

  

    public String getPermutation(int n, int k) {

        // Create a list to store numbers from 1 to n

        ArrayList<Integer> list = new ArrayList<>();

        // StringBuilder to build the final permutation

        StringBuilder str = new StringBuilder("");

  
        int fact = 1;  // To store factorial value (number of permutations for each position)

        k = k - 1;     // Convert k to 0-based index (because index starts from 0)

  

        // Calculate (n-1)! and fill the list with numbers 1 to n

        for(int i = 1; i < n; i++){

            fact *= i;         // factorial for (n-1)!

            list.add(i);       // add numbers to list

        }

        list.add(n);           // add the last number

        // Build the permutation string

        while(true){

            // Pick the element at index (k / fact)

            str.append(list.get(k / fact)); 

            // Remove that element from the list (since it’s already used)

            list.remove(k / fact);

            // If all elements are used, stop the loop

            if(list.size() == 0){

                break;

            }

            // Update k to find next index for smaller list
            k = k % fact;

            // Update factorial for the next position
            fact = fact / list.size();
        }

        // Return the final permutation as a string

        return str.toString();
    }
}
```