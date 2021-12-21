# Jump Game
**Author:** Debdyut Hajra </br>
**Created date:** 19 December 2021 </br>
**Last updated:** 19 December 2021 </br>

**Problem Link:** [55. Jump Game](https://leetcode.com/problems/jump-game/) </br>
**Tags:** Array, Dynamic Programming, Greedy

## Problem

Given an array where each index indicates the maximum number of indices which can be skipped starting from that index. Following this rule, check if it is possible to reach from first index to last index.

**Example:**

**Input:** nums = [2,3,1,1,4] </br>
**Output:** true </br>
**Explanation:** Jump 1 step from index 0 to 1, then 3 steps to the last index. </br>

**Constraints:**

- `1 <= nums.length <= 104`
- `0 <= nums[i] <= 105`

## Solutions

We will look at the following solutions:
1. [Recursive](#1-recursive)
2. [Recursive DP](#2-recursive-dp)
3. [Iterative DP](#3-iterative-dp)
4. [Greedy (Recommened Solution)](#4-greedy)

### 1. Recursive

The primary intuition behind the solution is from a given index we will consider all possible jumps and check if we may reach the last index.

```java
public boolean canJump(int[] nums) {
    // Check if we can reach last element
    // starting from first index
    return canJump(nums, nums.length, 0);
}

boolean canJump(int[] nums, int n, int idx) {
    // If we have reached the last element, return true
    if (idx >= n-1) {
        return true;
    }
    
    // For each possible jump at current index
    for (int i = 1; i <= nums[idx]; i++) {
        // For the correponding jump
        // check if possible to finally reach the last index 
        boolean isPossible = canJump(nums, n, idx + i);
        if (isPossible) {
            // Possible to reach the last index
            return true;
        }
    }
    // Not possible to reach the last index
    return false;
}
```

### 2. Recursive DP

In the above recusive solution, we can save some computations by memoizing values which are already computed (dynamic programming). We may observe that only 1 paramter is changing for each recusive call - the current index. Hence, we can introduce a new dp array of dimensions `[n]`. As a result, when we encounter the same combination of the paramters, we can reuse the pre-computed value. 

```java
public boolean canJump(int[] nums) {
    // Create dp
    int n = nums.length;
    Boolean[] dp = new Boolean[n];

    // Check if we can reach last element
    // starting from first index        
    return canJump(nums, nums.length, 0, dp);
}

boolean canJump(int[] nums, int n, int idx, Boolean[] dp) {
    // If we have reached the last element, return true
    if (idx >= n-1) {
        return true;
    }
    
    // If value is pre-computed, return the value
    if (dp[idx] != null) {
        return dp[idx];
    }
    
    // By default, let us consider
    // Not possible to reach the last index
    boolean flag = false;

    // For each possible jump at current index
    for (int i = 1; i <= nums[idx]; i++) {
        // For the correponding jump
        // check if possible to finally reach the last index 
        boolean isPossible = canJump(nums, n, idx + i, dp);
        if (isPossible) {
            // Possible to reach the last index
            flag = true;
            break;
        }
    }

    // Store the value for future re-use
    return dp[idx] = flag;
}
```

### 3. Iterative DP
The preceding recursive solution has been transformed to form an iterative solution for the given problem. If we draw the recursion tree, we may observe that we get the concrete results starting from the last house. Hence, we begin the loop from the last index.
```java
public boolean canJump(int[] nums) {
    // Initialize dp
    int n = nums.length;
    boolean[] dp = new boolean[n];        
    dp[n-1] = true;

    // Begin looping from the end
    for (int i = n-1; i >= 0; i--) {
        // For each possible jump from current index
        // while we don't exceed the array boundary
        // and while we have not encountered a 'true' value
        for (int j = 1; j <= nums[i] && (i+j < n) && !dp[i]; j++) {
            dp[i] = dp[i] || dp[i+j];  
        }             
    }
    
    // Return if we can reach last element
    // starting from first index
    return dp[0];
}
```
### 4. Greedy

The primary intuition behind the solution is that we will check that maximum extent we can reach starting from a given index. We will also check if we can reach the given index on the firsthand.

**nums:** Input array

```java
public boolean canJump(int[] nums) {
    int n = nums.length;
    int maxIdx = 0;
    // For each index
    for (int i = 0; i < n && maxIdx < n-1; i++) {
        // Check if the current index is reachable
        if (maxIdx < i) {
            // If not, return false
            return false;
        }
        // Track the maximum index which can be reached 
        maxIdx = Math.max(maxIdx, i + nums[i]);
    }
    // Return if the last element can be reached
    return maxIdx >= n-1;
}
```