Problem Statement:** You are given a set of N jobs where each job comes with a **deadline** and **profit**. The profit can only be earned upon completing the job within its deadline. Find the **number of jobs** done and the **maximum profit** that can be obtained. Each job takes a **single unit** of time and only **one job** can be performed at a time.

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

# Aproach : - 
- since we want to max the profit , we sort the Jobs array  based on max profit ( descending order ) using comparators
- we create a new **Days Array** to store on what day which job will be done **( maxDeadLine +1 size)**
- now for each job (first for loop ) , we try to find a valid day,  starting from the Deadline day we check if we have a day without any job **(i.e day[i] == -1)**
- if Yes we add that job in count and **store job index at day[i] index** 
- else we move to the next job

****

```java
  

    public static void jobSequencingProblem(int jobs[][]) {
    
	    // Step 1: find the Max DeadLine Day 
        int maxDayDeadLine = 0;
        for (int[] a : jobs) {
            maxDayDeadLine = Math.max(a[1], maxDayDeadLine);
        }
        
        
		// Step 2: create a new Days array initialize to -1 
		// this array will store index of job that on what day which job is being done 
        int day[] = new int[maxDayDeadLine + 1];
        Arrays.fill(day, -1);
        
        
        
        // Step 3: Sort the Jobs array in decreasing Order i.e job with max profit at top 
        Arrays.sort(jobs, (a, b) -> Integer.compare(b[2], a[2]));
     
        int maxProfit = 0;
        
        
		// step 4 : iterating through the sorted array 
        for (int i = 0; i < jobs.length; i++) {
            int idx = jobs[i][0];
            int deadLine = jobs[i][1];
            int profit = jobs[i][2];
            
            
		// step 5: starting from deadline checking if we have a day without job through another for loop 
		// also j>0 here because we won't have a 0th day ,so we can't use that index 
            for (int j = deadLine; j > 0; j--) {

                if (day[j] == -1) { // found a valid day to do the job

                    maxProfit += profit; // adding the profit to MaxProfit

                    day[j] = idx; // Updating the job index to Days array 

                    break;

                }

            }

        }

        System.out.println(maxProfit);

  

    }
