
Given a binary tree, determine if it is **height-balanced**.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/06/balance_1.jpg)

**Input:** root = `[3,9,20,null,null,15,7]`
**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/06/balance_2.jpg)

**Input:** root = `[1,2,2,3,3,null,null,4,4]`
**Output:** false

**Example 3:**

**Input:** root = `[]`
**Output:** true

---
---
# Brute Force: 
```java
class Solution {

    // This function finds height (depth) of the tree
    public int height(TreeNode root){

        // If node is null, height is 0
        if(root == null) return 0;

        // Get height of left side
        int left = height(root.left);

        // Get height of right side
        int right = height(root.right);

        // Height of this node = 1 + bigger height of left or right
        return 1 + Math.max(left, right);
    }

    // This function checks if tree is balanced or not
    public boolean isBalanced(TreeNode root) {

        // If tree is empty, it is balanced
        if(root == null) return true;

        // Find height of left subtree
        int l = height(root.left);

        // Find height of right subtree
        int r = height(root.right);

        // If difference of heights is more than 1, tree is not balanced
        if(Math.abs(l - r) > 1) return false;

        // Recursively check left part is balanced or not
        boolean left = isBalanced(root.left);

        // Recursively check right part is balanced or not
        boolean right = isBalanced(root.right);

        // If either side is not balanced, tree is not balanced
        if(!left || !right) return false;

        // If all checks pass, tree is balanced
        return true;
    }
}

```

---
---
# Optimal :
### **Part 1: Base Condition**

- If `root == null`
    
    - Return `0`
        
    - Because an empty tree has height `0` and is balanced
        

---

### **Part 2: Work Done at Current Node**

At each node:

- Find height of left subtree
    
- Find height of right subtree
    

Then:

- If `left == -1` OR `right == -1`
    
    - Return `-1`
        
    - Because one subtree is already unbalanced
        
- If `|left - right| > 1`
    
    - Return `-1`
        
    - Because current node breaks balance rule
        
- Otherwise:
    
    - Return
        
        `1 + max(left, right)`
        
    - Add `1` for the current node
        

---

### **Part 3: Breaking Into Smaller Problems**

For each node:

- Solve balance + height for left subtree
    
- Solve balance + height for right subtree
    
- Combine results to decide balance at current node
    
- Return result upward to parent
    

---

## **Final Check**

In `isBalanced()`:

- Call `height(root)`
    
- If result is `-1`
    
    - Tree is **not balanced**
        
- Else
    
    - Tree **is balanced**

---
## **Time and Space Complexity**

### **Time Complexity**

`O(n)`

Each node is processed once.

### **Space Complexity**

`O(h)`

Where `h` is the height of the tree (recursion stack).
```java
class Solution {

    public boolean isBalanced(TreeNode root) {

        // Get the height of the tree
        // If tree is not balanced, height() will return -1
        int ans = height(root);

        // If ans is not -1, tree is balanced
        return ans != -1;
    }

    public int height(TreeNode root) {

        // If node is null, height is 0
        if (root == null) return 0;

        // Get left subtree height
        int left = height(root.left);

        // Get right subtree height
        int right = height(root.right);

        // If left or right returned -1, it means subtree is not balanced
        // So return -1 to parent
        if (left == -1 || right == -1) return -1;

        // If height difference is more than 1, tree is not balanced
        // So return -1
        if (Math.abs(left - right) > 1) return -1;

        // If balanced, return height = 1 + max(left, right)
        return 1 + Math.max(left, right);
    }
}

```