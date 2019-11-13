---
title: '算法：链表-LeetCode'
date: 2019-11-10 22:58:17
categories: 算法
tags: 
- Review
- 二刷
description: 反转链表、移除链表元素、两两交换链表中的节点、删除链表中的节点、删除链表的倒数第N个节点、回文链表
---

## linkedlist
- 链表
    - [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)：反转链表[【206题解】](#206题解)
    - [203. Remove Linked List Elements](https://leetcode.com/problems/remove-linked-list-elements/)：移除链表元素[【203题解】](#203题解)
    - [24. Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/)：两两交换链表中的节点[【24题解】](#24题解)
    - [237. Delete Node in a Linked List](https://leetcode.com/problems/delete-node-in-a-linked-list/)：删除链表中的节点[【237题解】](#237题解)
    - [19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)：删除链表的倒数第N个节点[【19题解】](#19题解)
    - [234. Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)：回文链表[【234题解】](#234题解)

### 206题解
- 递归解法
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        // 时间复杂度：O(n)
        // 空间复杂度：O(n)
        // 递归解法
        // 终止条件：head为空或head.next为空
        // 递归过程：先递归后一个，将head.next.next指向head，再将head.next设为空
        
        // condiction
        if(head==null || head.next==null) {
            return head;
        }
        // recursion
        ListNode node = reverseList(head.next);
        // 是head不是node
        head.next.next = head;
        head.next = null;
        return node;
    }
}
```
- 迭代解法
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        // 时间复杂度：O(n)
        // 空间复杂度：O(1)
        // 迭代解法：三指针（pre、cur、next）
         
        ListNode pre = null;
        ListNode cur = head;
        while(cur != null) { 
            // 当cur存在时，才声明next
            ListNode next = cur.next;
            cur.next = pre;
            pre = cur;
            cur = next;
        }
        // 返回的是pre
        return pre;
    }
}
```

### 203题解
- 涉及删除头结点：dummyHead解法
```java
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        // 时间复杂度：O(n)
        // 空间复杂度：O(1)
        // dummyHead解法：dummyHead.next指向head
        // 游标p指向dummyHead，cur指向p.next
        // 当cur不为空时，while循环
        // -- 如果值是val，删除
        // -- 否则，游标指向下一个
        // 返回dummyHead.next
        
        ListNode dummyHead = new ListNode(0);
        dummyHead.next = head;
        
        ListNode p = dummyHead;
        ListNode cur = p.next;
        
        while(cur != null) {
            if(cur.val == val) {
                p.next = p.next.next;
                // p指针不需要移动（e.g.[1,1]）
                cur = cur.next;
            }
            else {
                p = p.next;
                cur = cur.next;
            }
        }
        
        return dummyHead.next;
    }
}
```
- 递归解法：
```java
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        // 时间复杂度：O(n)
        // 空间复杂度：O(n)
        // 递归解法：先递归后一个，将head.next指向node，判断返回node或head
        
        // condiction
        if(head==null) {
            return head;
        }
        // recursion
        ListNode node = removeElements(head.next, val);
        head.next = node;
        return head.val==val ? node : head; 
        
    }
}
```

### 24题解
- 涉及头结点：dummyHead
```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        // 时间复杂度：O(n)
        // 空间复杂度：O(1)
        // 涉及头结点：dummyHead，四指针（p、node1、node2、next）
        
        ListNode dummyHead = new ListNode(0);
        dummyHead.next = head;
        ListNode p = dummyHead;
        while(p.next!=null && p.next.next!=null) {
            ListNode node1 = p.next;
            ListNode node2 = node1.next;
            ListNode next = node2.next;
            
            p.next = node2;
            node2.next = node1;
            node1.next = next;
            // 此时的下一个结点变为node1
            p = node1;
        }
        return dummyHead.next;
        
    }
}
```
### 237题解
- 直接用下一个结点的值覆盖，然后删除下一个结点
```java
class Solution {
    public void deleteNode(ListNode node) {
        // 时间复杂度：O(1)
        // 空间复杂度：O(1)
        // 与之前的不同，而是给定结点，删除掉这个结点
        // 因为没有前一个结点，所以直接用下一个结点的值覆盖
        // 然后删除下一个结点
        
        if(node==null) {
            return;
        }
        node.val = node.next.val;
        node.next = node.next.next;
    }
}
```
### 19题解
- 滑动窗口：要删除结点的前一个与最后一个结点(null)相距n+1
```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        // 时间复杂度：O(n)
        // 空间复杂度：O(1)
        // 链表不能根据index索引，又要one pass
        // 滑动窗口：要删除结点的前一个与最后一个结点(null)相距n+1
      
        ListNode dummyHead = new ListNode(0);
        dummyHead.next = head;
        
        ListNode cur = dummyHead;
        ListNode fast = dummyHead;
        // n+1：fast指向尾(null)，cur指向要删除结点的前一个
        for(int i=0; i<n+1; i++) {
            fast = fast.next;
        }
        while(fast!=null) {
            cur = cur.next;
            fast = fast.next;
        }
        cur.next = cur.next.next;
        return dummyHead.next;
    }
}
```
### 234题解
- 双指针+reverse
```java
class Solution {
    public boolean isPalindrome(ListNode head) {
        // 时间复杂度：O(n)
        // 空间复杂度：O(1)
        // 双指针+reverse
        
        ListNode p = head;
        ListNode q = head;
        while(q!=null && q.next!=null) {
            p = p.next;
            q = q.next.next;
        }
        // 奇数情况，将p后移设，前后等长
        if(q != null) {
            p = p.next;    
        }
        p = reverse(p);
        q = head;
        // p在后，q在前，所以是当p不为空时
        while(p != null) {
            if(p.val != q.val) {
                return false;
            }
            p = p.next;
            q = q.next;
        }
        return true;
    }
    private ListNode reverse(ListNode head) {
        if(head==null || head.next==null) {
            return head;
        }
        // recursion
        ListNode node = reverse(head.next);
        // 是head不是node
        head.next.next = head;
        head.next = null;
        return node;
    }
}
```
