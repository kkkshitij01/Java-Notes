Q: Print preorder traversal of tree in Optimal way : 

# Approach: 
### **1. Initialize**

- `curr = root`
    
- `ans` = list to store preorder traversal
    
---

## **2. Traverse while `curr != null`**

### **Case A: `curr.left == null`**

- No left subtree → visit node immediately (preorder requires visiting root first)
    
- Add `curr.data` to result
    
- Move to right: `curr = curr.right`
    

---

### **Case B: `curr.left != null`**

- Find the **rightmost node** in the left subtree → call it `prev`
    

`while (prev.right != null && prev.right != curr)     prev = prev.right`

#### **Case B1: Thread does not exist (`prev.right == null`)**

- Create the thread: `prev.right = curr`
    
- Visit the current node → add `curr.data` (because preorder)
    
- Move left: `curr = curr.left`
    

#### **Case B2: Thread exists (`prev.right == curr`)**

- This means left subtree is completely processed
    
- Remove the thread: `prev.right = null`
    
- Move to right: `curr = curr.right`
    

---

## **3. Return the traversal list**

The list now contains preorder traversal in O(n) time and O(1) extra space.


```java
class Solution {
    public ArrayList<Integer> preOrder(Node root) {

        ArrayList<Integer> ans = new ArrayList<>();
        Node curr = root;

        // Traverse until all nodes are processed
        while (curr != null) {

            // Case 1: No left child → visit node and move right
            if (curr.left == null) {
                ans.add(curr.data);   // preorder: process node
                curr = curr.right;
            } 
            else {

                // Case 2: Find the inorder predecessor (rightmost node in left subtree)
                Node prev = curr.left;
                while (prev.right != null && prev.right != curr) {
                    prev = prev.right;
                }

                // Case 2a: First time visiting this predecessor
                if (prev.right == null) {
                    prev.right = curr;       // create temporary link
                    ans.add(curr.data);      // preorder: visit node before going left
                    curr = curr.left;        // move to left subtree
                } 
                else {

                    // Case 2b: Link already exists → remove link and go right
                    prev.right = null;       // restore tree
                    curr = curr.right;
                }
            }
        }

        return ans;
    }
}

```