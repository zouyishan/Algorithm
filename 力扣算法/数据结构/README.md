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
