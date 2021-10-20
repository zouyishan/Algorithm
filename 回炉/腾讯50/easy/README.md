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
