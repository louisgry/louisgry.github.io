---
title: 'BinaryTree: LeetCode 226. Invert Binary Tree'
date: 2019-09-25 00:19:32
categories: 算法
tags: binarytree
---
- binarytree
    - 226 Invert Binary Tree：https://leetcode.com/problems/invert-binary-tree/
        - 反转二叉树，左右子树对调
        - Input: Given binary tree [4,2,7,1,3,6,9]
        - Output: return binary tree [4,7,2,9,6,3,1]
        <!-- more -->
    - 思路：递归
    - 时间复杂度：O(n)
    - 空间复杂度：O(h)
    ```java
    public TreeNode invertTree(TreeNode root) {
        // 递归终止条件
        if (root==null) {
            return null;
        }
        // 递归过程
        TreeNode left = invertTree(root.left);
        TreeNode right = invertTree(root.right);
        // swap
        root.left = right;
        root.right = left;
        return root;
    }
    ```