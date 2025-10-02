**Problem Statement:** We are given two arrays that represent the arrival and departure times of trains that stop at the platform. We need to find the minimum number of platforms needed at the railway station so that no train has to wait.

**Examples 1:**

**Input:** N=6, 
arr[] = {9:00, 9:45, 9:55, 11:00, 15:00, 18:00} 
dep[] = {9:20, 12:00, 11:30, 11:50, 19:00, 20:00}

**Output:**3

**Explanation:** There are at-most three trains at a time. The train at 11:00 arrived but the trains which had arrived at 9:45 and 9:55 have still not departed. So, we need at least three platforms here.
  


# Approach : 
- Since we want the minimum number of platform , we will sort both the array ( arrival and departure) without considering the order (since arrival and departure time will get misplaced of a train while sorting)
- then we create a 
	MAX:             to store the max number of platform used 
    PLATFORM:  number of platform being utilized at index[i] 
    IDX :               to keep track of the train departures 
- Now we start a for-loop to iterate through the sorted arrival time array. Inside the loop, we:
- For every train in the **sorted arrival array**:
	- - **Check departures**: While the train at `dep[idx]` has already departed (i.e., `dep[idx] < arr[i]`), it means a platform is freed:
    - Decrease the **PLATFORM** count by 1 (`platform--`)
    - Move to the next departure (`idx++`)
-  then 
- **Add current train**: Increase the **PLATFORM** count by 1 (`platform++`) because the current train has arrived and needs a platform.
    
- **Update maximum**: Update **MAX** to store the maximum number of platforms needed so far (`max = Math.max(max, platform)`)
```java

  public int minPlatform(int arr[], int dep[]) {

        Arrays.sort(arr);

        Arrays.sort(dep);

        int max= 0 ;

        int platform = 0; 

        int idx = 0; 

        for(int i = 0 ; i< arr.length; i++){

            while( dep[idx] < arr[i]){

                platform--;

                idx++;

            }

            platform++;

            max = Math.max(max, platform);

        }

        return max;

    }
```

# Complexity

- **Time complexity:** O(nlogn) 
- (Sorting takes O(nlogn) and traversal of arrays takes O(n) so overall time complexity is O(nlogn)).

- **Space complexity:** O(1)