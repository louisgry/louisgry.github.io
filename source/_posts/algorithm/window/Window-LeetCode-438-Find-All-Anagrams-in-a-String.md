---
title: 'Window: LeetCode 438. Find All Anagrams in a String'
date: 2019-09-18 23:31:13
categories: 算法
tags: window
---
- window
    - 438 Find All Anagrams in a String：https://leetcode.com/problems/find-all-anagrams-in-a-string/
        - 找字符串的Anagram的开始下标
        - Input: s: "cbaebabacd" p: "abc"
        - Output: [0, 6]
        - Explanation:
            - The substring with start index = 0 is "cba", which is an anagram of "abc".
            - The substring with start index = 6 is "bac", which is an anagram of "abc".
            <!-- more -->
    - 思路：滑动窗口
    - 时间复杂度：O(m+n)
    - 空间复杂度：O(1)
    ```java
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> indexList = new ArrayList<Integer>();
        int i=0, j=0;
        int count = p.length();
        int[] freq = new int[256];
        for (char c : p.toCharArray()){
            freq[c]++;
        }
        while (j < s.length()){
            if (freq[s.charAt(j++)]-- > 0){
                count--;
            }
            if (count == 0){
                indexList.add(i);
            }
            if (j-i==p.length() && freq[s.charAt(i++)]++ >= 0){
                count++;
            }
        }
        return indexList;
    }
    ```