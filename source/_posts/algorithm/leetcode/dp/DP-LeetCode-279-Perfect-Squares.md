---
title: 'DP: LeetCode 279. Perfect Squares'
date: 2019-09-09 23:31:13
categories: 算法
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
    