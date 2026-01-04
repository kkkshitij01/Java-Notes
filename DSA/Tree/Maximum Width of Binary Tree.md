Given the `root` of a binary tree, return _the **maximum width** of the given tree_.

The **maximum width** of a tree is the maximum **width** among all levels.

The **width** of one level is defined as the length between the end-nodes (the leftmost and rightmost non-null nodes), where the null nodes between the end-nodes that would be present in a complete binary tree extending down to that level are also counted into the length calculation.

It is **guaranteed** that the answer will in the range of a **32-bit** signed integer.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/05/03/width1-tree.jpg)

**Input:** root = `[1,3,2,5,3,null,9]`

**Output:** 4

**Explanation:** The maximum width exists in the third level with length 4 (5,3,null,9).

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/03/14/maximum-width-of-binary-tree-v3.jpg)

**Input:** root = `[1,3,2,5,null,null,9,6,null,7]`

**Output:** 7

**Explanation:** The maximum width exists in the fourth level with length 7 (6,null,null,null,null,null,7).

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/05/03/width3-tree.jpg)

**Input:** root = `[1,3,2,5]`

**Output:** 2

**Explanation:** The maximum width exists in the second level with length 2 (3,2).

**Constraints:**

- The number of nodes in the tree is in the range `[1, 3000]`.
- `-100 <= Node.val <= 100`
---
---

# Optimal : 
**Function: `widthOfBinaryTree(root)`**

- Use **BFS (level order)** with a queue `q` storing `{node, index}` pairs.

- Start by adding root with index `0`.
    
- For each level:
    
    - Take `size = q.size()` → number of nodes at this level.
        
    - For each node in this level:
        
        - If `i == 0` → store `min = index` → left-most index of this level.
            
        - Add left child with index → `2*(index - min) + 1`.
            
        - Add right child with index → `2*(index - min) + 2`.
            
        - If `i == 0` → store `first = index` → first node index.
            
        - If `i == size-1` → store `last = index` → last node index.
            
    - Width of level = `last - first + 1`.
        
    - Update `ans` if this width is larger.
        
- Return `ans` → maximum width found in any level.
    

### **Why we do `currIdx - min`**

- To **shrink index values per level** so they don’t become too large.
    
- It keeps positions correct while avoiding integer overflow.

|   |   |   |
|---|---|---|
|**Time Complexity (TC)**|`O(n)`|BFS visits every node once|

|                           |        |                                                            |
| ------------------------- | ------ | ---------------------------------------------------------- |
| **Space Complexity (SC)** | `O(n)` | Queue stores at most one full level (worst case ≈ n nodes) |

```java
class Pair{
    TreeNode node ; // stores the tree node
    int idx ;       // stores the index of node in that level
    Pair(TreeNode node , int idx){
        this.node = node ;
        this.idx = idx;
    }
}

class Solution {
    public int widthOfBinaryTree(TreeNode root) {

        // Queue used for level order traversal (BFS)
        Queue<Pair> q = new LinkedList<>();

        int ans = 0; // stores the max width found

        // add root with index 0
        q.offer(new Pair(root , 0));

        // start BFS
        while(!q.isEmpty()){

            int size = q.size(); // number of nodes in current level
            int first = 0, last = 0, min = 0;

            for(int i = 0; i < size; i++){

                // remove front pair from queue
                Pair temp = q.poll();
                TreeNode node = temp.node;
                int currIdx = temp.idx; // actual index of this node

                // store minimum index of level to normalize large index values
                if(i == 0){
                    min = currIdx; // first node index is min
                }

                // add left child with formula 2*(normalized index)+1
                // why? because in binary tree, left child index follows this rule
                if(node.left != null){
                    q.offer(new Pair(node.left , 2*(currIdx - min) + 1));
                }

                // add right child with formula 2*(normalized index)+2
                if(node.right != null){
                    q.offer(new Pair(node.right , 2*(currIdx - min) + 2));
                }

                // store first node index of this level
                // why? because width starts from first node
                if(i == 0){
                    first = currIdx;
                }

                // store last node index of this level
                // why? because width ends at last node
                if(i == size - 1){
                    last = currIdx;
                }
            }

            // update ans if this level width is greater
            // width = last index - first index + 1
            if(ans < last - first + 1){
                ans = last - first + 1;
            }
        }

        return ans;
    }
}

```