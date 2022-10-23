---
title: "LeetCode 每日一题"
date: 2022-04-28
lastmod: 2022-08-12
categories: 
- LeetCode
tags: 
- LeetCode
---

> 因为不是写题解的模式，感觉这样记录每日一题意义不大。停更！
>
> 可以访问 [我的力扣主页](https://leetcode.cn/u/quwny/) 查看题解。

## [890. 查找和替换模式](https://leetcode.cn/problems/find-and-replace-pattern/)

`tag: 字符串、模式匹配、中等`

```java
/**
 * 解题思路：建立双向映射规则，单词与模式进行匹配。
 * 单词只包含小写字母。
 */
class Solution {
    public List<String> findAndReplacePattern(String[] words, String pattern) {
        List<String> res = new ArrayList<>(); // 存储结果
        char[] pts = pattern.toCharArray(); // 转换为字符数组
        // 保存映射规则
        char[] rules1 = new char[26];
        char[] rules2 = new char[26];
        for (String word : words) {
            Arrays.fill(rules1, '\0');
            Arrays.fill(rules2, '\0');
            int n = word.length();
            for (int i = 0; i < n; ++i) {
                char ch = word.charAt(i);
                int j = pts[i] - 'a';
                // 建立双向映射规则
                if (rules1[j] == '\0' && rules2[ch - 'a'] == '\0') {
                    rules1[j] = ch;
                    rules2[ch - 'a'] = pts[i];
                    continue;
                }
                // 如果不符合映射规则，跳出循环  
                if (rules1[j] != ch) {
                    n = -1;
                    break;
                }
            }
            if (n != -1) {
                res.add(word);
            }
       }
       return res;
    }
}
```

## [875. 爱吃香蕉的珂珂](https://leetcode.cn/problems/koko-eating-bananas/)

`tag: 二分查找、中等`

```java
/**
 * 解题思路：二分查找
 */
class Solution {
    public int minEatingSpeed(int[] piles, int h) {
        // 二分查找边界，low -- 最小边界 1，high -- 最大边界 香蕉堆中的最大香蕉数
        int low = 1, high = Arrays.stream(piles).max().getAsInt();

        int k = high;

        // 二分查找
        while (low < high) {
            int speed = (high - low) / 2 + low;

            // 以 speed 速度吃香蕉所花费的时间
            long time = getTime(piles, speed);

            if (time <= h) {
                // 更新 k
                k = speed;
                high = speed;
            } else {
                low = speed +1;
            }
        }
        return k;
    }

    /**
     * 以 speed 速度吃香蕉所花费的时间
     */
    private long getTime(int[] piles, int speed) {
        long time = 0;
        for (int pile : piles) {
            // 吃 pile 需要花费的时间
            int currTime = (pile + speed - 1) / speed;
            time += currTime;
        }
        return time;
    }
}
```

## [467. 环绕字符串中唯一的子字符串](https://leetcode.cn/problems/unique-substrings-in-wraparound-string/)

`tag: 字符串、动态规划、中等`

`tips: 看官方才发现是动态规划，我的处理方式没有官方的好。`

```java
/**
 * 解题思路：字符串 s 是无限环绕字符串 "...zabcdefghijklmnopqrstuvwxyza..."。
 * 因此，可以快速的通过判断前后两个字符的差值是否为 1 或者 - 25，判定子串是否在 s 中。
 * 
 * 难点：唯一性如何保证？
 * 方式一：使用哈希表或者集合存储子串 -- 这种方式超时
 * 方式二：因为子串是有规律的连续串，可以通过判断当前连续子串的长度和结尾字符进行快速判断唯一性。
 */
class Solution {
    public int findSubstringInWraproundString(String p) {
        int n = p.length(); // 字符串长度
        int res = 0; // 结果
        // 存储每个字符目前的最长连续子串长度
        int[] lens = new int[26];
        // 初始赋值为 0
        Arrays.fill(lens, 0);

        // 特殊处理 -- 第一个字符
        lens[p.charAt(0) - 'a'] = 1;
        res += 1;

        // 子串连续的起点和终点
        int s = 0, e = 0;
        // 遍历字符串，取连续的子串
        for (int i = 1; i < n; ++i) {
            // 判断是否连续 -- 需要考虑无限循环 -- 对 26 进行取余
            char ch1 = p.charAt(i - 1);
            char ch2 = p.charAt(i);

            // 如果不是连续的
            if (((ch1 - 'a' + 1) % 26 + 'a') != ch2) {
                s = i;
            }
            e = i;

            // 精髓所在
            int len = e - s + 1;
            // 当前子串的长度大于已经存在的子串，则添加没有遍历过的子串
            if (lens[ch2 - 'a'] < len) {
                res += len - lens[ch2 - 'a'];
                // 更新长度
                lens[ch2 - 'a'] = len;
            }
        }
        return res;
    }
}
```

## [965. 单值二叉树](https://leetcode.cn/problems/univalued-binary-tree/)

`tag: 单值二叉树、遍历、简单`

```java
class Solution {
    // 根结点的值
    private Integer val;

    public boolean isUnivalTree(TreeNode root) {
        val = root.val;
        return preOrder(root);
    }

    private boolean preOrder(TreeNode root) {
        // 边界 -- true
        if (root == null) {
            return true;
        }
        // 值不相等
        if (root.val != val) {
            return false;
        }
        // 遍历左子树
        if (preOrder(root.left)) {
            // 遍历右子树
            return preOrder(root.right);
        }
        // 左子树存在值不相同的结点
        return false;
    }
}
```

## [961. 在长度 2N 的数组中找出重复 N 次的元素](https://leetcode.cn/problems/n-repeated-element-in-size-2n-array/)

```java
/**
 * 解题思路：随机选取一个元素，判断是否是重复元素（为了方便，选择第一个），如果不是，舍弃此元素，并使用投票算法。
 */
class Solution {
    public int repeatedNTimes(int[] nums) {
        // res -- 结果
        int res = nums[0], n = nums.length;
        // 计数
        int count = 1, curr = nums[0];
        for (int i = 1; i < n; ++i) {
            if (nums[i] == res) {
                return res;
            }

            // 投票算法
            if (curr == nums[i]) {
                ++count;
            } else {
                if (--count <= 0) {
                    curr = nums[i];
                    count = 1;
                }
            }

            if (count > 1) {
                return curr;
            }
        }

        return curr;
    }
}
```

## [436. 寻找右区间](https://leetcode.cn/problems/find-right-interval/)

`tag: 排序、双指针、中等`

```java
/**
 * 解题思路：双指针
 */
class Solution {
    public int[] findRightInterval(int[][] intervals) {
        int n = intervals.length;
        // 存储左边界及对应下标
        int[][] sInterval = new int[n][2];
        // 存储右边界及对应下标
        int[][] eInterval = new int[n][2];
        //
        int[] res = new int[n];

        // 赋值
        for (int i = 0; i < n; ++i) {
            sInterval[i][0] = intervals[i][0];
            sInterval[i][1] = i;
            eInterval[i][0] = intervals[i][1];
            eInterval[i][1] = i;
        }
        // 排序 -- 从小到大
        Arrays.sort(sInterval, (o1, o2) -> o1[0] - o2[0]);
        Arrays.sort(eInterval, (o1, o2) -> o1[0] - o2[0]);

        for (int i = 0, j = 0; i < n; ++i) {
            // 如果 ei > sj，遍历
            while (j < n && sInterval[j][0] < eInterval[i][0]) {
                ++j;
            }
            res[eInterval[i][1]] = -1;
            if (j < n) {
                res[eInterval[i][1]] = sInterval[j][1];
            }
        }

        return res;
    }
}
```

## [462. 最少移动次数使数组元素相等 II](https://leetcode.cn/problems/minimum-moves-to-equal-array-elements-ii/)

`tag: 排序、中位数`

```java
/**
 * 参考了官方题解。
 * TODO 留个坑 快速选择第 k 小的数
 */
class Solution {
    public int minMoves2(int[] nums) {
        // 排序 小 --> 大
        Arrays.sort(nums);
        // mid - 中位数
        int count = 0, mid = nums[nums.length / 2];
        // 计算移动次数
        for (int num : nums) {
            count += Math.abs(num - mid);
        }
        return count;
    }
}
```

## [668. 乘法表中第k小的数](https://leetcode.cn/problems/kth-smallest-number-in-multiplication-table/)

`tag: 二分查找`

yukiyama 大佬解析二分查找：[二分查找从入门到入睡](https://leetcode.cn/circle/discuss/ooxfo8/)

```java
/**
 * 看了官方题解
 * 解题思路：二分查找。边界为 [1, mn]
 */
class Solution {
    public int findKthNumber(int m, int n, int k) {
        int l = 1, r = m * n;
        while (l < r) {
            int x = l + (r - l) / 2;
            // count -- 小于 x 的个数
            // x / n -- 元素小于 x 的行数
            int count = x / n * n;
            // 剩余没有完全小于 x 的行
            for (int i = x / n + 1; i <= m; ++i) {
                count += x / i;
            }
            // TODO 解释为什么 count == k 时还要继续二分？
            if (count >= k) {
                // 取左半部分
                r = x;
            } else {
                // 取右半部分
                l = x + 1;
            }
        }
        return l;
    }
}
```

## [953. 验证外星语词典](https://leetcode.cn/problems/verifying-an-alien-dictionary/)

`tag: 字符串、字典序`

```java
/**
 * 解题思路：使用数组保存字母表的顺序。两两比较是否符合字典序。
 */
class Solution {
    private int[] o = new int[26];

    public boolean isAlienSorted(String[] words, String order) {

        // 保存字典序
        for (int i = 0; i < 26; ++i) {
            o[order.charAt(i) - 'a'] = i;
        }

        int n = words.length;
        // 两两比较是否符合字典序
        for (int i = 1; i < n; ++i) {
            if (order(words[i - 1], words[i])) {
                continue;
            }
            return false;
        }
        return true;
    }

    // 比较是否符合字典序
    private boolean order(String s1, String s2) {
        int len1 = s1.length();
        int len2 = s2.length();
        int j = 0, k = 0;

        while (j < len1 && k < len2) {
            char ch1 = s1.charAt(j++);
            char ch2 = s2.charAt(k++);
            if (ch1 == ch2) {
                continue;
            }
            // 不符合字典序
            if (o[ch1 - 'a'] > o[ch2 - 'a']) {
                return false;
            }
            return true;
        }
        return len1 <= len2;
    }
}
```

## [面试题 04.06. 后继者](https://leetcode.cn/problems/successor-lcci/)

`tag: 二叉搜索树、中序后继`

```java
/**
 * 解题思路：二叉搜索树，左子树 < 结点 < 右子树。因此结点 A 的中序后继结点 B 是大于并最接近 A 的结点。
 * 存在三种情况
 * 1. 结点 A 有右子树，则后继结点为右子树的最左结点 B
 * 2. 结点 A 没有右子树且结点 A 属于最靠近该结点 A 的左子树中，则后继结点是左子树的父结点 B
 * 3. 结点 A 没有右子树且不在某一左子树中，则后继结点为 null
 */
class Solution {
    public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
        TreeNode post = null;
        // 右子树不空
        if (p.right != null) {
            // 后继结点为右子树的最左结点
            post = p.right;
            while (post.left != null) {
                post = post.left;
            }
            return post;
        }
        while (root != null) {
            if (p.val >= root.val) {
                // 跳转到右子树中
                root = root.right;
            } else {
                // 跳转到左子树，并记录当前结点
                post = root;
                root = root.left;
            }
        }
        return post;
    }
}
```

## [812. 最大三角形面积](https://leetcode.cn/problems/largest-triangle-area/)

`tag: 三角形面积`

```java
/**
 * 解题思路：求最大面积。任取三个坐标求面积，返回最大值。
 * 三个坐标求三角形面积公式：A = 1 / 2 * |x1(y2 - y3) + x2(y3 - y1) + x3(y1 - y2)|
 */
class Solution {
    public double largestTriangleArea(int[][] points) {
        int n = points.length; // 坐标个数
        double area = 0; // 面积
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                for (int k = j + 1; k < n; ++k) {
                    double temp = 0.5 * Math.abs(points[i][0] * (points[j][1] - points[k][1])
                            + points[j][0] * (points[k][1] - points[i][1])
                            + points[k][0] * (points[i][1] - points[j][1]));
                    area = Math.max(area, temp);
                }
            }
        }
        return area;
    }
}
```

## [6. Z 字形变换](https://leetcode.cn/problems/zigzag-conversion/)

`tag: 字符串、Z 字形变换`

```java
/**
 * 解题思路：根据 Z 字型的规律，重新生成字符串。
 * 1·····1·····1
 * 2···2·2···2
 * 3·3···3·3
 * 4·····4
 */
class Solution {
    public String convert(String s, int numRows) {
        int n = s.length(); // 字符串长度
        // 边界
        if (n <= 1 || numRows == 1) {
            return s;
        }
        StringBuilder sb = new StringBuilder();
        // String str = ""; // 存储新的字符串
        for (int i = 0; i < numRows; ++i) {
            int pos = i;
            while (pos < n) {
                if (i != numRows - 1) {
                    sb.append(s.charAt(pos));
                    // str += s.charAt(pos);
                    pos += 2 * (numRows - i) - 2;
                }
                if (i != 0 && pos < n) {
                    sb.append(s.charAt(pos));
                    // str += s.charAt(pos);
                    pos += 2 * i;
                }
            }
        }
        return sb.toString();
        // return str;
    }
}
```

## [面试题 01.05. 一次编辑](https://leetcode.cn/problems/one-away-lcci/)

`tag: 字符串，匹配`

```java
/**
 * 解题思路：字符串有三种操作，插入、删除、替换。插入和删除可以视为一种情况，替换为另一种情况。
 */
class Solution {
    public boolean oneEditAway(String first, String second) {
        // 字符串长度
        int fn = first.length(), sn = second.length();

        // 字符串长度差大于 1
        if (Math.abs(fn - sn) > 1) {
            return false;
        }

        // 遍历字符串
        int i = 0, j = 0, flag = 0;
        while (i < fn && j < sn) {
            // 出现不相同的字符
            if (first.charAt(i++) != second.charAt(j++)) {
                ++flag;
                if (fn > sn) {
                    --j;
                }
                if (fn < sn) {
                    --i;
                }
            }
            // 不相同的字符大于 1
            if (flag > 1) {
                return false;
            }
        }
        return true;
    }
}
```

~~昨晚做题，突然没电关机，早上起来做的。难受！~~

## [944. 删列造序](https://leetcode.cn/problems/delete-columns-to-make-sorted/)

`tag: 字符串、排序`

```java
/**
 * 解题思路：按列比较是否是升序的。
 */
class Solution {
    public int minDeletionSize(String[] strs) {
        // 字符串个数
        int m = strs.length;
        // 字符串长度
        int n = strs[0].length();
        // 存储结果
        int result = 0;
        // 遍历字符串
        for (int i = 0; i < n; ++i) {
            // 按列遍历
            // 第一列的第 i 个字符
            char pre = strs[0].charAt(i);
            for (int j = 1; j < m; ++j) {
                // 当前列字符
                char curr = strs[j].charAt(i);
                // 如果不是升序，跳出循环，result + 1
                if (pre > curr) {
                    ++result;
                    break;
                }
                pre = curr;
            }
        }
        return result;
    }
}
```

## [449. 序列化和反序列化二叉搜索树](https://leetcode.cn/problems/serialize-and-deserialize-bst/)

`tag: 序列化、二叉搜索树、后序遍历、栈`

```java
/**
 * 解题思路：二叉搜索树，中序遍历是有序的。还原一颗二叉树需要通过中序遍历数组 + (先序遍历 / 后序遍历)
 * 序列化：得到先序遍历/后序遍历数组，再转化为字符串 -- 可以递归实现，这里使用栈实现
 * 反序列化：将字符串转化为先序遍历/后序遍历数组，根据二叉搜索树的特性（有序），进行还原。
 */
public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        List<Integer> list = postOrder(root);
        // 将元素列表转换为字符串
        String str = list.toString();
        // 去掉 []
        return str.substring(1, str.length() - 1);
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        if (data == "") {
            return null;
        }
        // 分割字符串
        String[] vals = data.split(", ");
        // 通过栈存储元素值
        Deque<Integer> stack = new ArrayDeque<>();
        for (String val : vals) {
            // 转化为整数
            stack.push(Integer.parseInt(val));
        }
        // 构造二叉搜索树
        return construct(Integer.MIN_VALUE, Integer.MAX_VALUE, stack);
    }

    // 根据后序数组 和 二叉搜索树的特性，构造二叉搜索树
    private TreeNode construct(int lower, int upper, Deque<Integer> stack) {
        if (stack.isEmpty() || lower > stack.peek() || upper < stack.peek()) {
            return null;
        }
        int val = stack.pop();
        TreeNode root = new TreeNode(val);
        // 后序遍历 -- 左 右 中 --> 进栈之后 -- 中 右 左
        root.right = construct(val, upper, stack);
        root.left = construct(lower, val, stack);
        return root;
    }

    // 后序遍历 -- 使用栈实现
    private List<Integer> postOrder(TreeNode root) {
        // 存储元素值
        List<Integer> list = new ArrayList<>();
        // 栈 -- 保存没有遍历完成的结点
        Deque<TreeNode> stack = new ArrayDeque<>();
        // 保存当前遍历结点的前一个结点
        TreeNode pre = null;
        while (root != null || !stack.isEmpty()) {
            // 结点不空，进栈并访问左子树
            if (root != null) {
                stack.push(root);
                root = root.left;
            } else {
                // 否则，判断是否访问了右子树
                root = stack.peek();
                // 如果没有右结点 或者 已经访问过右结点
                if (root.right == null || root.right == pre) {
                    list.add(stack.pop().val);
                    // 当前遍历的结点
                    pre = root;
                    // 置下一个访问的结点为空
                    root = null;
                } else {
                    // 否则，访问右子树
                    root = root.right;
                }
            }
        }
        return list;
    }
}
```

## [1305. 两棵二叉搜索树中的所有元素](https://leetcode.cn/problems/all-elements-in-two-binary-search-trees/)

`tag: 中序遍历、归并、栈`

```java
/**
 * 解题思路：二叉搜索树 -- 中序遍历元素有序。中序遍历得到有序列表，再合并
 */
class Solution {
    public List<Integer> getAllElements(TreeNode root1, TreeNode root2) {
        return merge(inOrder(root1), inOrder(root2));
    }

    // 合并有序列表
    private List<Integer> merge(List<Integer> result1, List<Integer> result2) {
        List<Integer> result = new ArrayList<>();
        int n1 = result1.size(), n2 = result2.size();
        int i = 0, j = 0;
        while (i < n1 && j < n2) {
            if (result1.get(i) <= result2.get(j)) {
                result.add(result1.get(i++));
            } else {
                result.add(result2.get(j++));
            }
        }
        // result.addAll(index, c) -- 使用此方法可以将剩余下标的元素添加到列表中
        while (i < n1) {
            result.add(result1.get(i++));
        }
        while (j < n2) {
            result.add(result2.get(j++));
        }
        return result;
    }

    // 中序遍历
    private List<Integer> inOrder(TreeNode root) {

        List<Integer> result = new ArrayList<>();

        // 使用栈保存还没有遍历完成的元素
        Deque<TreeNode> stack = new ArrayDeque<>();

        while (root != null || !stack.isEmpty()) {
            // 当前结点不空，进栈
            if (root != null) {
                stack.push(root);
                // 遍历左子树
                root = root.left;
            } else {
                // 出栈
                TreeNode curr = stack.pop();
                // 遍历完成的元素进列表
                result.add(curr.val);
                // 遍历右子树
                root = curr.right;
            }
        }
        return result;
    }
}
```

## [942. 增减字符串匹配](https://leetcode.cn/problems/di-string-match/)

`tag: 贪心、双指针`

```java
class Solution {
    public int[] diStringMatch(String s) {
        /**
         * 解题思路：遍历字符串，双指针分别头尾遍历 0~n+1，如果是 'I'，头指针 + 1，否则尾指针 - 1。
         */
        // 字符串长度
        int n = s.length();
        // 存储结果
        int[] result = new int[n + 1];
        // 头指针，尾指针
        int l = 0, r = n;
        for (int i = 0; i < n; ++i) {
            // result[i] < result[i + 1]
            if (s.charAt(i) == 'I') {
                result[i] = l++;
            } else {
                // result[i] > result[i + 1]
                result[i] = r--;
            }
        }
        // 处理最后一个字符
        result[n] = s.charAt(n - 1) == 'I' ? l : r;
        return result;
    }
}
```

## [442. 数组中重复的数据](https://leetcode-cn.com/problems/find-all-duplicates-in-an-array/)

`tag: 重复、数组`

```java
class Solution {
    public List<Integer> findDuplicates(int[] nums) {
        /**
         * 要求：T：O(n)，S：O(1)
         * 解题思路：遍历数组 nums，如果 nums[nums[i]] 大于 n，说明已经访问过，为重复元素，
         * 否则 nums[nums[i]] + n。
         */
        // 元素的个数
        int n = nums.length;
        List<Integer> result = new ArrayList<>();
        for (int i = 0; i < n; ++i) {
            // 获得元素下标
            int j = (nums[i] - 1) % n;
            // 如果 nums[nums[i]] 大于 n，说明已经访问过，为重复元素
            if (nums[j] > n) {
                result.add(j + 1);
            }
            // nums[nums[i]] + n
            nums[j] += n;
        }
        return result;
    }
}
```

## [933. 最近的请求次数](https://leetcode-cn.com/problems/number-of-recent-calls/)

`tag: 队列`

```java
class RecentCounter {
    // 使用队列保存 [t- 3000, t] 的时间
    private Queue<Integer> time;

    public RecentCounter() {
        // 创建队列
        time = new ArrayDeque<>();
    }

    public int ping(int t) {
        // 将超出 [t- 3000, t] 范围的时间点出队列
        while (!time.isEmpty() && t - 3000 > time.peek()) {
            time.poll();
        }
        time.offer(t);
        return time.size();
    }
}
```

## [1823. 找出游戏的获胜者](https://leetcode-cn.com/problems/find-the-winner-of-the-circular-game/)

`tag: 队列、约瑟夫环`

```java
class Solution {
    public int findTheWinner(int n, int k) {
        /**
         * 解题思路：
         * 按照游戏规则进行模拟，使用队列，从当前 curr 遍历 k - 1 个数，并添加到队尾，第 k 个数出队。
         */
        // 队列
        Queue<Integer> queue = new ArrayDeque<>();
        // 添加 n 个元素
        for (int i = 1; i <= n; ++i) {
            queue.offer(i);
        }

        while (n-- > 1) {
            for (int i = 1; i < k; ++i) {
                queue.offer(queue.poll());
            }
            queue.poll();
        }
        return queue.peek();
    }
}
```

## [937. 重新排列日志文件](https://leetcode-cn.com/problems/reorder-data-in-log-files/)

`tag: 日志、排序、自定义排序规则`

```java
class Solution {
    public String[] reorderLogFiles(String[] logs) {
        /**
         * 解题思路：
         * 1.先提取出字母日志，通过最后一个字符判断是否是字母日志
         * 2.对字母日志数组进行排序 -- 先按照内容，再按照标识符
         * 3.按顺序添加数字日志
         */
        int n = logs.length, j = 0;
        String[] newLogs = new String[n]; // 保存排好序的日志
        for (int i = 0; i < n; ++i) {
            if (Character.isDigit(logs[i].charAt(logs[i].length() - 1))) {
                continue;
            }
            // 添加字母日志
            newLogs[j++] = logs[i];
        }
        // 对字母日志进行排序
        Arrays.sort(newLogs, 0, j, (s1, s2) -> {
            // 第一个空格的下标
            int index1 = s1.indexOf(' ');
            int index2 = s2.indexOf(' ');
            // 得到日志内容
            String s1_content = s1.substring(index1 + 1);
            String s2_content = s2.substring(index2 + 1);
            // 判断日志内容是否相等
            int res = s1_content.compareTo(s2_content);
            if (res != 0) {
                return res;
            }
            // 日志内容相等，获取日志标志
            String s1_tag = s1.substring(0, index1);
            String s2_tag = s2.substring(0, index2);
            // 判断日志标志大小
            return s1_tag.compareTo(s2_tag);
        });
        // 将数字日志添加到新的日志数组中
        for (int i = 0; i < n; ++i) {
            if (Character.isDigit(logs[i].charAt(logs[i].length() - 1))) {
                newLogs[j++] = logs[i];
            }
        }
        // 返回结果
        return newLogs;
    }
}
```

## [591. 标签验证器](https://leetcode-cn.com/problems/tag-validator/)

`tag: 栈、验证器、标签、困难`

```java
class Solution {
    public boolean isValid(String code) {
        // 解题思路：想到了用栈，但感觉条件匹配好复杂，看了一下官方题解，自己敲一下。
        int n = code.length(); // 字符串长度
        // 栈，存放开始标签的名称
        Deque<String> stack = new ArrayDeque<>();
        int i = 0;
        // 遍历字符串
        while (i < n) {
            // 如果是 '<'
            if (code.charAt(i) == '<') {
                // 如果该字符是最后一个字符，则只能是 '>'，返回 false
                if (i == n - 1) {
                    return false;
                }
                // 如果下一个字符是 '/'，说明是结束标签，寻找下一个 '>' 的位置
                if (code.charAt(i + 1) == '/') {
                    // 返回 '>' 第一次出现的下标的位置
                    int j = code.indexOf('>', i + 2);
                    // 没有找到，不是合法结束标签
                    if (j < 0) {
                        return false;
                    }
                    // 获得标签名称 tag_name
                    String tagName = code.substring(i + 2, j);
                    // 栈不空，弹出栈顶元素 -- 标签名，匹配是否相同，如果不相同则不是合法标签
                    if (stack.isEmpty() || !stack.peek().equals(tagName)) {
                        return false;
                    }
                    // 相同，结束当前标签
                    stack.pop();

                    i = j + 1; // 继续遍历元素

                    // 如果栈空，说明是已遍历完所有标签，i 应该为 n
                    if (stack.isEmpty() && i != n) {
                        return false;
                    }
                } else if (code.charAt(i + 1) == '!') {
                    // 如果下一个字符是 '！'，说明是 cdata，匹配是否是合法的 cdata -- <![CDATA[CDATA_CONTENT]]>
                    // 此时栈中没有开始标签，说明不是合法标签
                    if (stack.isEmpty()) {
                        return false;
                    }
                    // 剩余的字符无法构成一个合法的 <![CDATA[
                    if (i + 9 >= n) {
                        return false;
                    }
                    String cdata = code.substring(i + 2, i + 9);
                    if (!"[CDATA[".equals(cdata)) {
                        return false;
                    }
                    // 寻找结束标志 -- ]]>
                    int j = code.indexOf("]]>", i + 9);
                    if (j < 0) {
                        return false;
                    }
                    // 继续遍历元素
                    i = j + 1;
                } else {
                    // 如果不是以上的情况，说明是一个开始标签
                    int j = code.indexOf('>', i + 1);
                    if (j < 0) {
                        return false;
                    }
                    // 标签名
                    String tagName = code.substring(i + 1, j);
                    // 判断标签名是否符合规则
                    if (tagName.length() < 1 || tagName.length() > 9) {
                        return false;
                    }
                    for (int k = 0; k < tagName.length(); ++k) {
                        // 判断是否是大写字母
                        if (!Character.isUpperCase(tagName.charAt(k))) {
                            return false;
                        }
                    }
                    // 开始标签名进栈
                    stack.push(tagName);
                    i = j + 1;
                }
            } else {
                // 说明是 tag_content，判断是否有开始标签
                if (stack.isEmpty()) {
                    return false;
                }
                ++i;
            }

        }
        // 如果栈空，是合法闭合标签
        return stack.isEmpty();
    }
}
```

## [908. 最小差值 I](https://leetcode-cn.com/problems/smallest-range-i/)

`tag: 数组、最小差值`

```java
class Solution {
    public int smallestRangeI(int[] nums, int k) {
        // 解题思路：最小差值： maxVal - minVal + diff >= 0 (-2k <= diff <= 0)
        // 遍历数组，记录最大值、最小值
        int minVal = Integer.MAX_VALUE, maxVal = Integer.MIN_VALUE;
        for (int num : nums) {
            minVal = minVal > num ? num : minVal;
            maxVal = maxVal < num ? num : maxVal;
        }
        // 最小差值
        return (maxVal - minVal - 2 * k) < 0 ? 0 : (maxVal - minVal - 2 * k);
    }
}
// 官方取最大值，最小值的方式 -- 数组流
int minNum = Arrays.stream(nums).min().getAsInt();
int maxNum = Arrays.stream(nums).max().getAsInt();
```

## [427. 建立四叉树](https://leetcode-cn.com/problems/construct-quad-tree/)

`tag: 四叉树、递归`

```java
// 解题思路：如果当前网格的值相同，则是叶子结点；否则，将当前网格划分为四个子网格，继续对这四个子网格进行递归。
class Solution {
    public Node construct(int[][] grid) {
        return dfs(grid, 0, grid.length - 1, 0, grid[0].length - 1);
    }

    private Node dfs(int[][] grid, int r0, int r1, int c0, int c1) {
        // 判断当前网格的值是否都相同
        boolean same = true;
        loop: for (int i = r0; i <= r1; ++i) {
            for (int j = c0; j <= c1; ++j) {
                // 如果不是全部相同，跳出循环
                if (grid[r0][c0] != grid[i][j]) {
                    same = false;
                    break loop;
                }
            }
        }
        // 如果当前网格数值全部相同，为叶子结点
        if (same) {
            // grid[r0][c0] == 1 -- 官方写法
            return new Node(grid[r0][c0] == 1 ? true : false, true);
        }

        // 划分
        Node node = new Node(true, false);
        node.topLeft = dfs(grid, r0, (r0 + r1) / 2, c0, (c0 + c1) / 2);
        node.topRight = dfs(grid, r0, (r0 + r1) / 2, (c0 + c1) / 2 + 1, c1);
        node.bottomLeft = dfs(grid, (r0 + r1) / 2 + 1, r1, c0, (c0 + c1) / 2);
        node.bottomRight = dfs(grid, (r0 + r1) / 2 + 1, r1, (c0 + c1) / 2 + 1, c1);
        return node;
    }
}
```

[官方题解2：使用 递归 + 二维前缀和](https://leetcode-cn.com/problems/construct-quad-tree/solution/jian-li-si-cha-shu-by-leetcode-solution-gcru/)

## [905. Sort Array By Parity](https://leetcode-cn.com/problems/sort-array-by-parity/)

`tag: 双指针、数组`

```java
// 解题思路：使用双指针，分别从头尾进行遍历，头指针遇到偶数跳过，尾指针遇到奇数跳过，否则进行交换两数。
class Solution {
    public int[] sortArrayByParity(int[] nums) {
        int n = nums.length;
        int left = 0, right = n - 1;
        while (left < right) {
            // 头指针遍历
            while (left < right && nums[left] % 2 == 0)
                ++left;
            // 尾指针遍历
            while (right > left && nums[right] % 2 == 1)
                --right;
            // 交换两个数
            int temp = nums[left];
            nums[left] = nums[right];
            nums[right] = temp;
        }
        return nums;
    }
}
```
