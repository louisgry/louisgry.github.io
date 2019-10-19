---
title: 'Pointers: LeetCode 125. Valid Palindrome'
date: 2019-09-14 23:31:13
categories: 算法
tags: pointers
---
- pointers
    - 125 Valid Palindrome：https://leetcode.com/problems/valid-palindrome/
        - 判断字符串是否为回文串，可跳过符号（空字符串是回文串）
        - Input: "A man, a plan, a canal: Panama"
        - Output: true
        <!-- more -->
    - 思路：双指针
    - 时间复杂度：O(n)
    - 空间复杂度：O(1)
    ```java
    public boolean isPalindrome(String s) {
        if(s==null)
            return true;
        String reg = "[^0-9a-zA-Z]";
        s = s.replaceAll(reg, "");
        int i=0, j=s.length()-1;
        boolean flag = true;
        while(i<j){
            char c1 = Character.toLowerCase(s.charAt(i));
            char c2 = Character.toLowerCase(s.charAt(j));
            if(c1 == c2){
                i++;
                j--;
            }
            else{
                flag = false;
                break;
            }
        }
        return flag;
    }
    ```