# Candy
**Author:** Debdyut Hajra </br>
**Created date:** 22 December 2021 </br>
**Last updated:** 22 December 2021 </br>

**Problem Link:** [135. Candy](https://leetcode.com/problems/candy/) </br>
**Tags:** Array, Greedy

## Problem

Given an array of ratings for a number of children, find the total minimum number fo candies to be distributed among the children so that:
1) Each child must get at least 1 candy.
2) If a child has more ratings than his/her adjacent child, then he/she should have more candy than them.

**Example:**

**Input:** ratings = [1,0,2] </br>
**Output:** 5 </br>
**Explanation:** You can allocate to the first, second and third child with 2, 1, 2 candies respectively. </br>

**Constraints:**

- `n == ratings.length`
- `1 <= n <= 2 * 10^4`
- `0 <= ratings[i] <= 2 * 10^4`

## Solutions

We will look at the following solutions:
1. [Greedy (Multiple-pass, O(n) space)](#1-greedy-multiple-pass-O-n-space)
2. [Greedy (One-pass, O(1) space)](#2-greedy-one-pass-O-1-space)

### 1. Greedy (Multiple-pass, O(n) space)

```java
public int candy(int[] ratings) {        
    int n = ratings.length;
    // If array empty, return 0
    if (n == 0) {
        return 0;
    }
    
    // Inititalize candies
    int[] candies = new int[n];    
    for (int i = 0; i < n; i++) {
        candies[i] = 1;
    }
    // Loop towards right from front
    for (int i = 1; i < n; i++) {
        // If rating for current child is greater than previous child        
        if (ratings[i] > ratings[i-1]) {                
            // Give an additional candy to the current child
            // compared to previous child
            candies[i] = candies[i-1] + 1;    
        }
    }
    // Loop towards left from end
    for (int i = n-1; i > 0; i--) {
        // If rating for current child is lesser than previous child 
        if (ratings[i] < ratings[i-1]) {
            // Ensure the previous child receives at least 1 more candy
            // than the current child
            candies[i-1] = Math.max(candies[i-1], candies[i] + 1);
        }
    }
    // Compute the total candies required
    int total = 0;
    for (int i = 0; i < n; i++) {
        total += candies[i];
    }
    // Return the total candies required
    return total;
}
```

### 2. Greedy (One-pass, O(1) space)

```java
public int candy(int[] ratings) {
    int  n = ratings.length;
    // If array empty, return 0
    if (n == 0) {
        return 0;
    }
    
    int total = 1; // Let candy count for first child is 1
    int up = 0; // track count for increasing sequence 
    int down = 0; // track count for decreasing sequence
    int peak = 0; // track the count at peak
    // For each child
    for (int i = 1; i < n; i++) {
        // If rating for current child is greater than previous child  
        if (ratings[i] > ratings[i-1]) {
            // Increment up count
            up++;
            // Reset down count                
            down = 0;
            // Update the count at peak
            peak = up;
            // Update total candies                
            total += 1 + up;
        } 
        // If rating for current child is equal to previous child  
        else if (ratings[i] == ratings[i-1]) {
            // Reset up, down and peak count
            up = down = peak = 0;
            // Update total candies
            total += 1;
        }
        // If rating for current child is lesser than previous child 
        else {
            // Reset up count
            up = 0;
            // Increment down count
            down++;
            // Update total candies
            // If peak becomes less than down
            // then we need to add 1 to peak    
            total += down + (peak >= down ? 0 : 1);
        }
    }
    // Return the total candies required
    return total;
}
```