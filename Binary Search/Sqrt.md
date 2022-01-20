
# Sqrt(x)
**Author:** Debdyut Hajra <br/>
**Created date:** 17 January 2022 <br/>
**Last updated:** 17 January 2022 <br/>

**Problem Link:** [69. Sqrt(x)](https://leetcode.com/problems/sqrtx/) <br/>
**Tags:** Array, Binary Search

## Problem

Given a non-negative integer, compute the approximate(0 decimal places) square root of the number.

**Example:**

**Input:** x = 8 <br/>
**Output:** 2 <br/>
**Explanation:** The square root of 8 is 2.82842..., and since the decimal part is truncated, 2 is returned.

**Constraints:**

- `0 <= x <= 2^31 - 1`

## Solutions

```java
public int mySqrt(int x) {
    // Base cases
    if (x == 0 || x == 1) {
        return x;
    }
    // Precision of result
    double epsilon = 1e-7;
    
    // Min possible value
    double start = 0;
    // Max possible value
    double end = x;     
    // While the answer is not present within accepatable precision value
    while (Math.abs(end - start) > epsilon) {
        // Find the mid element in the current range
        double mid = start + (end - start) / 2;
        // Compute the square for the current number
        double sq = mid * mid;            
        // If the current square value matches the given number
        if (sq == x) {
            // Return the result
            return (int) mid;
        }
        // If current square exceeds the given number
        else if (sq > x) {
            // The square root will lie on the left of the mid
            end = mid;
        }
        else {                
            // The square root lies on the right of the mid
            start = mid;
        }
    }
    // Return the result
    return (int) end;
}
```