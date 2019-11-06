---
title: 'BinaryTree: LeetCode 111. Minimum Depth of Binary Tree'
date: 2019-09-24 00:20:02
categories: Algorithm
tags: 
- binarytree
- recursion
---

- binarytree
    - 111 Minimum Depth of Binary Tree：https://leetcode.com/problems/minimum-depth-of-binary-tree/
        - 求二叉树的最低深度
        - Input: Given binary tree [3,9,20,null,null,15,7]
        - Output: 2
        <!-- more -->
    - 思路：递归
    - 时间复杂度：O(n)
    - 空间复杂度：O(h)
    ```java
    public int minDepth(TreeNode root) {
        // 递归终止条件
        if (root==null) {
            return 0;
        }
        if (root.left==null && root.right==null) {
            return 1;
        }
        // 递归过程
        int min = Integer.MAX_VALUE;
        if (root.left!=null) {
            min = Math.min(min, minDepth(root.left)+1);
        }
        if (root.right!=null) {
            min = Math.min(min, minDepth(root.right)+1);
        }
        return min;
    }
    ```