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
