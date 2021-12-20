# Partition Array for Maximum Sum
**Author:** Debdyut Hajra <br/>
**Created date:** 18 December 2021 <br/>
**Last updated:** 18 December 2021 <br/>

**Problem Link:** [1043. Partition Array for Maximum Sum](https://leetcode.com/problems/partition-array-for-maximum-sum/) <br/>
**Tags:** Array, Dynamic Programming

## Problem

Given an array of itegers, find the maximum possible sum that can be obtained by partitioning the array into sub-arrays of size less than or equal to a given permitted length. Each element of a partition is transformed to the maximum element of the corresponding partition.

**Example:**

**Input:** arr = [1,15,7,9,2,5,10], k = 3 <br/>
**Output:** 84 <br/>
**Explanation:** arr becomes [15,15,15,9,10,10,10] <br/>

**Constraints:**

-   `1 <= arr.length <= 500`
-   `0 <= arr[i] <= 109`
-   `1 <= k <= arr.length`

## Solutions

We will look at the following solutions:
1. [Recursive](#1-recursive)
2. [Recursive DP](#2-recursive-dp)
3. [Iterative DP](#3-iterative-dp)

### 1. Recursive
The primary intuition behind the solution is that given a particular index we will try to consider all sub-arrays we can create of the given size. And compute the maximum possible sum in the corresponding scenario. 

**arr** Input array <br/>
**n:** Length of array <br/>
**k:** Maximum permitted length of sub-array <br/>
**idx:** Current index <br/>

```java
public int maxSumAfterPartitioning(int[] arr, int k) {
    int n = arr.length;
    // Get the maximum possible sum starting from front
    return maxSumAfterPartitioning(arr, n, k, 0);
}

int maxSumAfterPartitioning(int[] arr, int n, int k, int idx) {
    // If reached the end of input array, return 0       
    if (idx == n) {
        return 0;
    }
    int max = Integer.MIN_VALUE;
    int maxSum = Integer.MIN_VALUE;
    // Consider all possible subarrays starting from current index
    // whose size is less than or equal to k
    for (int i = 0; (i < k) && ((idx + i) < n); i++) {
        // Track the maximum element in the sub-array
        max = Math.max(max, arr[idx+i]);
        // Compute the maximum sum considering sub-array - arr[idx, idx+i]
        int tmpSum = max * (i+1) + maxSumAfterPartitioning(arr, n, k, idx+i+1);
        // Track the maximum possible starting from idx
        maxSum = Math.max(maxSum, tmpSum);
    }
    // Return the maximum possible sum
    return maxSum;
}
```
### 2. Recursive DP
In the above recusive solution, we can save some computations by memoizing values which are already computed (dynamic programming). We may observe that 1 paramter is changing for each recusive call - the current index. Hence, we may introduce a new dp array of dimensions `[n]`. As a result, when we encounter the same combination of the paramters, we can reuse the pre-computed value. 

**arr** Input array <br/>
**n:** Length of array <br/>
**k:** Maximum permitted length of sub-array <br/>
**idx:** Current index <br/>
**dp:** Memoized array <br/>
```java
public int maxSumAfterPartitioning(int[] arr, int k) {    
    int n = arr.length;
    // Initialize the dp
    int[] dp = new int[n];
    for (int i = 0; i < n; i++) {
        dp[i] = -1; 
    }
    // Get the maximum possible sum starting from front
    return maxSumAfterPartitioning(arr, n, k, 0, dp);
}

int maxSumAfterPartitioning(int[] arr, int n, int k, int idx, int[] dp) {        
    // If reached the end of input array, return 0
    if (idx == n) {
        return 0;
    }
    
    // If value is pre-computed, return the value
    if (dp[idx] != -1) {
        return dp[idx];
    }
    
    int max = Integer.MIN_VALUE;
    int maxSum = Integer.MIN_VALUE;
    // Consider all possible subarrays starting from current index
    // whose size is less than or equal to k
    for (int i = 0; (i < k) && ((idx + i) < n); i++) {
        // Track the maximum element in the sub-array
        max = Math.max(max, arr[idx+i]);
        // Compute the maximum sum considering sub-array - arr[idx, idx+i]
        int tmpSum = max * (i+1) + maxSumAfterPartitioning(arr, n, k, idx+i+1, dp);
        // Track the maximum possible starting from idx
        maxSum = Math.max(maxSum, tmpSum);
    }
    // Return the maximum possible sum
    // Store the value for future re-use
    return dp[idx] = maxSum;
}
```
### 3. Iterative DP
The preceding recursive solution has been transformed to form an iterative solution for the given problem. If we draw the recursion tree, we may observe that we get the concrete results starting from the last row. Hence, we begin the loop from the last index. 

**arr** Input array <br/>
**n:** Length of array <br/>
**k:** Maximum permitted length of sub-array <br/>
**dp:** Memoized array <br/>

```java
public int maxSumAfterPartitioning(int[] arr, int k) {    
    int n = arr.length;
    // Create the dp
    int[] dp = new int[n+1];
    // Begin looping from the end        
    for (int i = n-1; i >= 0; i--) {
        int max = Integer.MIN_VALUE;
        // Consider all possible subarrays starting from current index
        // whose size is less than or equal to k
        for (int j = 0; j < k && (i+j < n); j++) {
            // Track the maximum element in the sub-array
            max = Math.max(max, arr[i+j]);
            // Compute the maximum sum considering sub-array - arr[i, i+j]
            int tmp = max * (j+1) + dp[i+j+1];
            // Track the maximum possible starting from index i
            dp[i] = Math.max(dp[i], tmp);
        }
    }
    // Get the maximum possible sum starting from front
    return dp[0];
}
```