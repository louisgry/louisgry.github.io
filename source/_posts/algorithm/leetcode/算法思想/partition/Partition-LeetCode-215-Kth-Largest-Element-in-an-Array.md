---
title: 'Partition: LeetCode 215. Kth Largest Element in an Array'
date: 2019-09-03 23:31:13
categories: Algorithm
tags: 
- partition
---
- partition
    - 215 Kth Largest Element in an Array：https://leetcode.com/problems/kth-largest-element-in-an-array/ 
        - 返回第K大的数
        - Input: [3,2,1,5,6,4] and k = 2
        - Output: 5
        <!-- more -->
    - 思路：快排Partition思路
    - 时间复杂度：O(n)
    -  空间复杂度：O(logn)
    ```java
        public int findKthLargest(int[] nums, int k) {
            return findKthLargest(nums, 0, nums.length-1, k-1);
        }

        private int findKthLargest(int[] nums, int l, int r, int k){
            if(l==r)
                return nums[l];
            int p = partition(nums, l, r);

            if( p == k )
                return nums[p];
            else if( k < p )
                return findKthLargest(nums, l, p-1, k);
            else // k > p
                return findKthLargest(nums, p+1 , r, k);
        }

        private int partition(int[] nums, int l , int r){
            int p = (int) (Math.random()%(r-l+1) + l);
            swap(nums, l, p);
            int lt = l + 1;
            for( int i = l + 1 ; i <= r ; i ++ )
                if( nums[i] > nums[l] )
                    swap(nums, i, lt++);
            swap(nums, l, lt-1);
            return lt-1;
        }

        private void swap(int[] nums, int i, int j){
            int t = nums[i];
            nums[i] = nums[j];
            nums[j] = t;
        }
    ```