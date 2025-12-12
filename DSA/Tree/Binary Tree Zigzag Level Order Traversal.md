Given the `root` of a binary tree, return _the zigzag level order traversal of its nodes' values_. (i.e., from left to right, then right to left for the next level and alternate between).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)

**Input:** root = `[3,9,20,null,null,15,7]`
**Output:** `[[3],[20,9],[15,7]]`

**Example 2:**

**Input:** root = `[1]`
**Output:** `[[1]]`

**Example 3:**

**Input:** root = `[]`
**Output:** `[]`

---
# Approach: 

### **1. Handle Empty Tree**

- If `root == null`, return an empty list.
    

---

### **2. Setup**

- Create `ans` → final list of all levels.
    
- Use a queue `q` for BFS traversal.
    
- Add root to queue.
    
- Create a boolean `rl` (right-to-left flag) initialized to `false`.
    
    - `false` → traverse normally (left → right)
        
    - `true` → reverse level output (right → left)
        

---

## **3. BFS Loop — Process the Tree Level by Level**

### **A. Get current level size**

- `size = q.size()`
    
- This tells how many nodes belong to the current level.
    

---

### **B. Read all nodes at this level**

- Create `levelList = new ArrayList<>()`.
    
- For each of the `size` nodes:
    
    1. Pop node from queue: `temp = q.poll()`
        
    2. Add its value to `levelList`
        
    3. Add its left child to queue (if exists)
        
    4. Add its right child to queue (if exists)
        

This collects all nodes _in natural left-to-right order_.

---

### **C. Reverse the level if needed**

- If `rl == true`, reverse the list:
    
    `Collections.reverse(levelList)`
    

---

### **D. Flip the direction flag**

- Toggle `rl = !rl`
    
- This ensures next level uses the opposite direction.
    

---

### **E. Add the processed level to result**

- Append `levelList` to `ans`.
    

---

## **4. Return the final zigzag traversal**

The `ans` list now contains all levels in zigzag order.


```java
class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {

        List<List<Integer>> ans = new ArrayList<>();

        // Flag to decide whether to reverse the current level
        boolean rl = false;

        if (root == null) return ans;

        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);

        while (!q.isEmpty()) {

            ArrayList<Integer> levelList = new ArrayList<>();
            int size = q.size(); // number of nodes in this level

            // Process all nodes of the current level
            while (size != 0) {
                TreeNode temp = q.poll();
                levelList.add(temp.val);

                // Push children for next level
                if (temp.left != null) q.offer(temp.left);
                if (temp.right != null) q.offer(temp.right);

                size--;
            }

            // Reverse the list if this level is a right-to-left level
            if (rl) {
                Collections.reverse(levelList);
            }

            // Flip direction for next level
            rl = !rl;

            ans.add(levelList);
        }

        return ans;
    }
}

```