---
title: "ABC 214D"
date: 2021-08-15T11:42:52+08:00
draft: false
markup: mmark
tags: ["Atcoder", "Union-Set"]
categories: ["Atcoder"]
---
<!--more-->

# 題目
計算樹上任兩點最短路徑的最大權重之和。

# 解法
樹代表**沒有環**，每個邊至少是一組點對最短路徑的最大權重。<br>
由小到大枚舉邊，計算有多少點對被這個邊隔開，再乘上權重。
範例:
```
o       o
 \  w  /
  o ─ o
 /     \
o       o
```
答案加上 $$3\times 3\times w$$

這可以用並查集做到。

```c++
int parent[100001];
 
int findParent(int x) {
    return x == parent[x] ? x : parent[x] = findParent(parent[x]);
}
 
struct Edge {
    int u, v, w;
    bool operator<(Edge e) {
        return w < e.w;
    }
} edges[100000];
 
ll n_member[100001];
 
void solve() {
    int n;
    cin >> n;
    for(int i = 1; i < n; i++) {
        cin >> edges[i].u >> edges[i].v >> edges[i].w;
    }
    sort(edges + 1, edges + n);
    iota(parent + 1, parent + n + 1, 1);
    fill_n(n_member + 1, n, 1);
    ll result = 0;
    for(int i = 1, x, y; i < n; i++) {
        x = findParent(edges[i].u);
        y = findParent(edges[i].v);
        result += n_member[x] * n_member[y] * edges[i].w;
        parent[y] = x;
        n_member[x] += n_member[y];
    }
    cout << result;
}
```