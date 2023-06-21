---
title: "02-数据结构"
subtitle: "链表"
layout: post
author: "rainchxy"
header-style: text
hidden: true
tags:
  - 数据结构
  - acwing
  - 链表
---

# 基础知识：
- 如果在switch() case:的多个case中，定义同名变量，编译会报错；但是如果在并列的if()else if()else if()中分别定义同名变量，编译是不会报错的。

# 链表：单链表、双链表
## 单链表
- 思想：
> - 数组模拟链表，而非struct；数组模拟链表可以实现指针实现链表的所有操作；(静态链表)
> - 用途:实现邻接表，用于存储图、树；
- 模板：
> - 用到4个属性：head头节点的下标，e[N]存储值,ne[N]存储下一个节点序号，idx存储当前用到的点的序号；两个数组用下标关联起来；空节点用序号-1表示;
> - 用到个操作：init，add_to_head(次函数包含了使用链表的80%操作)，add，remove；
> - init中，不用将ne[N]都初始化为-1，因为head被init为-1了，且每次插入操作后，该-1会一直被保留在尾部。
> - 插入不区分是head还是其他位置；但是删除区分是在head位置，还是其他位置。
- [习题](https://www.acwing.com/problem/content/828/)

```
#include <iostream>

using namespace std;

const int N=1e5+10;
int head, e[N], ne[N], idx;

void init(){
    head=-1;
    idx=0;
    //do not need to initialize ne[N] to -1,because head is initialized to -1.
}

void add_to_head(int x){
    e[idx]=x; ne[idx]=head; head=idx++;
}

void add(int k, int x){
    e[idx]=x; ne[idx]=ne[k]; ne[k]=idx++;
}

void remove(int k){
    ne[k]=ne[ne[k]];
}

int main(){
    init();
    int m;
    cin >> m;
    while(m--){
        char op;
        cin >> op;
        int k,x;
        switch(op){
            
            case 'H':
                cin >> x;
                add_to_head(x);
            break;
            case 'I':
                cin >> k >> x;
                //do not neet to distinguish head or other places
                add(k-1, x);
            break;
            case 'D':
                cin >> k;
                if(!k)head=ne[head];
                else remove(k-1);
            break;
            default:
            break;
        }
    }
    for(int i=head; i!=-1; i=ne[i])cout << e[i] << ' ';

    return 0;
}
```

## 双链表
- 思想：
> - 数组模拟链表，而非struct；数组模拟链表可以实现指针实现链表的所有操作；(静态链表)
> - 用途:用于实现某些问题的优化；
- 模板：
> - 属性：相比单链表多了左向指针与尾序号2个属性。实际用4个属性，是因为有2个属性head,tail默认放在0,1位置了：e[N]存储值,l[N]存储左侧下一个节点序号，r[N]存储右侧下一个节点序号，idx存储当前用到的点的序号；两个数组用下标关联起来；链表结尾用序号1表示（而非-1）;
> - 操作：init，add，remove；
> - init操作总共3步：头尾互指，当前待使用位置idx；
> - add操作总共6步：新建节点，新建2条指向关系，该变2条指向关系,idx++；在k的左边/右边插入是同等操作，只需实现1个即可；
> - remove操作总共2步：让k的左右两点互指即可；
> - 第一次插入的位置是2，所以第k次插入的位置是k+1;(不同于单链表的k-1)
> - 只实现向k右侧的insert操作，那么向头节点插入是insert(0,x);向尾节点插入是insert(l[1],x);向k左侧插入是insert(l[k+1],x);向k右侧插入是insert(k+1,x)。
- [习题](https://www.acwing.com/problem/content/829/)

```
#include <iostream>

using namespace std;

const int N=1e5+10;
int e[N], l[N], r[N], idx;

void init(){
    r[0]=1; l[1]=0; idx=2;
}

//insert to right k
void insert(int k, int x){
    e[idx] = x; l[idx] = k; r[idx] = r[k]; l[r[k]] = idx; r[k] = idx++;
}

void remove(int k){
    l[r[k]] = l[k]; r[l[k]] = r[k];
}

int main(){
    init();
    int m;
    cin >> m;
    while(m--){
        string op;
        cin >> op;
        int k,x;
        if("L" == op){
            cin >> x;
            insert(0, x);
	    //insert head is add to right side of 0;
        }else if("R" == op){
            cin >> x;
            insert(l[1], x);
	    //insert tail is add to right side of l[1];
        }else if("IL" == op){
            cin >> k >> x;
            insert(l[k+1], x);
	    //is l[k+1],not add(k,x)
        }else if("IR" == op){
            cin >> k >> x;
            insert(k+1, x);
	    //first insert idx is 2, so k+1(different from single list)
        }else if("D" == op){
            cin >> k;
            remove(k+1);
        }
    }
    for(int i=r[0]; i!=1; i=r[i]){
        //the end of double list is not -1, is 1.
        cout << e[i] << ' ';
    }
    return 0;
}
```

References
----------

- <https://www.acwing.com/>
- <https://www.markdownguide.org/>
