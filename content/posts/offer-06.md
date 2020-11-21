---
title: "剑指 Offer 06. 从尾到头打印链表"
date: 2020-11-21T15:37:05+08:00
draft: false
---

## 题目

输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。

 

示例 1：

    输入：head = [1,3,2]
    输出：[2,3,1]

## 一解(辅助栈法)

思路简单，栈先进后出

将元素全放入栈后

在取出放入打印数组中

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
    public int[] reversePrint(ListNode head) {
        Stack<Integer> stack = new Stack<Integer>();
        for(ListNode p = head; p != null; p = p.next) {
            stack.push(p.val);
        }
        int size = stack.size();
        int[] print = new int[size];
        for(int i = 0; i<size; i++) {
            print[i] = stack.pop();
        }
        return print;
    }
}
```

## 二解(递归法)

我们定义一个类变量 list 用于存放链表值

递归的遍历链表并将元素值放入 list 中

最后生成 print 数组即可

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
    ArrayList<Integer> list = new ArrayList<Integer>();
    public int[] reversePrint(ListNode head) {
        addVal(head);
        int size = list.size();
        int[] print = new int[size];
        for(int i = 0; i< size; i++) {
            print[i] = list.get(i);
        }
        return print;
    }
    void addVal(ListNode p) {
        if(p == null) {
            return;
        }
        addVal(p.next);
        list.add(p.val);
    }
}
```


