---
title: 'Pointers: LeetCode 345. Reverse Vowels of a String'
date: 2019-09-16 23:31:13
categories: 算法
tags: 
- pointers
- string
---
- pointers
    - 345 Reverse Vowels of a String：https://leetcode.com/problems/reverse-vowels-of-a-string/
        - swap字符串中的元音字母
        - Input: "hello"
        - Output: "holle"
        <!-- more -->
    - 思路：双指针
    - 时间复杂度：O(n)
    - 空间复杂度：O(n)
    ```java
    public String reverseVowels(String s) {
        int i=0, j=s.length()-1;
        char[] arr = s.toCharArray();
        String vowels = "aeiouAEIOU";
        while(i<j){
            if(!vowels.contains(arr[i]+"")){
                i++;
            }
            if(!vowels.contains(arr[j]+"")){
                j--;
            }
            if(vowels.contains(arr[i]+"")&&vowels.contains(arr[j]+"")){
                char c = arr[i];
                arr[i] = arr[j];
                arr[j] = c;
                i++;
                j--;
            }
        }
        return new String(arr);
    }
    ```