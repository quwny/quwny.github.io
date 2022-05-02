---
title: "LeetCode 每日一题"
date: 2022-04-28T15:57:58+08:00
lastmod: 2022-05-02
categories: 
- LeetCode
tags: 
- LeetCode
description: "LeetCode 每日一题"
draft: false
---

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
