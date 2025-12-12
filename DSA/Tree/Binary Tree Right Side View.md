Given the `root` of a binary tree, imagine yourself standing on the **right side** of it, return _the values of the nodes you can see ordered from top to bottom_.

**Example 1:**

**Input:** root =` [1,2,3,null,5,null,4]`

**Output:** `[1,3,4]`

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/11/24/tmpd5jn43fs-1.png)

**Example 2:**

`**Input:** root = [1,2,3,4,null,null,null,5]`

`**Output:** [1,3,4,5]`

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/11/24/tmpkpe40xeh-1.png)

**Example 3:**

`**Input:** root = [1,null,3]`

`**Output:** [1,3]`

**Example 4:**

`**Input:** root = []`

`**Output:** []`

---
# Approach: 

### **1. Start DFS**

- Call `help(root, 0, ans)`
    
- `ans` will store one value per level — the rightmost node seen.
    

---

## **2. Function: `help(TreeNode root, int level, ArrayList<Integer> ans)`**

### **Base Case**

- If `root == null`, simply return (no node to process).
    

---

### **Step 1 — Check if this is the first node at this level**

- If `ans.size() == level`, it means this level has no value stored yet.
    
- Add the current node’s value:
    

`ans.add(root.val)`

- Since we visit **right child first**, this ensures the first node we see per level is the rightmost one.
    

---

### **Step 2 — Traverse children**

- First visit the right subtree:
    

`help(root.right, level + 1, ans)`

- Then visit the left subtree:
    

`help(root.left, level + 1, ans)`

```java
class Solution {
    public List<Integer> rightSideView(TreeNode root) {

        ArrayList<Integer> ans = new ArrayList<>();

        // Start from root at level 0
        help(root, 0, ans);

        return ans;
    }

    public void help(TreeNode root, int level, ArrayList<Integer> ans) {

        // Base case: if no node, stop
        if (root == null) {
            return;
        }

        // If we are visiting this level for the first time,
        // the current node is the rightmost node seen so far.
        if (ans.size() == level) {
            ans.add(root.val);
        }

        // Visit right subtree first so rightmost nodes get recorded first
        help(root.right, level + 1, ans);

        // Then visit left subtree
        help(root.left, level + 1, ans);
    }
}

```