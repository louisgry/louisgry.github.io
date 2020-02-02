---
title: 'LeetCode-数据结构'
date: 2019-10-22 16:48:17
categories: 算法
tags: 
- 数据结构
description: 栈、队列、链表、二叉树、集合与映射
---

- 数据结构
    - [stack](#stack)
    - [queue](#queue)
    - [linkedlist](#linkedlist)
    - [binarytree](#binarytree)
    - [collection](#collection)


## 数据结构
### stack
- 栈
    - [20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)：[【20题解】](#20题解) 
    - [144. Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/)：[【144题解】](#144题解)

### queue
- 队列
    - [347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)：[【347题解】](#347题解)


### linkedlist
- 链表
    - [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)：[【206题解】](#206题解)
    - [203. Remove Linked List Elements](https://leetcode.com/problems/remove-linked-list-elements/)：[【203题解】](#203题解)
    - [24. Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/)：[【24题解】](#24题解)
    - [237. Delete Node in a Linked List](https://leetcode.com/problems/delete-node-in-a-linked-list/)：[【237题解】](#237题解)
    - [19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)：[【19题解】](#19题解)
    - more
    - [234. Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)：[【234题解】](#234题解)

### binarytree
- 二叉树
    - [104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)：[【104题解】](#104题解)
    - [111. Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/)：[【11题解】](#111题解)
    - [226. Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)：[【226题解】](#226题解)
    - [112. Path Sum](https://leetcode.com/problems/path-sum/)：[【112题解】](#112题解)
    - [257. Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/)：[【257题解】](#257题解)
    - [437. Path Sum III](https://leetcode.com/problems/path-sum-iii/)：[【437题解】](#437题解)
    - [235. Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)：[【235题解】](#235题解)
    - more
    - [100. Same Tree](https://leetcode.com/problems/same-tree/)：[【100题解】](#100题解)
    - [101. Symmetric Tree](https://leetcode.com/problems/symmetric-tree/)：[【101题解】](#101题解)
    - [222. Count Complete Tree Nodes](https://leetcode.com/problems/count-complete-tree-nodes/)：[【222题解】](#222题解)
    - [110. Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/)：[【110题解】](#110题解)
    - [404. Sum of Left Leaves](https://leetcode.com/problems/sum-of-left-leaves/)：[【404题解】](#404题解)
    - [113. Path Sum II](https://leetcode.com/problems/path-sum-ii/)：[【113题解】](#113题解)
    - [129. Sum Root to Leaf Numbers](https://leetcode.com/problems/sum-root-to-leaf-numbers/)：[【129题解】](#129题解)

### collection
- 查找表
    - [349. Intersection of Two Arrays](https://leetcode.com/problems/intersection-of-two-arrays/)：[【349题解】](#349题解)
    - [350. Intersection of Two Arrays II](https://leetcode.com/problems/intersection-of-two-arrays-ii/)：[【350题解】](#350题解)
    - [1. Two Sum](https://leetcode.com/problems/two-sum/)：[【1题解-Map】](#1题解-Map)
    - [454. 4Sum II](https://leetcode.com/problems/4sum-ii/)：[【454题解】](#454题解)
    - [447. Number of Boomerangs](https://leetcode.com/problems/number-of-boomerangs/)：[【447题解】](#447题解)
    - [219. Contains Duplicate II](https://leetcode.com/problems/contains-duplicate-ii/)：[【219题解】](#219题解)
    - [220. Contain Duplicate III](https://leetcode.com/problems/contains-duplicate-iii/)：[【220题解】](#220题解)
    - more
    - [242. Valid Anagram](https://leetcode.com/problems/valid-anagram/)：[【242题解】](#242题解)
    - [202. Happy Number](https://leetcode.com/problems/happy-number/)：[【202题解】](#202题解)
    - [290. Word Pattern](https://leetcode.com/problems/word-pattern/)：[【290题解】](#290题解)
    - [804. Unique Morse Code Words](https://leetcode.com/problems/unique-morse-code-words/)：[【804题解】](#804题解)


## 题解

**stack**
### 20题解
- stack
    - 20 Valid Parentheses：https://leetcode.com/problems/valid-parentheses/
        - 判断括号是否匹配
        - Input: "()[]{}"
        - Output: true
    - 思路：栈（只关心最近一次操作）
    - 时间复杂度：O(n)
    - 空间复杂度：O(n)
    ```java
    public boolean isValid(String s) {
        // 遍历string
        // -- 如果是左括号，压入栈
        // -- 如果是右括号，弹出栈
        // ---- 如果不匹配，返回false
        // ---- 如果for结束，栈不为空，返回false
        // ---- 否则，返回true
        
        Stack<Character> stack = new Stack<Character>();
        char[] matchs = {'(', '{', '['};
        
        for(int i=0; i<s.length(); i++) {
            // s.charAt(i) (
            if(s.charAt(i)=='(' || s.charAt(i)=='{' || s.charAt(i)=='[') {
                stack.push(s.charAt(i));    
            }
            else {
                // 【Runtime Error："]"】没有进行size为0的判断
                if(stack.size()==0) {
                    return false;
                }
                char c = stack.pop();
                char match;
                if(s.charAt(i)==')') {
                    match = matchs[0];
                }
                else if(s.charAt(i)=='}') {
                    match = matchs[1];
                }
                else {
                    match = matchs[2];
                }
                
                if(c != match) {
                    return false;
                }
            }
        }
        
        if(stack.size()!=0) {
            return false;
        }
        return true;
    }
    ```

### 144题解
- stack
    - 144 Binary Tree Preorder Traversal：https://leetcode.com/problems/binary-tree-preorder-traversal/
        - 二叉树的前序遍历
        - Input: binary tree [1,2,3]
        - Output: [1,2,3]
    - 思路1：递归
    - 时间复杂度：O(n)
    - 空间复杂度：O(h)
    ```java
    public List<Integer> preorderTraversal(TreeNode root) {
        // 递归解法：根节点->左节点->右节点
        // 要返回List，所以需要辅助函数来做递归

        List<Integer> res = new ArrayList<Integer>();
        preorderTraversal(root, res);
        return res;
    }
    private void preorderTraversal(TreeNode root, List<Integer> res) {
        // condition
        if(root == null) {
            return;
        }
        // recursion
        res.add(root.val);
        preorderTraversal(root.left, res);
        preorderTraversal(root.right, res);
    }
    ```
    - 思路2：非递归（栈+Command）
    - 时间复杂度：O(n)
    - 空间复杂度：O(h)
    ```java
    public List<Integer> preorderTraversal(TreeNode root) {
        // 非递归解法：栈（递归的本质是栈）
        // 把第一个指令go-root压入栈，while循环直到stack为空
        // -- 如果指令是print，把command.node的值add进List
        // -- 否则指令就是go，把指令倒序压入stack：print-root, go-left, go-right
        
        List<Integer> res = new ArrayList<Integer>();
        // 【Runtime Error: []】没有进行非空判断
        if(root == null) {
            return res;
        }
        Stack<Command> stack = new Stack<Command>();
        stack.push(new Command("go", root));
        while(!stack.empty()) {
            // 取出栈顶元素
            Command command = stack.pop();
            if(command.s == "print") {
                res.add(command.node.val);
            }
            else {
                // 如果是go命令，倒序压入指令
                if(command.node.right != null) {
                    stack.push(new Command("go", command.node.right));
                }
                if(command.node.left != null) {
                    stack.push(new Command("go", command.node.left));
                }
                stack.push(new Command("print", command.node));
            }
        }
        return res;
    }
    public class Command {
        // 指令：print、go
        String s;
        // 指令要作用于某个节点上
        TreeNode node;
        Command(String s, TreeNode node) {
            this.s = s;
            this.node = node;
        }
    }
    ```

---
**queue**

### 347题解
- queue
    - 347 Top K Frequent Elements：https://leetcode.com/problems/top-k-frequent-elements/
        - 返回前k个出现频率最高的元素
        - Input: nums = [1,1,1,2,2,3], k = 2
        - Output: [1,2]
    - 思路：前K大（优先队列+PairComparator），频率（Map）
    - 时间复杂度：O(nlogk)
    - 空间复杂度：O(n+k)
    ```java
    import javafx.util.Pair;

    public List<Integer> topKFrequent(int[] nums, int k) {
        // 优先队列，频率为优先级(频率，元素)
        // 怎么计算频率：Map（元素，频率）
        // 怎么根据频率排序：PairComparator
        
        HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
        for(int i=0; i<nums.length; i++) {
            if(!map.containsKey(nums[i])) {
                map.put(nums[i], 1);
            }
            else {
                map.put(nums[i], map.get(nums[i])+1);
            }
        }
        
        PriorityQueue<Pair<Integer, Integer>> pq = new PriorityQueue<>(new PairComparator());
        for(Integer num : map.keySet()) {
            int freq = map.get(num);
            // 维护最高的k个：如果频率大于队首的频率，队首出队，该元素入队
            if(pq.size() == k) {
                if(freq > pq.peek().getKey()) {
                    pq.poll();
                    pq.add(new Pair<Integer, Integer>(freq, num));
                }
            }
            else {
                pq.add(new Pair<Integer, Integer>(freq, num));
            }
        }
        
        List<Integer> res = new ArrayList<Integer>();
        while(!pq.isEmpty()) {
            res.add(pq.poll().getValue());
        }
        return res;
    }
    private class PairComparator implements Comparator<Pair<Integer, Integer>> {
        @Override
        public int compare(Pair<Integer, Integer> p1, Pair<Integer, Integer> p2) {
            if(p1.getKey() != p2.getKey()){
                return p1.getKey() - p2.getKey();
            }
            return p1.getValue() - p2.getValue();
        }
    }
    ```

---
**linkedlist**
### 206题解
- linkedlist
    - 206 Reverse Linked List：https://leetcode.com/problems/reverse-linked-list/
        - 反转一个链表
        - Input: 1->2->3->4->5->NULL
        - Output: 5->4->3->2->1->NULL
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

### 203题解
- linkedlist
    - 203 Remove Linked List Elements：https://leetcode.com/problems/remove-linked-list-elements/
        - 删除链表中特定值的所有元素
        - Input: 1->2->6->3->4->5->6, val = 6
        - Output: 1->2->3->4->5
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
                cur.next = cur.next.next;
            }
            else{
                cur = cur.next;
            }
        }
        return dummyHead.next;
    }
    ```

### 24题解
- linkedlist
    - 24 Swap Nodes in Pairs：https://leetcode.com/problems/swap-nodes-in-pairs/
        - 链表两两交换节点
        - Input: 1->2->3->4->null
        - Output: 2->1->4->3->null
    - 思路：dummy head
    - 时间复杂度：O(n)
    - 空间复杂度：O(1)
    ```java
    public ListNode swapPairs(ListNode head) {
        // 涉及头结点：dummyHead，四指针（p、node1、node2、next）
        // 定义pre指向dummyHead
        // while循环：当pre.next(node1)和pre.next.next(node2)不为空时
        // -- 定义node1、node2
        // -- 更改指向
        // -- 将pre指向下一个目标之前(node1)
        ListNode dummyHead = new ListNode(0);
        dummyHead.next = head;
        
        ListNode pre = dummyHead;
        
        while(pre.next != null && pre.next.next != null) {
            ListNode node1 = pre.next;
            ListNode node2 = node1.next;
            ListNode next = node2.next;
            
            pre.next = node2;
            node2.next = node1;
            node1.next = next;
            
            pre = node1;
        }
        
        return dummyHead.next;
    }
    ```
  
### 237题解
- linkedlist
    - 237 Delete Node in a Linked List：https://leetcode.com/problems/delete-node-in-a-linked-list/
        - 给定链表中的一个节点，删除该节点
        - Input: head = [4,5,1,9], node = 5
        - Output: [4,1,9]
        - Explanation: You are given the second node with value 5, the linked list should become 4 -> 1 -> 9 after calling your function.
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
  
### 19题解
- linkedlist
    - 19 Remove Nth Node From End of List：https://leetcode.com/problems/remove-nth-node-from-end-of-list/
        - 删除链表的倒数第N个元素
        - Input: 1->2->3->4->5, n = 2
        - Output: 1->2->3->5
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
  
### 234题解
- linkedlist
    - 234 Palindrome Linked List：https://leetcode.com/problems/palindrome-linked-list/
        - 判断链表是否是回文的
        - Input: 1->2->2->1
        - Output: true
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

---
**binarytree**
### 104题解
- 104题解
    - 104 Maximum Depth of Binary Tree：https://leetcode.com/problems/maximum-depth-of-binary-tree/
        - 返回二叉树的最大深度
        - Input: Given binary tree [3,9,20,null,null,15,7]
        - Output: 3
    - 知识：深度K=「log2n」+1（向下取整）
    - 思路：递归
    - 时间复杂度：O(n)，n是节点数
    - 空间复杂度：O(h)，h是树深度
    ```java
    public int maxDepth(TreeNode root) {
        // 递归终止条件
        if (root==null) {
            return 0;
        }
        // 递归过程
        return Math.max(maxDepth(root.left), maxDepth(root.right))+1;
    }
    ```
  
### 111题解
- 111题解
    - 111 Minimum Depth of Binary Tree：https://leetcode.com/problems/minimum-depth-of-binary-tree/
        - 求二叉树的最低深度
        - Input: Given binary tree [3,9,20,null,null,15,7]
        - Output: 2
    - 思路：递归
    - 时间复杂度：O(n)
    - 空间复杂度：O(h)
    ```java
    public int minDepth(TreeNode root) {
        // 递归终止条件
        if (root==null) {
            return 0;
        }
        if (root.left==null && root.right==null) {
            return 1;
        }
        // 递归过程
        int min = Integer.MAX_VALUE;
        if (root.left!=null) {
            min = Math.min(min, minDepth(root.left)+1);
        }
        if (root.right!=null) {
            min = Math.min(min, minDepth(root.right)+1);
        }
        return min;
    }
    ```
  
### 226题解
- 226题解
    - 226 Invert Binary Tree：https://leetcode.com/problems/invert-binary-tree/
        - 反转二叉树，左右子树对调
        - Input: Given binary tree [4,2,7,1,3,6,9]
        - Output: return binary tree [4,7,2,9,6,3,1]
    - 思路：递归
    - 时间复杂度：O(n)
    - 空间复杂度：O(h)
    ```java
    public TreeNode invertTree(TreeNode root) {
        // 递归终止条件
        if (root==null) {
            return null;
        }
        // 递归过程
        TreeNode left = invertTree(root.left);
        TreeNode right = invertTree(root.right);
        // swap
        root.left = right;
        root.right = left;
        return root;
    }
    ```

### 112题解
- 112题解
    - 112 Path Sum：https://leetcode.com/problems/path-sum/
        - 找出二叉树路径中的是否有一条和等于sum
        - Input: binary tree [5,4,8,11,null,13,4,7,2]
        - Output: true
    - 思路：递归
    - 时间复杂度：O(n)
    - 空间复杂度：O(h)
    ```java
    public boolean hasPathSum(TreeNode root, int sum) {
        // 递归终止条件
        if (root == null) {
            return false;
        }
        if (root.left==null && root.right==null) {
            return root.val == sum;
        }
        // 递归过程
        return hasPathSum(root.left, sum-root.val) ||
                hasPathSum(root.right, sum-root.val);
    }
    ```

### 257题解
- 257题解
    - 257 Binary Tree Paths：https://leetcode.com/problems/binary-tree-paths/
        - 返回二叉树所有路径的path
        - Input: binary tree [1,2,3,null,5]
        - Output: ["1->2->5", "1->3"]
    - 思路：递归
    - 时间复杂度：O(n)
    - 空间复杂度：O(h)
    ```java
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> result = new ArrayList<String>();
        // 递归终止条件
        if (root == null ) {
            return result;
        }
        if (root.left==null && root.right==null) {
            result.add(Integer.toString(root.val));
            return result;
        }
        // 递归过程
        List<String> leftPath = binaryTreePaths(root.left);
        for (String i : leftPath) {
            StringBuilder sb = new StringBuilder(Integer.toString(root.val));
            sb.append("->");
            sb.append(i);
            result.add(sb.toString());
        }
        List<String> rightPath = binaryTreePaths(root.right);
        for (String j : rightPath) {
            StringBuilder sb = new StringBuilder(Integer.toString(root.val));
            sb.append("->");
            sb.append(j);
            result.add(sb.toString());
        }
        return result;
    }
    ```

### 437题解
- 437题解
    - 437 Path Sum III：https://leetcode.com/problems/path-sum-iii/
        - 求二叉树中等于给定sum的路径，路径可以不从根节点开始
        - Input: binary tree [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8
        - Output: 3
        - Explanation: three paths [[5,3], [5,2,1], [-3,11]]
    - 思路：递归嵌套递归
    - 时间复杂度：O(n)
    - 空间复杂度：O(h)
    ```java
    public int pathSum(TreeNode root, int sum) {
        // condition
        if(root == null) {
            return 0;
        }
        // recursion
        return findPath(root, sum)
                + pathSum(root.left, sum) + pathSum(root.right, sum);
    }
    private int findPath(TreeNode node, int sum) {
        // condition
        if(node == null) {
            return 0;
        }
        int res = 0;
        if(node.val == sum){
            res += 1;
        }
        // recursion
        res += findPath(node.left, sum-node.val);
        res += findPath(node.right, sum-node.val);

        return res;
    }
    ```

### 235题解
- 235题解
    - 235 Lowest Common Ancestor of a Binary Search Tree：https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/
        - 寻找两个节点最近的公共祖先
        - Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
        - Output: 2
    - 思路：递归
    - 时间复杂度：O(n)
    - 空间复杂度：O(h)
    ```java
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // condition
        if(root == null) {
            return null;
        }
        // recursion
        if(p.val<root.val && q.val<root.val) {
            return lowestCommonAncestor(root.left, p, q);
        }
        if(p.val>root.val && q.val>root.val) {
            return lowestCommonAncestor(root.right, p, q);
        }
        return root;
    }
    ```
  
### 100题解
- 100题解
    - 100 Same Tree：https://leetcode.com/problems/same-tree/
        - 判断两颗二叉树是否相同
        - Input: binary tree [1,2,null], [1,null,2]
        - Output: false
    - 思路：递归
    - 时间复杂度：O(n)
    - 空间复杂度：O(h)
    ```java
    public boolean isSameTree(TreeNode p, TreeNode q) {
        if (p==null && q==null) {
            return true;
        }
        if (p==null || q==null) {
            return false;
        }
        if (p.val != q.val) {
            return false;
        }
        return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
    }
    ```

### 101题解
- 101题解
    - 101 Symmetric Tree：https://leetcode.com/problems/symmetric-tree/
        - 判断二叉树是否对称
        - Input: binary tree [1,2,2,null,3,null,3]
        - Output: false
    - 思路：递归
    - 时间复杂度：O(n)
    - 空间复杂度：O(h)
    ```java
    public boolean isSymmetric(TreeNode root) {
        // 递归终止条件
        if (root == null) {
            return true;
        }
        // 递归过程
        return isMirror(root.left, root.right);
    }
    private boolean isMirror(TreeNode p, TreeNode q) {
        if (p==null && q==null) {
            return true;
        }
        if (p==null || q==null) {
            return false;
        }
        if (p.val != q.val) {
            return false;
        }
        return isMirror(p.left, q.right) && isMirror(p.right, q.left);
    }
    ```

### 222题解
- 222题解
    - 222 Count Complete Tree Nodes：https://leetcode.com/problems/count-complete-tree-nodes/
        - 计算完全二叉树节点个数
        - Input: binary tree [1,2,3,4,5,6,null]
        - Output: 6
    - 思路：递归
    - 时间复杂度：O(n)
    - 空间复杂度：O(h)
    ```java
    public int countNodes(TreeNode root) {
        // 递归终止条件
        if (root==null) {
            return 0;
        }

        // 递归过程
        return 1+countNodes(root.left)+countNodes(root.right);
    }
    ```

### 110题解
- 110题解
    - 110 Balanced Binary Tree：https://leetcode.com/problems/balanced-binary-tree/
        - 判断是否为平衡二叉树（每个节点的左右子树高度差不超过1）
        - Input: binary tree [1,2,2,3,3,null,null,4,4]
        - Output: false
    - 思路：递归
    - 时间复杂度：O(n)
    - 空间复杂度：O(h)
    ```java
    public boolean isBalanced(TreeNode root) {
        // 递归终止条件
        if (root == null) {
            return true;
        }
        if (Math.abs(getDepth(root.left)-getDepth(root.right))>1) {
            return false;
        }

        // 递归过程
        return isBalanced(root.left) && isBalanced(root.right);
    }
    private int getDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        return 1 + Math.max(getDepth(root.left), getDepth(root.right));
    }
    ```

### 404题解
- 404题解
    - 404 Sum of Left Leaves：https://leetcode.com/problems/sum-of-left-leaves/
        - 求左叶子节点的和
        - Input: binary tree [3,9,20,null,null,15,7]
        - Output: 24
    - 思路：递归
    - 时间复杂度：O(n)
    - 空间复杂度：O(h)
    ```java
    public int sumOfLeftLeaves(TreeNode root) {
        int sum = 0;
        // 递归终止条件
        if (root == null) {
            return 0;
        }
        if (root.left!=null && root.left.left==null && root.left.right==null) {
            sum += root.left.val;
        }
        // 递归过程
        sum += sumOfLeftLeaves(root.left) + sumOfLeftLeaves(root.right);
        return sum;
    }
    ```

### 113题解
- 113题解
    - 113 Path Sum II：https://leetcode.com/problems/path-sum-ii/
        - 返回二叉树路径中的所有等于sum的路径
        - Input: binary tree [5,4,8,11,null,13,4,7,2,null,null,5,1]
        - Output: [[5,4,11,2],[5,8,4,5]]
    - 思路：递归
    - 时间复杂度：O(n)
    - 空间复杂度：O(h)
    ```java
    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        List<List<Integer>> result = new ArrayList<>();
        ArrayList<Integer> middle = new ArrayList<>();
        getPathSum(root, sum, middle, result);
        return result;
    }
    private void getPathSum(TreeNode root, int sum, ArrayList<Integer> middle, List<List<Integer>> result) {
        // condition
        if(root == null) {
            return;
        }
        middle.add(root.val);
        if(root.left==null && root.right==null) {
            if(root.val == sum) {
                result.add(new ArrayList<>(middle));
            }
        }
        // recursion
        getPathSum(root.left, sum-root.val, middle, result);
        getPathSum(root.right, sum-root.val, middle, result);
        middle.remove(middle.size()-1);
    }
    ```
  
### 129题解
- 129题解
    - 129 Sum Root to Leaf Numbers：https://leetcode.com/problems/sum-root-to-leaf-numbers/
        - 求所有路径组成的数字的和
        - Input: binary tree [1,2,3]
        - Output: 25
        - Explanation:
            - The root-to-leaf path 1->2 represents the number 12.
            - The root-to-leaf path 1->3 represents the number 13.
            - Therefore, sum = 12 + 13 = 25.
    - 思路：递归
    - 时间复杂度：O(n)
    - 空间复杂度：O(h)
    ```java
    public int sumNumbers(TreeNode root) {
        return getSum(root, 0);
    }
    private int getSum(TreeNode root, int curSum) {
        // condition
        if(root == null) {
            return 0;
        }
        curSum = curSum*10 + root.val;
        if(root.left==null && root.right==null) {
            return curSum;
        }
        // recursion
        return getSum(root.left, curSum) + getSum(root.right, curSum);
    }
    ```


---
**collection**
### 349题解
- 349题解
    - 349 Intersection of Two Arrays：https://leetcode.com/problems/intersection-of-two-arrays/
        - 找两个数组的交集
        - Input: nums1 = [1,2,2,1], nums2 = [2,2]
        - Output: [2]
    - 思路：HashSet，一个set、一个resultSet
    - 时间复杂度：O(n+m)
    - 空间复杂度：O(n)
    ```java
    public int[] intersection(int[] nums1, int[] nums2) {
            HashSet<Integer> set = new HashSet<>();
            for(int i=0; i<nums1.length; i++) {
                set.add(nums1[i]);
            }
            HashSet<Integer> resultSet = new HashSet<>();
            for(int num : nums2) {
                if(set.contains(num)) {
                    resultSet.add(num);
                }
            }
            int[] res = new int[resultSet.size()];
            int index = 0;
            for(Integer num : resultSet) {
                res[index++] = num;
            }
            return res;
        }
    ```

### 350题解
- 350题解
    - 350 Intersection of Two Arrays II：https://leetcode.com/problems/intersection-of-two-arrays-ii/
        - 找两个数组的交集（包括重复的）
        - Input: nums1 = [1,2,2,1], nums2 = [2,2]
        - Output: [2,2]
    - 思路：HashMap，计算元素个数
    - 时间复杂度：O(n+mlogn)
    - 空间复杂度：O(n)
    ```java
    public int[] intersect(int[] nums1, int[] nums2) {
        HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
        for(int num : nums1) {
            if(!map.containsKey(num)) {
                map.put(num, 1);
            }
            else {
                map.put(num, map.get(num)+1);
            }
        }

        List<Integer> result = new ArrayList<Integer>();
        for(int num : nums2) {
            if(map.containsKey(num) && map.get(num)>0) {
                result.add(num);
                map.put(num, map.get(num)-1);
            }
        }
        int[] res = new int[result.size()];
        int index = 0;
        for(Integer num : result) {
            res[index++] = num;
        }
        return res;
    }
    ```

### 1题解-Map
- 1题解
    - 1 Two Sum：https://leetcode.com/problems/two-sum/
        - 找出数组中和等于target的数字的下标（注意nums不是有序的）
        - Input: nums = [2, 7, 11, 15], target = 9
        - Output: [0,1]
    - 思路：将元素a放入Map中，之后查找target-a是否存在
    - 时间复杂度：O(n)
    - 空间复杂度：O(n)
    ```java
    public int[] twoSum(int[] nums, int target) {
        HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
        int[] res = new int[2];
        for(int i=0; i<nums.length; i++) {
            int complement = target - nums[i];
            if(map.containsKey(complement)) {
                res[0] = i;
                res[1] = map.get(complement);
            }
            map.put(nums[i], i);
        }
        return res;
    }
    ```

### 454题解
- 454题解
    - 454 4Sum II：https://leetcode.com/problems/4sum-ii/
        - 求四个整型数组的有多少种组合相加等于0
        - Input: A = [ 1, 2], B = [-2,-1], C = [-1, 2], D = [ 0, 2]
        - Output: 2
        - Explanation: The two tuples are:
            1. (0, 0, 0, 1) -> A[0] + B[0] + C[0] + D[1] = 1 + (-2) + (-1) + 2 = 0
            2. (1, 1, 0, 0) -> A[1] + B[1] + C[0] + D[0] = 2 + (-1) + (-1) + 0 = 0
    - 思路：将C+D的所有组合放入Map中，查找0-A-B
    - 时间复杂度：O(n^2)
    - 空间复杂度：O(n^2)
    ```java
    public int fourSumCount(int[] A, int[] B, int[] C, int[] D) {
        HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
        for(int i=0; i<C.length; i++) {
            for (int j = 0; j < D.length; j++) {
                int sum = C[i] + D[j];
                if(!map.containsKey(sum)) {
                    map.put(sum, 1);
                }
                else {
                    map.put(sum, map.get(sum)+1);
                }
            }
        }

        int res = 0;
        for(int i=0; i<A.length; i++) {
            for (int j=0; j<B.length; j++) {
                if(map.containsKey(0-A[i]-B[j])) {
                    res += map.get(0-A[i]-B[j]);
                }
            }
        }
        return res;
    }
    ```

### 447题解
- 447题解
    - 447 Number of Boomerangs：https://leetcode.com/problems/number-of-boomerangs/
        - 寻找符合Boomerangs定义的三元组的个数
        - Input: [[0,0],[1,0],[2,0]]
        - Output: 2
        - Explanation: The two boomerangs are [[1,0],[0,0],[2,0]] and [[1,0],[2,0],[0,0]]
    - 思路：以i为枢纽点，把其他点跟i的距离放入map<距离，频数>，查找距离相同的点组合个数(频数*频数-1)
    - 时间复杂度：O(n^2)
    - 空间复杂度：O(n)
    ```java
    public int numberOfBoomerangs(int[][] points) {
        int res = 0;
        // 遍历枢纽点i
        for(int i=0; i<points.length; i++) {
            // 把其他点的距离放入Map
            HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
            for(int j=0; j<points.length; j++) {
                if(j != i) {
                    // 对比距离时使用距离的平方，无浮点数误差问题
                    int dist = dis(points[i], points[j]);
                    if(!map.containsKey(dist))
                        map.put(dist, 1);
                    else
                        map.put(dist, map.get(dist)+1);
                }
            }
            // 计算相同距离点的个数的组合
            for(Integer dis : map.keySet())
                res += map.get(dis) * (map.get(dis)-1);
        }
        return res;
    }
    private int dis(int[] a, int[] b) {
        return (a[0]-b[0])*(a[0]-b[0]) + (a[1]-b[1])*(a[1]-b[1]);
    }
    ```

### 219题解
- 219题解
    - 219 Contains Duplicate II：https://leetcode.com/problems/contains-duplicate-ii/
        - 判断数组是否在k长度内有重复元素
        - Input: nums = [1,2,3,1], k = 3
        - Output: true
    - 思路：Set+滑动窗口
    - 时间复杂度：O(n)
    - 空间复杂度：O(k)
    ```java
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        HashSet<Integer> set = new HashSet<Integer>();
        for(int i=0; i<nums.length; i++) {
            if(set.contains(nums[i])){
                return true;
            }
            set.add(nums[i]);

            // 保持set中最多有k个元素（边界：i-k）
            if(set.size() == k+1){
                set.remove(nums[i-k]);
            }
        }
        return false;
    }
    ```
  
### 220题解
- 220题解
    - 220 Contain Duplicate III：https://leetcode.com/problems/contains-duplicate-iii/
        - 判断数组是否在k长度内有差值不大于t的两个数
        - Input: nums = [1,2,3,1], k = 3, t = 0
        - Output: true
    - 思路：Set+滑动窗口：TreeSet(有序)，查找比x-t大的最小的元素(ceiling)是否<=x+t
    - 时间复杂度：O(nlogk)
    - 空间复杂度：O(k)
    ```java
    public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
        // 需要用long，否则会发生整型溢出
        TreeSet<Long> set = new TreeSet<Long>();
        for(int i=0; i<nums.length; i++) {
            if(set.ceiling((long)nums[i]-(long)t) != null && set.ceiling((long)nums[i]-(long)t)<=(long)nums[i]+(long)t){
                return true;
            }
            set.add((long)nums[i]);
            if(set.size() == k+1){
                set.remove((long)nums[i-k]);
            }
        }
        return false;
    }
    ```

### 242题解
- 242题解
    - 242 Valid Anagram：https://leetcode.com/problems/valid-anagram/
        - 判断两个字符串是否为回文串
        - Input: s = "anagram", t = "nagaram"
        - Output: true
    - 思路：Hash Table
    - 时间复杂度：O(n)
    - 空间复杂度：O(26)
    ```
    public boolean isAnagram(String s, String t) {
        if(s.length() != t.length()){
            return false;
        }
        // HashTable
        int[] freq = new int[26];
        for(int i=0; i<s.length(); i++) {
            freq[s.charAt(i)-'a']++;
        }

        for(int i=0; i<t.length(); i++){
            freq[t.charAt(i)-'a']--;
            if(freq[t.charAt(i)-'a'] < 0) {
                return false;
            }
        }
        return true;
    }
    ```

### 202题解
- 202题解
    - 202 Happy Number：https://leetcode.com/problems/happy-number/
        - 判断一个数字是否为happy number
        - Input: 19
        - Output: true
        - Explanation: 
            - 1^2 + 9^2 = 82
            - 8^2 + 2^2 = 68
            - 6^2 + 8^2 = 100
            - 1^2 + 0^2 + 0^2 = 1
    - 思路：Set
    - 时间复杂度：O(?)
    - 空间复杂度：O(?)
    ```
   public boolean isHappy(int n) {
        if(n<1) {
            return false;
        }
        HashSet<Integer> set = new HashSet<Integer>();
        int t;
        int newN;
        while(n!=1 && !set.contains(n)) {
            set.add(n);
            newN = 0;
            while(n>0) {
                t = n%10;
                n /= 10;
                newN += t*t;
            }
            n = newN;
        }
        return n == 1;
    }
    ```

### 290题解
- 290题解
    - 290 Word Pattern：https://leetcode.com/problems/word-pattern/
        - 判断所给的string是否是pattern的形式
        - Input: pattern = "abba", str = "dog cat cat dog"
        - Output: true
    - 思路：Map
    - 时间复杂度：O(nlogm)
    - 空间复杂度：O(n+m)
    ```
    public boolean wordPattern(String pattern, String str) {
        HashMap<Character, String> map = new HashMap<Character, String>();
        char[] patterns = pattern.toCharArray();
        String[] strs = str.split(" ");

        if(patterns.length != strs.length) {
            return false;
        }

        for(int i=0; i<patterns.length; i++) {
            char c = patterns[i];
            if(!map.containsKey(c)) {
                if(map.containsValue(strs[i])) {
                    return false;
                }
                map.put(c, strs[i]);
            }
            else {
                String value = map.get(c);
                if(!value.equals(strs[i])) {
                    return false;
                }
            }
        }
        return true;
    }
    ```

### 804题解
- 804题解
    - 804 Unique Morse Code Words：https://leetcode.com/problems/unique-morse-code-words/
        - 返回不同摩斯码的个数
        - Input: words = ["gin", "zen", "gig", "msg"]
        - Output: 2
        - Explanation: There are 2 different transformations, "--...-." and "--...--.".
    - 思路：使用Set返回不同摩斯码的个数
    - 时间复杂度：O(n+s)
    - 空间复杂度：O(n)
    ```
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
