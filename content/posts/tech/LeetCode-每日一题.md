---
title: "LeetCode 每日一题"
date: 2022-04-28T15:57:58+08:00
lastmod: 2022-05-12
categories: 
- LeetCode
tags: 
- LeetCode
description: "LeetCode 每日一题"
draft: false
---

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
