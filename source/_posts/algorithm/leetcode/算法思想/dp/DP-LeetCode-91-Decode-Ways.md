---
title: 'DP: LeetCode 91. Decode Ways'
date: 2019-09-10 23:31:13
categories: Algorithm
tags: 
- dp
- memorySearch
---
- dp
    - 91 Decode Ways：https://leetcode.com/problems/decode-ways/
        - 求数字解码对应的字母含义的组合个数
        - Input: "12"
        - Output: 2
        - Explanation: It could be decoded as "AB" (1 2) or "L" (12)
        <!-- more -->
    - 思路1：记忆化搜索
    - 时间复杂度：O(n)
    - 空间复杂度：O(n)
    ```java
    int[] memo;
    public int numDecodings(String s) {
        memo = new int[s.length()+1];
        Arrays.fill(memo, -1);
        memo[s.length()] = 1;
        return getNum(s, 0);
    }
    private int getNum(String s, int i) {
        int n = s.length();
        if(n == 0) {
            return 0;
        }
        if(memo[i] > -1) {
            return memo[i];
        }
        if(s.charAt(i) == '0') {
            return memo[i] = 0;
        }
        int res = getNum(s, i+1);
        if(i<s.length()-1 && Integer.parseInt(s.substring(i, i+2)) <= 26) {
            res += getNum(s, i+2);
        }
        return memo[i] = res;
    }
    ```
    - 思路2：动态规划
    ```java
    public int numDecodings(String s) {
        int n = s.length();
        if(n == 0) {
            return 0;
        }
        int[] memo = new int[n+1];
        memo[n] = 1;
        memo[n-1] = s.charAt(n-1)=='0' ? 0 : 1;
        for(int i=n-2; i>=0; i--) {
            if(s.charAt(i) == '0') {
                continue;
            }
            else {
                memo[i] = Integer.parseInt(s.substring(i, i+2)) <= 26 ? memo[i+1] + memo[i+2] : memo[i+1];
            }
        }
        return memo[0];
    }
    ```