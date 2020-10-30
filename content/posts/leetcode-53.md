---
title: "Leetcode 53 最大子序和"
date: 2020-10-30T10:46:00+08:00
draft:  false
---

## 题目

给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

示例:

    输入: [-2,1,-3,4,-1,2,1,-5,4]
    输出: 6
    解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。


## 思路

这是一道动态规划题目，按照一般步骤，我们需要定义 dp[i] 的意义

在这题中 dp[i] 被定义为 以nums[i]结尾的最大子序和

那么按照数学归纳法，假设我们已知 dp[i-1] 如何求 dp[i]

稍加分析，我们会发现，dp[i] 的值只有两种情况

    dp[i] = dp[i-1] + nums[i]

    OR

    dp[i] = nums[i]

那么写出状态转移方程 dp[i] = max{dp[i-1] + nums[i], nums[i]}

最终答案就是 dp[0...i] 中最大的一个值

我们利用遍历得出即可

## 题解

```Java
class Solution {
    public int maxSubArray(int[] nums) {
        int length = nums.length;
        int[] dp = new int[length];
        dp[0] = nums[0];
        for (int i=1; i<length; i++) {
            dp[i] = Math.max(nums[i], dp[i-1] + nums[i]);
        }
        int result = Integer.MIN_VALUE;
        for (int i=0; i<length; i++) {
            result = Math.max(result, dp[i]);
        }
        return result;
    }
}
```

## 总结

这是一题比较特别的动态规划，与其他题目对 dp[i] 的定义有所不同

但只需实际分析 dp[i] 会出现的两种结果，即可得出答案