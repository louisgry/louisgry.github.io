---
title: 'BinaryTree: LeetCode 235. Lowest Common Ancestor of a Binary Search Tree'
date: 2019-10-06 00:19:32
categories: Algorithm
tags: 
- binarytree
- recursion
---

- binarytree
    - 235 Lowest Common Ancestor of a Binary Search Tree：https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/
        - 寻找两个节点最近的公共祖先
        - Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
        - Output: 2
        <!-- more -->
    - 思路：递归
    - 时间复杂度：O(n)
    - 空间复杂度：O(h)
    ```java
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // condition
        if(root == null) {
            return null;
        }
        // recursion
        if(p.val<root.val && q.val<root.val) {
            return lowestCommonAncestor(root.left, p, q);
        }
        if(p.val>root.val && q.val>root.val) {
            return lowestCommonAncestor(root.right, p, q);
        }
        return root;
    }
    ```