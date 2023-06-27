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
- 注意在cout或cin中使用表达式的方式，要将表达式整体加上括号：cout << empty()?"YES":"NO" << endl;会报错，但是cout << (empty()?"YES":"NO") << endl;不会。
- 为了有效过滤掉输入字符串末尾的无效字符，使用如下输入方式：char op[2], str[N]; scanf("%s%s", op, str);
- char数组s,p可以采用如下的输入方式，从1开始存储；但是int数组却不可以：int n, m; char s[M], p[N]; cin >> n >> p + 1 >> m >> s + 1;


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

# 栈与队列
- 栈：先进后出；队列：先进先出。
- stl可以做的，数组模拟的都可以实现；但是数组模拟的操作，在stl中并不一定有。
## 栈
- 思想：
> - 数组模拟栈,插入、弹出都在栈尾；
- 模板：
> - 属性：2个， stk[N], tt(tail的意思，初始值为0，每次插入前先++，即从1开始存储。好处很多：方便判断是否为空；即使栈为空是调用query操作，不会导致内存错误。)
> - 操作：4个，插入、弹出、访问栈顶元素、判断栈是否为空。
- [习题](https://www.acwing.com/problem/content/830/)

```
const int N=1e5+10;
int stk[N], tt=0;

void push(int x){
    stk[++tt]=x;
}

void pop(){
    tt--;
}

bool empty(){
    return tt<=0;
}

int query(){
    return stk[tt];
}
```

## 队列
- 思想：
> - 数组模拟队列；队尾插入、对头弹出；
- 模板：
> - 属性：3个， q[N], hh, tt; hh初始为0，tail初始为-1(不同于栈)，每次插入前先++(同栈)，即从0开始存储，且方便通过hh与tt判断是否为空(hh>tt则为空);
> - 操作：5个，插入、弹出、访问队头元素、访问队尾元素(比stl多的)、判断队列是否为空。
- [习题]()

```
int q[N], hh=0, tt=-1;

void push(int x){
    q[++tt]=x;
}

void pop(){
    hh++;
}

bool empty(){
    return hh>tt;
}

int query(){
    return q[hh];
}
```

# 单调栈，单调队列
## 单调栈
- 思想：
> - 利用栈后入先出的性质，可以方便处理如下操作：顺序遍历的同时，又能方便处理离当前位置最近的问题。
> - 以下习题为例，顺序遍历的同时，始终维护一个以当前数据结尾的单调递增栈(即每次处理完当前数据后，都要将当前数据加入栈尾)。
> - 以下习题为例，即每次在栈顶新增当前元素前，都先从当前元素向前，逆序的删除所有比当前元素大的逆序数对，从保证当前数据插入后，维持一个以当前数据结尾的，离当前数据最近的单调递增栈。
- 模板：
> - 最常见的应用：给定一个整数序列，求某一个数左边/右边最近的且比这个数大/小大数是几。
> - 模板中为了节省时间，不再使用栈常规的方法empty,push,pop等，而是直接使用这些方法的数组实现。
- 习题[单调栈](https://www.acwing.com/problem/content/description/832/)

```
#include <iostream>

using namespace std;

const int N=1e5+10;
int stk[N], tt=0;

int main(){
    int n;
    cin >> n;
    while(n--){
        int x;
        cin >> x;
        //left side and little than x,so use >= not > to pop.
        while(tt && stk[tt]>=x)tt--;
        if(tt)cout << stk[tt] << ' ';
        else cout << "-1 ";
        
        stk[++tt]=x;
    }
    return 0;
}
```

## 单调队列
- 思想：
> - 利用队列的性质“先进先出”，以数组实现的队列中“先进先出”也叫“后进前出”。
> - 滑动窗口问题使用队列是因为：窗口滑动且维持单调队列时，需要同时在前后两端进行操作。(但是没有完全被“后进前出”的思想束缚，使用的是"前出后同时进出"的方法，滑动的同时也维持队列中需要对应的数据是单调的)。
> - 因此常用于求滑动窗口中的最大/最小值问题。
> - 以下习题为例，顺序遍历的同时，始终维护一个以当前数据结尾的单调队列(即每次处理完当前数据后，都要将当前数据加入栈尾)。递增队列用于求窗口中的最小值；递减队列用于求窗口中的最大值。
> - 以下习题找最小值为例，即每次在队尾新增当前元素前，都先从队尾向前，顺序的删除所有大于等于当前元素的逆序数对，从保证当前数据插入后，维持一个以当前数据结尾的，离当前数据最近的单调递增队列，那么滑窗中的第1个值，就是此滑窗中的最小值。
- 模板：
> - 常用于求滑动窗口中的最大/最小值问题。
- 习题[滑动窗口](https://www.acwing.com/problem/content/156/)

```
#include <iostream>

using namespace std;

const int N=1e6+10;
int a[N];
int q[N], hh=0, tt=-1;

int main(){
    int n,k;
    cin >> n >> k;
    for(int i=0; i<n; i++)cin>>a[i];
    for(int i=0; i<n; i++){
        //move slide window for one step every cycle.
        if(tt>=hh && i-k+1>q[hh])hh++;
        while(tt>=hh && a[q[tt]]>=a[i])tt--;
        q[++tt]=i;
        if(i>=k-1)cout << a[q[hh]] << ' ';
    }
    puts("");
    
    hh=0; tt=-1;
    for(int i=0; i<n; i++){
        if(tt>=hh && i-k+1>q[hh])hh++;
        while(tt>=hh && a[q[tt]]<=a[i])tt--;
        q[++tt]=i;
        if(i>=k-1)cout << a[q[hh]] << ' ';
    }
    return 0;
}
```

# KMP字符串
- 在自动驾驶应用中不使用，先不管。

# Trie
- 思想：
> - 前缀树或字典树，空节点，用下标0表示。(数组的所有元素默认为0)
> - 所以只需要维护前缀树的结构关系，与每个节点上出现单词的个数。
- 模板：
> - 以字符串为例：所有输入的字符串总长度为N，那么就用数组开辟N个节点；同时每个节点有26个小写英文字母的可能，因此用二维数组son[N][26]表示，第一维序号N表示节点编号，第二维序号表示可能的字母序号。这样就把该树的所有可能性都表示出来了。
> - 只考虑第1维的情况下，就像使用一维数组表示链表一样，是通过序号表示指针表示连接关系的，并且也需要一个idx来表示下一个可用的节点。默认将所有单词的第1个字符都存储在第0号节点上；而之后的每个单词只要前缀不重复，那么每个字符都会单独占用一个节点；因此idx初始为0，除单词的第1个字符外，其他字符如果需要建立节点，就需要先++idx。
> - 此外还需要一个数组cnt[N]来记录每个节点作为一个单词结尾的次数。(为了编程方便，cnt可以与son的结尾字符的编号相同，也可以固定错位1个。)
> - 综上所述：需要3个属性son[N][26], cnt[N], idx；两个操作insert(char str[]), query(char str[])。
> - 注：遍历一个char str[]表示字符串的方法for(int i=0; str[i]; i++)。

```
const int N=1e5+10;
int sun[N][26], cnt[N], idx=0;

void insert(char s[]){
    int p=0;  //start from root node every time.
    for(int i=0; s[i]; i++){
        int u=s[i]-'a';
        if(!sun[p][u])sun[p][u]=++idx;
        p=sun[p][u];
    }
    cnt[p]++; //num in cnt point to end+1 of s.
}

int query(char s[]){
    int p=0;
    for(int i=0; s[i]; i++){
        int u=s[i] - 'a';
        if(!sun[p][u])return 0;
        p=sun[p][u];
    }
    return cnt[p];
}
```




References
----------

- <https://www.acwing.com/>
- <https://www.markdownguide.org/>
