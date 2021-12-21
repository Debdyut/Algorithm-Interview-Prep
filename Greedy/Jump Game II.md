# Jump Game II
**Author:** Debdyut Hajra </br>
**Created date:** 19 December 2021 </br>
**Last updated:** 19 December 2021 </br>

**Problem Link:** [45. Jump Game II](https://leetcode.com/problems/jump-game-ii/) </br>
**Tags:** Array, Dynamic Programming, Greedy

## Problem

Given an array where each index indicates the maximum number of indices which can be skipped starting from that index. Following this rule, find the minimum number of jumps required to reach the last element starting from the first element.

**Example:**

**Input:** nums = [2,3,1,1,4] </br>
**Output:** 2 </br>
**Explanation:** The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index. </br>

**Constraints:**

- `1 <= nums.length <= 10^4`
- `0 <= nums[i] <= 1000`

## Solutions

We will look at the following solutions:
1. [Recursive](#1-recursive)
2. [Recursive DP](#2-recursive-dp)
3. [Iterative DP](#3-iterative-dp)
4. [Greedy (Recommened Solution)](#4-greedy)

### 1. Recursive

The primary intuition behind the solution is from a given index we will consider all possible jumps and find the minimum possible steps required to reach the last element.

```java
public int jump(int[] nums) {
    int n = nums.length;
    // Return the minimum number of jumps
    // required to reach the last element
    // starting from the first element
    return jump(nums, n, 0);
}

public int jump(int[] nums, int n, int idx) {
    // If we have reached the last element or beyond
    // number of jumps required is 0
    if (idx >= n-1) {
        return 0;
    }
    
    int min = (int) 1e8;
    // For each possible jump from the current index
    for (int i = 1; i <= nums[idx]; i++) {
        // Compute the minimum possible jumps required
        // for the corresponding jump from the current index
        min = Math.min(min, 1 + jump(nums, n, idx + i));
    }
    // Return the minimum possible jumps required
    return min;
}
```

### 2. Recursive DP

In the above recusive solution, we can save some computations by memoizing values which are already computed (dynamic programming). We may observe that only 1 paramter is changing for each recusive call - the current index. Hence, we can introduce a new dp array of dimensions `[n]`. As a result, when we encounter the same combination of the paramters, we can reuse the pre-computed value. 

```java
public int jump(int[] nums) {
    // Initialize the dp
    int n = nums.length;
    int[] dp = new int[n];
    for (int i = 0; i < n; i++) {
        dp[i] = -1;
    }

    // Return the minimum number of jumps
    // required to reach the last element
    // starting from the first element
    return jump(nums, n, 0, dp);
}

public int jump(int[] nums, int n, int idx, int[] dp) {
    // If we have reached the last element or beyond
    // number of jumps required is 0
    if (idx >= n-1) {
        return 0;
    }
    
    // If value is pre-computed, return the value
    if (dp[idx] != -1) {
        return dp[idx];
    }
    
    int min = (int) 1e8;
    // For each possible jump from the current index
    for (int i = 1; i <= nums[idx]; i++) {
        // Compute the minimum possible jumps required
        // for the corresponding jump from the current index
        min = Math.min(min, 1 + jump(nums, n, idx + i, dp));
    }
    // Return the minimum possible jumps required
    // Store the value for future reuse
    return dp[idx] = min;
}
```

### 3. Iterative DP
The preceding recursive solution has been transformed to form an iterative solution for the given problem. If we draw the recursion tree, we may observe that we get the concrete results starting from the last house. Hence, we begin the loop from the last index.
```java
public int jump(int[] nums) {
    // Initialize the dp
    int n = nums.length;
    int[] dp = new int[n];        
    final int MAX_VALUE = (int) 1e8;
    for (int i = 0; i < n-1; i++) {
        dp[i] = MAX_VALUE;
    }
    // Begin looping from the end
    for (int i = n-1; i >= 0; i--) {
        // For each possible jump from current index
        // while we don't exceed the array boundary
        for (int j = 1; j <= nums[i] && (i+j < n); j++) {
            // Compute the minimum possible jumps required
            // for the corresponding jump from the current index
            dp[i] = Math.min(dp[i], 1 + dp[i+j]);
        }
    }

    // Return the minimum number of jumps
    // required to reach the last element
    // starting from the first element
    return dp[0];
}
```
### 4. Greedy

The primary intuition behind the solution is that we will check that maximum extent we can reach starting from a given index. We will also check if we can reach the given index on the firsthand.

**nums:** Input array

```java
public int jump(int[] nums) {
    int n = nums.length;
    // Maximum index we may reach
    int maxIdx = 0;
    // Current maximum index we can reach for the computed jumps
    int currIdx = 0;
    // Count jumps
    int jumps = 0;
    // For each index
    for (int i = 0; i < n-1; i++) {
        // Find the maximum index we may reach
        maxIdx = Math.max(maxIdx, i + nums[i]);
        // If we need to move beyond our current reach                      
        if (i == currIdx) {
            // Increment the jumps count
            jumps++;
            // Update our current reach
            currIdx = maxIdx; 
        }
    }
    // Return the minimum number of jumps
    // required to reach the last element
    // starting from the first element
    return jumps;
}
```