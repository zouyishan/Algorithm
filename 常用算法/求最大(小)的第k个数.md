1 nlogn算法(有啥好说的 直接sort排完输出a[k]。。。)




------------

2 线性O(n)算法 类似与快排。但是不用全给排好，相当于一个二分找。

原理：从数组中选择一个基准。然后进行类似与快排的操作:
```cpp
while (i <= j) {
	while (x > a[i]) i++; while (x < a[j]) j--;
	if (i <= j) swap(a[i], a[j]), i++, j--;
}
```
然后我们就可以得到此时的i - 1就是基准数位于数组的第几位。

这时i - 1 左边全是小于基准的数，右边全是大于基准的数。这是我们将i - 1 和k进行比较，然后决定下一步该找那个区间，知道找到的数 i - 1 == k我么就返回此时的基准值，代码如下：

> 2021 1 11 更：增加了大顶堆的找第k大的实现，同时改了一下快排代码：
```cpp
//
// Created by 86131 on 2021/1/10.
//

#include <iostream>
#include <algorithm>
#include <time.h>
#include <cstdlib>
using namespace std;
#define maxn 60000000
int n, a[maxn], k, t, b[maxn];
int res;
inline int read(){
    int x = 0, f = 1;
    char ch = getchar();
    while(ch < '0' || ch > '9'){
        if (ch == '-')
            f = -1;
        ch = getchar();
    }
    while(ch >= '0' && ch <= '9'){
        x = (x<<1) + (x<<3) + (ch^48);
        ch = getchar();
    }
    return x * f;
}

int quicksort(int l, int r) {
    int mid = (l + r) >> 1;
    int x = a[mid];
    int i = l, j = r;
    while (i <= j) {
        while (a[i] < x) i++;
        while (a[j] > x) j--;
        if (i <= j) swap(a[i++], a[j--]);
    }
    if (k <= j) quicksort(l, j);
    else if (k >= i) quicksort(i, r);
    return a[k];
}

void siftup(int i) {
    int t, flag = 0;
    while (i * 2 <= k && !flag) {
        t = a[i] < a[i * 2] ? i * 2 : i;
        if (i * 2 + 1 <= k) t = a[t] < a[i * 2 + 1] ? i * 2 + 1 : t;
        if (t != i) swap(a[i], a[t]), i = t;
        else flag = 1;
    }
}

void constructheap() {
    for (int i = k / 2; i >= 1; i--) siftup(i);
}

int main(void) {
    n = read(); k = read();
    srand((unsigned)time(NULL));
    for (int i = 1; i <= n; i++) a[i] = rand(), b[i] = a[i];

    cout << quicksort(1, n) << endl;

    for (int i = 1; i <= n; i++) a[i] = b[i];
    constructheap();
    for (int i = k + 1; i <= n; i++) {
        if (a[1] > a[i]) {
            a[1] = a[i];
            siftup(1);
        }
    }
    cout << a[1] << endl;
    sort(b + 1, b + 1 + n);
    cout << b[k] << endl;
    return 0;
}
```
找最大最小只需要换一下 `x < a[i]`  和  `x > a[j]`  即可。


------------

3,当然还有个更简单的办法 STL大法好。STL里面有个东西叫做nth_element(), 专门为这个问题设计的吧。。。。只用这样:
```cpp
nth_element(a + 1, a + k, a + n + 1);
```
就可以求出第k小的数了。

我们再在这个函数上面填两笔 :
```cpp
bool myfunction(int i, int j) { return i > j; }

nth_element(a + 1, a + k, a + n + 1, myfunction);
```
这样就可以求出第k大的数了~~~

具体代码就是:
```cpp
#include <iostream>
#include <algorithm>
#include <cstring>
#include <iomanip>
#include <set>
#include <queue>
#include <functional>
#include <cstdio>
#include <vector>
#include <map>
#include <cctype>
#include <cmath>
using namespace std;
#define ll long long
#define gcd(x, y) __gcd(x, y)
#define For(i, a, b) for(int i = (a), _ = (b); i <= _; i++)
#define For_(i, a, b) for(int i = (a), _ = (b); i < _; i++)
#define Forn(i, a, b) for(int i = (a), _ = (b); i >= _; i--)
#define Forn_(i, a, b) for(int i = (a), _ = (b); i > _; i--)
#define For0(i, a) for(int _ = (a), i = _; ; i++)
#define For0_(i, a) for(int _ = (a), i = _; i; i--)
int min_ = 0XFFFFFFFF, max_ = 1 << 31;
template <typename T>
inline void read(T& x){
    x = 0; int f = 1; char ch = getchar();while(ch < '0' || ch > '9'){if (ch == '-') f = -1; ch = getchar();}
    while(ch >= '0' && ch <= '9'){x = (x << 1) + (x << 3) + (ch ^ 48); ch = getchar();}x *= f;
}
template <typename T>
inline void write(T x) {if (x < 0) {putchar('-'); x = -x;}if (x > 9) write(x / 10);putchar(x % 10 + '0');}
void write2() { putchar(10); } void write1() { putchar(32); }
const int dx[] = { -1, 0, 1, 0 }; const int dy[] = { 0, 1, 0, -1 };
const int next[8][2] = { {1, 1}, {1, -1}, {1, 0}, {-1, 1}, {-1, -1}, {-1, 0}, {0, 1}, {0, -1} };
#define maxn 5000010
#define mod 1000000007
//自定义函数区
inline int quickpow(int x, int y) {int re = 1;while (y) {if (y & 1) re = 1ll * re * x % mod;x = 1ll * x * x % mod; y >>= 1;}return re;}
int tot, first[maxn], n, vis[maxn]; ll dis[maxn];
struct edge { ll dis; int to, next; }e[maxn * 2];
struct node { int dis; int pos; bool operator <(const node& x) const { return x.dis < dis; } };
priority_queue<node> q;
inline void add(int x, int y, ll dis) { tot++; e[tot].dis = dis; e[tot].to = y; e[tot].next = first[x]; first[tot] = tot; }
inline void dijkstra() { dis[n] = 0; q.push((node) {0, n});while (!q.empty()) {
	node tmp = q.top(); q.pop(); int x = tmp.pos, d = tmp.dis; if (vis[x]) continue; vis[x] = 1;
	for (int i = first[x]; i; i = e[i].next) {int y = e[i].to;if (dis[y] > dis[x] + e[i].dis) {dis[y] = dis[x] + e[i].dis;if (!vis[y]) { q.push((node) {dis[y], y}); }}}}
}
#define inv(x) quickpow(x, mod - 2)
//变量区 
int t, x, y, k;
bool flag;
ll ans;
string s;
int a[maxn];
//题目函数区
bool myfunction(int i, int j) { return i > j; }
int main(void) {
	read(t);
	while (t--) {
		read(n), read(k);
		For (i, 1, n) read(a[i]);
		nth_element(a + 1, a + k, a + n + 1, myfunction);
		write(a[k]); write2();
	}
	return 0;
}
```
