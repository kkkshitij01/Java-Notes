Given an array of `points` where `points[i] = [xi, yi]` represents a point on the **X-Y** plane and an integer `k`, return the `k` closest points to the origin `(0, 0)`.

The distance between two points on the **X-Y** plane is the Euclidean distance (i.e., `√(x1 - x2)2 + (y1 - y2)2`).

You may return the answer in **any order**. The answer is **guaranteed** to be **unique** (except for the order that it is in).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/03/closestplane1.jpg)

**Input:** points = `[[1,3],[-2,2]], k = 1`
**Output:**` [[-2,2`]]
**Explanation:**
The distance between (1, 3) and the origin is sqrt(10).
The distance between (-2, 2) and the origin is sqrt(8).
Since sqrt(8) < sqrt(10), (-2, 2) is closer to the origin.
We only want the closest k = 1 points from the origin, so the answer is just `[[-2,2]].`

**Example 2:**

**Input:** points =` [[3,3],[5,-1],[-2,4]], k = 2`
**Output:**` [[3,3],[-2,4]]`
**Explanation:** The answer` [[-2,4],[3,3]] would also be accepted.`

**Constraints:**

- `1 <= k <= points.length <= 104`
- `-104 <= xi, yi <= 104`

---
---
```java
class Solve implements Comparable<Solve>{

    // this class represents one point and its distance from origin
    // it helps priority queue decide which point is closer

    int arr[];   // stores the point (x, y)
    int dis ;    // stores distance from origin

    Solve(int arr[]){

        // save the point
        this.arr = arr;

        // calculate distance = x^2 + y^2
        // we do not use sqrt because comparison works fine without it
        dis = arr[0]* arr[0] + arr[1]*arr[1];
    }

    @Override
    public int compareTo(Solve s){

        // this method tells priority queue how to sort objects
        // smaller distance should come first (min heap)
        return this.dis - s.dis;
    }
}

class Solution {
    public int[][] kClosest(int[][] points, int k) {

     // min heap that stores Solve objects (points with distance)
     PriorityQueue<Solve> pq = new PriorityQueue<>();

     // add all points into heap
     for(int arr[] : points){

        // wrap each point inside Solve object
        // so that heap can compare based on distance
        pq.add(new Solve(arr));
     }

     // result array to store k closest points
     int ans[][] = new int[k][2];

     int i = 0; 

     // remove k smallest distance points from heap
     while(k >i ){

        // remove closest point and store its original array
        ans[i] = pq.remove().arr;

        i++;
     }

     // return k closest points
     return ans;
    }
}
```