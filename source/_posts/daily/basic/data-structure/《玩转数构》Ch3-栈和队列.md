---
title: 《玩转数构》Ch3-栈和队列
date: 2019-10-29 15:06:09
categories: Daily
tags: 
- 数构
- 栈&队列
description: 栈的应用（撤销、系统栈、括号匹配）、栈的基本实现；数组队列、循环队列
---
# Ch3-栈和队列
<!-- more -->
https://github.com/liuyubobobo/Play-with-Data-Structures/tree/master/03-Stacks-and-Queues
- 3-1：栈和栈的应用，撤销操作和系统栈
    - 栈：后进先出的数据结构（LIFO）
    - 栈的应用1：撤销操作
        - 撤销：出栈 
    - 栈的应用2：程序调用的系统栈
        - 函数的嵌套调用
        - 递归调用
- 3-2：栈的基本实现
    - 入栈：push（复杂度均摊为O(1)）
    - 出栈：pop
    - 看栈顶元素：peek
    - 获取大小：size
    - 判断是否为空：empty
- 3-3：栈的另一个应用，括号匹配
    - 20 Valid Parentheses：https://leetcode.com/problems/valid-parentheses/
        - 判断括号是否匹配
        - Input: "()[]{}"
        - Output: true
    - 思路：使用栈
    - 时间复杂度：O(n)
    - 空间复杂度：O(n)
    ```java
    public boolean isValid(String s) {
        Stack<Character> stack = new Stack<Character>();
        for(int i=0; i<s.length(); i++) {
            // 注意是单引号，char
            if(s.charAt(i)=='(' || s.charAt(i)=='{' || s.charAt(i)=='[') {
                stack.push(s.charAt(i));
            }
            else {
                if(stack.size() == 0) {
                    return false;
                }
                char c = stack.pop();
                char match;
                if(s.charAt(i)==')') {
                    match = '(';
                }
                else if(s.charAt(i)=='}') {
                    match = '{';
                }
                else {
                    assert s.charAt(i)==']';
                    match = '[';
                }
                
                if(match != c){
                    return false;
                }
            }
        }
        if(stack.size() != 0) {
            return false;
        }
        return true;
    }
    ```
- 3-4：关于Leetcode的更多说明
    - 代码提交可以加上main方法
    - 方法不能是private
    - 关于学习方法和今后提高自学能力的疑问
- 3-5：数组队列
    - 队列：先进先出的数据结构（FIFO）
    - 入队：add（addLast）
    - 出队：poll（removeFirst）
    - 查看队首元素：peek
    - 获取大小：size
    - 判断是否为空：isEmpty
- 3-6：循环队列
    - 数组队列的问题：删除队首元素O(n)的复杂度
    - 循环：front + tail指针
    - front==tail：队列为空
    - (tail+1)%c==front：队列满
- 3-7：循环队列的实现
    - 入队：add
    - 出队：poll
    - 查看队首元素：peek
    - 获取大小：size
    - 判断是否为空：isEmpty
- 3-8：数组队列和循环队列的比较
    - 数据规模：n=100000
    - 循环队列比数组队列快140倍
    - 队列的应用：广度优先遍历
