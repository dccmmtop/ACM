---
title: Interesting Punch-Bowl
tags:
    - 优先队列
    - 搜索
categories: acm
date: 2016-10-9
---


## [题目连接](http://acm.nyist.net/JudgeOnline/problem.php?pid=547)
### 题意：
有一个正方形的区域 区域上面有高度不同的1*1的立方体自然有凸有凹
凹的地方可以储水
问一共可以储存多少水

### 分析：
每次都从最低的边界向内部注水（仅与边界相邻的内部）
如果能够注水 就把注水后的柱子当作新的边界
也就是把边界放到一个优先队列中 每次都让高度最小的边界出队(优先队列)


```c++
#include<stdio.h>
#include<string.h>
using namespace std;
int n,m,sum=0;
int bowl[350][350];
int vis[350][350];
int dir[4][2]= {0,1,1,0,0,-1,-1,0};
struct node
{
int x;
int y;
int h;
friend bool operator <(node a,node b)
{
return a.h>b.h;
}
};
priority_queueq;
void chuShiHua()///首先把边界放入队列中
{
memset(vis,0,sizeof(vis));
node b;
for(int i=1; i<=m; i++)
{
b.x=i;
b.y=1;
b.h=bowl[i][1];
vis[i][1]=1;
q.push(b);
b.x=i;
b.y=n;
b.h=bowl[i][n];
vis[i][n]=1;
q.push(b);
}
for(int i=2; i<=n-1; i++)
{
b.x=1;
b.y=i;
b.h=bowl[1][i];
vis[1][i]=1;
q.push(b);
b.x=m;
b.y=i;
b.h=bowl[m][i];
vis[m][i]=1;
q.push(b);
}
}

void bfs()
{
chuShiHua();
while(!q.empty())
{
node a,b;
a=q.top();

    q.pop();
    for(int i=0; i<4; i++)
    {
        int x=a.x+dir[i][0];
        int y=a.y+dir[i][1];
        if(x<1||x>m||y<1||y>n)
            continue;

        if(vis[x][y]==0)
        {
            if(bowl[x][y]<a.h)///可以向内部注水
            {

                sum+=a.h-bowl[x][y];
                b.x=x;
                b.y=y;
                b.h=a.h;
                vis[x][y]=1;
                q.push(b);
            }
            else///不可以
            {
                b.x=x;
                b.y=y;
                b.h=bowl[x][y];
                vis[x][y]=1;
                q.push(b);

            }
        }
    }

}
}

int main()
{
while(~scanf("%d%d",&n,&m))
{

    memset(bowl,0,sizeof(bowl));
    for(int i=1; i<=m; i++)
        for(int j=1; j<=n; j++)
            scanf("%d",&bowl[i][j]);
    sum=0;
    bfs();
    printf("%d\n",sum);
}
return 0;
}
```
