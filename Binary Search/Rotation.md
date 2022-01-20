
# Rotation
**Author:** Debdyut Hajra <br/>
**Created date:** 16 January 2022 <br/>
**Last updated:** 16 January 2022 <br/>

**Problem Link:** [Rotation](https://practice.geeksforgeeks.org/problems/rotation4723/1#) <br/>
**Tags:** Array, Binary Search

## Problem

Given an integer array sorted in ascending order and rotated, find the number of right rotations.

**Example:**

**Input:** <br/>
N = 5 <br/>
Arr[] = {5, 1, 2, 3, 4} <br/>
**Output:** 1 <br/>
**Explanation:** The given array is 5 1 2 3 4. <br/>
The original sorted array is 1 2 3 4 5. <br/>
We can see that the array was rotated 1 times to the right. <br/>

**Constraints:**

- `1 <= N <=10^5`
- `1 <= Arri <= 10^7`

## Solutions

```java
int findKRotation(int arr[], int n) {
    // Base case
    if (n == 0 || n == 1) {
        return 0;
    }
    
    // Take the left limit as the starting element of array
    int start = 0;
    // Take the right limit as the last element of array
    int end = n-1;
    // Track the minimum element, defaults to -1 if target not found
    int min = -1;
    // While left pointer does not cross the right pointer
    while (start <= end) {
        // Find the middle element between the left and right pointer
        // We didn't do (start + end) / 2, as it may result in integer overflow
        int mid = start + (end - start) / 2;
        // Find the element on the left of the current mid element,
        // wrap the index if required.
        int prevPos = (mid - 1 + n) %n;
        // If the mid element is less than the element on its left,
        // it is the result
        if (arr[mid] < arr[prevPos]) {
            // Set mid as min and return
            min = mid;
            break;
        }
        // If the mid element is less than the last element in array
        // the min element should be on the left
        else if (arr[mid] < arr[n-1]) {
            end = mid-1;
        }
        // Min element on the right
        else {
            start = mid+1;
        }
    }
    // Return the min element index
    return min;
}
```