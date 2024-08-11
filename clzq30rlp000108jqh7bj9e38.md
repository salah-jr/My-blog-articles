---
title: "24. Swap Nodes in Pairs"
seoTitle: "24-swap-nodes-in-pairs"
seoDescription: "24-swap-nodes-in-pairs"
datePublished: Wed Sep 14 2022 10:00:00 GMT+0000 (Coordinated Universal Time)
cuid: clzq30rlp000108jqh7bj9e38
slug: 24-swap-nodes-in-pairs
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1723411957288/1e467721-e528-4e4a-8a17-cbd6187a815d.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1723411963267/fac3698e-bd04-4f18-94cb-c04af97d272f.jpeg
tags: leetcode, problem-solving-skills

---

### Problem Statement

Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list’s nodes (i.e., only the nodes themselves may be changed).

**Example:**

```plaintext
plaintextCopy codeInput: head = [1,2,3,4]
Output: [2,1,4,3]
```

```plaintext
plaintextCopy codeInput: head = []
Output: []
```

```plaintext
plaintextCopy codeInput: head = [1]
Output: [1]
```

### Approach

To solve this problem of swapping nodes in pairs, we can use an iterative approach that traverses the linked list and swaps every two adjacent nodes. Here’s a step-by-step breakdown:

1. **Handle Edge Cases:** First, check if the list is empty or has only one node. If so, return the list as is because there’s nothing to swap.
    
2. **Initialize Pointers:** Use three pointers:
    
    * `slow` to point to the first node of the pair to be swapped.
        
    * `fast` to point to the second node of the pair.
        
    * `prev` to keep track of the last node of the previous pair to link it to the newly swapped nodes.
        
3. **Swap Pairs Iteratively:**
    
    * Adjust pointers to swap the nodes.
        
    * Move the `slow` and `fast` pointers to the next pair.
        
    * Update the `prev` pointer to link the swapped pairs correctly.
        
4. **Update the Head:** After the first swap, update the head of the list to point to the second node, which becomes the new head.
    

Solution

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
    public ListNode swapPairs(ListNode head) {
        if (head == null || head.next == null)
            return head;

        ListNode slow = head;
        ListNode fast = slow.next;
        ListNode prev = null;

        // New head will be the second node
        head = head.next;

        while (fast != null) {
            slow.next = fast.next;
            fast.next = slow;

            if (prev != null)
                prev.next = fast;

            if (slow.next == null)
                break;

            prev = slow;
            slow = slow.next;
            fast = slow.next;
        }

        return head;
    }
}
```

### Analysis

* **Time Complexity:** O(N), where N is the number of nodes in the linked list. Each node is visited once during the traversal.
    
* **Space Complexity:** O(1), as no additional space is used except for a few pointers.
    

### Conclusion

This solution effectively swaps adjacent nodes in a linked list using an iterative approach with three pointers. It ensures that all pairs of nodes are swapped in a single traversal, making the solution both time and space efficient. By carefully updating pointers, the algorithm maintains the correct order of nodes while swapping them in pairs.

You can find all my solutions at [Github](https://github.com/salah-jr/My-leetCode-solutions/tree/main/src/com/salah)

**Thank you for reading! Stay tuned for the next article in this series, where we'll tackle another exciting LeetCode problem.**