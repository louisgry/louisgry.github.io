---
title: 'Greedy: LeetCode 435. Non-overlapping Intervals'
date: 2019-10-27 19:11:55
categories: 算法
tags: 
- greedy
- dp
---
- greedy、dp
    - 435 Non-overlapping Intervals：https://leetcode.com/problems/non-overlapping-intervals/
        - 给定一个区间，问最少删除多少个区间，可以让这些区间之间不重叠
        - Input: [[1,2],[2,3],[3,4],[1,3]]
        - Output: 1
        <!-- more -->
    - 思路：贪心，选择区间结尾最早的且和前面不重叠
    - 时间复杂度：O(n)
    - 空间复杂度：O(n)
    ```java
    public int eraseOverlapIntervals(int[][] intervals) {
        if(intervals.length == 0) {
            return 0;
        }
        Arrays.sort(intervals, new Comparator<int[]>() {
            @Override
            public int compare(int[] o1, int[] o2) {
                if (o1[1] != o2[1]) {
                    return o1[1] - o2[1];
                }
                return o1[0] - o2[0];
            }
        });
        int res = 1;
        int pre = 0;
        for(int i=1; i<intervals.length; i++) {
            if(intervals[i][0] >= intervals[pre][1]) {
                res++;
                pre = i;
            }
        }
        return intervals.length - res;
    }
    ```
    - 思路2：动态规划
    - 时间复杂度：O(n)
    - 空间复杂度：O(n)
    ```java
    public int eraseOverlapIntervals(int[][] intervals) {
        if(intervals.length == 0) {
            return 0;
        }

        Arrays.sort(intervals, new Comparator<int[]>() {
            @Override
            public int compare(int[] o1, int[] o2) {
                if (o1[0] != o2[0]) {
                    return o1[0] - o2[0];
                }
                return o1[1] - o2[1];
            }
        });

        // memo[i] 表示使用 intervals[0...i] 的区间能构成的最长不重叠区间序列的长度；
        int[] memo = new int[intervals.length];
        Arrays.fill(memo, 1);

        for (int i = 1; i < intervals.length; i++)
            for (int j = 0; j < i; j++)
                if (intervals[i][0] >= intervals[j][1])
                    memo[i] = Math.max(memo[i], 1 + memo[j]);

        int res = 0;
        for(int i = 0; i < memo.length; i++) {
            res = Math.max(res, memo[i]);
        }

        return intervals.length - res;
    }
    ```