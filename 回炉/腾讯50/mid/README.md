# 两数相加
https://leetcode-cn.com/problems/add-two-numbers/
```c
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
# 字符串转整数
超级大模拟了属于是 

https://leetcode-cn.com/problems/string-to-integer-atoi/
```c
class Solution {
    public static int check(long res, long fushu) {
        if (fushu == 1) {
            res = -res;
            if (res < (1 << 31)) return 1 << 31;
        } else {
            if (res > (1 << 31) - 1) return (1 << 31) - 1;
        }
        return 0;
    }
    public int myAtoi(String s) {
        long flag = 0, res = 0, fushu = 0;
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == ' ' && flag == 0) continue; 
            if ((s.charAt(i) >= 'a' && s.charAt(i) <= 'z') || (s.charAt(i) >= 'A' && s.charAt(i) <= 'Z') || (flag == 1 && s.charAt(i) == ' ') || (s.charAt(i) == '.') || (flag == 1 && (s.charAt(i) == '+' || s.charAt(i) == '-'))) {
                if (fushu == 1) res = -res;
                return (int)res;
            }
            if (s.charAt(i) == '-' && flag == 0) {
                flag = 1;
                fushu = 1; 
                continue;
            }
            if (s.charAt(i) == '+' && flag == 0) {
                flag = 1;
                fushu = 0; 
                continue;
            }
            
            if (s.charAt(i) >= '0' || s.charAt(i) <= '9') {
                flag = 1;
                int z = 0;
                res *= 10;
                res += (s.charAt(i) - '0');
                if ((z = check(res, fushu)) != 0) {
                    return z;
                }
            }
        }
        if (fushu == 1) res = -res;
        return (int)res;
    }
}
```
# 盛水最多的容器
https://leetcode-cn.com/problems/container-with-most-water/
```c
class Solution {
    public static int max_(int l, int r) {
        if (l > r) return l;
        return r;
    }
    public static int min_(int l, int r) {
        if (l > r) return r;
        return l;
    }

    public int maxArea(int[] height) {
        int l = 0, r = height.length - 1, res = 0;

        while (l < r) {
            res = max_(res, (r - l) * min_(height[l], height[r]));
            if (height[l] < height[r]) {
                l++;
            } else {
                r--;
            }
        }
        return res;
    }
}
```
# 最接近的三数之和
https://leetcode-cn.com/problems/3sum-closest/
思路很值得借鉴，要求a, b, c, target。 可以通过target - a即可简化
```c
class Solution {
    public static int abs(int x, int y) {
        if (x > y) return x - y;
        return y - x;
    }

    public int threeSumClosest(int[] nums, int target) {
        Arrays.sort(nums);
        int res = 0, kk = (1 << 31 - 1);
        for (int i = 0; i < nums.length - 2; i++) {
            int newTarget = target - nums[i];
            int l = i + 1, r = nums.length - 1;
            while (l < r) {
                if (nums[l] + nums[r] < newTarget) {
                    if (abs(nums[l] + nums[r], newTarget) < kk) {
                        res = nums[l] + nums[r] - newTarget + target;
                        kk = abs(nums[l] + nums[r], newTarget);
                    }
                    l++;
                } else if (nums[l] + nums[r] == newTarget) {
                    return target;
                } else {
                    if (abs(nums[l] + nums[r], newTarget) < kk) {
                        res = nums[l] + nums[r] - newTarget + target;
                        kk = abs(nums[l] + nums[r], newTarget);
                    }
                    r--;
                }
            }
        }
        return res;
    }
}
```
