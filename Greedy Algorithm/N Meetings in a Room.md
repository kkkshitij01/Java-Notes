**Problem Statement:** There is **one** meeting room in a firm. You are given two arrays, start and end each of size N.For an index ‘i’, start[i] denotes the starting time of the ith meeting while end[i]  will denote the ending time of the ith meeting. Find the maximum number of meetings that can be accommodated if only one meeting can happen in the room at a  particular time. Print the order in which these meetings will be performed.

**Example:**

**Input:**  N = 6,  start[] = {1,3,0,5,8,5}, end[] =  {2,4,5,7,9,9}

**Output:** 1 2 4 5

**Explanation:** See the figure for a better understanding.


# Approach: 

- Here we want max number of meetings that can be done in single meeting room one at  a time .
- First we create a new 2d array to store {index , start time , end time}
```java
    int arr[][] = new int[start.length][3];

        for (int i = 0; i < start.length; i++) {

            arr[i][0] = i;          // index 

            arr[i][1] = start[i];   // start time 

            arr[i][2] = end[i];     // end time 

        }
```


- then we sort the meetings in increasing order based on there end time ( the one which ends first comes earlier )
 ```java
  Arrays.sort(arr, (a, b) -> Integer.compare(a[2] , b[2]));  // sorting using comparators 
  ```


- now we initialize count and prevEnd 
  ```java 
  int count = 0;
  int prevEnd = Integer.MIN_VALUE;

        for (int i = 0; i < arr.length; i++) {

            if (prevEnd < arr[i][1]) { // if end time of previous meeting ends before the start time of next meeting 
                prevEnd = arr[i][2]; // updating the prevEnd with end time of current meeting 
                count++;

            }

        }
  ```

# Code : 
```java  

    public static void solve(int start[], int end[]) {

        int arr[][] = new int[start.length][3];

        for (int i = 0; i < start.length; i++) {

            arr[i][0] = i;

            arr[i][1] = start[i];

            arr[i][2] = end[i];

        }

        Arrays.sort(arr, (a, b) -> (a[2] - b[2]));

        int count = 0;

        int prevEnd = Integer.MIN_VALUE;

        for (int i = 0; i < arr.length; i++) {

            if (prevEnd < arr[i][1]) {

                prevEnd = arr[i][2];

                count++;

            }

        }

        System.out.println(count);

    }
```