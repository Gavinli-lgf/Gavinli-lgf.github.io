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
- [习题](https://www.acwing.com/problem/content/787/)


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
- 思想：也是分治；不同于快排选任意值分界，这里选中间序号分界；先递归划分子区间，再在子区间合并的时候排序；
- 模板：
	- 确定分界点：mid = l + r >> 1;
	- 递归排序左右两部分；
	- 归并——合二为一时将两个已排序的数组按顺序合并为一；(双指针算法)
	- 注：合并时，if(q[i] <= q[j])tmp[k++] = q[i++];一定先用"<="才能保证归并算法的稳定性；
- 其他知识点
	- 排序算法的稳定性：如果两个相同大小的数值，在排序前后相对位置不变，那么该排序算法是稳定的。(快排是不稳定的，归并排序是稳定的。快排经过优化也可以变成稳定的。)
- [习题](https://www.acwing.com/problem/content/789/)


```
void merge_sort(int q[], int l, int r)
{
    if (l >= r) return;

    int mid = l + r >> 1;
    merge_sort(q, l, mid);
    merge_sort(q, mid + 1, r);

    int k = 0, i = l, j = mid + 1;
    while (i <= mid && j <= r)
        if (q[i] <= q[j]) tmp[k ++ ] = q[i ++ ];
        else tmp[k ++ ] = q[j ++ ];

    while (i <= mid) tmp[k ++ ] = q[i ++ ];
    while (j <= r) tmp[k ++ ] = q[j ++ ];

    for (i = l, j = 0; i <= r; i ++, j ++ ) q[i] = tmp[j];
}
```

# 二分：整数二分，浮点数二分
- 二分的本质是可以按照某一性质，将整个数组已分为二。利用二分就可以查找这两个性质的边界。"单调性"只是这个性质之一。
1. 整数二分
- 思想：
	- 二分查找分2种：满足左侧性质的右边界点；满足右侧性质的左边界点。以从小到大排序的数组为例，左侧性质为查找"<x"或"<=x"的右边界；右侧性质为查找">x"或">=x"的左边界。
- 模板：
> - [yxc两种二分模板](https://www.acwing.com/blog/content/31/)
> - 通过循环查寻，循环条件是while(l < r);
> - 当查找满足左侧性质的右边界时，一定是if(check(mid))l = mid;else r = mid - 1;此时对应"mid - 1"的mid取值一定是"+1"的，即mid = l + r + 1 >> 1;
> - 当查找满足右侧性质的左边界时，一定是if(check(mid))r = mid;else l = mid + 1;此时对应"mid + 1"的mid取值一定是只有"l + r"的，即mid = l + r >> 1;
> - 最终的l值就是查找到的边界，因此return l;
> - 思考顺序：先任一计算一个mid；然后根据check()函数思考取间的划分，即l,r与mid的关系；如果有取到"mid - 1"，则再在求mid时取"l + r + 1"，否则取"l + r"。
> - 简便的记忆方法："r,l"按左右顺序排列，且值差1，因此r,l的取值为l取mid，r取mid-1;r取mid，l取mid+1。再对应到mid的取值上，mid-1时，mid=l+r+1>>1；mid+1时，mid=l+r。
- [习题](https://www.acwing.com/problem/content/791/)

```
满足if(check(mid))l = mid;else r = mid - 1时：
int bsearch_2(int l, int r)
{
    while (l < r)
    {
        int mid = l + r + 1 >> 1;
        if (check(mid)) l = mid;
        else r = mid - 1;
    }
    return l;
}
```

```
满足if(check(mid))r = mid;else l = mid + 1时：
int bsearch_1(int l, int r)
{
    while (l < r)
    {
        int mid = l + r >> 1;
        if (check(mid)) r = mid;
        else l = mid + 1;
    }
    return l;
}
```

2. 浮点数二分
- 思想
> - 浮点型没有整除问题，范围可以严格的每次缩小一半，因此也没有边界问题。
> - 循环结束条件，当划分的范围长度足够小时，任务搜索结束。
- 模板：
> - 经验：范围足够小的阈值，要小于题目要求的精度。比如题目要求6为小数，则范围阈值取1e-8。
- 习题：求开平方。

```
求x的开平方
double search(double x){
    double l = 0, r = x;
    while(r - l > 1e-8){
        double mid = (l + r) / 2;
        if(mid * mid > x)r = mid;
        else l = mid;
    }
    return l;
}

```


References
----------

- <https://www.acwing.com/>
- <https://www.markdownguide.org/>
