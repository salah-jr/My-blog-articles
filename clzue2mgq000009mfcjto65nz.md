---
title: "98. Validate Binary Search Tree"
seoTitle: "98. Validate Binary Search Tree"
seoDescription: "98. Validate Binary Search Tree"
datePublished: Sat Sep 30 2023 09:00:00 GMT+0000 (Coordinated Universal Time)
cuid: clzue2mgq000009mfcjto65nz
slug: leetcode-98-validate-binary-search-tree
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1723672390406/98d8aee0-9dc7-4486-8d79-08a3b4afa727.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1723672417270/50c99523-a6ec-49af-b013-ed44f226c6f4.jpeg
tags: leetcode, problem-solving-skills

---

### Problem Statement

Given the root of a binary tree, determine if it is a valid binary search tree (BST). A valid BST is defined as follows:

* The left subtree of a node contains only nodes with values less than the node's value.
    
* The right subtree of a node contains only nodes with values greater than the node's value.
    
* Both the left and right subtrees must also be binary search trees.
    

**Example:**

```plaintext
Input: root = [2,1,3]
Output: true
```

```plaintext
plaintextCopy codeInput: root = [5,1,4,null,null,3,6]
Output: false
Explanation: The root node's value is 5 but its right child's value is 4.
```

### Approach

To validate whether a binary tree is a Binary Search Tree (BST), we can perform a depth-first traversal and ensure that for each node:

1. All values in the left subtree are strictly less than the current node's value.
    
2. All values in the right subtree are strictly greater than the current node's value.
    

We can achieve this by using a helper function that takes additional parameters (`low` and `high`) to track the permissible range for each node's value as we traverse the tree.

1. **Base Case:** If the current node is `null`, we return `true` as an empty tree is a valid BST.
    
2. **Validation:**
    
    * Check if the current node's value violates the BST properties.
        
    * Specifically, the node's value must be greater than `low` (if `low` is not `null`) and less than `high` (if `high` is not `null`).
        
3. **Recursive Checks:**
    
    * Recursively check the left subtree, updating the `high` bound to the current node's value.
        
    * Recursively check the right subtree, updating the `low` bound to the current node's value.
        

Solution

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public boolean isValidBST(TreeNode root) {
        if (root == null)
            return true;
        return helper(root, null, null);
    }

    static boolean helper(TreeNode root, Integer low, Integer high) {
        if (root == null)
            return true;

        if ((low != null && root.val <= low) || (high != null && root.val >= high))
            return false;

        return helper(root.right, root.val, high) && helper(root.left, low, root.val);
    }
}
```

Analysis

* **Time Complexity:** O(N), where N is the number of nodes in the tree. Each node is visited exactly once.
    
* **Space Complexity:** O(H), where H is the height of the tree. This accounts for the space used by the recursive call stack.
    

### Conclusion

This solution efficiently validates a binary search tree by recursively checking the constraints at each node. The approach ensures that the entire tree adheres to the BST properties, and the solution is optimal in both time and space complexity.

You can find all my solutions at [Github](https://github.com/salah-jr/My-leetCode-solutions/tree/main/src/com/salah)

**Thank you for reading! Stay tuned for the next article in this series, where we'll tackle another exciting LeetCode problem.**