
# Investigation 
**Author:** Debdyut Hajra <br/>
**Created date:** 28 December 2021 <br/>
**Last updated:** 28 December 2021 <br/>

**Problem Link:** [Investigation](https://vjudge.net/problem/LightOJ-1068) <br/>
**Tags:** Digit Dynamic Programming

## Problem

Given a range [a, b], find all numbers which are divisible by k and also its sum of digits must be divisible by k.

**Example:**

**Input:**  <br/>
3 <br/>
1 20 1 <br/>
1 20 2 <br/>
1 1000 4 <br/>

**Output:**  <br/>
Case 1: 20  <br/>
Case 2: 5  <br/>
Case 3: 64  <br/>

**Constraints:**

- `T ≤ 200`
- `1 ≤ A ≤ B < 2^31`
- `0 < K < 10000`

## Solutions

We will look at the following solutions:
1. [Digit DP](#1-digit-dp)
2. [Driver code](#2-driver-code)

### 1. Digit DP
The primary intuition behind the solution is that given some items have been selected and given the remaining weight, whether or not we can pick the current item. Also, how the result will be impacted if we choose to not pick the current item, such that the remaining weight remains the same for the next item. We then try to find the combination for which we get the maximum sum of values.

```java
static int solve(int n, int k, int[][][] dp) {
    // Reverse the integer and store as string
    StringBuilder sb = new StringBuilder(Integer.toString(n)).reverse();    

    // Return the count of numbers in [0, n]
    // which satisfy the given condition
    return solve(sb.length() - 1, 0, 0, false, sb.toString(), k, dp);
}

static int solve(
    int pos,
    int sum, 
    int num, 
    boolean flag,
    String n, 
    int k, 
    int[][][] dp) 
{
    // Base case: When we reach the end of the current integer
    // if sum and num are both completely divisible by k    
    if (pos == -1) {
        // update count
        return (sum == 0 && num == 0) ? 1 : 0;
    }

    // If value is pre-computed, return value
    if(flag && dp[pos][sum][num] != -1) 
        return dp[pos][sum][num];

    // If flag is set to true
    // we can set any digit at current position
    // Else we can go till the current digit
    int limit = flag ? 9 : (n.charAt(pos) - '0');

    // Track the count
    int ans = 0;
    // For all possible digits we can place
    for(int i = 0; i <= limit; i++) {
        // Get the digit sum modulo K      
        int s = ( sum + i) %k;
        // Get the number module k
        int t = ( num * 10 + i) %k;
        // If we are within the limit
        if (i < limit) {
            // We can set any digit in the next iteration
            ans += solve(pos-1, s, t, true, n, k, dp);
        } else {
            // Else it will depend on the current flag
            ans += solve(pos-1, s, t, flag, n, k, dp);
        }
    }

    // Update dp
    if(flag) {
        dp[pos][sum][num] = ans;
    }

    // Return answer
    return ans;
}
```
### 2. Driver code
```java
public static void main(String[] args) throws Exception {
    PrintWriter out = new PrintWriter(System.out);
    Main solution = new Main();
    InputReader in = solution.new InputReader(System.in);        

    int t = in.readInt();

    for (int i = 1; i <= t; i++) {
        int a = in.readInt();
        int b = in.readInt();
        int k = in.readInt();

        int[][][] dp = new int[15][150][150];         
        for (int p = 0; p < dp.length; p++) {
            for (int q = 0; q < dp[0].length; q++) {
                for (int r = 0; r < dp[0][0].length; r++) {
                    dp[p][q][r] = -1;
                }
            }    
        }

        long res = 0;
        if (k <= 90) {
            res = solve(b, k, dp) - solve(a-1, k, dp);
        }

        out.println(String.format("Case " + i + ": " + res));            
    }
    
    out.close();
}
```