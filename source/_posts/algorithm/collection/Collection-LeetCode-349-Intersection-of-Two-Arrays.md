---
title: 'Collection: LeetCode 349. Intersection of Two Arrays'
date: 2019-10-07 20:27:37
categories: 算法
tags: collection
---

- collection
    - 349 Intersection of Two Arrays：https://leetcode.com/problems/intersection-of-two-arrays/
        - 找两个数组的交集
        - Input: nums1 = [1,2,2,1], nums2 = [2,2]
        - Output: [2]
        <!-- more -->
    - 思路：Set
    - 时间复杂度：O(n+m)
    - 空间复杂度：O(n)
    ```java
    public int[] intersection(int[] nums1, int[] nums2) {
        HashSet<Integer> record = new HashSet<Integer>();
        for(int num : nums1) {
            record.add(num);
        }

        HashSet<Integer> result = new HashSet<Integer>();
        for(int num : nums2) {
            if(record.contains(num)) {
                result.add(num);
            }
        }

        int[] res = new int[result.size()];
        int index = 0;
        for(Integer num : result) {
            res[index++] = num;
        }
        return res;
    }
    ```