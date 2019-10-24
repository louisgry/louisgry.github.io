---
title: 'Window: LeetCode 3. Longest Substring Without Repeating Characters'
date: 2019-09-17 23:31:13
categories: 算法
tags: 
- window
- string
---
- window
    - 3 Longest Substring Without Repeating Characters：https://leetcode.com/problems/longest-substring-without-repeating-characters/
        - 求字符串不重复子串的最长值
        - Input: "abcabcbb"
        - Output: 3 
        - Explanation: The answer is "abc", with the length of 3.
        <!-- more -->
    - 思路：滑动窗口，使用freq[256]记录重复字符
    - 时间复杂度：O(n)
    - 空间复杂度：O(1)
    ```java
    public int lengthOfLongestSubstring(String s) {
        int[] freq = new int[256]; // 记录字母是否重复
        int i=0, j=-1;
        int len = 0;
        while(i<s.length()){
            if(j+1<s.length() && freq[s.charAt(j+1)]==0){
                j++;
                freq[s.charAt(j)]++;
            }
            else{
                freq[s.charAt(i)]--;
                i++;
            }
            len = Math.max(len, j-i+1); // 注意：是最长子串max
        }
        return len;
    }
    ```