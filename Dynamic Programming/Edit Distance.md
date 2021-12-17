
# Edit Distance
**Author:** Debdyut Hajra <br/>
**Created date:** 17 December 2021 <br/>
**Last updated:** 17 December 2021 <br/>

**Problem Link:** [72. Edit Distance](https://leetcode.com/problems/edit-distance/) <br/>
**Tags:** Array, Dynamic Programming, String

## Problem

Given 2 words, find the minimum number of operations required to convert word1 to word2.

**Example:**

**Input:** word1 = "horse", word2 = "ros" <br/>
**Output:** 3 <br/>
**Explanation:**  <br/>
horse -> rorse (replace 'h' with 'r')
rorse -> rose (remove 'r')
rose -> ros (remove 'e')

**Constraints:**

-   `0 <= word1.length, word2.length <= 500`
-   `word1 and word2 consist of lowercase English letters.`

## Solutions

We will look at the following solutions:
1. [Recursive](#1-recursive)
2. [Recursive DP](#2-recursive-dp)
3. [Iterative DP](#3-iterative-dp)
4. [Space optimized DP](#4-space-optimized-dp)

### 1. Recursive
The primary intuition behind the solution is that given the position of 2 pointers on the 2 given words, compare the characters at these positions. If they are equal, no further action is required, and both the pointers may be moved forward to compare the subsequent characters. If they are not equal, then we may try to replace the character in the first word, and then increment both the pointers. Instead if we choose to insert the required character at the given position, then only the second pointer would move and the first pointer will continue to point to the same character. But if we choose to delete the character, the first pointer would move instead. In all the three cases, we would increment the number of operations by 1. 

**word1:** Input word 1 <br/>
**word2:** Input word 2 <br/>
**idx1:** Pointer on word 1 <br/>
**idx2:** Pointer on word 2 <br/>

```java
public int minDistance(String word1, String word2) {
    // Get the minimum edit distance starting from front
    return minDistance(word1, word2, 0, 0);
}

public int minDistance(String word1, String word2, int idx1, int idx2) {
    // Check if reached end of the any of the 2 words
    if (idx1 == word1.length() || idx2 == word2.length()) {
        // Return the number of characters if remaining for the other string
        // If word1 is not exhausted, delete the remaining characters
        // If word2 is not exhausted, insert the remaining characters
        return Math.max(word1.length() -  idx1, word2.length() - idx2);
    }
    
    int min = Integer.MAX_VALUE;
    // If the characters at the given indices are equal
    if (word1.charAt(idx1) == word2.charAt(idx2)) {
        // No action required
        // Increment both pointers
        min = minDistance(word1, word2, idx1+1, idx2+1);
    }
    else {
        // Replace and check the minimum edit distance as a result
        min = 1 + minDistance(word1, word2, idx1+1, idx2+1);
        // Insert and check the minimum edit distance as a result
        min = Math.min(min, 1 + minDistance(word1, word2, idx1, idx2+1));
        // Delete and check the minimum edit distance as a result
        min = Math.min(min, 1 + minDistance(word1, word2, idx1+1, idx2));
    }
    
    // Return the minimum edit distance from the given indices
    return min;
}
```
### 2. Recursive DP
In the above recusive solution, we can save some computations by memoizing values which are already computed (dynamic programming). We may observe that 2 paramters are changing for each recusive call - the pointers on the 2 words. Hence, we may introduce a new dp array of dimensions `[m][n]`. As a result, when we encounter the same combination of the paramters, we can reuse the pre-computed value. 

**word1:** Input word 1 <br/>
**word2:** Input word 2 <br/>
**idx1:** Pointer on word 1 <br/>
**idx2:** Pointer on word 2 <br/>
**dp:** Memoized array <br/>
```java
public int minDistance(String word1, String word2) {
    // Initialize the dp array
    int m = word1.length();
    int n = word2.length();
    int[][] dp = new int[m][n];
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            dp[i][j] = -1;
        }
    }
    // Get the minimum edit distance starting from front
    return minDistance(word1, word2, 0, 0, dp);
}

public int minDistance(
    String word1, 
    String word2, 
    int idx1, 
    int idx2, 
    int[][] dp) 
{
    // Check if reached end of the any of the 2 words
    if (idx1 == word1.length() || idx2 == word2.length()) {
        // Return the number of characters if remaining for the other string
        // If word1 is not exhausted, delete the remaining characters
        // If word2 is not exhausted, insert the remaining characters
        return Math.max(word1.length() -  idx1, word2.length() - idx2);
    }
    
    if (dp[idx1][idx2] != -1) {
        return dp[idx1][idx2];
    }
    
    int min = Integer.MAX_VALUE;
    // If the characters at the given indices are equal
    if (word1.charAt(idx1) == word2.charAt(idx2)) {
        // No action required
        // Increment both pointers
        min = minDistance(word1, word2, idx1+1, idx2+1, dp);
    }
    else {
        // Replace and check the minimum edit distance as a result
        min = 1 + minDistance(word1, word2, idx1+1, idx2+1, dp);
        // Insert and check the minimum edit distance as a result
        min = Math.min(min, 1 + minDistance(word1, word2, idx1, idx2+1, dp));
        // Delete and check the minimum edit distance as a result
        min = Math.min(min, 1 + minDistance(word1, word2, idx1+1, idx2, dp));
    }

    // Return the minimum edit distance from the given indices
    return dp[idx1][idx2] = min;
}
```
### 3. Iterative DP
The preceding recursive solution has been transformed to form an iterative solution for the given problem. If we draw the recursion tree, we may observe that we get the concrete results starting from the last row. Hence, we begin the loop from the last index. 

**word1:** Input word 1 <br/>
**word2:** Input word 2 <br/>
**dp:** Memoized array <br/>

```java
public int minDistance(String word1, String word2) {
    // Initialize the dp array
    int m = word1.length();
    int n = word2.length();
    int[][] dp = new int[m+1][n+1];
    for (int i = 0; i <= n; i++) {
        dp[m][i] = n - i;
    }
    for (int i = 0; i <= m; i++) {
        dp[i][n] = m - i;
    }
    // Begin looping from the end
    for (int i = m-1; i >= 0; i--) {
        for (int j = n-1; j >= 0; j--) {
            // If the characters at the given indices are equal
            if (word1.charAt(i) == word2.charAt(j)) {
                // No action required
                // Increment both pointers
                dp[i][j] = dp[i+1][j+1];
            } else {
                // Replace and check the minimum edit distance as a result
                dp[i][j] = 1 + dp[i+1][j+1];
                // Insert and check the minimum edit distance as a result
                dp[i][j] = Math.min(dp[i][j], 1 + dp[i][j+1]);
                // Delete and check the minimum edit distance as a result
                dp[i][j] = Math.min(dp[i][j], 1 + dp[i+1][j]);
            }
        }
    }
    // Get the minimum edit distance starting from front        
    return dp[0][0];
}
```
### 4. Space optimized DP
The preceeding dp solutions require an `O(m x n)` extra space. The below code optimizes the space complexity to `O(n)`.

**word1:** Input word 1 <br/>
**word2:** Input word 2 <br/>
**dp:** Memoized array <br/>
```java
public int minDistance(String word1, String word2) {
    // Initialize the dp array
    int m = word1.length();
    int n = word2.length();
    int[] dp = new int[n+1];
    for (int i = 0; i <= n; i++) {
        dp[i] = n - i;
    }
    // Begin looping from the end        
    for (int i = m-1; i >= 0; i--) {             
        int[] tmp = new int[n+1];            
        tmp[n] = m - i;
        for (int j = n-1; j >= 0; j--) {            
            // If the characters at the given indices are equal    
            if (word1.charAt(i) == word2.charAt(j)) {
                // No action required
                // Increment both pointers
                tmp[j] = dp[j+1];
            } else {
                // Replace and check the minimum edit distance as a result
                tmp[j] = 1 + dp[j+1];
                // Insert and check the minimum edit distance as a result
                tmp[j] = Math.min(tmp[j], 1 + tmp[j+1]);
                // Delete and check the minimum edit distance as a result
                tmp[j] = Math.min(tmp[j], 1 + dp[j]);
            }
        }
        // Update the dp array
        dp = tmp;
    }  
    // Get the minimum edit distance starting from front      
    return dp[0];
}
```