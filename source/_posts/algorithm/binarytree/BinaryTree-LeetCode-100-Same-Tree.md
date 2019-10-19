---
title: 'BinaryTree: LeetCode 100. Same Tree'
date: 2019-09-26 00:19:32
categories: 算法
tags: binarytree
---

- binarytree
    - 100 Same Tree：https://leetcode.com/problems/same-tree/
        - 判断两颗二叉树是否相同
        - Input: binary tree [1,2,null], [1,null,2]
        - Output: false
        <!-- more -->
    - 思路：递归
    - 时间复杂度：O(n)
    - 空间复杂度：O(h)
    ```java
    public boolean isSameTree(TreeNode p, TreeNode q) {
        if (p==null && q==null) {
            return true;
        }
        if (p==null || q==null) {
            return false;
        }
        if (p.val != q.val) {
            return false;
        }
        boolean left = isSameTree(p.left, q.left);
        boolean right = isSameTree(p.right, q.right);
        return left && right;
    }
    ```