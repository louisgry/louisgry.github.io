---
title: 'DP: LeetCode 343. Integer Break'
date: 2019-09-07 23:31:13
categories: Algorithm
tags:  
- dp
- memorySearch
---
- dp
    - 343 Integer Break：https://leetcode.com/problems/integer-break/ 
        - 给定正整数n，分割n为至少两个数之和，返回分割后的数字相乘的最大值
        - Input: 10
        - Output: 36
        - Explanation: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36.
        <!-- more -->
    - Key：f(n) = max(i*f(n-i))
    - 递归：Time Limit Exceeded（暴力解法：回溯遍历这个数分割的所有可能性 O(2^n)）
    - 思路1：记忆化搜索
    - 时间复杂度：O(n^2)
    - 空间复杂度：O(n)
    ```java
    int[] memo;
    public int integerBreak(int n) {
        memo = new int[n+1];
        Arrays.fill(memo, -1);
        return breakInteger(n);
    }

    // break
    private int breakInteger(int n){
        if(n==1)
            return 1;
        if(memo[n] != -1)
            return memo[n];
        int max = -1;
        for(int i=1; i<=n-1; i++)
            max = max3(max, i*(n-i), i*breakInteger(n-i));
        memo[n] = max;
        return max;
    }

    // max3
    private int max3(int a, int b, int c){
        return Math.max(a, Math.max(b, c));
    }
    ```
    - 思路2：动态规划
    - 时间复杂度：O(n^2)
    - 空间复杂度：O(n)
    ```java
    public int integerBreak(int n) {
        int[] memo = new int[n+1];
        Arrays.fill(memo, -1);
        
        memo[1] = 1;
        for(int i=2; i<=n; i++)
            // 求解memo[i]
            for(int j=1; j<=i-1; j++)
                memo[i] = max3(memo[i], j*(i-j), j*memo[i-j]);
        return memo[n];
    }

    // max3
    private int max3(int a, int b, int c){
        return Math.max(a, Math.max(b, c));
    }
    ```