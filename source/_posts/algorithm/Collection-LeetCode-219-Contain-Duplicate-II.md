---
title: 'Collection: LeetCode 219. Contain Duplicate II'
date: 2019-10-16 20:03:20
tags: 算法
---

- collection
    - 219 Contains Duplicate II：https://leetcode.com/problems/contains-duplicate-ii/
        - 判断数组是否在k长度内有重复元素
        - Input: nums = [1,2,3,1], k = 3
        - Output: true
		<!-- more -->
    - 思路：Set+滑动窗口
    - 时间复杂度：O(n)
    - 空间复杂度：O(k)
    ```java
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        HashSet<Integer> set = new HashSet<Integer>();
        for(int i=0; i<nums.length; i++) {
            if(set.contains(nums[i])){
                return true;
            }
            set.add(nums[i]);

            // 保持set中最多有k个元素（边界：i-k）
            if(set.size() == k+1){
                set.remove(nums[i-k]);
            }
        }
        return false;
    }
    ```
