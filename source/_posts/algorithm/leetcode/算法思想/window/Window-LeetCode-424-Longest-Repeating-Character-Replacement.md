---
title: 'Window: LeetCode 424. Longest Repeating Character Replacement'
date: 2019-09-21 23:31:13
categories: Algorithm
tags: 
- window
- string
---
- window
    - 424 Longest Repeating Character Replacement：https://leetcode.com/problems/longest-repeating-character-replacement/
        - 如字符串s可以替换k次，求最长的连续重复字符的个数
        - Input: s = "ABAB", k = 2    
        - Output: 4
        - Explanation: Replace the two 'A's with two 'B's or vice versa.
        <!-- more -->
    - 思路：滑动窗口
    - 时间复杂度：O(n)
    - 空间复杂度：O(1)
    ```java
    public int characterReplacement(String s, int k) {
        int i=0, j=0;
        int max = 0;
        int maxCount = 0;
        int[] freq = new int[256];
        while (j<s.length()) {
            freq[s.charAt(j)]++;
            maxCount = Math.max(maxCount, freq[s.charAt(j)]);
            j++;
            while(j-i-maxCount > k) {
                freq[s.charAt(i++)]--;
            }
            max = Math.max(max, j-i);
        }
        return max;
    }
    ```