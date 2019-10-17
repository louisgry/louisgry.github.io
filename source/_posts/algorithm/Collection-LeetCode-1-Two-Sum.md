---
title: 'Collection: LeetCode 1. Two Sum'
date: 2019-10-12 20:18:05
tags: 算法
---

- collection
    - 1 Two Sum：https://leetcode.com/problems/two-sum/
        - 找出数组中和等于target的数字的下标（注意nums不是有序的）
        - Input: nums = [2, 7, 11, 15], target = 9
        - Output: [0,1]
        <!-- more -->
    - 思路：将元素a放入Map中，之后查找target-a是否存在
    - 时间复杂度：O(n)
    - 空间复杂度：O(n)
    ```java
    public int[] twoSum(int[] nums, int target) {
        HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
        int[] res = new int[2];
        for(int i=0; i<nums.length; i++) {
            int complement = target - nums[i];
            if(map.containsKey(complement)) {
                res[0] = i;
                res[1] = map.get(complement);
            }
            map.put(nums[i], i);
        }
        return res;
    }
    ```