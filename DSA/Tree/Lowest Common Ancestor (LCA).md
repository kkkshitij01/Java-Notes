Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor): “The lowest common ancestor is defined between two nodes `p` and `q` as the lowest node in `T` that has both `p` and `q` as descendants (where we allow **a node to be a descendant of itself**).”

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

**Input:** root = `[3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1`
**Output:** 3
**Explanation:** The LCA of nodes 5 and 1 is 3.

**Example 2:**

![](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

**Input:** root = `[3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4`
**Output:** 5
**Explanation:** The LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.

**Example 3:**

**Input:** root = `[1,2], p = 1, q = 2`
**Output:** 1

---
---

# Optimal :
**Function: `lowestCommonAncestor(root, p, q)`**

### **Part 1 — Base Conditions**

- If `root` is `null` → stop → return `null`
    
- If `root == p` OR `root == q` → we found one target → return `root`
    

### **Part 2 — Break into Smaller Questions**

- Check left side → `left = LCA(root.left, p, q)`
    
- Check right side → `right = LCA(root.right, p, q)`
    

### **Part 3 — Work / Decision**

- If `left` and `right` both are not `null` → `p` is on one side and `q` on other → current `root` is the answer → return `root`
    
- If only `left` is not `null` → answer exists in left part → return `left`
    
- If only `right` is not `null` → answer exists in right part → return `right`
    
- Else → neither found → return `null`
### **Complexity**

|Time|Space|
|---|---|
|O(n)|O(h)|

- `n` = nodes visited once
    
- `h` = recursion stack height

```java
class Solution {

    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {

        // If node is null, or it matches p or q, return it because it can be the answer
        if(root == null || root == p || root == q){
            return root;
        }

        // Search LCA in the left subtree
        TreeNode left = lowestCommonAncestor(root.left, p, q);

        // Search LCA in the right subtree
        TreeNode right = lowestCommonAncestor(root.right, p, q);

        // If we found p in one side and q in other side, current node is the LCA
        if(left != null && right != null){
            return root;
        }

        // If both nodes are found in left side, return left result
        if(left != null){
            return left;
        }

        // If both nodes are found in right side, return right result
        if(right != null){
            return right;
        }

        // If nothing is found, return null
        return null;
    }
}

```