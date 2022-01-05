# 链表排序
https://leetcode-cn.com/problems/sort-list/
```java
class Solution {
    public ListNode sortList(ListNode head) {
        return sortList(head, null);
    }

    public ListNode sortList(ListNode head, ListNode tail) {
        if (head == null) {
            return head;
        }
        if (head.next == tail) {
            head.next = null;
            return head;
        }
        ListNode slow = head, fast = head;
        while (fast != tail) {
            slow = slow.next;
            fast = fast.next;
            if (fast != tail) {
                fast = fast.next;
            }
        }
        ListNode mid = slow;
        ListNode list1 = sortList(head, mid);
        ListNode list2 = sortList(mid, tail);
        ListNode sorted = merge(list1, list2);
        return sorted;
    }

    public ListNode merge(ListNode head1, ListNode head2) {
        ListNode dummyHead = new ListNode(0);
        ListNode temp = dummyHead, temp1 = head1, temp2 = head2;
        while (temp1 != null && temp2 != null) {
            if (temp1.val <= temp2.val) {
                temp.next = temp1;
                temp1 = temp1.next;
            } else {
                temp.next = temp2;
                temp2 = temp2.next;
            }
            temp = temp.next;
        }
        if (temp1 != null) {
            temp.next = temp1;
        } else if (temp2 != null) {
            temp.next = temp2;
        }
        return dummyHead.next;
    }
}
```
# 格雷编码
https://leetcode-cn.com/problems/gray-code/
```java
class Solution {
    public List<Integer> grayCode(int n) {
        List<Integer> res = new ArrayList<Integer>() {{ add(0); }};
        int head = 1;
        for (int i = 0; i < n; i++) {
            for (int j = res.size() - 1; j >= 0; j--) // 这里很妙啊，前面的所有加上 head << 1，而且必须是从后往前找的！！！
                res.add(head + res.get(j));
            head <<= 1;
        }
        return res;
    }
}
```
# 不同路径
https://leetcode-cn.com/problems/unique-paths/

hh 这个要学学
```java
class Solution {
    public int uniquePaths(int m, int n) {
        int[][] res = new int[m][n];
        for (int i = 0; i < m; i++) {
            res[i][0] = 1;
        }
        for (int j = 0; j < n; j++) {
            res[0][j] = 1;
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                res[i][j] = res[i - 1][j] + res[i][j - 1];
            }
        }
        return res[m - 1][n - 1];
    }
}
```
这个都没学会，拿来的熊心豹子胆敢去面试的
```java
class Solution {
    public int uniquePaths(int m, int n) {
        int[][] res = new int[m][n];

        for (int i = 0; i < m; i++) {
            res[i][0] = 1;
        }

        for (int i = 0; i < n; i++) {
            res[0][i] = 1;
        }

        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                res[i][j] += res[i - 1][j] + res[i][j - 1];
            }
        }
        return res[m - 1][n - 1];
    }
}
```
# 旋转链表
https://leetcode-cn.com/problems/rotate-list/
```java
class Solution {
    public static ListNode resverse(ListNode head) {
        ListNode pre = head;
        int tmp_val = head.val, tmp = 0;
        while (head.next != null) {
            ListNode next = head.next;
            tmp = next.val;
            next.val = tmp_val;
            tmp_val = tmp;
            head = head.next;
        }
        pre.val = tmp_val;
        return pre;
    }

    public ListNode rotateRight(ListNode head, int k) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode pre = head;
        int count = 0;
        while (head != null) {
            head = head.next;
            count++;
        }
        k = k % count;
        for (int i = 0; i < k; i++) {
            pre = resverse(pre);
        }
        return pre;
    }
}
```
这个大模拟了......二刷了
```java
class Solution {
    public static int level = 0;
    public ListNode rotateRight(ListNode head, int k) {
        if (k == 0 || head == null || head.next == null) {
            return head;
        }

        level = 0;
        ListNode pos = null;
        dfs(head, pos, k);
        if (pos == null) {
            k %= level;
            if (k == 0) {
                return head;
            }
            ListNode pre = null;
            pos = head;
            for (int i = 0; i < level - k; i++) {
                pre = pos;
                pos = pos.next;
            }
            pre.next = null;
        }
        getLast(pos).next = head;
        return pos;
    }

    public void dfs(ListNode root, ListNode pos, int k) {
        if (root == null) {
            return;
        }
        dfs(root.next, pos, k);
        level++;
        if (level == k) {
            pos = root;
        }
    }

    public ListNode getLast(ListNode node) {
        while (node.next != null) {
            node = node.next;
        }
        return node;
    }
}
```
# 螺旋矩阵 II
https://leetcode-cn.com/problems/spiral-matrix-ii/

emmmm 就这, 跟着思路模拟就行
```java
class Solution {
    public int[][] generateMatrix(int n) {
        int[][] res = new int[n][n];
        int i = 1, x = 0, y = 0;
        while (i <= n * n) {
            while (y < n && res[x][y] == 0) res[x][y++] = i++;
            y--; x++;
            // System.out.println(x + " " + y);
            while (x < n && res[x][y] == 0) res[x++][y] = i++;
            x--; y--;
            // System.out.println(x + " " + y);
            while (y >= 0 && res[x][y] == 0) res[x][y--] = i++;
            y++; x--;
            // System.out.println(x + " " + y);
            while (x >= 0 && res[x][y] == 0) res[x--][y] = i++;
            x++; y++;
            // System.out.println(x + " " + y);
        }
        return res;
    }
}
```

# 二叉搜索树中第K小的元素
https://leetcode-cn.com/problems/kth-smallest-element-in-a-bst/

主要思路就是通过中序遍历找到第k小。注意这里的static不能直接从零开始，要对其进行赋值，不然，肯定没！！！
```java
class Solution {
    public static int count = 0;
    public static int get_res(TreeNode root, int k) {
        if (root == null) return 0;
        int left = get_res(root.left, k);
        if (0 == count) {
            return left;
        }
        count--;
        if (0 == count) {
            return root.val;
        }
        int right = get_res(root.right, k);
        if (right == 0) return left;
        if (left == 0) return right;
        return 0;
    }

    public int kthSmallest(TreeNode root, int k) {
        count = k;
        return get_res(root, k);
    }
}
```
二刷，还行吧, 现在有感觉了，这么简单的题，当时居然不会做，顶不住
```java
class Solution {
    public static int count = 0, res = 0;
    public int kthSmallest(TreeNode root, int k) {
        count = 0;
        res = 0;
        dfs(root, k);

        return res;
    }
    
    public void dfs(TreeNode root, int k) {
        if (root != null) {
            dfs(root.left, k);
            count++;
            if (count == k) {
                res = root.val;
            }
            dfs(root.right, k);
        }
    }
}
```
# 两数相加
https://leetcode-cn.com/problems/add-two-numbers/
```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode now = new ListNode(0);
        ListNode res = now;
        Integer carry = 0;
        while (l1 != null || l2 != null) {
            Integer c = 0;
            if (l1 != null) {
                c += l1.val;
            }
            if (l2 != null) {
                c += l2.val;
            }
            c += carry;
            carry = c / 10;
            c %= 10;
            now.next = new ListNode(c);
            now = now.next;
            if (l1 != null) l1 = l1.next;
            if (l2 != null) l2 = l2.next;
        }
        if (carry != 0) now.next = new ListNode(carry);

        return res.next;
    }
}
```
# 字符串转整数
超级大模拟了属于是 

https://leetcode-cn.com/problems/string-to-integer-atoi/
```c
class Solution {
    public static int check(long res, long fushu) {
        if (fushu == 1) {
            res = -res;
            if (res < (1 << 31)) return 1 << 31;
        } else {
            if (res > (1 << 31) - 1) return (1 << 31) - 1;
        }
        return 0;
    }
    public int myAtoi(String s) {
        long flag = 0, res = 0, fushu = 0;
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == ' ' && flag == 0) continue; 
            if ((s.charAt(i) >= 'a' && s.charAt(i) <= 'z') || (s.charAt(i) >= 'A' && s.charAt(i) <= 'Z') || (flag == 1 && s.charAt(i) == ' ') || (s.charAt(i) == '.') || (flag == 1 && (s.charAt(i) == '+' || s.charAt(i) == '-'))) {
                if (fushu == 1) res = -res;
                return (int)res;
            }
            if (s.charAt(i) == '-' && flag == 0) {
                flag = 1;
                fushu = 1; 
                continue;
            }
            if (s.charAt(i) == '+' && flag == 0) {
                flag = 1;
                fushu = 0; 
                continue;
            }
            
            if (s.charAt(i) >= '0' || s.charAt(i) <= '9') {
                flag = 1;
                int z = 0;
                res *= 10;
                res += (s.charAt(i) - '0');
                if ((z = check(res, fushu)) != 0) {
                    return z;
                }
            }
        }
        if (fushu == 1) res = -res;
        return (int)res;
    }
}
```
# 盛水最多的容器
https://leetcode-cn.com/problems/container-with-most-water/
```c
class Solution {
    public static int max_(int l, int r) {
        if (l > r) return l;
        return r;
    }
    public static int min_(int l, int r) {
        if (l > r) return r;
        return l;
    }

    public int maxArea(int[] height) {
        int l = 0, r = height.length - 1, res = 0;

        while (l < r) {
            res = max_(res, (r - l) * min_(height[l], height[r]));
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
真的菜啊.....真不敢相信我的代码居然能写成上面这样
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
# 最接近的三数之和
https://leetcode-cn.com/problems/3sum-cl
