A **path** in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence **at most once**. Note that the path does not need to pass through the root.

The **path sum** of a path is the sum of the node's values in the path.

Given the `root` of a binary tree, return _the maximum **path sum** of any **non-empty** path_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/13/exx1.jpg)

**Input:** root =` [1,2,3]`
**Output:** 6
**Explanation:** The optimal path is 2 -> 1 -> 3 with a path sum of 2 + 1 + 3 = 6.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/13/exx2.jpg)

**Input:** root = `[-10,9,20,null,null,15,7]`
**Output:** 42
**Explanation:** The optimal path is 15 -> 20 -> 7 with a path sum of 15 + 20 + 7 = 42.

**Constraints:**

- The number of nodes in the tree is in the range `[1, 3 * 104]`.
- `-1000 <= Node.val <= 1000`

---
---
# Approach :

**Function: `maxPathSum(root)`**  
• Goal → find the highest sum of any path in the tree  
• Create array `maxValue[1]` to track the best answer globally  
• Set `maxValue[0] = -∞` (very small) so real sums can replace it  
• Call helper `maxPathDown(root, maxValue)`  
• Return `maxValue[0]` → final diameter

---

**Function: `maxPathDown(node, maxValue)`**

• Step 1 → Base case  
 - If `node == null` → no value to add → return `0`

• Step 2 → Break into smaller problems  
 - Solve left subtree → get best path from left  
 - Solve right subtree → get best path from right

• Step 3 → Remove negative paths early  
 - If left path sum `< 0` → ignore it → use `0`  
 - If right path sum `< 0` → ignore it → use `0`  
 - Done using → `left = max(0, LCS left)` and `right = max(0, LCS right)`

• Step 4 → Main work (turning point)  
 - Try path that goes through current node connecting both sides  
 - `currentSum = left + right + node.val`  
 - If `currentSum > maxValue[0]` → update `maxValue[0]`

• Step 5 → Return value upward to parent  
 - Parent can only take **one side** + current node  
 - So return → `node.val + max(left, right)`

```java
class Solution {

    public int maxPathSum(TreeNode root) {

        int[] maxValue = new int[1];

        maxValue[0] = Integer.MIN_VALUE;

        maxPathDown(root, maxValue);

        return maxValue[0];
    }

    public int maxPathDown(TreeNode node, int[] maxValue) {

        if (node == null) return 0;
        
        // 1. Recursive calls: If a child returns a negative path, treat it as 0 immediately.

        int left = Math.max(0, maxPathDown(node.left, maxValue));

        int right = Math.max(0, maxPathDown(node.right, maxValue));
        
        // 2. Update the global maximum (The "Turning Point" logic)

        // Since left/right are already sanitized (>=0), we just add them.

        maxValue[0] = Math.max(maxValue[0], left + right + node.val);
        
        // 3. Return the max gain to the parent

        return node.val + Math.max(left, right);

    }

}

```