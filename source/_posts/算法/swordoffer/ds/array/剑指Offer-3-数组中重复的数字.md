---
title: '剑指Offer: 3. 数组中重复的数字'
date: 2019-10-24 20:11:34
categories: Algorithm
tags: 
- 剑指Offer
- hashTable
---

- 数组中重复的数字
    - https://www.nowcoder.com/practice/623a5ac0ea5b4e5f95552655361ae0a8
    - 请找出数组中任意一个重复的数字
    <!-- more -->
    - 【WA】输入为空时，没有进行边界条件判断
    - 思路：Hash Table
    - 时间复杂度：O(n)
    - 空间复杂度：O(n)
    ```java
    public boolean duplicate(int numbers[],int length,int [] duplication) {
        if(numbers==null || length==0) {
            return false;
        }
        int[] freq = new int[length];
        for(int i=0; i<numbers.length; i++) {
            freq[numbers[i]]++;
            if(freq[numbers[i]]>1){
                duplication[0] = numbers[i];
                return true;
            }
        }
        return false;
    }
    ```