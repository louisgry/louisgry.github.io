---
title: 'Collection: LeetCode 454. 4Sum II'
date: 2019-10-13 20:16:56
categories: Algorithm
tags:  
- collection
- map
---

- collection
    - 454 4Sum II：https://leetcode.com/problems/4sum-ii/
        - 求四个整型数组的有多少种组合相加等于0
        - Input: A = [ 1, 2], B = [-2,-1], C = [-1, 2], D = [ 0, 2]
        - Output: 2
        - Explanation: The two tuples are:
            1. (0, 0, 0, 1) -> A[0] + B[0] + C[0] + D[1] = 1 + (-2) + (-1) + 2 = 0
            2. (1, 1, 0, 0) -> A[1] + B[1] + C[0] + D[0] = 2 + (-1) + (-1) + 0 = 0
        <!-- more -->
    - 思路：将C+D的所有组合放入Map中
    - 时间复杂度：O(n^2)
    - 空间复杂度：O(n^2)
    ```java
    public int fourSumCount(int[] A, int[] B, int[] C, int[] D) {
        HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
        for(int i=0; i<C.length; i++) {
            for (int j = 0; j < D.length; j++) {
                int sum = C[i] + D[j];
                if(!map.containsKey(sum)) {
                    map.put(sum, 1);
                }
                else {
                    map.put(sum, map.get(sum)+1);
                }
            }
        }

        int res = 0;
        for(int i=0; i<A.length; i++) {
            for (int j=0; j<B.length; j++) {
                if(map.containsKey(0-A[i]-B[j])) {
                    res += map.get(0-A[i]-B[j]);
                }
            }
        }
        return res;
    }
    ```
