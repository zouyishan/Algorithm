善用ctrl + f：
* [数字序列中某一位的数字](#数字序列中某一位的数字)
* [复杂链表的复制](#复杂链表的复制)
* [数值的整数次方](#数值的整数次方)
* [矩阵中的路径](#矩阵中的路径)
* [二维数组中查找](#二维数组中查找)

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
