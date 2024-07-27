---
title: "13. Roman to Integer"
seoTitle: "13. Roman to Integer"
seoDescription: "13. Roman to Integer"
datePublished: Sun Aug 08 2021 10:00:00 GMT+0000 (Coordinated Universal Time)
cuid: clz35xaf0000608mc14gketkl
slug: 13-roman-to-integer
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1722026174057/77142000-80a3-42ca-a4bc-54cb83cde7e4.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1722026168375/c02a7dca-f0ba-4c64-bb57-8955282e6577.jpeg
tags: leetcode, problem-solving-skills

---

**Problem Statement:**

Convert a Roman numeral to an integer. Roman numerals are represented by seven different symbols: I, V, X, L, C, D, and M. For example, `2` is written as `II` in Roman numeral, just two ones added together. The Roman numeral `12` is written as `XII`, which is simply `X + II`. The number `27` is written as `XXVII`, which is `XX + V + II`.

**Examples:**

```java
Input: s = "III"
Output: 3
Explanation: III = 3

Input: s = "LVIII"
Output: 58
Explanation: L = 50, V= 5, III = 3.

Input: s = "MCMXCIV"
Output: 1994
Explanation: M = 1000, CM = 900, XC = 90, and IV = 4.
```

**Approach:**

To convert a Roman numeral to an integer, we iterate through the string from the end to the beginning, mapping each Roman character to its corresponding value. If a smaller value precedes a larger value, it is subtracted from the total; otherwise, it is added.

Here's a detailed explanation of the approach:

1. **Mapping Roman Numerals to Values:**
    
    * Each Roman numeral character is mapped to its integer value (e.g., 'I' -&gt; 1, 'V' -&gt; 5, etc.).
        
2. **Iteration and Calculation:**
    
    * Start from the end of the string and work backward.
        
    * For each character, determine its integer value.
        
    * If this value is less than one-fourth of the running total (`4 * num < ans`), subtract it from the total. This handles cases like `IV` (4), where `I` (1) is subtracted from `V` (5).
        
    * Otherwise, add the value to the total.
        

**Solution:**

```java
class Solution {
    public int romanToInt(String s) {
        int ans = 0, num = 0;

        for (int i = s.length() - 1; i >= 0; i--) {
            switch(s.charAt(i)) {
                case 'I': num = 1; break;
                case 'V': num = 5; break;
                case 'X': num = 10; break;
                case 'L': num = 50; break;
                case 'C': num = 100; break;
                case 'D': num = 500; break;
                case 'M': num = 1000; break;
            }
            
            if (4 * num < ans) ans -= num;
            else ans += num;
        }

        return ans;
    }
}
```

**Analysis:**

* **Time Complexity:** O(n), where n is the length of the string. We traverse the string once.
    
* **Space Complexity:** O(1), as we are using a constant amount of extra space.
    

**Optimization:**

This solution is already optimized for both time and space. The use of a switch-case statement for mapping Roman numerals to their values ensures efficient lookups.

**Conclusion:**

This solution converts a Roman numeral to an integer by iterating through the string from end to start, adding or subtracting values based on their position relative to other values. This method ensures an accurate and efficient conversion.

You can find all my solutions at [Github](https://github.com/salah-jr/My-leetCode-solutions/tree/main/src/com/salah)

**Thank you for reading! Stay tuned for the next article in this series, where we'll tackle another exciting LeetCode problem.**