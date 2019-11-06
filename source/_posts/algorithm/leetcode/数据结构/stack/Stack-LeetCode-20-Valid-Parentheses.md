---
title: 'Stack: LeetCode 20. Valid Parentheses'
date: 2019-10-24 13:31:43
categories: Algorithm
tags: 
- stack
- string
---
- stack
    - 20 Valid Parentheses：https://leetcode.com/problems/valid-parentheses/
        - 判断括号是否匹配
        - Input: "()[]{}"
        - Output: true
        <!-- more -->
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