---
title: "19. Remove Nth Node From End of List"
seoTitle: "19. Remove Nth Node From End of List"
seoDescription: "19. Remove Nth Node From End of List"
datePublished: Thu Aug 08 2024 21:35:44 GMT+0000 (Coordinated Universal Time)
cuid: clzlsrt0n000e09jn14h599ds
slug: leetcode-19-remove-nth-node-from-end-of-list
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1723152880179/e6e273d1-389a-4a68-8413-b8cfc2dfd930.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1723152887978/254e876e-e9b0-4950-80fb-4d5dd6001208.jpeg
tags: leetcode, linkedlists, problem-solving-skills

---

**Problem Statement:**

Given the `head` of a linked list, remove the `n<sup>th</sup>` node from the end of the list and return its head.

**Examples:**

```java
Example 1:
Input: head = [1], n = 1
Output: []

Example 2:
Input: head = [1,2], n = 1
Output: [1]
```

**Approach:**

To solve this problem efficiently, we use a two-pass approach:

1. **First Pass:** Count the total number of nodes in the linked list.
    
2. **Second Pass:** Traverse the list again to remove the n-th node from the end. We can find this node by iterating to the `(count - n)`\-th node, where `count` is the total number of nodes.
    

Here's a detailed explanation of the approach:

1. **Count Nodes:** Traverse the list to determine the total number of nodes.
    
2. **Determine Target Node:** Compute the position of the node to be removed.
    
3. **Remove Node:** Traverse the list again to remove the target node.
    

**Solution:**

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        // Step 1: Count the total number of nodes
        int countNodes = 1;
        ListNode cur = head;
        ListNode prev = cur;

        while (cur.next != null) {
            cur = cur.next;
            countNodes++;
        }

        // Step 2: Check if we need to remove the head node
        if (n == countNodes) {
            return head.next;
        }

        // Step 3: Find the node to be removed
        int nodeNum = countNodes - n + 1;
        cur = head;
        prev = cur;
        int i = 1;
        
        while (cur.next != null && i < nodeNum) {
            if (i + 1 == nodeNum) {
                cur.next = cur.next.next;
                break;
            }
            cur = cur.next;
            i++;
        }

        return head;
    }
}
```

**Analysis:**

* **Time Complexity:** O(L), where L is the number of nodes in the list. We make two passes through the list: one for counting and one for removing the node.
    
* **Space Complexity:** O(1). We use a constant amount of extra space, regardless of the size of the input.
    

**Conclusion:**

This solution efficiently removes the n-th node from the end of a linked list by leveraging a two-pass approach. The first pass counts the total number of nodes, and the second pass identifies and removes the target node. This method ensures that the problem is solved in linear time with constant space usage.

You can find all my solutions at [Github](https://github.com/salah-jr/My-leetCode-solutions/tree/main/src/com/salah)

**Thank you for reading! Stay tuned for the next article in this series, where we'll tackle another exciting LeetCode problem.**