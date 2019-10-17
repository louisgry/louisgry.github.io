---
title: 'Collection: LeetCode 242. Valid Anagram'
date: 2019-10-09 20:25:21
tags: 算法
---

- collection
    - 242 Valid Anagram：https://leetcode.com/problems/valid-anagram/
        - 判断两个字符串是否为回文串
        - Input: s = "anagram", t = "nagaram"
        - Output: true
        <!-- more -->
    - 思路：Hash Table
    - 时间复杂度：O(n)
    - 空间复杂度：O(26)
    ```java
    public boolean isAnagram(String s, String t) {
        if(s.length() != t.length()){
            return false;
        }
        // HashTable
        int[] freq = new int[26];
        for(int i=0; i<s.length(); i++) {
            freq[s.charAt(i)-'a']++;
        }

        for(int i=0; i<t.length(); i++){
            freq[t.charAt(i)-'a']--;
            if(freq[t.charAt(i)-'a'] < 0) {
                return false;
            }
        }
        return true;
    }
    ```