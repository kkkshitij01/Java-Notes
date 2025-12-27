Given two sorted arrays **a`[]`** and **b`[]`** of size **n** and **m** respectively, the task is to merge them in sorted order without using any **extra space**. Modify **a`[]`** so that it contains the first **n** elements and modify **b`[]`** so that it contains the last **m** elements.

**Examples:**

**Input**: a`[] = [2, 4, 7, 10], b[] = [2, 3]`
**Output**:` a[] = [2, 2, 3, 4], b[] = [7, 10]`
**Explanation**: After merging the two non-decreasing arrays, we get, `[2, 2, 3, 4, 7, 10]`

**Input**: a`[] = [1, 5, 9, 10, 15, 20], b[] = [2, 3, 8, 13]`
**Output**: a`[] = [1, 2, 3, 5, 8, 9], b[] = [10, 13, 15, 20]`
**Explanation**: After merging two sorted arrays we get `[1, 2, 3, 5, 8, 9, 10, 13, 15, 20].`

**Input**: a`[] = [0, 1], b[] = [2, 3]`
**Output**: a`[] = [0, 1], b[] = [2, 3]`
**Explanation**: After merging two sorted arrays` we get [0, 1, 2, 3].`

---
---
# Better Approach 

### **Step 1: Initialize pointers**

• `first` → points to the **last element of array `a`**  
• `second` → points to the **first element of array `b`**

These positions hold the elements most likely to be misplaced.

---

### **Step 2: Compare elements**

• Compare `a[first]` with `b[second]`

---

### **Step 3: Swap if needed**

• If `a[first] > b[second]`  
→ The element in `a` is too big  
→ The element in `b` is too small  
→ Swap them

This moves large values to `b` and small values to `a`.

---

### **Step 4: Move pointers**

• Decrease `first` to check the next largest element in `a`  
• Increase `second` to check the next smallest element in `b`

---

### **Step 5: Stop condition**

• If `a[first] <= b[second]`  
→ Both arrays are already in correct relative order  
→ Stop swapping

---

### **Step 6: Sort both arrays**

• Sort array `a` to arrange all smaller elements correctly  
• Sort array `b` to arrange all larger elements correctly

This final sorting fixes any disorder caused by swapping.

---

## **Time Complexity**

`O(n log n + m log m)`

Due to sorting both arrays.

---

## **Space Complexity**

`O(1)`

No extra array is used.
```java
class Solution {
    public void mergeArrays(int a[], int b[]) {

        // first points to the last element of array a
        int first = a.length - 1;

        // second points to the first element of array b
        int second = 0;

        // We compare the largest element of a with the smallest element of b
        while(first >= 0 && second < b.length){

            // If the element in a is bigger than the element in b,
            // they are in the wrong array, so we swap them
            if(a[first] > b[second]){
                int temp = a[first];
                a[first] = b[second];
                b[second] = temp;
            }
            // If a[first] is not greater, arrays are already in correct order
            // so no more swaps are needed
            else{
                break;
            }

            // Move to the next largest element in a
            first--;

            // Move to the next smallest element in b
            second++;
        }

        // Sort array a so all smaller elements come in correct order
        Arrays.sort(a);

        // Sort array b so all larger elements come in correct order
        Arrays.sort(b);
    }
}

```


---
---
# Optimal 