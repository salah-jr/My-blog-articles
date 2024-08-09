---
title: "48. Rotate Image"
seoTitle: "48. Rotate Image"
seoDescription: "48. Rotate Image"
datePublished: Tue Aug 02 2022 10:00:00 GMT+0000 (Coordinated Universal Time)
cuid: clzn9t2dc00030al55rf007zh
slug: 48-rotate-image
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1723241951769/0672e963-cf99-48f8-a988-b58855b7dfe3.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1723241982875/3ca7a407-bae1-4909-9518-32168e721e8d.jpeg
tags: leetcode, problem-solving-skills

---

**Problem Statement:**

You are given an `n x n` 2D `matrix` representing an image, rotate the image by **90** degrees (clockwise).

You have to rotate the image in place which means you have to modify the input 2D matrix directly. **DO NOT** allocate another 2D matrix and do the rotation.

**Examples:**

```java
Input: matrix = [
  [1,2,3],
  [4,5,6],
  [7,8,9]
]
Output: [
  [7,4,1],
  [8,5,2],
  [9,6,3]
]
```

**Approach:**

To rotate the matrix by 90 degrees clockwise, we can break the process down into two main steps:

1. **Transpose the Matrix:** Convert all rows of the matrix into columns. This is done by swapping the elements `matrix[i][j]` with `matrix[j][i]` for all pairs `(i, j)` where `i < j`.
    
2. **Reverse the Rows:** After transposing the matrix, each row is reversed to achieve the final rotated matrix.
    

**Solution:**

```java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length - 1;
        
        // Step 1: Transpose the matrix
        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < i; j++) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }

        // Step 2: Reverse each row
        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix.length / 2; j++) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[i][n - j];
                matrix[i][n - j] = temp;
            }
        }
    }
}
```

**Analysis:**

* **Time Complexity:** O(n2), where n is the number of rows (or columns) in the matrix. Both transposing the matrix and reversing the rows take O(n2) time.
    
* **Space Complexity:** O(1). The algorithm rotates the matrix in place, using only constant extra space.
    

**Conclusion:**

This solution effectively rotates the image (matrix) by 90 degrees clockwise by first transposing the matrix and then reversing each row. This method is efficient in terms of both time and space, as it leverages in-place operations.

You can find all my solutions at [Github](https://github.com/salah-jr/My-leetCode-solutions/tree/main/src/com/salah)

**Thank you for reading! Stay tuned for the next article in this series, where we'll tackle another exciting LeetCode problem.**