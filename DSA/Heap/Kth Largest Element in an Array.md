Given an integer array `nums` and an integer `k`, return _the_ `kth` _largest element in the array_.

Note that it is the `kth` largest element in the sorted order, not the `kth` distinct element.

Can you solve it without sorting?

**Example 1:**

**Input:** nums = `[3,2,1,5,6,4], k = 2`
**Output:** 5

**Example 2:**

**Input:** nums = `[3,2,3,1,2,4,5,5,6], k = 4`
**Output:** 4

**Constraints:**

- `1 <= k <= nums.length <= 105`
- `-104 <= nums[i] <= 104`


---
---

# Optimal

``` java
class Solution {
    public int findKthLargest(int[] nums, int k) {

        // max heap (largest element comes first)
        PriorityQueue<Integer> pq= new PriorityQueue<>(Comparator.reverseOrder());

        // add all elements into heap
        for(int i = 0 ;i<  nums.length; i++){
            pq.add(nums[i]);
        }

        int i = 1; 

        // remove largest element k-1 times
        while(i< k ){

            // remove current largest
            pq.remove();

            i++;
        }

        // next removed element is kth largest
        return pq.remove();
    }
}
```