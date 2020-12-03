---
title: "Leetcode 155 最小栈"
date: 2020-12-03T20:32:30+08:00
draft: false
---

## 题目

设计一个支持 push ，pop ，top 操作，并能在常数时间内检索到最小元素的栈。

push(x) —— 将元素 x 推入栈中。

pop() —— 删除栈顶的元素。

top() —— 获取栈顶元素。

getMin() —— 检索栈中的最小元素。
 

示例:

    输入：
    ["MinStack","push","push","push","getMin","pop","top","getMin"]
    [[],[-2],[0],[-3],[],[],[],[]]

    输出：
    [null,null,null,null,-3,null,0,-2]

解释：

    MinStack minStack = new MinStack();
    minStack.push(-2);
    minStack.push(0);
    minStack.push(-3);
    minStack.getMin();   --> 返回 -3.
    minStack.pop();
    minStack.top();      --> 返回 0.
    minStack.getMin();   --> 返回 -2.
 

提示：

    pop、top 和 getMin 操作总是在 非空栈 上调用。

>来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/min-stack
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 题解

用链表实现栈

在 push 和 pop 的时候生成最小元素存入 minELe 中

(我后来看题解发现，原来不用手动实现栈

```Java
class MinStack {
    class ListNode {
        int val;
        ListNode next;
        ListNode(int val) {
            this.val = val;
        }
    }
    int minEle = Integer.MAX_VALUE;
    ListNode head;
    /** initialize your data structure here. */
    public MinStack() {

    }
    
    public void push(int x) {
        ListNode t = new ListNode(x);
        t.next = this.head;
        this.head = t;
        if(this.minEle > x) {
            this.minEle = x;
        }
    }
    
    public void pop() {
        this.head = this.head.next;
        this.minEle = Integer.MAX_VALUE;
        for(ListNode t = this.head; t != null; t = t.next) {
            if(t.val < this.minEle) {
                this.minEle = t.val;
            }
        }
    }
    
    public int top() {
        return this.head.val;
    }
    
    public int getMin() {
        return this.minEle;
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
```

还有几种有趣的做法

一种是用辅助栈保存插入元素时，栈内的最小元素

另一种是自定义栈，让栈本身保存值和对应最小元素
