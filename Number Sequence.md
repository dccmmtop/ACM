---
title: Number Sequence
tags: [杭电,acm,KMP]
categories: acm
---

 [题目链接](http://acm.hdu.edu.cn/showproblem.php?pid=1711)

### 题意

有一个主串和一个子串，找出子串与主串（完全）相匹配的位置，如果有多个位置都可以把子串匹配完，那么就输出最小开始匹配的下标。

### 分析

[数字的KMP算法](http://blog.csdn.net/v_july_v/article/details/7041827#)

```c++
#include <cstdio>
const int maxn = 10000 + 10, maxm = 1000000 + 10;
int n, m;
int p[maxn], t[maxm], next[maxn]; //定义模式串，文本串，next数组
void getNext() //O(m)复杂度求Next数组
{
    int i = 0, j = 0, k = -1;
    next[0] = -1;
    while(j < m)
    {
        if(k == -1 || p[k] == p[j]) next[++j] = ++k;
        else k = next[k];
    } 
}
int kmp()
{
    int j = 0; //初始化模式串的前一个位置
    getNext(); //生成next数组
    for(int i = 0; i < n; i++) //遍历文本串
    {
        while(j && p[j] != t[i]) j = next[j];//持续走直到可以匹配
        if(p[j] == t[i]) j++; //匹配成功继续下一个位置（j先加1 此时j的位置所对应的值在i的位置所对应的后面）
        
        //if(j==m)说明子串已经匹配完了（子串的长度是m，m-1是子串的最后一个字符）
        if(j == m) return i-m+2; //找到后返回第一个匹配的位置,因为返回的是逻辑下标（下标从1开始所以是i-m+2，不信自己在纸上画画 （看下图 ））
    }
    return -1; //找不到返回-1
}
int main()
{
    int T;
    scanf("%d", &T);
    while(T--)
    {
        scanf("%d%d", &n, &m);
        for(int i = 0; i < n; i++) scanf("%d", t+i);
        for(int i = 0; i < m; i++) scanf("%d", p+i);
        printf("%d\n", kmp());
    }
    return 0;
}

```

**为什么是return i-m+2**

![为什么是return i-m+2](http://ogbkru1bq.bkt.clouddn.com/tttt.jpg)

