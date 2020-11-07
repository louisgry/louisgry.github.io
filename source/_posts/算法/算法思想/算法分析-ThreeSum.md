---
title: '算法分析-ThreeSum'
date: 2019-12-01 21:10:21
categories: 算法
tags: 
- 算法分析
description: 暴力解法、二分搜索、双指针
---


## ThreeSum问题
- 问题定义：找出数组中使得A+B+C=0的组合数量
- 接口定义：ThreeSum interface
```java
public interface ThreeSum {
    /**
     * 统计数组中和为0的三元组的数量
     * @param nums
     * @return 统计个数
     */
    public int countZero(int[] nums);
}
```

## ThreeSum题解
- 复杂度排序：logn < n < nlogn < n^2 < n^2logn < n^3

### 1. 暴力解法
```java
public class ThreeSumBrute implements ThreeSum {
    /**
     * 思路1：暴力解法，直接for循环三次
     * 复杂度：O(n^3)、O(1)
     */
    @Override
    public int countZero(int[] nums) {
        int res = 0;
        // 循环三次
        for(int i=0; i<nums.length; i++) {
            for(int j=i+1; j<nums.length; j++) {
                for(int k=j+1; k<nums.length; k++) {
                    if(nums[i]+nums[j]+nums[k] == 0) {
                        res++;
                    }
                }
            }
        }
        return res;
    }
}
```

### 2. 二分搜索
```java
public class ThreeSumBinarySearch implements ThreeSum {
    /**
     * 思路2：二分搜索，不含重复元素时可用
     * 复杂度：O(n^2logn)、O(1)
     */
    @Override
    public int countZero(int[] nums) {
        int res = 0;
        // 先排序
        Arrays.sort(nums);
        // 循环两次
        for(int i=0; i<nums.length; i++) {
            for(int j=i+1; j<nums.length; j++) {
                int target = nums[i]+nums[j];
                // 二分查找-(A+B)
                int index = binarySearch(nums, 0-target);
                // index不能在j前面，不然会重复统计
                if(index > j) {
                    res++;
                }
            }
        }
        return res;
    }
    private int binarySearch(int[] nums, int target) {
        int l=0, r=nums.length-1;
        while(l <= r) {
            int mid = l+(r-l)/2;
            if(nums[mid] == target) {
                return mid;
            }
            else if(nums[mid] < target) {
                l = mid + 1;
            }
            else {
                r = mid - 1;
            }
        }
        throw new IllegalStateException("No solution");
    }
}
```

### 3. 双指针
```java
public class ThreeSumTwoPointer implements ThreeSum {
    /**
     * 思路3：双指针，适用于无重复
     * 复杂度：O(n^2)、O(1)
     */
    @Override
    public int countZero(int[] nums) {
        int res = 0;
        // 先排序
        Arrays.sort(nums);
        // 循环两次
        for(int i=0; i<nums.length-2; i++) {
            int l=i+1, r=nums.length-1;
            int target = -nums[i];
            // 双指针寻找target
            while(l < r) {
                int sum = nums[l] + nums[r];
                // 找到target，l++，r--
                if(sum == target) {
                    res++;
                    l++;
                    r--;
                }
                // 比target小，l++
                else if (sum < target) {
                    l++;
                }
                // 比target大，r--
                else {
                    r--;
                }
            }
        }
        return res;
    }
}
```