目录：

[链表的两数相加](#链表的两数相加)

[K个一组翻转链表](#K个一组翻转链表)

[二叉树的右视图](#二叉树的右视图)

[二叉树的锯齿形层次遍历](#二叉树的锯齿形层次遍历)

[有序链表的合并](#有序链表的合并)

[两个栈实现队列](#两个栈实现队列)

[二叉树镜像](#二叉树镜像)

[相交链表](#相交链表)

[重建二叉树](#重建二叉树)

[栈的压入弹出序列](#栈的压入弹出序列)

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
# K个一组翻转链表

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

**聊几个api**，
* LinkedList.poll()出栈第一个数，如果没有则是null
* LinkedList.remove()移除最后一个元素

* LinkedList.add()添加在末尾
* LinkedList.addFirst()添加在最前面
* LinkedList.add(0, value)添加在最前面

* LinkedList.getFrist()获取前面一个元素
* LinkedList.getLast()获取最后一个元素

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

# 相交链表
一道伤心的题

链接：https://leetcode-cn.com/problems/intersection-of-two-linked-lists/

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if (headA == null || headB == null) return null;
        ListNode you = headA, she = headB;
        while (you != she) {    // 若是有缘，你们早晚会相遇
            you = you == null ? headB : you.next;   // 当你走到终点时，开始走她走过的路
            she = she == null ? headA : she.next;   // 当她走到终点时，开始走你走过的路
        }
        // 如果你们喜欢彼此，请携手一起走完剩下的旅程（将下面这个 while 块取消注释）。
        // 一路上，时而你踩着她的影子，时而她踩着你的影子。渐渐地，你变成了她，她也变
        // 成了你。
        /* while (she != null) {
            you = she->next;
            she = you->next;
        } */
        // 但终究，她为了选择正确的道路。就此返回
        // 但你任然驻足在那个突然闯进你生活，给你带来快乐的，和她相遇的原地。
        // 等着被清除。
        you = null; // Help GC
        return she;
    }
}
```

# 重建二叉树

题意：通过前序和中序来重建后序

题目连接：https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof/

```java
class Solution {
    private Map<Integer, Integer> indexMap;
    public TreeNode myBuildTree(int[] preorder, int[] inorder, int preorder_left, int preorder_right, int inorder_left, int inorder_right) {
        if (preorder_left > preorder_right) {
            return null;
        }
        // 前序遍历中的第一个节点就是根节点
        int preorder_root = preorder_left;
        
        // 在中序遍历中定位根节点
        int inorder_root = indexMap.get(preorder[preorder_root]);
        
        // 先把根节点建立出来
        TreeNode root = new TreeNode(preorder[preorder_root]);
        // 得到左子树中的节点数目
        int size_left_subtree = inorder_root - inorder_left;
        // 递归地构造左子树，并连接到根节点
        // 先序遍历中「从 左边界+1 开始的 size_left_subtree」个元素就对应了中序遍历中「从 左边界 开始到 根节点定位-1」的元素
        root.left = myBuildTree(preorder, inorder, preorder_left + 1, preorder_left + size_left_subtree, inorder_left, inorder_root - 1);
        // 递归地构造右子树，并连接到根节点
        // 先序遍历中「从 左边界+1+左子树节点数目 开始到 右边界」的元素就对应了中序遍历中「从 根节点定位+1 到 右边界」的元素
        root.right = myBuildTree(preorder, inorder, preorder_left + size_left_subtree + 1, preorder_right, inorder_root + 1, inorder_right);
        return root;
    }
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        int n = preorder.length;
        // 构造哈希映射，帮助我们快速定位根节点
        indexMap = new HashMap<Integer, Integer>();
        for (int i = 0; i < n; i++) {
            indexMap.put(inorder[i], i);
        }
        return myBuildTree(preorder, inorder, 0, n - 1, 0, n - 1);
    }
}
```

# 栈的压入弹出序列

题目链接：https://leetcode-cn.com/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof/

思路：觉着最近算法越来越拉跨了,哎.............思路就是模拟pop和push吧，一个push，一个用while来pop就行了。

**栈stack的常见api**：
pop: 弹出栈顶元素

push：压入一个元素

peek：得到顶部的值，但是不会弹栈。
```java
class Solution {
    public boolean validateStackSequences(int[] pushed, int[] popped) {
        Stack<Integer> stack = new Stack<>();
        int i = 0;
        for(int num : pushed) {
            stack.push(num); // num 入栈
            while(!stack.isEmpty() && stack.peek() == popped[i]) { // 循环判断与出栈
                stack.pop();
                i++;
            }
        }
        return stack.isEmpty();
    }
}
```
