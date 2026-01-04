You are given a BST(Binary Search Tree) with **n** number of nodes and value **x**. your task is to find the greatest value node of the BST which is smaller than or equal to x.  
**Note:** when x is smaller than the smallest node of BST then returns -1.

**Examples:**

**Input:**
n = 7       
```
					2
                     \
                      81
                    /     \
                 42       87
                   \       \
                    66      90
                   /
                 45
```
x = 87

**Output:** 87

**Explanation:** 87 is present in tree so floor will be 87.

**Input:**
n = 4       
```
						  6
                           \
                            8
                          /   \
                        7       9
```                        
x = 11

**Output:** 9

**Input:**
n = 4 
```                       
						  6
                           \
                            8
                          /   \
                        7       9
```
x = 5

**Output:** -1  

**Constraint:**  
1 <= Node data <= 109  
1 <= n <= 105

---
---

# Optimal
### **Function: `floor(root, x)` — Notes**

- **Goal:** find the largest value in BST that is `≤ x`
    
- Create `ans = -1` to store possible floor value
    
- While `root` is not `null`:
    
    - If `root.data == x` → exact floor found → return `x`
        
    - If `root.data < x` → it can be a floor → save in `ans` → move to `root.right` to find a bigger valid floor
        
    - If `root.data > x` → floor must be smaller → move to `root.left`
        
- Return `ans` → final stored floor value
    

### **TC & SC**

| Metric | Complexity | Reason                            |
| ------ | ---------- | --------------------------------- |
| **TC** | `O(h)`     | Only one path is traversed in BST |
| **SC** | `O(1)`     | No extra space used               |

```java
class Solution {
    public static int floor(Node root, int x) {
        int ans = -1; // Store the potential answer here

        while (root != null) {
            if (root.data == x) {
                return x; // Exact match is always the floor
            } else if (root.data < x) {
                ans = root.data; //  Remember this node! It might be the floor.
                root = root.right; // Try to find a larger value that fits
            } else {
                // Current value is too big, floor must be on the left
                root = root.left;
            }
        }
        return ans;
    }
}
```