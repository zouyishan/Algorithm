题目链接：https://codeforces.ml/gym/310606/problem/C

> 题意:给你n个物品,每个物品都有序号，放在堆栈中。你有一个清单，清单上有m个物品必须顺序
> 取出(必须在堆栈中把这件物品以上的都取出来，再把这件物品取出，然后剩下的放回去)一共有
> 2*(k-1)+1次。但是你可以给取出来的那一大部分排个序，使花费最少。

&nbsp;

> 解析:用一个mx记录向下拿到的最深位置,位于该位置之上的，对答案的贡献只有1.
> 位于之下的对答案贡献为 2*(该位置-i)+1

```cpp
#include <iostream>
#include <algorithm>
#include <set>
#include <queue>
#include <cstdio>
#include <cmath>
#include <vector>
#include <string>
#include <cstring>
#include <map>
#include <stack>
using namespace std;
#define For(i, a, b) for(int i = (a), _ = (b); i <= _; i++)
#define IO ios::sync_with_stdio(false);cin.tie(0);cout.tie(0)
#define maxn 200090
#define inf 0x7fffffff
long long t, s, s1, ans;
int n, m;
int a[200010], b[200010];
int main() {
	IO;
	cin >> s;
	while (s--) {
		ans = 0;
		cin >> n >> m;
		int dep = 0;
		for (int i = 1; i <= n; i++) {
			cin >> a[i]; b[a[i]] = i;
		}
		for (int i = 1; i <= m; i++) {
			cin >> t;
			if (b[t] < dep) {
				ans++;
			}
			else {
				ans += (b[t] - i) * 2 + 1;
				dep = b[t];
			}
		}
		cout << ans << endl;
	}
    return 0;
}
```
