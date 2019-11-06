---
title: 'Pointers: LeetCode 11. Container With Most Water'
date: 2019-09-15 23:31:13
categories: Algorithm
tags: 
- pointers
---
- pointers
    - 11 Container With Most Water：https://leetcode.com/problems/container-with-most-water/ 
        - 求Container的最大面积
        - Input: [1,8,6,2,5,4,8,3,7]
        - Output: 49
        <!-- more -->
    - 思路：双指针
    - 时间复杂度：O(n)
    - 空间复杂度：O(1)
    ```java
    public int maxArea(int[] height) {
        int area = 0;
        int i = 0;
        int j = height.length-1;
        while(i<j){
            int t = (j-i)*Math.min(height[i], height[j]);
            if(t>area)
                area = t;
            if(height[i]<height[j])
                i++;
            else
                j--;
        }
        return area;
    }
    ```