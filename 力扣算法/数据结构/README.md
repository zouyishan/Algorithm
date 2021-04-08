目录(善用ctrl + f)：
* [重排链表](#重排链表)
* [二叉搜索树的后序遍历](#二叉搜索树的后序遍历)
* [环形链表2](#环形链表2)
* [环形链表](#环形链表)
* [前k个高频元素](#前k个高频元素)
* [最近公共祖先](#最近公共祖先)
* [中序遍历](#中序遍历)
* [反转链表](#反转链表)
* [合并二叉树](#合并二叉树)
* [二叉树最大深度](#二叉树最大深度)
* [树的直径](#树的直径)
* [链表的两数相加](#链表的两数相加)
* [K个一组翻转链表](#K个一组翻转链表)
* [二叉树的右视图](#二叉树的右视图)
* [二叉树的锯齿形层次遍历](#二叉树的锯齿形层次遍历)
* [有序链表的合并](#有序链表的合并)
* [两个栈实现队列](#两个栈实现队列)
* [二叉树镜像](#二叉树镜像)
* [相交链表](#相交链表)
* [重建二叉树](#重建二叉树)
* [栈的压入弹出序列](#栈的压入弹出序列)
* [链表中倒数第k个节点](#链表中倒数第k个节点)
* [链表中是否存在环](#链表中是否存在环)
* [LRU](#LRU)
&nbsp;

# 重排链表
题目连接：https://leetcode-cn.com/problems/reorder-list/

思路：好像就是一个简单得模拟，但是我不知道我得脑子犯抽了还是什么，就是想不出个很好的模拟解法。慢慢来
```java
class Solution {
    public void reorderList(ListNode head) {
        if (head == null) return;
        ArrayList<ListNode> res = new ArrayList<ListNode>();
        ListNode node = head;
        while (node != null) {
            res.add(node);
            node = node.next;
        }

        int i = 0, j = res.size() - 1;
        while (i < j) {
            res.get(i).next = res.get(j);
            i++;
            if (i == j) break;
            res.get(j).next = res.get(i);
            j--;
        }
        res.get(i).next = null;

    }
}
```

# 二叉搜索树的后序遍历
题目链接：https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/

思路：通过后续遍历和二叉搜索树的性质来做，太秒了！！！
```java
class Solution {
    public boolean verifyPostorder(int[] postorder) {
        return recur(postorder, 0, postorder.length - 1);
    }
    boolean recur(int[] postorder, int i, int j) {
        if(i >= j) return true;
        int p = i;
        while(postorder[p] < postorder[j]) p++;
        int m = p;
        while(postorder[p] > postorder[j]) p++;
        return p == j && recur(postorder, i, m - 1) && recur(postorder, m, j - 1);
    }
}
```
# 环形链表
题目链接：https://leetcode-cn.com/problems/linked-list-cycle/

思路：快慢指针啦！！！
```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        if (head == null || head.next == null) return false;
        ListNode slow = head, fast = head;
        while (true) {
            if (fast == null || fast.next == null) return false;
            slow = slow.next;
            fast = fast.next.next;
            if (slow == fast) return true;
        }
    }
}
```
# 环形链表2
题目链接；https://leetcode-cn.com/problems/linked-list-cycle-ii/

思路：当快指针和慢指针相遇了以后，然后再移动慢指针和头指针，相遇的那个点就是入口点了！！！！
```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode slow = head, quick = head;
        while (quick != null) {
            slow = slow.next;
            if (quick.next != null && quick != null) {
                quick = quick.next.next;
            } else return null;
            if (slow == quick) {
                while (head != slow) {
                    head = head.next;
                    slow = slow.next;
                }
                return slow;
            }
        }
        return null;
    }
}
```
# 前k个高频元素
题目链接：https://leetcode-cn.com/problems/top-k-frequent-elements/

思路：通过优先队列和哈希表来做整理,了解一下使用
```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
        for (int num : nums) {
            if (map.containsKey(num)) {
                map.put(num, map.get(num) + 1);
            } else map.put(num, 1);
        }
        PriorityQueue<Integer> pq = new PriorityQueue<Integer>((v1, v2) -> map.get(v1) - map.get(v2));
        for (Integer a : map.keySet()) {
            if (pq.size() < k) pq.add(a);
            else if (map.get(a) > map.get(pq.peek())) {
                pq.remove();
                pq.add(a);
            }
        }
        int[] res = new int[k];
        int count = 0;
        while (!pq.isEmpty()) {
            res[count++] = pq.remove();
        }
        return res;
    }
}
```
&nbsp;
# 最近公共祖先
题目链接：https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/

思路：递归求解，太妙了！！！！！
```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null || root == p || root == q) return root;
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if(left == null && right == null) return null;
        if(left == null) return right;
        if(right == null) return left;
        return root;
    }
}
```
# 中序遍历
题目链接：https://leetcode-cn.com/problems/binary-tree-inorder-traversal/

思路：我们用非递归的方法来做
```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new LinkedList<Integer>();
        LinkedList<TreeNode> ans = new LinkedList<TreeNode>();
        TreeNode k = root;
        while (!ans.isEmpty() || k != null) {
            if (k != null) {
                ans.add(k);
                k = k.left;
            } else {
                TreeNode temp = ans.getLast();
                ans.removeLast();
                res.add(temp.val);
                k = temp.right;
            }
        }
        return res;
    }
}
```
# 反转链表
题目链接：https://leetcode-cn.com/problems/reverse-linked-list/

思路：从头开始反转就行

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode pre = null;
        ListNode cur = head;
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
# 合并二叉树
题目链接：https://leetcode-cn.com/problems/merge-two-binary-trees/

思路：递归啊！！！递归啊！！！ 为啥当时写的能这么臭啊！！！！难
```java
class Solution {
    public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
        if (root1 == null) return root2;
        if (root2 == null) return root1;
        TreeNode res =  new TreeNode(root1.val + root2.val);
        res.left = mergeTrees(root1.left, root2.left);
        res.right = mergeTrees(root1.right, root2.right);
        return res;
    }
}
```
&nbsp;
# 二叉树最大深度
题目链接：https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/

目的：更好理解递归，尽量让自己的写法更加优雅
```java
class Solution {
    public int maxDepth(TreeNode root) {
        if (root == null) return 0;
        int left = maxDepth(root.left);
        int right = maxDepth(root.right);
        return Math.max(left, right) + 1;
    }
}
```
&nbsp;
# 树的直径
题目链接：https://leetcode-cn.com/problems/diameter-of-binary-tree/

思路：恶心至极啊！！！我们不能只是依赖通过根节点的路径。只能在dfs里面下文章。L 和 R 分别是左子树的最大值和右子树的最大值。直接相加就可以得到最长的直径。
```java
class Solution {
    int ans;
    public int diameterOfBinaryTree(TreeNode root) {
        ans = 0;
        dfs(root);
        return ans;
    }
    public int dfs(TreeNode root) {
        if (root == null) return 0;
        int a = dfs(root.left);
        int b = dfs(root.right);
        ans = Math.max(ans, a + b);
        return Math.max(a, b) + 1;
    }
}
```
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
    int[] preorder;
    HashMap<Integer, Integer> dic = new HashMap<>();
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        this.preorder = preorder;
        for(int i = 0; i < inorder.length; i++)
            dic.put(inorder[i], i);
        return recur(0, 0, inorder.length - 1);
    }
    TreeNode recur(int root, int left, int right) {
        if(left > right) return null;                          // 递归终止
        TreeNode node = new TreeNode(preorder[root]);          // 建立根节点
        int i = dic.get(preorder[root]);                       // 划分根节点、左子树、右子树
        node.left = recur(root + 1, left, i - 1);              // 开启左子树递归
        node.right = recur(i + root - left + 1, i + 1, right); // 开启右子树递归
        return node;                                           // 回溯返回根节点
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

# 链表中是否存在环
链接：https://leetcode-cn.com/problems/linked-list-cycle/

思路：快慢指针

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        if (head == null || head.next == null) {
            return false;
        }
        ListNode slow = head;
        ListNode fast = head.next;
        while (slow != fast) {
            if (fast == null || fast.next == null) {
                return false;
            }
            slow = slow.next;
            fast = fast.next.next;
        }
        return true;
    }
}
```
&nbsp;
&nbsp;
# 链表中倒数第k个节点
链接：https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/

思路：我以前用的是直接遍历，然后这肯定被面试官乱锤，肯定要用快慢指针的！！！！

暴力：

```java
class Solution {
    public ListNode getKthFromEnd(ListNode head, int k) {
        int i = 0;
        ListNode res = head;
        while (head != null) {
            head = head.next; i++;
        }
        int z = i - k;
        for (int m = 1; m <= z; m++) {
            res = res.next;
        }
        return res;
    }
}
```

正确：

```java
class Solution {
    public ListNode getKthFromEnd(ListNode head, int k) {
        ListNode former = head, latter = head;
        for(int i = 0; i < k; i++)
            former = former.next;
        while(former != null) {
            former = former.next;
            latter = latter.next;
        }
        return latter;
    }
} 
```

# LRU

经典算法，题目链接：https://leetcode-cn.com/problems/lru-cache-lcci/

思路：通过hashmap维护键值对，list维护最短时间的最新值

```java
class LRUCache {
    public int capacity;
    public HashMap map = null;
    public LinkedList list = null;
    public LRUCache(int capacity) {
        this.capacity = capacity;
        map = new HashMap<Integer, Integer>();
        list = new LinkedList<Integer>();
    }
    
    public int get(int key) {
        if (map.containsKey(key)) {
            list.remove((Integer)key);
            list.addFirst(key);
            return (int)map.get(key);
        }
        return -1;
    }
    
    public void put(int key, int value) {
        if (map.containsKey(key)) {
            list.remove((Integer)key);
            list.addFirst(key);
            map.put(key, value);
            return;
        }
        if (list.size() == capacity) {
            map.remove(list.removeLast());
            map.put(key, value);
            list.addFirst((Integer)key);
        } else {
            map.put(key, value);
            list.addFirst((Integer)key);
        }
    }
}
```
