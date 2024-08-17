---
title: "167. Two Sum II - Input Array Is Sorted"
seoTitle: "167. Two Sum II - Input Array Is Sorted"
seoDescription: "167. Two Sum II - Input Array Is Sorted"
datePublished: Fri May 19 2023 09:00:00 GMT+0000 (Coordinated Universal Time)
cuid: clzxajcp400010al7dfu3gu9g
slug: 167-two-sum-ii-input-array-is-sorted
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1723847897025/8433a493-959e-4d32-ad2d-da78ee907437.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1723847904108/50d07e23-4e28-488f-91ab-8b2668ac6002.jpeg
tags: leetcode, problem-solving-skills

---

### Problem Statement

Given a **1-indexed** array of integers `numbers` that is already sorted in non-decreasing order, find two numbers such that they add up to a specific `target` number. Return the indices of the two numbers (1-indexed) as an integer array `answer` of size 2, where `1 <= answer[0] < answer[1] <= numbers.length`.

The tests are guaranteed to have exactly one solution, and you may not use the same element twice.

The solution must use only constant extra space.

```java
Input: numbers = [2,7,11,15], target = 9
Output: [1,2]
Explanation: The sum of 2 and 7 is 9. Therefore, index 1 and index 2 are returned.
```

```java
codeInput: numbers = [2,3,4], target = 6
Output: [1,3]
```

```java
codeInput: numbers = [-1,0], target = -1
Output: [1,2]
```

### Approach

This problem is a classic example where the **two-pointer** technique is highly effective. Given that the array is sorted, we can use two pointers to scan the array from both ends towards the center:

1. **Initialization:**
    
    * Start with two pointers: `l` (left) at the beginning of the array and `r` (right) at the end of the array.
        
2. **Sum Calculation:**
    
    * Calculate the sum of the elements at the `l` and `r` pointers.
        
3. **Adjust Pointers:**
    
    * If the sum is greater than the `target`, it means we need a smaller sum, so we move the `r` pointer left.
        
    * If the sum is less than the `target`, we move the `l` pointer right to increase the sum.
        
4. **Termination:**
    
    * The loop continues until the sum equals the target. Since there is always exactly one solution, the loop is guaranteed to terminate with the correct answer.
        

### Solution

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {    
        int l = 0, r = numbers.length - 1;
        while (numbers[l] + numbers[r] != target) {
            if (numbers[l] + numbers[r] > target) r--;
            else l++;
        }
        return new int[]{l+1, r+1};
    }
}
```

### Analysis

* **Time Complexity:** O(N), where N is the number of elements in the array. Each element is considered at most once.
    
* **Space Complexity:** O(1), as we only use a constant amount of extra space for the pointers.
    

### Conclusion

This solution leverages the sorted nature of the input array to efficiently find the two numbers that add up to the target. By using the two-pointer technique, the algorithm runs in linear time, making it both time-efficient and easy to implement.

You can find all my solutions at [Github](https://github.com/salah-jr/My-leetCode-solutions/tree/main/src/com/salah)

**Thank you for reading! Stay tuned for the next article in this series, where we'll tackle another exciting LeetCode problem.**