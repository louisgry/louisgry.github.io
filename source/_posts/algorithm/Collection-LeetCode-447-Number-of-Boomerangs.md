---
title: 'Collection: LeetCode 447. Number of Boomerangs'
date: 2019-10-14 20:16:20
tags: 算法
---

- collection
    - 447 Number of Boomerangs：https://leetcode.com/problems/number-of-boomerangs/
        - 寻找符合Boomerangs定义的三元组的个数
        - Input: [[0,0],[1,0],[2,0]]
        - Output: 2
        - Explanation: The two boomerangs are [[1,0],[0,0],[2,0]] and [[1,0],[2,0],[0,0]]
        <!-- more -->
    - 思路：以i为枢纽点，把其他点跟i的距离放入Map，查找距离相同点的个数的组合
    - 时间复杂度：O(n^2)
    - 空间复杂度：O(n)
    ```java
    public int numberOfBoomerangs(int[][] points) {
        int res = 0;
        // 遍历枢纽点i
        for(int i=0; i<points.length; i++) {
            // 把其他点的距离放入Map
            HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
            for(int j=0; j<points.length; j++) {
                if(j != i) {
                    // 对比距离时使用距离的平方，无浮点数误差问题
                    int dist = dis(points[i], points[j]);
                    if(!map.containsKey(dist))
                        map.put(dist, 1);
                    else
                        map.put(dist, map.get(dist)+1);
                }
            }
            // 计算相同距离点的个数的组合
            for(Integer dis : map.keySet())
                res += map.get(dis) * (map.get(dis)-1);
        }
        return res;
    }
    private int dis(int[] a, int[] b) {
        return (a[0]-b[0])*(a[0]-b[0]) + (a[1]-b[1])*(a[1]-b[1]);
    }
    ```