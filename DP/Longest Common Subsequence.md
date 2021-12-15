
# Longest Common Subsequence
**Author:** Debdyut Hajra <br/>
**Created date:** 15 December 2021 <br/>
**Last updated:** 15 December 2021 <br/>

**Problem Link:** [1143. Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/) <br/>
**Tags:** Array, Dynamic Programming, Matrix

## Problem

Given 2 strings, find the longest common subsequence. Subsequence is a subset of the characters in a string, in their original order.

**Example:**

**Input:** text1 = "abcde", text2 = "ace" <br/> 
**Output:** 3 <br/>  
**Explanation:** The longest common subsequence is "ace" and its length is 3. <br/>

**Constraints:**

-   `1 <= text1.length, text2.length <= 1000`
-   `text1 and text2 consist of only lowercase English characters.`

## Solutions

We will look at the following solutions:
1. [Recursive](#1-recursive)
2. [Recursive DP](#2-recursive-dp)
3. [Iterative DP](#3-iterative-dp)
4. [Space optimized DP](#4-space-optimized-dp)

### 1. Recursive
The primary intuition behind the solution is that starting from given indices, find the longest subsequence length. If the characters at the given indices we will move both the pointers and simultaneously increment the length by 1. If they don't match, we will take the longest subsequence length by incrementing the pointers on text1 and text2 in turns.  

**text1:** String 1 <br/>
**text2:** String 2 <br/>
**idx1:** Current pointer position on text1 <br/>
**idx2:** Current pointer position on text1 <br />

```java
public int longestCommonSubsequence(String text1, String text2) {
    // The maximum length starting from front
    return longestCommonSubsequence(text1, text2, 0, 0);
}

int longestCommonSubsequence(
    String text1, 
    String text2, 
    int idx1, 
    int idx2) 
{
    // Base case: If we have reached the end of any of the strings
    // return 0
    if (idx1 == text1.length() || idx2 == text2.length()) {
        return 0;
    }        
    
    int max = Integer.MIN_VALUE;
    // If the characters are equal at the given indices
    if (text1.charAt(idx1) == text2.charAt(idx2)) {

        // Increment the length and find the longest desired length
        // from the remaining length of the strings
        max = 1 + longestCommonSubsequence(text1, text2, idx1+1, idx2+1);
    } else {

        // Get the longest desired length
        // by incrementing the index on text1
        max = longestCommonSubsequence(text1, text2, idx1+1, idx2);

        // Get the longest desired length
        // by incrementing the index on text2
        max = Math.max(max, longestCommonSubsequence(text1, text2, idx1, idx2+1));
    }

    // Return the longest length possible 
    // starting from the given indices
    return max;
}
```
### 2. Recursive DP
In the above recusive solution, we can save some computations by memoizing values which are already computed (dynamic programming). We may observe that 2 paramters are changing for each recusive call - the pointer on text1 and the pointer on text2. Hence, we may introduce a new dp array of dimensions `[m][n]`. As a result, when we encounter the same combination of the paramters, we can reuse the pre-computed value. 

**text1:** String 1 <br/>
**text2:** String 2 <br/>
**idx1:** Current pointer position on text1 <br/>
**idx2:** Current pointer position on text1 <br />
**dp:** Memoized array <br/>
```java
public int longestCommonSubsequence(String text1, String text2) {
    // Initialize the dp
    int m = text1.length();
    int n = text2.length();
    int[][] dp = new int[m][n];
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            dp[i][j] = -1;
        }
    }
    // The maximum length starting from front
    return longestCommonSubsequence(text1, text2, 0, 0, dp);
}

int longestCommonSubsequence(
    String text1, 
    String text2, 
    int idx1, 
    int idx2, 
    int[][] dp) 
{
    // Base case: If we have reached the end of any of the strings
    // return 0
    if (idx1 == text1.length() || idx2 == text2.length()) {
        return 0;
    }        
    
    // If value is pre-computed
    // return the value
    if (dp[idx1][idx2] != -1) {
        return dp[idx1][idx2];
    }
        
    int max = Integer.MIN_VALUE;
    // If the characters are equal at the given indices
    if (text1.charAt(idx1) == text2.charAt(idx2)) {

        // Increment the length and find the longest desired length
        // from the remaining length of the strings
        max = 1 + longestCommonSubsequence(text1, text2, idx1+1, idx2+1, dp);
    } else {
        // Get the longest desired length
        // by incrementing the index on text1
        max = longestCommonSubsequence(text1, text2, idx1+1, idx2, dp);

        // Get the longest desired length
        // by incrementing the index on text2
        max = Math.max(max, 
                longestCommonSubsequence(text1, text2, idx1, idx2+1, dp));
    }

    // The maximum length starting from front
    // Store the value for future reuse
    return dp[idx1][idx2] = max;
}
```
### 3. Iterative DP
The preceding recursive solution has been transformed to form an iterative solution for the given problem. If we draw the recursion tree, we may observe that we get the concrete results starting from the last row. Hence, we begin the loop from the last index. 

**text1:** String 1 <br/>
**text2:** String 2 <br/>
**dp:** Memoized array <br/>

```java
public int longestCommonSubsequence(String text1, String text2) {
    // Create the dp
    int m = text1.length();
    int n = text2.length();
    int[][] dp = new int[m+1][n+1];
    
    // Begin looping from the end of the strings
    for (int i = m-1; i >= 0; i--) {
        for (int j = n-1; j >= 0; j--) {
            // If the characters are equal at the given indices
            if (text1.charAt(i) == text2.charAt(j)) {
                // Increment the length 
                // and find the longest desired length
                // from the remaining length of the strings
                dp[i][j] = 1 + dp[i+1][j+1];
            } else {
                // Get the longest desired length
                // by incrementing the index on text1
                dp[i][j] = dp[i+1][j];

                // Get the longest desired length
                // by incrementing the index on text2
                dp[i][j] = Math.max(dp[i][j], dp[i][j+1]);
            }
        }    
    }
    // The maximum length starting from front
    return dp[0][0];
}
```
### 4. Space optimized DP
The preceeding dp solutions require an `O(m x n)` extra space. The below code optimizes the space complexity to `O(n)`.

**text1:** String 1 <br/>
**text2:** String 2 <br/>
**dp:** Memoized array <br/>
```java
public int longestCommonSubsequence(String text1, String text2) {
    // Create the dp
    int m = text1.length();
    int n = text2.length();
    int[] dp = new int[n+1];
    
    // Begin looping from the end of the strings
    for (int i = m-1; i >= 0; i--) {
        int[] tmp = new int[n+1];
        for (int j = n-1; j >= 0; j--) {
            // If the characters are equal at the given indices
            if (text1.charAt(i) == text2.charAt(j)) {
                // Increment the length 
                // and find the longest desired length
                // from the remaining length of the strings
                tmp[j] = 1 + dp[j+1];
            } else {
                // Get the longest desired length
                // by incrementing the index on text1
                tmp[j] = dp[j];

                // Get the longest desired length
                // by incrementing the index on text2
                tmp[j] = Math.max(tmp[j], tmp[j+1]);
            }
        }
        // Update the dp for the next iteration    
        dp = tmp;
    }
    // The maximum length starting from front
    return dp[0];
}
```