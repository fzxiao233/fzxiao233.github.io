---
title: "Leetcode 226 翻转二叉树"
date: 2020-11-05T08:47:55+08:00
draft: false
---

## 题目

翻转一棵二叉树。

示例：

输入：

         4
       /   \
      2     7
     / \   / \
    1   3 6   9
输出：

         4
       /   \
      7     2
     / \   / \
    9   6 3   1

>来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/invert-binary-tree
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 思路

翻转一颗二叉树，那还不简单

只有做到

把每一个节点的左节点和右节点互换

就将二叉树翻转了

那么很容易想到用递归

## 题解

定义递归函数 invertTree(root) 

作用是将 root 的 root.left 和 root.right 互换 并返回互换后的  root

结束条件是 

```Java
if (root == null) return root;
```

互换就很简单了不解释

完整解答

```Java
public TreeNode invertTree(TreeNode root) {
        TreeNode result = root;
        if (root == null) {
            return result;
        }

        root.left = invertTree(root.left);
        root.right = invertTree(root.right);
        TreeNode temp = root.left;
        root.left = root.right;
        root.right = temp;
        return root;
    }
```

## 总结

好像没有什么好总结的哦，因为被我一遍过了(
