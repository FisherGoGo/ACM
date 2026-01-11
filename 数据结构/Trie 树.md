---
date: 2025-03-05
tags:
  - DS
  - String
title: Trie树
---
# 主要内容

- **Trie树**又称字典树，是存字符串的数据结构
- 我们可以用它来检索一个字符串是否在**字典**中出现过

# Code
```cpp
int Trie[maxn][26];
int end[maxn*30];
int node;

void insert(string s){
	int len=s.length();
	int rt=0;
	for(int i=0;i<len;i++){
		int c=s[i]-'a';
		if(!Trie[rt][c]) Trie[rt][c]=++node;
		rt=Trie[rt][c];
	}
	end[rt]=1;
}

bool find(string s){
	int len=s.length();
	int rt=0;
	for(int i=0;i<len;i++){
		int c=s[i]-'a';
		if(!Trie[rt][c]) return false;
		rt=Trie[rt][c];
	}
	if(end[rt]) return true;
	else return false;
}
```