Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

**Clarification:** The input/output format is the same,You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/15/serdeser.jpg)

**Input:** root =` [1,2,3,null,null,4,5]`
**Output:** `[1,2,3,null,null,4,5]`

**Example 2:**

**Input:** root = `[]`
**Output:**` []
`
**Constraints:**

- The number of nodes in the tree is in the range `[0, 104]`.
- `-1000 <= Node.val <= 1000`


---
---

# Optimal : 
```java
public class Codec {

    // convert tree into string
    public String serialize(TreeNode root) {

        // if tree is empty, return empty string
        if(root == null) return "";

        // string builder to store result
        StringBuilder sb = new StringBuilder("");

        // queue for level order traversal
        Queue<TreeNode> q = new LinkedList<>();

        // start from root
        q.offer(root);

        // process all nodes level by level
        while(!q.isEmpty()){

            TreeNode temp = q.poll();

            // if node is null, store marker
            if(temp == null){
                sb.append("* ");
                continue;
            }

            // store node value
            sb.append(temp.val).append(" ");

            // add left and right child (even if null)
            q.offer(temp.left);
            q.offer(temp.right);
        }

        // return final string
        return sb.toString();
    }

    // convert string back to tree
    public TreeNode deserialize(String data) {

        // if string is empty, no tree
        if(data.length()== 0 ) return null;

        // queue to rebuild tree level by level
        Queue<TreeNode> q = new LinkedList<>();

        // split string into values
        String []values = data.split(" ");

        // first value is root
        TreeNode root = new TreeNode(Integer.parseInt(values[0]));

        // add root to queue
        q.offer(root);

        // iterate over remaining values
        for(int i = 1 ; i< values.length; i++){

            // get parent node
            TreeNode parent = q.poll();

            // process left child
            if(!values[i].equals("*")){
                TreeNode left = new TreeNode(Integer.parseInt(values[i]));
                parent.left = left;

                // add left child to queue
                q.offer(parent.left);
            }

            // move to next value for right child
            i++;

            // process right child
            if(!values[i].equals("*")){
                TreeNode right = new TreeNode(Integer.parseInt(values[i]));
                parent.right = right;

                // add right child to queue
                q.offer(parent.right);
            }

        }

        // return rebuilt tree
        return root;
    }
}
```

## Approach

## **1. Use Level Order Traversal (BFS)**

We process the tree **level by level**.

Why BFS?

- It preserves structure naturally
- Easy to reconstruct using queue

---

# Serialization (Tree → String)

## **1. Handle Empty Tree**

if(root == null)  
    return ""

---

## **2. Initialize**

- `StringBuilder` → store result
- `Queue` → for BFS

q.offer(root)

---

## **3. BFS Traversal**

While queue is not empty:

### **A. Remove node**

temp = q.poll()

---

### **B. If node is null**

append "*"  
continue

Why?

To preserve structure.

---

### **C. If node exists**

append temp.val

Add children:

q.offer(temp.left)  
q.offer(temp.right)

---

## **4. Final String**

Output looks like:

1 2 3 * * 4 5 * * * *

---

# Deserialization (String → Tree)

## **1. Handle Empty Input**

if(data.length() == 0)  
    return null

---

## **2. Convert String to Array**

values = data.split(" ")

---

## **3. Create Root**

root = new TreeNode(values[0])

Add to queue:

q.offer(root)

---

## **4. Rebuild Tree Using BFS**

Loop through array:

### **A. Get parent node**

parent = q.poll()

---

### **B. Assign Left Child**

if(values[i] != "*")  
    create left node  
    attach to parent  
    add to queue

---

### **C. Move to Right Child**

i++

---

### **D. Assign Right Child**

if(values[i] != "*")  
    create right node  
    attach to parent  
    add to queue

---

## **5. Continue Until Array Ends**

Tree gets rebuilt level by level.