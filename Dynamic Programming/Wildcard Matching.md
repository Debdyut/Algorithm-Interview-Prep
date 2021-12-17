
# Wildcard Matching
**Author:** Debdyut Hajra <br/>
**Created date:** 17 December 2021 <br/>
**Last updated:** 17 December 2021 <br/>

**Problem Link:** [44. Wildcard Matching](https://leetcode.com/problems/wildcard-matching/) <br/>
**Tags:** Array, Dynamic Programming, String

## Problem

Given a input string and a pattern, check if the string matches the pattern. `'?'` can match any character and `'*'` can match any sequence of 0 or more characters. 

**Example:**

**Input:** s = "aa", p = "a"
**Output:** false
**Explanation:** "a" does not match the entire string "aa".

**Constraints:**

-   `0 <= s.length, p.length <= 2000`
-   `s contains only lowercase English letters.`
-   `p contains only lowercase English letters, '?' or '*'.`

## Solutions

We will look at the following solutions:
1. [Recursive](#1-recursive)
2. [Recursive DP](#2-recursive-dp)
3. [Iterative DP](#3-iterative-dp)
4. [Space optimized DP](#4-space-optimized-dp)

### 1. Recursive
The primary intuition behind the solution is that given the position of 2 pointers on the string and the pattern, we will compare the characters at those positions. If they match or the pattern has a `'?'` at the position, then matching is successful at the given position, and we can increment the positions of the 2 pointers. If instead there is a `'*'`, then there can be 3 possible scenarios. We may either consume the character from the string while choosing to increment the pointer on the pattern, or keep the latter at the same position. Otherwise we can also choose to not consume any character, instead move the pointer from `'*'` to the next character on the pattern.

**s:** Input string <br/>
**p:** Input pattern <br/>
**idx1:** Pointer on string <br/>
**idx2:** Pointer on pattern <br/>

```java
public boolean isMatch(String s, String p) {
    // Return if string is matching starting from front
    return isMatch(s, p, 0, 0);
}

private boolean isMatch(String s, String p, int idx1, int idx2) {
    // If we reach the end of the pattern
    if (idx2 == p.length()) {
        // Check if we are also at the end of the string
        return idx1 == s.length();
    }
    
    // If we have reached the end of the string
    if (idx1 == s.length()) {
        // Make sure only '*' is left 
        // in the remaining part of pattern
        boolean flag = true;
        while (idx2 != p.length()) {
            if (p.charAt(idx2) != '*') {
                flag = false;
                break;
            }
            idx2++;
        }
        return flag;
    }
            
    boolean flag = false;
    // If the characters at given indices are equal
    // or the pattern has a '?' at the given position
    // Then we have a match
    // Increment the poistions of indices on the string and pattern
    if (s.charAt(idx1) == p.charAt(idx2) || p.charAt(idx2) == '?') {
        flag = isMatch(s, p, idx1+1, idx2+1);
    }
    // If the pattern has a '*' at the given position
    if (p.charAt(idx2) == '*') {
        flag = flag || isMatch(s, p, idx1+1, idx2) // Multiple match
                    || isMatch(s, p, idx1+1, idx2+1) // One match
                    || isMatch(s, p, idx1, idx2+1); // No match
    }        
    // Return the result
    return flag;
}
```
### 2. Recursive DP
In the above recusive solution, we can save some computations by memoizing values which are already computed (dynamic programming). We may observe that 2 paramters are changing for each recusive call - the pointers on the string and pattern. Hence, we may introduce a new dp array of dimensions `[m][n]`. As a result, when we encounter the same combination of the paramters, we can reuse the pre-computed value. 

**s:** Input string <br/>
**p:** Input pattern <br/>
**idx1:** Pointer on string <br/>
**idx2:** Pointer on pattern <br/>
**dp:** Memoized array <br/>
```java
public boolean isMatch(String s, String p) {
    // Initialize the dp
    int m = s.length();
    int n = p.length();
    Boolean[][] dp = new Boolean[m+1][n+1];
    // Return if string is matching starting from front
    return isMatch(s, p, 0, 0, dp);
}

private boolean isMatch(
    String s, 
    String p, 
    int idx1, 
    int idx2, 
    Boolean[][] dp) 
{        
    // If value is pre-computed
    // Return the value
    if (dp[idx1][idx2] != null) {
        return dp[idx1][idx2];
    }
    
    // If we reach the end of the pattern
    if (idx2 == p.length()) {
        // Check if we are also at the end of the string
        // Store the value
        return dp[idx1][idx2] = (idx1 == s.length());
    }
    
    // If we have reached the end of the string
    if (idx1 == s.length()) {
        // Make sure only '*' is left 
        // in the remaining part of pattern
        boolean flag = true;
        while (idx2 != p.length()) {
            if (p.charAt(idx2) != '*') {
                flag = false;
                break;
            }
            idx2++;
        }
        return dp[idx1][idx2] = flag;
    }
            
    boolean flag = false;
    // If the characters at given indices are equal
    // or the pattern has a '?' at the given position
    // Then we have a match
    // Increment the poistions of indices on the string and pattern
    if (s.charAt(idx1) == p.charAt(idx2) || p.charAt(idx2) == '?') {
        flag = isMatch(s, p, idx1+1, idx2+1, dp);
    }
    // If the pattern has a '*' at the given position
    if (p.charAt(idx2) == '*') {
        flag = flag || isMatch(s, p, idx1+1, idx2, dp) // Multiple match 
                    || isMatch(s, p, idx1+1, idx2+1, dp) // One match 
                    || isMatch(s, p, idx1, idx2+1, dp); // No match
    }        
    // Return the result
    // Store the value
    return dp[idx1][idx2] = flag;
}
```
### 3. Iterative DP
The preceding recursive solution has been transformed to form an iterative solution for the given problem. If we draw the recursion tree, we may observe that we get the concrete results starting from the last row. Hence, we begin the loop from the last index. 

**s:** Input string <br/>
**p:** Input pattern <br/>
**dp:** Memoized array <br/>

```java
public boolean isMatch(String s, String p) {
    // Create the dp
    int m = s.length();
    int n = p.length();
    boolean[][] dp = new boolean[m+1][n+1];
    // If both string and pattern are exhausted
    // Result will be true
    dp[m][n] = true;
    // There maybe one or more '*' at the end of the string
    for(int i=p.length()-1;i>=0;i--){
        if(p.charAt(i)!='*')
            break;
        else
            dp[m][i]=true;
    }
    // Begin looping from the end
    for (int i = m-1; i >= 0; i--) {
        for (int j = n-1; j >= 0; j--) {
            // If the characters at given indices are equal
            // or the pattern has a '?' at the given position
            // Then we have a match
            // Increment the poistions of indices on the string and pattern
            if (s.charAt(i) == p.charAt(j) || p.charAt(j) == '?') {
                dp[i][j] = dp[i+1][j+1]; 
            }
            // If the pattern has a '*' at the given position
            if (p.charAt(j) == '*') {
                dp[i][j] = dp[i+1][j] // Multiple match  
                           || dp[i+1][j+1] // One match 
                           || dp[i][j+1]; // No match
            }
        }
    }
    // Return the result
    return dp[0][0];
} 
```
### 4. Space optimized DP
The preceeding dp solutions require an `O(m x n)` extra space. The below code optimizes the space complexity to `O(n)`.

**s:** Input string <br/>
**p:** Input pattern <br/>
**dp:** Memoized array <br/>
```java
public boolean isMatch(String s, String p) {
    // Create the dp
    int m = s.length();
    int n = p.length();
    boolean[] dp = new boolean[n+1];
    // If both string and pattern are exhausted
    // Result will be true
    dp[n] = true;
    // There maybe one or more '*' at the end of the string 
    for(int i=p.length()-1;i>=0;i--){
        if(p.charAt(i)!='*')
            break;
        else
            dp[i]=true;
    }
    for (int i = m-1; i >= 0; i--) {
        boolean[] tmp = new boolean[n+1];
        for (int j = n-1; j >= 0; j--) {
            // If the characters at given indices are equal
            // or the pattern has a '?' at the given position
            // Then we have a match
            // Increment the poistions of indices on the string and pattern
            if (s.charAt(i) == p.charAt(j) || p.charAt(j) == '?') {
                tmp[j] = dp[j+1]; 
            }
            // If the pattern has a '*' at the given position
            if (p.charAt(j) == '*') {
                tmp[j] = dp[j] // Multiple match 
                         || dp[j+1] // One match
                         || tmp[j+1]; // No match
            }
        }
        // Update the dp
        dp = tmp;
    }
    // Return the result
    return dp[0];
}   
```