
Given two integer arrays `preorder` and `inorder` where `preorder` is the preorder traversal of a binary tree and `inorder` is the inorder traversal of the same tree, construct and return _the binary tree_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)

**Input:** preorder = `[3,9,20,15,7], inorder = [9,3,15,20,7]
**Output:** `[3,9,20,null,null,15,7]``

**Example 2:**

**Input:** preorder =` [-1], inorder = [-1]`
**Output:** `[-1]`

**Constraints:**

- `1 <= preorder.length <= 3000`
- `inorder.length == preorder.length`
- `-3000 <= preorder[i], inorder[i] <= 3000`
- `preorder` and `inorder` consist of **unique** values.
- Each value of `inorder` also appears in `preorder`.
- `preorder` is **guaranteed** to be the preorder traversal of the tree.
- `inorder` is **guaranteed** to be the inorder traversal of the tree.


---
---

# Optimal : 

```java
class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {

        // map stores value -> index in inorder array
        // helps to find root position quickly
        HashMap<Integer , Integer > map = new HashMap<>();

        // fill the map with inorder values and their indices
        for(int i = 0 ; i< inorder.length; i++){
            map.put(inorder[i], i);
        }

        // start recursion with full inorder range
        // preItr[0] keeps track of current index in preorder
        return solve(preorder, inorder , new int[1] , 0 , inorder.length-1 , map);
    }

    public TreeNode solve(int preorder[] , int inorder[] , int preItr[] , int left , int right , HashMap<Integer,Integer> map){

        // if range becomes invalid, no node to create
        if(left> right ) return null;

        // get index of current root in inorder array
        int idx = map.get(preorder[preItr[0]]); // index of current Element

        // create node using current preorder value
        // then move preorder pointer forward
        TreeNode node = new TreeNode(preorder[preItr[0]++]);

        // build left subtree using left part of inorder
        node.left = solve(preorder , inorder , preItr , left , idx-1 , map);

        // build right subtree using right part of inorder
        node.right = solve(preorder , inorder , preItr, idx+1 , right , map);

        // return constructed node
        return node;
    }
}
```

## **1. Store inorder indices in HashMap**

value → index

Reason:

- Avoid O(n) search every time
- Reduce time complexity

---

## **2. Maintain Preorder Pointer**

preItr`[0]`

This tracks:

> Current root index in preorder

⚠ Using array because Java passes primitives by value.

---

## **3. Recursive Construction**

Call:

solve(preorder, inorder, preItr, left, right)

Where:

| Variable | Meaning                |
| -------- | ---------------------- |
| left     | start of inorder range |
| right    | end of inorder range   |

---

## **4. Build Tree**

### Step A — Base Condition

if(left > right)  
    return null

No elements → no node.

---

### Step B — Pick Root

root = preorder`[preItr[0]]`

Increment pointer:

`preItr[0]++`

---

### Step C — Find Root in Inorder

idx = map.get(root)

---

### Step D — Divide into Subtrees

| Subtree | Range         |
| ------- | ------------- |
| Left    | left → idx-1  |
| Right   | idx+1 → right |

---

### Step E — Recursive Calls

node.left = solve(left side)  
node.right = solve(right side)


