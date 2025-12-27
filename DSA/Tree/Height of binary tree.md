Given the `root` of a binary tree, return _its maximum depth_.

A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/26/tmp-tree.jpg)

**Input:** root = `[3,9,20,null,null,15,7]`

**Output:** 3

**Example 2:**

**Input:** root =` [1,null,2]`

**Output:** 2

---
---
# Optimal : 
## **Part 1: Base Condition**

• If the current node is `null`  
→ There is no tree.  
→ Depth is `0`.

This stops the recursion.

---

## **Part 2: Work Being Done**

• The current node itself contributes **1 level** to the depth.

So we add:

`1 +`

---

## **Part 3: Break the Problem into Smaller Problems**

• Find the depth of the **left subtree**.  
• Find the depth of the **right subtree**.

These are smaller versions of the same problem.

`help(root.left) help(root.right)`

• Take the **maximum** of both because the deeper side decides the height.

---

## **Final Formula**

`1 + max(leftDepth, rightDepth)`
```java
class Solution {

    public int maxDepth(TreeNode root) {
    
        return help(root);
    }

    public int help(TreeNode root ){

        // If the current node is null,
        // it means there is no tree, so depth is 0
        if(root == null){
            return 0;
        }

        // We count 1 for the current node
        // Then we find depth of left subtree and right subtree
        // We take the bigger one because depth means the longest path
        return 1 + Math.max(help(root.left), help(root.right));
    }
}
```
