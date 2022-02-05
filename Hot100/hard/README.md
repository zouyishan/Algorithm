# 矩阵中的最长递增路径
记忆化火葬场。爆调
```java
class Solution {
    public static int res = 0, ans = 0;
    public static int[][] next = new int[][]{{1, 0}, {-1, 0}, {0, -1}, {0, 1}};
    public static int[][] max_ = null;
    public int longestIncreasingPath(int[][] matrix) {
        if (matrix.length == 0 || matrix[0].length == 0) {
            return 0;
        }
        res = 0;
        max_ = new int[matrix.length][matrix[0].length];
        boolean[][] visit = new boolean[matrix.length][matrix[0].length];
        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[0].length; j++) {
                max_[i][j] = -1;
            }
        }

        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[0].length; j++) {
                if (max_[i][j] == -1) {
                    visit[i][j] = true;
                    ans = 0;
                    dfs(visit, i, j, matrix, 1, matrix[i][j]);
                    visit[i][j] = false;
                } else {
                    res = res < max_[i][j] ? max_[i][j] : res;
                }
            }
        } 

        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[0].length; j++) {
                System.out.print(max_[i][j] + " ");
            }
            System.out.println();
        }
        return res;
    }

    public void dfs(boolean[][] visit, int x, int y, int[][] matrix, int count, int val) {
        res = res < count ? count : res;
        boolean flag = false;

        for (int i = 0; i < 4; i++) {
            int tempx = x + next[i][0];
            int tempy = y + next[i][1];
            if (tempx >= 0 && tempy >= 0 && tempx < matrix.length && tempy < matrix[0].length && visit[tempx][tempy] == false && matrix[tempx][tempy] > val) {
                visit[tempx][tempy] = true;

                flag = true;
                if (max_[tempx][tempy] == -1) {
                    dfs(visit, tempx, tempy, matrix, count + 1, matrix[tempx][tempy]);
                    max_[x][y] = Math.max(max_[x][y], max_[tempx][tempy] + 1);
                } else {
                    visit[tempx][tempy] = false;
                    ans = Math.max(count + max_[tempx][tempy], ans);
                    res = Math.max(count + max_[tempx][tempy], res);
                    max_[x][y] = Math.max(max_[x][y], max_[tempx][tempy] + 1);
                    continue;
                }
                visit[tempx][tempy] = false;
            }
        }

        if (!flag) {
            ans = Math.max(count, ans);
            max_[x][y] = 1;
        }
        if (max_[x][y] == -1) {
            max_[x][y] = ans - count + 1;
        }
    }
}
```
# 买卖股票的最佳时机Ⅳ
没想到，基于买卖股票的最佳时机Ⅲ，居然真的成了.....hh 是我没想到的
```java
class Solution {
    public int maxProfit(int k, int[] prices) {
        if (prices.length <= 1 || k == 0) {
            return 0;
        }

        int[] buy = new int[k];
        int[] sell = new int[k];
        Arrays.fill(buy, -prices[0]);
        Arrays.fill(sell, 0);

        for (int i = 1; i < prices.length; i++) {
            buy[0] = Math.max(buy[0], -prices[i]);
            sell[0] = Math.max(sell[0], prices[i] + buy[0]);
            for (int j = 1; j < k; j++) {
                buy[j] = Math.max(buy[j], sell[j - 1] - prices[i]);
                sell[j] = Math.max(sell[j], prices[i] + buy[j]);
            }
        }

        return sell[k - 1];
    }
}
```
# 买卖股票的最佳时机Ⅲ
这个dp........就nm 离大谱。注意这里的 sell1 是必须减去buy2的。具体仔细体会

https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/
```java
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        int buy1 = -prices[0], sell1 = 0;
        int buy2 = -prices[0], sell2 = 0;
        for (int i = 1; i < n; ++i) {
            buy1 = Math.max(buy1, -prices[i]);
            sell1 = Math.max(sell1, buy1 + prices[i]);
            buy2 = Math.max(buy2, sell1 - prices[i]);
            sell2 = Math.max(sell2, buy2 + prices[i]);
        }
        return sell2;
    }
}
```

# 分发糖果
https://leetcode-cn.com/problems/candy/
```java
class Solution {
    public int candy(int[] ratings) {
        int res = 1, pre = 1, des = 0, inc = 1;

        for (int i = 1; i < ratings.length; i++) {
            if (ratings[i] >= ratings[i - 1]) {
                des = 0;
                pre = ratings[i] == ratings[i - 1] ? 1 : pre + 1;
                res += pre;
                inc = pre;
            } else {
                des++;
                if (inc == des) {
                    des++;
                }
                res += des;
                pre = 1;
            }
        }
        return res;
    }
}
```
