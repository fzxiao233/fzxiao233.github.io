---
title: "Leetcode 206 反转链表"
date: 2020-11-21T14:59:58+08:00
draft: false
---

## 题目

反转一个单链表。

示例:

    输入: 1->2->3->4->5->NULL
    输出: 5->4->3->2->1->NULL

进阶:

你可以迭代或递归地反转链表。你能否用两种方法解决这道题？

>来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/reverse-linked-list
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 一解(迭代法)

使用一个辅助栈

先将链表内所有节点放入辅助栈中，因为栈是先入后出的，所以在取出的时候能够自然的将链表反转

```Java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
        if(head == null || head.next == null) {
            return head;
        }
        Stack<ListNode> stack = new Stack<ListNode>();
        for(ListNode p = head; p != null; p = p.next) {
            stack.push(p);
        }
        ListNode p = null;
        ListNode q = null;
        ListNode res = null;
        while(!stack.isEmpty()) {
            p = stack.pop();
            if(q == null) {
                res = p;
                q = p;
                continue;
            }
            q.next = p;
            q = p;
        }
        q.next = null;
        return res;
    }
}
```

显然不需要辅助栈也能实现

我们定义两个变量

prev 和 curr 分别表示前一个节点和当前节点

当 curr 不是 null 即链表未到底时，循环的将 curr.next 指定为上一元素 prev 

并将当前元素放入上一元素 将当前元素的下一元素(curr.next) 存入当前元素(curr)

```Java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode curr = head;
        while(curr != null) {
            ListNode tempNode = curr.next;
            curr.next = prev;
            prev = curr;
            curr = tempNode;
        }
        return prev;
    }
}
```

## 二解(递归法)

看一个例子

有这么一个链表

    1 -> 2 -> 3 -> null

当递归到最底前 即 head.next == null 时，我们返回当前节点

此时的 head = 3

返回后，此时 head = 2

我们要将链表反转也就是需要将 3 指向 2

只需 head.next.next = head

即

    2 -> 3 -> 2

此时链表发生循环，还需要

head.next = null

即

    3 -> 2 -> null

完整代码如下

别忘了考虑链表为空的情况

```Java
class Solution {
    public reverseList(ListNode head) {
        if(head == null || head.next == null) {
            return head;
        }
        Listnode p = reverseList(head.next);
        head.next.next = head;
        head.next = null;
        return p;
    }
}
```