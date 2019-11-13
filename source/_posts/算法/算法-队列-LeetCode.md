---
title: '算法：队列-LeetCode'
date: 2019-11-09 12:48:17
categories: 算法
tags: 
- Review
- 二刷
description: 二叉树的层次遍历、完全平方数、前K个高频元素
---

## queue
- 队列
    - [102. Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)：二叉树的层次遍历[【102题解】](#102题解)
    - [279. Perfect Squares](https://leetcode.com/problems/perfect-squares/)：完全平方数[【279题解-Queue】](#279题解-Queue)
    - [347. Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)：前K个高频元素[【347题解】](#347题解)

### 102题解
- 基于队列的BFS层次遍历
```java
import javafx.util.Pair;

class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        // 时间复杂度：O(n)
        // 空间复杂度：O(n)
        // 层次遍历：BFS（基于队列）
        // 先进行非空判断
        // 队列（节点值，层）：LinkedList<Pair<TreeNode, Integer>>
        // 初始化是(root, 0)
        // 当queue不为空时，while循环
        // -- 如果level==res.size()：是新一层，res新增list
        // -- 把值add进level层的res
        // -- 如果有左子树，入队left，level+1
        // -- 如果有右子树，入队right，level+1
        
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        if(root == null) {
            return res;
        }
        
        LinkedList<Pair<TreeNode, Integer>> queue = new LinkedList<Pair<TreeNode, Integer>>();
        queue.addLast(new Pair<TreeNode, Integer>(root, 0));
        while(!queue.isEmpty()) {
            Pair<TreeNode, Integer> front = queue.removeFirst();
            TreeNode node = front.getKey();
            int level = front.getValue();
            // 新的一层
            if(level == res.size()) {
                res.add(new ArrayList<Integer>());
            }
            // get level层的res
            res.get(level).add(node.val);
            if(node.left != null) {
                queue.addLast(new Pair<TreeNode, Integer>(node.left, level+1));
            }
            if(node.right != null) {
                queue.addLast(new Pair<TreeNode, Integer>(node.right, level+1));
            }
        }
        return res;
    }
}
```

### 279题解-Queue
- 转化为BFS求无权图的最短路径
```java
import javafx.util.Pair;

class Solution {
    public int numSquares(int n) {
        // 时间复杂度：O(n)
        // 空间复杂度：O(n)
        // 转化为在无权图中找最短路径
        // 建图：0到n每个数字代表节点，若数字x到y相差一个完全平方数则连接这条边
        // 队列（数字，步数）。先入队（n，0）
        // 声明BFS的visited数组(0到n个)。最后一个数字n设为已访问
        // 当队列不为空时，while循环
        // -- 取出队首，若num为0，return step
        // -- for循环，i从1开始自增，当num-i*i>=0
        // ---- 如果num-i*i没有被访问过，入队，step+1。且visited设为true
        
        LinkedList<Pair<Integer, Integer>> queue = new LinkedList<Pair<Integer, Integer>>();
        queue.addLast(new Pair<Integer, Integer>(n, 0));
        
        boolean[] visited = new boolean[n+1];
        visited[n] = true;
        
        while(!queue.isEmpty()) {
            Pair<Integer, Integer> front = queue.removeFirst();
            int num = front.getKey();
            int step = front.getValue();
            
            if(num == 0) {
                return step;
            }
            
            for(int i=1; num-i*i>=0; i++) {
                if(!visited[num-i*i]) {
                    queue.addLast(new Pair<Integer, Integer>(num-i*i, step+1));
                    visited[num-i*i] = true;
                }
            }
        }
        throw new IllegalStateException("No solution");
    }
}
```
- 优化：`num-i*i`计算了四次

```java
import javafx.util.Pair;

class Solution {
    public int numSquares(int n) {
        // 时间复杂度：O(n)
        // 空间复杂度：O(n)
        // 转化为在无权图中找最短路径
        // 建图：0到n每个数字代表节点，若数字x到y相差一个完全平方数则连接这条边
        // 队列（数字，步数）。先入队（n，0）
        // 声明BFS的visited数组(0到n个)。最后一个数字n设为已访问
        // 当队列不为空时，while循环
        // -- 取出队首，若num为0，return step
        // -- for循环，i从1开始自增，当num-i*i>=0
        // ---- 如果num-i*i没有被访问过，入队，step+1。且visited设为true
        
        LinkedList<Pair<Integer, Integer>> queue = new LinkedList<Pair<Integer, Integer>>();
        queue.addLast(new Pair<Integer, Integer>(n, 0));
        
        boolean[] visited = new boolean[n+1];
        visited[n] = true;
        
        while(!queue.isEmpty()) {
            Pair<Integer, Integer> front = queue.removeFirst();
            int num = front.getKey();
            int step = front.getValue();
            
            if(num == 0) {
                return step;
            }
            
            for(int i=1; num-i*i>=0; i++) {
                // 优化1：用a存储
                int a = num-i*i;
                if(!visited[a]) {
                    // 优化2：提前return
                    if(a == 0) {
                        return step + 1;
                    }
                    queue.addLast(new Pair<Integer, Integer>(a, step+1));
                    visited[a] = true;
                }
            }
        }
        throw new IllegalStateException("No solution");
    }
}
```

### 347题解
- 前K大（优先队列+PairComparator），频率（Map）
```java
import javafx.util.Pair;

class Solution {
    public List<Integer> topKFrequent(int[] nums, int k) {
        // 时间复杂度：O(nlogk)
        // 空间复杂度：O(n+k)
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
}
```