善用ctrl + f：
* [数组中数字出现的次数 II](#数组中数字出现的次数 II)
* [把数字翻译成字符串](#把数字翻译成字符串)
* [丑数](#丑数)
* [把字符串转换成整数](#把字符串转换成整数)
* [无重复字符的最长子串](#无重复字符的最长子串)
* [礼物的最大价值](#礼物的最大价值)
* [把数组排成最小的数](#把数组排成最小的数)
* [数组中数字出现的次数](#数组中数字出现的次数)
* [二叉树中和为某一值的路径](#二叉树中和为某一值的路径)
* [数字序列中某一位的数字](#数字序列中某一位的数字)
* [复杂链表的复制](#复杂链表的复制)
* [数值的整数次方](#数值的整数次方)
* [矩阵中的路径](#矩阵中的路径)
* [二维数组中查找](#二维数组中查找)

# 数组中数字出现的次数 II
https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-ii-lcof/

直接用位运算
```java
class Solution {
    public int singleNumber(int[] nums) {
        int[] count = new int[33];
        for (int i = 0; i < nums.length; i++) {
            get(count, nums[i]);
        }
        for (int i = 0; i <= 32; i++) {
            count[i] %= 3;
        }
        int res = 0, k = 0;
        for (int i = 0; i <= 32; i++) {
            // System.out.println(count[i]);
            res += Math.pow(2, k) * count[i];
            k++;
        }
        return res;
    }
    public static void get(int[] count, int val) {
        int k = 0;
        while (val != 0) {
            count[k++] += (val & 1);
            val >>= 1;
        }
    }
}
```

# 把数字翻译成字符串
https://leetcode-cn.com/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/

这个dp和爬楼梯的dp有什么区别吗？？？？
```java
class Solution {
    public int translateNum(int num) {
        String str = String.valueOf(num);

        int[] dp = new int[str.length() + 1];
        dp[1] = 1;
        dp[0] = 1;

        for (int i = 2; i <= str.length(); i++) {
            String temp = str.substring(i - 2, i);
            if (temp.compareTo("25") <= 0 && temp.compareTo("10") >= 0) {
                dp[i] = dp[i - 1] + dp[i - 2];
            } else {
                dp[i] = dp[i - 1];
            }
        }

        return dp[str.length()];
    }
}
```
# 丑数
https://leetcode-cn.com/problems/chou-shu-lcof/

dp，不能枚举k，要枚举第n个数
```go
func nthUglyNumber(n int) int {
    dp := make([]int, n + 1)
    dp[1] = 1
    p1, p2, p3 := 1, 1, 1
    max := func(a, b, c int) int {
        if a >= b {
            if b >= c {
                return c
            } else {
                return b
            }
        } else {
            if a >= c {
                return c
            } else {
                return a
            }
        }
    }

    for i := 2; i <= n; i++ {
        ans1, ans2, ans3 := dp[p1] * 2, dp[p2] * 3, dp[p3] * 5
        num := max(ans1, ans2, ans3)
        
        dp[i] = num
        if num == ans1 {
            p1++
        }
        if num == ans2 {
            p2++
        }
        if num == ans3 {
            p3++
        }
    }
    return dp[n]
}
```

# 把字符串转换成整数
https://leetcode-cn.com/problems/ba-zi-fu-chuan-zhuan-huan-cheng-zheng-shu-lcof/

go渐入佳境，注意这个无符号直接赋值的情况，因为它可以表示的值太大了
```go
func strToInt(str string) int {
    if len(str) == 0 {
        return 0
    }
    count, sig := 0, 1
    for count < len(str) && str[count] == ' ' {
        count++
    }
    if count < len(str) && str[count] == '-' {
        sig = -1
        count++
    } else if count < len(str) && str[count] == '+' {
        sig = 1
        count++
    }

    ans := 0
    for count < len(str) && str[count] <= '9' && str[count] >= '0' {
        ans = ans * 10 + int(str[count] - '0')
        count++
        if sig == -1 {
            if ans >= 1 << 31 {
                return (1 << 31) * -1
            }
        } else {
            if ans > ((1 << 31) - 1) {
                return (1 << 31) - 1
            }
        }
    }
    
    return ans * sig
}
```
# 无重复字符的最长子串
https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/

小模拟，用go谢谢回回味道
```go
func lengthOfLongestSubstring(s string) int {
    a := make([]int, 200)
    ans := 0
    res := 0
    for i := range s {
        if a[s[i] - 'a' + 80] == 0 {
            ans++
        } else {
            if i + 1 - a[s[i] - 'a' + 80] > ans {
                ans++
            } else {
                ans = i + 1 - a[s[i] - 'a' + 80]
            }
        }
        a[s[i] - 'a' + 80] = i + 1
        if ans > res {
            res = ans
        }
    }
    return res
}
```
# 礼物的最大价值
https://leetcode-cn.com/problems/li-wu-de-zui-da-jie-zhi-lcof/

又是一个dp
```go
func maxValue(grid [][]int) int {
    n := len(grid)
    m := len(grid[0])

    dp := make([][]int, n + 1)
    for i := 0; i < len(dp); i++ {
        dp[i] = make([]int, m + 1)
    }

    max := func(a, b int) int {
        if a > b {
            return a
        }
        return b
    }

    for i := 1; i <= n; i++ {
        for j := 1; j <= m; j++ {
            dp[i][j] = max(dp[i][j - 1], dp[i - 1][j]) + grid[i - 1][j - 1]
        }
    }
    return dp[n][m]
}
```
# 把数组排成最小的数
https://leetcode-cn.com/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof/

论如何用好参数的重要性
```go
func minNumber(nums []int) string {
    sort.Slice(nums, func(i, j int) bool {
        x := strconv.Itoa(nums[i]) + strconv.Itoa(nums[j])
        y := strconv.Itoa(nums[j]) + strconv.Itoa(nums[i])
        return x < y
    })

    res := ""
    for _, k := range nums {
        res += strconv.Itoa(k)
    }
    return res
}
```

# 数组中数字出现的次数
https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof/

异或用的很好。要好好学学
```go
func singleNumbers(nums []int) []int {
    res := 0
    for _, k := range nums {
        res ^= k
    }
    pos := 1
    for (pos & res) == 0 {
        pos <<= 1
    }

    a, b := 0, 0
    for _, k := range nums {
        if k & pos == 0 {
            a ^= k
        } else {
            b ^= k
        } 
    }

    return []int{a, b}
}
```

# 二叉树中和为某一值的路径
https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/

学习下go的语法
```go
func pathSum(root *TreeNode, target int) [][]int {
    res := [][]int{}
    ans := []int{}

    var dfs func(*TreeNode, int)
    dfs = func(root *TreeNode, target int) {
        if root != nil {
            target -= root.Val
            ans = append(ans, root.Val)
            defer func() {ans = ans[:len(ans) - 1]}()
            if root.Left == nil && root.Right == nil && target == 0 {
                res = append(res, append([]int{}, ans...))
                return
            }
            dfs(root.Left, target)
            dfs(root.Right, target)
        }
    }
    dfs(root, target)
    return res
}
```
# 数字序列中某一位的数字
https://leetcode-cn.com/problems/shu-zi-xu-lie-zhong-mou-yi-wei-de-shu-zi-lcof/

模拟，哎 没敢想出来
```go
func findNthDigit(n int) int {
    if n == 0 {
        return 0
    }

    start, digital, count := 1, 1, 9
    
    for n > count {
        n -= count
        digital++
        start *= 10
        count = 9 * start * digital
    }

    k := start + (n - 1) / digital
    index := (n - 1) % digital

    numStr := strconv.Itoa(k)

    return int(numStr[index] - '0')
}
```
# 复杂链表的复制
注意用好哈希表就可以: https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/
```java
class Solution {
    public Node copyRandomList(Node head) {
        if (head == null) {
            return null;
        }

        Map<Node, Node> map = new HashMap<>();
        Node res = new Node(head.val);
        Node head1 = res, head2 = head, ans = res;
        map.put(head, res);
        while (head.next != null) {
            res.next = new Node(head.next.val);
            map.put(head.next, res.next);
            res = res.next;
            head = head.next;
        }

        while (head2 != null) {
            if (head2.random != null) {
                head1.random = map.get(head2.random);
            }
            head2 = head2.next;
            head1 = head1.next;
        }

        return ans;
    }
}
```
# 数值的整数次方
快速幂：https://leetcode-cn.com/problems/shu-zhi-de-zheng-shu-ci-fang-lcof/
```java
class Solution {
    public double myPow(double x, int n) {
        if (x == 1) {
            return x;
        }
        long b = n;
        double res = 1.0;
        if (n < 0) {
            x = 1 / x;
            b = -b;
        }

        while (b != 0) {
            if ((b & 1) == 1) {
                res *= x;
            }
            x *= x;
            b >>= 1;
        }

        return res;
    }
}
```
# 矩阵中的路径
要记住剪枝：https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/
```java
class Solution {
    public static int[] nextx = new int[]{1, -1, 0, 0};
    public static int[] nexty = new int[]{0, 0, 1, -1};
    public static boolean res = false;

    public boolean exist(char[][] board, String word) {
        res = false;
        boolean[][] book = new boolean[board.length][board[0].length];
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                if (board[i][j] == word.charAt(0)) {
                    book[i][j] = true;
                    dfs(board, i, j, word, 1, book);
                    if (res) {
                        return res;
                    }
                    book[i][j] = false;
                }
            }
        }

        return res;
    }

    public void dfs(char[][] board, int x, int y, String word, int count, boolean[][] book) {
        if (count == word.length()) {
            res = true;
            return;
        }

        for (int i = 0; i < 4; i++) {
            int tempx = x + nextx[i];
            int tempy = y + nexty[i];
            if (tempx >= 0 && tempx < board.length && tempy >= 0 && tempy < board[0].length && book[tempx][tempy] == false && board[tempx][tempy] == word.charAt(count)) {
                book[tempx][tempy] = true;
                dfs(board, tempx, tempy, word, count + 1, book);
                book[tempx][tempy] = false;
                if (res) {
                    return;
                }
            }
        }
    }
}
```
# 二维数组中查找
二分秒掉: https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/
```java
class Solution {
    public boolean findNumberIn2DArray(int[][] matrix, int target) {
        if (matrix.length == 0 || matrix[0].length == 0) {
            return false;
        }

        for (int i = 0; i < Math.min(matrix[0].length, matrix.length); i++) {
            if (matrix[i][i] == target || matrix[i][matrix[0].length - 1] == target || matrix[matrix.length - 1][i] == target) {
                return true;
            }
            if (matrix[i][i] > target) {
                return false;
            }

            if (matrix[i][matrix[0].length - 1] > target) {
                int l = i, r = matrix[0].length - 1;
                while (l <= r) {
                    int mid = (l + r) >> 1;
                    int res = matrix[i][mid];
                    if (res > target) {
                        r = mid - 1;
                    } else if (res < target) {
                        l = mid + 1;
                    } else {
                        return true;
                    }
                }
            }

            if (matrix[matrix.length - 1][i] > target) {
                int l = i, r = matrix.length - 1;
                while (l <= r) {
                    int mid = (l + r) >> 1;
                    int res = matrix[mid][i];
                    if (res > target) {
                        r = mid - 1;
                    } else if (res < target) {
                        l = mid + 1;
                    } else {
                        return true;
                    }
                }
            }
        }
        return false;
    }
}
```
