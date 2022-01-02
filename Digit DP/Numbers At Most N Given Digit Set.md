
# Numbers At Most N Given Digit Set
**Author:** Debdyut Hajra <br/>
**Created date:** 31 December 2021 <br/>
**Last updated:** 31 December 2021 <br/>

**Problem Link:** [902. Numbers At Most N Given Digit Set](https://leetcode.com/problems/numbers-at-most-n-given-digit-set/) <br/>
**Tags:** Digit Dynamic Programming

## Problem

Given a range [0, n], find count of numbers which can be generated using the allowed digits.

**Example:**

**Input:** digits = ["1","3","5","7"], n = 100 <br/>
**Output:** 20 <br/>
**Explanation:** <br/>
The 20 numbers that can be written are: <br/>
1, 3, 5, 7, 11, 13, 15, 17, 31, 33, 35, 37, 51, 53, 55, 57, 71, 73, 75, 77. <br/>

**Constraints:**

- `1 <= digits.length <= 9`
- `digits[i].length == 1`
- `digits[i] is a digit from '1' to '9'.`
- `All the values in digits are unique.`
- `digits is sorted in non-decreasing order.`
- `1 <= n <= 10^9`

## Solutions

We will look at the following digit dp solution. The intuition is that we will try to construct the possible numbers within the given boundary. We will add trailing zeros iff it is present in the allowed digits' array.

```java
public int atMostNGivenDigitSet(String[] digits, int n) {
    // Create an inverted arraylist of the digits in n
    List<Integer> ls = new ArrayList<>();
    while (n > 0) {
        ls.add(n % 10);
        n /= 10;
    }
    
    // Initialize dp[position][all digits allowed flag][non zero previous digit flag]
    int[][][] dp = new int[10][2][2];
    for (int i = 0; i < dp.length; i++) {
        for (int j = 0; j < dp[0].length; j++) {
            for (int k = 0; k < dp[0][0].length; k++) {
                dp[i][j][k] = -1;
            }
        }    
    }
    // Return numbers which satisfy desired condition in range [0, n]
    return atMostNGivenDigitSet(digits, n, ls, ls.size()-1, false, false, dp) - 1;
}

int atMostNGivenDigitSet(
    String[] digits, 
    int n, 
    List<Integer> ls, 
    int pos, 
    boolean flag,
    boolean nonZero,
    int[][][] dp) 
{
    // If all digits exhausted
    if (pos == -1) {
        // Successfully generated a number            
        return 1;
    }

    // If value is pre-computed, return the value
    if (dp[pos][flag?1:0][nonZero?1:0] != -1) {
        return dp[pos][flag?1:0][nonZero?1:0];
    }
    
    // If flag is set to true
    // we can set any digit at current position
    // Else we can go till the current digit
    int limit = flag ? 9 : ls.get(pos);
    
    // If only zeros have been encountered till now,
    // allow 0 to be added at this digit place
    int count = nonZero ? 0 : atMostNGivenDigitSet(digits, n, ls, pos-1, true, false, dp);
    // For all allowed digits in the digit array which are within the limit
    for (int i = 0; i < digits.length && Integer.valueOf(digits[i]) <= limit; i++) {             
        // If the current digit is less than the limit 
        if (Integer.valueOf(digits[i]) < limit) {
            // We can set any digit in the next iteration
            // Set nonzero flag to true to denote a non-zero digit was encountered
            // This will allow 0 to be added henceforth only if 0 is present in input digit array
            count += atMostNGivenDigitSet(digits, n, ls, pos-1, true, true, dp);
        } else {
            // Else it will depend on the current flag
            count += atMostNGivenDigitSet(digits, n, ls, pos-1, flag, true, dp);
        }
    }

    // Return numbers which satisfy desired condition
    // Store the value for future reuse
    return dp[pos][flag?1:0][nonZero?1:0] = count;
}
```