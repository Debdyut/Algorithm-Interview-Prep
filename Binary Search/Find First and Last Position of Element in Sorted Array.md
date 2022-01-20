
# Find First and Last Position of Element in Sorted Array
**Author:** Debdyut Hajra <br/>
**Created date:** 14 January 2022 <br/>
**Last updated:** 14 January 2022 <br/>

**Problem Link:** [34. Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/) <br/>
**Tags:** Array, Binary Search

## Problem

Given an integer array sorted in ascending order and containing duplicate elements, find the first and last occurances of a target element. Return { -1, -1 } if target not found.

**Example:**

**Input:** nums = [5,7,7,8,8,10], target = 8 <br/>
**Output:** [3,4]

**Constraints:**

- `0 <= nums.length <= 10^5`
- `-10^9 <= nums[i] <= 10^9`
- `nums is a non-decreasing array.`
- `-10^9 <= target <= 10^9`

## Solutions

```java
public int[] searchRange(int[] nums, int target) {
    // Initialize the result array
    int[] res = { -1, -1 };
    int n = nums.length;
    // Retrieve the first occurance
    res[0] = getFirstIdx(nums, n, target);
    // If target is not found, return default value
    if (res[0] == -1) {
        return res;
    }
    // Retrieve the last occurance
    res[1] = getLastIdx(nums, n, target);
    // Return result
    return res;
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