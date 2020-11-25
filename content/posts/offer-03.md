---
title: "剑指Offer 03 数组中的重复数字"
date: 2020-11-25T14:48:34+08:00
draft: false
---

## 题目

找出数组中重复的数字。


在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

示例 1：

    输入：
    [2, 3, 1, 0, 2, 5, 3]
    输出：2 或 3 

>来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 一解(排序法)

先将数组排序，那么重复的数字必然排在一起

```Java
class Solution {
    public int findRepeatNumber(int[] nums) {
        Arrays.sort(nums);
        for(int i = 1; i < nums.length; i++) {
            if(nums[i] == nums[i-1]) {
                return nums[i];
            }
        }
        return -1;
    }
}
```

## 对应数组法

构造一个新数组，用于存储已经找到的值

如果值已存在，那该数重复

```Java
class Solution {
    public int findRepeatNumber(int[] nums) {
        int[] mapNums = new int[nums.length];
        for(int i = 0; i < nums.length; i++) {
            if(nums[i] == mapNums[nums[i]]) {
                return nums[i];
            } else {
                mapNums[nums[i]] = nums[i];
            }
        }
        return -1;
    }
}
```

## 哈希集合法

构造 HashSet

遍历数组将元素放入

如果放入失败，则该数重复

```Java
class Solution {
    public int findRepeatNumber(int[] nums) {
        var set = new HashSet<Integer>();
        for(int i = 0; i < nums.length; i++) {
            if(!set.add(nums[i])) {
                return nums[i]
            }
        }
        return -1;
    }
}
```