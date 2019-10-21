---
title: 'BinaryTree: LeetCode 101. Symmetric Tree'
date: 2019-09-27 00:19:32
categories: 算法
tags: 
- binarytree
- recursion
---

- binarytree
    - 101 Symmetric Tree：https://leetcode.com/problems/symmetric-tree/
        - 判断二叉树是否对称
        - Input: binary tree [1,2,2,null,3,null,3]
        - Output: false
        <!-- more -->
    - 思路：递归
    - 时间复杂度：O(n)
    - 空间复杂度：O(h)
    ```java
    public boolean isSymmetric(TreeNode root) {
        // 递归终止条件
        if (root == null) {
            return true;
        }
        // 递归过程
        return isMirror(root.left, root.right);
    }
    private boolean isMirror(TreeNode p, TreeNode q) {
        if (p==null && q==null) {
            return true;
        }
        if (p==null || q==null) {
            return false;
        }
        if (p.val != q.val) {
            return false;
        }
        return isMirror(p.left, q.right) && isMirror(p.right, q.left);
    }
    ```
