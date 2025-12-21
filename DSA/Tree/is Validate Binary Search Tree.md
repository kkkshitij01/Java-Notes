Given the `root` of a binary tree, _determine if it is a valid binary search tree (BST)_.

A **valid BST** is defined as follows:

- The left subtree of a node contains only nodes with keys **strictly less than** the node's key.
- The right subtree of a node contains only nodes with keys **strictly greater than** the node's key.
- Both the left and right subtrees must also be binary search trees.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/12/01/tree1.jpg)

**Input:** root =` [2,1,3]`
**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/12/01/tree2.jpg)

**Input:** root = `[5,1,4,null,null,3,6]`
**Output:** false
**Explanation:** The root node's value is 5 but its right child's value is 4.

---
---
# Optimal : 
## **Overview**

- Start with the widest possible range.
    
- Recursively validate left and right subtrees.
    
- If any node violates its allowed range, the tree is not a BST.
    

---

## **Step-by-Step Explanation**

### **1. Initial Call**

- Call the helper function with:
    
    `min = Long.MIN_VALUE max = Long.MAX_VALUE`
    
- This allows the root to take any valid integer value.
    

---

### **2. Helper Function**

`help(TreeNode root, long min, long max)`

This function checks whether the subtree rooted at `root` is a valid BST **within the given range**.

---

### **3. Base Case**

- If `root == null`
    
    - This subtree is valid.
        
    - Return `true`.
        

---

### **4. Range Validation**

- If:
    
    `root.val <= min OR root.val >= max`
    
    - The BST rule is violated.
        
    - Return `false`.
        

Strict inequality is important because duplicates are **not allowed** in a BST.

---

### **5. Recursive Calls**

- For the **left subtree**:
    
    - Allowed range becomes `(min, root.val)`
        
- For the **right subtree**:
    
    - Allowed range becomes `(root.val, max)`
        

`help(root.left, min, root.val) help(root.right, root.val, max)`

Both must return `true` for the current subtree to be valid.
```java

class Solution {

    public boolean isValidBST(TreeNode root) {

        // Start recursive validation with the widest possible range
        // Any node value must lie between Long.MIN_VALUE and Long.MAX_VALUE
        return help(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }

    public boolean help(TreeNode root, long min, long max){

        // If we reach a null node, it means this subtree is valid
        if(root == null) return true;

        // If current node violates the allowed range,
        // then BST property is broken
        if(root.val >= max || root.val <= min) return false;

        // Recursively check:
        // - Left subtree must have values strictly less than root.val
        // - Right subtree must have values strictly greater than root.val
        return help(root.left, min, root.val)
            && help(root.right, root.val, max);
    }
}

```

---
---
# FAILED : Level order traversal Check
This code only checks **Immediate Family** (Parent vs. Child). It forgets **Ancestry** (Grandparents).


```
Example: 
	  5
     / \
    1   7
       / \
      4   8
```


```java
class Solution {

    public boolean isValidBST(TreeNode root) {

        // Queue is used to perform level-order traversal (BFS)
        Queue<TreeNode> qu = new LinkedList<>();

        // Start traversal from the root
        qu.offer(root);

        while(!qu.isEmpty()){

            // Take the next node from the queue
            TreeNode currentNode = qu.poll();
            int val = currentNode.val;

            // Check left child
            if(currentNode.left != null){

                // If left child value is greater than or equal to current node,
                // this violates the immediate BST property
                if(val <= currentNode.left.val){
                    return false;
                }

                // Add left child to queue for further checking
                qu.offer(currentNode.left);
            }

            // Check right child
            if(currentNode.right != null){

                // If right child value is less than or equal to current node,
                // this violates the immediate BST property
                if(val >= currentNode.right.val){
                    return false;
                }

                // Add right child to queue for further checking
                qu.offer(currentNode.right);
            }
        }

        // If no local violations were found, return true
        return true;
    }
}

```

