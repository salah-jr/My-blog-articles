---
title: "77. Combinations"
seoTitle: "77. Combinations"
seoDescription: "77. Combinations"
datePublished: Thu Sep 14 2023 09:00:00 GMT+0000 (Coordinated Universal Time)
cuid: clzyocz6c00010ame5gl978dm
slug: 77-combinations
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1723931573798/627b8f20-97bb-4812-84aa-21c8a916f7db.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1723931583509/afaf39b0-fa60-4658-8f4f-b1b68a681eb1.jpeg
tags: leetcode, problem-solving-skills

---

### Problem Statement

Given two integers `n` and `k`, return all possible combinations of `k` numbers chosen from the range `[1, n]`.

**Example:**

```plaintext
Input: n = 4, k = 2
Output: 
[
  [1,2],
  [1,3],
  [1,4],
  [2,3],
  [2,4],
  [3,4]
]
```

```plaintext
Input: n = 1, k = 1
Output: 
[
  [1]
]
```

### Approach

The problem requires generating all possible combinations of `k` numbers selected from the range `[1, n]`. This can be efficiently solved using a **backtracking** approach, which is well-suited for problems that involve exploring all possible subsets or combinations.

#### Backtracking Explanation:

1. **Recursive Function:**
    
    * The function `combine` recursively builds combinations by iterating through numbers starting from `start` to `n`.
        
    * For each number `i`, we add it to the current combination and then recursively call `combine` with updated parameters to generate further combinations.
        
2. **Base Case:**
    
    * When `k` (the number of elements left to pick) reaches 0, the current combination is complete, and we add it to the result list.
        
3. **Backtracking:**
    
    * After generating combinations that include the current number `i`, we remove it from the current combination (`comb.remove(comb.size() - 1)`) before the next iteration. This ensures we explore all possible combinations.
        

### Solution

```java
class Solution {
    public static List<List<Integer>> combine(int n, int k) {
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        combine(result, new ArrayList<Integer>(), 1, n, k);
        return result;
    }

    public static void combine(List<List<Integer>> result, List<Integer> comb, int start, int n, int k) {
        if (k == 0) {
            result.add(new ArrayList<Integer>(comb));
            return;
        }
        
        for (int i = start; i <= n; i++) {
            comb.add(i);
            combine(result, comb, i + 1, n, k - 1);
            comb.remove(comb.size() - 1);
        }
    }
}
```

### Analysis

* **Time Complexity:** The time complexity is O((n/k)⋅k). The number of combinations is (n/k) (n choose k), and for each combination, we spend O(k) time copying it to the result list.
    
* **Space Complexity:** O(k), where `k` is the depth of the recursive call stack plus the space required to store the result list.
    

### Conclusion

The backtracking method efficiently generates all combinations by exploring each potential combination one step at a time, ensuring that all valid combinations are included in the result list. This approach is optimal for solving problems involving combinations, permutations, or subsets.

You can find all my solutions at [Github](https://github.com/salah-jr/My-leetCode-solutions/tree/main/src/com/salah)

**Thank you for reading! Stay tuned for the next article in this series, where we'll tackle another exciting LeetCode problem.**