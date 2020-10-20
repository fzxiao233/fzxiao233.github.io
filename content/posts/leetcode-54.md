---
title: "Leetcode 54 螺旋矩阵 附 人生中的第一次面试：字节跳动"
date: 2020-10-20T22:35:45+08:00
draft: false
---

## 背景

身为一名大一的学生，却想着投实习简历玩，居然无意命中字节跳动

于是乎，亲爱的面试官一题螺旋矩阵，让尚未刷题学习的我，成功回家等通知

多说无意，好好学习，下面看题目

## 题目

给定一个包含 m x n 个元素的矩阵（m 行, n 列），请按照顺时针螺旋顺序，返回矩阵中的所有元素。

示例 1:

    输入:
    [
        [ 1, 2, 3 ],
        [ 4, 5, 6 ],
        [ 7, 8, 9 ]
    ]
    输出: [1,2,3,6,9,8,7,4,5]

## 分析

正在面试的我，一脸懵逼，无从下手，面试官提示我这是一道很简单(本题难度中等)的题目 最后写了一点点，时间到了（菜 我 菜

其实这个题目，只需要抓住矩阵的边界条件，就可以很明朗的解决

不妨设输入矩阵为 matrix

那么矩阵中的每一行下标 用 x 表示

该行中的的元素下标 用 y 表示

那么访问矩阵中的每一个元素即可表示为

    element = matrix[x][y]

按照要求，我们需要螺旋的遍历这个矩阵

遍历方向是 右 => 下 => 上 => 左

每次碰到边界，即转向

循环直至完成遍历

定义

矩阵的行数 rows 

矩阵的列数 columns

注意到 rows 等于矩阵数组的长度

columns 等于 矩阵中一行的长度

即

    rows := len(matrix)

    columns := len(matrix[0])

那么边界条件即为

    0 <= x < rows

    0 <= y < columns

那么向右遍历的实质是 

    x = x
    y = y + 1

同理易得向下

    x = x + 1
    y = y

向左

    x = x
    y = y - 1

向上

    x = x - 1
    y = y

将方向控制抽象为两个数组

    directionX := [0, 1, 0, -1]
    directionY := [1, 0, -1, 0]

将改变方向定义为

    directionIndex = 0
    tx := x + directionX[directionIndex]
    ty := y + directionY[directionIndex]

显然 tx, ty 即为新访问坐标，那么 directionIndex 是什么

容易想到 directionIndex 的值 分别对应了不同的遍历方式

需要转向时，只需让 directionIndex 自增 以切换到下一种改变方式

Wait, 如果这样自增下去，directionIndex 就会越界

解决方案如下

    directionIndex = (directionIndex + 1) % 4

通过取模运算， 限制了 directionIndex 始终保持在方向数组的定义内

这样就结束了吗，显然不是

指针只是在最外圈打转而已，我们还需判断当前位置的元素是否已经访问过了

引入一个新矩阵 visited，矩阵大小与原输入矩阵大小相同，用于标记是否访问过

    visited := make([][]bool, rows)
    for i:=0; i<rows; i++ {
        visited[i] = make([]bool, columns)
    }

我们还需要更新边界条件

向原有的条件

    0 <= x < rows

    0 <= y < columns

后加入一条

    visited[x][y] != true

这样，当遇到已经遍历过的元素时，也会触发转向

综上述，我们来写代码

## 题解

```Golang
func spiralOrder(matrix [][]int) []int {
	var result []int  // 定义结果数组
	if len(matrix) == 0 {  // 判空
		return []int{}
    }
    
    rows, columns := len(matrix), len(matrix[0])  // 得到矩阵行，列数
    
    // 定义方向数组
	directionX := []int{0, 1, 0, -1}
    directionY := []int{1, 0, -1, 0}
    
    // 定义方向变量
    directionIndex := 0
    
    // 定义访问矩阵
	visited := make([][]bool, rows)
	for i := 0; i < rows; i++ {
		visited[i] = make([]bool, columns)
    }
    
    // 定义访问坐标
    var x, y int
    
    // 遍历，循环次数等于元素个数
	for i := 0; i < rows*columns; i++ {
		result = append(result, matrix[x][y])  // 将 x, y 处元素加入结果数组
		visited[x][y] = true  // 设定 x, y 已被访问
		tx, ty := x+directionX[directionIndex], y+directionY[directionIndex]  // 获得下一访问坐标 tx, ty
		if 0 <= tx && tx < rows && 0 <= ty && ty < columns && !visited[tx][ty] {  // 判断边界条件
			x, y = tx, ty
		} else {
            // 切换方向
			directionIndex = (directionIndex + 1) % 4
			x, y = x+directionX[directionIndex], y+directionY[directionIndex]
		}
	}
	return result
}
```

## 总结

涉及各种方式遍历的题目，无非就在考虑 边界条件 和 如何更改遍历方式

