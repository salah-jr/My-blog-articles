---
title: "113. Path Sum II"
seoTitle: "113. Path Sum II"
seoDescription: "113. Path Sum II"
datePublished: Wed Apr 20 2022 10:00:00 GMT+0000 (Coordinated Universal Time)
cuid: clzonuw6d00000amgcxip18xc
slug: 113-path-sum-ii
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1723411600954/def809fa-2713-4b2e-9d05-cd6e9c3da94f.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1723411606845/0e4d5dfa-5479-4cd9-aea6-9413fe7d653d.jpeg
tags: leetcode, problem-solving-skills

---

### Problem Statement

Given a binary tree and a target sum, find all root-to-leaf paths where the sum of the node values in the path equals the target sum. Each path should be represented as a list of integers from the root to the leaf.

**Example:**

```plaintext
codeInput: root = [5,4,8,11,null,13,4,7,2,null,null,null,1], targetSum = 22
Output: [
  [5,4,11,2],
  [5,8,4,5]
]
```

```plaintext
codeInput: root = [1,2,3], targetSum = 5
Output: []
```

```plaintext
codeInput: root = [1,2], targetSum = 1
Output: []
```

### Approach

To solve this problem, we use a depth-first search (DFS) approach:

1. **Recursive DFS Traversal:** Traverse the binary tree starting from the root. For each node, subtract its value from the target sum and continue the search in its left and right subtrees.
    
2. **Path Tracking:** Maintain a list to track the current path. Add the node’s value to this list as we traverse down and remove it when backtracking.
    
3. **Check Path Validity:** When a leaf node is reached, check if the remaining target sum is zero. If so, add the current path to the result list.
    

### Solution

Here’s the Java code implementing the above approach:

```java
javaCopy codeimport java.util.*;

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
    public List<List<Integer>> pathSum(TreeNode root, int targetSum) {
        List<List<Integer>> res = new ArrayList<>();
        helper(root, targetSum, new ArrayList<Integer>(), res);
        return res;
    }

    void helper(TreeNode root, int targetSum, ArrayList<Integer> sol, List<List<Integer>> res) {
        if (root == null) return;

        // Add the current node’s value to the path
        sol.add(root.val);

        // Check if it’s a leaf node and the path’s sum equals the target
        if (root.left == null && root.right == null && targetSum == root.val) {
            res.add(new ArrayList<>(sol));
        } else {
            // Continue the search on the left and right subtrees
            helper(root.left, targetSum - root.val, sol, res);
            helper(root.right, targetSum - root.val, sol, res);
        }

        // Backtrack: remove the last node’s value before returning
        sol.remove(sol.size() - 1);
    }
}
```

### Analysis

* **Time Complexity:** O(N), where N is the number of nodes in the tree. Each node is visited once.
    
* **Space Complexity:** O(H⋅L), where H is the height of the tree and L is the maximum length of a path. This includes space for the recursion stack and the list of results.
    

### Conclusion

This solution effectively identifies all root-to-leaf paths that sum up to a given target using a depth-first search strategy. By maintaining a running list of node values and verifying against the target sum, the algorithm efficiently finds and returns all valid paths. This approach ensures comprehensive tree traversal with manageable space complexity.

You can find all my solutions at [Github](https://github.com/salah-jr/My-leetCode-solutions/tree/main/src/com/salah)

**Thank you for reading! Stay tuned for the next article in this series, where we'll tackle another exciting LeetCode problem.**