善用ctrl + f:
* [二叉树的右视图](#二叉树的右视图)
* [缺失的第一个正数](#缺失的第一个正数)
* [合并区间](#合并区间)
* [下一个排列](#下一个排列)
* [反转链表2](#反转链表2)
* [螺旋数组](#螺旋数组)
* [接雨水](#接雨水)
* [括号生成](#括号生成)
* [前序和中序构建二叉树](#前序和中序构建二叉树)
* [相交链表](#相交链表)
* [二叉树的最近公共祖先](#二叉树的最近公共祖先)
* [买卖股票的最佳时机](#买卖股票的最佳时机)
* [三数之和](#三数之和)
* [LRU缓存机制](#LRU缓存机制)
* [数组中第K个最大元素](#数组中第K个最大元素)
* [翻转链表](#翻转链表)
* [K个一组翻转链表](#K个一组翻转链表)
* [二叉树锯齿形层序遍历](#二叉树锯齿形层序遍历)
* [无重复字串的最长字串](#无重复字串的最长字串)

# 二叉树的右视图
题目链接：https://leetcode-cn.com/problems/binary-tree-right-side-view/

思路：dfs进去就可以了~~~
```java
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> res = new LinkedList<Integer>();
        if (root == null) return res;
        dfs(root, 0, res);
        return res;
    }
    public void dfs(TreeNode root, int level, List<Integer> res) {
        if (res.size() <= level) {
            res.add(root.val);
        }
        if (root.right != null) dfs(root.right, level + 1, res);
        if (root.left != null) dfs(root.left, level + 1, res);
    }
}
```
二刷右视图
```java
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        if (root == null) {
            return new LinkedList<>();
        }
        List<Integer> res = new LinkedList<>();
        dfs(res, root, 1);
        return res;
    }
    public void dfs(List<Integer> res, TreeNode root, int level) {
        if (res.size() < level) {
            res.add(root.val);
        }
        if (root.right != null) {
            dfs(res, root.right, level + 1);
        }
        if (root.left != null) {
            dfs(res, root.left, level + 1);
        }
    }
}
```
# 缺失的第一个正数
题目链接: https://leetcode-cn.com/problems/first-missing-positive/

思路：相当于是数组本身当作一个桶。每次遍历i的时候将对应的nums[i]放到属于nums[i]的位置，反正是很精妙的~

```java
class Solution {
    public int firstMissingPositive(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
            while (nums[i] > 0 && nums[i] <= nums.length && nums[i] != i + 1 && nums[nums[i] - 1] != nums[i]) {
                swap(nums, i, nums[i] - 1);
            }
        }
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != i + 1) return i + 1;
        }
        return nums.length + 1;
    }

    public void swap(int[] nums, int x, int y) {
        int temp = nums[x];
        nums[x] = nums[y];
        nums[y] = temp;
    }
}
```
二刷的新解法
```java
class Solution {
    public int firstMissingPositive(int[] nums) {
        if (nums.length == 0) {
            return 1;
        }

        for (int i = 0; i < nums.length; i++) {
            if (nums[i] > 0 && nums[i] <= nums.length && nums[nums[i] - 1] != nums[i]) {
                int temp = nums[nums[i] - 1];
                nums[nums[i] - 1] = nums[i];
                nums[i] = temp;
                i--;
            }
        }

        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != i + 1) {
                return i + 1;
            }
        }
        
        return nums.length + 1;
    }
}
```
&nbsp;
# 合并区间
题目链接：https://leetcode-cn.com/problems/merge-intervals/

思路：就模拟吧~~ 注意一下这个toArray方法啊！
```java
class Solution {
    public int[][] merge(int[][] intervals) {
        LinkedList<int[]> res = new LinkedList<int[]>();
        Arrays.sort(intervals, (int[] a, int[] b) -> {
            return a[0] - b[0];
        });
        res.add(new int[]{intervals[0][0], intervals[0][1]});
        for (int i = 1; i < intervals.length; i++) {
            if (intervals[i][0] > res.get(res.size() - 1)[1]) {
                res.add(new int[]{intervals[i][0], intervals[i][1]});
            } else {
                res.get(res.size() - 1)[1] = Math.max(res.get(res.size() - 1)[1], intervals[i][1]);
            }
        }
        return res.toArray(new int[1][]);
    }
}
```
合并区间二刷
```java
class Solution {
    public int[][] merge(int[][] intervals) {
        if (intervals.length <= 1) {
            return intervals;
        }

        int[][] res = new int[intervals.length][2];
        
        Arrays.sort(intervals, (int[] a, int[] b) -> {
            return a[0] - b[0];
        });
        int count = 0;
        int end = intervals[0][1], start = intervals[0][0];
        for (int i = 1; i < intervals.length; i++) {
            if (intervals[i][0] <= end) {
                end = Math.max(end, intervals[i][1]);
            } else {
                res[count][0] = start; res[count][1] = end;
                start = intervals[i][0]; end = intervals[i][1];
                count++;
            }
        }
        res[count][0] = start; res[count][1] = end;
        int[][] ans = new int[count + 1][2];
        for (int i = 0; i <= count; i++) {
            ans[i][0] = res[i][0];
            ans[i][1] = res[i][1];
        }
        return ans;
    }
}
```
&nbsp;
# 下一个排列
题目链接：https://leetcode-cn.com/problems/next-permutation/

思路：从最后开始找到第一个非递增的数，然后再将其和严格大于它的数交换位置，然后从它的位置后面的数都交换位置。

估计没说清楚。 看看图：

![31](https://user-images.githubusercontent.com/57765968/114211261-fcc55d80-9992-11eb-822c-b1490e2a27bb.gif)

```java
class Solution {
    public void nextPermutation(int[] nums) {
        int i = nums.length - 1, temp = 0;
        int n = i, total = i;
        while (i > 0 && nums[i - 1] >= nums[i]) i--;
        if (i != 0) {
            i--;
            while (n > i && nums[n] <= nums[i]) n--;
            swap(nums, i, n);
        } else i--;
        i++;
        while (i < total) {
            swap(nums, i, total);
            i++; total--;
        }

    }
    public void swap(int[] nums, int x, int y) {
        int temp = nums[x];
        nums[x] = nums[y];
        nums[y] = temp;
    }
}
```
二刷，我好笨啊，写了个最笨的模拟
```java
class Solution {
    public void nextPermutation(int[] nums) {
        if (nums.length <= 1) {
            return;
        }

        int endValue = nums[nums.length - 1], startValue = -1, count = 2;
        int resEnd = 0;
        while (count <= nums.length) {
            for (int i = nums.length - count; i >= 0; i--) {
                if (nums[i] < endValue && i > startValue) {
                    startValue = i;
                    resEnd = nums.length - count + 1;
                    break;
                }
            }
            endValue = nums[nums.length - count];
            count++;
        }

        if (startValue == -1) {
            for (int i = 0; i < nums.length / 2; i++) {
                swap(nums, i, nums.length - i - 1);
            }
            return;
        }
        
        swap(nums, resEnd, startValue);

        Arrays.sort(nums, startValue + 1, nums.length);
        return;
    }
    

    public void swap(int[] nums, int x, int y) {
        int temp = nums[x];
        nums[x] = nums[y];
        nums[y] = temp;
    }
}
```

# 反转链表2
题目链接：https://leetcode-cn.com/problems/reverse-linked-list-ii/

思路：老暴力了，先new一个值的next记录一下头节点，然后就是一顿模拟了
```java
class Solution {
    public ListNode reverseBetween(ListNode head, int left, int right) {
        if (left == right || head.next == null) return head;
        ListNode ans = new ListNode(0, head);
        ListNode cur = ans, pre = null;
        for (int i = 0; i < left; i++) {
            pre = cur;
            cur = cur.next;
        }
        pre.next = null; // 清空 cur为链表开始
        int size = right - left;
        ListNode end = cur;
        for (int i = 1; i <= size; i++) end = end.next;

        ListNode tmp = end.next; // 最后一个节点
        end.next = null;
        ListNode p = reverse(cur);

        pre.next = p;
        cur.next = tmp;
        return ans.next;
    }
    
    public ListNode reverse(ListNode root) {
        ListNode cur = root, pre = null;
        while (cur != null) {
            ListNode next = cur.next;
            cur.next = pre;
            pre = cur;
            cur = next;
        }
        return pre;
    }
}
```
二刷，我好笨啊，呜呜呜呜
```java
class Solution {
    public ListNode reverseBetween(ListNode head, int left, int right) {
        if (left == right) {
            return head;
        }

        ListNode node = head, preLeft = null, leftNode = null, tailRight = null, rightNode = null;
        Boolean flag = false;
        for (int i = 1; ; i++) {
            if (i == left) {
                leftNode = node;
                flag = true;
            }
            if (!flag) {
                preLeft = node;
            }
            if (i == right) {
                rightNode = node;
            }
            node = node.next;
            if (rightNode != null) {
                tailRight = node;
                break;
            }
        }

        if (preLeft != null) {
            preLeft.next = null;
        }
        rightNode.next = null;

        change(leftNode);
        if (preLeft != null) {
            preLeft.next = rightNode;
        }
        leftNode.next = tailRight;

        if (left == 1) {
            return rightNode;
        }
        return head;
    }

    public void change(ListNode node) {
        ListNode pre = null;
        while (node != null) {
            ListNode temp = node.next;
            node.next = pre;
            pre = node;
            node = temp;
        }
    }
}
```
# 螺旋数组
题目链接：https://leetcode-cn.com/problems/spiral-matrix/

思路：又是一个模拟。
```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        int l = 0, r = matrix[0].length - 1, top = 0, down = matrix.length - 1;
        List<Integer> res = new LinkedList<Integer>();
        while (true) {
            for (int i = l; i <= r; i++) res.add(matrix[top][i]);
            top++;
            if (top > down) break;
            for (int i = top; i <= down; i++) res.add(matrix[i][r]);
            r--;
            if (l > r) break;
            for (int i = r; i >= l; i--) res.add(matrix[down][i]);
            down--;
            if (top > down) break;
            for (int i = down; i >= top; i--) res.add(matrix[i][l]);
            l++;
            if (l > r) break;
        }
        return res;
    }
}
```
二刷螺旋数组，oye 还是老方法好用
```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        int total = m * n, i = 0, j = 0, cur = 1;
        List<Integer> res = new ArrayList<>();
        while (cur <= total) {
            while (j < n && i < m && i >= 0 && j >= 0 && matrix[i][j] != -100) {
                res.add(matrix[i][j]);
                matrix[i][j] = -100;
                j++; cur++;
            }
            j--; i++;

            while (j < n && i < m && i >= 0 && j >= 0 && matrix[i][j] != -100) {
                res.add(matrix[i][j]);
                matrix[i][j] = -100;
                i++; cur++;
            }
            i--; j--;

            while (j < n && i < m && i >= 0 && j >= 0 && matrix[i][j] != -100) {
                res.add(matrix[i][j]);
                matrix[i][j] = -100;
                j--; cur++;
            }
            i--; j++;

            while (j < n && i < m && i >= 0 && j >= 0 && matrix[i][j] != -100) {
                res.add(matrix[i][j]);
                matrix[i][j] = -100;
                i--; cur++;
            }
            i++; j++;
        }
        return res;
    }
}
```
# 接雨水
题目链接：https://leetcode-cn.com/problems/trapping-rain-water/

思路：**第i个能接的水是左右最大值的最小值然后在减去原来的height[i]的值**
```java
class Solution {
    public int trap(int[] height) {
        if (height.length == 1 || height.length == 0) return 0; 
        int[] lmax = new int[height.length];
        lmax[0] = height[0]; 
        for (int i = 1; i < height.length; i++) lmax[i] = Math.max(lmax[i - 1], height[i]);

        int[] rmax = new int[height.length];
        rmax[height.length - 1] = height[height.length - 1];
        for (int i = height.length - 2; i >= 0; i--) rmax[i] = Math.max(rmax[i + 1], height[i]);

        int ans = 0;
        for (int i = 0; i < height.length; i++) {
            ans += Math.min(lmax[i], rmax[i]) - height[i];
        }
        return ans;
    }
}
```
二刷, 啊 我是废物
```java
class Solution {
    public int trap(int[] height) {
        if (height.length <= 1) {
            return 0;
        }

        int[] lmax = new int[height.length];
        int[] rmax = new int[height.length];
        lmax[0] = height[0];
        rmax[height.length - 1] = height[height.length - 1];
        for (int i = 1; i < height.length; i++) {
            lmax[i] = Math.max(lmax[i - 1], height[i]);
        }
        for (int i = height.length - 2; i >= 0; i--) {
            rmax[i] = Math.max(rmax[i + 1], height[i]);
        }

        int res = 0;
        for (int i = 0; i < height.length; i++) {
            res += Math.min(lmax[i], rmax[i]) - height[i];
        }
        return res;
    }
}
```
# 括号生成
题目链接：https://leetcode-cn.com/problems/generate-parentheses/

思路：暴力递归！！！
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
二刷，还是记下
```java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> res = new LinkedList<>();
        if (n == 0) {
            return res;
        }
        generate("", 0, 0, n, res);
        return res;
    }
    public void generate(String str, int left, int right, int total, List<String> res) {
        if (left == total && right == total) {
            res.add(str);
            return;
        }
        if (right > left) {
            return;
        }
        if (left < total) {
            generate(str + "(", left + 1, right, total, res);
        }
        if (right < total) {
            generate(str + ")", left, right + 1, total, res);    
        }
    }
}
```
&nbsp;
# 前序和中序构建二叉树
题目链接：https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/

思路：中序那个区间应该好弄，但是要注意的就是前序的那个区间，是用中序的数量来弄的。
```java
class Solution {
    HashMap<Integer, Integer> map = null;
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        map = new HashMap<Integer, Integer>();
        for (int i = 0; i < preorder.length; i++) map.put(inorder[i], i);
        return dfs(preorder, 0, preorder.length - 1, inorder, 0, preorder.length - 1);
    }

    public TreeNode dfs(int[] preorder, int lindex, int lend, int[] inorder, int rindex, int rend) {
        if (lindex > lend) return null;
        TreeNode root = new TreeNode(preorder[lindex]);
        int num = map.get(preorder[lindex]);
        root.left = dfs(preorder, lindex + 1, lindex + num - rindex, inorder, rindex, num - 1);
        root.right = dfs(preorder, lindex + num - rindex + 1, lend, inorder, num + 1, rend);
        return root;
    }
}
```
# 相交链表
题目链接：https://leetcode-cn.com/problems/intersection-of-two-linked-lists/

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode he = headA, she = headB;
        while (he != she) {
            he = he == null ? headB : he.next;
            she = she == null ? headA : she.next;
        }
        return she;
    }
}
```
二刷相交链表
```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if (headA == headB) {
            return headB;
        }
        ListNode nodeA = headA, nodeB = headB;

        while (nodeA != nodeB) {
            nodeA = nodeA == null ? headB : nodeA.next;
            nodeB = nodeB == null ? headA : nodeB.next;
        }

        return nodeA;
    }
}
```
&nbsp;
# 二叉树的最近公共祖先
题目链接：https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/

思路：递归好暴力啊！！！！ 什么LCA 什么tarjan都是浮云！！！
```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null || root == p || root == q) return root;
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if (left == null && right == null) return null;
        if (left == null) return right;
        if (right == null) return left;
        return root;
    }
}
```
二刷公共祖先, 还行
```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null || root == p || root == q) {
            return root;
        }
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if (left == null) {
            return right;
        }
        if (right == null) {
            return left;
        }
        return root;
    }
}
```
# 买卖股票的最佳时机
题目链接：https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/

思路：很多方法都能做的

```java
class Solution {
    public int maxProfit(int[] prices) {
        int ans = 0;
        if (prices.length == 0) return ans;
        int[] temp = new int[prices.length];
        for (int i = 1; i < prices.length; i++) {
            temp[i] = prices[i] - prices[i - 1];
            temp[i] += Math.max(temp[i - 1], 0);
            ans = Math.max(temp[i], ans);
        }
        return ans;
    }
}
```
另一种方法
```java
class Solution {
    public int maxProfit(int[] prices) {
        int ans = 0, temp = 0;
        if (prices.length == 0) return ans;
        for (int i = 1; i < prices.length; i++) {
            temp += prices[i] - prices[i - 1];
            if (temp < 0) temp = 0;
            ans = Math.max(temp, ans);
        }
        return ans;
    }
}
```
哦 二刷，我越来越废物了啊..........
```java
class Solution {
    public int maxProfit(int[] prices) {
        if (prices.length == 1) {
            return 0;
        }
        int res_ = 0, ans = 0;
        int[] res = new int[prices.length - 1];
        for (int i = 1; i < prices.length; i++) {
            res[i - 1] = prices[i] - prices[i - 1];
        }
        for (int i = 0; i < prices.length - 1; i++) {
            ans += res[i];
            if (ans < 0) {
                ans = 0;
            }
            res_ = res_ > ans ? res_ : ans;
        }
        return res_;
    }
}
```
&nbsp;
# 三数之和
题目链接：https://leetcode-cn.com/problems/3sum/

思路：大模拟，也没啥好要注意的感觉.........
```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        LinkedList<List<Integer>> res = new LinkedList<List<Integer>>();
        Arrays.sort(nums);
        if (nums.length < 3) return res;
        for (int i = 0; i < nums.length - 1; i++) {
            if (nums[i] > 0) break;
            if (i > 0 && nums[i] == nums[i - 1]) continue;
            int l = i + 1, r = nums.length - 1;
            while (l < r) {
                int sum = nums[i] + nums[l] + nums[r];
                if (sum == 0) {
                    List<Integer> temp = new LinkedList<Integer>();
                    temp.add(nums[i]);
                    temp.add(nums[l]);
                    temp.add(nums[r]);
                    res.add(temp);
                    while (l < r && nums[l] == nums[l + 1]) l++;
                    while (l < r && nums[r] == nums[r - 1]) r--;
                    l++; r--;
                } 
                else if (sum > 0) r--;
                else l++;
            }
        }
        return res;
    }
}
```
哎呀，艹了，就硬模拟，很烦
```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> res = new LinkedList<>();
        if (nums.length < 3) {
            return res;
        }
        
        Arrays.sort(nums);
        for (int i = 0; i < nums.length - 1; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            int start = i + 1, end = nums.length - 1;
            while (start < end) {
                if (nums[start] + nums[end] + nums[i] == 0) {
                    List<Integer> ans = new LinkedList<>();
                    ans.add(nums[i]);
                    ans.add(nums[start]);
                    ans.add(nums[end]);
                    res.add(ans);
                    while (start + 1 < nums.length && nums[start] == nums[start + 1]) {
                        start++;
                    }
                    while (end > 0 && nums[end] == nums[end - 1]) {
                        end--;
                    }
                    start++; end--;
                } else if (nums[start] + nums[end] + nums[i] < 0) {
                    while (start + 1 < nums.length && nums[start] == nums[start + 1]) {
                        start++;
                    }
                    start++;
                } else {
                    while (end > 0 && nums[end] == nums[end - 1]) {
                        end--;
                    }
                    end--;
                }
            }

        }
        return res;
    }
}
```
# LRU缓存机制
题目链接：https://leetcode-cn.com/problems/lru-cache/

思路：就是LRU算法。这个不会真的要被打=_=
```java
class LRUCache {
    int capacity = 0;
    HashMap<Integer, Integer> map = null;
    LinkedList<Integer> list = null;
    public LRUCache(int capacity) {
        this.capacity = capacity;
        map = new HashMap<Integer, Integer>();
        list = new LinkedList<Integer>();
    }
    
    public int get(int key) {
        if (map.containsKey(key)) {
            list.remove((Integer)key);
            list.addFirst(key);
            return map.get(key);
        }
        return -1;
    }
    
    public void put(int key, int value) {
        if (map.containsKey(key)) {
            list.remove((Integer)key);
            list.addFirst((Integer)key);
            map.put(key, value);
            return;
        }
        if (list.size() == capacity) {
            map.remove(list.getLast());
            list.removeLast();
            list.addFirst((Integer)key);
            map.put(key, value);
            return;
        }
        map.put(key, value);
        list.addFirst(key);
    }
}
```
我TM直接艹了，过了，好像又没过.......
```java
class LRUCache {
    public Map<Integer, Integer> cache;
    public List<Integer> list;
    public int capacity;

    public LRUCache(int capacity) {
        cache = new HashMap<>();
        this.capacity = capacity;
        list = new LinkedList<>();
    }
    
    public int get(int key) {
        if (cache.containsKey(key)) {
            list.remove((Integer) key);
            list.add(0, key);
            return cache.get(key);
        }
        return -1;
    }
    
    public void put(int key, int value) {
        if (cache.containsKey(key)) {
            cache.put(key, value);
            list.remove((Integer) key);
            list.add(0, key);
            return;
        }

        if (list.size() == this.capacity) {
            cache.remove(list.get(list.size() - 1));
            list.remove((int) list.size() - 1);
            cache.put(key, value);
            list.add(0, key);
            return;
        }
        
        cache.put(key, value);
        list.add(0, key);
    }
}
```
# 数组中第K个最大元素
题目链接：https://leetcode-cn.com/problems/kth-largest-element-in-an-array/

思路：反正觉着就挺常规的。堆排更简单
```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        return quicksearch(nums, 0, nums.length - 1, k - 1);
    }
    public int quicksearch(int[] nums, int l, int r, int k) {
        int mid = (l + r) >> 1;
        int x = nums[mid];
        int i = l, j = r;
        while (i <= j) {
            while (nums[i] > x) i++;
            while (nums[j] < x) j--;
            if (i <= j) {
                int temp = nums[i];
                nums[i] = nums[j];
                nums[j] = temp;
                i++; j--;
            }
        }
        if (k <= j) quicksearch(nums, l, j, k);
        else if (k >= i) quicksearch(nums, i, r, k);
        return nums[k];
    }
}
```
二刷，堆排和快排实现第K个最大元素
```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        return heapSort(nums, k);
    }

    public int find(int[] nums, int k, int l, int r) {
        int mid = (l + r) >> 1;
        int x = nums[mid];
        int i = l, j = r;
        while (i <= j) {
            while (nums[j] < x) {
                j--;
            }
            while (nums[i] > x) {
                i++;
            }
            if (i <= j) {
                int temp = nums[i];
                nums[i] = nums[j];
                nums[j] = temp;
                i++; j--;
            }
        }
        if (k >= i) {
            find(nums, k, i, r);
        }
        if (k <= j) {
            find(nums, k, l, j);
        }
        return nums[k];
    }

    public void swap(int[] nums, int x, int y) {
        int temp = nums[x];
        nums[x] = nums[y];
        nums[y] = temp;
    }

    public void shiftDown(int k, int[] nums, int total) {
        boolean flag = false;
        int res = 0;
        while (k * 2 < total && flag == false) {
            res = nums[k] > nums[k * 2] ? k * 2 : k;
            if (k * 2 + 1 < total) {
                res = nums[res] > nums[k * 2 + 1] ? k * 2 + 1 : res;
            }
            if (k == res) {
                flag = true;
            } else {
                swap(nums, res, k);
                k = res;
            }
        } 
    }

    public void construct(int[] nums, int k) {
        for (int i = k / 2; i >= 0; i--) {
            shiftDown(i, nums, k);
        }
    }

    public int heapSort(int[] nums, int k) {
        construct(nums, k);
        for (int i = k; i < nums.length; i++) {
            if (nums[0] < nums[i]) {
                nums[0] = nums[i];
                shiftDown(0, nums, k);
            }
        }
        return nums[0];
    }
}
```
# 翻转链表
题目链接：https://leetcode-cn.com/problems/reverse-linked-list/

应该是面试答的好，面试官配合一下搞出的这个.........
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode pre = null, cur = head;
        while (cur != null) {
            ListNode next = cur.next;
            cur.next = pre;
            pre = cur;
            cur = next;
        }
        return pre;
    }
}
```
二刷反转链表
```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode pre = null;
        while (head != null) {
            ListNode temp = head.next;
            head.next = pre;
            pre = head;
            head = temp;
        }
        return pre;
    }
}
```
&nbsp;
# K个一组翻转链表
题目链接：https://leetcode-cn.com/problems/reverse-nodes-in-k-group/

思路：就是一通乱模拟，注意end那个找到尾节点的时候要记住设置next为null。不然会没法找的
```java
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        ListNode first = new ListNode(0);
        ListNode end = first, res = first;
        end.next = head;
        while (true) {
            for (int i = 0; i < k && end != null; i++) end = end.next;
            if (end == null) break;
            ListNode next = end.next;
            end.next = null;
            ListNode pre = first.next;
            ListNode tmp = reverse(first.next); 
            first.next = tmp;
            pre.next = next;
            first = pre;
            end = pre;
        }
        return res.next;

    }
    public ListNode reverse(ListNode root) {
        ListNode cur = root, pre = null;
        while (cur != null) {
            ListNode next = cur.next;
            cur.next = pre;
            pre = cur;
            cur = next;
        }
        return pre;
    }
}
```
二刷K个一组翻转链表
```java
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        ListNode res = new ListNode(0), node = res;
        res.next = head;
        while (true) {
            boolean flag = false;
            ListNode pre = node, start = node.next, end = null, tailNode = null;
            for (int i = 1; i <= k; i++) {
                node = node.next;
                if (node == null) {
                    flag = true; break;
                }
            }
            if (flag) {
                break;
            }
            end = node;
            tailNode = end.next;
            end.next = null;
            reverse(start);
            pre.next = end;
            start.next = tailNode;
            node = start;
        }
        return res.next;
    }

    public void reverse(ListNode node) {
        ListNode pre = null;
        while (node != null) {
            ListNode tmp = node.next;
            node.next = pre;
            pre = node;
            node = tmp;
        }
    }
}
```
&nbsp;

# 二叉树锯齿形层序遍历
题目链接：https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/

思路：递归，这里我开始想的优点麻烦了，主要不知道res里面的List数组怎么加，其实就加上一个空就行了啊！！！！ 然后我们在去空数组里面添加数据就行了 yes！

这样写吧........
```java
public class temp {
    public static class TreeNode {
        TreeNode left;
        TreeNode right;
        int val;

        public TreeNode(int val) {
            this.val = val;
        }
    }

    public static void main(String[] args) {
        TreeNode root = new TreeNode(1);
        root.left = new TreeNode(3);
        root.right = new TreeNode(2);
        TreeNode rootLeft = root.left;
        TreeNode rootRight = root.right;
        rootLeft.left = new TreeNode(5);
        rootLeft.right = new TreeNode(8);
        rootRight.left = new TreeNode(11);
        List<List<Integer>> res = new LinkedList<>();
        dfs(0, root, res);

        for (int i = 0; i < res.size(); i++) {
            for (int j = 0; j < res.get(i).size(); j++) {
                System.out.print(res.get(i).get(j) + " ");
            }
            System.out.println();
        }
    }

    public static void dfs(int level, TreeNode node, List<List<Integer>> res) {
        if (node == null) {
            return;
        }

        if (res.size() <= level) {
            res.add(new LinkedList<>());
        }

        if (level % 2 == 0) {
            res.get(level).add(node.val);
        } else {
            res.get(level).add(0, node.val);
        }

        dfs(level + 1, node.left, res);
        dfs(level + 1, node.right, res);
    }
}
```
下面的ans好多余啊.......受不了
```java
class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        LinkedList<List<Integer>> res = new LinkedList<List<Integer>>();
        if (root == null) return res;
        LinkedList<Integer> ans = null;
        dfs(res, ans, 0, root);
        return res;
    }
    public void dfs(LinkedList<List<Integer>> res, LinkedList<Integer> ans, int level, TreeNode node) {
        if (res.size() == level) {
            ans = new LinkedList<Integer>();
            res.add(ans);
        }
        if (level % 2 == 0) {
            res.get(level).add(node.val);
        } else {
            res.get(level).add(0, node.val);
        }
        if (node.left != null) dfs(res, ans, level + 1, node.left);
        if (node.right != null) dfs(res, ans, level + 1, node.right);
    }
}
```

在贴一下非递归的解法，这思路就是奇数偶数添加在前还是在后的问题了啊！！！！
```java
class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        LinkedList<List<Integer>> res = new LinkedList<List<Integer>>();
        if (root == null) return res;
        LinkedList<TreeNode> ans = new LinkedList<TreeNode>();
        ans.add(root);
        int count = 0;
        while (true) {
            int size = ans.size();
            LinkedList<Integer> temp = new LinkedList<Integer>();
            if (size == 0) break;
            for (int i = 0; i < size; i++) {
                TreeNode tmp = ans.poll();
                if (count % 2 == 0) temp.add(tmp.val);
                else temp.add(0, tmp.val);
                
                if (tmp.left != null) ans.add(tmp.left);
                if (tmp.right != null) ans.add(tmp.right);
            }
            count++;
            res.add(temp);
        }
        return res;
    }
}
```

# 无重复字串的最长字串

题目链接：https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/

思路：暴力！！！！
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int[] book = new int[500];
        int res = 0, ans = 0;
        for (int i = 0; i < s.length(); i++) {
            if (book[s.charAt(i) - ' '] == 0) {
                res++;
                book[s.charAt(i) - ' '] = i + 1;
            } else {
                if (i + 1 - book[s.charAt(i) - ' '] > res) {
                    res++;
                } else {
                    res = i - book[s.charAt(i) - ' '] + 1;
                }
                book[s.charAt(i) - ' '] = i + 1;
            }
            ans = Math.max(ans, res);
        }
        return ans;

    }
}
```
二刷无重复字串的最长字串
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        if (s.length() == 0) {
            return 0;
        }
        
        int res = 0, ans = 0;
        int[] flag = new int[200];
        for (int i = 0; i < s.length(); i++) {
            if (flag[s.charAt(i) - ' ' + 32] == 0) {
                flag[s.charAt(i) - ' ' + 32] = i + 1;
                ans++;
            } else if (i + 1 - flag[s.charAt(i) - ' ' + 32] <= ans) {
                ans = i + 1 - flag[s.charAt(i) - ' ' + 32];
            } else {
                ans++;
            }
            flag[s.charAt(i) - ' ' + 32] = i + 1;
            res = res > ans ? res : ans;
        }
        return res;
    }
}
```
