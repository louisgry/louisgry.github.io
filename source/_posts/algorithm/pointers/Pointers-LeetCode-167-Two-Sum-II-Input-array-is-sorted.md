---
title: 'Pointers: LeetCode 167. Two Sum II - Input array is sorted'
date: 2019-09-04 23:31:13
categories: 算法
tags: pointers
---
- pointers
    - 167 Two Sum II：https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/ 
        - 给定排序好的数组，找出两数和等于target的下标，index1 < index2
        - Input: numbers = [2,7,11,15], target = 9
        - Output: [1,2]
        <!-- more -->
    - 思路：使用双指针，一个在前，一个在后，指针随sum与target的差距而变化
    - 时间复杂度：O(n)
    - 空间复杂度：O(1)
    ```java
    public int[] twoSum(int[] numbers, int target) {
        int index1;
        int index2 = numbers.length-1;
        int[] result = {0, 0};
        for(index1=0; index1<numbers.length; ){
            if(numbers[index1]+numbers[index2]==target){
                result[0] = index1+1;
                result[1] = index2+1;
                break;
            }
            else if(numbers[index1]+numbers[index2]<target)
                index1++;
            else
                index2--;
        }
        return result;
    }
    ```