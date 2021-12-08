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
