善用ctrl + f:

* [买卖股票的最佳时机](#买卖股票的最佳时机)
* [三数之和](#三数之和)
* [LRU缓存机制](#LRU缓存机制)
* [数组中第K个最大元素](#数组中第K个最大元素)
* [翻转链表](#翻转链表)
* [K个一组翻转链表](#K个一组翻转链表)
* [二叉树锯齿形层序遍历](#二叉树锯齿形层序遍历)
* [无重复字串的最长字串](#无重复字串的最长字串)

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
&nbsp;

# 二叉树锯齿形层序遍历
题目链接：https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/

思路：递归，这里我开始想的优点麻烦了，主要不知道res里面的List数组怎么加，其实就加上一个空就行了啊！！！！ 然后我们在去空数组里面添加数据就行了 yes！

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
