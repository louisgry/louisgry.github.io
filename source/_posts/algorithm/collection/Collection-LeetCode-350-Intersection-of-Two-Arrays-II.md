---
title: 'Collection: LeetCode 350. Intersection of Two Arrays II'
date: 2019-10-08 20:26:27
categories: 算法
tags: collection
---

- collection
    - 350 Intersection of Two Arrays II：https://leetcode.com/problems/intersection-of-two-arrays-ii/
        - 找两个数组的交集（包括重复的）
        - Input: nums1 = [1,2,2,1], nums2 = [2,2]
        - Output: [2,2]
        <!-- more -->
    - 思路：Map
    - 时间复杂度：O(n+mlogn)
    - 空间复杂度：O(n)
    ```java
    public int[] intersect(int[] nums1, int[] nums2) {
        HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
        for(int num : nums1) {
            if(!map.containsKey(num)) {
                map.put(num, 1);
            }
            else {
                map.put(num, map.get(num)+1);
            }
        }

        List<Integer> result = new ArrayList<Integer>();
        for(int num : nums2) {
            if(map.containsKey(num) && map.get(num)>0) {
                result.add(num);
                map.put(num, map.get(num)-1);
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