---
title: 'Backtracking: LeetCode 77. Combinations'
date: 2019-10-29 15:27:24
categories: Algorithm
tags: 
- backtracking
---
- backtracking
    - 77 Combinations：https://leetcode.com/problems/combinations/
        - 在n个数字中选出k个数字的所有组合
        - Input: n = 4, k = 2
        - Output: [[2,4], [3,4], [2,3], [1,2], [1,3], [1,4]]
        <!-- more -->
    - 思路：回溯法
    - 时间复杂度：O(n^k)
    - 空间复杂度：O(k)
    ```java
    List<List<Integer>> res;
    public List<List<Integer>> combine(int n, int k) {
        res = new ArrayList<List<Integer>>();
        if(n<=0 || k<=0 || k>n) {
            return res;
        }
        generateCombinations(n, k, 1, new LinkedList<Integer>());
        return res;
    }
    private void generateCombinations(int n, int k, int start, LinkedList<Integer> c) {
        if(c.size() == k) {
            res.add((List<Integer>)c.clone());
            return;
        }
        for(int i=start; i<=n; i++) {
            c.addLast(i);
            generateCombinations(n, k, i+1, c);
            c.removeLast();
        }
        return;
    }
    ```
    - 优化：剪枝
    ```java
    List<List<Integer>> res;
    public List<List<Integer>> combine(int n, int k) {
        res = new ArrayList<List<Integer>>();
        if(n<=0 || k<=0 || k>n) {
            return res;
        }
        generateCombinations(n, k, 1, new LinkedList<Integer>());
        return res;
    }
    private void generateCombinations(int n, int k, int start, LinkedList<Integer> c) {
        if(c.size() == k) {
            res.add((List<Integer>)c.clone());
            return;
        }
        // 还有k-c.size()个空位，所以，[i...n]中至少有k-c.size()个元素
        // i最多为n-(k-c.size())+1
        for(int i=start; i<=n-(k-c.size())+1; i++) {
            c.addLast(i);
            generateCombinations(n, k, i+1, c);
            c.removeLast();
        }
        return;
    }
    ```