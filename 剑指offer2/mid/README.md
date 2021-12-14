# 二维数组中查找
二分秒掉: https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/
```java
class Solution {
    public boolean findNumberIn2DArray(int[][] matrix, int target) {
        if (matrix.length == 0 || matrix[0].length == 0) {
            return false;
        }

        for (int i = 0; i < Math.min(matrix[0].length, matrix.length); i++) {
            if (matrix[i][i] == target || matrix[i][matrix[0].length - 1] == target || matrix[matrix.length - 1][i] == target) {
                return true;
            }
            if (matrix[i][i] > target) {
                return false;
            }

            if (matrix[i][matrix[0].length - 1] > target) {
                int l = i, r = matrix[0].length - 1;
                while (l <= r) {
                    int mid = (l + r) >> 1;
                    int res = matrix[i][mid];
                    if (res > target) {
                        r = mid - 1;
                    } else if (res < target) {
                        l = mid + 1;
                    } else {
                        return true;
                    }
                }
            }

            if (matrix[matrix.length - 1][i] > target) {
                int l = i, r = matrix.length - 1;
                while (l <= r) {
                    int mid = (l + r) >> 1;
                    int res = matrix[mid][i];
                    if (res > target) {
                        r = mid - 1;
                    } else if (res < target) {
                        l = mid + 1;
                    } else {
                        return true;
                    }
                }
            }
        }
        return false;
    }
}
```
