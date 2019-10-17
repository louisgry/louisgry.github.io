---
title: 'Collection: LeetCode 202. Happy Number'
date: 2019-10-10 20:23:22
categories: 算法
tags: collection
---

- collection
    - 202 Happy Number：https://leetcode.com/problems/happy-number/
        - 判断一个数字是否为happy number
        - Input: 19
        - Output: true
        - Explanation: 
            - 1^2 + 9^2 = 82
            - 8^2 + 2^2 = 68
            - 6^2 + 8^2 = 100
            - 1^2 + 0^2 + 0^2 = 1
        <!-- more -->
    - 思路：Set
    - 时间复杂度：O(?)
    - 空间复杂度：O(?)
    ```java
   public boolean isHappy(int n) {
        if(n<1) {
            return false;
        }
        HashSet<Integer> set = new HashSet<Integer>();
        int t;
        int newN;
        while(n!=1 && !set.contains(n)) {
            set.add(n);
            newN = 0;
            while(n>0) {
                t = n%10;
                n /= 10;
                newN += t*t;
            }
            n = newN;
        }
        return n == 1;
    }
    ```