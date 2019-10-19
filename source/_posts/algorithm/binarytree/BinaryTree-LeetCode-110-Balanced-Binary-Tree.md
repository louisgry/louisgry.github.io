---
title: 'BinaryTree: LeetCode 110. Balanced Binary Tree'
date: 2019-09-29 00:19:32
categories: 算法
tags: binarytree
---
- binarytree
    - 110 Balanced Binary Tree：https://leetcode.com/problems/balanced-binary-tree/
        - 判断是否为平衡二叉树（每个节点的左右子树高度差不超过1）
        - Input: binary tree [1,2,2,3,3,null,null,4,4]
        - Output: false
        <!-- more -->
    - 思路：递归
    - 时间复杂度：O(n)
    - 空间复杂度：O(h)
    ```java
    public boolean isBalanced(TreeNode root) {
        // 递归终止条件
        if (root == null) {
            return true;
        }
        if (Math.abs(getDepth(root.left)-getDepth(root.right))>1) {
            return false;
        }

        // 递归过程
        return isBalanced(root.left) && isBalanced(root.right);
    }
    private int getDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        return 1 + Math.max(getDepth(root.left), getDepth(root.right));
    }
    ```
