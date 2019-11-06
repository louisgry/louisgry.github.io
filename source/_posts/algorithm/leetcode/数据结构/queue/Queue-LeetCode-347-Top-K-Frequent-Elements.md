---
title: 'Queue: LeetCode 347. Top K Frequent Elements'
date: 2019-10-26 17:16:03
categories: Algorithm
tags: 
- queue
- map
---
- queue
    - 347 Top K Frequent Elements：https://leetcode.com/problems/top-k-frequent-elements/
        - 返回前k个出现频率最高的元素
        - Input: nums = [1,1,1,2,2,3], k = 2
        - Output: [1,2]
        <!-- more -->
    - 思路：优先队列
    - 时间复杂度：O(nlogk)
    - 空间复杂度：O(n+k)
    ```java
    import javafx.util.Pair;

    public List<Integer> topKFrequent(int[] nums, int k) {
        // 统计每个元素出现的频率：map（元素，频率）
        HashMap<Integer, Integer> freq = new HashMap<Integer, Integer>();
        for(int i=0; i<nums.length; i++) {
            if(!freq.containsKey(nums[i])) {
                freq.put(nums[i], 1);
            }
            else {
                freq.put(nums[i], freq.get(nums[i])+1);
            }
        }
        // 维护出现频率最高的k个元素：优先队列（频率，元素）
        PriorityQueue<Pair<Integer, Integer>> pq = new PriorityQueue<Pair<Integer, Integer>>(new PairComparator());
        for(Integer num : freq.keySet()) {
            int numFreq = freq.get(num);
            if(pq.size() == k) {
                if(numFreq > pq.peek().getKey()) {
                    pq.poll();
                    pq.add(new Pair<Integer, Integer>(numFreq, num));
                }
            }
            else {
                pq.add(new Pair<Integer, Integer>(numFreq, num));
            }
        }
        
        ArrayList<Integer> res = new ArrayList<Integer>();
        while(!pq.isEmpty()) {
            res.add(pq.poll().getValue());
        }
        return res;
    }
    // 基于Pair的最大堆
    private class PairComparator implements Comparator<Pair<Integer, Integer>>{
        @Override
        public int compare(Pair<Integer, Integer> p1, Pair<Integer, Integer> p2) {
            if(p1.getKey() != p2.getKey()) {
                return p1.getKey() - p2.getKey();
            }
            return p1.getValue() - p2.getValue();
        }
    }
    ```