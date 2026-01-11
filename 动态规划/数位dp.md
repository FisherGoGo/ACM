---
title: 数位dp
date: 2024-11-22
tags:
  - dp
  - algorithm
---
# 内容
- **数位DP**就是一些形如：*在区间 $L-R$ 中有多少满足某条件的数* 之类的问题
- 如果我们对每一个数进行检验，在题目范围太大时，明显时会超时的，那怎么办
- 这时候我们可以运用递归的记忆化搜索的思想，去一位一位处理，大大降低复杂度

# 实现
- 我们主要用递归函数去实现，如下
``` cpp
int dp[pos][];
int num[];

int dfs(int pos,int last/...,int flag,int lead){ 
	int ans=0;
	if(pos==0) return 1; //递归终点
	if(!lead&&!flag&&dp[pos][]!=-1) return dp[pos][]; //记忆化
	int max_num=flag?num[pos]:9;
	for(int i=0;i<=max_num;i++){
		if(...) continue;    //不满足题目
		ans+=dfs(pos-1,last/...,flag&&i==num[pos],i==0&&lead);
	}
	if(!lead&&!flag) dp[pos][]=ans;
	return ans;
}
```
- 来解释下代码的一些小点
	- $last/...$ 用于记录有关题意的信息，比如有时候要求相邻数是否满足某条件，就要求记录此消息，有时会用来记录其他信息
	- $pos$ 用于记录当前数位位置
	- $flag$ 用于记录这一位枚举是否受范围限制
	- $lead$ 用于记录是否是有前导零


# 例题
- [洛谷 P2602](https://www.luogu.com.cn/problem/P2602)
- [洛谷 P4999](https://www.luogu.com.cn/problem/P4999)
- [洛谷 P2657](https://www.luogu.com.cn/problem/P2657)
