---
title: 'DP: LeetCode 416. Partition Equal Subset Sum'
date: 2019-09-11 23:31:13
categories: 算法
tags:  
- dp
- memorySearch
---
- dp
    - 416 Partition Equal Subset Sum：https://leetcode.com/problems/partition-equal-subset-sum/
        - 给定数组nums，求其是否可以被分成两个子数组，其和相等（注意：最多有200个数字，每个数字最大为100）
        - Input: [1, 5, 11, 5]
        - Output: true
        - Explanation: The array can be partitioned as [1, 5, 5] and [11].
        <!-- more -->
    - 是背包问题：在n个物品中选一定物品，是否可以完全填满sum/2的背包
    - Key：状态：F(i,c) = F(i-1,c) || F(i-1, c-w(i))
    - 数据规模 $n*sum/2=100*100*200/2$：最多有200个数字，每个数字最大为100
    - 思路1：记忆化搜索
    - 时间复杂度：O(n*sum)
    - 空间复杂度：O(n*sum)
    ```java
    int[][] memo; // memo[i][c]: -1是未计算，0是不可填充，1是可以填充
    public boolean canPartition(int[] nums) {
        int sum = 0;
        for(int i=0; i<nums.length; i++){
            assert nums[i]>0;
            sum += nums[i];
        }
        if(sum%2 != 0)
            return false;
        memo = new int[nums.length][sum/2+1]; // 表示0...sum/2，故+1
        for(int i=0; i<nums.length; i++)
            Arrays.fill(memo[i], -1);
        return tryPartition(nums, nums.length-1, sum/2);
    }
    // 使用nums[0...index]是否可以完全填满sum的背包
    private boolean tryPartition(int[] nums, int index, int sum){
        if(sum==0) // 背包没有空间，即填满了
            return true;
        if(sum<0 || index<0)
            return false;
        if(memo[index][sum] != -1)
            return memo[index][sum] == 1;
        memo[index][sum] = tryPartition(nums, index-1, sum) || tryPartition(nums, index-1, sum-nums[index]) ? 1: 0;
        return memo[index][sum] == 1;
    }
    ```
    - 思路2：动态规划
    - 时间复杂度：O(n*sum)
    - 空间复杂度：O(sum)
    ```java
    public boolean canPartition(int[] nums) {
        int sum = 0;
        int n = nums.length;
        for(int i=0; i<n; i++){
            assert nums[i]>0;
            sum += nums[i];
        }
        int C = sum/2;
        if(sum%2 != 0)
            return false;
        boolean[] memo = new boolean[C+1]; // 表示0...sum/2
        for(int j=0; j<=C; j++)
            memo[j] = (nums[0] == j); // j -> c
        for(int i=1; i<n; i++)
            for(int j=C; j>=nums[i]; j--)
                memo[j] = memo[j] || memo[j-nums[i]];
            
        return memo[C];
    }
    ```