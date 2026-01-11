---
date: 2024-11-20
title: 深度优先搜索
tags:
  - algorithm
---
# DFS
## 内容

- 顾名思义，DFS就是优先深度搜索，先把一种情况搜到底，然后再搜索其他情况
- 用树来表示，就是从一个点开始，往下搜到底，然后往上返回到第一个分支，再开始搜到底，直到全部搜完或者求得答案。~~由于本人还不会插入图片，因此图片省去~~
>我们可以设置一个**vis**数组来判断节点是否被搜索过，以防不必要的重复搜索

## 实现

- 可以用函数进行递推，如下
```
void dfs(int depth){          \\depth为当前搜索深度
	for(---){
		if(vis[i]==0){
			vis[i]=1;         \\标记为搜索过
			dfs(depth+1);     \\继续搜
			vis[i]=0;         \\别忘了搜完将其状态回归
		}
	}
	if(depth==N){             \\搜完了
		---
		return ;
	} 
	return ;
}
```


## 运用

- 最经典的应用，就是用**DFS**求全排列
	- 例题：[洛谷 P1157](https://www.luogu.com.cn/problem/P1157)
	- 题目：给定**n**和**r**，求出$1 - n$ 共**n**个数中挑取**r**个数的组合
	- 尽管题目是求组合，但是和求排列差不多，不过我们不去搜索那些已经求出的组合，代码如下
	``` cpp
	#include<iostream>
	#include<iomanip>
	using namespace std;
	
	int n,r;
	int vis[30]; //是否用过
	int ans[40]; //存储答案
	
	void dfs(int now,int depth){
	    ans[depth]=now;
	    vis[now]=1;
	    for(int i=now;i<=n;i++){ //i=now 就不会重复求
	        if(vis[i]==0) dfs(i,depth+1);
	    }
	    vis[now]=0;
	    if(depth==r){ //搜完了
	        for(int i=1;i<=r;i++){
	            cout<<setw(3)<<ans[i];
	        }
	        cout<<endl;
	        return ;
	    }
	    return ;
	}
	
	
	int main(){
	    cin>>n>>r;
	    for(int i=1;i<=n;i++){
	        dfs(i,1);
	    }
	}
	```

- 当然，还有经典的**八皇后**问题
	- 例题：[洛谷 P1219](https://www.luogu.com.cn/problem/P1219)
	- 题目：$n \times n$ 的棋盘，放置 $n$ 个棋子，要求每行每排每斜行都只有一个棋子
	- 这道题也很简单，我们每放置一个棋子，就标记其所在的行，列，斜行都标记上，然后找合法的位置放棋子
	- 代码：
	```cpp
	#include<iostream>
	using namespace std;
	
	int vis[20]; \\行
	int vis_a[50]; \\左上到右下斜
	int vis_b[50]; \\右上到左下斜
	int ans[20];
	int Ans;
	int n;
	int Cnt=1;
	
	void dfs(int pos,int cnt){
	    vis[pos]=1; \\标记
	    vis_a[n+pos-cnt]=1;
	    vis_b[cnt+pos]=1;
	    ans[cnt]=pos;
	    for(int i=1;i<=n;i++){
	        if(vis[i]==0&&vis_a[n+i-cnt-1]==0&&vis_b[cnt+1+i]==0) dfs(i,cnt+1); \\找到合法位置
	    }
	    if(cnt==n){
	        if(Cnt<=3) {
	            for(int i=1;i<=n;i++) cout<<ans[i]<<" ";
	            cout<<endl;
	            Cnt++;
	        }
	        Ans++;
	        vis_a[n+pos-cnt]=0;
	        vis_b[cnt+pos]=0;
	        vis[pos]=0;
	        return ;
	    }
	    vis_a[n+pos-cnt]=0; \\重置
	    vis_b[cnt+pos]=0;
	    vis[pos]=0;
	    return ;
	}
	
	
	
	int main(){
	    cin>>n;
	    for(int i=1;i<=n;i++){
	        dfs(i,1);
	    }
	    cout<<Ans;
	    return 0;
	}
	```



# BFS

## 内容
- 我们先搜完当前能到达的所有情况，再往下搜
- 用树表示表示，就是先搜完当前同一层的所有节点，再往下一层一层地搜

## 实现
- 知道了原理，但是和**DFS**不太一样呢，那我们该怎么办
- 我们知道C++有一种叫队列的结构，先进来的先出，后进来的后出，对应**BFS**先进来的节点处理完再处理后边的节点，因此我们可以使用队列 ~~如果不知道队列可以看博客的**STL结构**文章~~
- 代码如下
``` cpp
queue <int> q;
int vis[10005];

void bfs(int firts){
	q.push(first);
	while(!q.empty()){
		int now=q.front();
		q.pop();
		for(---){
			if(!vis[i]){
				q.push(i);
				vis[i]=1;
			}
		}
	}
}
```

## 运用
- [马的遍历](https://www.luogu.com.cn/problem/P1443)
	- 题目：在一个 $n \times n$ 的棋盘上，一个马从 $(x,y)$ 出发问抵达棋盘每一点最少要几步
	- 此时我们可以用广度优先搜索来做，因为题目要求求最少步数，广度优先搜索可以保证我们到达此点时用的步数最少
	- 代码：
	```cpp
	#include<iostream>
	#include<queue>
	using namespace std;
	
	int n,m;
	int x,y;
	
	struct node{
	    int a;
	    int b;
	};
	queue <node> q;
	
	int squ[500][500];
	int dx[10]={0,2,2,-2,-2,1,1,-1,-1};
	int dy[10]={0,1,-1,1,-1,2,-2,2,-2};
	
	
	int main(){
	    cin>>n>>m>>x>>y;
	    for(int i=1;i<=n;i++){
	        for(int j=1;j<=m;j++){
	            squ[i][j]=-1;
	        }
	    }
	
	    squ[x][y]=0;
	    node A;
	    A.a=x;A.b=y;
	    q.push(A);
	    while(!q.empty()){
	        node Now=q.front();
	        q.pop();
	        for(int i=1;i<=8;i++){
	            int nx=Now.a+dx[i],ny=Now.b+dy[i];
	            if(nx>0&&nx<=n&&ny>0&&ny<=m&&squ[nx][ny]<0){ \\马不越界
	                squ[nx][ny]=squ[Now.a][Now.b]+1;
	                node nxt;
	                nxt.a=nx;
	                nxt.b=ny;
	                q.push(nxt);
	            }
	        }   
	    }
	
	    for(int i=1;i<=n;i++){
	        for(int j=1;j<=m;j++){
	            cout<<squ[i][j]<<" ";
	        }
	        cout<<endl;
	    }
	}
	```
