
# Number of occurrence
**Author:** Debdyut Hajra <br/>
**Created date:** 14 January 2022 <br/>
**Last updated:** 14 January 2022 <br/>

**Problem Link:** [Number of occurrence](https://practice.geeksforgeeks.org/problems/number-of-occurrence2259/1) <br/>
**Tags:** Array, Binary Search

## Problem

Given an integer array sorted in ascending order and containing duplicate elements, find the number of occurances of a target element.

**Example:**

**Input:** <br/>
N = 7, X = 2 <br/>
Arr[] = {1, 1, 2, 2, 2, 2, 3} <br/>
**Output:** 4 <br/>
**Explanation:** 2 occurs 4 times in the given array.

**Constraints:**

- `1 ≤ N ≤ 10^5`
- `1 ≤ Arr[i] ≤ 10^6`
- `1 ≤ X ≤ 10^6`

## Solutions

```java
int count(int[] arr, int n, int x) {
    // Retrieve the first occurance
    int firstIdx = getFirstIdx(arr, n, x);
    // If target element is not found, return count as 0
    if (firstIdx == -1) {
        return 0;
    }
    // Retrieve the last occurance
    int lastIdx = getLastIdx(arr, n, x);
    // Calculate and return count
    return lastIdx - firstIdx + 1;
}

private int getFirstIdx (int[] nums, int n, int target) {
    // Take the left limit as the starting element of array
    int start = 0;
    // Take the right limit as the last element of array
    int end = n-1;
    // Track the first occurance, defaults to -1 if target not found
    int firstIdx = -1;
    // While left pointer does not cross the right pointer
    while (start <= end) {
        // Find the middle element between the left and right pointer
        // We didn't do (start + end) / 2, as it may result in integer overflow
        int mid = start + (end - start) / 2;
        // If the middle element is equal to the target
        // set this element as the first occurance
        // and continue checking on the left
        if (nums[mid] == target) {
            firstIdx = mid;
            end = mid - 1;
        } 
        // If target lies on left of the middle element
        else if (nums[mid] > target) {
            // Update the right pointer
            end = mid - 1;
        }
        // Else if target lies on right of the middle element
         else {
            // Update the left pointer
            start = mid + 1;
        }
    }
    // Return the first occurance of the target element
    return firstIdx;
}

private int getLastIdx (int[] nums, int n, int target) {
    // Take the left limit as the starting element of array
    int start = 0;
    // Take the right limit as the last element of array
    int end = n-1;
    // Track the last occurance, defaults to -1 if target not found
    int lastIdx = -1;
    // While left pointer does not cross the right pointer
    while (start <= end) {
        // Find the middle element between the left and right pointer
        // We didn't do (start + end) / 2, as it may result in integer overflow
        int mid = start + (end - start) / 2;
        // If the middle element is equal to the target
        // set this element as the last occurance
        // and continue checking on the right
        if (nums[mid] == target) {
            lastIdx = mid;
            start = mid + 1;
        } 
        // If target lies on left of the middle element
        else if (nums[mid] > target) {
            end = mid - 1;
        } 
        // Else if target lies on right of the middle element
        else {
            // Update the left pointer
            start = mid + 1;
        }
    }
    // Return the last occurance of the target element
    return lastIdx;
}
```