Given a **Binary Tree**, you need to **find all the possible paths** from the **root node** to all the **leaf nodes** of the binary tree.

**Note:** The paths should be returned such that paths from the left subtree of any node are **listed first**, followed by paths from the right subtree.

**Examples:**

**Input:** root`[] = [1, 2, 3, 4, 5, N, N]`
![ex-3](https://media.geeksforgeeks.org/wp-content/uploads/20241007105251989873/ex-3.webp)
**Output:**` [[1, 2, 4], [1, 2, 5], [1, 3]]`
**Explanation:** All the possible paths from root node to leaf nodes are: 1 -> 2 -> 4, 1 -> 2 -> 5 and 1 -> 3

**Input:** roo`t[] = [1, 2, 3]  `
![](https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/700553/Web/Other/blobid0_1745821559.jpg)  
**Output:**` [[1, 2], [1, 3]] `
**Explanation:** All the possible paths from root node to leaf nodes are: 1 -> 2 and 1 -> 3

**Input:** root`[] = [10, 20, 30, 40, 60, N, N]`
**![](https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/700553/Web/Other/blobid1_1745821586.jpg)  
Output:**` [[10, 20, 40], [10, 20, 60], [10, 30]]`  
**Explanation:** All the possible paths from root node to leaf nodes are: 10 -> 20 -> 40, 10 -> 20 -> 60 and 10 -> 30

---


# Approach: 
• **Function: `Paths(Node root)`**  
→ Finds **all root to leaf paths** in a binary tree.  
→ Left subtree paths are added **before** right subtree paths.

---

• **Create an empty list `ans`**  
→ Stores all root-to-leaf paths.

• **If `root` is null**  
→ Return empty `ans`.

• **Create a temporary list `temp`**  
→ Stores the current path from root to current node.

• **Call recursive function `solve(root, ans, temp)`**

---

• **Function: `solve(Node root, ans, temp)`**

• **If current node is a leaf**  
→ (`root.left == null && root.right == null`)  
→ Add `root.data` to `temp`.  
→ Add a **copy** of `temp` to `ans`.  
→ Remove last element from `temp` (backtrack).  
→ Return.

---

• **If current node is not a leaf**  
→ Add `root.data` to `temp`.

• **If left child exists**  
→ Call `solve(root.left, ans, temp)`.

• **If right child exists**  
→ Call `solve(root.right, ans, temp)`.

• **After exploring both sides**  
→ Remove last element from `temp` (backtracking).

---

• **Why backtracking is needed**  
→ Ensures the path is reset correctly when moving to another branch.

• **Why left paths come first**  
→ Left recursion is called before right recursion.

---

• **Time Complexity:** `O(n)`  
→ Every node is visited once.

• **Space Complexity:** `O(h)`  
→ `h` = height of the tree (recursion stack).

```java
/*
Definition for Binary Tree Node
class Node
{
    int data;
    Node left;
    Node right;

    Node(int data)
    {
        this.data = data;
        left = null;
        right = null;
    }
}
*/

class Solution {
    public static ArrayList<ArrayList<Integer>> Paths(Node root) {

        ArrayList<ArrayList<Integer>> ans = new ArrayList<>();

        // If tree is empty, there are no root-to-leaf paths
        if(root == null) return ans;

        // Temporary list to store current path
        ArrayList<Integer> temp = new ArrayList<>();

        // Start DFS from root
        solve(root, ans, temp);

        return ans;
    }

    public static void solve(Node root, ArrayList<ArrayList<Integer>> ans, ArrayList<Integer> temp){

        // If current node is a leaf node
        // (no left and no right child)
        if(root.left == null && root.right == null){

            // Add leaf node to current path
            temp.add(root.data);

            // Store a copy of the current path in answer
            ans.add(new ArrayList<>(temp));

            // Backtrack: remove the leaf node before returning
            temp.remove(temp.size() - 1);
            return;
        }

        // Add current node to the path
        temp.add(root.data);

        // Recur for left subtree if it exists
        if(root.left != null){
            solve(root.left, ans, temp);
        }

        // Recur for right subtree if it exists
        if(root.right != null){
            solve(root.right, ans, temp);
        }

        // Backtrack: remove current node before going up the recursion
        temp.remove(temp.size() - 1);
    }
}

```