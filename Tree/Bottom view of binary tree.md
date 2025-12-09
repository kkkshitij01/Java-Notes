You are given the **root** of a binary tree, and your task is to return its **bottom view**. The bottom view of a binary tree is the set of nodes visible when the tree is viewed from the bottom.

**Note:** If there are **multiple** bottom-most nodes for a horizontal distance from the root, then the **latter** one in the level order traversal is considered.

**Examples :**

**Input:** root =` [1, 2, 3, 4, 5, N, 6] ` 
    ![](https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/912663/Web/Other/blobid2_1759760218.jpg)  
**Output:**`[4, 2, 5, 3, 6]  `

**Explanation:** The Green nodes represent the bottom view of below binary tree.  
    ![](https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/912663/Web/Other/blobid3_1759760226.jpg)    

**Input:** root = [20, 8, 22, 5, 3, 4, 25, N, N, 10, 14, N, N, 28, N]  
    ![](https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/912663/Web/Other/blobid0_1759760166.jpg)    
    **Output:**` [5, 10, 4, 28, 25]  `
    
**Explanation:** The Green nodes represent the bottom view of below binary tree.  
    ![](https://media.geeksforgeeks.org/img-practice/prod/addEditProblem/912663/Web/Other/blobid1_1759760190.jpg)


---

# Approach: 
## **1. Handle empty tree**

- If `root == null`, return empty answer list.
    

---

## **2. Initialize**

- `map` → TreeMap storing hd → bottom node
    
- `q` → queue for BFS
    
- Add root as a pair: `(root, 0)`
    

---

## **3. BFS traversal**

Continue until the queue becomes empty.

### For each node in the queue:

#### **A. Poll the node**

`temp = current.node level = current.level`

#### **B. Update the map**

`map.put(level, temp.data)`

⚠ **This overwrites previous values**, meaning bottom-most (last visited) node is retained  
→ exactly what we want for bottom view.

---

#### **C. Add children with updated hd**

- Left child → hd - 1
    
- Right child → hd + 1
    

`if (temp.left != null)     q.offer(new Pair(temp.left, level - 1));  if (temp.right != null)     q.offer(new Pair(temp.right, level + 1));`

---

## **4. Build final answer**

- Iterate over TreeMap entries in sorted order.
    
- Add each value to the answer list.
    

TreeMap guarantees output from **leftmost hd to rightmost hd**.


```java
class Pair {
    Node node;
    int level; // horizontal distance (HD)

    Pair(Node node, int level) {
        this.node = node;
        this.level = level;
    }
}

class Solution {
    public ArrayList<Integer> bottomView(Node root) {

        ArrayList<Integer> ans = new ArrayList<>();
        if (root == null) return ans;

        // TreeMap stores (HD → node value), sorted by HD automatically
        Map<Integer, Integer> map = new TreeMap<>();

        // Queue for BFS (node + horizontal distance)
        Queue<Pair> q = new LinkedList<>();
        q.offer(new Pair(root, 0));

        // BFS traversal
        while (!q.isEmpty()) {

            int size = q.size();

            while (size != 0) {

                Pair current = q.poll();
                Node temp = current.node;
                int level = current.level;

                // For bottom view: always overwrite value at this horizontal level
                map.put(level, temp.data);

                // Move left child → HD decreases by 1
                if (temp.left != null) {
                    q.offer(new Pair(temp.left, level - 1));
                }

                // Move right child → HD increases by 1
                if (temp.right != null) {
                    q.offer(new Pair(temp.right, level + 1));
                }

                size--;
            }
        }

        // Collect values in sorted order of horizontal distance
        for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
            ans.add(entry.getValue());
        }

        return ans;
    }
}

```