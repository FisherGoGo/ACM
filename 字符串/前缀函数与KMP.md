---
date: 2025-03-05
tags:
  - String
  - algorithm
title: 前缀函数与KMP
---
# 前缀函数

- 我们给定一个长度为 $n$ 的字符串 $s$ ，它的前缀函数被定义为一个长度为 $n$ 的数组 $\pi$ 
- $\pi_i$ 的定义是 $s$ 的 $\left[0,1,2...i\right]$ 区间中一对相等的真前缀和真后缀的长度，如果有多对，就取最长的那一对，既 $s\left[0,...k\right] = s\left[i-k+1,...i\right]$      
- 如果不存在，$\pi_i = 0$ 
- 特别规定 $\pi_0=0$ 

## 计算

- 如果我们用朴素算法进行计算，复杂度会是 $O(n^3)$ ，所以我们需要优化

### 优化一

- 我们可以发现相邻的 $\pi_i$ 最多增加 $1$ ，所以我们只需要在 $s\left[0,...\pi_{i-1}+1\right]$ 中进行比对，最好的情况是一次就成功

### 优化二

- 在最好情况下 $s_{i+1}=s_{\pi_i}$ ，此时的 $\pi_{i+1}=\pi_i+1$ 
- 那如果失配了我们应该怎么办
- 我们希望寻找仅次于 $\pi_i$ 的第二长度 $j$ 使得 $s\left[0,...j-1\right]=s\left[i-j+1,...i\right]$ 成立，此时我们只需要比较 $s_{i+1}$ 和 $s_j$ 就行，如此反复，直到 $j=0$ ，此时 $\pi_{i+1}=0$ 
- 因为 $s\left[0,...\pi_i-1\right]=s\left[i-\pi_i+1,...i\right]$ ，对于第二长度 $j$ 就有
$$s\left[0,...j-1\right]=s\left[i-j+1,...i\right]=s\left[\pi_i-j,...\pi_i-1\right]$$
- 或者说，对于长度 $i$ 的第 $2$ 小满足的长度 $j$ 为 
$$j=\pi_{i-1}$$
>举个例子，对于字符串 $ababc...abab$ ，如果我们新加一个 $a$ ，此时 $\pi_i$ 为 $4$ ，但是 $s[\pi_i] \ne s[i+1]$ ，所以我们找第二长度 $j$ ，既 $\pi[\pi_i-1]$ ，$\pi_3$ ，$s[\pi_3+1]=s[i+1]$ ，配对成功，我们可以理解为对于已经配对完的 $abab$ ，如果下一个配对不成功，但是 $abab$ 的前缀函数为 $2$ ，说明我们可以直接往前跳过那些已经配对成功的字符串

## Code

- 了解完了原理，优化完后，代码十分简短，并且可以做到 $O(n)$ 在线处理

```cpp
void pre_init(string s){
	int len=s.length();
	for(int i=1;i<len;i++){
		int j=pi[i-1];
		while(j>0&&s[i]!=s[j]) j=pi[j-1];
		if(s[i]==s[j]) j++;
		pi[i]=j;
	}
}
```

# 应用

## KMP

- 求字符串 $t$ 在字符串 $s$ 中的出现位置，我们只需要用一个分隔符号 # 把 $t$ 和 $s$ 连接起来，然后处理一边新串的前缀函数，最后我们看新串那一部分的前缀函数中 $\pi_i=len_t$ 的位置，那既是串 $t$ 出现的右端点

```cpp
void KMP(string t,string s){
	string st=s+'#'+t;
	int len=st.length();
	for(int i=1;i<len;i++){
		int j=pi[i-1];
		while(j>0&&st[i]!=st[j]) j=pi[j-1];
		if(st[i]==st[j]) j++;
		pi[i]=j;
	}

	vector <int> pos;
	for(int i=t.length()+1;i<len;i++){
		if(pi[i]==t.length()) pos.push_back(i);
	}
}
```

## 统计每个前缀的出现次数

- 我们先来讨论在字符串 $s$ 中每一个前缀的出现次数
- 对于每一个右端点 $i$ ，有长度为 $\pi_i$ 的前缀，有 $\pi_{\pi_i-1}$ 的前缀，有 $\pi[\pi[\pi_i-1]-1]$ 的前缀，以此类推，直到长度变为 $0$ 

```cpp
void cal(string s){
	int  len=0;
	for(int i=0;i<len;i++) ans[pi[i]]++;
	for(int i=len-1;i>=0;i--) ans[pi[i-1]]+=ans[i];
	for(int i=0;i<=n;i++) ans[i]++;
}
```
