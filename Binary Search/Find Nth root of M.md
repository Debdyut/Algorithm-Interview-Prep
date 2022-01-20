
# Find Nth root of M
**Author:** Debdyut Hajra <br/>
**Created date:** 17 January 2022 <br/>
**Last updated:** 17 January 2022 <br/>

**Problem Link:** [Find Nth root of M](https://practice.geeksforgeeks.org/problems/find-nth-root-of-m5843/1#) <br/>
**Tags:** Array, Binary Search

## Problem

Given a non-negative integer, compute the nth root of the number. If the nth root is not an integer return -1.

**Example:**

**Input:** n = 2, m = 9 <br/>
**Output:** 3 <br/>
**Explanation:** 32 = 9

**Constraints:**

- `1 <= n <= 30`
- `1 <= m <= 10^9`

## Solutions

```java
public int NthRoot(int n, int m) {
    // Base case
    if (m == 0 || m == 1) {
        return m;
    }
    
    // Min possible value
    int start = 1;
    // Max possible value
    int end = m;
    // While the start does not exceed end
    while (start <= end) {
        // Compute mid of the range
        int mid = start + (end - start) / 2;
        // Calculate the nth power of mid
        long tmp = pow(mid, n, m);
        // If the current nth power is equal to the target number
        if (tmp == m) {
            // Return result
            return mid;        
        }
        // If current nth power is greater than target
        else if (tmp == -1) {
            // nth root of the number may lie on the left of mid
            end = mid - 1;
        } else {
            // nth root of the number may lie on the right of mid
            start = mid + 1;
        }
    }
    return -1;
}

long pow(int x, int n, int m) {
    long res = 1;
    // Multiply x for n times
    while(n > 0) {
        res *= x;
        n--;
        // If res is greater than m, return -1
        // to indicate tmp is greater than m
        if (res > m) {
            return -1;
        }
    }
    // return nth power of x
    return res;
}
```