
# 369 Numbers
**Author:** Debdyut Hajra <br/>
**Created date:** 31 December 2021 <br/>
**Last updated:** 31 December 2021 <br/>

**Problem Link:** [NUMTSN - 369 Numbers](https://www.spoj.com/problems/NUMTSN/) <br/>
**Tags:** Digit Dynamic Programming

## Problem

Given a range [a, b], find count of numbers in range which have equal number of 3s, 6s and 9s, and has at least one 3.

**Example:**

**Sample Input** <br/>
3 <br/>
121 4325 <br/>
432 4356 <br/>
4234 4325667 <br/>

**Sample Output** <br/>
60 <br/>
58 <br/>
207159 <br/>

**Constraints:**

- `T<=100`
- `1<=A<=B<=10^50`

## Solutions

We will look at the following digit dp solution.

```java
// Initialize mod value
private static final long MOD = 1000000007;

private static long solve(String a, String b, long[][][][] dp) {
    // Reduce 1 from the lower limit (a)
    StringBuilder sb = new StringBuilder(a);
    int p = sb.length()-1;
    while (true) {
        if (sb.charAt(p) == '0') {
            sb.setCharAt(p, (char) (9 + '0'));
            p--;
        } else {
            sb.setCharAt(p,  (char) (sb.charAt(p) - 1 ));
            break;
        }
    }
    a = sb.toString();

    // Return numbers which satisfy desired condition in range [a, b] = f([0,b]) - f([0,a-1]) 
    return (solve(b, dp) - solve(a, dp) + MOD) %MOD;
}

private static long solve(String s, long[][][][] dp) {
    // If the number is zero
    if (s.length() == 1 && Integer.valueOf(s) == 0) {
        // There are 0 possible numbers which satisfy the condition
        return 0;
    }        

    // Create an inverted arraylist of the digits in n
    List<Integer> ls = new ArrayList<>();
    for (int i = s.length()-1; i >= 0; i--)  {
        ls.add(s.charAt(i) - '0');            
    }

    // Return numbers which satisfy desired condition in range [0, n]
    return solve(ls, ls.size()-1, false, 0, 0, 0, dp);
}

private static long solve(List<Integer> ls, int pos, boolean flag, int c3, int c6, int c9, long[][][][] dp) {
    // If c3/c6/c9 go beyong 16, then it cannot be a 369 number 
    // given maximum number of digits is 50
    if (c3 >= 17 || c6 >= 17 || c9 >= 17) {
        return 0;
    }

    // If all digits exhausted
    if (pos == -1) {
        // Check if condition is satisfied
        return (c3 > 0 && c3 == c6 && c6 == c9) ? 1 : 0;
    }

    // If value is pre-computed, return the value
    if (flag && dp[pos][c3][c6][c9] != -1) {
        return dp[pos][c3][c6][c9];
    }

    // If flag is set to true
    // we can set any digit at current position
    // Else we can go till the current digit
    int limit = flag ? 9 : ls.get(pos);

    // Track the count of numbers which satisfy the condition
    long count = 0;
    // For all possible digits we can place
    for (int i = 0; i <= limit; i++) {
        // If the current digit is less than the limit 
        if (i < limit) {        
            // We can set any digit in the next iteration
            // Increase count c3/c6/c9 if applicable
            count = (count + solve(ls, pos-1, true, (i == 3) ? c3+1 : c3, (i == 6) ? c6+1 : c6, (i == 9) ? c9+1 : c9, dp)) %MOD;
        } else {
            // Else it will depend on the current flag
            count = (count + solve(ls, pos-1, flag, (i == 3) ? c3+1 : c3, (i == 6) ? c6+1 : c6, (i == 9) ? c9+1 : c9, dp)) %MOD;
        }
    }

    // Store the value for future reuse
    if (flag) {
        dp[pos][c3][c6][c9] = count;
    }

    // Return numbers which satisfy desired condition    
    return count;
}

public static void main(String[] args) {
    PrintWriter out = new PrintWriter(System.out);
    Judge solution = new Judge();
    InputReader in = solution.new InputReader(System.in);        

    int t = in.readInt();

    // Initialize dp[position][count of 3][count of 6][count of 9]
    long[][][][] dp = new long[50][18][18][18];
    for (int i = 0; i < dp.length; i++) {
        for (int j = 0; j < dp[0].length; j++) {
            for (int k = 0; k < dp[0][0].length; k++) {
                for (int l = 0; l < dp[0][0][0].length; l++) {                        
                    dp[i][j][k][l] = -1;                        
                }
            }
        }
    }

    while (t-- > 0) {            
        String a = in.readString();
        String b = in.readString();                        

        out.println(solve(a, b, dp));
    }

    out.close();
}
```