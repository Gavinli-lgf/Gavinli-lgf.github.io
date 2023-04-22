---
title: "02-图的存储"
subtitle: "链式前向星存图法"
layout: post
author: "rainchxy"
header-style: text
hidden: true
tags:
  - 图
  - 数据结构
---
图的存储方法很多，最常见的除了邻接矩阵、邻接表和边集数组外，还有链式前向星。链式前向星是一种静态链表存储，用边集数组和邻接表相结合，可以快速访问一个顶点的所有邻接点，在算法竞赛中广泛应用。

链式前向星存储包括两种结构：

  1. 边集数组：edge[ ]，edge[i]表示第i条边；
  2. 头结点数组：head[ ]，head[i]存以i为起点的第一条边的下标(在edge[]中的下标)

```cpp
struct node{
    int to,next,w;
}edge[maxe];//边集数组，边数一般要设置比maxn*maxn大的数，如果题目有要求除外

int head[maxn];//头结点数组
```
![](/img/leetbook-graph/chained-forward-star-1.png)
![](/img/leetbook-graph/chained-forward-star-2.png)
![](/img/leetbook-graph/chained-forward-star-3.png)

添加一条边u v w的代码如下：
```cpp
void add(int u,int v,int w){//添加一条边
    edge[cnt].to=v;
    edge[cnt].w=w;
    edge[cnt].next=head[u];
    head[u]=cnt++;
}
```
如果是有向图，每输入一条边，执行一次add(u,v,w)即可；如果是无向图，则需要执行两次add(u,v,w); add(v,u,w)。

如何使用链式前向星访问一个结点u的所有邻接点呢？
```cpp
for(int i=head[u];i!=-1;i=edge[i].next){
    int v=edge[i].to; //u的邻接点
    int w=edge[i].w; //u—v的权值
       …
}
```
![](/img/leetbook-graph/chained-forward-star-4.png)

总代码：

```cpp
#include<iostream>//创建无向网的链式前向星 
#include<cstring>
using namespace std;
const int maxn=100000+5;
int maxx[maxn],head[maxn];
int n,m,x,y,w,cnt;

struct Edge{
	int to,w,next;
}e[maxn];

void add(int u,int v,int w){//添加一条边u--v 
 	e[cnt].to=v;
 	e[cnt].w=w;
	e[cnt].next=head[u];
	head[u]=cnt++;
} 

void printg(){//输出链式前向星
	cout<<"----------链式前向星如下：----------"<<endl;
	for(int v=1;v<=n;v++){
		cout<<v<<"：  ";
		for(int i=head[v];~i;i=e[i].next){
			int v1=e[i].to,w1=e[i].w;
			cout<<"["<<v1<<" "<<w1<<"]\t";
		}
		cout<<endl;
	}
}

int main(){
	cin>>n>>m;
	memset(head,-1,sizeof(head));
	cnt=0;
	for(int i=1;i<=m;i++){
		cin>>x>>y>>w;
		add(x,y,w);//添加边
		add(y,x,w);//添加反向边 
	}
	printg();
	return 0;
}
/*输入样例
4 5
1 2 5
1 4 3
2 3 8
2 4 12
3 4 9
*/
```


References
----------

- <https://cloud.tencent.com/developer/article/1912251>
