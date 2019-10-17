---
title: 'Collection: LeetCode 290. Word Pattern'
date: 2019-10-11 20:18:41
tags: 算法
---

- collection
    - 290 Word Pattern：https://leetcode.com/problems/word-pattern/
        - 判断所给的string是否是pattern的形式
        - Input: pattern = "abba", str = "dog cat cat dog"
        - Output: true
        <!-- more -->
    - 思路：Map
    - 时间复杂度：O(nlogm)
    - 空间复杂度：O(n+m)
    ```java
    public boolean wordPattern(String pattern, String str) {
        HashMap<Character, String> map = new HashMap<Character, String>();
        char[] patterns = pattern.toCharArray();
        String[] strs = str.split(" ");

        if(patterns.length != strs.length) {
            return false;
        }

        for(int i=0; i<patterns.length; i++) {
            char c = patterns[i];
            if(!map.containsKey(c)) {
                if(map.containsValue(strs[i])) {
                    return false;
                }
                map.put(c, strs[i]);
            }
            else {
                String value = map.get(c);
                if(!value.equals(strs[i])) {
                    return false;
                }
            }
        }
        return true;
    }
    ```