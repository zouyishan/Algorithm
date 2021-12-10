# 二叉树中的最大路径和
https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/

自己想的大部分哦，好好看看这个递归：
```java
class Solution {
    public static int res = 0;
    public static int get_max(TreeNode root) {
        if (root == null) return 0;
        int left_value = Math.max(get_max(root.left), 0);
        int right_value = Math.max(get_max(root.right), 0);
        int pre_val = root.val;
        pre_val += left_value + right_value;
        res = Math.max(res, pre_val);
        return root.val + Math.max(left_value, right_value); 
    }
    
    public int maxPathSum(TreeNode root) {
        res = 1 << 31;
        get_max(root);
        return res;
    }
}
```
二刷，这题，真的搞不懂为什么title是困难？？？？
```java
class Solution {
    int res = (1 << 31);
    public int maxPathSum(TreeNode root) {
        res = (1 << 31);
        dfs(root);
        return res;
    }

    public int dfs(TreeNode root) {
        if (root != null) {
            int left = dfs(root.left);
            int right = dfs(root.right);
            if (left >= 0 && right >= 0) {
                if (res < left + right + root.val) {
                    res = left + right + root.val;
                }
                return root.val + Math.max(left, right);
            }
            int left_ = left > 0 ? left : 0;
            int right_ = right > 0 ? right : 0;
            if (res < left_ + right_ + root.val) {
                res = left_ + right_ + root.val;
            }

            return root.val + left_ + right_;
        }
        return 0;
    }
}
```
# 寻找两个正序数组的中位数
一个看起来像easy的hard题目：
https://leetcode-cn.com/problems/median-of-two-sorted-arrays/
```c
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int[] res = new int[nums1.length + nums2.length];
        int l1 = 0, l2 = 0, i = 0;
        while (l1 < nums1.length && l2 < nums2.length) {
            if (nums1[l1] < nums2[l2]) {
                res[i++] = nums1[l1++];
            } else {
                res[i++] = nums2[l2++];
            }
        }
        while (l1 < nums1.length) {
            res[i++] = nums1[l1++];
        }
        while (l2 < nums2.length) {
            res[i++] = nums2[l2++];
        }

        if (((nums1.length + nums2.length) / 2 ) * 2 == nums1.length + nums2.length) {
            return (res[(nums1.length + nums2.length) / 2] + res[((nums1.length + nums2.length) / 2) - 1]) / 2.0;
        } else {
            return res[(nums1.length + nums2.length) / 2];
        }
    }
}
```
