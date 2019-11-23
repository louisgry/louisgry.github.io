---
title: '剑指Offer: 9. 两个栈实现队列'
date: 2019-11-07 17:15:26
categories: Algorithm
tags: 
- 剑指Offer
- stack
---
- 9 用两个栈来实现队列
    - https://www.nowcoder.com/practice/54275ddae22f475981afa2244dd448c6
    - 用两个栈来实现队列的push和pop
    <!-- more -->
    - 思路：push到stack1的最低端，stack2作辅助
    ```java
    public class Solution {
        // 思路：push到stack1的最低端，stack2作辅助
        private Stack<Integer> stack1 = new Stack<Integer>();
        private Stack<Integer> stack2 = new Stack<Integer>();
    
        public void push(int node) {
            while(!stack1.empty()) {
                stack2.push(stack1.pop());
            }
            stack1.push(node);
            while(!stack2.empty()) {
                stack1.push(stack2.pop());
            }
        }
    
        public int pop() {
            return stack1.pop();
        }
    }
    ```