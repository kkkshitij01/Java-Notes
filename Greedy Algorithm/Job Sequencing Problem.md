**Problem Statement:** You are given a set of N jobs where each job comes with a **deadline** and **profit**. The profit can only be earned upon completing the job within its deadline. Find the **number of jobs** done and the **maximum profit** that can be obtained. Each job takes a **single unit** of time and only **one job** can be performed at a time.

**Examples**

**Example 1:**

**Input:** N = 4, Jobs = {(1,4,20),(2,1,10),(3,1,40),(4,1,30)}

**Output**: 2 60

**Explanation:** The 3rd job with a deadline 1 is performed during the first unit of time .The 1st job is performed during the second unit of time as its deadline is 4.
Profit = 40 + 20 = 60

**Example 2:**

**Input:** N = 5, Jobs = {(1,2,100),(2,1,19),(3,2,27),(4,1,25),(5,1,15)}

**Output:** 2 127

**Explanation:** The  first and third job both having a deadline 2 give the highest profit. 
Profit = 100 + 27 = 127

```java
import java.util.Arrays;

  

public class JobSequencing {

public static void printArray(int arr[][]) {

for (int i = 0; i < arr.length; i++) {

for (int j = 0; j < arr[i].length; j++) {

System.out.print(arr[i][j] + " ");

}

System.out.println();

}

System.out.println();

}

  

public static void jobSequencingProblem(int arr[][]) {

int maxDayDeadLine = 0;

for (int[] a : arr) {

maxDayDeadLine = Math.max(a[1], maxDayDeadLine);

}

System.out.println(maxDayDeadLine);

int day[] = new int[maxDayDeadLine + 1];

Arrays.fill(day, -1);

Arrays.sort(arr, (a, b) -> Integer.compare(b[2], a[2]));

int profit = 0;

for (int i = 0; i < arr.length; i++) {

int idx = arr[i][0];

int deadLine = arr[i][1];

int currProfit = arr[i][2];

for (int j = deadLine; j > 0; j--) {

if (day[j] == -1) {

profit += currProfit;

day[j] = idx;

break;

}

}

}

System.out.println(profit);

  

}

  

public static void main(String[] args) {

int arr[][] = {

{ 1, 2, 100 }, // Job 1

{ 2, 1, 19 }, // Job 2

{ 3, 2, 27 }, // Job 3

{ 4, 1, 25 }, // Job 4

{ 5, 3, 15 } // Job 5

};

  

jobSequencingProblem(arr);

}

}
```