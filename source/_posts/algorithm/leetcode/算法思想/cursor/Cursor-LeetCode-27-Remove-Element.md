---
title: 'Cursor: LeetCode 27. Remove Element'
date: 2019-09-02 23:31:13
categories: Algorithm
tags: 
- cursor
---
- cursor
    - 27 Remove Element：https://leetcode.com/problems/remove-element/ 
        - 给定nums，删除指定的值val
        - Input: nums = [3,2,2,3], val = 3,
        - Output: Your function should return length = 2, with the first two elements of nums being 2.
        <!-- more -->
    - 思路：cursor
    - 时间复杂度：O(n)
    - 空间复杂度：O(1)
     ```java
    public int removeElement(int[] nums, int val) {
        int k = 0;
        for(int i=0; i<nums.length; i++){
            if(nums[i] != val){
                if(i!=k){
                    int t = nums[i];
                    nums[i] = nums[k];
                    nums[k] = t;
                    k++;
                }
                else
                    k++;
            }
        }
        return k;
    }
     ```