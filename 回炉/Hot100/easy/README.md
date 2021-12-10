# 二叉树的直径
你管着，叫简单题？？？？？？很难绷得住

https://leetcode-cn.com/problems/diameter-of-binary-tree/
```java
class Solution {
    public static int res = 0;
    public int diameterOfBinaryTree(TreeNode root) {
        res = 0;
        dfs(root);
        return res - 1;
    }

    public int dfs(TreeNode root) {
        if (root != null) {
            int left = dfs(root.left);
            int right = dfs(root.right);
            if (right + left + 1 > res) {
                res = right + left + 1;
            }
            return Math.max(left, right) + 1;
        }
        return 0;
    }
}
```
# KMP字符匹配
啊哈，我又活了！！！！: https://leetcode-cn.com/problems/implement-strstr/
```java
class Solution {
    public int strStr(String haystack, String needle) {
        if (needle.length() == 0) {
            return 0;
        }
        if (haystack.length() == 0) {
            return -1;
        }

        int[] next = new int[needle.length()];
        int k = 0;
        for (int i = 1; i < needle.length(); i++) {
            while (k > 0 && needle.charAt(i) != needle.charAt(k)) {
                k = next[k - 1];
            }
            if (needle.charAt(i) == needle.charAt(k)) {
                k++;
            }
            next[i] = k;
        }

        k = 0;
        for (int i = 0; i < haystack.length(); i++) {
            while (k > 0 && haystack.charAt(i) != needle.charAt(k)) {
                k = next[k - 1];
            }
            
            if (needle.charAt(k) == haystack.charAt(i)) {
                k++;
                if (k == needle.length()) {
                    return i - k + 1;    
                }
                continue;
            }
        }
        return -1;
    }
}
```
