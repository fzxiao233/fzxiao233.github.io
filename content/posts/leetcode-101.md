---
title: "Leetcode 101 对称二叉树"
date: 2020-11-03T07:46:37+08:00
draft: false
---

## 题目

给定一个二叉树，检查它是否是镜像对称的。

 

例如，二叉树 [1,2,2,3,4,4,3] 是对称的。

        1
       / \
      2   2
     / \ / \
    3  4 4  3
 

但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:

        1
       / \
      2   2
       \   \
        3    3


> 来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/symmetric-tree
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 思路分析

一个对称的二叉树具有以下几点性质

1. 节点 p,q 的值相等（第一步即 root 节点自身比较，显然相等）
2. 节点 p 左孩子与 节点 q 右孩子相等
3. 节点 p 右孩子与 节点 q 左孩子相等

这里我们选用递归来写

```Java
public boolean check(TreeNode p, TreeNode q) {

}
```

明确递归跳出的条件

1. 节点均为 null 时返回 true
   
   此时该节点显然对称

   ```Java
   if (p == null && q == null) {
       return true;
   }
   ```

2. 节点仅有一个为 null 时返回 false

    显然此时不对称

    ```Java
   if (p == null || q == null) {
       return false;
   }
   ```

3. 当前节点的值相等，且满足对称二叉树的 2，3 条性质

    ```Java
   return p.val == q.val && check(p.left, q.right) && check(p.right, q.left);
   ```
   
下面看完整题解

## 题解

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
    public boolean isSymmetric(TreeNode root) {
        return check(root, root);
    }
    public boolean check(TreeNode p, TreeNode q) {
        if (p == null && q == null) {
            return true;
        }
        if (p == null || q == null) {
            return false;
        }
        return p.val == q.val && check(p.left, q.right) && check(p.right, q.left);
    }
}
```

## 总结

这个题目摆明了就是让你递归，只要明确了二叉树对称的性质，即可轻松解题

能用递归解决的问题，大部分也能用含有辅助栈的迭代解决

## 迭代解法

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
    public boolean isSymmetric(TreeNode root) {
        return check(root, root);
    }
    public boolean check(TreeNode u, TreeNode v) {
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(u);
        queue.offer(v);
        while(!queue.isEmpty()) {
            u = queue.poll();
            v = queue.poll();
            if (u == null && v == null) {
                continue;
            }
            if (u == null || v == null || u.val != v.val) {
                return false;
            }
            queue.offer(u.left);
            queue.offer(v.right);
            queue.offer(u.right);
            queue.offer(v.left);
        }
        return true;
    }
}
```

其实本质和递归是一样的，因为递归其实自带了函数栈

我们每次判断后将一对子节点压入栈中

迭代的取出并判断

最终栈元素为零时，改二叉树就对称

否则就会在判断过程中被提前返回