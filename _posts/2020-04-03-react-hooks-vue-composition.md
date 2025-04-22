---
layout: post
title:  "C++ STL 常用技巧笔记（二）"
date:   2025-04-16 10:00:00 +0800
categories: C++ STL
subtitle: "Thinking in React vs. Thinking in Vue"
author: "xr_日计划"
header-style: text
tags:
  - vector
  - priority_queue
  - deque
  - C++ STL
  - set
  - list
---


**这是一篇整理自我平时学习 C++ STL 使用技巧的笔记（二），包含了 `deque`、`vector`、`list`、`set` 以及 `priority_queue` 的常见操作和竞赛应用。**

# 1. `deque` 的 `resize(num, elem)` 机制

- 用法：`dq.resize(n, val);`  
- 作用：将 `deque` 的大小改为 `n`：
  - 若 `n > 当前大小`，会在尾部补上 `val`;
  - 若 `n < 当前大小`，会截断多余元素。

示例代码：
```cpp
deque<int> dq{1, 2};
dq.resize(5, 7); // 变为 {1, 2, 7, 7, 7}
```
e2. vector 的 distance(it1, it2) 与 list 的 splice(pos, lst)
distance(it1, it2)：计算两个迭代器间的距离（支持所有 STL 容器）
-------------------------------------------

```ts
vector<int> v{1, 2, 3, 4};
int d = distance(v.begin(), v.end()); // d = 4
```
list::splice(pos, lst)：将另一个 list 的内容插入到指定位置（O(1) 操作）
```ts
list<int> a{1, 2}, b{3, 4};
a.splice(a.end(), b); // a = {1, 2, 3, 4}, b 为空
```

3. priority_queue 的 STL 用法与竞赛场景
最大堆（默认）：
```cpp
priority_queue<int> maxq;
```
最小堆：
```cpp
priority_queue<int, vector<int>, greater<int>> minq;
```
自定义结构：
需重载 < 运算符或传入 lambda 比较器，常用于 Dijkstra 算法等。

应用场景：
Top-K 问题

图最短路径（堆优化 Dijkstra）

贪心策略动态选最大/最小值

4. set 的基本用法与二分操作
自动去重且有序；
常见操作：
```cpp
set<int> s;
s.insert(3);
s.erase(3);
auto it = s.find(3);
```
二分查找：

```cpp
s.lower_bound(x); // >= x 的最小值
s.upper_bound(x); // > x 的最小值
```
应用场景：
实时维护有序数据

查找某值附近的最近值

多个区间重叠判断

动态维护中位数

5. vector 与 list 在火车排序模拟中的对比

容器类型	        优点	             缺点	                  适用场景
vector	  随机访问快，插入末尾快	  中间插入慢	  索引频繁、长度稳定的场景
list	       中间插入/删除快	    随机访问慢	   模拟链式结构、频繁插入删除的模拟问题

这篇笔记为 STL 的实战用法提供了一些核心思路，尤其在刷算法题时能迅速定位合适容器。
后续还会继续整理笔记 3（如 map、unordered_map、堆优化 BFS 等），欢迎交流！
-------------------------------------------
