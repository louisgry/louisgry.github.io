---
title: '剑指Offer: 10. 斐波那契数列'
date: 2019-11-12 20:22:56
categories: Algorithm
tags: 
- 剑指Offer
- recursion
- memorySearch
- dp
---

- 10 斐波那契数列
    - https://www.nowcoder.com/practice/c6c7742f5ba7442aada113136ddea0c3
    - 输出斐波那契数列的第n项（从0开始，第0项为0）
    <!-- more -->
    - 思路1：递归
    - 时间复杂度：O(2^n)
    - 空间复杂度：O(1)
    ```java
    public int Fibonacci(int n) {
        // 递归
        if(n==0)
            return 0;
        if(n==1)
            return 1;
        return Fibonacci(n-1)+Fibonacci(n-2);
    }
    ```
    - 思路2：记忆化搜索
    - 时间复杂度：O(n)
    - 空间复杂度：O(n)
    ```java
    int[] memo;
    public int Fibonacci(int n) {
        // 记忆化搜索
        memo = new int[n+2];
        Arrays.fill(memo, -1);
        return getFib(n);
    }
    private int getFib(int n) {
        if(n==0)
            return 0;
        if(n==1)
            return 1;
        if(memo[n] == -1)
            memo[n] = getFib(n-1) + getFib(n-2);
        return memo[n];
    }
    ```
    - 思路3：动态规划
    - 时间复杂度：O(n)
    - 空间复杂度：O(n)
    ```java
    public int Fibonacci(int n) {
        // 动态规划
        int[] memo = new int[n+2];
        if(n == 0) {
            memo[0] = 0;
        }
        else if(n == 1) {
            memo[1] = 1;
        }
        else {
            memo[0] = 0;
            memo[1] = 1;
        }
        for(int i=2; i<=n; i++) {
            memo[i] = memo[i-1] + memo[i-2];
        }
        return memo[n];
    }
    ```