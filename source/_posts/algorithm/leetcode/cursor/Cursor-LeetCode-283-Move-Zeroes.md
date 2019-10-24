---
title: 'Cursor: LeetCode 283. Move Zeroes'
date: 2019-09-01 23:31:13
categories: 算法
tags: 
- cursor
---
- cursor
    - 283 Move Zeroes：https://leetcode.com/problems/move-zeroes/
        - 把数组中的零移到后面去
        - Input: [0,1,0,3,12]
        - Output: [1,3,12,0,0]
        <!-- more -->
    - 问题：使用了O(n)的空间，可以原地吗？
    - 优化1：用一个k游标去保存非零元素的个数
    ```java
    public void moveZeroes(int[] nums) {
        // 优化1：原地，不用O(n)的空间
        // 时间复杂度：O(n)
        // 空间复杂度：O(1)
        int k = 0;
        for(int i=0; i<nums.length; i++)
            if(nums[i]!=0)
                nums[k++] = nums[i];
        for(int i=k; i<nums.length; i++)
            nums[i] = 0;
    }
    ```
    - 优化2：swap 0和非0（用游标k，就不用再for循环了）
    ```java
    public void moveZeroes(int[] nums) {
        // 优化2：swap 0和非0（用游标k，就不用再for循环了）
        // 时间复杂度：O(n)
        // 空间复杂度：O(1)
        int k = 0;
        for(int i=0; i<nums.length; i++)
            if(nums[i]!=0){
                // swap(nums[i], nums[k])
                if(i!=k){
                    int t = nums[i];
                    nums[i] = nums[k];
                    nums[k] = t;
                    k++;
                }
                else // i==k
                    k++;
            }
    }
    ```
