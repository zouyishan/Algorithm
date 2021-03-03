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
