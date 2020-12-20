链接：https://ac.nowcoder.com/acm/contest/10272/E

题目并不难，从坐标轴上的点和不是坐标轴上的点开始考虑，就是有点烦。

```cpp
# include <bits/stdc++.h>
using namespace std;
int l, r, x, y;
string s;
int k, mx, my;
int main(void) {
	cin >> k;
	while (k--) {
		cin >> mx >> my;
		cin >> s;
		l = 0; r = 0; x = 0; y = 0;
		for (int i = 0; i < s.size(); i++) {
			if (s[i] == 'L') l--;
			else if (s[i] == 'R') r++;
			else if (s[i] == 'U') y++;
			else if (s[i] == 'D') x--;
		} 
		if (l + r == mx && x + y == my) {
			cout << "Impossible" << endl;
			continue;
		}
		if (mx == 0 && my == 0) {
			cout << "Impossible" << endl;
			continue;
		}
		
		if (mx == 0) {
			if (r == 0 && l == 0) {
				if (my > 0 && x + y >= my) {
					cout << "Impossible" << endl;
					continue;
				} 
				else if (my > 0 && x + y < my) {
					for (int i = 1; i <= -x; i++) cout << "D";
					for (int i = 1; i <= y; i++) cout << "U";
				}
				else if (my < 0 && x + y <= my) {
					cout << "Impossible" << endl;
					continue;
				}
				else if (my < 0 && x + y > my) {
					for (int i = 1; i <= y; i++) cout << "U";
					for (int i = 1; i <= -x; i++) cout << "D";
				}
			}
			else {
				if(-l>r){
					for (int i = 1; i <= -l; i++) cout << "L";
					for (int i = 1; i <= -x; i++) cout << "D";
					for (int i = 1; i <= y; i++) cout << "U";
					for (int i = 1; i <= r; i++) cout << "R";
				}
				else{
					for (int i = 1; i <= r; i++) cout << "R";
					for (int i = 1; i <= -x; i++) cout << "D";
					for (int i = 1; i <= y; i++) cout << "U";
					for (int i = 1; i <= -l; i++) cout << "L";
				}
			}
			
		}
		else if (my == 0) {
			if (x == 0 && y == 0) {
				if (mx > 0 && l + r >= mx) {
					cout << "Impossible" << endl;
					continue;
				}
				else if (mx > 0 && l + r < mx) {
					for (int i = 1; i <= -l; i++) cout << "L";
					for (int i = 1; i <= r; i++) cout << "R";
				}
				else if (mx < 0 && l + r <= mx) {
					cout << "Impossible" << endl;
					continue;
				}
				else if (mx < 0 && l + r > mx) {
					for (int i = 1; i <= r; i++) cout << "R";
					for (int i = 1; i <= -l; i++) cout << "L";
				}
			}
			else {
				if(y>-x) {
					for (int i = 1; i <= y; i++) cout << "U";
					for (int i = 1; i <= -l; i++) cout << "L";
					for (int i = 1; i <= r; i++) cout << "R";
					for(int i = 1; i <= -x; i ++) cout << "D";
				}
				else {
					for(int i = 1; i <= -x; i ++) cout << "D";
					for (int i = 1; i <= -l; i++) cout << "L";
					for (int i = 1; i <= r; i++) cout << "R";
					for (int i = 1; i <= y; i++) cout << "U";
				}
			}
		}
		
		else {
			if (l + r != mx) {
				for (int i = 1; i <= -l; i++) cout << "L";
				for (int i = 1; i <= r; i++) cout << "R";
				for (int i = 1; i <= -x; i++) cout << "D";
				for (int i = 1; i <= y; i++) cout << "U";
			}
			else {
				for (int i = 1; i <= -x; i++) cout << "D";
				for (int i = 1; i <= y; i++) cout << "U";
				for (int i = 1; i <= -l; i++) cout << "L";
				for (int i = 1; i <= r; i++) cout << "R";
			}
		}
		cout << endl;
	}
}
```
