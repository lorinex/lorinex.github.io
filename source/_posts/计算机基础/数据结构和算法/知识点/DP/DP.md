---
title: DP
categories:
  - 计算机基础
  - 数据结构和算法
  - 知识点
  - DP
date: 2023-09-13 23:21:52
tags:
---
# DP

## DP基本思想：

如果各个子问题不是独立的（即：重复的），不同的子问题的个数只是多项式量级（即：有限的），如果我们能够保存已经解决的子问题的答案（一般用数组），而在需要的时候再找出已求得的答案，这样就可以避免大量的重复计算。

概念

最优子结构：一个大问题的最优解，必定包含小问题的最优解（数塔）

重叠子问题：保存一些数据，可以避免重复计算

无后效性



## 经典问题：数塔

![](知识点/DP/image/image.png "")

- 例题：免费馅饼
	**Problem Description**
	都说天上不会掉馅饼，但有一天gameboy正走在回家的小径上，忽然天上掉下大把大把的馅饼。说来gameboy的人品实在是太好了，这馅饼别处都不掉，就掉落在他身旁的10米范围内。馅饼如果掉在了地上当然就不能吃了，所以gameboy马上卸下身上的背包去接。但由于小径两侧都不能站人，所以他只能在小径上接。由于gameboy平时老呆在房间里玩游戏，虽然在游戏中是个身手敏捷的高手，但在现实中运动神经特别迟钝，每秒种只有在移动不超过一米的范围内接住坠落的馅饼。现在给这条小径如图标上坐标：  
	
	![](https://webvpn.bupt.edu.cn/https/77726476706e69737468656265737421f9fa46d1253c6757300b9aa8965c2e32a5060e/img_convert/f5bf3c42a4c0d580409572acae853c1e.png "")
	为了使问题简化，假设在接下来的一段时间里，馅饼都掉落在0-10这11个位置。开始时gameboy站在5这个位置，因此在第一秒，他只能接到4,5,6这三个位置中其中一个位置上的馅饼。问gameboy最多可能接到多少个馅饼？（假设他的背包可以容纳无穷多个馅饼）
	Input  
	输入数据有多组。每组数据的第一行为以正整数n(0<n<100000)，表示有n个馅饼掉在这条小径上。在结下来的n行中，每行有两个整数x,T(0<T<100000),表示在第T秒有一个馅饼掉在x点上。同一秒钟在同一点上可能掉下多个馅饼。n=0时输入结束。
	Output  
	每一组输入数据对应一行输出。输出一个整数m，表示gameboy最多可能接到m个馅饼。  
	提示：本题的输入数据量比较大，建议用scanf读入，用cin可能会超时。
	样例
	```C++
	input
	6
	5 1
	4 1
	6 1
	7 2
	7 2
	8 3
	0
	output
	4
	```
	
	思路
	类似于数塔
	找出输入的最大时间，从最大时间下开始向前找能接到的最多的馅饼数。最后输出时间为0，位置为5的馅饼数。
	状态转移方程：dp[i][j]=max(dp[i+1][j+1],max(dp[i+1][j],dp[i+1][j-1]))
	实现过程：
	该题样例可以建立成一个二维矩阵，如图所示
	![](Screenshot_20220124_223931.jpg "")
	处理所有的数据，最后取a[0][5]就行
	ac代码
	```C++
	#include<bits/stdc++.h>
	#define ll long long
	#define ms(a) memset(a,0,sizeof(a))
	#define pi acos(-1.0)
	#define INF 0x3f3f3f3f
	const double E=exp(1);
	const int maxn=1e5+10;
	using namespace std;
	int dp[maxn][20];
	int main(int argc, char const *argv[])
	{
	  // 状态转移方程：dp[i]=max(dp[i+1][j+1],max(dp[i+1][j-1],dp[i+1][j]))
	  int n;
	  while(~scanf("%d",&n)&&n)
	  {
	    ms(dp);
	    int res=0;
	    int place,time;
	    for(int i=0;i<n;i++)
	    {
	      scanf("%d%d",&place,&time);
	      dp[time][place]++;
	      res=max(res,time);
	    }
	    for(int i=res;i>=0;i--)
	    {
	      for(int j=0;j<11;j++)
	      {
	        dp[i][j]+=max(dp[i+1][j-1],max(dp[i+1][j+1],dp[i+1][j]));
	      }
	    }  
	    printf("%d\n",dp[0][5]);
	  }
	  return 0;
	}
	```
	



## 经典问题：最长有序子序列

![](知识点/DP/image/image_1.png "")

f[i]:从第二个开始往前找比自身小的数，选择比自身小的数的f[i]的最大值，然后加一成为自己的f[i]

- 最少拦截系统
	题目
	某国为了防御敌国的导弹袭击,发展出一种导弹拦截系统.但是这种导弹拦截系统有一个缺陷:虽然它的第一发炮弹能够到达任意的高度,但是以后每一发炮弹都不能超过前一发的高度.某天,雷达捕捉到敌国的导弹来袭.由于该系统还在试用阶段,所以只有一套系统,因此有可能不能拦截所有的导弹.  
	怎么办呢?多搞几套系统呗!你说说倒蛮容易,成本呢?成本是个大问题啊.所以俺就到这里来求救了,请帮助计算一下最少需要多少套拦截系统.
	分析
	Dilworth定理：对于一个偏序集，最少链划分数=最长反链长度
	即求最长升序子序列长度
	```C++
	#include<bits/stdc++.h>
	using namespace std;
	
	int main(){
	  int n,i,j,a[5000],b[5000],max;
	  while(scanf("%d",&n)==1){
	    memset(b,0,sizeof(b));
	    for(i=0;i<n;i++){
	      scanf("%d",&a[i]);
	    }
	    b[0]=1;max=1;
	    for(i=1;i<n;i++){
	      for(j=0;j<i;j++){
	        if(a[j]<a[i] && b[j]>b[i]){
	          b[i]=b[j];
	        }
	      }
	      b[i]++;
	      if(b[i]>max)  max=b[i];
	    }
	    printf("%d\n",max);
	  }
	  return 0;
	} 
	```
	
	



![](知识点/DP/image/image_2.png "")



