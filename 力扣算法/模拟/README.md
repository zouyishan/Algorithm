# 大数加法：

主要就是进位，单纯模拟：https://www.nowcoder.com/practice/11ae12e8c6fe48f883cad618c2e81475?tpId=117&tab=answerKey

```cpp
class Solution {
public:
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     * 计算两个数之和
     * @param s string字符串 表示第一个整数
     * @param t string字符串 表示第二个整数
     * @return string字符串
     */
    string solve(string s, string t) {
        // write code here
        if (s.size() < t.size()) {
            string temp = s;
            s = t;
            t = temp;
        }
        int length1 = s.size(), length2 = t.size(), flag = 0, p = 0, q = 0;
        while (length1 > 0) {
            q = 0;
            int p = s[length1 - 1] - '0';
            if (length2 > 0) {
                q = t[length2 - 1] - '0';
            }
            int total = p + q + flag;
            s[length1 - 1] = (total % 10) + '0';
            if (total >= 10) {   
                flag = total / 10;
            } else flag = 0;
            length1--; length2--;
        }
        if (flag != 0) s = "1" + s;
        return s;
    }
};
```
&nbsp;
&nbsp;
# 大数乘法

令m和n分别表示num1和num2的长度，并且它们均不为0，则num1和num2的乘积长度为`m + n - 1`或`m + n`。证明如下：
![image](https://user-images.githubusercontent.com/57765968/109413431-4ca22380-79e8-11eb-9498-3111c8e13043.png)
所以就可以写出如下代码：
```cpp
class Solution {
public:
    string multiply(string num1, string num2) {
        if (num1 == "0" || num2 == "0") return "0";
        int length1 = num1.size(), length2 = num2.size();
        vector<int> ans(length1 + length2);
        
        for (int i = length1 - 1; i >= 0; i--) {
            int p = num1[i] - '0';
            for (int j = length2 - 1; j >= 0; j--) {
                int q = num2[j] - '0';
                ans[i + j + 1] += p * q;
            }
        }
        for (int i = length1 + length2 - 1; i >= 1; i--) {
            ans[i - 1] += ans[i] / 10;
            ans[i] %= 10;
        }
        string res = "";
        int index = ans[0] == 0 ? 1 : 0;
        while (index < length1 + length2) {
            res.push_back(ans[index++] + '0');
        }
        return res;
    }
};
```

&nbsp;
&nbsp;
# 无重复最长字串

要避免的坑：

首先有空格，我们不能直接s[i] - 'a',这样会炸数组下限，所以要加上个100

再者不能开始就是0，因为字符串开始就是0，所以赋值序号是要i + 1

还要注意我们的ans 的范围和第二次找到的字符的范围。

```cpp
class Solution {
public:
    int a[300], res = 1, ans = 1;
    int lengthOfLongestSubstring(string s) {
        if (s.length() <= 1) return s.length();
        a[s[0] - ' '] = 1;
        for (int i = 1; i < s.length(); i++) {
            if (!a[s[i] - ' ']) {
                res++; a[s[i] - ' '] = i + 1;
            } else {
                if (i + 1 - a[s[i] - ' '] <= res) {
                    res = i + 1 - a[s[i] - ' '];
                } else {
                    res++; 
                }
                a[s[i] - ' '] = i + 1;
            }
            ans = max(ans, res);
        }
        return ans;
    }
};
```
&nbsp;
&nbsp;
# K和数对的最大数目

题目链接：https://leetcode-cn.com/problems/max-number-of-k-sum-pairs/

先排序，然后双指针，然后思想就是如下代码：
```java
if (nums[l] + nums[r] == k) {
    count++; l++; r--;
} 
else if (nums[l] + nums[r] > k) r--;
else l++;
```

```java
class Solution {
    public int maxOperations(int[] nums, int k) {
        Arrays.sort(nums);
        int count = 0;
        int l = 0, r = nums.length - 1;
        while (l < r) {
            if (nums[l] + nums[r] == k) {
                count++; l++; r--;
            } 
            else if (nums[l] + nums[r] > k) r--;
            else l++;
        }
        return count;
    }
}
```
