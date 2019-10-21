---
title: 'BinaryTree: LeetCode 129. Sum Root to Leaf Numbers'
date: 2019-10-04 00:19:32
categories: 算法
tags: 
- binarytree
- recursion
---
- binarytree
    - 129 Sum Root to Leaf Numbers：https://leetcode.com/problems/sum-root-to-leaf-numbers/
        - 求所有路径组成的数字的和
        - Input: binary tree [1,2,3]
        - Output: 25
        - Explanation:
            - The root-to-leaf path 1->2 represents the number 12.
            - The root-to-leaf path 1->3 represents the number 13.
            - Therefore, sum = 12 + 13 = 25.
            <!-- more -->
    - 思路：递归
    - 时间复杂度：O(n)
    - 空间复杂度：O(h)
    ```java
    public int sumNumbers(TreeNode root) {
        return getSum(root, 0);
    }
    private int getSum(TreeNode root, int curSum) {
        // condition
        if(root == null) {
            return 0;
        }
        curSum = curSum*10 + root.val;
        if(root.left==null && root.right==null) {
            return curSum;
        }
        // recursion
        return getSum(root.left, curSum) + getSum(root.right, curSum);
    }
    ```
