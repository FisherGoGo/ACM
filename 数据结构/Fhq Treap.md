---
date: 2024-11-26
tags:
  - DS
title: Fhq Treap
---
# 内容
- 强推教学视频[【AgOHの数据结构】平衡树专题之贰 fhq Treap（无旋Treap）](https://www.bilibili.com/video/BV1ft411E7JW?spm_id_from=333.788.recommend_more_video.5&vd_source=f0c9904d89ec29a147dec0f6756bca3f)
- **fhq treap**是一种由范浩强大神提出的数据结构
- 它不同于普通平衡树，它简单易懂，支持操作多，代码简单~~大部分都是复读机~~，并且不用旋转操作就可以维护二叉搜索树的平衡
- **Treap**是**Tree+Heap**，既二叉搜索树和二叉堆的集合，**Treap**中存储值和索引，使值满足二叉搜索树的性质，索引满足二叉堆的性质来使其平衡
- **Treap**之所以可以平衡，是因为它的索引是随机化的，因为如果$1,2,3,4,5,6...$等值按顺序插入，肯定会影响平衡，但如果打乱他们的顺序呢？让他们以$2,4,1,5,6,3..$的随机顺序插入，那么就可以不那么影响平衡了；而二叉堆维护随机索引，相当于把插入随机化，使插入的数不知道到哪里了去，相当于次序随机了
>关于二叉堆，你只要知道它就像优先队列一样，父节点的优先级一定大于其所有子节点的优先级
- 而**Fhq Treap**维护树平衡的随机操作就是：**分裂**与**合并** ，就像拼图一样

# 代码
## 节点定义
- 我们需要保存：
	- 左右子树编号
	- 值
	- 索引
	- 子树大小

```cpp
struct Tree{
	int l,r;
    int val,key;
    int size;
}fhq[100005];
int cnt,root;

#include<random>
std::mt19937 rnd(233); //随机数

int newnode(int val){
	fhq[++cnt].val=val;
	fhq[cnt].key=rnd();
	fhq[cnt].size=1;
	return cnt;
}
void update(int now){
	fhq[now].size=fhq[fhq[now].l].size+fhq[fhq[now].r].size+1;
	return ;
}
```

## 分裂
- 一般有按值分裂和按大小分裂 
- 按值分裂：把一颗树分裂成两颗，其中一颗树全部小于等于给定的值，另外一颗全部大于给定的值
- 按大小分裂：把树拆成两棵树，其中一棵树的大小等于给定的大小，剩余部分在另外一颗树里
- 一般来说，我们用按值分裂；如果我们维护区间信息，我们按大小分裂

```cpp
void split(int now,int val,int &x,int &y){
	if(!now) x=y=0;
	else{
		if(fhq[now].val<=val){
			x=now;
			split(fhq[now].r,val,fhq[now].r,y);
		}
		else {
			y=now;
			split(fhq[now].l,val,x,fhq[now].l);
		}
		update(now);
	}
	return ;
}
```

## 合并
- 把**x**和**y**合并成一颗树，其中**x**树上的值都小于**y**树上的值，合并后的树也满足**Treao**的条件
	```cpp
	int merge(int x,int y){
	    if(!x||!y) return x+y;
	    if(fhq[x].key>fhq[y].key){ //这里填 > >= < <= 都可以
	        fhq[x].r=merge(fhq[x].r,y);
	        update(x);
	        return x;
	    }
	    else {
	        fhq[y].l=merge(x,fhq[y].l);
	        update(y);
	        return y;
	    }
	}
	```

## 插入
- 假如插入的值为$val$，我们把树按$val$分裂成$x$和$y$，然后合并新节点和$x$，再合并新的$x$和$y$，是不是很简单
- 因为按值分裂，所以$x$上的值都小于等于$val$，$y$上的值都大于$val$，因此这么合并没问题
```cpp
void insert(int val){
	int x,y,z;
	split(root,val,x,y);
	root=merge(merge(x,newnode(val)),y);
	return ;
}
```

## 删除
- 比如我们要删除$val$
- 我们先把树按$val$分裂成$x$和$z$，再把$x$按$val-1$分裂成$x$和$y$，此时$y$树上所有的值都等于$val$
- 我们去掉$y$的根节点，可以通过合并$y$的左右子树来实现
- 最后合并$x,y,z$
```cpp
void del(int val){
	split(root,val,x,z);
	split(x,val-1,x,y);
	y=merge(fhq[y].l,fhq[y].r);
	root=merge(merge(x,y),z);
	return ;
}
```

## 查询值的排名
- 加入查询的值为$val$，把树按$val-1$分裂成$x$和$y$，那么$x$的大小加一就是$val$的排名
- 最后再合并回去
```cpp
void rank(int val){
	split(root,val-1,x,y);
	cout<<fhq[x].size+1;
	root=merge(x,y);
	return ;
}
```

## 查询第K大的数
- 类似普通二叉树的查询方法
```cpp
void getnum(int rank){
    int now=root;
    while(now){
        if(fhq[fhq[now].l].size+1==rank) break;
        else if(fhq[fhq[now].l].size>=rank) now=fhq[now].l;
        else {
            rank-=fhq[fhq[now].l].size+1;
            now=fhq[now].r;
        }
    }
    cout<<fhq[now].val<<endl;
    return ;
}
```

## 查询前驱/后驱
- 前驱：按照$val-1$分成$x$和$y$，$x$最右侧端点的数就是它的前驱
- 后驱：按照$val$分成$x$和$y$，$y$最左侧端点的数就是它的后驱
```cpp
void pre(int val){
    split(root,val-1,x,y);
    int now=x;
    while(fhq[now].r){
        now=fhq[now].r;
    }
    cout<<fhq[now].val<<endl;
    root=merge(x,y);
    return ;
}

void nxt(int val){
    split(root,val,x,y);
    int now=y;
    while(fhq[now].l) now=fhq[now].l;
    cout<<fhq[now].val<<endl;
    root=merge(x,y);
    return ;
}
```

# **Fhq Treap**的区间操作
