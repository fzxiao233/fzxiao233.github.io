---
title: "剑指Offer 05 替换空格——StringBuilder的应用"
date: 2020-11-25T14:46:31+08:00
draft: false
---


## 题目

请实现一个函数，把字符串 s 中的每个空格替换成"%20"。

 

示例 1：

    输入：s = "We are happy."
    输出："We%20are%20happy."

>来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

利用 StringBuilder 来构造字符串

如果对应字符为' ' 则换为 "%20"

```Java
class Solution {
    public String replaceSpace(String s) {
        var sb = new StringBuilder();
        for(int i = 0; i < s.length(); i++) {
            if(s.charAt(i) != ' ') {
                sb.append(s.charAt(i));
            } else {
                sb.append("%20");
            }
        }
        return sb.toString();
    }
}
```