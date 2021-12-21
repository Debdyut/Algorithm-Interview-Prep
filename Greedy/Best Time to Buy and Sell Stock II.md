# Best Time to Buy and Sell Stock II
**Author:** Debdyut Hajra </br>
**Created date:** 21 December 2021 </br>
**Last updated:** 21 December 2021 </br>

**Problem Link:** [122. Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/) </br>
**Tags:** Array, Dynamic Programming, Greedy

## Problem

Given an array where each element denotes the price of shares for the corresponding day. Find the maximum profit which can be made, given that you may hold only one share at a time. You may buy and sell the share on the same day. 

**Example:**

**Input:** prices = [7,1,5,3,6,4] </br>
**Output:** 7 </br>
**Explanation:** Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.
Total profit is 4 + 3 = 7. </br>

**Constraints:**

- `1 <= prices.length <= 3 * 10^4`
- `0 <= prices[i] <= 10^4`

## Solutions

We will look at the following solutions:
1. [Recursive](#1-recursive)
2. [Recursive DP](#2-recursive-dp)
3. [Iterative DP](#3-iterative-dp)
4. [Greedy (Recommened Solution)](#4-greedy)

### 1. Recursive

The primary intuition behind the solution is that given a share is bought on a day, what are the possible profits we can make by selling the share on the same day or on subsequent days, and the corresponding maximum profit we can make in that scenrio. 

```java
public int maxProfit(int[] prices) {
    int n = prices.length;

    // Return the maximum profit which can be obtained
    // starting from first day
    return maxProfit(prices, n, 0);
}

int maxProfit(int[] prices, int n, int idx) {
    // If we have reached beyond last day, return 0
    if (idx == n) {
        return 0;
    }
    
    int maxProfit = 0;
    // For all days starting from the current
    for (int i = idx; i < n; i++) {
        // Compute the maximum the profit 
        // if share is bought on the given day
        // and sold on the current day
        int tmpProfit = prices[i] - prices[idx] + maxProfit(prices, n, i+1);
        maxProfit = Math.max(maxProfit, tmpProfit);
    }

    // Return the maximum profit
    return maxProfit;
}
```

### 2. Recursive DP

In the above recusive solution, we can save some computations by memoizing values which are already computed (dynamic programming). We may observe that only 1 paramter is changing for each recusive call - the current index. Hence, we can introduce a new dp array of dimensions `[n]`. As a result, when we encounter the same combination of the paramters, we can reuse the pre-computed value. 

```java
public int maxProfit(int[] prices) {
    // Initialize the dp
    int n = prices.length;
    int[] dp = new int[n];
    for (int i = 0; i < n; i++) {
        dp[i] = -1;
    }

    // Return the maximum profit which can be obtained
    // starting from first day
    return maxProfit(prices, n, 0, dp);
}

int maxProfit(int[] prices, int n, int idx, int[] dp) {
    // If we have reached beyond last day, return 0
    if (idx == n) {
        return 0;
    }
    
    // If value is pre-computed, return the value
    if (dp[idx] != -1) {
        return dp[idx];
    }
    
    int maxProfit = 0;
    // For all days starting from the current
    for (int i = idx; i < n; i++) {
        // Compute the maximum the profit 
        // if share is bought on the given day
        // and sold on the current day
        int tmpProfit = prices[i] - prices[idx] + maxProfit(prices, n, i+1, dp);
        maxProfit = Math.max(maxProfit, tmpProfit);
    }

    // Return the maximum profit
    // Store the value for future re-use
    return dp[idx] = maxProfit;
}
```

### 3. Iterative DP
The preceding recursive solution has been transformed to form an iterative solution for the given problem. If we draw the recursion tree, we may observe that we get the concrete results starting from the last house. Hence, we begin the loop from the last index.
```java
public int maxProfit(int[] prices) {
    // Create dp
    int n = prices.length;
    int[] dp = new int[n+1];

    // Begin looping from last index
    for (int i = n-1; i >= 0; i--) {
        // For all current and subsequent days 
        for (int j = i; j < n; j++) {
            // Compute the maximum the profit 
            // if share is bought on the given day
            // and sold on the current day
            dp[i] = Math.max(dp[i], prices[j] - prices[i] + dp[j+1]);
        }
    }

    // Return the maximum profit
    // Store the value for future re-use
    return dp[0];
}
```
### 4. Greedy

The primary intuition behind the solution is that on the monotonically increasing sub-arrays we will keep adding the profits for each day.

**nums:** Input array

```java
public int maxProfit(int[] prices) {
    int n = prices.length;
    int profit = 0;
    // For each day
    for (int i = 0; i < n-1; i++) {
        // If the price for next day is higher
        if (prices[i+1] > prices[i]) {
            // Add the corresponding profit
            profit += prices[i+1] - prices[i];
        }
    }
    // Return the total profit
    return profit;
}
```