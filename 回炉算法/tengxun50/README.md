# 两数相加
https://leetcode-cn.com/problems/add-two-numbers/submissions/
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
