---
title: 'DP: LeetCode 279. Perfect Squares'
date: 2019-09-09 23:31:13
categories: Algorithm
tags: 
- dp
- memorySearch
- queue
---
- dp、queue
    - 279 Perfect Squares：https://leetcode.com/problems/perfect-squares/
        - 给定一个正整数n，求其由最少个完全平方数组成的和等于n的个数
        - Input: n = 13
        - Output: 2
        - Explanation: 13 = 4 + 9.
        <!-- more -->
    - Key：f(n) = min(1+f(n-i^2))
    - 思路1：记忆化搜索
    - 时间复杂度：O(n)
    - 空间复杂度：O(n)
    ```java
    int[] memo;
    public int numSquares(int n) {
        memo = new int[n+1];
        Arrays.fill(memo, -1);
        return squares(n);
    }
    
    private int squares(int n){
        if(n==0)
            return 0;
        if(memo[n] != -1)
            return memo[n];
        int min = Integer.MAX_VALUE;
        for(int i=1; n-i*i>=0; i++)
            min = Math.min(min, 1+squares(n-i*i));
        memo[n] = min;
        return min;
    }
    ```
    - 思路2：动态规划
    ```java
    public int numSquares(int n) {
        int[] memo = new int[n+1];
        Arrays.fill(memo, Integer.MAX_VALUE);
        memo[0] = 0;
        for(int i=1; i<=n; i++)
            for(int j=1; i-j*j>=0; j++)
            memo[i] = Math.min(memo[i], 1+memo[i-j*j]);
        return memo[n];
    }
    ```
    - 思路3：queue
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
