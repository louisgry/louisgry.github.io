---
title: "剑指Offer: 4. 二维数组中的查找"
date: 2019-10-24 20:41:43
categories: Algorithm
tags: 
- 剑指Offer
- pointers
---

- 4 二维数组中的查找
    - https://www.nowcoder.com/practice/abc3fe2ce8e146608e868a70efebf62e
    - 判断二维数组中是否含有某一整数
    <!-- more -->
    - 思路：双指针
    - 时间复杂度：O(n+m)
    - 空间复杂度：O(1)
    ```java
    public boolean Find(int target, int [][] array) {
        int i = 0;
        int j = array[0].length - 1;
        while(i<array.length && j>=0) {
            if(array[i][j] == target) {
                return true;
            }
            else if(array[i][j] > target) {
                j--;
            }
            else {
                i++;
            }
        }
        return false;
    }
    ```