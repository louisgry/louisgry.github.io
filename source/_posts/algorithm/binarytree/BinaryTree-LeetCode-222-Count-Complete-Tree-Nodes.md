---
title: 'BinaryTree: LeetCode 222. Count Complete Tree Nodes'
date: 2019-09-28 00:19:32
categories: 算法
tags: 
- binarytree
- recursion
---

- binarytree
    - 222 Count Complete Tree Nodes：https://leetcode.com/problems/count-complete-tree-nodes/
        - 计算完全二叉树节点个数
        - Input: binary tree [1,2,3,4,5,6,null]
        - Output: 6
        <!-- more -->
    - 思路：递归
    - 时间复杂度：O(n)
    - 空间复杂度：O(h)
    ```java
    public int countNodes(TreeNode root) {
        // 递归终止条件
        if (root==null) {
            return 0;
        }

        // 递归过程
        return 1+countNodes(root.left)+countNodes(root.right);
    }
    ```