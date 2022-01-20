
# Search in Rotated Sorted Array II
**Author:** Debdyut Hajra <br/>
**Created date:** 16 January 2022 <br/>
**Last updated:** 16 January 2022 <br/>

**Problem Link:** [81. Search in Rotated Sorted Array II](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/) <br/>
**Tags:** Array, Binary Search

## Problem

Given an integer array containing duplicate elements, sorted in ascending order and rotated, find the target element.

**Example:**

**Input:** nums = [2,5,6,0,0,1,2], target = 0 <br/>
**Output:** true

**Constraints:**

- `1 <= nums.length <= 5000`
- `-10^4 <= nums[i] <= 10^4`
- `nums is guaranteed to be rotated at some pivot.`
- `-10^4 <= target <= 10^4`

## Solutions

```java
public boolean search(int[] nums, int target) {
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
        // We have found the desired element, return true
        if (nums[mid] == target) {
            return true;
        }
        // If element at start, mid and end are same, decrement end
        if (nums[start] == nums[mid] && nums[mid] == nums[end]) {
            end--;
        }
        // If all elements are sorted to the left of mid
        else if (nums[start] <= nums[mid]) {
            // Check if target lies on the left of mid
            if (nums[start] <= target && target <= nums[mid]) {                
                end = mid-1;
            } else {
                start = mid+1;
            }
        } else {
            if (nums[mid] <= target && target <= nums[end]) {
                start = mid+1;
            } else {
                end = mid-1;
            }
        }
    }
    // Element not found
    return false;
}
```