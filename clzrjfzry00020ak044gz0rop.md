---
title: "3. Longest Substring Without Repeating Characters"
seoTitle: "3. Longest Substring Without Repeating Characters"
seoDescription: "3. Longest Substring Without Repeating Characters"
datePublished: Tue Aug 09 2022 10:00:00 GMT+0000 (Coordinated Universal Time)
cuid: clzrjfzry00020ak044gz0rop
slug: leetcode-3-longest-substring-without-repeating-characters
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1723500022663/450f7ef1-d5a7-4845-8f41-d4b4d5b599c5.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1723500031335/5e69dc50-c533-45c7-9ed1-ffe77cfc275e.jpeg
tags: leetcode, problem-solving-skills

---

**Problem Statement:**

Given a string `s`, find the length of the longest substring without repeating characters.

**Examples:**

```plaintext
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```

```plaintext
plaintextCopy codeInput: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

```plaintext
plaintextCopy codeInput: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
             Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

**Approach:**

To solve this problem, we can use the **sliding window** technique combined with a set to keep track of the characters in the current window. The idea is to expand the window by moving the right pointer (`j`) until a duplicate character is found. Then, move the left pointer (`i`) to remove characters until the duplicate is eliminated, thus maintaining a substring without repeating characters.

1. **Edge Cases:** First, handle cases where the string length is 0 or 1, as these can be returned immediately.
    
2. **Sliding Window Technique:**
    
    * Use a `Set` to store characters in the current window.
        
    * Start with both pointers (`i` and `j`) at the beginning of the string.
        
    * Expand the window by moving `j` and adding characters to the set.
        
    * If a duplicate character is found, move `i` to shrink the window until the duplicate is removed.
        
    * Keep track of the maximum length of the substring without repeating characters.
        

**Solution:**

```java
import java.util.HashSet;
import java.util.Set;

class Solution {
    public int lengthOfLongestSubstring(String s) {
        if (s.length() == 1) return 1;
        if (s.length() == 0) return 0;

        Set<Character> set = new HashSet<>();
        int i = 0, j = 0, max = 0;
        
        while (j < s.length()) {
            if (set.add(s.charAt(j))) {
                max = Math.max(set.size(), max);
                j++;
            } else {
                set.remove(s.charAt(i));
                i++;
            }
        }

        return max;
    }
}
```

**Analysis:**

* **Time Complexity:** O(N), where NNN is the length of the string. Both `i` and `j` traverse the string once, so the algorithm runs in linear time.
    
* **Space Complexity:** O(min(N,M)), where N is the length of the string and M is the size of the character set. The space is used by the set to store characters of the substring.
    

**Conclusion:**

This solution efficiently finds the length of the longest substring without repeating characters using the sliding window technique. It handles edge cases and ensures that the window is adjusted dynamically to maintain the uniqueness of characters. The algorithm runs in linear time and uses minimal extra space, making it an optimal solution for this problem.

You can find all my solutions at [Github](https://github.com/salah-jr/My-leetCode-solutions/tree/main/src/com/salah)

**Thank you for reading! Stay tuned for the next article in this series, where we'll tackle another exciting LeetCode problem.**