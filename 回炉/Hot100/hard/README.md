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
