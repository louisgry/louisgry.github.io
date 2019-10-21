---
title: 'LinkedList: LeetCode 24. Swap Nodes in Pairs'
date: 2019-10-20 16:15:17
categories: 算法
tags: 
- linkedlist
- dummyHead
---
- linkedlist
    - 24 Swap Nodes in Pairs：https://leetcode.com/problems/swap-nodes-in-pairs/
        - 链表两两交换节点
        - Input: 1->2->3->4->null
        - Output: 2->1->4->3->null
        <!-- more -->
    - 思路：dummy head
    - 时间复杂度：O(n)
    - 空间复杂度：O(1)
    ```java
    public ListNode swapPairs(ListNode head) {
        ListNode dummyHead = new ListNode(0);
        dummyHead.next = head;        
        
        ListNode p = dummyHead;
        // 注意：边界是p.next和p.next.next
        while(p.next!=null && p.next.next!=null) {
            ListNode node1 = p.next;
            ListNode node2 = node1.next;
            ListNode next = node2.next;
            
            node2.next = node1;
            node1.next = next;
            p.next = node2;
            p = node1;
        }
        return dummyHead.next;
    }
    ```