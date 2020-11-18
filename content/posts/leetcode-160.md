---
title: "Leetcode 160 相交链表"
date: 2020-11-18T15:12:57+08:00
draft: false
---

## 题目

编写一个程序，找到两个单链表相交的起始节点。

注意：

- 如果两个链表没有交点，返回 null.
- 在返回结果后，两个链表仍须保持原有的结构。
- 可假定整个链表结构中没有循环。
- 程序尽量满足 O(n) 时间复杂度，且仅用 O(1) 内存
  
示例 1：

    输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
    输出：Reference of the node with value = 8
    输入解释：相交节点的值为 8 （注意，如果两个链表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。

>来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/intersection-of-two-linked-lists
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

### 一解

稍加分析，想到了两种方法

一是用 HashMap 边遍历边存节点，然后判断节点是否出现在 HashMap 中

但是这显然违背了 O(1) 内存

二是类似 strStr 函数的实现

直接暴力遍历

但是这是 O(mn) 的时间复杂度

总之先 AC 吧，就用第二种写了

```Java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if(headA == null || headB == null) {
            return null;
        }
        for(ListNode p = headA; p != null; p = p.next) {
            for(ListNode q = headB; q != null; q = q.next) {
                if (p == q) {
                    return p;
                }
            }
        }
        return null;
    }
}
```

AC 了，就是结果惨不忍睹

![](/imgs/leetcode-160/0.png)

## 二解

本方法就是官方题解中的第三解 双指针法

基本思路我们用一个例子来说明

假定两个链表

    a = [1, 3, 5, 7, 9, 11]
    b = [2, 4, 9, 11]

我们把这两个链表以不同顺序连在一起

    c = [1, 3, 5, 7, 9, 11, 2, 4, 9, 11]
    d = [2, 4, 9, 11, 1, 3, 5, 7, 9, 11]

得到 c, d 两个链表

我们发现有几个特点

- 链表长度一致（a + b = b + a）
- 尾端（交叉端一致）即 [9, 11] 这一段

    因为链表是前部不一致，后部交叉到一起，所以无论以什么顺序连接，两链表的最尾端总是一样的

那么本题就很简单了，以两个指针p, q同时遍历两个链表

指针到达尾部时，定位到另一链表的头部继续遍历

由于交叉端一致，总会有 p == q

此时，p/q 就是交叉的节点

```Java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if(headA == null || headB == null) {
            return null;
        }
        ListNode p = headA;
        ListNode q = headB;
        while(p != q) {
            p = p != null ? p.next: headB;
            q = q != null ? q.next: headA;
        }
        return p;
    }
}
```

有人就要问了，要是 p, q不是交叉的，岂不是会一直循环然后超时？

其实不会，我们看这个例子

    a = [1, 3, 4]
    b = [2, 4, 5, 6]

每次执行 p, q 的值我们用下面的这个表格表示

|p|q|
|-|-|
|1|2|
|3|4|
|4|5|
|null|6|
|2|null|
|4|1|
|5|3|
|6|4|
|null|null|

因为移动步数总是一致的（a + b）则，最后一步会同时到达 null

此时跳出循环 s