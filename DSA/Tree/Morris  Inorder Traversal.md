Q: Print Inorder traversal of tree in Optimal way : 

---

# Approach: 

### **1. Initialize**

- `curr = root`
    
- `ans` = list to store inorder result
    

---

## **2. Traverse while `curr != null`**

### **Case A: `curr.left == null`**

- No left subtree → directly visit this node (in inorder, we process node after left subtree)
    
- Add `curr.data` to answer
    
- Move right: `curr = curr.right`
    

---

### **Case B: `curr.left != null`**

- Find the **rightmost node** in the left subtree (`prev`)
    

`while (prev.right != null && prev.right != curr)     prev = prev.right`

#### **Case B1: Thread does not exist (`prev.right == null`)**

- Create a thread to return later:  
    `prev.right = curr`
    
- Move left: `curr = curr.left`
    

> (Do NOT visit `curr.data` yet — inorder visits after left subtree)

---

#### **Case B2: Thread exists (`prev.right == curr`)**

- This means the left subtree has finished processing
    
- Remove the thread: `prev.right = null`
    
- Visit the node → add `curr.data`
    
- Move right: `curr = curr.right`
    

---

## **3. Return the result**

The list contains the inorder traversal in O(n) time and O(1) space.

```java
class Solution {
    public ArrayList<Integer> inOrder(Node root) {

        ArrayList<Integer> ans = new ArrayList<>();
        Node curr = root;

        while (curr != null) {

            // Case 1: No left child → visit node and move right
            if (curr.left == null) {
                ans.add(curr.data);       // inorder: process node when coming from left
                curr = curr.right;
            } 
            else {

                // Case 2: Find inorder predecessor (rightmost node in curr.left)
                Node prev = curr.left;
                while (prev.right != null && prev.right != curr) {
                    prev = prev.right;
                }

                // Case 2a: First time visiting predecessor → create temporary link
                if (prev.right == null) {
                    prev.right = curr;     // make threaded link
                    curr = curr.left;      // move to left subtree
                } 
                else {

                    // Case 2b: Thread already exists → remove it and visit curr
                    prev.right = null;     // restore original tree
                    ans.add(curr.data);    // inorder: visit now after left is done
                    curr = curr.right;     // move to right subtree
                }
            }
        }

        return ans;
    }
}

```