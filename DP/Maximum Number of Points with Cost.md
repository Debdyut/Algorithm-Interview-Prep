
# Maximum Number of Points with Cost
**Author:** Debdyut Hajra <br/>
**Created date:** 15 December 2021 <br/>
**Last updated:** 15 December 2021 <br/>

**Problem Link:** [1937. Maximum Number of Points with Cost](https://leetcode.com/problems/maximum-number-of-points-with-cost/) <br/>
**Tags:** Array, Dynamic Programming, Matrix

## Problem

Given a `m x n` matrix, try to maximize the points following below rules:

1. Select a cell from each row which gives `points[i][j]`
2. Deduct `abs(j - prevCol)` from the point obtained.

**Example:**

**Input:** points = [[1,2,3],[1,5,1],[3,1,1]] <br/>
**Output:** 9 <br/>
**Explanation:** <br/>
The blue cells denote the optimal cells to pick, which have coordinates (0, 2), (1, 1), and (2, 0).
You add 3 + 5 + 3 = 11 to your score.
However, you must subtract abs(2 - 1) + abs(1 - 0) = 2 from your score.
Your final score is 11 - 2 = 9.

**Constraints:**

-   `m == points.length`
-   `n == points[r].length`
-   `1 <= m, n <= 105`
-   `1 <= m * n <= 105`
-   `0 <= points[r][c] <= 105`

## Solutions

We will look at the following solutions:
1. [Recursive](#1-recursive)
2. [Recursive DP](##2-recursive-dp)
3. [Iterative DP](#3-iterative-dp)
4. [Space optimized DP](#4-space-optimized-dp)
5. [Time-optimized DP](#5-time-optimized-dp)

### 1. Recursive
The primary intuition behind the solution is that given a particular cell is computed, find the maximum points which can be obtained by selecting a corresponding cell from the below row and considering the points deduction in each case. 

**points:** Input matrix <br/>
**m:** Number of rows <br/>
**n:** Number of columns <br/>
**row:** Current row <br />
**prevCol:** Column selected in previous row <br /> 

```java
public long maxPoints(int[][] points) {
    // Number of rows
    int m = points.length;
    // Number of columns
    int n = points[0].length;

    // Find maximum points starting from row 0
    return maxPoints(points, m, n, 0, -1);
}

private long maxPoints(
    int[][] points, 
    int m, 
    int n, 
    int row, 
    int prevCol) 
{
    // Base case: For (m+1)th column return 0
    if (row == m) {
        return 0;
    }        
    
    long max = Long.MIN_VALUE;
    // Given prevCol was selected in earlier step
    // For each point in the row
    for (int i = 0; i < n; i++) {
        // Compute the points that can be obtained from this cell
        long tmp = points[row][i] + maxPoints(points, m, n, row+1, i);

        // If a row is already selected 
        // -1: no rows selected
        if (prevCol >= 0) {
            // Deduct penalty
            tmp -= Math.abs(i - prevCol);
        } 
        // Store the maximum points that can be obtained till this step
        max = Math.max(max, tmp);
    }
    // Return the maximum points that can be obtained
    return max;
}
```
### 2. Recursive DP
In the above recusive solution, we can save some computations by memoizing values which are already computed (dynamic programming). We may observe that 2 paramters are changing for each recusive call - the current row and the previously selected column. Hence, we may introduce a new dp array of dimensions `[m][n]`. As a result, when we encounter the same combination of the paramters, we can reuse the pre-computed value. 

**points:** Input matrix <br/>
**m:** Number of rows <br/>
**n:** Number of columns <br/>
**row:** Current row <br />
**prevCol:** Column selected in previous row <br /> 
**dp:** Memoized array <br/>
```java
public long maxPoints(int[][] points) {
    // Number of rows
    int m = points.length;
    // Number of columns
    int n = points[0].length;
    
    // Initialize dp array
    long[][] dp = new long[m][n];
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            dp[i][j] = -1;
        }
    }

    // Find maximum points starting from row 0
    return maxPoints(points, m, n, 0, -1, dp);
}

private long maxPoints(
    int[][] points, 
    int m, 
    int n, 
    int row, 
    int prevCol, 
    long[][] dp) 
{
    // Base case: For (m+1)th column return 0
    if (row == m) {
        return 0;
    }        
    
    // If value is pre-computed
    // return the value
    if (prevCol >= 0 && dp[row][prevCol] != -1) {
        return dp[row][prevCol];
    }
    
    long max = Long.MIN_VALUE;
    // Given prevCol was selected in earlier step
    // For each point in the row
    for (int i = 0; i < n; i++) {
        // Compute the points that can be obtained from this cell
        long tmp = points[row][i] + maxPoints(points, m, n, row+1, i, dp);

        // If a row is already selected 
        // -1: no rows selected
        if (prevCol >= 0) {
            tmp -= Math.abs(i - prevCol);
        } 

        // Store the maximum points that can be obtained till this step
        max = Math.max(max, tmp);
    }

    // Store the value for future reuse
    if (prevCol >= 0) dp[row][prevCol] = max;
    
    // Return the maximum points that can be obtained
    return max;
}
```
### 3. Iterative DP
The preceding recursive solution has been transformed to form an iterative solution for the given problem. If we draw the recursion tree, we may observe that we get the concrete results starting from the last row. Hence, we begin the loop from the last index. 

**points:** Input matrix <br/>
**m:** Number of rows <br/>
**n:** Number of columns <br/>
**dp:** Memoized array <br/>

```java
public long maxPoints(int[][] points) {
    // Number of rows
    int m = points.length;
    // Number of columns
    int n = points[0].length;
    
    // Create dp array
    long[][] dp = new long[m][n];
    // Begin looping from last row
    for (int i = m-1; i >= 0; i--) {
        // For each colum            
        for (int j = 0; j < n; j++) {
            // Compute maximum points that can be obtained from given cell
            long max = points[i][j];
            // For each column in the below row
            for (int k = 0; k < n && i < m-1; k++) {
                // Check which column gives the maximum points
                max = Math.max(max, points[i][j] + dp[i+1][k] - Math.abs(j - k));
            }
            // Store the maximum points that can be obtained
            dp[i][j] = max;
        }
    }
    
    // For each colum in the first row
    // find the maximum points
    long max = Long.MIN_VALUE;
    for (int i = 0; i < n; i++) {
        max = Math.max(max, dp[0][i]);
    }

    // Return the maximum points
    return max;
}
```
### 4. Space optimized DP
The preceeding dp solutions require an `O(m x n)` extra space. The below code optimizes the space complexity to `O(n)`.

**points:** Input matrix <br/>
**m:** Number of rows <br/>
**n:** Number of columns <br/>
**dp:** Memoized array <br/>
```java
public long maxPoints(int[][] points) {
    // Number of rows
    int m = points.length;
    // Number of columns
    int n = points[0].length;
    
    // Create dp array
    long[] dp = new long[n];
    // Begin looping from last row
    for (int i = m-1; i >= 0; i--) {                      
        long[] tmp = new long[n];
        // For each colum
        for (int j = 0; j < n; j++) {
            // Compute maximum points that can be obtained from given cell
            long max = points[i][j];
            // For each column in the below row
            for (int k = 0; k < n && i < m-1; k++) {
                // Check which column gives the maximum points
                max = Math.max(max, points[i][j] + dp[k] - Math.abs(j - k));
            }
            // Store the maximum points that can be obtained
            tmp[j] = max;
        }
        // Update the dp array for the next iteration
        dp = tmp;
    }
    
    // For each colum in the first row
    // find the maximum points
    long max = Long.MIN_VALUE;
    for (int i = 0; i < n; i++) {
        max = Math.max(max, dp[i]);
    }

    // Return the maximum points
    return max;
}
```
### 5. Time-optimized DP
The preceeding dp solution has an `O(m x n x n)` or `O(n^3)` time complexity. The below code optimizes the time complexity to `O(n^2)`.
```java
public long maxPoints(int[][] points) {
    // Number of rows
    int m = points.length;
    // Number of columns
    int n = points[0].length;
    
    // Create dp array
    long[] dp = new long[n];
    // Begin looping from last row
    for (int i = m-1; i >= 0; i--) {            
        long[] tmp = new long[n];
        
        // Precompute the prefix max values on the left
        long[] prefixMax = new long[n];
        for (int j = 0; j < n; j++) {
            if (j == 0) prefixMax[j] = dp[j] + j;
            else prefixMax[j] = Math.max(dp[j] + j, prefixMax[j-1]);
        }
        
        // Compute the maximum points at each cell
        for (int j = 0; j < n; j++) {
            tmp[j] = Math.max(tmp[j], points[i][j] - j + prefixMax[j]);                
        }
        
        // Precompute the suffix max values on the right
        long[] suffixMax = new long[n];
        for (int j = n-1; j >= 0; j--) {
            if (j == n-1) suffixMax[j] = dp[j] - j;
            else suffixMax[j] = Math.max(dp[j] - j, suffixMax[j+1]);
        }
        
        // Compute the maximum points at each cell
        for (int j = 0; j < n; j++) {
            tmp[j] = Math.max(tmp[j], points[i][j] + j + suffixMax[j]);                
        }
        // Update the dp array for the next iteration
        dp = tmp;
    }
    
    // For each colum in the first row
    // find the maximum points
    long max = Long.MIN_VALUE;
    for (int i = 0; i < n; i++) {
        max = Math.max(max, dp[i]);
    }

    // Return the maximum points
    return max;
}
```