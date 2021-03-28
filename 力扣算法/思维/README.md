目录：
* [和为K的子数组](#和为k的子数组)
* [减绳子](#减绳子)
* [盛水最多的容器](#盛水最多的容器)
* [数组中第k大](#数组中第k大)
&nbsp;
# 和为k的子数组
链接：https://leetcode-cn.com/problems/subarray-sum-equals-k/

思路：通过用HashMap计算pre(前缀和) - k的个数得出有多少个符合的子数组。秒..........
```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
        int count = 0, pre = 0;
        map.put(0, 1);
        for (int num : nums) {
            pre += num;
            if (map.containsKey(pre - k)) {
                count += map.get(pre - k);
            }
            map.put(pre, map.getOrDefault(pre, 0) + 1);
        }
        return count;
    }
}
```
# 数组中第k大
题目链接：https://leetcode-cn.com/problems/kth-largest-element-in-an-array/

注意的细节：这个第k大是从1开始，我们要将k的值减一才行。此外注意细节就行了~~~~
```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        return search(nums, 0, nums.length - 1, k - 1);
    }
    public int search(int[] nums, int l, int r, int k) {
        int mid = (l + r) >> 1;
        int x = nums[mid];
        int i = l, j = r;
        while (i <= j) {
            while (nums[i] > x) i++;
            while (nums[j] < x) j--;
            if (i <= j) {
                int temp = nums[i];
                nums[i] = nums[j];
                nums[j] = temp;
                i++; j--;
            }
        }
        if (j >= k) search(nums, l, j, k);
        else if (i <= k) search(nums, i, r, k);
        return nums[k];
    }
}
```

# 盛水最多的容器
题目链接：https://leetcode-cn.com/problems/container-with-most-water/

思路：双指针。
```java
class Solution {
    public int maxArea(int[] height) {
        int res = -1, pre = 0, end = height.length - 1;
        while (pre < end) {
            res = Math.max(res, (end - pre) * Math.min(height[pre], height[end]));
            if (height[end] >= height[pre]) pre++;
            else end--;
        }
        return res;
    }
}
```
# 减绳子
**思路**
在一个绳子切成m段，对于相同m段，那么每段长度相同是乘积最高的
对于m段，可以计算得知，当每段为3时乘积最大。
所以将绳子的长度除3取余数，有0,1,2三种情况
* 如果余0，那么正好三等分
* 如果余1，那么13<22,故拿出之前一段3，将其拆分成2,2（见n=4时的情况）
* 如果余2，那么直接*2

神奇的数学知识又增加了

链接：https://leetcode-cn.com/problems/jian-sheng-zi-lcof/solution/jian-zhi-14-ijian-sheng-zi-tan-xin-suan-5yfed/

```java
class Solution {
    public int cuttingRope(int n) {
        if(n <= 3) return n - 1;
        int a = n / 3, b = n % 3;
        if(b == 0) return (int)Math.pow(3, a);
        if(b == 1) return (int)Math.pow(3, a - 1) * 4;
        return (int)Math.pow(3, a) * 2;
    }
}
```
