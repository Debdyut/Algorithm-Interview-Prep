
# Climbing Stairs
**Author:** Debdyut Hajra <br/>
**Created date:** 23 December 2021 <br/>
**Last updated:** 23 December 2021 <br/>

**Problem Link:** [70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/) <br/>
**Tags:** Math, Dynamic Programming, Memoization

## Problem

Find the number of ways to reach the top of a staircase if we can move 1 or 2 steps at a time.

**Example:**

**Input:** n = 2 <br/>
**Output:** 2 <br/>
**Explanation:** There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps

**Constraints:**

-   `1 <= n <= 45`

## Solutions

We will look at the following solutions:
1. [Recursive](#1-recursive)
2. [Recursive DP](#2-recursive-dp)
3. [Iterative DP](#3-iterative-dp)
4. [Space optimized DP](#4-space-optimized-dp)

### 1. Recursive
The primary intuition behind the solution is that from a given step, we will move to the next step and another step beyond the latter. For both the case we will count the number of ways we can reach the top of the staircase. 

**n:** Number of steps <br/>
**idx:** Current step <br/>

```java
public int climbStairs(int n) {
    // Return number of ways to reach the top of staircase
    return climbStairs(n, 0);
}

public int climbStairs(int n, int idx) {
    // If we have reached the top
    if (idx == n) {        
        return 1;
    }
    // If we have skipped the top step
    if (idx > n) {            
        return 0;
    }
    
    // Count number of ways if we move 1 step
    int count = climbStairs(n, idx+1);
    // Add number of ways if we move 2 steps
    count += climbStairs(n, idx+2);

    // Return the total number of possibilities
    return count;
}
```
### 2. Recursive DP
In the above recusive solution, we can save some computations by memoizing values which are already computed (dynamic programming). We may observe that only 1 paramter is changing for each recusive call - the current index. Hence, we may introduce a new dp array of dimensions `[n]`. As a result, when we encounter the same combination of the paramters, we can reuse the pre-computed value. 

**n:** Number of steps <br/>
**idx:** Current step <br/>
**dp:** Memoized array <br/>
```java
public int climbStairs(int n) {
    // Initialize dp
    int[] dp = new int[n]; 
    for (int i = 0; i < n; i++) {
        dp[i] = -1;
    }
    // Return number of ways to reach the top of staircase
    return climbStairs(n, 0, dp);
}

public int climbStairs(int n, int idx, int[] dp) {
    // If we have reached the top
    if (idx == n) {
        return 1;
    }
    // If we have skipped the top step
    if (idx > n) {            
        return 0;
    }
    
    // If value is pre-computed, return the value
    if (dp[idx] != -1) {
        return dp[idx];
    }
    
    // Count number of ways if we move 1 step
    int count = climbStairs(n, idx+1, dp);
    // Add number of ways if we move 2 steps
    count += climbStairs(n, idx+2, dp);
    
    // Return the total number of possibilities
    // Store the value for future re-use
    return dp[idx] = count;
}
```
### 3. Iterative DP
The preceding recursive solution has been transformed to form an iterative solution for the given problem. If we draw the recursion tree, we may observe that we get the concrete results starting from the last row. Hence, we begin the loop from the last index. 

**n:** Number of steps <br/>
**dp:** Memoized array <br/>

```java
public int climbStairs(int n) {
    // Initialize dp
    int[] dp = new int[n+2];
    // Number of ways if we start from top step is 1
    dp[n] = 1;
    // Begin looping from the end
    for (int i = n-1; i >= 0; i--) {
        // Count number of ways if we move 1/2 step
        dp[i] = dp[i+1] + dp[i+2];
    }
    // Return the total number of possibile ways
    return dp[0];
}
```
### 4. Space optimized DP
The preceeding dp solutions require an `O(n)` extra space. The below code optimizes the space complexity to `O(1)`.

**n:** Number of steps <br/>
```java
public int climbStairs(int n) {
    // Top step        
    int a = 1;
    // Beyong the top step
    int b = 0;
    // Begin looping from the end
    for (int i = n-1; i >= 0; i--) {
        // Count number of ways if we move 1/2 step
        int c = a + b;
        // Update the values for the next iteration
        b = a;
        a = c;
    }
    // Return the total number of possibile ways
    return a;
}
```