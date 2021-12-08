# 跳跃游戏
初级版本，是否有环形链表的：https://leetcode-cn.com/problems/linked-list-cycle/
```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        ListNode first = head, end = head;
        while (end != null) {
            if (end.next == null) break;
            first = first.next;
            end = end.next.next;
            if (first == end) return true;
        }
        return false;
    }
}
```
进阶版的，找到环形链表的入口： https://leetcode-cn.com/problems/linked-list-cycle/
```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        if (head == null || head.next == null) {
            return null;
        }

        ListNode slow = head, fast = head;
        while (fast != null) {
            if (fast.next == null) {
                return null;
            }
            fast = fast.next.next;
            slow = slow.next;
            if (fast == slow) {
                ListNode cur = head;
                while (cur != slow) {
                    cur = cur.next;
                    slow = slow.next;
                }
                return cur;
            }
        }
        return null;
    }
}
```
# 盛水的容器
双指针的题

https://leetcode-cn.com/problems/container-with-most-water/
```java
class Solution {
    public int maxArea(int[] height) {
        int l = 0, r = height.length - 1, res = -1;
        while (l < r) {
            res = Math.max(Math.min(height[r], height[l]) * (r - l), res);
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
# 跳跃游戏
确实很难绷得住，写了个超过13.3%的。

https://leetcode-cn.com/problems/jump-game/
```java
class Solution {
    public boolean canJump(int[] nums) {
        if (nums.length <= 1) {
            return true;
        }
        boolean[] book = new boolean[nums.length];
        book[0] = true;
        for (int i = 0; i < nums.length; i++) {
            if (book[i] == true) {
                if (nums[i] + i >= nums.length - 1) {
                    return true;
                }
                for (int j = 1; j <= nums[i]; j++) {
                    book[i + j] = true;
                }
            }
        }
        return false;
    }
}
```
聪明的解法
```java
class Solution {
    public boolean canJump(int[] nums) {
        int temp = 0;
        for (int i = 0; i < nums.length; i++) {
            temp = Math.max(nums[i] + i, temp);
            if (temp + 1 >= nums.length) {
                return true;
            }
            if (i == temp) {
                return false;
            }
        }
        return false;
    }
}
```
# 组合总和
https://leetcode-cn.com/problems/combination-sum/

dfs，这个和括号序列很像很像，但是开始做的时候还是没有思路....................
```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> res = new LinkedList<>();
        List<Integer> ans = new LinkedList<>();
        dfs(res, ans, target, 0, candidates);
        return res;
    }

    public void dfs(List<List<Integer>> res, List<Integer> ans, int target, int idx, int[] total) {
        if (idx >= total.length) {
            return;
        }
        if (target == 0) {
            res.add(new LinkedList<>(ans));
            return;
        }

        dfs(res, ans, target, idx + 1, total);

        if (target - total[idx] >= 0) {
            ans.add(total[idx]);
            dfs(res, ans, target - total[idx], idx, total);
            ans.remove((int) ans.size() - 1);
        }
    }
}
```
# 在排序数组中查找元素的第一个和最后一个位置
二分的思想淋漓尽致。多学学

https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/

多看看下面的代码，你就能领悟二分查找了
```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int leftIdx = binarySearch(nums, target, true);
        int rightIdx = binarySearch(nums, target, false);
        if (leftIdx <= rightIdx && rightIdx < nums.length && nums[leftIdx] == target && nums[rightIdx] == target) {
            return new int[]{leftIdx, rightIdx};
        } 
        return new int[]{-1, -1};
    }

    public int binarySearch(int[] nums, int target, boolean lower) {
        int left = 0, right = nums.length - 1, ans = nums.length;
        while (left <= right) {
            int mid = (left + right) / 2;
            if (nums[mid] > target || (lower && nums[mid] >= target)) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        
        if (lower) {
            return right + 1; // left
        } else {
            return left - 1; // right
        }
    }
}
```

# 删除链表的倒数第N个结点
https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/

还行，dfs做的
```java
class Solution {
    public static int level = 0;

    public ListNode removeNthFromEnd(ListNode head, int n) {
        level = 0;
        if (head.next == null) {
            return null;
        }
        return dfs(head, n, null);
    }

    public ListNode dfs(ListNode cur, int n, ListNode pre) {
        if (cur == null) {
            return null;
        }
        dfs(cur.next, n, cur);
        level++;
        if (level == n) {
            if (pre == null) {
                return cur.next;
            }
            pre.next = cur.next;
            cur.next = null;
        }
        return cur;
    }
}
```

# 全排列
二刷全排列了啊。还行吧

https://leetcode-cn.com/problems/permutations/
```java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> res = new LinkedList<>();
        if (nums.length == 1) {
            res.add(Arrays.asList(nums[0]));           
            return res;
        }

        dfs(res, nums, 0, nums.length, new LinkedList<>(), new boolean[nums.length]);
        return res;
    }

    public void dfs(List<List<Integer>> res, int[] nums, int count, int total, List<Integer> ans, boolean[] book) {
        if (count == total) {
            res.add(new LinkedList<>(ans));
            return;
        }

        for (int i = 0; i < nums.length; i++) {
            if (book[i] == false) {
                book[i] = true;
                ans.add(nums[i]);
                dfs(res, nums, count + 1, total, ans, book);
                book[i] = false;
                ans.remove((Integer) nums[i]);
            }
        }
    }
}
```
