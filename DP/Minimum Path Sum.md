
# Minimum Path Sum
**Author:** Debdyut Hajra <br/>
**Created date:** 14 December 2021 <br/>
**Last updated:** 14 December 2021 <br/>

**Problem Link:** [64. Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/) <br/>
**Tags:** Array, Dynamic Programming, Matrix

## Problem

Given a `m x n` matrix, find the minimum cost to move from `[0][0]` to `[m-1][n-1]`, moving only right or down in each step.

**Example:**

**Input:** grid = [[1,3,1],[1,5,1],[4,2,1]] <br/>
**Output:** 7 <br/>
**Explanation:** Because the path 1 → 3 → 1 → 1 → 1 minimizes the sum. <br/>

**Constraints:**

-   `m == grid.length`
-   `n == grid[i].length`
-   `1 <= m, n <= 200`
-   `0 <= grid[i][j] <= 100`

## Solutions

We will look at the following solutions:
1. [Recursive](#1-recursive)
2. [Recursive DP](#2-recursive-dp)
3. [Iterative DP](#3-iterative-dp)
4. [Space optimized DP](#4-space-optimized-dp)
5. [Print path (helper method)](#5-print-path-helper-method)

### 1. Recursive
The primary intuition behind the solution is that from a given cell we will either move to the right or move down, based on which path results in minimum cost, until we reach the destination cell. 

**grid:** Input matrix <br/>
**m:** Number of rows <br/>
**n:** Number of columns <br/>

```java
public int minPathSum(int[][] grid) {
	// Number of rows
    int m = grid.length;
    // Number of columns
    int n = grid[0].length;

    // Return the minimum cost to go from [0][0] to [m-1][n-1]
    return minPathSum(grid, m, n, 0, 0); 
}

public int minPathSum(int[][] grid, int m, int n, int x, int y) 
{                   
	// Check if out of bounds
    if (x >= m || y >= n) {
        return Integer.MAX_VALUE;
    }
    
    // Check if reached bottom right corner
    if (x == m-1 && y == n-1) {
	    // Return the value at the cell
        return grid[m-1][n-1];
    }
    
    // Minimum cost if going down
    int down = minPathSum(grid, m, n, x+1, y);
    
    // Minimum cost if going right
    int right = minPathSum(grid, m, n, x, y+1);               

	// Result is the sum of current cell value
	// and the minimum of going down or right    
    return grid[x][y] + Math.min(down, right);
}
```
### 2. Recursive DP
In the above recusive solution, we can save some computations by memoizing values which are already computed (dynamic programming). We may observe that 2 paramters are changing for each recusive call - the row and the column. Hence, we may introduce a new dp array of dimensions `[m][n]`. As a result, when we encounter the same combination of the paramters, we can reuse the pre-computed value. 

**grid:** Input matrix <br/>
**m:** Number of rows <br/>
**n:** Number of columns <br/>
**dp:** Memoized array <br/>
```java
public int minPathSum(int[][] grid) {                    
    // Number of rows
    int m = grid.length;
    // Number of columns
    int n = grid[0].length;
    
    // Intialize dp
    int[][] dp = new int[m][n];
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            dp[i][j] =  -1;
        }
    }
    
    // Return the minimum cost to go from [0][0] to [m-1][n-1]
    return minPathSum(grid, m, n, 0, 0, dp); 
}

public int minPathSum(int[][] grid, int m, int n, int x, int y, int[][] dp) {                
	// Check if out of bounds
    if (x >= m || y >= n) {
        return Integer.MAX_VALUE;
    }
    
    // Check if reached bottom right corner
    if (x == m-1 && y == n-1) {
	    // Return the value at the cell
        return grid[m-1][n-1];
    }
    
    // If value is already pre-computed
    if (dp[x][y] != -1) {
	    // Return the pre-computed value
        return dp[x][y];
    }
    
    // Minimum cost if going down
    int down = minPathSum(grid, m, n, x+1, y, dp);
    
    // Minimum cost if going right
    int right = minPathSum(grid, m, n, x, y+1, dp);               
    
    // Result is the sum of current cell value
	// and the minimum of going down or right  
    // Store the value for use in subsequent step
    return dp[x][y] = grid[x][y] + Math.min(down, right);
}
```
### 3. Iterative DP
The preceding recursive solution has been transformed to form an iterative solution for the given problem. If we draw the recursion tree, we may observe that we get the concrete results starting from the last row. Hence, we begin the loop from the last index. 

**grid:** Input matrix <br/>
**m:** Number of rows <br/>
**n:** Number of columns <br/>
**dp:** Memoized array <br/>

```java
public int minPathSum(int[][] grid) {                    
    // Number of rows
    int m = grid.length;
    // Number of columns
    int n = grid[0].length;
    
    // Create dp
    int[][] dp = new int[m][n];
    // Begin looping backwards from [m-1][n-1] to [0][0]
    for (int i = m-1; i >= 0; i--) {
        for (int j = n-1; j >= 0; j--) {
	        // Minimum cost if going down
            int down = ( i == m-1 ) ? Integer.MAX_VALUE : dp[i+1][j];
			// Minimum cost if going right 
            int right = ( j == n-1 ) ? Integer.MAX_VALUE : dp[i][j+1];
            
            // Check if at bottom right corner
            if (i == m-1 && j == n-1) {
	            // Add 0
                down = right = 0;
            }
            // Result is the sum of current cell value
			// and the minimum of going down or right  
            dp[i][j] = grid[i][j] + Math.min(down, right); 
        }
    }
    // Return the minimum cost to go from [0][0] to [m-1][n-1]
    return dp[0][0]; 
}
```
### 4. Space optimized DP
The preceeding dp solutions require an `O(m x n)` extra space. The below code optimizes the space complexity to `O(n)`.

**grid:** Input matrix <br/>
**m:** Number of rows <br/>
**n:** Number of columns <br/>
**N:** Number of items
```java
public int minPathSum(int[][] grid) 
{                    
    // Number of rows
    int m = grid.length;
    // Number of columns
    int n = grid[0].length;
    
    // Create dp
    int[] dp = new int[n];
    // Begin looping backwards from [m-1][n-1] to [0][0]
    for (int i = m-1; i >= 0; i--) {
        int[] tmp = new int[n];
        for (int j = n-1; j >= 0; j--) {
            int max = 0;
	        // Minimum cost if going down
            if ( i < m-1 ) max = dp[j];
            // Minimum cost if going right 
            int right = ( j == n-1 ) ? Integer.MAX_VALUE : tmp[j+1];
            
			// Check if at bottom right corner
            if (i == m-1 && j == n-1) {
	            // Add 0
                down = right = 0;
            }
            // Result is the sum of current cell value
			// and the minimum of going down or right  
            tmp[j] = grid[i][j] + Math.min(down, right); 
        }
        // Update dp for next iteration
        dp = tmp;
    }
    // Return the minimum cost to go from [0][0] to [m-1][n-1]
    return dp[0]; 
}
```
### 5. Print path (helper method)
Print the minimum cost path. Works with 2 and 3 above.
```java
// Create a result collection
List<String> res = new ArrayList<>();
// Begin from top-left corner
for (int i = 0, j = 0; i != m-1 || j != n-1; ) {            
    // Add current cell to result
    res.add("[" + i + ", " + j + "]");
    // Find the next dp value
    int tmp = dp[i][j] - grid[i][j];
    // Determine whether it matches with the below or right cell
    if (i < m-1 && tmp == dp[i+1][j]) {
        // Move down
        i++;
    } else {
        // Move right
        j++;
    }
}
// Print the result
System.out.println(String.join(" -> ", res.toArray(new String[res.size()])));
```