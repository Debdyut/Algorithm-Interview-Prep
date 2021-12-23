# Largest Number
**Author:** Debdyut Hajra </br>
**Created date:** 23 December 2021 </br>
**Last updated:** 23 December 2021 </br>

**Problem Link:** [179. Largest Number](https://leetcode.com/problems/largest-number/) </br>
**Tags:** Array, Greedy

## Problem

Given an array of integers, find the maximum number which can be built by arranging the numbers.

**Example:**

**Input:** nums = [10,2]
**Output:** "210"

**Input:** nums = [0,0]
**Output:** "0"

**Constraints:**

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 10^9`

## Solutions

The primary intuition behind the solution is that given 2 integers, we will arrange the integers side by side to form the 2 possible numbers and check which number is greater. Based on this result we will decide which integer will precede the other.

**height:** Input array

```java
public String largestNumber(int[] nums) {
    int n = nums.length;
    // Convert to String array
    String[] arr = new String[n];
    for (int i = 0; i < n; i++) {
        arr[i] = Integer.toString(nums[i]);
    }
    
    // Custum sort
    Arrays.sort(arr, (a, b) -> (-1) * (a+b).compareTo(b+a));
    
    // Build the number
    String res = String.join("", arr);
    
    // If the number is not 0, return result
    return res.startsWith("0") ? "0" : res;
}
```