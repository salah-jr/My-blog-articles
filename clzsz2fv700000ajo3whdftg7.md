---
title: "21. Merge Two Sorted Lists"
seoTitle: "21. Merge Two Sorted Lists"
seoDescription: "21. Merge Two Sorted Lists"
datePublished: Sun Dec 12 2021 10:00:00 GMT+0000 (Coordinated Universal Time)
cuid: clzsz2fv700000ajo3whdftg7
slug: 21-merge-two-sorted-lists
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1723586843126/ce5d41bb-c6c3-469f-b129-08af2af61075.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1723586850180/1773305f-bea7-4872-9b63-e5a6d7baf386.jpeg
tags: leetcode, problem-solving-skills

---

**Problem Statement:**

You are given the heads of two sorted linked lists `list1` and `list2`. Merge the two lists in a way that the resulting list is also sorted and return the head of the merged linked list.

**Examples:**

```plaintext
Input: list1 = [1,2,4], list2 = [1,3,4]
Output: [1,1,2,3,4,4]
```

```plaintext
plaintextCopy codeInput: list1 = [], list2 = []
Output: []
```

```plaintext
plaintextCopy codeInput: list1 = [], list2 = [0]
Output: [0]
```

**Approach:**

To merge two sorted linked lists, we can use an iterative approach that goes through both lists simultaneously. The idea is to compare the nodes of the two lists one by one, and link the smaller node to the new merged list.

1. **Initialize a Dummy Node:**
    
    * A dummy node (`dummy`) is used to simplify the merging process, allowing us to easily return the head of the new merged list at the end.
        
2. **Iterate Through Both Lists:**
    
    * Use two pointers, `list1` and `list2`, to traverse the input lists.
        
    * Compare the values of the nodes pointed to by `list1` and `list2`.
        
    * Append the smaller node to the merged list and move the corresponding pointer forward.
        
3. **Handle Remaining Nodes:**
    
    * Once one of the lists is fully traversed, append the remaining nodes of the other list to the merged list.
        
    * Since both lists are already sorted, the remaining nodes can be directly linked.
        
4. **Return the Merged List:**
    
    * The head of the merged list is [`dummy.next`](http://dummy.next), as `dummy` was initialized with a placeholder value.
        

**Solution:**

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        ListNode dummy = new ListNode(-1);
        ListNode head = dummy;

        while (list1 != null && list2 != null) {
            if (list1.val < list2.val) {
                dummy.next = list1;
                list1 = list1.next;
            } else {
                dummy.next = list2;
                list2 = list2.next;
            }
            dummy = dummy.next;
        }

        if (list1 != null) {
            dummy.next = list1;
        } else {
            dummy.next = list2;
        }

        return head.next;
    }
}
```

**Analysis:**

* **Time Complexity:** O(N+M)), where N and M are the lengths of `list1` and `list2` respectively. We traverse each list exactly once.
    
* **Space Complexity:** O(1), as no additional space is required apart from the pointers.
    

**Conclusion:**

This solution effectively merges two sorted linked lists into one sorted list using an iterative approach with two pointers. The use of a dummy node simplifies the merging process, ensuring that all edge cases are handled gracefully. The solution is optimal in terms of both time and space complexity.

You can find all my solutions at [Github](https://github.com/salah-jr/My-leetCode-solutions/tree/main/src/com/salah)

**Thank you for reading! Stay tuned for the next article in this series, where we'll tackle another exciting LeetCode problem.**