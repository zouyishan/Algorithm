# 编辑距离
https://leetcode-cn.com/problems/edit-distance/

D[i][j] 表示 A 的前 i 个字母和 B 的前 j 个字母之间的编辑距离。

我们用`A = horse`，`B = ros` 作为例子，来看一看是如何把这个问题转化为规模较小的若干子问题的。

1. 在单词 A 中插入一个字符：如果我们知道 horse 到 ro 的编辑距离为 a，那么显然 horse 到 ros 的编辑距离不会超过 a + 1。这是因为我们可以在 a 次操作后将 horse 和 ro 变为相同的字符串，只需要额外的 1 次操作，在单词 A 的末尾添加字符 s，就能在 a + 1 次操作后将 horse 和 ro 变为相同的字符串；

2. 在单词 B 中插入一个字符：如果我们知道 hors 到 ros 的编辑距离为 b，那么显然 horse 到 ros 的编辑距离不会超过 b + 1，原因同上；

3. 修改单词 A 的一个字符：如果我们知道 hors 到 ro 的编辑距离为 c，那么显然 horse 到 ros 的编辑距离不会超过 c + 1，原因同上。

那么从 horse 变成 ros 的编辑距离应该为 **min(a + 1, b + 1, c + 1)**。

```java
class Solution {
    public int minDistance(String word1, String word2) {
        int n = word1.length();
        int m = word2.length();

        if (n == 0 || m == 0) {
            return n == 0 ? m : n;
        }

        int[][] dp = new int[n + 1][m + 1];
        for (int i = 0; i <= n; i++) {
            dp[i][0] = i;
        }
        for (int j = 0; j <= m; j++) {
            dp[0][j] = j;
        }

        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= m; j++) {
                // 向A串中添加一个数
                int a = dp[i - 1][j] + 1;
                // 向B串中添加一个数
                int b = dp[i][j - 1] + 1;
                int c = dp[i - 1][j - 1];
                // 如果对应位置的数相同就不用修改，不同就修改次数加1
                if (word1.charAt(i - 1) != (word2.charAt(j - 1))) {
                    c += 1;
                }
                // 找到最小的一个数就可以了
                dp[i][j] += Math.min(Math.min(a, b), c);
            }
        }
        return dp[n][m];
    }
}
```
