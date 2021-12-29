
# Digit Sum 
**Author:** Debdyut Hajra <br/>
**Created date:** 29 December 2021 <br/>
**Last updated:** 29 December 2021 <br/>

**Problem Link:** [PR003004 - Digit Sum](https://www.spoj.com/problems/PR003004/) <br/>
**Tags:** Digit Dynamic Programming

## Problem

Given a range [a, b], find sum of digits of all the numbers in the range.

**Example:**

**Input:**  <br/>
3  <br/>
0 10  <br/>
28 31  <br/>
1234 56789  <br/>

**Output:**  <br/>
46  <br/>
28  <br/>
1128600  <br/>

**Constraints:**

- `T ≤ 100`
- `0 <= a <= b <= 10^15`

## Solutions

We will look at the following solutions:
1. [Digit DP](#1-digit-dp)
2. [Driver code](#2-driver-code)

### 1. Digit DP

```java
private static long solve(long a, long b) {
    // Sum of digits [0,b] - Sum of digits [0,a-1] = Sum of digits [a, b]  
    return solve(b) - solve(a-1);
}

private static long solve(long n) {
    // If given number is less than or equal to 0
    if (n <= 0) {
        return 0;
    }

    // Initialize dp
    long[][][] dp = new long[19][170][2];
    for (int i = 0; i < dp.length; i++) {
        for (int j = 0; j < dp[0].length; j++) {
            for (int k = 0; k < dp[0][0].length; k++) {
                dp[i][j][k] = -1;
            }
        }    
    }

    // Create list of digits, in reverse order
    List<Integer> ls = new ArrayList<>();
    while (n > 0) {
        ls.add((int) (n % 10));
        n /= 10;
    }
    // Sum of digits [0, n]
    return solve(ls, ls.size()-1, false, 0, dp);
}

private static long solve(List<Integer> n, int pos, boolean flag, int sum, long[][][] dp) {
    // If all digits are exhausted
    if (pos == -1) {
        // Return sum of digits of the number
        return sum;
    }

    // If value is pre-computed, return the value
    if (dp[pos][sum][flag?1:0] != -1) {
        return dp[pos][sum][flag?1:0];
    }

    // If flag is set to true
    // we can set any digit at current position
    // Else we can go till the current digit
    int limit = flag ? 9 : n.get(pos);

    // Track the total sum
    long ans = 0;
    // For all possible digits we can place
    for (int i = 0; i <= limit; i++) {
        // If we are within the limit 
        if (i < limit) {
            // We can set any digit in the next iteration
            ans += solve(n, pos-1, true, sum+i, dp);
        }  else {
            // Else it will depend on the current flag
            ans += solve(n, pos-1, flag, sum+i, dp);
        }
    }
    // Update dp
    return dp[pos][sum][flag?1:0] = ans;
}
```
### 2. Driver code
```java
public static void main(String[] args) {
    PrintWriter out = new PrintWriter(System.out);
    Judge solution = new Judge();
    InputReader in = solution.new InputReader(System.in);

    int t = in.readInt();

    while (t-- > 0) {            
        long a = in.readLong();
        long b = in.readLong();

        out.println(solve(a, b));
    }

    out.close();
}
```