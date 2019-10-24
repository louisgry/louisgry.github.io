---
title: 'LinkedList: LeetCode 203. Remove Linked List Elements'
date: 2019-10-19 22:11:26
categories: 算法
tags: 
- linkedlist
- dummyHead
---

- linkedlist
    - 203 Remove Linked List Elements：https://leetcode.com/problems/remove-linked-list-elements/
        - 删除链表中特定值的所有元素
        - Input: 1->2->6->3->4->5->6, val = 6
        - Output: 1->2->3->4->5
        <!-- more -->
    - 思路：dummy head
    - 时间复杂度：O(n)
    - 空间复杂度：O(1)
    ```java
    public ListNode removeElements(ListNode head, int val) {
        ListNode dummyHead = new ListNode(0);
        dummyHead.next = head;
        
        ListNode cur = dummyHead;
        while(cur.next != null) {
            if(cur.next.val == val){
                ListNode delNode = cur.next;
                cur.next = delNode.next;
            }
            else{
                cur = cur.next;
            }
        }
        return dummyHead.next;
    }
    ```