目录：
* [最小路径和](#最小路径和)
* [完全平方数](#完全平方数)
* [最长公共字串](#最长公共字串)
* [最长递增子序列](#最长递增子序列)
* [连续子数组最大和](#连续子数组最大和)
* [最小路径和](#最小路径和)

# 最小路径和
题目链接：https://leetcode-cn.com/problems/minimum-path-sum/

思路：最简单的dp啊！！！就这难度？？？？再给我来难点的！！！！
```java
class Solution {
    public int minPathSum(int[][] grid) {
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (i == 0 && j == 0) continue;
                else if (i == 0) grid[i][j] += grid[i][j - 1];
                else if (j == 0) grid[i][j] += grid[i - 1][j];
                else grid[i][j] += Math.min(grid[i - 1][j], grid[i][j - 1]);
            }
        }
        return grid[grid.length - 1][grid[0].length - 1];
    }
}
```

# 完全平方数
题目链接：https://leetcode-cn.com/problems/perfect-squares/

思路：dp[i] = Math.min(dp[i], dp[i - j * j] + 1);dp太阴间了。多复习吧=_=
```java
class Solution {
    public int numSquares(int n) {
        int[] dp = new int[n + 1];
        dp[0] = 0;
        for (int i = 1; i <= n; i++) {
            dp[i] = i;
            for (int j = 1; i - j * j >= 0; j++) {
                dp[i] = Math.min(dp[i], dp[i - j * j] + 1);
            }
        }
        return dp[n];
    }
}
```

# 最长公共子串
题目连接：https://leetcode-cn.com/problems/maximum-length-of-repeated-subarray/submissions/

思路：当作一个二维数组，通过dp[i][j]和dp[i - 1][j - 1]来找关系。
```cpp
class Solution {
public:
    int min_ = 0;
    int ans[2000][2000];
    int findLength(vector<int>& A, vector<int>& B) {
        for (int i = 1; i <= A.size(); i++) {
            for (int j = 1; j <= B.size(); j++) {
                if (A[i - 1] == B[j - 1]) {
                    ans[i][j] = ans[i - 1][j - 1] + 1;
                    if (ans[i][j] > min_) min_ = ans[i][j];
                }
                else ans[i][j] = 0;
            }
        }
        return min_;
    }
};
```
&nbsp;
&nbsp;
# 最长递增子序列

题目连接：https://leetcode-cn.com/problems/longest-increasing-subsequence/

思路：最通常的一个思路就是dp[i] = max(dp[i], dp[j] + 1)(nums[i] > nums[j])这种方法时间复杂度为O(n * n)。

普通遍历
```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int n = nums.size(), res = -1;
        vector<int> dp(n, 0);
        for (int i = 0; i < nums.size(); i++) {
            dp[i] = 1;
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) dp[i] = max(dp[i], dp[j] + 1);
            }
            res = max(res, dp[i]);
        }
        return res;
    }
};
```
&nbsp;
&nbsp;

维护一个递增数组，数组长度就是我们要找的最长的上升子序列。但是这个数组里面的数并不是正确的。注意`pos`的赋值

二分 + 贪心
```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int len = 1, n = nums.length;
        if (n == 0) {
            return 0;
        }
        int[] d = new int[n + 1];
        d[len] = nums[0];
        for (int i = 1; i < n; ++i) {
            if (nums[i] > d[len]) {
                d[++len] = nums[i];
            } else {
                int l = 1, r = len, pos = 0; 
                while (l <= r) {
                    int mid = (l + r) >> 1;
                    if (d[mid] < nums[i]) {
                        l = mid + 1;
                    } else {
                        pos = mid;
                        r = mid - 1;
                    }
                }
                d[pos] = nums[i];
            }
        }
        return len;
    }
}
```
&nbsp;
&nbsp;
# 连续子数组最大和

题目：https://leetcode-cn.com/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/

如何`num[i - 1]`大于0，就加上，如果是0的话就不加啊！！

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int max_ = nums[0];
        for (int i = 1; i < nums.length; i++) {
            nums[i] += Math.max(nums[i - 1], 0);
            max_ = Math.max(max_, nums[i]);
        }
        return max_;
    }
}
```
