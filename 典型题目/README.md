* [编辑距离](#编辑距离)
* [马拉车最长回文子串](#马拉车最长回文子串)
* [设计跳表](#设计跳表)
* [KMP字符串匹配算法](#KMP字符串匹配算法)
* [多线程打印ABC](#多线程打印ABC)
* [约瑟夫杯](#约瑟夫杯)

# 编辑距离
https://leetcode-cn.com/problems/edit-distance/

D[i][j] 表示 A 的前 i 个字母和 B 的前 j 个字母之间的编辑距离。

我们用`A = horse`，`B = ros` 作为例子，来看一看是如何把这个问题转化为规模较小的若干子问题的。

1. 在单词 A 中插入一个字符：如果我们知道 horse 到 ro 的编辑距离为 a，那么显然 horse 到 ros 的编辑距离不会超过 a + 1。这是因为我们可以在 a 次操作后将 horse 和 ro 变为相同的字符串，只需要额外的 1 次操作，在单词 A 的末尾添加字符 s，就能在 a + 1 次操作后将 horse 和 ro 变为相同的字符串；

2. 在单词 B 中插入一个字符：如果我们知道 hors 到 ros 的编辑距离为 b，那么显然 horse 到 ros 的编辑距离不会超过 b + 1，原因同上；

3. 修改单词 A 的一个字符：如果我们知道 hors 到 ro 的编辑距离为 c，那么显然 horse 到 ros 的编辑距离不会超过 c + 1，原因同上。

那么从 horse 变成 ros 的编辑距离应该为 **min(a + 1, b + 1, c + 1)**。

```java
class Solution {
    public int minDistance(String word1, String word2) {
        int n = word1.length();
        int m = word2.length();

        if (n == 0 || m == 0) {
            return n == 0 ? m : n;
        }

        int[][] dp = new int[n + 1][m + 1];
        for (int i = 0; i <= n; i++) {
            dp[i][0] = i;
        }
        for (int j = 0; j <= m; j++) {
            dp[0][j] = j;
        }

        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= m; j++) {
                // 向A串中添加一个数
                int a = dp[i - 1][j] + 1;
                // 向B串中添加一个数
                int b = dp[i][j - 1] + 1;
                int c = dp[i - 1][j - 1];
                // 如果对应位置的数相同就不用修改，不同就修改次数加1
                if (word1.charAt(i - 1) != (word2.charAt(j - 1))) {
                    c += 1;
                }
                // 找到最小的一个数就可以了
                dp[i][j] += Math.min(Math.min(a, b), c);
            }
        }
        return dp[n][m];
    }
}
```

# 马拉车最长回文子串
![image](https://user-images.githubusercontent.com/57765968/152648419-29d2130c-a5fc-4d9a-ac62-c9aeffd7e8bb.png)
着重理解中心点center，和最大距离right，和对应的record[i]的length长度。
```java
class Solution {
    public String longestPalindrome(String s) {
        if (s.length() == 1) {
            return s;
        }

        StringBuilder str = new StringBuilder("#");
        for (int i = 0; i < s.length(); i++) {
            str.append(s.charAt(i));
            str.append('#');
        }
        s = str.toString();

        int center = -1, right = -1, length = 0;
        int start = 0, end = 0;
        int[] record = new int[s.length()];
        for (int i = 0; i < s.length(); i++) {
            if (right >= i) {
                int mirror = center * 2 - i;
                // 这里的minLength一定要和right - i比较
                int minLength = Math.min(record[mirror], right - i);
                length = expand(s, i - minLength, i + minLength);
            } else {
                length = expand(s, i, i);
            }

            record[i] = length;
            if (i + length > right) {
                center = i;
                right = i + length;
            }

            if (end - start < length * 2) {
                start = i - length;
                end = i + length;
            }
        }

        String res = "";
        for (int i = start; i <= end; i++) {
            if (s.charAt(i) != '#') {
                res += s.charAt(i);
            }
        }
        return res;
    }

    public int expand(String s, int l, int r) {
        while (l >= 0 && r < s.length() && s.charAt(l) == s.charAt(r)) {
            l--;
            r++;
        }
        return (r - l - 2) / 2;
    }
}
```

# 设计跳表
https://leetcode-cn.com/problems/design-skiplist/

这个跳表的随机层数，能够搞出一个论文，这是我没想到的....
```java
class Skiplist {
    // 最大层数
    private static int DEFAULT_MAX_LEVEL = 32;

    // 随机因子
    private static double DEFAULT_P_FACTOR = 0.25;

    // 头节点
    Node head = new Node(null, DEFAULT_MAX_LEVEL);

    // 当前的最高层数，从0开始
    int currentLevel = 0;

    public boolean search(int target) {
        Node searchNode = head;
        for (int i = currentLevel; i >= 0; i--) {
            searchNode = findClosest(searchNode, i, target);
            if (searchNode.next[i] != null && searchNode.next[i].value == target){
                return true;
            }
        }
        return false;
    }

    public void add(int num) {
        int level = randomLevel();
        Node updateNode = head;
        Node newNode = new Node(num, level + 1);
        // 计算出当前num 索引的实际层数，从该层开始添加索引
        for (int i = currentLevel; i >= 0; i--) {
            //找到本层最近离num最近的list
            updateNode = findClosest(updateNode, i, num);
            if (i <= level) {
                if (updateNode.next[i] == null) {
                    updateNode.next[i] = newNode;
                } else {
                    Node temp = updateNode.next[i];
                    updateNode.next[i] = newNode;
                    newNode.next[i] = temp;
                }
            }
        }
        // 如果随机出来的层数比当前的层数还大，那么超过currentLevel的head 直接指向newNode
        if (level > currentLevel) { 
            for (int i = currentLevel; i < level; i++) {
                head.next[i] = newNode;
            }
            currentLevel = level;
        }
    }

    public boolean erase(int num) {
        boolean flag = false;
        Node searchNode = head;
        for (int i = currentLevel; i >= 0; i--) {
            searchNode = findClosest(searchNode, i, num);
            if (searchNode.next[i] != null && searchNode.next[i].value == num){
                //找到该层中该节点
                searchNode.next[i] = searchNode.next[i].next[i];
                flag = true;
                continue;
            }
        }
        return flag;
    }

    private Node findClosest(Node node, int levelIndex, int value) {
        while ((node.next[levelIndex]) != null && value > node.next[levelIndex].value){
            node = node.next[levelIndex];
        }
        return node;
    }

    private static int randomLevel(){
        int level = 1;
        while (Math.random() < DEFAULT_P_FACTOR && level < DEFAULT_MAX_LEVEL) {
            level ++;
        }
        return level;
    }

    class Node {
        public Integer value;
        public Node[] next;

        public Node(Integer value, int size) {
            this.value = value;
            this.next = new Node[size];
        }
    }
}
```

# KMP字符串匹配算法
https://leetcode-cn.com/problems/implement-strstr/

当匹配不到对应的字符串时，不是从头开始进行匹配，而是找`next[k - 1]`进行匹配。
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

# 多线程打印ABC

```java
public class ThreadTest implements Runnable {
    public static volatile int tag = 0;
    public static Object obj = new Object();
    public String str;
    public int flag;
    public ThreadTest(String str, int flag) {
        this.str = str;
        this.flag = flag;
    }

    public static void main(String[] args) {
        new Thread(new ThreadTest("B", 2)).start();
        new Thread(new ThreadTest("A", 1)).start();
        new Thread(new ThreadTest("C", 3)).start();
    }

    @Override
    public void run() {
        synchronized (obj) {
            for (int i = 0; i < 10; i++) {
                while (this.flag != tag + 1) {
                    try {
                        obj.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
                System.out.println(this.str);
                tag = (tag + 1) % 3;
                obj.notifyAll();
            }
        }
    }
}
```

# 约瑟夫杯
```java
import java.util.Scanner;

public class Main {
	static int out=0;//标记已被淘汰的人数
	public static void main(String[] args) {
		Scanner sc=new Scanner(System.in);
		int n=sc.nextInt();
		int k=sc.nextInt();
		//将n个人存储在数组中
		int[]arr=new int[n+1];
		for (int i = 1; i < arr.length; i++) {arr[i]=i;}
		//动态变化的索引,用于模拟循环报数
		int in=1;
		int ik=1;
		//当淘汰人数等于n-1时会跳出循环
		while (out<n-1) {
			//被置为0的视为已经被淘汰,所以碰到0和碰到非0的操作是不一样的
			if(arr[in]==0) {
				//已被淘汰 不需要报数
				if(in==n)in=1;else in++;
			}else {
				if(ik==k) {
					arr[in]=0;//置为0(淘汰)
					ik=0;//报数从1开始,因为下面有K++ 所以置0
					out++;
				}
				if(in==n)in=1;else in++;
				ik++;
			}
		}
		//这时候数组中肯定只剩一个人不为0了,那么这个人的序号就是最后的answer
		for (int i = 1; i < arr.length; i++) {
			if(arr[i]!=0) {
				System.out.println(arr[i]);
				break;
			}
		}
	}
}
```
