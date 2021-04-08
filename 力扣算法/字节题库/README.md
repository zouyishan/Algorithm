善用ctrl + f:

* [二叉树锯齿形层序遍历](#二叉树锯齿形层序遍历)
* [无重复字串的最长字串](#无重复字串的最长字串)

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
