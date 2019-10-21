---
title: 'BinaryTree: LeetCode 104. Maximum Depth of Binary Tree'
date: 2019-09-23 00:19:32
categories: 算法
tags: 
- binarytree
- recursion
---
- binarytree
    - 104 Maximum Depth of Binary Tree：https://leetcode.com/problems/maximum-depth-of-binary-tree/
        - 返回二叉树的最大深度
        - Input: Given binary tree [3,9,20,null,null,15,7]
        - Output: 3
        <!-- more -->
    - 知识：深度K=「log2n」+1（向下取整）
    - 思路：递归
    - 时间复杂度：O(n)，n是节点数
    - 空间复杂度：O(h)，h是树深度
    ```java
    public int maxDepth(TreeNode root) {
        // 递归终止条件
        if (root==null) {
            return 0;
        }
        // 递归过程
        return Math.max(maxDepth(root.left), maxDepth(root.right))+1;
    }
    ```