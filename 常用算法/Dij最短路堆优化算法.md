相当于一个贪心。每次从最短距离开始找，看其他点能不能通过这个最短距离而缩小。spfa可能会被卡，推荐dij
```cpp
#include <iostream>
#include <algorithm>
#include <set>
#include <queue>
#include <cstdio>
#include <vector>
#include <map>
#include <stack>
#include <cmath>
using namespace std;
#define For(i, a, b) for(int i = (a), _ = (b); i <= _; i++)
#define IO ios::sync_with_stdio(false);cin.tie(0);cout.tie(0)
#define maxn 200000
#define inf 0x7fffffff

struct node {
	int next, to, dis;
}e[maxn << 1];
int tot, first[maxn], dis[maxn], vis[maxn];
int n, m, t;

struct edge {
	int dis, pos;
	bool operator < (const edge& x) const {
		return x.dis < dis;
	}
};

void add (int x, int y, int d) {
	tot++;
	e[tot].to = y;
	e[tot].dis = d;
	e[tot].next = first[x];
	first[x] = tot;
}

priority_queue<edge> q;
void dijkstra() {
	dis[t] = 0; q.push(edge {0, t});
	while (!q.empty()) {
		edge tmp = q.top(); q.pop();
		if (vis[tmp.pos]) continue;
		vis[tmp.pos]++;
		for (int i = first[tmp.pos]; i; i = e[i].next) {
			if (dis[e[i].to] > dis[tmp.pos] + e[i].dis) {
				dis[e[i].to] = dis[tmp.pos] + e[i].dis;
				if (!vis[e[i].to]) q.push(edge {dis[e[i].to], e[i].to});
			}
		}
	}
}

void init() {
	For(i, 1, n) {
		dis[i] = inf;
	}
}

int main(void) {
	IO;
	cin >> n >> m >> t;
	init();
	For(i, 1, m) {
		int a, b, c;
		cin >> a >> b >> c;
		add(a, b, c);
	}
	dijkstra();
	For (i, 1, n) cout << dis[i] << " ";
    return 0;
}
```
