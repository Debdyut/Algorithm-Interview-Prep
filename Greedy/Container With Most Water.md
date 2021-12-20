# Container With Most Water
**Author:** Debdyut Hajra </br>
**Created date:** 19 December 2021 </br>
**Last updated:** 19 December 2021 </br>

**Problem Link:** [Container With Most Water](https://leetcode.com/problems/container-with-most-water/) </br>
**Tags:** Array, Dynamic Programming

## Problem

Given an height array, find the maximum volume which can be created by any 2 height elements and the corresponding distance between them.

**Example:**

**Input:** height = [1,8,6,2,5,4,8,3,7] </br>
**Output:** 49 </br>
**Explanation:** The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.

**Constraints:**

- `n == height.length`
- `2 <= n <= 105`
- `0 <= height[i] <= 104`

## Solutions

The primary intuition behind the solution is that for the volume created by using middle elements, to become greater than that created by relatively outer elements, should be enclosed by compartively larger height boundaries. So we start by taking 2 pointers at the 2 extremes of the array, and move inwards. At each step we calculate the resultant volume and keep track of the maximum volume encountered. Next we check which pointer is pointing to the smaller boundary, and move the subsequent pointer inwards.

**height:** Input array

```java
public int maxArea(int[] height) {
    // Length of array
    int n = height.length;
    
    // Take 2 pointers
    // Place first pointer on first element
    // Place the other pointer on the last element
    int i = 0;
    int j = n - 1;
    
    int res = Integer.MIN_VALUE;
    // While the first pointer does not cross the second pointer
    // If they overlap, the volume of water will be zero
    while ( i < j) {
        // Compute the volume of water for the current positions
        int tmp = Math.min(height[i], height[j]) * (j - i);

        // Store the max of the already computed value
        // and the current value
        res = Math.max(res, tmp);
        
        // If the first pointer points to smaller height
        if (height[i] < height[j]) {
            // Move the first pointer to the left
            i++;
        } else {
            // Move the second pointer to the right
            j--;
        }
    }
    // Return the maximum volume possible
    return res;
}
```