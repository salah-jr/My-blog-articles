---
title: "53. Maximum Subarray"
seoTitle: "53. Maximum Subarray"
seoDescription: "53. Maximum Subarray"
datePublished: Fri Jul 07 2023 09:00:00 GMT+0000 (Coordinated Universal Time)
cuid: clzvtgjs0000008mph30fbplh
slug: leetcode-53-maximum-subarray
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1723758716042/326b8a7d-f37f-4723-9b13-51d159909534.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1723758740197/de6706b2-ccb1-4ea2-9f5b-dfecb866963a.jpeg
tags: leetcode, problem-solving-skills

---

**Problem Statement:**

Given an integer array `nums`, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

**Examples:**

```java
Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
```

```java
codeInput: nums = [1]
Output: 1
```

```java
codeInput: nums = [5,4,-1,7,8]
Output: 23
```

**Approach:**

The problem of finding the maximum subarray sum can be efficiently solved using **Kadane’s Algorithm**. This algorithm works by iterating through the array and maintaining two values:

1. The current subarray sum (`sum`).
    
2. The maximum sum encountered so far (`max`).
    

**Solution:**

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int sum = 0;
        int max = Integer.MIN_VALUE;

        for (int i : nums) {
            sum += i;

            if (sum > max) {
                max = sum;
            }
            
            if (sum < 0) {
                sum = 0;
            }
        }
        return max;
    }
}
```

**Analysis:**

* **Time Complexity:** O(N), where N is the number of elements in the array. The array is traversed only once.
    
* **Space Complexity:** O(1), since the algorithm uses a constant amount of extra space.
    

**Conclusion:**

Kadane’s Algorithm efficiently finds the maximum sum of a contiguous subarray in linear time. It does this by dynamically determining whether to continue with the current subarray or start a new one, ensuring an optimal solution with minimal overhead.

You can find all my solutions at [Github](https://github.com/salah-jr/My-leetCode-solutions/tree/main/src/com/salah)

**Thank you for reading! Stay tuned for the next article in this series, where we'll tackle another exciting LeetCode problem.**