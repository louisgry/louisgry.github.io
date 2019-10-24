---
title: 'DP: LeetCode 70. Climbing Stairs'
date: 2019-09-06 23:31:13
categories: 算法
tags:  
- dp
- memorySearch
---
- dp
    - 70 Climbing Stairs：https://leetcode.com/problems/climbing-stairs/
        - 给定n，求step的组合，step仅为1和2
        - Input: 2
        - Output: 2
        - Explanation: There are two ways to climb to the top.
            - 1 step + 1 step
            - 2 steps
            <!-- more -->
    - 递归解法：StackOverflowError栈溢出（原因没有指定n=1的情况）
        - 过不了：Time Limit Exceeded 
    - Key：爬上n-1阶台阶再爬1阶，或者爬上n-2阶台阶再爬2阶
    - 记忆化搜索memo：自顶向下的解决问题
    - 时间复杂度：O(n)
    - 空间复杂度：O(n)
    ```java
    int[] memo;
    public int climbStairs(int n) {
        memo = new int[n+2];
        Arrays.fill(memo, -1);
        return climbing(n);
    }
    
    public int climbing(int n){
        if(n==1)
            return 1;
        if(n==2)
            return 2;
        if(memo[n]==-1)
            memo[n] = climbing(n-1) + climbing(n-2);
        return memo[n];
    }
    ```
    - 思路2：动态规划：自底向上的解决问题
    - 时间复杂度：O(n)
    - 空间复杂度：O(n)
    ```java
    public int climbStairs(int n) {
        int[] memo = new int[n+2];
        Arrays.fill(memo, -1);
        memo[1] = 1;
        memo[2] = 2;
        for(int i=3; i<=n; i++)
           memo[i] = memo[i-1]+memo[i-2];
        return memo[n];
    }
    ```