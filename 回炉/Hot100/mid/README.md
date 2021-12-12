# 不同的二叉搜索树
是个dp哦，n个结点的二叉树的个数等于左边二叉树的数量乘上右边二叉树的数量。左边的二叉树为0的时候，右边的就为dp[n - 1]。同理右边的二叉树为0 的时候，左边的为dp[n - 1] : https://leetcode-cn.com/problems/unique-binary-search-trees/
```java
class Solution {
    public int numTrees(int n) {
        if (n == 1) {
            return 1;
        }

        int[] dp = new int[n + 1];
        dp[0] = 1;
        dp[1] = 1;

        for (int i = 2; i <= n; i++) {
            int num = 0;
            for (int j = 0; j < i; j++) {
                num = num + (dp[i - j - 1] * dp[j]);
            } 
            dp[i] = num;
        }
        return dp[n];
    }
}
```
# 单词搜素
经典的dfs啊: https://leetcode-cn.com/problems/word-search/
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
            }
        }
    }
}
```

# 最长回文子串
这里用的是马拉车算法,确实可以O(n)：https://leetcode-cn.com/problems/longest-palindromic-substring/
```java
class Solution {
    public String longestPalindrome(String s) {
        if (s.length() == 1) {
            return s;
        }

        StringBuilder str = new StringBuilder("#");
        for (int i = 0; i < s.length(); i++) {
            str.append(s.charAt(i));
            str.append('#');
        }
        s = str.toString();

        int center = -1, right = -1, length = 0;
        int start = 0, end = 0;
        int[] record = new int[s.length()];
        for (int i = 0; i < s.length(); i++) {
            if (right >= i) {
                int mirror = center * 2 - i;
                int minLength = Math.min(record[mirror], right - i);
                length = expand(s, i - minLength, i + minLength);
            } else {
                length = expand(s, i, i);
            }

            record[i] = length;
            if (i + length > right) {
                center = i;
                right = i + length;
            }

            if (end - start < length * 2) {
                start = i - length;
                end = i + length;
            }
        }

        String res = "";
        for (int i = start; i <= end; i++) {
            if (s.charAt(i) != '#') {
                res += s.charAt(i);
            }
        }
        return res;
    }

    public int expand(String s, int l, int r) {
        while (l >= 0 && r < s.length() && s.charAt(l) == s.charAt(r)) {
            l--;
            r++;
        }
        return (r - l - 2) / 2;
    }
}
```
# 岛屿数量
经典dfs的 补一下：https://leetcode-cn.com/problems/number-of-islands/
```java
class Solution {
    public static int[] startx = new int[] {1, -1, 0, 0};
    public static int[] starty = new int[] {0, 0, 1, -1};
    public int numIslands(char[][] grid) {
        boolean[][] book = new boolean[grid.length][grid[0].length];

        int res = 0;
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (book[i][j] == false && grid[i][j] == '1') {
                    res++;
                    dfs(book, grid, i, j);
                }
            }
        }
        return res;
    }

    public void dfs(boolean[][] book, char[][] grid, int x, int y) {
        for (int i = 0; i <= 3; i++) {
            int tempx = x + startx[i];
            int tempy = y + starty[i];
            if (tempx >= 0 && tempx < grid.length && tempy >= 0 && tempy < grid[0].length && book[tempx][tempy] == false && grid[tempx][tempy] == '1') {
                book[tempx][tempy] = true;
                dfs(book, grid, tempx, tempy);
            }
        }
    }
}
```
# 跳跃游戏
初级版本，是否有环形链表的：https://leetcode-cn.com/problems/linked-list-cycle/
```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        ListNode first = head, end = head;
        while (end != null) {
            if (end.next == null) break;
            first = first.next;
            end = end.next.next;
            if (first == end) return true;
        }
        return false;
    }
}
```
进阶版的，找到环形链表的入口： https://leetcode-cn.com/problems/linked-list-cycle/
```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        if (head == null || head.next == null) {
            return null;
        }

        ListNode slow = head, fast = head;
        while (fast != null) {
            if (fast.next == null) {
                return null;
            }
            fast = fast.next.next;
            slow = slow.next;
            if (fast == slow) {
                ListNode cur = head;
                while (cur != slow) {
                    cur = cur.next;
                    slow = slow.next;
                }
                return cur;
            }
        }
        return null;
    }
}
```
# 盛水的容器
双指针的题

https://leetcode-cn.com/problems/container-with-most-water/
```java
class Solution {
    public int maxArea(int[] height) {
        int l = 0, r = height.length - 1, res = -1;
        while (l < r) {
            res = Math.max(Math.min(height[r], height[l]) * (r - l), res);
            if (height[l] < height[r]) {
                l++;
            } else {
                r--;
            }
        }
        return res;
    }
}
```
# 跳跃游戏
确实很难绷得住，写了个超过13.3%的。

https://leetcode-cn.com/problems/jump-game/
```java
class Solution {
    public boolean canJump(int[] nums) {
        if (nums.length <= 1) {
            return true;
        }
        boolean[] book = new boolean[nums.length];
        book[0] = true;
        for (int i = 0; i < nums.length; i++) {
            if (book[i] == true) {
                if (nums[i] + i >= nums.length - 1) {
                    return true;
                }
                for (int j = 1; j <= nums[i]; j++) {
                    book[i + j] = true;
                }
            }
        }
        return false;
    }
}
```
聪明的解法
```java
class Solution {
    public boolean canJump(int[] nums) {
        int temp = 0;
        for (int i = 0; i < nums.length; i++) {
            temp = Math.max(nums[i] + i, temp);
            if (temp + 1 >= nums.length) {
                return true;
            }
            if (i == temp) {
                return false;
            }
        }
        return false;
    }
}
```
# 组合总和
https://leetcode-cn.com/problems/combination-sum/

dfs，这个和括号序列很像很像，但是开始做的时候还是没有思路....................
```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> res = new LinkedList<>();
        List<Integer> ans = new LinkedList<>();
        dfs(res, ans, target, 0, candidates);
        return res;
    }

    public void dfs(List<List<Integer>> res, List<Integer> ans, int target, int idx, int[] total) {
        if (idx >= total.length) {
            return;
        }
        if (target == 0) {
            res.add(new LinkedList<>(ans));
            return;
        }

        dfs(res, ans, target, idx + 1, total);

        if (target - total[idx] >= 0) {
            ans.add(total[idx]);
            dfs(res, ans, target - total[idx], idx, total);
            ans.remove((int) ans.size() - 1);
        }
    }
}
```
# 在排序数组中查找元素的第一个和最后一个位置
二分的思想淋漓尽致。多学学

https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/

多看看下面的代码，你就能领悟二分查找了
```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int leftIdx = binarySearch(nums, target, true);
        int rightIdx = binarySearch(nums, target, false);
        if (leftIdx <= rightIdx && rightIdx < nums.length && nums[leftIdx] == target && nums[rightIdx] == target) {
            return new int[]{leftIdx, rightIdx};
        } 
        return new int[]{-1, -1};
    }

    public int binarySearch(int[] nums, int target, boolean lower) {
        int left = 0, right = nums.length - 1, ans = nums.length;
        while (left <= right) {
            int mid = (left + right) / 2;
            if (nums[mid] > target || (lower && nums[mid] >= target)) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        
        if (lower) {
            return right + 1; // left
        } else {
            return left - 1; // right
        }
    }
}
```

# 删除链表的倒数第N个结点
https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/

还行，dfs做的
```java
class Solution {
    public static int level = 0;

    public ListNode removeNthFromEnd(ListNode head, int n) {
        level = 0;
        if (head.next == null) {
            return null;
        }
        return dfs(head, n, null);
    }

    public ListNode dfs(ListNode cur, int n, ListNode pre) {
        if (cur == null) {
            return null;
        }
        dfs(cur.next, n, cur);
        level++;
        if (level == n) {
            if (pre == null) {
                return cur.next;
            }
            pre.next = cur.next;
            cur.next = null;
        }
        return cur;
    }
}
```

# 全排列
二刷全排列了啊。还行吧

https://leetcode-cn.com/problems/permutations/
```java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> res = new LinkedList<>();
        if (nums.length == 1) {
            res.add(Arrays.asList(nums[0]));           
            return res;
        }

        dfs(res, nums, 0, nums.length, new LinkedList<>(), new boolean[nums.length]);
        return res;
    }

    public void dfs(List<List<Integer>> res, int[] nums, int count, int total, List<Integer> ans, boolean[] book) {
        if (count == total) {
            res.add(new LinkedList<>(ans));
            return;
        }

        for (int i = 0; i < nums.length; i++) {
            if (book[i] == false) {
                book[i] = true;
                ans.add(nums[i]);
                dfs(res, nums, count + 1, total, ans, book);
                book[i] = false;
                ans.remove((Integer) nums[i]);
            }
        }
    }
}
```
