# Remove Duplicate Letters
**Author:** Debdyut Hajra </br>
**Created date:** 24 December 2021 </br>
**Last updated:** 24 December 2021 </br>

**Problem Link:** [316. Remove Duplicate Letters](https://leetcode.com/problems/remove-duplicate-letters/) </br>
**Tags:** String, Stack, Greedy, Monotonic Stack

## Problem

Given a string, find the lexicographically smallest string by removing the duplicate occurances of the characters in the string.

**Example:**

**Input:** s = "bcabc" </br>
**Output:** "abc"

**Constraints:**

- `1 <= s.length <= 10^4`
- `s consists of lowercase English letters.`

## Solutions

We will look at the following solutions:
1. [Stack](#1-stack)
2. [Greedy](#2-greedy)

### 1. Stack

The primary intuition behind the solution is that at each step we will use the character if we have not already used it. We will delete all characters which are lexicographically larger than the current character and have occurances in the subsequent part of the string. If we are removing a character from the result, we will reset its status to not used.

```java
public String removeDuplicateLetters(String s) {
    // Count the occurances of all characters in the string
    int[] freq = new int[26];
    for (char c : s.toCharArray()) {
        freq[c - 'a'] += 1; 
    }
    // Track if a character is already used
    boolean[] visited = new boolean[26];
    // Build the result
    Stack<Character> st = new Stack<>();
    // Foreach character in the string
    for (char c : s.toCharArray()) {
        // character index scaled to 26 characters
        int i = c - 'a';
        // Reduce the count of the character encountered
        freq[i]--;
        // Skip if the character has already been used
        if (visited[i]) {
            continue;
        }
        // For all characters in the stack which are
        // lexicographically larger than the current character and
        // which have occurances remaining in the next part of the string
        while (!st.empty() && st.peek() > c && freq[st.peek() - 'a'] > 0) {
            // Remove the character
            char tmp = st.pop();   
            // Reset the status of the character to not used     
            visited[tmp - 'a'] = false;
        }       
        // Add the current character to the stack     
        st.push(c);
        // Set it to used
        visited[i] = true;
    }
    // Generate the string result from the stack
    StringBuilder sb = new StringBuilder();
    while (!st.empty()) {
        sb.insert(0, st.pop());    
    }
    // Return the result
    return sb.toString();
}
```

### 2. Greedy

At each step we will find the lexicographically smallest character and add it to result. Then we will remove all occurances of the character from the string and repeat the former step till we find the entire result. There will be at max 26 recursive calls, hence still time complexity is O[n].

```java
public String removeDuplicateLetters(String s) {
    // Count the occurances of all characters in the string
    int[] freq = new int[26];
    for (char c : s.toCharArray()) {
        freq[c - 'a'] += 1; 
    }
    // Let 0 be the position for the smallest character
    int pos = 0; 
    // Foreach character in the string
    for (int i = 0; i < s.length(); i++) {
        // If the character at pos is
        // lexicographically larger than the current character
        if (s.charAt(i) < s.charAt(pos)) {
            // Update pos to the current character 
            pos = i;
        }
        // If no more occurances of a particular character is remaining
        if (--freq[s.charAt(i) - 'a'] == 0) {
            // Exit loop
            break;
        }
    }

    String res = null;
    // If string is empty, return empty string
    if (s.length() == 0) {
        res = "";
    } else {
        // Use the minimum value character
        res = s.charAt(pos);
        // Move to next position
        String tmp = s.substring(pos + 1);
        // Remove all occurances of the character used
        tmp = tmp.replaceAll("" + s.charAt(pos), "");
        // Recursively get the result for the subsequent part of string
        res += removeDuplicateLetters(tmp);
    }

    // Return result
    return res;
}
```