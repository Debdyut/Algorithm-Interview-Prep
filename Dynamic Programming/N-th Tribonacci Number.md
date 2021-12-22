
# N-th Tribonacci Number
**Author:** Debdyut Hajra <br/>
**Created date:** 22 December 2021 <br/>
**Last updated:** 22 December 2021 <br/>

**Problem Link:** [1137. N-th Tribonacci Number](https://leetcode.com/problems/n-th-tribonacci-number/) <br/>
**Tags:** Math, Dynamic Programming, Memoization

## Problem

Find n<sup>th</sup> tribonacci number, given:
- `f(0) = 0`
- `f(1) = 1`
- `f(2) = 1`
- `f(n) = f(n-1) + f(n-2) + f(n-3)`

**Example:**

**Input:** n = 4 <br/>
**Output:** 4 <br/>
**Explanation:** <br/>
T_3 = 0 + 1 + 1 = 2 <br/>
T_4 = 1 + 1 + 2 = 4

**Constraints:**

- `0 <= n <= 37`
- `The answer is guaranteed to fit within a 32-bit integer, ie. answer <= 2^31 - 1.`

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
public int tribonacci(int n) {
    // Base case: f(0) = 0
    if (n == 0) {
        return 0;
    }
    // Base case: f(1) = 1 & f(2) = 1
    if (n == 1 || n == 2) {
        return 1;
    }      
    // Standard formula of tribonacci number  
    return tribonacci(n-1) + tribonacci(n-2) + tribonacci(n-3);
}
```
### 2. Recursive DP
In the above recusive solution, we can save some computations by memoizing values which are already computed (dynamic programming). We may observe that only 1 paramter is changing for each recusive call - the current number. Hence, we may introduce a new dp array of dimensions `[n]`. As a result, when we encounter the same combination of the paramters, we can reuse the pre-computed value. 

**n:** Input number <br/>
**dp:** Memoized array <br/>
```java
public int tribonacci(int n) {
    // Initialize dp array
    int[] dp = new int[n+1];
    for (int i = 0; i <= n; i++) {
        dp[i] = -1;
    }
    // Return (n)th tribonacci number
    return tribonacciRec(n, dp);
}

public int tribonacciRec(int n, int[] dp) {
    // Base case: f(0) = 0
    if (n == 0) {
        return 0;
    }
    // Base case: f(1) = 1 & f(2) = 1
    if (n == 1 || n == 2) {
        return 1;
    }        

    // If value is pre-computed, return the value
    if (dp[n] != -1) {
        return dp[n];
    }        
    // Standard formula of tribonacci number  
    return dp[n] = tribonacciRec(n-1, dp) + tribonacciRec(n-2, dp) + tribonacciRec(n-3, dp);
}
```
### 3. Iterative DP
The preceding recursive solution has been transformed to form an iterative solution for the given problem. If we draw the recursion tree, we may observe that we get the concrete results starting from the last row. Hence, we begin the loop from the last index. 

**n:** Input number <br/>
**dp:** Memoized array <br/>

```java
public int tribonacci(int n) {
    // Base case: f(0) = 0
    if (n == 0) {
        return 0;
    }
    // Base case: f(1) = 1 & f(2) = 1
    if (n == 1 || n == 2) {
        return 1;
    }
    // Initialize dp
    int[] dp = new int[n+1];
    dp[1] = dp[2] = 1;
    // Compute the tribonacci numbers
    for (int i = 3; i <= n; i++) {
        // Standard formula of tribonacci number
        dp[i] = dp[i-1] + dp[i-2] + dp[i-3];
    }
    // Return (n)th tribonacci number
    return dp[n];
}
```
### 4. Space optimized DP
The preceeding dp solutions require an `O(n)` extra space. The below code optimizes the space complexity to `O(1)`.

**n:** Input number <br/>
**dp:** Memoized array <br/>
```java
public int tribonacci(int n) {
    // Base case: f(0) = 0
    if (n == 0) {
        return 0;
    }
    // Base case: f(1) = 1 & f(2) = 1
    if (n == 1 || n == 2) {
        return 1;
    }
    // Initialize the first 3 tribonacci numbers
    int a = 0, b = 1, c = 1;      
    // Compute the tribonacci numbers  
    for (int i = 3; i <= n; i++) {
        // Standard formula of tribonacci number
        int d = a + b + c;
        a = b;
        b = c;
        c = d;
    }
    // Return (n)th tribonacci number
    return c;
}
```