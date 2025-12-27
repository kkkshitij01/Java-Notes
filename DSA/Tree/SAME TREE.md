Given the roots of two binary trees `p` and `q`, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/12/20/ex1.jpg)

**Input:** p = `[1,2,3], q = [1,2,3]`
**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/12/20/ex2.jpg)

**Input:** p = `[1,2], q = [1,null,2]`
**Output:** false

**Example 3:**

![](https://assets.leetcode.com/uploads/2020/12/20/ex3.jpg)

**Input:** p = `[1,2,1], q = [1,1,2]`
**Output:** false

---

# Approach 1 :
### **Part 1: Base Condition**

There are two base cases:

- If **both nodes are `null`**
    
    - Trees match till here
        
    - Return `true`
        
- If **one node is `null` and the other is not**
    
    - Trees are different
        
    - Return `false`
        
- If **values of nodes are not equal**
    
    - Trees are different
        
    - Return `false`
        

---

### **Part 2: Work Done at Current Node**

At the current node:

- Compare the value of `p.val` and `q.val`
    
- If values do not match, return `false`
    

---

### **Part 3: Breaking Into Smaller Problems**

If current nodes match:

- Recursively check:
    
    - Left subtree of `p` with left subtree of `q`
        
    - Right subtree of `p` with right subtree of `q`
        
- Store results:
    
    - `left = check left subtrees`
        
    - `right = check right subtrees`
        
- Return:
    

`left && right`

Both sides must match.

---
## **Time and Space Complexity**

### **Time Complexity**

`O(n)`

Each node is visited once.

### **Space Complexity**

`O(h)`

Where `h` is the height of the tree (recursion stack).


```java
class Solution {

    // This function checks if both trees are exactly same
    public boolean preOrderTraversal(TreeNode p , TreeNode q){

        // If both nodes are null, trees match here, so return true
        if(p == null && q == null) return true;

        // If one is null OR values are not same, trees are not same, return false
        if(p == null || q == null || p.val != q.val) return false;

        // Check left parts of both trees
        boolean left = preOrderTraversal(p.left , q.left);

        // Check right parts of both trees
        boolean right = preOrderTraversal(p.right , q.right);

        // Both sides must be same, so return left AND right result
        return left && right;
    }

    // This function starts the checking from root
    public boolean isSameTree(TreeNode p, TreeNode q) {

        // Call preorder check on both trees
        return preOrderTraversal(p, q);
    }
}

```


---
---


# Optimal and Clean : 
### **Part 1 — Base Conditions**

- If `p` and `q` are both `null` → no nodes here, so they match → return `true`
    
- If only one is `null` → structure breaks → return `false`
    
- If `p.val` ≠ `q.val` → values differ → return `false`
    

### **Part 2 — Work at Current Node**

- Compare `p.val` with `q.val`
    
- If equal, continue checking children
    

### **Part 3 — Break into Smaller Questions**

- Ask: Are left subtrees same? → `isSameTree(p.left, q.left)`
    
- Ask: Are right subtrees same? → `isSameTree(p.right, q.right)`
    
- Both must be true, so we return:
    

`leftResult && rightResult`

### **Key Notes**

- We check **node + structure together**, not separately
    
- Recursion moves down until leaf nodes
    
- First mismatch stops further calls early
    

### **Complexity**

|Time|Space|
|---|---|
|O(n)|O(h)|

- `n` = total nodes
    
- `h` = tree height (recursion stack)

```java
class Solution {
   
    public boolean isSameTree(TreeNode p, TreeNode q) {

        // If both nodes are empty (null), then they are same here
        if(p == null && q == null) return true;

        // If one node is empty OR values are different, trees are not same
        if(p == null || q == null || p.val != q.val) return false;

        // Check both left sides AND right sides, both must be same
        return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
    }
}

```
