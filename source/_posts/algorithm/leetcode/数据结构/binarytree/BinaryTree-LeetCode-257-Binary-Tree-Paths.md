---
title: 'BinaryTree: LeetCode 257. Binary Tree Paths'
date: 2019-10-02 00:19:32
categories: Algorithm
tags: 
- binarytree
- recursion
---

- binarytree
    - 257 Binary Tree Paths：https://leetcode.com/problems/binary-tree-paths/
        - 返回二叉树所有路径的path
        - Input: binary tree [1,2,3,null,5]
        - Output: ["1->2->5", "1->3"]
        <!-- more -->
    - 思路：递归
    - 时间复杂度：O(n)
    - 空间复杂度：O(h)
    ```java
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> result = new ArrayList<String>();
        // 递归终止条件
        if (root == null ) {
            return result;
        }
        if (root.left==null && root.right==null) {
            result.add(Integer.toString(root.val));
            return result;
        }
        // 递归过程
        List<String> leftPath = binaryTreePaths(root.left);
        for (String i : leftPath) {
            StringBuilder sb = new StringBuilder(Integer.toString(root.val));
            sb.append("->");
            sb.append(i);
            result.add(sb.toString());
        }
        List<String> rightPath = binaryTreePaths(root.right);
        for (String j : rightPath) {
            StringBuilder sb = new StringBuilder(Integer.toString(root.val));
            sb.append("->");
            sb.append(j);
            result.add(sb.toString());
        }
        return result;
    }
    ```