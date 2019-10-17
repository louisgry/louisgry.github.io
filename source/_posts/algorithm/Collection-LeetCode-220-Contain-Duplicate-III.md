---
title: 'Collection: LeetCode 220. Contain Duplicate III'
date: 2019-10-17 19:53:49
categories: 算法
tags: collection
---
- collection
    - 220 Contain Duplicate III：https://leetcode.com/problems/contains-duplicate-iii/
        - 判断数组是否在k长度内有差值不大于t的两个数
        - Input: nums = [1,2,3,1], k = 3, t = 0
        - Output: true
        <!-- more -->
    - 思路：set+滑动窗口
    - 时间复杂度：O(nlogk)
    - 空间复杂度：O(k)
    ```java
    public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
        // 需要用Long和long，会发生整型溢出
        TreeSet<Long> set = new TreeSet<Long>();
        for(int i=0; i<nums.length; i++) {
            if(set.ceiling((long)nums[i]-(long)t) != null && set.ceiling((long)nums[i]-(long)t)<=(long)nums[i]+(long)t){
                return true;
            }
            set.add((long)nums[i]);
            if(set.size() == k+1){
                set.remove((long)nums[i-k]);
            }
        }
        return false;
    }
    ```