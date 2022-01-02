
# Numbers with same first and last digit
**Author:** Debdyut Hajra <br/>
**Created date:** 31 December 2021 <br/>
**Last updated:** 31 December 2021 <br/>

**Problem Link:** [Numbers with same first and last digit](https://practice.geeksforgeeks.org/problems/numbers-with-same-first-and-last-digit4228/1#) <br/>
**Tags:** Digit Dynamic Programming

## Problem

Given a range [a, b], find the count of numbers in the range which have the same first and last digit.

**Example:**

**Input:** L = 7, R = 68
**Output:** 9
**Explanation:** Numbers with the same first and last digits in the given range are: [7, 8, 9, 11, 22, 33, 44 ,55, 66].

**Constraints:**

- `1 <= L,R <= 10^8`

## Solutions

We will look at the following digit dp solution. The primary intuition is that we will try to build all the numbers and in process track the corresponding first and last digits to check our condition.

```java
static int numbersInRange(int L, int R){
    // Left limit cannot be greater than right limit
    if (L > R) {
        return 0;
    }
    // Return numbers which satisfy desired condition in range [L, R]
    return numbersInRange(R) - numbersInRange(L-1);
}

static int numbersInRange(int n){
    // If number is zero, there is only 1 possible result - 0 
    if (n <= 0) {
        return 1;
    }
    
    // Initialize dp[position][all digits allowed flag][possible first digits]
    int[][][] dp = new int[9][2][10];
    for (int i = 0; i < dp.length; i++) {
        for (int j = 0; j < dp[0].length; j++) {
            for (int k = 0; k < dp[0][0].length; k++) {
                dp[i][j][k] = -1;
            }
        }    
    }
    
    // Create an inverted arraylist of the digits in n
    List<Integer> num = new ArrayList<>();
    while (n > 0) {
        num.add(n%10);
        n /= 10;
    }
    
    // Return numbers which satisfy desired condition in range [0, n]
    return numbersInRange(num, num.size()-1, false, 0, -1, dp);
}

static int numbersInRange(List<Integer> num, int pos, boolean flag, int fd, int ld, int[][][] dp) {
    // If all digits exhausted
    if (pos == -1) {        
        // Check if condition is satisfied
        return (fd == ld) ? 1 : 0;
    }
    
    // If value is pre-computed, return the value
    if (dp[pos][flag?1:0][fd] != -1) {
        return dp[pos][flag?1:0][fd];
    }
    
    // If flag is set to true
    // we can set any digit at current position
    // Else we can go till the current digit
    int limit = flag ? 9 : num.get(pos);
    
    // Track the count of numbers which satisfy the condition
    int count = 0;
    // For all possible digits we can place
    for (int i = 0; i <= limit; i++) {
        // If the current digit is less than the limit 
        if (i < limit) {
            // We can set any digit in the next iteration
            // Update first digit if it is still 0
            // Update last digit to current digit
            count += numbersInRange(num, pos-1, true, (fd == 0) ? i : fd, i, dp);
        } else {
            // Else it will depend on the current flag
            count += numbersInRange(num, pos-1, flag, (fd == 0) ? i : fd, i, dp);
        }
    }
    
    // Return numbers which satisfy desired condition
    // Store the value for future reuse
    return dp[pos][flag?1:0][fd] = count;
}
```