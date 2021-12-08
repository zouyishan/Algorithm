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
