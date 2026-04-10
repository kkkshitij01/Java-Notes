
Given the `root` of a binary tree, flatten the tree into a "linked list":

- The "linked list" should use the same `TreeNode` class where the `right` child pointer points to the next node in the list and the `left` child pointer is always `null`.
- The "linked list" should be in the same order as a pre-order traversal of the binary tree.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/14/flaten.jpg)

**Input:** root = `[1,2,5,3,4,null,6]`
**Output:**` [1,null,2,null,3,null,4,null,5,null,6]`

**Example 2:**

**Input:** root = `[]`
**Output:** `[]`

**Example 3:**

**Input:** root = `[0]`
**Output:** `[0]`

**Constraints:**

- The number of nodes in the tree is in the range `[0, 2000]`.
- `-100 <= Node.val <= 100`

---
---


# Optimal : 
### Key Observation

Preorder traversal order is:

root → left → right

But this solution processes in **reverse preorder**:

right → left → root

Why?

> So we can **build the linked list from back to front** using a `prev` pointer.

---

## Approach

### **1. Use Reverse Preorder Traversal**

Instead of normal preorder, we do:

flatten(right)  
flatten(left)

This ensures:

- We process nodes in reverse order
- So we can **attach current node to already processed list**

---

### **2. Maintain a `prev` Pointer**

prev → points to previously processed node

This represents:

> The **next node** in the final linked list

---

### **3. Modify Links**

At each node:

root.right = prev  
root.left = null  
prev = root

So:

- Current node connects to the already built list
- Then becomes the new `prev`

---

```java
class Solution {

    // prev stores the previously processed node
    // it helps in linking current node to the next node in flattened list
    public TreeNode prev = null;

    public void flatten(TreeNode root) {

        // if current node is null, nothing to process
        if(root == null) return ; 

        // first flatten the right subtree
        flatten(root.right);

        // then flatten the left subtree
        flatten(root.left);
        
        // make current node point to previously processed node
        root.right = prev;

        // set left child to null as per linked list structure
        root.left = null;

        // update prev to current node
        prev = root;
    }
}

```