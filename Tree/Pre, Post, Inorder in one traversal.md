
Given a binary tree with root node. Return the **In-order**,**Pre-order** and **Post-order** traversal of the binary tree.

Examples:

**Input :** root = `[1, 3, 4, 5, 2, 7, 6 ]`

**Output :** `[ [5, 3, 2, 1, 7, 4, 6] , [1, 3, 5, 2, 4, 7, 6] , [5, 2, 3, 7, 6, 4, 1] ]`

**Explanation :** The In-order traversal is `[5, 3, 2, 1, 7, 4, 6].`

The Pre-order traversal is `[1, 3, 5, 2, 4, 7, 6].`

The Post-order traversal is` [5, 2, 3, 7, 6, 4, 1].`

  

![](https://static.takeuforward.org/content/ProblemSetter-rOlkMuo4)

**Input :** root = `[1, 2, 3, null, null, null, 6 ]`

**Output :**` [ [2, 1, 3, 6] , [1, 2, 3, 6] , [2, 6, 3, 1] ]`

**Explanation :** The In-order traversal is `[2, 1, 3, 6].`

The Pre-order traversal is `[1, 2, 3, 6].`

The Post-order traversal is `[2, 6, 3, 1].`

![](https://static.takeuforward.org/content/ProblemSetter-OExDzFDr)



---
## **1. Initialize Lists**

- `preorder`
    
- `inorder`
    
- `postorder`
    

These will store the respective traversals.

---

## **2. Use a Stack to Control Traversal**

- Create a stack `sc`.
    
- Push `(root, 1)` because we start processing the root for **preorder**.
    

---

## **3. Process Stack Until Empty**

### **Case A: `count == 1` → Preorder Step**

- Add current node's data to `preorder`.
    
- Push the same node back with `count = 2` (next stage).
    
- If it has a left child, push `(left, 1)` to process its subtree.
    

This imitates:

`preorder: root → left → right`

---

### **Case B: `count == 2` → Inorder Step**

- Add to `inorder`.
    
- Push same node back with `count = 3`.
    
- If right child exists, push `(right, 1)`.
    

This imitates:

`inorder: left → root → right`

---

### **Case C: `count == 3` → Postorder Step**

- Add to `postorder`.
    
- Do not push node back; this node is fully processed.
    

This imitates:

`postorder: left → right → root`

---

# **4. Build Final Output**

Return a list containing:

1. inorder
    
2. preorder
    
3. postorder
    

(Your order is correct based on how you added them.)

---

```java
class Pair {
    TreeNode node;
    int count;

    Pair(TreeNode node, int count) {
        this.node = node;
        this.count = count;
    }
}

class Solution {
    List<List<Integer>> treeTraversal(TreeNode root) {

        List<Integer> preorder = new ArrayList<>();
        List<Integer> inorder = new ArrayList<>();
        List<Integer> postorder = new ArrayList<>();
        List<List<Integer>> ans = new ArrayList<>();

        // If tree is empty, return empty result
        if (root == null) {
            ans.add(preorder);
            ans.add(inorder);
            ans.add(postorder);
            return ans;
        }

        Stack<Pair> sc = new Stack<>();
        sc.push(new Pair(root, 1));

        // Iterative traversal using a single stack
        while (!sc.isEmpty()) {

            Pair current = sc.pop();
            TreeNode temp = current.node;
            int count = current.count;

            // count = 1 → Preorder position
            if (count == 1) {
                preorder.add(temp.data);

                // Push same node for inorder step
                sc.push(new Pair(temp, 2));

                // Preorder: Go left
                if (temp.left != null) {
                    sc.push(new Pair(temp.left, 1));
                }
            }

            // count = 2 → Inorder position
            else if (count == 2) {
                inorder.add(temp.data);

                // Push same node for postorder step
                sc.push(new Pair(temp, 3));

                // Inorder: Go right
                if (temp.right != null) {
                    sc.push(new Pair(temp.right, 1));
                }
            }

            // count = 3 → Postorder position
            else {
                postorder.add(temp.data);
            }
        }

        // Correct order: preorder, inorder, postorder
        ans.add(preorder);
        ans.add(inorder);
        ans.add(postorder);

        return ans;
    }
}

```