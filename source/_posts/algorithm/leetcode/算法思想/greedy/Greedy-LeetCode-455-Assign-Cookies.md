---
title: 'Greedy: LeetCode 455. Assign Cookies'
date: 2019-10-27 16:57:29
categories: Algorithm
tags: 
- greedy
---
- greedy
    - 455 Assign Cookies：https://leetcode.com/problems/assign-cookies/
        - 分饼干，s为饼干大小，g为满意的最小值，返回最多可以让几个小朋友开心
        - Input: g=[1,2,3], s=[1,1]
        - Output: 1
        <!-- more -->
    - 思路：贪心，最大的饼干满足最贪的小朋友
    - 时间复杂度：O(nlogn)
    - 空间复杂度：O(1)
    ```java
    public int findContentChildren(int[] g, int[] s) {
        Arrays.sort(g);
        Arrays.sort(s);

        int gi = g.length-1, si = s.length-1;
        int res = 0;
        while(gi>=0 && si>=0) {
            if(s[si] >= g[gi]) {
                res++;
                si--;
                gi--;
            }
            else {
                gi--;
            }
        }
        return res;
    }
    ```

