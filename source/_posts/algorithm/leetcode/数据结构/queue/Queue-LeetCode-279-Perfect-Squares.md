---
title: 'Queue: LeetCode 279. Perfect Squares'
date: 2019-10-26 18:16:03
categories: Algorithm
tags: 
- queue
---
- queue
    - 279 Perfect Squares：https://leetcode.com/problems/perfect-squares/
        - 给定一个正整数n，求其由最少个完全平方数组成的和等于n的个数
        - Input: n = 13
        - Output: 2
        - Explanation: 13 = 4 + 9.
        <!-- more -->
    - 思路3：queue
    - 时间复杂度：O(n)
    - 空间复杂度：O(n)
    ```java
    import javafx.util.Pair;

    public int numSquares(int n) {
        int res = 0;
        LinkedList<Pair<Integer, Integer>> queue = new LinkedList<Pair<Integer, Integer>>();
        queue.addLast(new Pair<Integer, Integer>(n, 0));
        
        boolean[] visited = new boolean[n+1];
        visited[n] = true;
        
        while(!queue.isEmpty()) {
            Pair<Integer, Integer> front = queue.removeFirst();
            int num = front.getKey();
            int step = front.getValue();
            
            if(num == 0) {
                res = step;
            }
            for(int i=1; num-i*i>=0; i++) {
                if(!visited[num-i*i]) {
                    queue.addLast(new Pair<Integer, Integer>(num-i*i, step+1));
                    visited[num-i*i] = true;
                }
            }
        }
        return res;
    }
    ```