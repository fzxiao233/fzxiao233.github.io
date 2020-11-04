---
title: "Leetcode 104 二叉树的最大深度"
date: 2020-11-04T10:43:58+08:00
draft: false
---

## 题目

给定一个二叉树，找出其最大深度。

二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。

说明: 叶子节点是指没有子节点的节点。

示例：
    给定二叉树 [3,9,20,null,null,15,7]，

        3
       / \
      9  20
        /  \
       15   7
返回它的最大深度 3 。

>来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/maximum-depth-of-binary-tree
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 思路

看见二叉树，我就写递归（

首先，我们要求的是二叉树的最大深度

这最大深度是怎么构成的呢

最大深度 = max{最大左深度，最大右深度} + 1

而计算左右深度的方法是相同的

那么就很容易的写出一个递归函数用于给出最大深度


## 题解

定义递归函数 search(TreeNode p) 给出的是节点 p 的最大深度

```Java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public int maxDepth(TreeNode root) {
        return search(root);
    }
    public int search(TreeNode p) {
        int val = 0;
        int ans = 0;
        if (p == null) {
            return 0;
        }
        val++;
        ans += Math.max(val + search(p.left), val +search(p.right));
        return ans;
    }
}
```

细细一想写了很多废话

二稿

```Java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public int maxDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        return Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;
    }
}
```

## 总结

二叉树我就是递归，明确递归函数的定义即可秒杀