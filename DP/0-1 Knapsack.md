
# 0-1 Knapsack
**Author:** Debdyut Hajra
**Created date:** 10 December 2021
**Last updated:** 10 December 2021

**Problem Link:** [0-1 Knapsack](https://practice.geeksforgeeks.org/problems/0-1-knapsack-problem0945/1#)
**Tags:** Array, Dynamic Programming

## Problem

Given a knapsack of weight `W` and `N` items with their corresponding values and weights provided in 2 arrays `val[0...N-1]` and `wt[0...N-1]`. Find the maximum sum of values such that the sum of the weights of the corresponding items is smaller than or equal to `W`.

**Example:**

**Input:** 
N = 3
W = 4
values[] = {1,2,3}
weight[] = {4,5,1}
**Output:** 3

**Constraints:**

1 ≤ N ≤ 1000  
1 ≤ W ≤ 1000  
1 ≤ wt[i] ≤ 1000  
1 ≤ v[i] ≤ 1000

## Solutions

We will look at the following solutions:
1. [Recursive]()
2. [Recursive DP]()
3. [Iterative DP]()
4. [Space optimized DP]()
5. [`arraymax` helper method]()

### 1. Recursive
The primary intuition behind the solution is that given some items have been selected and given the remaining weight, whether or not we can pick the current item. Also, how the result will be impacted if we choose to not pick the current item, such that the remaining weight remains the same for the next item. We then try to find the combination for which we get the maximum sum of values.

**W:** Knapsack capacity
**n:** Number of items
**wt:** Weights associated with the N items
**val:** Values associated with the N items
**idx:** Current index

```java
static int knapSack(int W, int wt[], int val[], int n) 
{
	// Return the maximum possible sum of values
	// starting from first index (0)
    return knapSack(W, wt, val, n, 0);
}

static int knapSack(int W, int wt[], int val[], int n, int idx) 
{ 
	// Base case: No items remaining
    if (idx == n) {
        return 0;
    }
    
    int res = Integer.MIN_VALUE;
    // Check if possible to add the item
    if (W >= wt[idx]) {
	    // If possible, take the value of the item
	    // and add the maximum sum of possible values
	    // from the next items in sequence
        res = val[idx] + knapSack(W-wt[idx], wt, val, n, idx+1); 
    }
    
    // Return the max result of picking the item (if possible)
    // and the result of not picking the item
    return Math.max(res, knapSack(W, wt, val, n, idx+1));
}
```
### 2. Recursive DP
In the above recusive solution, we can save some computations by memoizing values which are already computed (dynamic programming). We may observe that 2 paramters are changing for each recusive call - the remianing weight and the current index. Hence, we can introduce a new dp array of dimensions `[n+1][W]`. As a result, when we encounter the same combination of the paramters, we can reuse the pre-computed value. 

**W:** Knapsack capacity
**n:** Number of items
**wt:** Weights associated with the N items
**val:** Values associated with the N items
**idx:** Current index
**dp:** Memoized array
```java
static int knapSack(int W, int wt[], int val[], int n) 
{ 
	// Initialize all results in dp to -1
	// All results beyond n items, default to 0
    int[][] dp = new int[n+1][1001];
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < 1001; j++) {
            dp[i][j] = -1;
        }
    }
    
    // Return the maximum possible sum of values
	// starting from first index (0)
    return knapSack(W, wt, val, n, 0, dp);
}

static int knapSack(int W, int wt[], int val[], int n, int idx, int[][] dp) 
{
	// If result is pre-computed, return the corresponding result
    if (dp[idx][W] != -1) {
        return dp[idx][W];
    }
    
    int res = Integer.MIN_VALUE;
    // Check if possible to add the item
    if (W >= wt[idx]) {
	    // If possible, take the value of the item
	    // and add the maximum sum of possible values
	    // from the next items in sequence
        res = val[idx] + knapSack(W-wt[idx], wt, val, n, idx+1, dp); 
    }
    
    // Return the max result of picking the item (if possible)
    // and the result of not picking the item
    // Store the result for reuse in subsequent steps
    return dp[idx][W] = Math.max(res, knapSack(W, wt, val, n, idx+1, dp));
}
```
### 3. Iterative DP
The preceding recursive solution has been transformed to form an iterative solution for the given problem. If we draw the recursion tree, we may observe that we get the concrete results starting from the last row. Hence, we begin the loop from the last index. 

**W:** Knapsack capacity
**n:** Number of items
**wt:** Weights associated with the N items
**val:** Values associated with the N items
**dp:** Memoized array

```java
static int knapSack(int W, int wt[], int val[], int n) 
{ 
	// All results beyond n items, default to 0
    int[][] dp = new int[n+1][1001];
    // Begin looping from last item
    for (int i = n-1; i >= 0; i--) {
	    // For all possible weight values
        for (int j = 0; j <= W; j++) {
	        // Check if possible to add the item
            if (j >= wt[i]) {
	            // If possible, take the value of the item
			    // and add the maximum sum of possible values
			    // from the next items in sequence
                dp[i][j] = val[i] + dp[i+1][j - wt[i]];
            }
            
			// Store the max result of picking the item (if possible)
			// and the result of not picking the item
			// Store the result for reuse in subsequent steps
            dp[i][j] = Math.max(dp[i][j], dp[i+1][j]);
        }
    }
    // Return the maximum possible sum of values
	// starting from first index (0)
    return dp[0][W];
}
```
### 4. Space optimized DP
The preceeding dp solutions require an O(n) extra space. The below code optimizes the space complexity to O(1).

**W:** Knapsack capacity
**n:** Number of items
**wt:** Weights associated with the N items
**val:** Values associated with the N items
**n:** Input size
```java
static int knapSack(int W, int wt[], int val[], int n) 
{
	// Stores the results for all possible weight values
	// starting from the next item in sequence  
    int[] next = new int[1001];
    // Begin looping from last item
    for (int i = n-1; i >= 0; i--) {
        int[] curr = new int[1001];
        // For all possible weight values
        for (int j = 0; j <= W; j++) {
	        // Check if possible to add the item
            if (j >= wt[i]) {
	            // If possible, take the value of the item
			    // and add the maximum sum of possible values
			    // from the next items in sequence
                curr[j] = val[i] + next[j - wt[i]];
            }
            // Store the max result of picking the item (if possible)
			// and the result of not picking the item
			// Store the result for reuse in subsequent steps
            curr[j] = Math.max(curr[j], next[j]);
        }
        // For next iteration, curr becomes the next
        next = curr;
    }
    // Return the maximum possible sum of values
    return next[W];
}
```