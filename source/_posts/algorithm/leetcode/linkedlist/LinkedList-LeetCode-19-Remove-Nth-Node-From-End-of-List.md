---
title: 'LinkedList: LeetCode 19. Remove Nth Node From End of List'
date: 2019-10-22 11:09:21
categories: 算法
tags: 
- linkedlist
- dummyHead
- pointers
---
- linkedlist
    - 19 Remove Nth Node From End of List：https://leetcode.com/problems/remove-nth-node-from-end-of-list/
        - 删除链表的倒数第N个元素
        - Input: 1->2->3->4->5, n = 2
        - Output: 1->2->3->5
        <!-- more -->
    - 思路：双指针，p和q之间的长度是固定的，只遍历一遍
    - 时间复杂度：O(n)
    - 空间复杂度：O(1)
    ```java
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummyHead = new ListNode(0);
        dummyHead.next = head;
        
        ListNode p = dummyHead;
        ListNode q = dummyHead;
        for(int i=0; i<n+1; i++){
            assert q != null;
            q = q.next;
        }
        
        while(q != null) {
            p = p.next;
            q = q.next;
        }
        p.next = p.next.next;
        
        return dummyHead.next;
    }
    ```
    - 基础解法：遍历两遍，第一遍求链表的size，第二遍使用dummyHead删除第size-n个元素
    - 时间复杂度：O(n)
    - 空间复杂度：O(1)
    ```java
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummyHead = new ListNode(0);
        dummyHead.next = head;
        
        ListNode cur = dummyHead.next;
        int index = 0;
        while(cur != null){
            index ++;
            cur = cur.next;
        }
        cur = dummyHead;
        for(int i=0; i<index-n; i++){
            cur = cur.next;
        }
        cur.next = cur.next.next;
        return dummyHead.next;
    }
    ```