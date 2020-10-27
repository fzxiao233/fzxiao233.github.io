---
title: "Leetcode 21 合成有序链表"
date: 2020-10-27T20:09:54+08:00
draft: false
---

## 题目

将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

示例：

    输入：1->2->4, 1->3->4
    输出：1->1->2->3->4->4


## 思路 && 题解

这边马上想到的是遍历

根据所给条件

我们遍历两链表

例：

    l1 = l1.Next

为了让新链表升序，我们需要比较当前节点数据的大小

将更小的节点拼接在新链表上

这里可以写出初步框架

不妨设定头结点为 headNode

结果链表为 result

```Golang
func mergeTowLists(l1 *ListNode, l2 *ListNode) *ListNode {
    headNode := &ListNode{}

    result := headNode

    for l1 != nil && l2 != nil {
        if l1.Val <= l2.Val {
			headNode.Next = &ListNode{Val:l1.Val}
			l1 = l1.Next
		} else {
			headNode.Next = &ListNode{Val:l2.Val}
			l2 = l2.Next
        }
        headNode = headNode.Next
    }
}
```

遍历结束后

必然存在一个链表不为 nil

由于链表本身已是升序排列

故只需将其链接在后即可

```Golang
    func mergeTowLists(l1 *ListNode, l2 *ListNode) *ListNode {
    headNode := &ListNode{}

    result := headNode

    for l1 != nil && l2 != nil {
        if l1.Val <= l2.Val {
			headNode.Next = &ListNode{Val:l1.Val}
			l1 = l1.Next
		} else {
			headNode.Next = &ListNode{Val:l2.Val}
			l2 = l2.Next
        }
        headNode = headNode.Next
    }
    if l1 == nil {
        headNode.Next = l2
    } else {
        headNode.Next = l1
    }
    return result.Next
}

```

注 由于我们是从头结点开始拼接的，故应返回头结点的下一节点


## 总结

第一次写链表题目，一时间被绕在里边了，不知道怎么切换节点 wwww

只要把下一节点赋给当前节点变量就好了

或许这题用递归写更合适？

## 递归解法

一个递归总要有个结束条件

我们的链表到底就是 nil 那么当l1, l2 两个链表都为 nil 的时候，递归应该结束

即

    if l1 == nil{
        return l1
    } else if l2 == nil {
        return l2
    }


再来分析递归的目的

我们要使得最后生成的链表头最小

那么递归就是找到最大值，将链表之后向前生成

如下代码

```Golang
    if l1.Val < l2.Val {
        l1.Next = mergeTwoLists(l1.Next, l2)
        return l1
    } else {
        l2.Next = mergeTwoLists(l1, l2.Next)
        return l2
    }
```

距离

l1 = [0,1]

l2 = [1,2]

    <1>

    l1.Val(0) < l2.Val(1)

    ->l1.Next(1), l2(1)
        <2>
            l1.Val(1) !< l2.Val(1)

            ->l1(1), l2.Next(2)
                <3>
                    l1.Val(1) < l2.Val(2)

                    -> l1.Next(nil), l2(2)
                        <4>
                            return l2(2)

                        l1.Next = l2(2)
                        return l1

                l2.Next = l1(1)->(2)
                return l2

            l1.Next = l2(1)->l1(1)->l2(2)
            return l1

        l1 = l1(0)->l2(1)->l1(1)->l2(2)

> -> 后表示传递给递归函数的值

完全代码如下

```Golang
func mergeTwoLists(l1 *ListNode, l2 *ListNode) *ListNode {
	if l1 == nil {
		return l2
	} else if l2 == nil {
		return l1
	}

	if l1.Val < l2.Val {
		l1.Next = mergeTwoLists(l1.Next, l2)
		return l1
	} else {
		l2.Next = mergeTwoLists(l1, l2.Next)
		return l2
	}
}
```

递归是一种自顶而下的方法，我也觉得有些难懂，但多加练习，一定会变好的