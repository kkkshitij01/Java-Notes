Given the **root** of a Binary Tree, find the **vertical traversal** of the tree starting from the leftmost level to the rightmost level.

**Note:** If there are multiple nodes passing through a vertical line, then they should be printed as they appear in **level order** traversal of the tree.

**Examples:**

**Input:** root =` [1, 2, 3, 4, 5, 6, 7, N, N, N, 8, N, 9, N, 10, 11, N]  `
     ![](https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/912839/Web/Other/blobid0_1759225165.webp)                    
**Output:**` [[4], [2], [1, 5, 6, 11], [3, 8, 9], [7], [10]]`
**Explanation:** The below image shows the horizontal distances used to print vertical traversal starting from the leftmost level to the rightmost level.  
     ![](https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/912839/Web/Other/blobid1_1759225180.webp)     

**Input:** root =` [1, 2, 3, 4, 5, N, 6]  `
     ![](https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/912839/Web/Other/blobid3_1759225275.webp)     
**Output:**` [[4], [2], [1, 5], [3], [6]]**Explanation:** From left to right the vertical order will be [[4], [2], [1, 5], [3], [6]]`

---
---
# Approach : 
### **1. Initialize**

- Create a `TreeMap<Integer, List<Integer>> map`.
    
- Create a queue for BFS.
    
- Push the root into the queue with HD = `0`.
    

---

### **2. Perform BFS traversal**

While the queue is not empty:

• Remove one `Pair` from the queue.  
• Extract:

- `node`
    
- `level` (horizontal distance)
    

---

### **3. Store node in TreeMap**

- If this `level` does not exist in map → create a new list.
    
- Add `node.data` to the list at this level.
    

---

### **4. Push children with updated horizontal distance**

- If left child exists → push `(left, level - 1)`
    
- If right child exists → push `(right, level + 1)`
    

This correctly assigns each node to its vertical line.

---

## **5. Build final answer**

- Traverse TreeMap from leftmost HD to rightmost HD.
    
- Add each list of nodes to the final answer.
    

---

## **Why BFS is important**

- BFS ensures nodes are added **top to bottom** within each vertical line.
    
- DFS could mix levels incorrectly.
    

---

## **Why TreeMap is used**

- Keeps vertical columns automatically sorted from left to right.
    
- No extra sorting needed later.
    

---

## **Time Complexity**

• **O(n log n)**

- BFS visits all nodes → O(n)
    
- TreeMap insertion → log n
    

## **Space Complexity**

• **O(n)**

- Queue + TreeMap storage.




```java
/*
class Node {
    int data;
    Node left;
    Node right;

    Node(int data) {
        this.data = data;
        left = null;
        right = null;
    }
}
*/
class Pair {
    Node node;
    int level;   // horizontal distance from root

    Pair(Node node, int level) {
        this.node = node;
        this.level = level;
    }
}

class Solution {
    public ArrayList<ArrayList<Integer>> verticalOrder(Node root) {
        
        // TreeMap stores vertical levels in sorted order (left to right)
        // Key   → horizontal distance
        // Value → list of node values at that distance
        Map<Integer, List<Integer>> map = new TreeMap<>();
        
        // Queue for level-order traversal (BFS)
        Queue<Pair> qu = new LinkedList<>();
        
        // Start from root with horizontal distance 0
        Node temp = root;
        qu.offer(new Pair(temp, 0));
        
        // BFS traversal of the tree
        while (!qu.isEmpty()) {
            
            Pair current = qu.poll();
            Node node = current.node;
            int level = current.level;

            // If this vertical level is seen for the first time,
            // create a new list for it
            if (!map.containsKey(level)) {
                map.put(level, new ArrayList<>());
            }

            // Add current node's value to its vertical level list
            map.get(level).add(node.data);

            // Move left → horizontal distance decreases by 1
            if (node.left != null) {
                qu.offer(new Pair(node.left, level - 1));
            }

            // Move right → horizontal distance increases by 1
            if (node.right != null) {
                qu.offer(new Pair(node.right, level + 1));
            }
        }
        
        // Prepare final answer list
        ArrayList<ArrayList<Integer>> ans = new ArrayList<>();
        
        // Traverse TreeMap in sorted order of horizontal distance
        for (Map.Entry<Integer, List<Integer>> entry : map.entrySet()) {

            // Add all nodes belonging to one vertical line
            ans.add(new ArrayList<>(entry.getValue()));
        }

        return ans;
    }
}

```