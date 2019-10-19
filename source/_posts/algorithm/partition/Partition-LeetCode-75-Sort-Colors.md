---
title: 'Partition: LeetCode 75. Sort Colors'
date: 2019-09-03 23:31:13
categories: 算法
tags: partition
---
- partition
    - 75 Sort Color：https://leetcode.com/problems/sort-colors/
        - 对三种颜色进行排序，用0,1,2表示颜色
        - Input: [2,0,2,1,1,0]
        - Output: [0,0,1,1,2,2]
        <!-- more -->
    - 思路：使用三路快排：只需一次for循环
    - 时间复杂度：O(n)
    - 空间复杂度：O(1)
    ```java
    public void sortColors(int[] nums) {
        int zero = -1;
        int two = nums.length;
        for(int i=0; i<two; ){
            // 1
            if(nums[i]==1)
                i++;
            // 2
            else if(nums[i]==2){
                two--;
                swap(nums, i, two);
            }
            // 0
            else{
                assert(nums[i]==0);
                zero++;
                swap(nums, i, zero);
                i++;
            }
        }
    }
    
    public void swap(int[] nums, int i, int j){
        int t = nums[i];
        nums[i] = nums[j];
        nums[j] = t;
    }
    ```