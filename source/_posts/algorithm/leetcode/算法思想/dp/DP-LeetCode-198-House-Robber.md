---
title: 'DP: LeetCode 198. House Robber'
date: 2019-09-08 23:31:13
categories: Algorithm
tags: 
- dp
- memorySearch
---
- dp
    - 198 House Robber：https://leetcode.com/problems/house-robber/ 
        - 给定nums数组代表连续相邻的家庭所有的money，若抢相邻的两家会触发报警，求所能抢的money的最大值
        - Input: [1,2,3,1]
        - Output: 4
        - Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
            - Total amount you can rob = 1 + 3 = 4.
            <!-- more -->
    - Key
        - 状态（函数的定义）：考虑偷取[x...n-1]范围里的房子
        - 状态转移方程：f(0)=max{v(0)+f(2), v(1)+f(3), v(2)+f(4), ..., v(n-3)+f(n-1), v(n-2), v(n-1)}
    - 思路1：记忆化搜索
    - 时间复杂度：O(n^2)
    - 空间复杂度：O(n)
    ```java
    int[] memo;
    public int rob(int[] nums) {
        memo = new int[nums.length];
        Arrays.fill(memo, -1);
        return tryRobber(nums, 0);
    }
    // 考虑抢劫[x...n-1]
    private int tryRobber(int[] nums, int index){
        if(index >= nums.length)
            return 0;
        if(memo[index] != -1)
            return memo[index];
        int max = 0;
        for(int i=index; i<nums.length; i++)
            max = Math.max(max, nums[i] + tryRobber(nums, i+2));
        memo[index] = max;
        return max;
    }
    ```
    - 思路2：动态规划
    - 时间复杂度：O(n^2)
    - 空间复杂度：O(n)
    ```java
    public int rob(int[] nums) {
        int n = nums.length;
        if(n==0)
            return 0;
        int[] memo = new int[n];
        Arrays.fill(memo, -1);
        memo[n-1] = nums[n-1];
        for(int i=n-2; i>=0; i--){
            // memo[i]
            for(int j=i; j<n; j++)
// memo[i] = Math.max(memo[i], nums[j] + memo[j+2]);
                memo[i] = Math.max(memo[i], nums[j] + (j+2<n ? memo[j+2] : 0));
        }
        return memo[0];
    }
    ```