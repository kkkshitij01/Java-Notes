Given the `root` of a binary tree, _check whether it is a mirror of itself_ (i.e., symmetric around its center).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/symtree1.jpg)

**Input:** root =` [1,2,2,3,4,4,3]`
**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/02/19/symtree2.jpg)

**Input:** root = `[1,2,2,null,3,null,3]`
**Output:** false

**Constraints:**

- The number of nodes in the tree is in the range `[1, 1000]`.
- `-100 <= Node.val <= 100`

---
---


# Optimal

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {

        // check if left and right side of tree are mirror of each other
        return solve(root.left , root.right);

    }

    public boolean solve(TreeNode left , TreeNode right ) {

        // if both nodes are null, they match
        if(left == null && right == null ) return true;

        // if one is null or values are different, not symmetric
        if (left == null || right == null || left.val != right.val|| left.val != right.val) return false;
     
        // check mirror:
        // left's left with right's right
        // left's right with right's left
        return solve(left.left , right.right)&&   solve(left.right, right.left);
    }
}
```

## **1. Start from Root**

Compare:

root.left  vs  root.right

---

## **2. Use Recursive Mirror Check**

Function:

solve(left, right)

This checks whether two trees are mirror images.

---

## **3. Mirror Matching Rule**

At each step:

left.left  ↔ right.right  
left.right ↔ right.left