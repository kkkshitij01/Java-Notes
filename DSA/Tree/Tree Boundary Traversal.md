Given a **root** of a Binary Tree, return its **boundary traversal** in the following order:

1. **Left Boundary:** Nodes from the root to the leftmost non-leaf node, preferring the left child over the right and excluding leaves.
    
2. **Leaf Nodes:** All leaf nodes from left to right, covering every leaf in the tree.
    
3. **Reverse Right Boundary:** Nodes from the root to the rightmost non-leaf node, preferring the right child over the left, excluding leaves, and added in reverse order.
    

**Note:** The root is included once, leaves are added separately to avoid repetition, and the right boundary follows traversal preference not the path from the rightmost leaf.

**Examples:**

**Input:** root = `[1, 2, 3, 4, 5, 6, 7, N, N, 8, 9, N, N, N, N]`

**Output:** `[1, 2, 4, 8, 9, 6, 7, 3]`

**Explanation:**

![](https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/700204/Web/Other/blobid6_1749213679.webp)

**Input:** root =` [1, N, 2, N, 3, N, 4, N, N]`

**Output:** `[1, 4, 3, 2]`

**Explanation:  

![](https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/912584/Web/Other/blobid0_1759298509.jpg)  
**Left boundary:` [1] (as there is no left subtree)`
Leaf nodes: `[4]`
Right boundary:` [3, 2] (in reverse order)`
Final traversal: `[1, 4, 3, 2]`

**Constraints:**  
1 ≤ number of nodes ≤ 105  
1 ≤ node->data ≤ 105

---
---

# Approach 

**Function: `boundaryTraversal(root)`**

- Create empty `ans` list.
    
- Add `root.data` first (root is always part of boundary).
    
- If root is also leaf → return `ans` (no more work).
    
- Call `Left(root.left, ans)` → add left boundary nodes.
    
- Call `leaf(root, ans)` → add all leaf nodes.
    
- Call `right(root.right, ans)` → add right boundary nodes in reverse.
    
- Return `ans` → final boundary order = **root → left boundary → leaves → right boundary reversed**.


**Function: `Left(root, ans)`**

- Go to left boundary only.
    
- If node is `null` OR leaf → stop (don’t add).
    
- Add `root.data` to `ans`.
    
- If `root.left` exists → move left, else move right.
    

**Function: `leaf(root, ans)`**

- Visit every node.
    
- If node is leaf → add `root.data`.
    
- Recur left and right to collect all leaves.
    

**Function: `right(root, ans)`**

- Go to right boundary only (bottom-up).
    
- If node is `null` OR leaf → stop.
    
- If `root.right` exists → move right, else move left.
    
- Add `root.data` to `ans` **after recursion** so it stores in reverse order.
    
```java
class Solution {

    // collect left boundary nodes
    public void Left(Node root , ArrayList<Integer> ans) {

        // stop if null or leaf node (leaf handled later)
        if(root == null || (root.left == null && root.right == null)) {
            return;
        }

        // add this node because it's on left boundary
        ans.add(root.data);

        // go left if possible, else go right as fallback boundary
        if(root.left != null) {
            Left(root.left, ans);
        } else {
            Left(root.right, ans);
        }
    }

    // collect all leaf nodes
    public void leaf(Node root, ArrayList<Integer> ans) {

        // stop if null
        if(root == null) {
            return;
        }

        // if this is a leaf node, add it
        if(root.left == null && root.right == null) {
            ans.add(root.data);
        }

        // check left and right for more leaf nodes
        leaf(root.left, ans);
        leaf(root.right, ans);
    }

    // collect right boundary nodes in reverse order
    public void right(Node root, ArrayList<Integer> ans) {

        // stop if null or leaf
        if(root == null || (root.left == null && root.right == null)) {
            return;
        }

        // go right first to reach bottom boundary, else go left
        if(root.right != null) {
            right(root.right, ans);
        } else {
            right(root.left, ans);
        }

        // add node after recursion to keep bottom-to-top order
        ans.add(root.data);
    }

    // main function to return boundary traversal
    public ArrayList<Integer> boundaryTraversal(Node root) {

        // list to store final answer
        ArrayList<Integer> ans = new ArrayList<>();

        // add root first
        ans.add(root.data);

        // if only one node, return
        if(root.left == null && root.right == null) {
            return ans;
        }

        // collect left boundary
        Left(root.left, ans);

        // collect leaf nodes
        leaf(root, ans);

        // collect right boundary
        right(root.right, ans);

        // return final boundary list
        return ans;
    }
}

```