---
title: 'DP: LeetCode 300. Longest Increasing Subsequence'
date: 2019-09-12 23:31:13
categories: 算法
tags: dp
---
- dp
    - 300 Longest Increasing Subsequence：https://leetcode.com/problems/longest-increasing-subsequence/
        - 求最长上升子序列的length 
        - Input: [10,9,2,5,3,7,101,18]
        - Output: 4 
        - Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4. 
        <!-- more -->
    - Key
        - 状态：f(i)表示以第i个数字为结尾的最长上升子序列的长度（即[0...i]里选择nums[i]可以获得的最长上升子序列）
        - 转移方程：f(i) = max(1+f(j) if nums[j] < nums[i])
    - 思路1：记忆化搜索
    - 时间复杂度：O(n^2)
    - 空间复杂度：O(n)
    ```java
    int[] memo;
    public int lengthOfLIS(int[] nums) {
        if(nums.length==0)
            return 0;
        memo = new int[nums.length];
        Arrays.fill(memo, -1);
        int max = 0;
        for(int i=0; i<nums.length; i++)
            max = Math.max(max, getMaxLen(nums, i));
        return max;
    }
    private int getMaxLen(int[] nums, int index){
        if(memo[index] != -1)
            return memo[index];
        int max = 1;
        for(int i=0; i<index; i++)
            if(nums[i] < nums[index])
                max = Math.max(max, 1+getMaxLen(nums, i));
        memo[index] = max;
        return memo[index];
    }
    ```
    - 思路2：动态规划
    - 时间复杂度：O(n^2)
    - 空间复杂度：O(n)
    ```java
    public int lengthOfLIS(int[] nums) {
        if(nums.length==0)
            return 0;
        int[] memo = new int[nums.length];
        Arrays.fill(memo, 1);
        for(int i=1; i<nums.length; i++)
            for(int j=0; j<i; j++)
                if(nums[j]<nums[i])
                    memo[i] = Math.max(memo[i], 1+memo[j]);
        int max = 0;
        for(int i=0; i<nums.length; i++)
            max = Math.max(max, memo[i]);
        return max;
    }
    ```