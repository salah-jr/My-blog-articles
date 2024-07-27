---
title: "1. Two Sum"
seoTitle: "Leetcode-Two-Sum-problem"
datePublished: Fri Jul 02 2021 10:00:00 GMT+0000 (Coordinated Universal Time)
cuid: clz1s4amu00050ajr6roq4706
slug: leetcode-two-sum
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1721945617212/93b5a8dc-d460-4d15-8049-08482dae5463.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1721945631489/c9060730-ea1d-469d-aa78-34efe70a2f0d.jpeg
tags: leetcode, problem-solving-skills, two-sum-problem

---

**Problem Statement:**

Given an array of integers `nums` and an integer `target`, return the indices of the two numbers such that they add up to `target`. Assume that each input has exactly one solution, and you may not use the same element twice. You can return the answer in any order.

**Example:**

```java
Input: nums = [2, 7, 11, 15], target = 9 
Output: [0, 1] 
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].
```

**Approach:**

To solve this problem efficiently, we use a hash map to store the difference between the target and each element as a key and the element's array index as a value. As we iterate through the array, we check if the current element exists in the hash map as a key. If it does, we have found the two indices (the current one and the value of this key in the map) that sum up to the target.

**Solution:**

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        HashMap<Integer,Integer> map = new HashMap<>();

        for (int i = 0; i < nums.length; i++) {
            if (map.containsKey(nums[i])) {
                return new int[]{map.get(nums[i]), i};
            }

            int remain = target - nums[i];

            map.put(remain , i);
        }

        return nums; // Ideally, this line should never be reached since there is exactly one solution.
    }
}
```

**Analysis:**

* **Time Complexity:** O(n), where n is the length of the array. We traverse the list containing n elements exactly once.
    
* **Space Complexity:** O(n), where n is the number of elements in the hash map.
    

**Optimization:**

This solution is already optimized with a linear time complexity due to the use of a hash map for constant time lookups and insertions.

**Conclusion:**

By using a hash map, we efficiently solve the Two Sum problem in linear time. This method ensures that we only pass through the array once, providing a quick and effective solution.

You can find all my solutions at [Github](https://github.com/salah-jr/My-leetCode-solutions/tree/main/src/com/salah)

**Thank you for reading! Stay tuned for the next article in this series, where we'll tackle another exciting LeetCode problem.**