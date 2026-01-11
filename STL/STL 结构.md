---
date: 2024-11-20
title: STL 结构
tags:
  - stl
---

# 队列 **queue**

## 内容
- 队列是一种前出后进的C++自带的结构
- 每个元素从队尾进来，并且从队首出去

## 函数

- 要使用队列必须调用**queue**函数库，下方代码创建了一个名为**q**数据类型为整形的队列
```cpp
#include<queue>
std::queue <int> q;

q.empty() //检验队列是否为空
q.push(a) //向队尾压入一个元素
q.pop() //弹出队首元素
q.front //返回队首元素
q.back() //返回队尾元素
q.size() //返回队列元素个数
```

## 优先队列
- **优先队列**是也是一种根队列一样的结构，但是他会自动将队列内元素按降序排序
- 下方代码创建一个名为**q**的优先队列
```cpp
priority_queue <int> q;
```
- 其他用法与队列相同
>你也可以自己决定优先队列的排序方式，使用重载运算符的方法，下方就提供一个让其按照升序的方式排列
```cpp
struct compare{
	bool operator()(int a,int b){
		return a>b;
	}
};

std::priority_queue <int,compare> q; 
```

## 应用
- 有时候可以在 *滑动窗口* ，*BFS* 中用到队列，并且 *优先队列* 也十分好用

# 栈 **stack**

## 内容
- 栈是一种先进先出的数据结构
- 先进来的元素被压入栈底，后进来的元素在栈顶

## 函数
- 下方创建一个栈
```cpp
#include<stack>
std::stack <int> s;

s.push() //压入元素
s.top() //返回栈顶元素
s.pop() //弹出栈顶元素
s.empty() //检查栈是否空
s.size() //返回栈的元素个数
```

## 应用
- 单调栈


# **Set**

## 内容
- **Set** 是一个关联容器，它存储了一组唯一的元素，并按照一定的顺序进行排序
- 它提供了高效的元素查找、插入和删除操作。它是基于红黑树实现的，因此具有对数时间复杂度的查找、插入和删除性能
- 其存的元素必须是可以被比较大小的，可以被复制和赋值的

## 函数
```cpp
#include<set>
std::set <int> s;

s.begin() //返回第一个元素
s.end() //返回末尾
--s.end() //可以用这个返回最后一个元素

s.insert(a) //插入元素
s.earse(a) //清除一个元素

s.find(a) //查找元素是否存在
if(s.find(a)!=s.end()) printf("a is in the set");

s.sizz() //返回元素数量
s.empty() //返回是否为空

//遍历元素
for(int num:s){
	std::cout<<num<<" "; 
}
```

>注意：**Set** 只能存储唯一元素，无法存储多个相同元素，如果要存储多个元素，使用 **multiset**


# **Map**

## 内容
- **Map** 提供了一种关联容器，用于存储键值对，每个键是唯一的，只能出现一次
- 容器中的元素是按照键的顺序自动排序的，这使得它非常适合需要快速查找和有序数据的场景
- 元素按照键的顺序自动排序，通常是升序

## 函数
```cpp
#include<map>
using namespace std;
map <key_type,value_type> m;

m[key]=value; //插入元素
value=m[key]; //访问元素

//遍历
for(map<key_type,value_type>::iterator it=m.begin();it!=m.end();it++){
	cout<< it -> first <<" "<< it -> second <<endl;
}

if(m.find(key)!=m.end()) //检查键是否存在
m.erase(key) //删除元素
m.clear() //清空
m.size() //获取大小
```

>**Map** 和 **Set** 可以套用，感兴趣可以试试 [洛谷P3879](https://www.luogu.com.cn/problem/P3879)