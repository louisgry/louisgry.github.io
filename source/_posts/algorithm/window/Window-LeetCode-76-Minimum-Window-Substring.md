---
title: 'Window: LeetCode 76. Minimum Window Substring'
date: 2019-09-19 23:31:13
categories: 算法
tags: window
---

- window
    - 76 Minimum Window Substring：https://leetcode.com/problems/minimum-window-substring/
        - 求字符串s中包含字符串t的最小窗口
        - Input: S = "ADOBECODEBANC", T = "ABC"
        - Output: "BANC"
        <!-- more -->
    - 思路：滑动窗口
    - 时间复杂度：O(n)
    - 空间复杂度：O(1)
    ```java
    public String minWindow(String s, String t) {
        int[] sFreq = new int[256];
        int count = 0;
        int[] tFreq = new int[256];
        for (int i=0; i<t.length(); i++){
            tFreq[t.charAt(i)]++;
        }
        int minLen = s.length()+1;
        int startIndex = -1;
        int i=0, j=-1;
        while (i<s.length()){
            if (j+1<s.length() && count<t.length()){
                sFreq[s.charAt(j+1)]++;
                if (sFreq[s.charAt(j+1)] <= tFreq[s.charAt(j+1)]){
                    count++;
                }
                j++;
            } else {
                assert count<=t.length();
                if(count==t.length() && j-i+1<minLen){
                    minLen = j-i+1;
                    startIndex = i;
                }
                sFreq[s.charAt(i)]--;
                if (sFreq[s.charAt(i)] < tFreq[s.charAt(i)]){
                    count--;
                }
                i++;
            }
        }

        if (startIndex != -1){
            return s.substring(startIndex, startIndex+minLen);
        }
        return "";
    }
    ```