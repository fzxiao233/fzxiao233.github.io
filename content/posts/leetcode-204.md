---
title: "Leetcode每日一题 204 计数质数"
date: 2020-12-03T19:53:41+08:00
draft: false
---

## 题目

统计所有小于非负整数 n 的质数的数量。

 

示例 1：

    输入：n = 10
    输出：4
    解释：小于 10 的质数一共有 4 个, 它们是 2, 3, 5, 7 。

>来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/count-primes
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

### 暴力法

质数的定义：一个大于 1 的只能被 1 和它本身整除的整数

那么只需要让这个数 x 与 [2, x-1] 内的所有整数取模，计数模等于 0 的次数

如果次数为零，则它为质数，否则为素数

```Java
class Solution {
    public int countPrimes(int n) {
        int count = 0;
        for(int i = 2; i < n; i++) {
            int t = 0;
            for(int c = 2; c < i; c++) {
                if(i % c == 0) {
                    t++;
                }
            }
            if(t == 0) {
                count++;
            }
        }
        return count;
    }
}
```

这个方法的时间复杂度为 O(n)， 当 n 很大的时候，无法短时间内完成

### 埃氏筛法

埃氏筛提出一个观点

如果 i 是一个质数，那么大于 i 的 i 的倍数一定不是质数

那么，我们从 2 开始，如果该数是质数(显然 2 是质数)就将比 2 大的 2 的倍数标记为合数

最好计数未标记数即可

这个方法还有优化的空间

因为 i 的倍数 2x 3x 在标记前一定被之前的标记回中标记过了

所以，我们可以从 x^2 开始标记

```Java
class Solution {
    public int countPrimes(int n) {
        boolean[] result = new boolean[n];
        Arrays.fill(result, false);
        for(int i = 2; i * i < n; i++) {
            if(result[i] == false) {
                for(int j = i * i; j < n; j = j + i) {
                    result[j] = true;
                }
            }
        }
        int count = 0;
        for (int i = 2; i < n; i++) {
            if(result[i] == false) {
                count++;
            }
        }
        return count;
    }
}
```

有了思路，代码很简单

## 吐槽

这个每日一题怎么写个简单啊，只有暴力法简单好吗！

