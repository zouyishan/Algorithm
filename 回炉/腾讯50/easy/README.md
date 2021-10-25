# 二叉搜索树的最近公共祖先
https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-search-tree/

很有意思的一个题目。这个方法也很新奇！！！但是这个只是针对二叉搜索树。

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        while (true) {
            if (p.val > root.val && q.val > root.val) {
                root = root.right;
            } else if (p.val < root.val && q.val < root.val) {
                root = root.left;
            } else {
                break;
            }
        }
        return root;
    }
}
```
真要看最近公共祖先可以参考这题：https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/
```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null || root == p || root == q) return root;
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if (left == null) return right;
        if (right == null) return left;
        return root;
    }
}
```
# 删除链表中的节点
https://leetcode-cn.com/problems/delete-node-in-a-linked-list/

有点新颖，还行吧
```java
class Solution {
    public void deleteNode(ListNode node) {
        node.val = node.next.val;
        if (node.next.next == null) {
            node.next = null;
            return;
        }
        deleteNode(node.next);
    }
}
```
# 反转字符串中的单词 III
https://leetcode-cn.com/problems/reverse-words-in-a-string-iii/
```java
class Solution {
    public String reverseWords(String s) {
        StringBuffer ret = new StringBuffer();
        int length = s.length();
        int i = 0;
        while (i < length) {
            int start = i;
            while (i < length && s.charAt(i) != ' ') {
                i++;
            }
            for (int p = start; p < i; p++) {
                ret.append(s.charAt(start + i - 1 - p));
            }
            while (i < length && s.charAt(i) == ' ') {
                i++;
                ret.append(' ');
            }
        }
        return ret.toString();
    }
}
```
# 整数反转
https://leetcode-cn.com/problems/reverse-integer/
```java
class Solution {
    public int reverse(int x) {
        long res = 0;
        while (x != 0) {
            long tmp = x % 10;
            res *= 10;
            res += tmp;
            x /= 10;
            if (res > ((1 << 31) - 1) || res < (1 << 31)) return 0;
        }
        return (int)res;
    }
}
```
# 反转字符串
https://leetcode-cn.com/problems/reverse-string/
```java
class Solution {
    public void reverseString(char[] s) {
        int l = 0, r = s.length - 1;
        while (l < r) {
            char tmp = ' ';
            tmp = s[l];
            s[l++] = s[r];
            s[r--] = tmp;
        }
    }
}
```
# 回文数
https://leetcode-cn.com/problems/palindrome-number/
```java
class Solution {
    public boolean isPalindrome(int x) {
        if (x < 0) return false;

        int res = 0, old = x;
        while (x != 0) {
            res *= 10;
            res += x % 10;
            x /= 10;
        }
        return res == old;
    }
}
```
# 最长公共前缀
https://leetcode-cn.com/problems/longest-common-prefix/
```c
class Solution {
    public String longestCommonPrefix(String[] strs) {
        if (strs == null || strs.length == 0) {
            return "";
        }
        String prefix = strs[0];
        int count = strs.length;
        for (int i = 1; i < count; i++) {
            prefix = longestCommonPrefix(prefix, strs[i]);
            if (prefix.length() == 0) {
                break;
            }
        }
        return prefix;
    }

    public String longestCommonPrefix(String str1, String str2) {
        int length = Math.min(str1.length(), str2.length());
        int index = 0;
        while (index < length && str1.charAt(index) == str2.charAt(index)) {
            index++;
        }
        return str1.substring(0, index);
    }
}
```
# 删除有序数组中的重复项
https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/
```java
class Solution {
    public int removeDuplicates(int[] nums) {
        if (nums.length == 0) return 0;
        int tmp = nums[0], res = 1;
        for (int i = 1; i < nums.length; i++) {
            if (tmp == nums[i]) continue;
            else {
                tmp = nums[i];
                nums[res] = tmp;
                res++;
            }
        }
        return res;
    }
}
```
# 合并两个有序数组
https://leetcode-cn.com/problems/merge-sorted-array/
```c
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int count = m + n - 1;
        while (m > 0 && n > 0) {
            if (nums1[m - 1] > nums2[n - 1]) nums1[count--] = nums1[--m];
            else nums1[count--] = nums2[--n];
        }
        while (m > 0) {
            nums1[count--] = nums1[--m];
        }
        while (n > 0) {
            nums1[count--] = nums2[--n];
        }
    }
}
```
# 存在重复元素
https://leetcode-cn.com/problems/contains-duplicate/
```c
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Map<Integer, Integer> maps = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            if (maps.containsKey(nums[i])) {
                return true;
            }
            maps.put(nums[i], 100);
        }
        return false;
    }
}
```
# 2 的幂
https://leetcode-cn.com/problems/power-of-two/
```c
class Solution {
    public boolean isPowerOfTwo(int n) {
        while (n != 0) {
            if (n == 1) return true;
            if (n % 2 != 0) {
                return false;
            }
            n /= 2;
        }
        return false;
    }
}
```
