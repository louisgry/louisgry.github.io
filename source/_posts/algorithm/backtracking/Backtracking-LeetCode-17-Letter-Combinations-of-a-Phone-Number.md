---
title: 'Backtracking: 17. Letter Combinations of a Phone Number'
date: 2019-09-22 23:31:13
categories: 算法
tags: 
- backtracking
---
- backtracking
    - 17 Letter Combinations of a Phone Number：https://leetcode.com/problems/letter-combinations-of-a-phone-number/
        - 返回数字字符串的所有字母组合
        - Input: "23"
        - Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"]
        <!-- more -->
    - 思路：递归
    - 时间复杂度：O(2^n)
    - 空间复杂度：O(n)
    ```java
    private String[] letterMap = {
            " ",
            "",
            "abc",
            "def",
            "ghi",
            "jkl",
            "mno",
            "pqrs",
            "tuv",
            "wxyz"
    };
    private ArrayList<String> resultList;
    public List<String> letterCombinations(String digits) {
        resultList = new ArrayList<String>();
        if (digits.equals("")) {
            return resultList;
        }
        findCombination(digits, 0, "");
        return resultList;
    }
    // s保存从digits[0...index-1]的字符串
    private void findCombination(String digits, int index, String s) {
        if (index==digits.length()) {
            resultList.add(s);
            return;
        }
        Character c = digits.charAt(index);
        String letters = letterMap[c-'0'];
        for (int i=0; i<letters.length(); i++) {
            findCombination(digits, index+1, s+letters.charAt(i));
        }
        return;
    }
    ```