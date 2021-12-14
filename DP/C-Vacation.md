
# Vacation
**Author:** Debdyut Hajra
**Created date:** 8 December 2021
**Last updated:** 8 December 2021

**Problem Link:** [C - Vacation](https://atcoder.jp/contests/dp/tasks/dp_c)
**Tags:** Array, Dynamic Programming

## Problem

John can select from a set of 3 activities - A, B, C for a given number of days. The happiness he may gain by performing each of these activities on a given day varies. He also gets bored doing the same type of activity for 2 consecutive days. Find the maximum happiness he can find.

  
**Example:**

**Input:** 
3
10 30 20
30 70 90
10 15 20
**Output:** 130
**Explanation:** Activity B on day 1 (happiness = 30) ; Activity C on day 2 (happiness = 90); Activity B on day 3 (happiness = 15).
Total amount happiness he can get = 30 + 90 + 15 = 135.

**Constraints:**

-   `1 <= arr.length <= 1e5`
-   `0 <= arr[i] <= 1e4`

## Solutions

We will look at the following solutions:
1. [Recursive]()
2. [Recursive DP]()
3. [Iterative DP]()
4. [Space optimized DP]()
5. [`arraymax` helper method]()

### 1. Recursive
The primary intuition behind the solution is that given the previous activity type picked, we cannot pick the same activity type in the immediate next row. We will pick from the remaining activities, considering all the possibilities in the process, to find the exact combination which gives the maximum happiness.

**arr:** Input array
**n:** Input size
**row:** Current row
**col:** Previously taken column

```java
private  static  long  maxHappiness(long[][] arr, int  n) {
	// Get the maximum happiness starting from first row (0)
	// col = -1 indicates no activity type chosen previously
	return  maxHappiness(arr, n, 0, -1);
}

private  static  long  maxHappiness(
	long[][] arr, 
	int  n, 
	int  row, 
	int  col) 
{
	// Base case: For any row beyond the array bounds
	// answer will be 0
	if (row == n) {
		return  0;
	}

	long  max = Long.MIN_VALUE;
	// Iterate for each activity type
	for (int  i = 0; i < 3; i++) {
		// If same activity type is already chosen in previous step
		// Skip this activity type
		if (i == col) continue;		
		
		// Compute the result of selecting this activity
		// and taking maximum possible happiness 
		// from the subsequent rows in the given scenario 
		max = Math.max(max, arr[row][i] + maxHappiness(arr, n, row+1, i));
	}
	// Return the maximum value for all possible scenrios
	// for the given scenario
	return  max;
}
```
### 2. Recursive DP
In the above recusive solution, we can save some computations by memoizing values which are already computed (dynamic programming). We may observe that 2 paramters are changing for each recusive call - the row and the col. Hence, we can introduce a new dp array of dimensions `[n+1][4]`. As a result, when we encounter the same combination of the paramters, we can reuse the pre-computed value. 

<strong>Note: </strong> In this step, we have refrained from using -1 to indicate the scenario where no activity type were selected in the previous step, as it will cause ArrayIndexOutOfBound exception. Instead, here column `0` of the dp array will play the said role. 

**arr:** Input array
**n:** Input size
**row:** Current row
**col:** Previously taken column
**dp:** Memoized array
```java
private  static  long  maxHappiness(long[][] arr, int  n) {
	// Fill the dp array with -1
    // -1 indicates value not computed for given indices
	long[][] dp = new  long[n+1][4];
	for (int  i = 0; i < dp.length; i++) {
		for (int  j = 0; j < 4; j++) {
			dp[i][j] = -1;
		}
	}
	
	// The result for the (n+1)th row is 0
	for (int  j = 0; j < 4; j++) {
		dp[n][j] = 0;
	}
	
	// Get the maximum happiness starting from first row (0)
	return  maxHappiness(arr, n, 0, 0, dp);
}

private  static  long  maxHappiness(
	long[][] arr, 
	int  n, 
	int  row, 
	int  col, 
	long[][] dp) 
{
	// If value is already computed
	// return the stored value
	if (dp[row][col] != -1) {
		return  dp[row][col];
	}
	
	long  max = Long.MIN_VALUE;
	for (int  i = 1; i <= 3; i++) {
		// If same activity type is already chosen in previous step
		// Skip this activity type
		if (i == col) continue;
		
		// Compute the result of selecting this activity
		// and taking maximum possible happiness 
		// from the subsequent rows in the given scenario 
		max = Math.max(max, 
				arr[row][i-1] + maxHappiness(arr, n, row+1, i, dp));
	}
	
	// Store(memoize) the computed value for future reuse
	return  dp[row][col] = max;
}
```
### 3. Iterative DP
The preceding recursive solution has been transformed to form an iterative solution for the given problem. If we draw the recursion tree, we may observe that we get the concrete results starting from the last row. Hence, we begin the loop from the last index.

<strong>Note:</strong> In this scenario we do not need any additional columns in the dp array to handle the scenario where no acticity type was chosen in the previous step.  

**arr:** Input array
**n:** Input size
**dp:** Memoized array

```java
private  static  long  maxHappiness(long[][] arr, int  n) {
	// Fill the dp array with -1
    // -1 indicates value not computed for given indices
	long[][] dp = new  long[n+1][3];
	for (int  i = 0; i < dp.length; i++) {
		for (int  j = 0; j < 3; j++) {
			dp[i][j] = -1;
		}
	}
	// The result for the (n+1)th row is 0
	for (int  j = 0; j < 3; j++) {
		dp[n][j] = 0;
	}
	
	// We begin looping from the last row
	for (int  i = n-1; i >= 0; i--) {
		// Compute result for each activity type
		for (int  j = 0; j < 3; j++) {
			// Current result is the value of the current array item
			// + the maximum result starting from next row
			// skipping the current activity type in the next step
			// arraymax takes an array and the index to be skipped
			dp[i][j] = arr[i][j] + arraymax(dp[i+1], j);
		}
	}
	
	// Answer is maximum element from 1st row of the dp array
	// Here -1 indicates no index of the given array to be skipped
	return  arraymax(dp[0], -1);
}
```
### 4. Space optimized DP
The preceeding dp solutions require an O(n) extra space. The below code optimizes the space complexity to O(1).
 **arr:** Input array
**n:** Input size
```java
private  static  long  maxHappiness(long[][] arr, int  n) {
	// Stores the result for the immediate next row
	// Automatically initialized to 0 for (n+1)th row 
	long[] next = new  long[3];
	for (int  i = n-1; i >= 0; i--) {
		// Stores the result for current row
		long[] curr = new  long[3];
		for (int  j = 0; j < 3; j++) {
			// Current result is the value of the current array item
			// + the maximum result starting from next row
			// skipping the current activity type in the next step
			// arraymax takes an array and the index to be skipped
			curr[j] = arr[i][j] + arraymax(next, j);
		}
		// Update the array for the next iteration
		next = curr;
	}
	
	// Answer is maximum element from 1st row of the dp array
	// Here -1 indicates no index of the given array to be skipped
	return  arraymax(next, -1);
}
```
### 5. `arraymax` helper method
Get maximum element from a given array of long values.

**arr:** Input array
**skipIdx:** Index to be skipped
```java
private  static  long  arraymax(long[] arr, int  skipIdx) {
	long  max = Long.MIN_VALUE;
	for (int  i = 0; i < arr.length; i++) {
		if (i == skipIdx) continue;
		max = Math.max(max, arr[i]);
	}
	return  max;
}
```