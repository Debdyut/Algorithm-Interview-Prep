
# Search Insert Position
**Author:** Debdyut Hajra <br/>
**Created date:** 16 January 2022 <br/>
**Last updated:** 16 January 2022 <br/>

**Problem Link:** [35. Search Insert Position](https://leetcode.com/problems/search-insert-position/) <br/>
**Tags:** Array, Binary Search

## Problem

Given an integer array sorted in ascending order, find the element position or the position where it can be inserted to keep the array sorted.

**Example:**

**Input:** nums = [1,3,5,6], target = 5 <br/>
**Output:** 2

**Constraints:**

- `1 <= nums.length <= 10^4`
- `-10^4 <= nums[i] <= 10^4`
- `nums contains distinct values sorted in ascending order.`
- `-10^4 <= target <= 10^4`

## Solutions

```java
public int searchInsert(int[] nums, int target) {
    int n = nums.length;
    // Take the left limit as the starting element of array
    int start = 0;
    // Take the right limit as the last element of array
    int end = n-1;
    // Track the ceil of the target element, defaults to end of array
    int ceil = n;
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
            // Update the ceil of target element
            ceil = mid;
            // Update the right pointer
            end = mid-1;
        }
        // Else if target lies on right of the middle element
        else {
            // Update the left pointer
            start = mid+1;
        }
    }
    // Return the position where the element can be inserted
    return ceil;
}
```