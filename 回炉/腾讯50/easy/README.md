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
