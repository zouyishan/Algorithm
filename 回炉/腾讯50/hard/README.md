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
