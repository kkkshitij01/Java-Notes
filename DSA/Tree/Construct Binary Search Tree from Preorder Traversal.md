Given an array of integers preorder, which represents the **preorder traversal** of a BST (i.e., **binary search tree**), construct the tree and return _its root_.

It is **guaranteed** that there is always possible to find a binary search tree with the given requirements for the given test cases.

A **binary search tree** is a binary tree where for every node, any descendant of `Node.left` has a value **strictly less than** `Node.val`, and any descendant of `Node.right` has a value **strictly greater than** `Node.val`.

A **preorder traversal** of a binary tree displays the value of the node first, then traverses `Node.left`, then traverses `Node.right`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/03/06/1266.png)

**Input:** preorder =` [8,5,1,7,10,12]`
**Output:**` [8,5,10,1,7,null,12]`

**Example 2:**

**Input:** preorder = `[1,3]`
**Output:** `[1,null,3]`

---
---
# Brute Force 
### **Key Observation**

- The **first element** of preorder is always the **root**.
    
- Every next value must be inserted following **BST rules**:
    
    - Smaller values → left subtree
        
    - Larger values → right subtree
        

---

### **Approach Overview**

- Create the root from the first element.
    
- Insert remaining elements **one by one** into the BST.
    
- Use **iterative traversal** to find the correct position for insertion.
    

---

### **Step-by-Step Explanation**

#### **1. Create Root**

- Initialize the root using `preorder[0]`.
    
- This becomes the starting point of the BST.
    

---

#### **2. Insert Remaining Elements**

- Loop through the preorder array from index `1`.
    
- For each value, call `helper(root, value)` to insert it.
    

---

#### **3. Helper Function Logic**

`helper(TreeNode root, int val)`

- Use two pointers:
    
    - `root` → to traverse the tree
        
    - `prev` → to remember the parent node
        

---

#### **4. Traverse the BST**

- While `root` is not `null`:
    
    - Store current node in `prev`
        
    - If `val < root.val` → move left
        
    - Else → move right
        

This continues until a `null` position is found.

---

#### **5. Insert the Node**

- Now `prev` is the correct parent:
    
    - If `val > prev.val` → insert as `prev.right`
        
    - Else → insert as `prev.left`
        

---

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {

    public TreeNode bstFromPreorder(int[] preorder) {

        // First element of preorder traversal is always the root of the BST
        TreeNode root = new TreeNode(preorder[0]);

        // Insert remaining elements one by one into the BST
        for(int i = 1; i < preorder.length; i++){
            helper(root, preorder[i]);
        }

        return root;
    }

    public void helper(TreeNode root, int val){

        // 'prev' will store the parent where the new node should be attached
        TreeNode prev = root;

        // Traverse the BST to find the correct position for val
        while(root != null){
            prev = root;

            // If val is smaller, go to left subtree
            if(root.val > val){
                root = root.left;
            }
            // If val is greater, go to right subtree
            else{
                root = root.right;
            }
        }

        // Insert the new node as left or right child of prev
        if(val > prev.val){
            prev.right = new TreeNode(val);
        } else {
            prev.left = new TreeNode(val);
        }
    }
}

```


---
---
# Optimal Approach 
### **Step 1: Start from the first number**

The first number in preorder is always the **root** of the tree.  
So we start building the tree from index `0`.

---

### **Step 2: Use an upper bound**

We pass an **upper limit** to each function call.

- A node value must be **smaller than the upper bound**.
    
- If it is bigger, it does not belong to that part of the tree.
    

---

### **Step 3: Base condition**

If:

- All numbers are used (`idx == length`), or
    
- The number is bigger than the upper bound
    

Then:  
→ We stop and return `null`.

This means **no node can be placed here**.

---
Else
### **Step 4: Create a new node**

If the number is valid:

- Create a new tree node using the current number.
    
- Move to the next number by increasing `idx`.
    

---

### **Step 5: Build the left subtree**

Left subtree values must be **smaller than the current node**.  
So we:

- Call the function again.
    
- Set the new upper bound as `node.val`.
    

---

### **Step 6: Build the right subtree**

Right subtree values must:

- Be **greater than the current node**
    
- But still **less than the previous upper bound**
    

So we:

- Call the function with the old upper bound.
    

---

### **Step 7: Return the node**

After left and right children are attached:  
→ Return the node to build the tree step by step.

```java
class Solution {

    int idx = 0; 
    // This index tells us which number from the preorder array we are using now

    public TreeNode bstFromPreorder(int[] preorder) {

        // We start building the BST from the first element
        // We give the largest possible value as the upper limit
        return help(Integer.MAX_VALUE , preorder);
    }

    public TreeNode help(int upperBound , int [] pre ){

        // If we have used all numbers OR
        // the current number is greater than the allowed upper bound,
        // then we cannot create a node here
        if(idx == pre.length || pre[idx] > upperBound){
            return null;
        }

        // Create a new tree node using the current preorder value
        // Then move index to the next number
        TreeNode node = new TreeNode(pre[idx++]);

        // Now we build the left subtree
        // Left subtree values must be smaller than current node value
        node.left = help(node.val , pre);

        // Now we build the right subtree
        // Right subtree values must be smaller than upperBound
        node.right = help(upperBound, pre);

        // Return the constructed node
        return node;
    }
}


```



---
---

# Trying other implementation for same logic 

```java
class Pointer{

    TreeNode node ; 

    int idx;

    Pointer( TreeNode node , int idx){

        this.node = node ; 

        this.idx = idx;

    }

}

class Solution {

    public TreeNode bstFromPreorder(int[] preorder) {

        return help(preorder , Integer.MAX_VALUE, 0 ).node;

    }

    public Pointer help(int arr[] ,int bound , int idx ){

        if(idx == arr.length || arr[idx] > bound){

            return new Pointer(null , idx);

        }

        TreeNode temp = new TreeNode(arr[idx++]);

        Pointer leftRes = help(arr, temp.val , idx);

        temp.left = leftRes.node;

        Pointer rightRes = help(arr, bound , leftRes.idx);

        temp.right = rightRes.node;

        return new Pointer(temp, rightRes.idx);

    }

}
```



# Array of size 1 method
```java
class Solution {

    public TreeNode bstFromPreorder(int[] preorder) {

        return help(preorder , new int[]{0} , Integer.MAX_VALUE);

    }

    public TreeNode help(int []pre , int idx[] , int bound){

        if(idx[0] == pre.length || pre[idx[0]] > bound){

            return null;

        }

        TreeNode temp = new TreeNode(pre[idx[0]++]);

        temp.left = help( pre , idx , temp.val);

        temp.right = help ( pre , idx , bound);

        return temp;

    }

}
```

