Given the `root` of a binary tree, return _the length of the **diameter** of the tree_.

The **diameter** of a binary tree is the **length** of the longest path between any two nodes in a tree. This path may or may not pass through the `root`.

The **length** of a path between two nodes is represented by the number of edges between them.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/06/diamtree.jpg)

**Input:** root = `[1,2,3,4,5]`

**Output:** 3

**Explanation:** 3 is the length of the path `[4,2,1,3]` or` [5,2,1,3].`

**Example 2:**

**Input:** root = `[1,2]`

**Output:** 1

---
---
# Optimal : 
### **Part 1: Base Condition**

• If node is `null` → height is `0`.  
• We return `0` because empty tree has no edges.

---

### **Part 2: Work Done**

• Recursively find:

- `left height` from left child
    
- `right height` from right child  
    • Calculate diameter passing through current node:
    

`left + right`

(This gives number of edges if path goes through this node)

• Update global answer:

`ans[0] = max(ans[0], left + right)`

We update because we want the **largest diameter found so far**.

---

### **Part 3: Smaller Problem Breakdown**

For each node:  
→ Find left subtree height  
→ Find right subtree height  
→ Use them to update diameter  
→ Return height to parent

`return 1 + max(left, right)`

We add `1` because the current node increases the height by one level.

```java
class Solution {

    public int diameterOfBinaryTree(TreeNode root) {

        // ans array stores the final diameter result, we use array so recursion can update it
        int ans[] = new int[1];

        // start DFS and height calculation
        getHeight(root, ans);

        // return the diameter value stored in ans[0]
        return ans[0];
    }

    public int getHeight(TreeNode root, int ans[]) {

        // if node is null, height is 0 and diameter cannot increase
        if (root == null) {
            return 0;
        }

        // find height of left side
        int left = getHeight(root.left, ans);

        // find height of right side
        int right = getHeight(root.right, ans);

        // update diameter: left height + right height gives longest path through this node
        ans[0] = Math.max(ans[0], left + right);

        // return height of this node = 1 + max of left or right
        return 1 + Math.max(left, right);
    }
}

```