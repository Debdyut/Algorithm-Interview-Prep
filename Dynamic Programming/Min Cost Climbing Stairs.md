
# Min Cost Climbing Stairs
**Author:** Debdyut Hajra <br/>
**Created date:** 23 December 2021 <br/>
**Last updated:** 23 December 2021 <br/>

**Problem Link:** [746. Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/) <br/>
**Tags:** Array, Dynamic Programming

## Problem

Given the cost at each step of a staircase, find the minimum cost to reach the top of the staircase starting from step 0 or step 1.

**Example:**

**Input:** cost = [10,15,20] <br/>
**Output:** 15 <br/>
**Explanation:** You will start at index 1.
- Pay 15 and climb two steps to reach the top.
The total cost is 15.

**Constraints:**

-   `2 <= cost.length <= 1000`
-   `0 <= cost[i] <= 999`

## Solutions

We will look at the following solutions:
1. [Recursive](#1-recursive)
2. [Recursive DP](#2-recursive-dp)
3. [Iterative DP](#3-iterative-dp)
4. [Space optimized DP](#4-space-optimized-dp)

### 1. Recursive
The primary intuition behind the solution is that from a given step, we will check which act gives the minimum cost - moving 1 step or 2 steps. 

**cost:** Input array <br/>
**n:** Number of steps <br/>
**idx:** Current step <br/>

```java
public int minCostClimbingStairs(int[] cost) {
    int n = cost.length;

    // Return the minimum cost to reach the top
    // starting from step 0/1
    return Math.min( minCostClimbingStairs(cost, n, 0), minCostClimbingStairs(cost, n, 1) );
}

public int minCostClimbingStairs(int[] cost, int n, int idx) {
    // If we have reached the top
    if (idx >= n) {
        return 0;
    }

    // Minimum cost to reach the top, moving 1 step from the current step
    int minCost = cost[idx] + minCostClimbingStairs(cost, n, idx+1);
    // Check if cost to reach the top is minimum, 
    // if we move 2 steps from the current step        
    minCost = Math.min(minCost, cost[idx] + minCostClimbingStairs(cost, n, idx+2));    
    
    // Return the minimum cost from current step
    return minCost;
}
```
### 2. Recursive DP
In the above recusive solution, we can save some computations by memoizing values which are already computed (dynamic programming). We may observe that only 1 paramter is changing for each recusive call - the current index. Hence, we may introduce a new dp array of dimensions `[n]`. As a result, when we encounter the same combination of the paramters, we can reuse the pre-computed value. 

**cost:** Input array <br/>
**n:** Number of steps <br/>
**idx:** Current step <br/>
**dp:** Memoized array <br/>
```java
public int minCostClimbingStairs(int[] cost) {
    // Initialize dp
    int n = cost.length;
    int[] dp = new int[n];
    for (int i = 0; i < n; i++) {
        dp[i] = -1;
    }

    // Return the minimum cost to reach the top
    // starting from step 0/1
    return Math.min( minCostClimbingStairs(cost, n, 0, dp),
                        minCostClimbingStairs(cost, n, 1, dp) );
}

public int minCostClimbingStairs(int[] cost, int n, int idx, int[] dp) {
    // If we have reached the top
    if (idx >= n) {
        return 0;
    }
    
    // If value is pre-computed, return the value
    if (dp[idx] != -1) {
        return dp[idx];
    }

    // Minimum cost to reach the top, moving 1 step from the current step
    int minCost = cost[idx] + minCostClimbingStairs(cost, n, idx+1, dp);        
    // Check if cost to reach the top is minimum, 
    // if we move 2 steps from the current step   
    minCost = Math.min(minCost, cost[idx] + minCostClimbingStairs(cost, n, idx+2, dp));    
    
    // Return the minimum cost from current step
    // Store the value for future re-use
    return dp[idx] = minCost;
}
```
### 3. Iterative DP
The preceding recursive solution has been transformed to form an iterative solution for the given problem. If we draw the recursion tree, we may observe that we get the concrete results starting from the last row. Hence, we begin the loop from the last index. 

**cost:** Input array <br/>
**n:** Number of steps <br/>
**dp:** Memoized array <br/>

```java
public int minCostClimbingStairs(int[] cost) {
    // Create the dp
    int n = cost.length;
    int[] dp = new int[n+2];
    // Begin looping from the end
    for (int i = n-1; i >= 0; i--) {
        // Minimum cost to reach the top, 
        // moving 1/2 step(s) from the current step
        dp[i] = cost[i] + Math.min(dp[i+1], dp[i+2]);
    }
    // Return the minimum cost to reach the top
    // starting from step 0/1
    return Math.min(dp[0], dp[1]);
}
```
### 4. Space optimized DP
The preceeding dp solutions require an `O(n)` extra space. The below code optimizes the space complexity to `O(1)`.

**cost:** Input array <br/>
**n:** Number of steps <br/>
```java
public int minCostClimbingStairs(int[] cost) {
    int n = cost.length;
    int a = 0; // (n)th index
    int b = 0; // (n+1)th index
    // Begin looping from the end
    for (int i = n-1; i >= 0; i--) {
        // Minimum cost to reach the top, 
        // moving 1/2 step(s) from the current step
        int c = cost[i] + Math.min(a, b);
        b = a;
        a = c;
    }
    // Return the minimum cost to reach the top
    // starting from step 0/1
    return Math.min(a, b);
}
```