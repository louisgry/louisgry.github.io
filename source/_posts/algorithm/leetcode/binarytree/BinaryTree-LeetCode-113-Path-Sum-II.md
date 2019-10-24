---
title: 'BinaryTree: LeetCode 113. Path Sum II'
date: 2019-10-03 00:19:32
categories: 算法
tags: 
- binarytree
- recursion
---

- binarytree
    - 113 Path Sum II：https://leetcode.com/problems/path-sum-ii/
        - 返回二叉树路径中的所有等于sum的路径
        - Input: binary tree [5,4,8,11,null,13,4,7,2,null,null,5,1]
        - Output: [[5,4,11,2],[5,8,4,5]]
        <!-- more -->
    - 思路：递归
    - 时间复杂度：O(n)
    - 空间复杂度：O(h)
    ```java
    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        List<List<Integer>> result = new ArrayList<>();
        ArrayList<Integer> middle = new ArrayList<>();
        getPathSum(root, sum, middle, result);
        return result;
    }
    private void getPathSum(TreeNode root, int sum, ArrayList<Integer> middle, List<List<Integer>> result) {
        // condition
        if(root == null) {
            return;
        }
        middle.add(root.val);
        if(root.left==null && root.right==null) {
            if(root.val == sum) {
                result.add(new ArrayList<>(middle));
            }
        }
        // recursion
        getPathSum(root.left, sum-root.val, middle, result);
        getPathSum(root.right, sum-root.val, middle, result);
        middle.remove(middle.size()-1);
    }
    ```