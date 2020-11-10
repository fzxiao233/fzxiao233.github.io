---
title: "Leetcode 543 二叉树的直径"
date: 2020-11-10T09:40:11+08:00
draft:  false
---

## 题目

    给定一棵二叉树，你需要计算它的直径长度。一棵二叉树的直径长度是任意两个结点路径长度中的最大值。这条路径可能穿过也可能不穿过根结点。

     

    示例 :
    给定二叉树

            1
           / \
          2   3
         / \     
        4   5    
    返回 3, 它的长度是路径 [4,2,1,3] 或者 [5,2,1,3]。

     

    注意：两结点之间的路径长度是以它们之间边的数目表示。

>来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/diameter-of-binary-tree
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 分析

题目给出 二叉树的直径 是 任意两个结点路径长度的最大值

原问题等价于 求二叉树 以任意结点所能得到的最大路径

路径长度 等于 该节点的左深度L 与 右深度R 的和 加一（加上当前根节点）

我们表示为 Dnode = L + R + 1

我们所求的答案就是 Dnode 的最大值

## 题解

首先，我们定义一个全局变量 ans 表示最终答案, 并将其初始化为1

最终答案为 ans - 1

```Java
class Solution{
    int ans;

    public int diameterOfBinaryTree(TreeNode root) {
        ans = 1;
        return ans - 1;
    }
}
```

然后我们定义一个新函数 depth 用于求出最大左右深度: L, R

同时更新 ans 的结果 

```Java
    public int depth(TreeNode p) {
        if (p == null) {
            return 0;
        }
        int L = depth(p.left);
        int R = depth(p.right);
        ans = Math.max(ans, L + R + 1)
        return Math.max(L, R) + 1;
    }
```

在原函数上加上调用即可

全解如下:

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
    int ans;
    public int diameterOfBinaryTree(TreeNode root) {
        ans = 1;
        depth(root);
        return ans - 1;
    }
    public int depth(TreeNode p) {
        if (p == null) {
            return 0;
        }
        int L, R =0;
        L = depth(p.left);
        R = depth(p.right);
        ans = Math.max(ans, L + R + 1);
        return Math.max(L, R) + 1;
    }
}
```

## 总结

本题需要注意条件的转换

求最大值问题可以通过不断更新所求值来实现