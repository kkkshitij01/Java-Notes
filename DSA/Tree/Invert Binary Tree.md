Given the `root` of a binary tree, invert the tree, and return _its root_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/14/invert1-tree.jpg)

**Input:** root =` [4,2,7,1,3,6,9]`
**Output:** `[4,7,2,9,6,3,1]`

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/14/invert2-tree.jpg)

**Input:** root = `[2,1,3]`
**Output:** `[2,3,1]`

**Example 3:**

**Input:** root =` []`
**Output:** `[]
`
**Constraints:**

- The number of nodes in the tree is in the range `[0, 100]`.
- `-100 <= Node.val <= 100` 

---
---

```java
class Solution {
    public TreeNode invertTree(TreeNode root) {

        // start recursion from root to invert the tree
        solve(root);

        // return the root of the inverted tree
        return root;
    }

    public void solve(TreeNode root){

        // if current node is null, nothing to invert
        if(root == null)return;

        // store left child temporarily
        TreeNode node = root.left;

        // swap left and right children
        root.left = root.right;
        root.right = node;

        // recursively invert left subtree
        solve(root.left);

        // recursively invert right subtree
        solve(root.right);

    }
}
```
