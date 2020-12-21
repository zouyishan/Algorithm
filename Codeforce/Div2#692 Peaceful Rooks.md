题目链接：https://codeforces.ml/contest/1465/problem/C

用并查集啊！！！！ 开始没想到 难受==

反正就是气死人啊

```cpp
#include <iostream>
#include <algorithm>
#include <set>
#include <queue>
#include <string>
#include <cstring>
#include <cstdio>
#include <vector>
#include <map>
#include <stack>
using namespace std;
#define For(i, a, b) for(int i = (a), _ = (b); i <= _; i++)
#define IO ios::sync_with_stdio(false);cin.tie(0);cout.tie(0)
#define maxn 5000010
int n, m, t, fa[200010];
void init() {
	for (int i = 1; i <= 200005; i++) fa[i] = i;
}
int findf(int x) {
	return x == fa[x] ? x : (fa[x] = findf(fa[x]));
}
int ans;
int main(void) {
	IO;
	cin >> t;
	while (t--) {
		cin >> n >> m; ans = 0;
		init();
		for (int i = 1; i <= m; i++) {
			int a, b; cin >> a >> b;
			if (a == b) continue;
			ans++;
			if (findf(a) == findf(b)) ans++;
			else fa[findf(a)] = findf(b);
		} 
		cout << ans << endl;
	}
    return 0;
}
```
