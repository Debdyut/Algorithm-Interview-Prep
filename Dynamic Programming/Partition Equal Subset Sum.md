
# Partition Equal Subset Sum
**Author:** Debdyut Hajra
**Created date:** 14 December 2021
**Last updated:** 14 December 2021

**Problem Link:** [Partition Equal Subset Sum](https://practice.geeksforgeeks.org/problems/subset-sum-problem2014/1#)
**Tags:** Array, Dynamic Programming

## Problem

Given an array, check if it is possible to create 2 subsets of equal sum using all the elements.

**Example:**

**Input:** 
**Input:** N = 4
arr = {1, 5, 11, 5}
**Output:** YES
**Explaination:**  The two parts are {1, 5, 5} and {11}.

**Constraints:**

1 ≤ N ≤ 100  
1 ≤ arr[i] ≤ 1000

## Solutions

We will look at the following solutions:
1. [Recursive](#1-recursive)
2. [Recursive DP](#2-recursive-dp)
3. [Iterative DP](#3-iterative-dp)
4. [Space optimized DP](#4-space-optimized-dp)
5. [arraysum (helper method)](#5-arraysum-helper-method)

### 1. Recursive
The primary intuition behind the solution is that if the sum of all the array elements is odd, then it is not possible to create 2 equal partitions. If sum is even, the target is to find a subset, whose sum is equal to half of the array sum.

**arr:** Input array
**N:** Number of items
**idx:** Current index
**sum:** Remaining sum

```java
static int equalPartition(int N, int arr[])
{
	// Calculate the sum of the array
    int  sum = arraysum(N, arr);
    // If sum is odd, equal subsets not possible
    if ((sum & 1) != 0) {
        return 0;
    }
    // Find if equal partition is possible
    return equalPartition(N, arr, 0, sum / 2);
}

private static int equalPartition(int N, int arr[], int idx, int sum)
{
	// If no items remain
    if (idx == N) {
	    // If sum is 0, then equal partition is possible
        return (sum == 0) ? 1 : 0;
    }
    
    int pick = 0;
    // Check if item is less than or equal to the remaining sum
    if (sum >= arr[idx]) 
    {
	    // Check if possible to create equal partitions
	    // by considering the current item
        pick = equalPartition(N, arr, idx + 1, sum - arr[idx]);
    }
    
    // Return 1 if possible to create equal partitions
    // by picking or not-picking the current item
    return pick | equalPartition(N, arr, idx + 1, sum);
}
```
### 2. Recursive DP
In the above recusive solution, we can save some computations by memoizing values which are already computed (dynamic programming). We may observe that 2 paramters are changing for each recusive call - the remianing weight and the current index. Hence, we can introduce a new dp array of dimensions `[n+1][W]`. As a result, when we encounter the same combination of the paramters, we can reuse the pre-computed value. 

**arr:** Input array
**N:** Number of items
**idx:** Current index
**sum:** Remaining sum
**dp:** Memoized array
```java
static int equalPartition(int N, int arr[])
{
	// Calculate the sum of the array
    int  sum = arraysum(N, arr);
    // If sum is odd, equal subsets not possible
    if ((sum & 1) != 0) {
        return 0;
    }
    // Initialize dp
    int[][] dp = new int[N+1][sum / 2 + 1];
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < dp[0].length; j++) {
            dp[i][j] = -1; 
        }
    }
    // All results for (n+1)th item defaults to 0
    // And sum = 0 case is set to 1 
    dp[N][0] = 1;
    
    // Find if equal partition is possible
    return equalPartition(N, arr, 0, sum / 2, dp);
}

private static int equalPartition(
	int N, 
	int arr[], 
	int idx, 
	int sum, 
	int[][] dp)
{
	// If value is pre-computed, return that value
    if (dp[idx][sum] != -1) {
        return dp[idx][sum];
    }
    
    int pick = 0;
    // Check if item is less than or equal to the remaining sum
    if (sum >= arr[idx])
    {
	    // Check if possible to create equal partitions
	    // by considering the current item
        pick = equalPartition(N, arr, idx + 1, sum - arr[idx], dp);
    }
    // Return 1 if possible to create equal partitions
    // by picking or not-picking the current item
    return dp[idx][sum] = pick | equalPartition(N, arr, idx + 1, sum, dp);
}
```
### 3. Iterative DP
The preceding recursive solution has been transformed to form an iterative solution for the given problem. If we draw the recursion tree, we may observe that we get the concrete results starting from the last row. Hence, we begin the loop from the last index. 

**arr:** Input array
**N:** Number of items
**dp:** Memoized array

```java
static int equalPartition(int N, int arr[])
{
	// Calculate the sum of the array
    int  sum = arraysum(N, arr);
    // If sum is odd, equal subsets not possible
    if ((sum & 1) != 0) {
        return 0;
    }    
    // Calcualte half of the array sum
    int sumHalf = sum / 2;
    // Create a dp array
    int[][] dp = new int[N+1][sumHalf + 1];
    // All results for (n+1)th item defaults to 0
    // And sum = 0 case is set to 1 
    dp[N][0] = 1;
    // Begin looping from the last item
    for (int i = N-1; i >= 0; i--) {
	    // For all possible sum values
        for (int j = 0; j <= sumHalf; j++) {
	        // Check if item is less than or equal to the remaining sum
            if (j >= arr[i]) {
	   	        // Check if possible to create equal partitions
			    // by considering the current item
                dp[i][j] = dp[i+1][j-arr[i]];
            }
            // Store if possible to create equal partitions
		    // by picking or not-picking the current item
            dp[i][j] = dp[i][j] | dp[i+1][j];
        }
    }
    // Return if equal partition is possible
    return dp[0][sumHalf];
}
```
### 4. Space optimized DP
The preceeding dp solutions require an O(n) extra space. The below code optimizes the space complexity to O(1).

**arr:** Input array
**N:** Number of items
```java
static int equalPartition(int N, int arr[])
{
	// Calculate the sum of the array
    int  sum = arraysum(N, arr);
    // If sum is odd, equal subsets not possible
    if ((sum & 1) != 0) {
        return 0;
    }
    // Calcualte half of the array sum
    int sumHalf = sum / 2;
    // The result starting from next item in sequence
    int[] next = new int[sumHalf + 1];
    // All results for (n+1)th item defaults to 0
    // And sum = 0 case is set to 1
    next[0] = 1;
    // Begin looping from the last item
    for (int i = N-1; i >= 0; i--) {
        int[] curr = new int[sumHalf + 1];
        // For all possible sum values
        for (int j = 0; j <= sumHalf; j++) {
	        // Check if item is less than or equal to the remaining sum
            if (j >= arr[i]) {
	           // Check if possible to create equal partitions
			   // by considering the current item
               curr[j] = next[j-arr[i]];
            }
            // Store if possible to create equal partitions
		    // by picking or not-picking the current item
            curr[j] = curr[j] | next[j];
        }
        // Current becomes next result array for next iteration
        next = curr;
    }
    return next[sumHalf];
}
```
### 5. arraysum (helper method)
Sum of the given array of integers.
```java
static int arraysum(int N, int[] arr) 
{
    int  sum = 0;
    for (int i = 0; i < N; i++) {
        sum += arr[i];
    }
    return sum;
}
```