# Gas Station
**Author:** Debdyut Hajra </br>
**Created date:** 22 December 2021 </br>
**Last updated:** 22 December 2021 </br>

**Problem Link:** [134. Gas Station](https://leetcode.com/problems/gas-station/) </br>
**Tags:** Array, Greedy

## Problem

Given the gas at a station and cost to reach next station, find the starting point such that moving in a clockwise direction we can return to the same point traversing all points in sequence.

**Example:**

**Input:** gas = [1,2,3,4,5], cost = [3,4,5,1,2] </br>
**Output:** 3 </br>
**Explanation:** </br>
Start at station 3 (index 3) and fill up with 4 unit of gas. Your tank = 0 + 4 = 4 </br>
Travel to station 4. Your tank = 4 - 1 + 5 = 8 </br>
Travel to station 0. Your tank = 8 - 2 + 1 = 7 </br>
Travel to station 1. Your tank = 7 - 3 + 2 = 6 </br>
Travel to station 2. Your tank = 6 - 4 + 3 = 5 </br>
Travel to station 3. The cost is 5. Your gas is just enough to travel back to station 3. </br>
Therefore, return 3 as the starting index. </br>

**Constraints:**

- `gas.length == n`
- `cost.length == n`
- `1 <= n <= 105`
- `0 <= gas[i], cost[i] <= 104`

## Solutions

The primary intuition behind the solution is that:
1) Starting from point A we cannot reach point B, implies no point between A and B can reach B.
2) If total gas is greater than or equal to total cost, then a solution must exist.

**height:** Input array

```java
public int canCompleteCircuit(int[] gas, int[] cost) {
    int n = gas.length;        
    int totalGas = 0;
    int totalCost = 0; 
    int tank = 0;
    int start = 0;
    // For each array element
    for (int i = 0; i < n; i++) {
        // Compute total gas
        totalGas += gas[i];
        // Compute total cost
        totalCost += cost[i];
        // Compute current gas in tank
        tank += gas[i] - cost[i];
        // If gas in tank is empty 
        // The current node cannot be reached from the previous start point
        if (tank < 0) {
            // Update start point to the next index
            start = i+1;
            // Set gas in tank to empty
            tank = 0;
        }
    }        
    // If total gas is less than total cost
    // then there is no solution
    if (totalGas < totalCost) {
        return  -1;
    }
    // Return the desired start point
    return start;
}
```