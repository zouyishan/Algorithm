目录：
* [每日温度](#每日温度)
* [根据身高重建队列](#根据身高重建队列)
* [找到数组中消失的数字](#找到数组中消失的数字)
* [大数加法](#大数加法)
* [大数乘法](#大数乘法)
* [无重复最长字串](#无重复最长字串)
* [K和数对的最大数目](#K和数对的最大数目)
* [二进制中1的个数](#二进制中1的个数)
* [左旋字符串](#左旋字符串)
* [二叉搜索树第k大节点](#二叉搜索树第k大节点)
* [和为s的连续正数序列](#和为s的连续正数序列)
* [三数之和](#三数之和)
* [子集](#子集)
* [括号生成](#括号生成)

# 每日温度
题目链接：https://leetcode-cn.com/problems/daily-temperatures/

思路：差分暴力！！！！！！不过好像有那个单调栈的最优解，但是。我不会啊啊啊啊啊啊=_=难
```java
class Solution {
    public int[] dailyTemperatures(int[] T) {
        int[] res = new int[T.length];
        int[] ans = new int[T.length];
        if (T.length == 1) return res;
        for (int i = 1; i < T.length; i++) res[i] = T[i] - T[i - 1];
        for (int i = 0; i < T.length; i++) {
            int z = 0;
            for (int j = i + 1; j < T.length; j++) {
                z += res[j];
                if (z > 0) {
                    ans[i] = j - i; break;
                }
            }
        }
        return ans;
    }
}
```

# 根据身高重建队列
题目链接；https://leetcode-cn.com/problems/queue-reconstruction-by-height/

思路：先对升高进行降序排列，然后再根据第二个参数所占用的位置来排列。这样就能成
```java
class Solution {
    public int[][] reconstructQueue(int[][] people) {
        Arrays.sort(people, new Comparator<int[]>() {
            public int compare(int[] person1, int[]person2) {
                if (person1[0] == person2[0]) {
                    return person1[1] - person2[1];
                }
                return person2[0] - person1[0];
            }
        });
        List<int[]> res = new ArrayList<int[]>();
        for (int[] person : people) {
            res.add(person[1], person);
        }
        return res.toArray(new int[res.size()][]);
    }
}
```

# 括号生成
题目链接：https://leetcode-cn.com/problems/generate-parentheses/

思路：就是基本的暴搜啊，和前面的那题，子集有异曲同工之妙啊！！！要加深对这个的理解了~！！！
```java
class Solution {
    public List<String> generateParenthesis(int n) {
        LinkedList<String> res = new LinkedList<String>();
        dfs("", 0, 0, n, res);
        return res;
    }
    public void dfs(String cur, int left, int right, int total, LinkedList<String> res) {
        if (left == total && right == total) {
            res.add(cur);
            return;
        }
        if (left < right) return;
        if (left < total) dfs(cur + "(", left + 1, right, total, res);
        if (right < total) dfs(cur + ")", left, right + 1, total, res);
    }
}
```

# 找到数组中消失的数字
题目链接： https://leetcode-cn.com/problems/find-all-numbers-disappeared-in-an-array/

思路：很精妙，我在有的数字上面加上length，也就是说数组中存在的数最后一定是大于length的，没有的就肯定不会大于
```java
class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        int length = nums.length;
        for (int num : nums) {
            int temp = (num - 1) % length;
            nums[temp] += length;
        }
        List<Integer> res = new LinkedList<Integer>();
        for (int i = 0; i < length; i++) {
            if (nums[i] <= length) res.add(i + 1);
        }
        return res;
    }
}
```
&nbsp;
# 大数加法：

主要就是进位，单纯模拟：https://www.nowcoder.com/practice/11ae12e8c6fe48f883cad618c2e81475?tpId=117&tab=answerKey

```cpp
class Solution {
public:
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

### Java版

注意使用`StringBuilder()`的api！！！

```java
class Solution {
    public String addStrings(String num1, String num2) {
        if (num1.length() < num2.length()) {
            String temp = num1;
            num1 = num2;
            num2 = temp;
        }
        int carry = 0;
        int length1 = num1.length(), length2 = num2.length();
        int p = 0, q = 0;
        StringBuilder sb = new StringBuilder("");
        while (length1 > 0) {
            p = num1.charAt(--length1) - '0'; q = 0;
            if (length2 > 0) q = num2.charAt(--length2) - '0';
            int sum = p + q + carry;
            sb.append(sum % 10);
            if (sum >= 10) carry = 1;
            else carry = 0;
        }
        if (carry > 0) sb.append('1');
        return sb.reverse().toString();
    }
}
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
&nbsp;
&nbsp;
# 二进制中1的个数

题目：https://leetcode-cn.com/problems/er-jin-zhi-zhong-1de-ge-shu-lcof/

思路：
```java
count += (n & 1);
n >>>= 1;
```
代码：
```java
public class Solution {
    // you need to treat n as an unsigned value
    public int hammingWeight(int n) {
        int count = 0;
        while (n != 0) {
            count += (n & 1);
            n >>>= 1;
        }
        return count;
    }
}
```
&nbsp;
&nbsp;
# 左旋字符串
题目链接：https://leetcode-cn.com/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/

题目用意：纯粹为了记录一下substring这个api的使用，substring(index)，从这个开始到后面的所有，substring(begin, end)。很明了，要begin开始到end结束的字符串，不包括end！

```java
class Solution {
    public String reverseLeftWords(String s, int n) {
        StringBuilder sb = new StringBuilder();
        return sb.append(s.substring(n, s.length())).append(s.substring(0, n)).toString();
    }
}
```
&nbsp;
&nbsp;
# 二叉搜索树第k大节点

题目链接：https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/

题目用意：加深一下中序遍历，活学活用！

```java
class Solution {
    int res = 0, ans = 0;
    public int kthLargest(TreeNode root, int k) {
        res = k;
        dfs(root);
        return ans;
    }
    public void dfs(TreeNode node) {
        if (node != null) {
            dfs(node.right);
            res--;
            if (res == 0) ans = node.val;
            dfs(node.left);
        }
    }
}
```
&nbsp;
# 和为s的连续正数序列
链接：https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/

题目思路：题目用的是双指针算法。emmmmm 具体看代码

```java
class Solution {
    public int[][] findContinuousSequence(int target) {
        int i = 1, j = 2, s = 3;
        List<int[]> res = new LinkedList<>();
        while (i < j) {
            if (s == target) {
                int[] ans = new int[j - i + 1];
                for (int k = i; k <= j; k++) ans[k - i] = k;
                res.add(ans);
            }
            if (s >= target) {
                s -= i;
                i++;
            } else {
                j++;
                s += j;
            }
        }
        return res.toArray(new int[0][]);
    }
}
```

# 三数之和

链接：https://leetcode-cn.com/problems/3sum/

思路：开始的时候进行排序，然后从前到后枚举节点，开始遍历注意要排除重复数！！！

```java
class Solution {
    public static List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> ans = new ArrayList();
        int len = nums.length;
        if(nums == null || len < 3) return ans;
        Arrays.sort(nums); // 排序
        for (int i = 0; i < len ; i++) {
            if(nums[i] > 0) break; 
            if(i > 0 && nums[i] == nums[i-1]) continue; 
            int L = i+1;
            int R = len-1;
            while(L < R){
                int sum = nums[i] + nums[L] + nums[R];
                if(sum == 0){
                    ans.add(Arrays.asList(nums[i],nums[L],nums[R]));
                    while (L<R && nums[L] == nums[L+1]) L++; 
                    while (L<R && nums[R] == nums[R-1]) R--; 
                    L++;
                    R--;
                }
                else if (sum < 0) L++;
                else if (sum > 0) R--;
            }
        }        
        return ans;
    }
}
```
&nbsp;
&nbsp;
# 子集

链接：https://leetcode-cn.com/problems/subsets/

思路：下面这个图和明白，我们递归只用判断是否需要选，选的话递归下去，不选的话也是就递归下去。

<img width="984" alt="1613704529-qfwaRN-E2C55933-0D86-4F8E-82B9-430682B5B099" src="https://user-images.githubusercontent.com/57765968/112477378-e5d91580-8dad-11eb-9200-8899fb380794.png">

解释可能有点拉跨，代码看的明白 √：

```java
class Solution {
    public static List<List<Integer>> res = null;
    public static LinkedList<Integer> ans = new LinkedList<Integer>();
    public List<List<Integer>> subsets(int[] nums) {
        res = new LinkedList<List<Integer>>();
        dfs(0, nums);
        return res;
    }
    public void dfs(int cur, int[] nums) {
       if (cur == nums.length) {
           res.add(new LinkedList<Integer>(ans));
           return;
       }
       dfs(cur + 1, nums); // 这里表示不选
       ans.add(nums[cur]); // 这里表示选
       dfs(cur + 1, nums); // 在将我们选的选择递归下去。
       ans.removeLast();
    }
}
```
