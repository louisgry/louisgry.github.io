---
title: '剑指Offer: 8. 二叉树的下一个节点'
date: 2019-11-06 17:10:26
categories: Algorithm
tags: 
- 剑指Offer
- binarytree
---
- 8 二叉树的下一个节点
    - https://www.nowcoder.com/practice/9023a0c988684a53960365b889ceaf5e
    - 给定一个二叉树和其中的一个结点，请找出中序遍历顺序的下一个结点并且返回。注意，树中的结点不仅包含左右子结点，同时包含指向父结点的指针。
    <!-- more -->
    ```java
    public TreeLinkNode GetNext(TreeLinkNode pNode)
    {
        // 中序遍历：左节点 -> 根节点 -> 右节点
        // 如果存在右子树，下一个节点为右子树的最左节点
        // 如果不存在右子树，while循环
        // -- if该节点为父节点的左子树，下一节点是父节点(node.next)
        // -- 沿着父结点追朔，直到找到某个结点是其父结点的左子树
        if(pNode == null)
            return null;
        
        if(pNode.right != null) {
            TreeLinkNode node = pNode.right;
            while(node.left != null) {
                node = node.left;
            }
            return node;
        }
        
        while(pNode.next != null) {
            if(pNode == pNode.next.left) {
                return pNode.next;
            }
            pNode = pNode.next;
        }
        return null;
    }
    ```