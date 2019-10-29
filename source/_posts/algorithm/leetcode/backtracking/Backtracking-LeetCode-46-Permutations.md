---
title: 'Backtracking: LeetCode 46. Permutations'
date: 2019-10-28 20:05:31
categories: 算法
tags: 
- backtracking
---
- backtracking
    - 46 Permutations：https://leetcode.com/problems/permutations/
        - 求数组的全排列
        - Input: [1,2,3]
        - Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
        <!-- more -->
    - 思路：回溯法，Perms(nums)={取出一个数字}+Perms(nums-这个数字)
    - 时间复杂度：O(n^n)
    - 空间复杂度：O(n)
    ```java
    private List<List<Integer>> res;
    private boolean[] used;
    public List<List<Integer>> permute(int[] nums) {
        res = new ArrayList<List<Integer>>();
        if(nums.length == 0) {
            return res;
        }
        used = new boolean[nums.length];
        LinkedList<Integer> p = new LinkedList<Integer>();
        generatePerm(nums, 0, p);
        return res;
    }
    private void generatePerm(int[] nums, int index, LinkedList<Integer> p) {
        if(index == nums.length) {
            res.add((List<Integer>) p.clone());
            return;
        }
        for(int i=0; i<nums.length; i++) {
            // used[i]没有被使用
            if(!used[i]) {
                p.addLast(nums[i]);
                used[i] = true;
                generatePerm(nums, index+1, p);
                p.removeLast();
                used[i] = false;
            }
        }
        return;
    }
    ```