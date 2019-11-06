---
title: 'LinkedList: LeetCode 237. Delete Node in a Linked List'
date: 2019-10-21 16:16:22
categories: Algorithm
tags: 
- linkedlist
- changeValue
---
- linkedlist
    - 237 Delete Node in a Linked List：https://leetcode.com/problems/delete-node-in-a-linked-list/
        - 给定链表中的一个节点，删除该节点
        - Input: head = [4,5,1,9], node = 5
        - Output: [4,1,9]
        - Explanation: You are given the second node with value 5, the linked list should become 4 -> 1 -> 9 after calling your function.
        <!-- more -->
    - 思路：修改链表的值
    - 时间复杂度：O(1)
    - 空间复杂度：O(1)
    ```java
    public void deleteNode(ListNode node) {
        if(node == null) {
            return;
        }
        if(node.next == null){
            node = null;
            return;
        }
        node.val = node.next.val;
        node.next = node.next.next;
        return;
    }
    ```