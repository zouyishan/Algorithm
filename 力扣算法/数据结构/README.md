# 链表的两数相加

题目链接：https://leetcode-cn.com/problems/add-two-numbers/

思路：要回想一下链表的基础操作。同时记住这种按位加法！

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode head = null, tail = null;
        int carry = 0;
        while (l1 != null || l2 != null) {
            int l1_value = 0, l2_value = 0;
            if (l1 != null) l1_value = l1.val;
            if (l2 != null) l2_value = l2.val;
            int sum = l2_value + l1_value + carry;
            if (head == null && tail == null) {
                head = tail = new ListNode(sum % 10);
            } else {
                tail.next = new ListNode(sum % 10);
                tail = tail.next;
            }
            carry = sum / 10;
            if (l1 != null) l1 = l1.next;
            if (l2 != null) l2 = l2.next;
        }
        if (carry > 0) tail.next = new ListNode(carry);
        return head;
    }
}
```
&nbsp;
&nbsp;
# * K个一组翻转链表

链接：https://leetcode-cn.com/problems/reverse-nodes-in-k-group/

思路：有头插法和尾插法，重点理解这题:
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;

        ListNode pre = dummy;
        ListNode end = dummy;

        while (end.next != null) {
            for (int i = 0; i < k && end != null; i++) end = end.next;
            if (end == null) break;
            ListNode start = pre.next;
            ListNode next = end.next;
            end.next = null;
            pre.next = reverse(start);
            start.next = next;
            pre = start;

            end = pre;
        }
        return dummy.next;
    }

    private ListNode reverse(ListNode head) {
        ListNode pre = null;
        ListNode curr = head;
        while (curr != null) {
            ListNode next = curr.next;
            curr.next = pre;
            pre = curr;
            curr = next;
        }
        return pre;
    }
}
```

# 二叉树的右视图

题目链接：https://leetcode-cn.com/problems/binary-tree-right-side-view/

思路：
* 可以通过dfs，先搜索右边的就是了！！
* 可以通过bfs，层序遍历找最后一个元素就可以了！！！

聊几个api，
* LinkedList.poll()出栈第一个数，如果没有则是null
* LinkedList.offer()添加在末尾
* LinkedList.add()添加在末尾
* LinkedList.remove()移除最后一个元素
* LinkedList.get(size() - 1)获取最后一个元素

注意：
* LinkedList本身就是双向链表。通过add在后面增加元素，通过remove删除最前面的元素。

### DFS版
```java
class Solution {
    List<Integer> res = new LinkedList<Integer>();
    public List<Integer> rightSideView(TreeNode root) {
        dfs(root, 0);
        return res;
    }
    private void dfs(TreeNode treeNode, int level) {
        if (treeNode == null) return;

        if (level == res.size()) res.add(treeNode.val);

        if (treeNode.right != null) dfs(treeNode.right, level + 1);
        if (treeNode.left != null) dfs(treeNode.left, level + 1);
        
    }
}
```

### BFS版
```java
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        Queue<TreeNode> list = new LinkedList<TreeNode>();
        List<Integer> res = new LinkedList<Integer>();
        list.add(root);
        while (!list.isEmpty()) {
            int size = list.size();
            for (int i = 1; i <= size; i++) {
                TreeNode temp = list.poll();
                if (temp == null) break;
                if (i == size) res.add(temp.val);
                if (temp.left != null) list.add(temp.left);
                if (temp.right != null) list.add(temp.right); 
            }
        }
        return res;
    }
}
```
&nbsp;
&nbsp;
# 二叉树的锯齿形层次遍历

题目链接：https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/

思路：注意使用双端队列！

### BFS版本
```java
class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> res = new LinkedList<List<Integer>>();
        if (root == null) {
            return res;
        }
        LinkedList<TreeNode> list = new LinkedList<TreeNode>();
        list.add(root);
        boolean flag = true;
        while (!list.isEmpty()) {
            int size = list.size();
            LinkedList<Integer> ans = new LinkedList<Integer>();
            for (int i = 1; i <= size; i++) {
                TreeNode treeNode = list.poll();
                if (flag) {
                    ans.add(treeNode.val);
                } else {
                    ans.addFirst(treeNode.val);
                }
                if (treeNode.left != null) list.add(treeNode.left);
                if (treeNode.right != null) list.add(treeNode.right);
            }
            res.add(ans);
            flag = !flag;
        }
        return res;
    }
}
```

### DFS
```java
class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> res = new LinkedList<List<Integer>>();
        dfs(root, res, 0);
        return res;
    }
    public void dfs(TreeNode cur, List<List<Integer>> ans, int level) {
        if (cur == null) return;
        // 是否new了
        if (ans.size() <= level) {
            List<Integer> list = new LinkedList<Integer>();
            ans.add(list);
        }

        // 是奇数的话就反序插入
        if (level % 2 != 0) {
            ans.get(level).add(0, cur.val);
        } else {
            ans.get(level).add(cur.val);
        }
        dfs(cur.left, ans, level + 1);
        dfs(cur.right, ans, level + 1);
    }
}
```
&nbsp;
&nbsp;

# 有序链表的合并
简单题吧==

题目链接：https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/

思路：同归并排序一样的排序方式！

```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode res = new ListNode(0);
        ListNode cur = res;
        while (l1 != null && l2 != null) {
            if (l1.val < l2.val) {
                cur.next = l1; l1 = l1.next;
            } else {
                cur.next = l2; l2 = l2.next;
            }
            cur = cur.next;
        }
        while (l1 != null) {
            cur.next = l1; l1 = l1.next;
            cur = cur.next;
        }
        while (l2 != null) {
            cur.next = l2; l2 = l2.next;
            cur = cur.next;
        }
        return res.next;
    }
}
```

# 两个栈实现队列

链接：https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/

思路：stack1用来push用来添加到队尾，stack2用来pop，用来移除最后一个元素。

```java
class CQueue {
    private Stack<Integer> stack1 = null;
    private Stack<Integer> stack2 = null;
    public CQueue() {
        stack1 = new Stack<Integer>();
        stack2 = new Stack<Integer>();
    }
    
    public void appendTail(int value) {
        stack1.push(value);
    }
    
    public int deleteHead() {
        if (stack2.isEmpty()) {
            while (!stack1.isEmpty()) {
                stack2.push(stack1.pop());
            }
        }
        if (stack2.isEmpty()) return -1;

        else return stack2.pop();
    }
}

/**
 * Your CQueue object will be instantiated and called as such:
 * CQueue obj = new CQueue();
 * obj.appendTail(value);
 * int param_2 = obj.deleteHead();
 */
```
&nbsp;
&nbsp;
# 二叉树镜像
题目：https://leetcode-cn.com/problems/er-cha-shu-de-jing-xiang-lcof/

思路：递归，左边等于右边，右边等于左边，递归下去就能成！
```java
class Solution {
    public TreeNode mirrorTree(TreeNode root) {
        if (root == null) return null;
        TreeNode temp = root.left;
        root.left = mirrorTree(root.right);
        root.right = mirrorTree(temp); 
        return root;
    }
}
```
