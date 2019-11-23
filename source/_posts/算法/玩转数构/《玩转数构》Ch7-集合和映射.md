---
title: 《玩转数构》Ch7-集合和映射
date: 2019-10-15 21:50:36
categories: 数据结构
tags: 
- 数构
- Set&Map
description: Set和Map（基于链表、基于二叉树）
---

# Ch7-集合和映射
<!-- more -->
https://github.com/liuyubobobo/Play-with-Data-Structures/tree/master/07-Set-and-Map
- 7-1：集合基础和基于二分搜索树的集合实现
    - 集合：Set中每个元素只能存在一次
    - 添加：add
        - 直接使用二分搜索树BST的add方法
    - 查找：contains
    - 删除：remove
        - 直接使用二分搜索树BST的remove方法
    - 测试：英文书傲慢与偏见和双城记的词汇量
- 7-2：基于链表的集合实现
    - BST二分搜索树和LinkedList链表：都属于动态数据结构
    - 添加：add
    - 查找：contains
    - 删除：remove
- 7-3：集合类的复杂度分析
    - 基于BST：增O(h)、查O(h)、删O(h)
    - 基于LinkedListd：增O(n)、查O(n)、删O(n)
    - 但 `h = log(n)`
    - 但数据近乎有序时退化为O(n)，使用AVL树解决
    
- 7-4：Leetcode中的集合问题和更多集合相关问题
    - TreeSet：
        - 底层使用红黑树，复杂度都是O(logn)即使有序也不会退化
        - 还有最大最小floor等操作
    - 有序集合和无序集合
        - BST是有序的
        - LinkedList是无序的
    - 多重集合：集合的元素可以重复
    - 804 Unique Morse Code Words：https://leetcode.com/problems/unique-morse-code-words/
        - 返回不同摩斯码的个数
        - Input: words = ["gin", "zen", "gig", "msg"]
        - Output: 2
        - Explanation: There are 2 different transformations, "--...-." and "--...--.".
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
- 7-5：映射基础
    - 映射Map（字典key-value）、python的`dict`
    - 常用方法
- 7-6：基于链表的映射实现
    - 添加：put
    - 修改：set
    - 查找：contains
    - 删除：remove
- 7-7：基于二分搜索树的映射实现
    - 添加：put
    - 修改：set
    - 查找：contains
    - 删除：remove
- 7-8：映射的复杂度分析和更多映射相关问题
    - 基于BST：O(logn)
    - 基于LinkedList：O(n)
    - 无序性使用哈希表来实现
- 7-9：Leetcode上更多集合和映射的问题
    - 基于哈希表实现的HashSet和HashMap