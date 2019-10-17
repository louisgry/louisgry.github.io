---
title: 'Collection: LeetCode 804. Unique Morse Code Words'
date: 2019-10-15 20:15:36
categories: 算法
tags: collection
---

- 804题解
    - 804 Unique Morse Code Words：https://leetcode.com/problems/unique-morse-code-words/
        - 返回不同摩斯码的个数
        - Input: words = ["gin", "zen", "gig", "msg"]
        - Output: 2
        - Explanation: There are 2 different transformations, "--...-." and "--...--.".
        <!-- more -->
    - 思路：使用Set返回不同摩斯码的个数
    - 时间复杂度：O(n+s)
    - 空间复杂度：O(n)
    ```java
    public int uniqueMorseRepresentations(String[] words) {
        String[] codes = {".-","-...","-.-.","-..",".","..-.","--.","....","..",".---","-.-",".-..","--","-.","---",".--.","--.-",".-.","...","-","..-","...-",".--","-..-","-.--","--.."};
        TreeSet<String> set = new TreeSet<String>();
        for(String word : words) {
            StringBuilder res = new StringBuilder();
            for(int i=0; i<word.length(); i++)
                res.append(codes[word.charAt(i)-'a']);
            set.add(res.toString());
        }
        return set.size();
    }
    ```