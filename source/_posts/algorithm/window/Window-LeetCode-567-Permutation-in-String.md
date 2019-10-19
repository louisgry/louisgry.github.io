---
title: 'Window: LeetCode 567. Permutation in String'
date: 2019-09-20 20:06:03
categories: 算法
tags: window
---
- window
    - 567 Permutation in String：https://leetcode.com/problems/permutation-in-string/
        - 在字符串s2里找是否有字符串s1的全排列
        - Input: s1 = "ab" s2 = "eidbaooo"
        - Output: True
        - Explanation: s2 contains one permutation of s1 ("ba")
        <!-- more -->
    - 思路：滑动窗口
    - 时间复杂度：O(n)
    - 空间复杂度：O(1)
    ```java
    public boolean checkInclusion(String s1, String s2) {
        int[] freq = new int[256];
        for (char c : s1.toCharArray()) {
            freq[c]++;
        }
        int i=0, count=0;
        for (int j=0; j<s2.length(); j++) {
            freq[s2.charAt(j)]--;
            if (freq[s2.charAt(j)] >= 0) {
                count++;
            }
            if (j >= s1.length()) {
                freq[s2.charAt(i)]++;
                if (freq[s2.charAt(i)] >= 1) {
                    count--;
                }
                i++;
            }
            if (count==s1.length()) {
                return true;
            }
        }
        return false;
    }
    ```