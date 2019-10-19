---
title: 'BinaryTree: LeetCode 112. Path Sum'
date: 2019-09-30 00:19:32
categories: 算法
tags: binarytree
---
- binarytree
    - 112 Path Sum：https://leetcode.com/problems/path-sum/
        - 找出二叉树路径中的是否有一条和等于sum
        - Input: binary tree [5,4,8,11,null,13,4,7,2]
        - Output: true
        <!-- more -->
    - 思路：递归
    - 时间复杂度：O(n)
    - 空间复杂度：O(h)
    ```java
    public boolean hasPathSum(TreeNode root, int sum) {
        // 递归终止条件
        if (root == null) {
            return false;
        }
        if (root.left==null && root.right==null) {
            return root.val == sum;
        }
        // 递归过程
        return hasPathSum(root.left, sum-root.val) ||
                hasPathSum(root.right, sum-root.val);
    }
    ```
