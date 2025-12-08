

Given the `root` of a binary tree, return _the level order traversal of its nodes' values_. (i.e., from left to right, level by level).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)

**Input:** root = `[3,9,20,null,null,15,7]`
**Output:** `[[3],[9,20],[15,7]]`

**Example 2:**

**Input:** root = `[1]`
**Output:** `[[1]]`

**Example 3:**

**Input:** root = `[]`
**Output:** `[]`

---

# Approach:
### **1. Handle Empty Tree**

- If `root == null`, return an empty list because there is no tree to process.
    

---

### **2. Setup**

- Create a result list `ans` which will contain lists for each level.
    
- Create a queue `q` to assist with BFS.
    
- Add the root node to the queue (`q.offer(root)`).
    

---

### **3. Process the Tree Level by Level**

We use a **while loop** that runs until the queue becomes empty:

#### **A. Determine how many nodes belong to this level**

- `size = q.size()`
    
- This tells us exactly how many nodes are in the current level.
    

#### **B. Create a new list to store this level's values**

- `subList = new ArrayList<>()`
    

#### **C. Process each node of the current level**

Repeat `size` times:

1. Remove the front node from the queue:  
    `temp = q.poll()`
    
2. Add its value to `subList`.
    
3. If it has a left child → add to queue.
    
4. If it has a right child → add to queue.
    

When all `size` nodes are processed, the entire level is complete.

#### **D. Add this completed level to the answer**

- `ans.add(subList)`
    

The loop continues until the queue becomes empty.

---

## **4. Return the Final List**

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {

        List<List<Integer>> ans = new ArrayList<>();
        Queue<TreeNode> q = new LinkedList<>();

        // If tree is empty, return empty list
        if (root == null) {
            return ans;
        }

        // Start BFS from the root
        q.offer(root);

        while (!q.isEmpty()) {

            int size = q.size();              // number of nodes at current level
            ArrayList<Integer> subList = new ArrayList<>();

            // Process all nodes in the current level
            while (size != 0) {

                TreeNode temp = q.poll();     // take next node in queue
                subList.add(temp.val);        // add its value to the level list

                // Add left child if exists
                if (temp.left != null) {
                    q.offer(temp.left);
                }

                // Add right child if exists
                if (temp.right != null) {
                    q.offer(temp.right);
                }

                size--; // decrease nodes left in this level
            }

            // Add this level's values to the final answer
            ans.add(subList);
        }

        return ans;
    }
}

```