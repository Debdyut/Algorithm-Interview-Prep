
# Binary Search
**Author:** Debdyut Hajra <br/>
**Created date:** 3 January 2022 <br/>
**Last updated:** 3 January 2022 <br/>

**Problem Link:** [704. Binary Search](https://leetcode.com/problems/binary-search/) <br/>
**Tags:** Array, Binary Search

## Problem

Given an integer array sorted in ascending order, find the target element. Return -1 if not found.

**Example:**

**Input:** nums = [-1,0,3,5,9,12], target = 9 <br/>
**Output:** 4 <br/>
**Explanation:** 9 exists in nums and its index is 4 <br/>

**Constraints:**

- `1 <= nums.length <= 10^4`
- `-104 < nums[i], target < 10^4`
- `All the integers in nums are unique.`
- `nums is sorted in ascending order.`

## Solutions

```java
public int search(int[] nums, int target) {
    int n = nums.length;
    // Take the left limit as the starting element of array
    int start = 0;
    // Take the right limit as the last element of array
    int end = n-1;
    // While left pointer does not cross the right pointer
    while (start <= end) {
        // Find the middle element between the left and right pointer
        // We didn't do (start + end) / 2, as it may result in integer overflow
        int mid = start + (end - start) / 2;

        // If the middle element is equal to the target
        // We have found the desired element, return index
        if (nums[mid] == target) {
            return mid;
        }
        // If target lies on left of the middle element
        else if (nums[mid] > target) {
            // Update the right pointer
            end = mid-1;
        } 
        // Else if target lies on left of the middle element
        else {
            // Update the left pointer
            start = mid+1;
        }
    }
    // Target element not found
    return -1;
}
```