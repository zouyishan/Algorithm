题目链接：https://codeforces.com/gym/309935/problem/E

思路：只要是三个数，中间那个数是位于左右两个数之间的就可以删除

例如： 1 2 3: |1 - 2| = 1, |2 - 3| = 1, 1 + 1 = 2 == |1 - 3|。在结果不变的情况下删掉了一个数让其变得更短。

注意初始化book这个东西：

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
#define maxn 200000
#define inf 0x7fffffff
int n, m, t;
int a[maxn], book[maxn];

int main() {
    IO;
    cin >> t;
    while (t--) {
    	cin >> n;
    	m = n;
    	bool flag = false;
    	memset(book, 0, sizeof(book));
    	for (int i = 1; i <= n; i++) cin >> a[i];
    	for (int i = 2; i <= n - 1; i++) {
    		flag = false;
    		if (a[i] > a[i - 1] && a[i + 1] > a[i]) book[i] = 1, flag = true;
    		else if (a[i] > a[i + 1] && a[i - 1] > a[i]) book[i] = 1, flag = true;
    		if (flag) m--;
		} 
		cout << m << endl;
		for (int i = 1; i <= n; i++) {
			if (!book[i]) cout << a[i] << " ";
		}
		cout << endl;
	}
    return 0;
}
```
