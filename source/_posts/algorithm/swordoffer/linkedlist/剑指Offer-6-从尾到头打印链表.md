---
title: '剑指Offer: 6. 从尾到头打印链表'
date: 2019-10-26 10:22:56
categories: 算法
tags: 
- 剑指Offer
- linkedlist
- recursion
- stack
---

- 6 从尾到头打印链表
    - https://www.nowcoder.com/practice/d0267f7f55b3412ba93bd35cfa8e8035
    - 反向打印链表
    <!-- more -->
    - 思路1：递归
    - 时间复杂度：O(n)
    - 空间复杂度：O(n)
    ```java
    ArrayList<Integer> res = new ArrayList<Integer>();
    public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
        if(listNode==null){
            return res;
        }
        printListFromTailToHead(listNode.next);
        res.add(listNode.val);
        return res;
    }
    ```
    - 思路2：栈，利用List的add(index, value)方法
    - 时间复杂度：O(n)
    - 空间复杂度：O(n)
    ```java
    ArrayList<Integer> res = new ArrayList<Integer>();
        ListNode cur = listNode;
        while(cur != null) {
            res.add(0, cur.val);
            cur = cur.next;
        }
        return res;
    ```