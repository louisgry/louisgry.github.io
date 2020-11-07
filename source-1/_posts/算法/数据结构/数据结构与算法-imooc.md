---
title: 数据结构与算法-imooc
date: 2019-09-01 22:26:51
categories: 算法
mathjax: true
tags: 
- Ebbinghaus
- 数据结构
description: 一、常用数据结构：1. 线性结构（数组、栈、队列、链表）、2. 树结构（二分搜索树、堆）、3. 图结构<br>二、排序查找算法：1. 排序、2. 查找<br>三、算法分析：1. 如何解题、2. 复杂度分析
---

# 一、常用数据结构
常见的三种结构：线性结构（数组、栈、队列、链表、哈希表），树结构（二叉树、堆、线段树、Trie、并查集、AVL、红黑树），图结构

## 线性结构
### 数组
```
List<Integer> list = new ArrayList<Integer>();
```
[code](https://github.com/louisgry/Algorithm/tree/master/data-structure/src/linear/ArrayList.java)
- 添加：`list.add(e)`，O(n)
    - `list.addFirst(e)`，O(n)
    - `list.addLast(e)`，**O(1)**
- 删除：`list.remove(index)、list.remove(e)`，O(n)
    - `list.removeFirst()`，O(n)
    - `list.removeLast()`，**O(1)**
- 查找：`list.get(index)`，**O(1)**
- 修改：`list.set(index, e)`，O(n)
- 包含：`list.contains(e)`，O(n)
- 扩容：`grow()`
    - add时`grow(2*data.length)`
    - remove时`grow(data.length/2)`（但size==data.length/4）


### 栈
```
Stack<Character> stack = new Stack<Character>();
```
[code](https://github.com/louisgry/Algorithm/tree/master/data-structure/src/linear/Stack.java)
- 入栈：`stack.push(e)`，O(1)
- 出栈：`stack.pop()`，O(1)
- 查看栈顶：`stack.peek()`，O(1)
- 判断是否为空：`stack.empty()`

### 队列
```
Queue<Pair<Integer, Integer>> queue = new LinkedList<Pair<Integer, Integer>>();
```
[code](https://github.com/louisgry/Algorithm/tree/master/data-structure/src/linear/Queue.java)
- 入队：`queue.add(e)`，O(logn)
- 出队：`queue.poll()`，O(logn)
- 查看队首：`queue.peek()`，O(1)
- 判定是否为空：`queue.isEmpty()`

### 链表
```
LinkedList<Integer> list = new LinkedList<Integer>();
```
[code](https://github.com/louisgry/Algorithm/tree/master/data-structure/src/linear/LinkedList.java)
- 添加：`list.add(e)`，O(n)
    - `list.addFirst(e)`，**O(1)**
    - `list.addLast(e)`，O(n)
- 删除：`list.remove(index)、list.remove(e)`，O(n)
    - `list.removeFirst()`，**O(1)**
    - `list.removeLast()`，O(n)
- 查找：`list.getFirst()`，**O(1)**、`list.getLast()`，O(n)
- 修改：`list.set(index, e)`，O(n)
- 包含：`list.contains(e)`，O(n)


### 哈希表

## 树结构
### 二分搜索树
```
TreeSet<Long> set = new TreeSet<>();
TreeMap<Integer, Integer> map = new TreeMap<Integer, Integer>();
```
[code](https://github.com/louisgry/Algorithm/tree/master/data-structure/src/tree/BST.java)
- 添加：`bst.add(e)`，O(logn)
- 删除：`bst.remove(e)`，O(logn)
- 包含：`bst.contains(e)`，O(logn)
- 遍历
    - 前序遍历：`bst.preOrder()`，O(n)，可借助Stack
    - 中序遍历：`bst.inOrder()`，O(n)
    - 后序遍历：`bst.postOrder()`，O(n)
    - 层序遍历：`bst.levelOrder()`，O(n)，可借助Queue
- 最值：
    - 最大值：`bst.maximum()`，O(logn)
    - 最小值：`bst.minimum()`，O(logn)

### 堆
```
PriorityQueue<Pair<Integer, Integer>> pq = new PriorityQueue<Pair<Integer, Integer>>(new PairComparator());
```
[code](https://github.com/louisgry/Algorithm/tree/master/data-structure/src/tree/Heap.java)


### 线段树

### Trie

### 并查集

### AVL

### 红黑树

## 图结构
### 图的基础
### 最小生成树
### 最短路径

# 二、排序查找算法
## 排序
### 选择排序

### 插入排序
- 插入排序思想：两层for循环，if后面(arr[j])比前面(arr[j-1)小则swap，否则break

### 归并排序

### 快速排序

## 查找
### 二分查找
- 迭代版：O(logn)
```java
int binarySearch(int[] nums, int target) {
    int l=0, r=nums.length-1;
    // l==r时，[l, r]区间依然有效
    while(l <= r) {
        int mid = l + (r-l)/2;
        if(nums[mid] == target) {
            return mid;
        }
        if(target > nums[mid]) {
            l = mid + 1;
        }
        else {
            r = mid - 1;
        }
    }
    return -1;
}
```
- 递归版：O(logn)，递归深度是logn（每次减少一半）
```java
int binarySearch(int[] nums, int l, int r, int target) {
    if(l > r) {
        return -1;
    }
    int mid = l + (r-l)/2;
    if(nums[mid] == target) {
        return mid;
    }
    else if(nums[mid] > target) {
        return binarySearch(nums, l, mid-1, target);
    }
    else {
        return binarySearch(nums, mid+1, r, target);
    }
}
```

# 三、算法分析
## 如何解题
### 边界条件
- 注意题目的条件：有序、10^7规模、logn...
- 正确是相对的：“对一组数据进行排序？”
    - 有大量重复元素：三路快排
    - 近乎有序：插入排序
    - 取值范围有限 (学生成绩)：计数排序
    - 稳定排序：归并排序
    - 链表存储、内存：外排序

### 解题思路
- 没有思路时：简单用例、暴力解法
- 常规思路
    - 遍历常见的数据结构
    - 遍历常见的算法思路
    - 用空间换时间（哈希表）
    - 数据预处理（排序）

## 复杂度分析
### 大O表示法
- `O(f(n))`：表示运行算法所需要执行的指令数，和f(n)成正比（表示算法执行的最低上界，最好能这么好）
- 量级不同不能省略：O(AlogA+B)
    - 如，邻接表图遍历：O(V+E)
- 一个时间复杂度问题
    - 字符串数组先将每一个字符串排序，再对数组进行字典序排序
    - 不是：O(n*nlogn+nlogn)=O(n^2logn)
    - 而是：O(n*slogs+s*nlogn)，s字符串最长长度，n数组长度
- 算法复杂度在某些情况下是用例相关的，但一般关于平均情况

### 数据规模
- 要想在1s内解决问题
    - O(n^2) -> 10^4
    - O(n) -> 10^8
    - O(nlogn) -> 10^7
- 对比：O(n)、O(n^2)、O(logn)、O(nlogn)
    - O(n) ==> 规模增加两倍，时间翻一倍（findMax）
    - O(n^2) ==> 规模增加两倍，时间翻四倍（selectionSort）
    - O(logn) ==> 规模增加两倍，2亿数据规模时间也不到1ms（binarySearch）
    - O(nlogn) ==> 规模增加两倍，时间差不多翻一倍 （mergeSort）

### 时间复杂度分析
- logn：二分查找：n经过几次除以2操作后等于1（log2n）、intToString（log10n）
- 2^n：斐波那契数列，f(n) = f(n-1) + f(n-2)（每次乘以2）
- sqrt(n)：判定是否为素数，`for(int i=2; i*i<=n; i++)`

### 递归复杂度分析
- 递归的空间复杂度：递归深度多少，空间复杂度就是多少（系统栈存储的状态个数）
- 递归方程及复杂度分析：公式法/主定理
    - $T(n)=aT(n/b)+f(n)$
    - 若$O(n^{log_ba}) > f(n)$：$O(n^{log_ba})$
    - 若$O(n^{log_ba}) < f(n)$：$f(n)$
    - 若$O(n^{log_ba}) = f(n)$：$O(n^{log_ba})*logn$