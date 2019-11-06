---
title: 'LinkedList: LeetCode 206. Reverse Linked List'
date: 2019-10-18 23:31:13
categories: Algorithm
tags: 
- linkedlist
- recursion
- iteration
---

- linkedlist
    - 206 Reverse Linked List：https://leetcode.com/problems/reverse-linked-list/
        - 反转一个链表
        - Input: 1->2->3->4->5->NULL
        - Output: 5->4->3->2->1->NULL
        <!-- more -->
    - 思路1：递归
    - 时间复杂度：O(n)
    - 空间复杂度：O(n)
    ```java
    public ListNode reverseList(ListNode head) {
        // condition
        if(head==null || head.next==null) {
            return head;
        }

        // recursion
        ListNode node = reverseList(head.next);
        head.next.next = head;
        head.next = null;
        
        return node;
    }
    ```
    - 思路2：迭代
    - 时间复杂度：O(n)
    - 空间复杂度：O(1)
    ```java
    public ListNode reverseList(ListNode head) {
        ListNode pre = null;
        ListNode cur = head;
        while(cur != null) {
            ListNode next = cur.next;
            
            cur.next = pre;
            pre = cur;
            cur = next;
        }
        return pre;
    }
    ```