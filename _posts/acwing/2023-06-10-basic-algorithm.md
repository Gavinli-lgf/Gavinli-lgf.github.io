---
title: "01-基础算法"
subtitle: "排序"
layout: post
author: "rainchxy"
header-style: text
hidden: true
tags:
  - 基础算法
  - acwing
  - 排序
---

# 基础知识：
    - 整数范围划分，都涉及边界问题，都需要模板；浮点不涉及边界问题。
    - 输入数据量比较大时，使用scanf(),效率高于cin。

# 排序：快排、归并
1. 快排
- 思想：分治；确定分界点，边调位置边分块，块分完了也排好序了。递归处理。
- 模板：
    - 递归结束条件if (l >= r) return;//区间内数据个数小于等于1个时，就不用排序了；所以比较循环的条件也必须是while(i<j);
    - 递归的第1次比较要将l,r的边界向两侧移动1位，是因为循环比较每次交换位置后，都要将交换位置的序号向内侧移动1位(用do-while实现)；因此循环的第1次先将序号外移，就能通用do-while的方式；
    - 分界点x左侧都<=x，右侧都>=x，所以i,j向内侧移动的条件是"<x"或">x"，所以一次循化过程并不会把与x相等的数都移动到中间；而可能分散在两个子区间。
    - 递归划分子区间时，分界点x的选取和子区间的划分，固定使用x=q[l],(l,j)(j+1,r);或者x=q[l+r>>1],(l,j)(j+1,r)两种方式，否则会出现边界问题。(即所有l,r与i,j使用是对称的，且对应的+或-1都要变，因此模板不要乱改。)
    - 每次swap之前，先要判断if(i<j)。因为分界点x在交换后的左右两侧中可能出现，那么就会出现某次循环完j<i的情况。
- 其他知识点：
    - C++中函数形参如果是数组，则实际传递的是指针，指向数组的首地址，如quick_sort(int q[])，因此存储原数据的数组int q[N]定义成全局的，或者mian中局部的变量，都是可以的。但是函数形参中传递vector时，就是一个拷贝，如quick_sort(voctor<int> q)，因此要用引用quick_sort(vector<int>& q)。
    - vector定义时用"()"其内部数值表示元素个数，相当于构造函数；用"{}"其内部数值相当于初始化，相当于赋值。

```
void quick_sort(int q[], int l, int r)
{
    if (l >= r) return;

    int i = l - 1, j = r + 1, x = q[l + r >> 1];
    while (i < j)
    {
        do i ++ ; while (q[i] < x);
        do j -- ; while (q[j] > x);
        if (i < j) swap(q[i], q[j]);
    }
    quick_sort(q, l, j), quick_sort(q, j + 1, r);
}
```
2. 归并
- 思想：
- 模板：


References
----------

- <https://www.acwing.com/blog/content/277/>
- <https://www.acwing.com/problem/content/787/>
