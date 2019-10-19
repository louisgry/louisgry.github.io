---
title: 'BinaryTree: LeetCode 404. Sum of Left Leaves'
date: 2019-10-01 00:19:32
categories: 算法
tags: binarytree
---

- binarytree
    - 练习1：404 Sum of Left Leaves：https://leetcode.com/problems/sum-of-left-leaves/
        - 求左叶子节点的和
        - Input: binary tree [3,9,20,null,null,15,7]
        - Output: 24
        <!-- more -->
    - 思路：递归
    - 时间复杂度：O(n)
    - 空间复杂度：O(h)
    ```java
    public int sumOfLeftLeaves(TreeNode root) {
        int sum = 0;
        // 递归终止条件
        if (root == null) {
            return 0;
        }
        if (root.left!=null && root.left.left==null && root.left.right==null) {
            sum += root.left.val;
        }
        // 递归过程
        sum += sumOfLeftLeaves(root.left) + sumOfLeftLeaves(root.right);
        return sum;
    }
    ```