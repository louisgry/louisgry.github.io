---
title: 'BinaryTree: LeetCode 437. Path Sum III'
date: 2019-10-05 00:19:32
categories: Algorithm
tags: 
- binarytree
- recursion
---
- binarytree
    - 437 Path Sum III：https://leetcode.com/problems/path-sum-iii/
        - 求二叉树中等于给定sum的路径，路径可以不从根节点开始
        - Input: binary tree [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8
        - Output: 3
        - Explanation: three paths [[5,3], [5,2,1], [-3,11]]
        <!-- more -->
    - 思路：递归嵌套递归
    - 时间复杂度：O(n)
    - 空间复杂度：O(h)
    ```java
    public int pathSum(TreeNode root, int sum) {
        // condition
        if(root == null) {
            return 0;
        }
        // recursion
        return findPath(root, sum)
                + pathSum(root.left, sum) + pathSum(root.right, sum);
    }
    private int findPath(TreeNode node, int sum) {
        // condition
        if(node == null) {
            return 0;
        }
        int res = 0;
        if(node.val == sum){
            res += 1;
        }
        // recursion
        res += findPath(node.left, sum-node.val);
        res += findPath(node.right, sum-node.val);

        return res;
    }
    ```