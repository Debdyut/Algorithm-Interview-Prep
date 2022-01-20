
# 33. Search in Rotated Sorted Array
**Author:** Debdyut Hajra <br/>
**Created date:** 16 January 2022 <br/>
**Last updated:** 16 January 2022 <br/>

**Problem Link:** [33. Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/) <br/>
**Tags:** Array, Binary Search

## Problem

Given an integer array sorted in ascending order and rotated, find the target element.

**Example:**

**Input:** nums = [4,5,6,7,0,1,2], target = 0 <br/>
**Output:** 4

**Constraints:**

- `1 <= nums.length <= 5000`
- `-10^4 <= nums[i] <= 10^4`
- `All values of nums are unique.`
- `nums is an ascending array that is possibly rotated.`
- `-10^4 <= target <= 10^4`

## Solutions

### 1. Double pass

```java
public int search(int[] nums, int target) {
    int n = nums.length;
    // Retrieve the position of the minimum element
    int minElementIdx = getMinElementIdx(nums);
    // If target is greater than the last element
    if (target > nums[n-1]) {
        // target may lie on the left
        return binarySearch(nums, target, 0, minElementIdx-1);
    } else {
        // target may lie on the rigth
        return binarySearch(nums, target, minElementIdx, n-1);
    }
}

int binarySearch(int[] nums, int target, int start, int end) {
    // While left pointer does not cross the right pointer
    if (start > end) {
        return -1;
    }
    // Find the middle element between the left and right pointer
    // We didn't do (start + end) / 2, as it may result in integer overflow
    int mid = start + (end - start) / 2;
    // If the middle element is equal to the target
    // We have found the desired element, return index
    if (nums[mid] == target) {
        return mid;
    }
    // If target lies on right of the middle element
    else if (nums[mid] < target) {        
        // Update the left pointer
        return binarySearch(nums, target, mid+1, end);
    } 
    // Else if target lies on left of the middle element
    else {
        // Update the right pointer
        return binarySearch(nums, target, start, mid-1);    
    }
}

int getMinElementIdx(int[] nums) {
    int n = nums.length;
    // Base case
    if (n == 1) {
        return 0;
    }

    // Take the left limit as the starting element of array
    int start = 0;
    // Take the right limit as the last element of array
    int end = n-1;
    // While left pointer does not cross the right pointer
    while (start <= end) {
        // Find the middle element between the left and right pointer
        // We didn't do (start + end) / 2, as it may result in integer overflow
        int mid = start + (end - start) / 2;
        // Find the element on the left of the current mid element,
        // wrap the index if required.
        int prev = (mid - 1 + n) %n;
        // If the mid element is less than the element on its left,
        // it is the result
        if (nums[mid] < nums[prev]) {
            return mid;
        } 
        // If the mid element is less than the last element in array
        // the min element should be on the left
        else if (nums[mid] < nums[n-1]) {
            end = mid-1;
        } 
        // Min element on the right
        else {
            start = mid+1;
        }
    }
    return -1;
}
```

### 2. Single pass

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
        // We have found the desired element, return true
        if (nums[mid] == target) {
            return mid;
        }
        // If all elements are sorted to the left of mid
        if (nums[start] <= nums[mid]) {
            // Check if target lies on the left of mid
            if (nums[start] <= target && target <= nums[mid]) {
                end = mid-1;
            }
            else {
                start = mid+1;
            }
        } else {
            if (nums[mid] <= target && target <= nums[n-1]) {
                start = mid+1;
            } else {
                end = mid-1;
            }
        }
    }
    // Element not found
    return -1;
}
```