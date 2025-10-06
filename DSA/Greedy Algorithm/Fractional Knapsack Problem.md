**Problem Statement:** The weight of **N** items and their corresponding values are given. We have to put these items in a knapsack of weight **W** such that the **total value** obtained is **maximized.**

**Note:** We can either take the item as a whole or break it into smaller units.

**Example**:

**Input:** N = 3, W = 50, values[] = {100,60,120}, weight[] = {20,10,30}.

**Output:** 240.00

**Explanation:** The first and second items  are taken as a whole  while only 20 units of the third item is taken. Total value = 100 + 60 + 80 = 240.00

# Approach:  
- We have two arrays:
	`val[]` → stores the values of items.
	`wt[]` → stores the weights of items.  
    And one variable `capacity` → the total weight limit of the knapsack.
- For each item, we calculate a new value:  
    `ratio = val[i] / wt[i]`  
    This tells us how much value we get per unit of weight.
    
-  We store all items in a 2D array of **double** type`arr[][]` as `{value, weight, (double) ratio}`.
    
-  We **sort** this array in **descending order** based on `ratio`.  
    This means the item with the highest value-to-weight ratio comes first.
    
-  Then we start a loop through all items:
    
    - If the current item’s weight is **less than or equal to** the remaining capacity →  
        take the whole item (add full value and reduce capacity).
        
    - Otherwise →  
        take only the fraction that fits (add ratio × remaining capacity, and set capacity = 0).
        
- When the capacity becomes 0, we stop.  
    The final value stored in `profit` is the **maximum total value** that can fit in the knapsack.

 ```java
  public double fractionalKnapsack(int[] val, int[] wt, int capacity) {

        double arr[][] = new double[val.length][3];

        for(int i = 0; i< val.length; i++){

            arr[i][0] = val[i];

            arr[i][1] = wt[i];

            arr[i][2] = (double)val[i]/wt[i]; // converting the (val/ wt ) result in double type 

        }

        Arrays.sort(arr , (a,b)->Double.compare(b[2],a[2])); // sorting in descending order

        double profit = 0; 

        for(int i =0 ;i < arr.length; i++){

            if(capacity>= arr[i][1]){

                capacity -= arr[i][1];

                profit += arr[i][0];

            }else{

                profit+= arr[i][2] * capacity;

                capacity = 0; 

            }

        }

        return profit;

    }
 ```