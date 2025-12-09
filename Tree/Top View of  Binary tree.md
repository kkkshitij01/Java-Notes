You are given the **root** of a binary tree, and your task is to return its **top view**. The top view of a binary tree is the set of nodes visible when the tree is viewed from the top.

**Note:**

- Return the nodes from the leftmost node to the rightmost node.
- If multiple nodes overlap at the same horizontal position, only the topmost (closest to the root) node is included in the view. 

**Examples:**

**Input:** root = `[1, 2, 3]  `

**Output:** `[2, 1, 3]  `

**Explanation:** The Green colored nodes represents the top view in the below Binary tree.  
 ![](https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/700490/Web/Other/blobid0_1733898095.png)

**Input:** root = `[10, 20, 30, 40, 60, 90, 100]  `

**Output:**` [40, 20, 10, 30, 100]  `

**Explanation:** The Green colored nodes represents the top view in the below Binary tree.  
  
![](https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/700490/Web/Other/blobid1_1733898122.png)  

**Constraints:**  
1 ≤ number of nodes ≤ 105  
1 ≤ node->data ≤ 105

---

# Approach:

## **1. Handle empty tree**

- If `root == null`, return empty list.
    

---

## **2. Initialize structures**

- `Queue<Pair> q` for BFS traversal
    
- `Map<Integer, Integer> map` (TreeMap) to store first seen node for each vertical level
    
- Push the root as a pair: `(root, 0)`
    

---

## **3. BFS Loop**

Continue until queue is empty.

### **For each level:**

- Poll `Pair current` from the queue
    
    - `temp = current.node`
        
    - `level = current.level`
        

### **A. Store in map if this level is new**

`if (!map.containsKey(level))     map.put(level, temp.data)`

This ensures **only the first node seen at this horizontal level** is recorded.

---

### **B. Add children to queue with updated levels**

- Left child → `level - 1`
    
- Right child → `level + 1`
    

`if (temp.left != null)     q.offer(new Pair(temp.left, level - 1))  if (temp.right != null)     q.offer(new Pair(temp.right, level + 1))`

---

## **4. Build final answer**

Traverse the TreeMap (which is sorted) and collect values:

`for each entry in map:     ans.add(entry.value)`

This gives nodes from **leftmost vertical line** to **rightmost vertical line**.

---
```java
class Pair {
    Node node;
    int level;   // horizontal distance (HD) from root

    Pair(Node nod, int lev) {
        this.node = nod;
        this.level = lev;
    }
}

class Solution {
    public ArrayList<Integer> topView(Node root) {

        ArrayList<Integer> ans = new ArrayList<>();
        if (root == null) return ans;

        // Queue for BFS, each element stores its node + horizontal distance
        Queue<Pair> q = new LinkedList<>();

        // TreeMap stores the first node seen at each horizontal level (sorted by key)
        Map<Integer, Integer> map = new TreeMap<>();

        // Start BFS from the root at horizontal distance 0
        q.offer(new Pair(root, 0));

        // BFS traversal
        while (!q.isEmpty()) {

            int size = q.size();

            while (size != 0) {

                Pair current = q.poll();
                Node temp = current.node;
                int level = current.level;

                // Store value if this horizontal distance is seen for the first time
                if (!map.containsKey(level)) {
                    map.put(level, temp.data);
                }

                // Move left: horizontal distance decreases by 1
                if (temp.left != null) {
                    q.offer(new Pair(temp.left, level - 1));
                }

                // Move right: horizontal distance increases by 1
                if (temp.right != null) {
                    q.offer(new Pair(temp.right, level + 1));
                }

                size--;
            }
        }

        // Extract values from TreeMap (already sorted by horizontal distance)
        for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
            ans.add(entry.getValue());
        }

        return ans;
    }
}


```