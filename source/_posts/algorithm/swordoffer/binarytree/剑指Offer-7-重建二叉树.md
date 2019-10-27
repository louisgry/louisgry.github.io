---
title: '剑指Offer: 7. 重建二叉树'
date: 2019-10-27 14:22:56
categories: 算法
tags: 
- 剑指Offer
- binarytree
- recursion
---
- 7 重建二叉树
    - https://www.nowcoder.com/practice/8a19cbe657394eeaac2f6ea9b0f6fcf6
    - 根据前序和中序遍历的结果，重建二叉树
    - 思路：递归
    <!-- more -->
    - 时间复杂度：O(n)
    - 空间复杂度：O(n)
    ```java
    import java.util.Arrays;

    public TreeNode reConstructBinaryTree(int [] pre,int [] in) {
        if(pre.length==0 || in.length==0) {
            return null;
        }
        // 前序第一个节点为根节点
        TreeNode root = new TreeNode(pre[0]);
        // 中序找前序的根
        for(int i=0; i<in.length; i++) {
            if(in[i] == pre[0]) {
                // copyOfRange左闭右开
                root.left = reConstructBinaryTree(Arrays.copyOfRange(pre, 1, i+1), Arrays.copyOfRange(in, 0, i));
                root.right = reConstructBinaryTree(Arrays.copyOfRange(pre, i+1, pre.length), Arrays.copyOfRange(in, i+1, in.length));
                break;
            }
        }
        return root;
    }
    ```