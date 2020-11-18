---
title: "Leetcode 141 环形链表——龟兔赛跑算法的应用"
date: 2020-11-18T14:36:31+08:00
draft: false
---

## 龟兔赛跑算法（Floyd 判圈算法）

假想有 乌龟 和 兔子 在链表上移动，兔子跑得快，乌龟跑得慢

如果乌龟和兔子均从链表上的同一个节点开始运动

当该链表有环时，兔子由于跑的快，会先进入环内开始循环，乌龟后进入环内

此时，总有一时刻，兔子会与乌龟相遇

如果该链表无环，兔子会先走出该链表

## 题目

给定一个链表，判断链表中是否有环。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。

如果链表中存在环，则返回 true 。 否则，返回 false 。

 

进阶：

你能用 O(1)（即，常量）内存解决此问题吗？

>来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/linked-list-cycle
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 分析

上面介绍了龟兔赛跑算法，这里我们就来实际使用一下

我们定义两个指针，分别是快指针 fast 和慢指针 slow 作为 兔子 和 乌龟

让 slow 指向链表头部， fast 指向头部的下一节点（为什么?）

当 slow 不等于 fast 时

让 slow 指针前进一个节点（乌龟跑得慢）

fast 指针前进两个节点（兔子跑得快）

如果有一刻，slow 等于 fast 则链表有环

否则无环

下面实现代码

## 题解

```Java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public boolean hasCycle(ListNode head) {
        if(head == null || head.next == null) {
            return false;
        }
        ListNode slow = head;
        ListNode fast = head.next;
        while(fast != slow) {
            if(fast == null || fast.next == null) {
                return false;
            }
            slow = slow.next;
            fast = fast.next.next;
        }
        return true;
    }
}
```

由于我们 while 循环的条件是 slow != fast 故我们让 fast 为 head 的下一节点，否则循环不会进入

这个题解就是使用了 O(1) 的内存空间

下面我们看一个使用 O(n) n 表示链表节点总数的方法

我们使用一个 hash 表来存储已访问过的节点

每次取到新节点时，判断节点是否存在于表中

如在，则链表有环

否则无环

```Java 
public class Solution {
    public boolean hasCycle(ListNode head) {
        if(head == null || head.next == null) {
            return false;
        }
        var nodes = new HashMap<ListNode, Integer>();
        for(ListNode p = head; p != null; p = p.next) {
            if(nodes.containsKey(p)) {
                return true;
            }
            nodes.put(p, 1);
        }
        return false;
    }
}
```

相比上一算法，这个方法比较粗暴，不具有优越性