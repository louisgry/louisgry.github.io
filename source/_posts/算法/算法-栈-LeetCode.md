---
title: '算法：栈-LeetCode'
date: 2019-11-06 16:48:17
categories: 算法
tags: 
- Review
- 二刷
description: 有效的括号、二叉树的前序遍历（递归、**非递归**）
---

## stack
- 栈
    - [20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)：有效的括号[【20题解】](#20题解) 
    - [144. Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/)：二叉树的前序遍历[【144题解】](#144题解)

### 20题解
- 栈：只关心最近一次操作
```java
class Solution {
    public boolean isValid(String s) {
        // 时间复杂度：O(n)
        // 空间复杂度：O(n)
        // 遍历string
        // -- 如果是左括号，压入栈
        // -- 如果是右括号，弹出栈
        // ---- 如果不匹配，返回false
        // ---- 如果for结束，栈不为空，返回false
        // ---- 否则，返回true
        
        Stack<Character> stack = new Stack<Character>();
        char[] matchs = {'(', '{', '['};
        
        for(int i=0; i<s.length(); i++) {
            // s.charAt(i) (
            if(s.charAt(i)=='(' || s.charAt(i)=='{' || s.charAt(i)=='[') {
                stack.push(s.charAt(i));    
            }
            else {
                // 【Runtime Error："]"】没有进行size为0的判断
                if(stack.size()==0) {
                    return false;
                }
                char c = stack.pop();
                char match;
                if(s.charAt(i)==')') {
                    match = matchs[0];
                }
                else if(s.charAt(i)=='}') {
                    match = matchs[1];
                }
                else {
                    match = matchs[2];
                }
                
                if(c != match) {
                    return false;
                }
            }
        }
        
        if(stack.size()!=0) {
            return false;
        }
        return true;
    }
}
```

### 144题解
- 递归解法
```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        // 时间复杂度：O(n)
        // 空间复杂度：O(h)
        // 递归解法：根节点->左节点->右节点
        // 要返回List，所以需要辅助函数来做递归

        List<Integer> res = new ArrayList<Integer>();
        preorderTraversal(root, res);
        return res;
    }
    private void preorderTraversal(TreeNode root, List<Integer> res) {
        // condition
        if(root == null) {
            return;
        }
        // recursion
        res.add(root.val);
        preorderTraversal(root.left, res);
        preorderTraversal(root.right, res);
    }
}
```
- 非递归解法（栈+Command）
```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        // 时间复杂度：O(n)
        // 空间复杂度：O(h)
        // 非递归解法：栈（递归的本质是栈）
        // 把第一个指令go-root压入栈，while循环直到stack为空
        // -- 如果指令是print，把command.node的值add进List
        // -- 否则指令就是go，把指令倒序压入stack：print-root, go-left, go-right
        
        List<Integer> res = new ArrayList<Integer>();
        // 【Runtime Error: []】没有进行非空判断
        if(root == null) {
            return res;
        }
        Stack<Command> stack = new Stack<Command>();
        stack.push(new Command("go", root));
        while(!stack.empty()) {
            // 取出栈顶元素
            Command command = stack.pop();
            if(command.s == "print") {
                res.add(command.node.val);
            }
            else {
                // 如果是go命令，倒序压入指令
                if(command.node.right != null) {
                    stack.push(new Command("go", command.node.right));
                }
                if(command.node.left != null) {
                    stack.push(new Command("go", command.node.left));
                }
                stack.push(new Command("print", command.node));
            }
        }
        return res;
    }
    // class 是小写的
    public class Command {
        // 指令：print、go
        String s;
        // 指令要作用于某个节点上
        TreeNode node;
        Command(String s, TreeNode node) {
            this.s = s;
            this.node = node;
        }
    }
}
```