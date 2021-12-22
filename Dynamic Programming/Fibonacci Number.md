
# Fibonacci Number
**Author:** Debdyut Hajra <br/>
**Created date:** 17 December 2021 <br/>
**Last updated:** 17 December 2021 <br/>

**Problem Link:** [509. Fibonacci Number](https://leetcode.com/problems/fibonacci-number/) <br/>
**Tags:** Math, Dynamic Programming, Recursion, Memoization

## Problem

Find n<sup>th</sup> fibonacci number, given:
- `f(0) = 0`
- `f(1) = 1`
- `f(n) = f(n-1) + f(n-2)`

**Example:**

**Input:** n = 2 <br/>
**Output:** 1 <br/>
**Explanation:** F(2) = F(1) + F(0) = 1 + 0 = 1. <br/>

**Constraints:**

- `0 <= n <= 30`

## Solutions

We will look at the following solutions:
1. [Recursive](#1-recursive)
2. [Recursive DP](#2-recursive-dp)
3. [Iterative DP](#3-iterative-dp)
4. [Space optimized DP](#4-space-optimized-dp)

### 1. Recursive
`Self-explanatory`

**n:** Input number <br/>

```java
public int fib(int n) {
    // Base case: f(0) = 0
    if (n == 0) {
        return 0;
    }    
    // Base case: f(1) = 1
    if (n == 1) {
        return 1;
    }
    // Standard formula of fibonacci number
    return fib(n-1) + fib(n-2);
}
```
### 2. Recursive DP
In the above recusive solution, we can save some computations by memoizing values which are already computed (dynamic programming). We may observe that only 1 paramter is changing for each recusive call - the current number. Hence, we may introduce a new dp array of dimensions `[n]`. As a result, when we encounter the same combination of the paramters, we can reuse the pre-computed value. 

**n:** Input number <br/>
**dp:** Memoized array <br/>
```java
public int fib(int n) {
    // Initialize dp array
    int[] dp = new int[n+1];
    for (int i = 0; i <= n; i++) {
        dp[i] = -1;
    }
    // Return (n)th fibonnaci number
    return fibRec(n, dp);
}

public int fibRec(int n, int[] dp) {
    // Base case: f(0) = 0
    if (n == 0) {
        return 0;
    }    
    // Base case: f(1) = 1
    if (n == 1) {
        return 1;
    }
    
    // If value is pre-computed, return the value
    if (dp[n] != -1) {
        return dp[n];
    }
    // Standard formula of fibonacci number
    // Store the value for future reuse 
    return dp[n] = fibRec(n-1, dp) + fibRec(n-2, dp);
}
```
### 3. Iterative DP
The preceding recursive solution has been transformed to form an iterative solution for the given problem. If we draw the recursion tree, we may observe that we get the concrete results starting from the last row. Hence, we begin the loop from the last index. 

**n:** Input number <br/>
**dp:** Memoized array <br/>

```java
public int fib(int n) {
    // Base case: f(0) = 0
    if (n == 0) {
        return 0;
    }
    // Base case: f(1) = 1    
    if (n == 1) {
        return 1;
    }        
    
    // Initialize dp
    int[] dp = new int[n+1];
    dp[1] = 1;
    // Compute the fibonacci numbers
    for (int i = 2; i <= n; i++) {
        // Standard formula of fibonacci number
        dp[i] = dp[i-1] + dp[i-2];
    }
    // Return the (n)th fibonacci number
    return dp[n];
}
```
### 4. Space optimized DP
The preceeding dp solutions require an `O(n)` extra space. The below code optimizes the space complexity to `O(1)`.

**n:** Input number <br/>
**dp:** Memoized array <br/>
```java
public int fib(int n) {
    // Base case: f(0) = 0
    if (n == 0) {
        return 0;
    }    
    // Base case: f(1) = 1
    if (n == 1) {
        return 1;
    }        
    // Initialize the first 2 fibonacci numbers
    int a = 0, b = 1;
    // Compute the fibonacci numbers
    for (int i = 2; i <= n; i++) {
        int tmp = b;
        // Standard formula of fibonacci number
        b = a + b;
        a = tmp;
    }
    // Return the (n)th fibonacci number
    return b;
}
```