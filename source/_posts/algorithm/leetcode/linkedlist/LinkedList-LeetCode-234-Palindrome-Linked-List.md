---
title: 'LinkedList: LeetCode 234. Palindrome Linked List'
date: 2019-10-23 12:13:56
categories: 算法
tags: 
- linkedlist
- recursion
- pointers
---
- linkedlist
    - 234 Palindrome Linked List：https://leetcode.com/problems/palindrome-linked-list/
        - 判断链表是否是回文的
        - Input: 1->2->2->1
        - Output: true
        <!-- more -->
    - 思路：双指针
    - 时间复杂度：O(n)
    - 空间复杂度：O(1)
    ```java
    public boolean isPalindrome(ListNode head) {
        ListNode p = head;
        ListNode q = head;
        while(q!=null && q.next!=null) {
            q = q.next.next;
            p = p.next;
        }
        if(q != null){
            p = p.next;
        }
        p = reverse(p);

        q = head;
        while(p != null) {
            if(p.val != q.val){
                return false;
            }
            p = p.next;
            q = q.next;
        }
        return true;
    }
    private ListNode reverse(ListNode node) {
        ListNode pre = null;
        ListNode cur = node;
        while(cur != null) {
            ListNode next = cur.next;
            cur.next = pre;
            pre = cur;
            cur = next;
        }
        return pre;
    }
    ```