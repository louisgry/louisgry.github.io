---
title: 'Stack: LeetCode 144. Binary Tree Preorder Traversal'
date: 2019-10-24 16:01:13
categories: Algorithm
tags: 
- stack
- recursion
- binarytree
---
- stack
    - 144 Binary Tree Preorder Traversal：https://leetcode.com/problems/binary-tree-preorder-traversal/
        - 二叉树的前序遍历
        - Input: binary tree [1,2,3]
        - Output: [1,2,3]
        <!-- more -->
    - 思路1：递归
    - 时间复杂度：O(n)
    - 空间复杂度：O(h)
    ```java
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        preorderTraversal(root, res);
        return res;
    }
    private void preorderTraversal(TreeNode node, List<Integer> list) {
        // condition
        if(node == null) {
            return;
        }
        // recursion
        list.add(node.val);
        preorderTraversal(node.left, list);
        preorderTraversal(node.right, list);
    }
    ```
    - 思路2：非递归（使用栈）
    - 时间复杂度：O(n)
    - 空间复杂度：O(h)
    ```java
    public class Command {
        String s;
        TreeNode node;
        Command(String s, TreeNode node){
            this.s = s;
            this.node = node;
        }
    }
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        if(root == null){
            return res;
        }
        Stack<Command> stack = new Stack<Command>();
        stack.push(new Command("go", root));
        while(!stack.empty()) {
            Command command = stack.pop();
            if(command.s == "print"){
                res.add(command.node.val);
            }
            else {
                assert command.s.equals("go");
                // 因为栈的先进后出，所以要反过来
                if(command.node.right != null){
                    stack.push(new Command("go", command.node.right));
                }
                if(command.node.left != null){
                    stack.push(new Command("go", command.node.left));
                }
                stack.push(new Command("print", command.node));
            }
        }
        return res;
    }
    ```