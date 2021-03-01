# 最长公共子串
题目连接：https://leetcode-cn.com/problems/maximum-length-of-repeated-subarray/submissions/

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
