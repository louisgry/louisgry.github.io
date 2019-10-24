---
title: 'Window: LeetCode 209. Minimum Size Subarray Sum'
date: 2019-09-05 23:31:13
categories: 算法
tags: 
- window
---
- window
    - 209 Minimum Size Subarray Sum：https://leetcode.com/problems/minimum-size-subarray-sum/ (Medium)
        - 求最短子序和sum<=s
        - Input: s = 7, nums = [2,3,1,2,4,3]
        - Output: 2
        <!-- more -->
        - 注意：子数组是否连续？没有解怎么办？
    - 滑动窗口：维护nums[i...j]，如果sum比s小 sum += nums[++j]，否则sum -= nums[i++]
    - 时间复杂度：O(n)
    - 空间复杂度：O(1)
    ```java
    public int minSubArrayLen(int s, int[] nums) {
        int i = 0, j = -1; // j最开始是-1，不包含任何元素
        int sum = 0;
        int min= nums.length+1; // 设一个最大的数n+1取不到
        while (i<nums.length){ // 遍历[0...n-1]，不是i<j
            if(j+1<nums.length && sum < s){
                j++;
                sum += nums[j];
            }
            else{
                sum -= nums[i];
                i++;
            }

            if(sum>=s)
               min = Math.min(len, j-i+1);
        }
        if(min==nums.length+1) // 没有判断过不了输出为0的测试用例：[1,1] 3
            return 0;
        return min;
    }
    ```
    - 思路2：二分搜索：对每一个i，使用二分搜索找j
    - 时间复杂度：O(nlogn)
    - 空间复杂度：O(n)
    ```java
    public int minSubArrayLen(int s, int[] nums) {
        int[] sums = new int[nums.length+1]; // 从1开始记录前面的和
        sums[0] = 0;
        for(int i=1; i<=nums.length; i++) // 注意边界：i<=n
            sums[i] = sums[i-1] + nums[i-1];
        int len = nums.length+1;
        for(int i=0; i<nums.length; i++){
            // 基于二分搜索的lowerBound: 在有序数组nums中寻找大于等于target的最小值
            int j = lowerBound(sums, sums[i]+s);
            if(j != sums.length)
                len = Math.min(len, j-i);
        }
        if(len == nums.length+1)
            return 0;
        return len;
    }

    private int lowerBound(int[] nums, int target){
        int l=0, r=nums.length;
        while(l != r){
            int mid = l + (r-l)/2;
            if(nums[mid]>=target)
                r = mid;
            else
                l = mid+1;
        }
        return l;
    }
    ```