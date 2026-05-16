# LeetCode 热题 100 题库

整理日期：`2026-05-16`

说明：
- 本文档按官方 `LeetCode 热题 100` 学习计划，从仓库原始分片题库聚合而成。
- 本文档只保留题面，不混入题解内容。
- Java 题解补充见 `study_docs/11_LeetCode热题100_Java题解补充.md`。

## 哈希

> 来源：`LeetCode_Collection_Part1.md:1`

## 1. 两数之和 (Easy)

给定一个整数数组  `nums`  和一个整数目标值  `target` ，请你在该数组中找出 和为目标值  `target`   的那 两个 整数，并返回它们的数组下标。
你可以假设每种输入只会对应一个答案，并且你不能使用两次相同的元素。
你可以按任意顺序返回答案。
 
示例 1：

```text
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

示例 2：

```text
输入：nums = [3,2,4], target = 6
输出：[1,2]
```

示例 3：

```text
输入：nums = [3,3], target = 6
输出：[0,1]
```

 
提示：

 `2 <= nums.length <= 104` 
 `-109 <= nums[i] <= 109` 
 `-109 <= target <= 109` 
只会存在一个有效答案

 
进阶：你可以想出一个时间复杂度小于  `O(n2)`  的算法吗？


---

> 来源：`LeetCode_Collection_Part2.md:1930`

## 49. 字母异位词分组 (Medium)

给你一个字符串数组，请你将 字母异位词 组合在一起。可以按任意顺序返回结果列表。
 
示例 1:

输入: strs = ["eat", "tea", "tan", "ate", "nat", "bat"]
输出: [["bat"],["nat","tan"],["ate","eat","tea"]]
解释：

在 strs 中没有字符串可以通过重新排列来形成  `"bat"` 。
字符串  `"nat"`  和  `"tan"`  是字母异位词，因为它们可以重新排列以形成彼此。
字符串  `"ate"`  ， `"eat"`  和  `"tea"`  是字母异位词，因为它们可以重新排列以形成彼此。

示例 2:

输入: strs = [""]
输出: [[""]]

示例 3:

输入: strs = ["a"]
输出: [["a"]]

 
提示：

 `1 <= strs.length <= 104` 
 `0 <= strs[i].length <= 100` 
 `strs[i]`  仅包含小写字母


---

> 来源：`LeetCode_Collection_Part_5.md:273`

## 128. 最长连续序列 (Medium)

给定一个未排序的整数数组  `nums`  ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。
请你设计并实现时间复杂度为  `O(n)`  的算法解决此问题。
 
 **示例 1：** 

```text
输入：nums = [100,4,200,1,3,2]
输出：4
解释：最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。
```

 **示例 2：** 

```text
输入：nums = [0,3,7,2,5,8,4,6,0,1]
输出：9
```

 **示例 3：** 

```text
输入：nums = [1,0,1,2]
输出：3
```

 
 **提示：** 

 `0 <= nums.length <= 105` 
 `-109 <= nums[i] <= 109`

---


---

## 双指针

> 来源：`LeetCode_Collection_Part_10.md:246`

## 283. 移动零 (Easy)

给定一个数组  `nums` ，编写一个函数将所有  `0`  移动到数组的末尾，同时保持非零元素的相对顺序。
 **请注意**  ，必须在不复制数组的情况下原地对数组进行操作。

 **示例 1:**

```text
输入: nums = [0,1,0,3,12]
输出: [1,3,12,0,0]
```

 **示例 2:**

```text
输入: nums = [0]
输出: [0]
```


 **提示** :

 `1 <= nums.length <= 104`
 `-231 <= nums[i] <= 231 - 1`


 **进阶：** 你能尽量减少完成的操作次数吗？

---


---

> 来源：`LeetCode_Collection_Part1.md:1185`

## 11. 盛最多水的容器 (Medium)

给定一个长度为  `n`  的整数数组  `height`  。有  `n`  条垂线，第  `i`  条线的两个端点是  `(i, 0)`  和  `(i, height[i])`  。
找出其中的两条线，使得它们与  `x`  轴共同构成的容器可以容纳最多的水。
返回容器可以储存的最大水量。
说明：你不能倾斜容器。
 
示例 1：

```text
输入：[1,8,6,2,5,4,8,3,7]
输出：49 
解释：图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。
```

示例 2：

```text
输入：height = [1,1]
输出：1
```

 
提示：

 `n == height.length` 
 `2 <= n <= 105` 
 `0 <= height[i] <= 104`


---

> 来源：`LeetCode_Collection_Part1.md:1646`

## 15. 三数之和 (Medium)

给你一个整数数组  `nums`  ，判断是否存在三元组  `[nums[i], nums[j], nums[k]]`  满足  `i != j` 、 `i != k`  且  `j != k`  ，同时还满足  `nums[i] + nums[j] + nums[k] == 0`  。请你返回所有和为  `0`  且不重复的三元组。
注意：答案中不可以包含重复的三元组。
 
 
示例 1：

```text
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
解释：
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0 。
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0 。
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0 。
不同的三元组是 [-1,0,1] 和 [-1,-1,2] 。
注意，输出的顺序和三元组的顺序并不重要。
```

示例 2：

```text
输入：nums = [0,1,1]
输出：[]
解释：唯一可能的三元组和不为 0 。
```

示例 3：

```text
输入：nums = [0,0,0]
输出：[[0,0,0]]
解释：唯一可能的三元组和为 0 。
```

 
提示：

 `3 <= nums.length <= 3000` 
 `-105 <= nums[i] <= 105`


---

> 来源：`LeetCode_Collection_Part2.md:1192`

## 42. 接雨水 (Hard)

给定  `n`  个非负整数表示每个宽度为  `1`  的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。
 
示例 1：

```text
输入：height = [0,1,0,2,1,0,1,3,2,1,2,1]
输出：6
解释：上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。
```

示例 2：

```text
输入：height = [4,2,0,3,2,5]
输出：9
```

 
提示：

 `n == height.length` 
 `1 <= n <= 2 * 104` 
 `0 <= height[i] <= 105`


---

## 滑动窗口

> 来源：`LeetCode_Collection_Part1.md:210`

## 3. 无重复字符的最长子串 (Medium)

给定一个字符串  `s`  ，请你找出其中不含有重复字符的 最长 子串 的长度。
 
示例 1:

```text
输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。注意 "bca" 和 "cab" 也是正确答案。
```

示例 2:

```text
输入: s = "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

示例 3:

```text
输入: s = "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

 
提示：

 `0 <= s.length <= 5 * 104` 
 `s`  由英文字母、数字、符号和空格组成


---

> 来源：`LeetCode_Collection_Part_15.md:493`

## 438. 找到字符串中所有字母异位词 (Medium)

给定两个字符串  `s`  和  `p` ，找到  `s`  **** 中所有  `p`  **** 的  **异位词** 的子串，返回这些子串的起始索引。不考虑答案输出的顺序。
 
 **示例 1:** 

```text
输入: s = "cbaebabacd", p = "abc"
输出: [0,6]
解释:
起始索引等于 0 的子串是 "cba", 它是 "abc" 的异位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的异位词。
```

 **示例 2:** 

```text
输入: s = "abab", p = "ab"
输出: [0,1,2]
解释:
起始索引等于 0 的子串是 "ab", 它是 "ab" 的异位词。
起始索引等于 1 的子串是 "ba", 它是 "ab" 的异位词。
起始索引等于 2 的子串是 "ab", 它是 "ab" 的异位词。
```

 
 **提示:** 

 `1 <= s.length, p.length <= 3 * 104` 
 `s`  和  `p`  仅包含小写字母

---


---

## 子串

> 来源：`LeetCode_Collection_Part_19.md:556`

## 560. 和为 K 的子数组 (Medium)

给你一个整数数组  `nums`  和一个整数  `k`  ，请你统计并返回 该数组中和为  `k`  **** 的子数组的个数 。
子数组是数组中元素的连续非空序列。
 
 **示例 1：** 

```text
输入：nums = [1,1,1], k = 2
输出：2
```

 **示例 2：** 

```text
输入：nums = [1,2,3], k = 3
输出：2
```

 
 **提示：** 

 `1 <= nums.length <= 2 * 104` 
 `-1000 <= nums[i] <= 1000` 
 `-107 <= k <= 107`

---


---

> 来源：`LeetCode_Collection_Part_8.md:1024`

## 239. 滑动窗口最大值 (Hard)

给你一个整数数组  `nums` ，有一个大小为  `k`  的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的  `k`  个数字。滑动窗口每次只向右移动一位。
返回 滑动窗口中的最大值 。
 
 **示例 1：** 

```text
输入：nums = [1,3,-1,-3,5,3,6,7], k = 3
输出：[3,3,5,5,6,7]
解释：
滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

 **示例 2：** 

```text
输入：nums = [1], k = 1
输出：[1]
```

 
 **提示：** 

 `1 <= nums.length <= 105` 
 `-104 <= nums[i] <= 104` 
 `1 <= k <= nums.length`

---


---

> 来源：`LeetCode_Collection_Part3.md:589`

## 76. 最小覆盖子串 (Hard)

给定两个字符串  `s`  和  `t` ，长度分别是  `m`  和  `n` ，返回 s 中的 最短窗口 子串，使得该子串包含  `t`  中的每一个字符（包括重复字符）。如果没有这样的子串，返回空字符串  `""` 。
测试用例保证答案唯一。
 
示例 1：

```text
输入：s = "ADOBECODEBANC", t = "ABC"
输出："BANC"
解释：最小覆盖子串 "BANC" 包含来自字符串 t 的 'A'、'B' 和 'C'。
```

示例 2：

```text
输入：s = "a", t = "a"
输出："a"
解释：整个字符串 s 是最小覆盖子串。
```

示例 3:

```text
输入: s = "a", t = "aa"
输出: ""
解释: t 中两个字符 'a' 均应包含在 s 的子串中，
因此没有符合条件的子字符串，返回空字符串。
```

 
提示：

 `m == s.length` 
 `n == t.length` 
 `1 <= m, n <= 105` 
 `s`  和  `t`  由英文字母组成

 
进阶：你能设计一个在  `O(m + n)`  时间内解决此问题的算法吗？

---


---

## 普通数组

> 来源：`LeetCode_Collection_Part2.md:2337`

## 53. 最大子数组和 (Medium)

给你一个整数数组  `nums`  ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。
子数组 是数组中的一个连续部分。
 
示例 1：

```text
输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
输出：6
解释：连续子数组 [4,-1,2,1] 的和最大，为 6 。
```

示例 2：

```text
输入：nums = [1]
输出：1
```

示例 3：

```text
输入：nums = [5,4,-1,7,8]
输出：23
```

 
提示：

 `1 <= nums.length <= 105` 
 `-104 <= nums[i] <= 104` 

 
进阶：如果你已经实现复杂度为  `O(n)`  的解法，尝试使用更为精妙的 分治法 求解。


---

> 来源：`LeetCode_Collection_Part2.md:2614`

## 56. 合并区间 (Medium)

以数组  `intervals`  表示若干个区间的集合，其中单个区间为  `intervals[i] = [starti, endi]`  。请你合并所有重叠的区间，并返回 一个不重叠的区间数组，该数组需恰好覆盖输入中的所有区间 。
 
示例 1：

```text
输入：intervals = [[1,3],[2,6],[8,10],[15,18]]
输出：[[1,6],[8,10],[15,18]]
解释：区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].
```

示例 2：

```text
输入：intervals = [[1,4],[4,5]]
输出：[[1,5]]
解释：区间 [1,4] 和 [4,5] 可被视为重叠区间。
```

示例 3：

```text
输入：intervals = [[4,7],[1,4]]
输出：[[1,7]]
解释：区间 [1,4] 和 [4,7] 可被视为重叠区间。
```

 
提示：

 `1 <= intervals.length <= 104` 
 `intervals[i].length == 2` 
 `0 <= starti <= endi <= 104`


---

> 来源：`LeetCode_Collection_Part_7.md:401`

## 189. 轮转数组 (Medium)

给定一个整数数组  `nums` ，将数组中的元素向右轮转  `k`  个位置，其中  `k`  是非负数。

 **示例 1:**

```text
输入: nums = [1,2,3,4,5,6,7], k = 3
输出: [5,6,7,1,2,3,4]
解释:
向右轮转 1 步: [7,1,2,3,4,5,6]
向右轮转 2 步: [6,7,1,2,3,4,5]
向右轮转 3 步: [5,6,7,1,2,3,4]
```

 **示例 2:**

```text
输入：nums = [-1,-100,3,99], k = 2
输出：[3,99,-1,-100]
解释:
向右轮转 1 步: [99,-1,-100,3]
向右轮转 2 步: [3,99,-1,-100]
```


 **提示：**

 `1 <= nums.length <= 105`
 `-231 <= nums[i] <= 231 - 1`
 `0 <= k <= 105`


 **进阶：**

尽可能想出更多的解决方案，至少有  **三种**  不同的方法可以解决这个问题。
你可以使用空间复杂度为  `O(1)`  的  **原地** 算法解决这个问题吗？

---


---

> 来源：`LeetCode_Collection_Part_8.md:992`

## 238. 除了自身以外数组的乘积 (Medium)

给你一个整数数组  `nums` ，返回 数组  `answer`  ，其中  `answer[i]`  等于  `nums`  中除了  `nums[i]`  之外其余各元素的乘积 。
题目数据  **保证**  数组  `nums` 之中任意元素的全部前缀元素和后缀的乘积都在   **32 位**  整数范围内。
请  **不要使用除法，** 且在  `O(n)`  时间复杂度内完成此题。
 
 **示例 1:** 

```text
输入: nums = [1,2,3,4]
输出: [24,12,8,6]
```

 **示例 2:** 

```text
输入: nums = [-1,1,0,-3,3]
输出: [0,0,9,0,0]
```

 
 **提示：** 

 `2 <= nums.length <= 105` 
 `-30 <= nums[i] <= 30` 
输入  **保证**  数组  `answer[i]`  在   **32 位**  整数范围内

 
 **进阶：** 你可以在  `O(1)`  的额外空间复杂度内完成这个题目吗？（ 出于对空间复杂度分析的目的，输出数组  **不被视为** 额外空间。）

---


---

> 来源：`LeetCode_Collection_Part2.md:1094`

## 41. 缺失的第一个正数 (Hard)

给你一个未排序的整数数组  `nums`  ，请你找出其中没有出现的最小的正整数。
请你实现时间复杂度为  `O(n)`  并且只使用常数级别额外空间的解决方案。

 
示例 1：

```text
输入：nums = [1,2,0]
输出：3
解释：范围 [1,2] 中的数字都在数组中。
```

示例 2：

```text
输入：nums = [3,4,-1,1]
输出：2
解释：1 在数组中，但 2 没有。
```

示例 3：

```text
输入：nums = [7,8,9,11,12]
输出：1
解释：最小的正数 1 没有出现。
```

 
提示：

 `1 <= nums.length <= 105` 
 `-231 <= nums[i] <= 231 - 1`


---

## 矩阵

> 来源：`LeetCode_Collection_Part3.md:485`

## 73. 矩阵置零 (Medium)

给定一个  `m x n`  的矩阵，如果一个元素为 0 ，则将其所在行和列的所有元素都设为 0 。请使用 原地 算法。

 
示例 1：

```text
输入：matrix = [[1,1,1],[1,0,1],[1,1,1]]
输出：[[1,0,1],[0,0,0],[1,0,1]]
```

示例 2：

```text
输入：matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]
输出：[[0,0,0,0],[0,4,5,0],[0,3,1,0]]
```

 
提示：

 `m == matrix.length` 
 `n == matrix[0].length` 
 `1 <= m, n <= 200` 
 `-231 <= matrix[i][j] <= 231 - 1` 

 
进阶：

一个直观的解决方案是使用   `O(mn)`  的额外空间，但这并不是一个好的解决方案。
一个简单的改进方案是使用  `O(m + n)`  的额外空间，但这仍然不是最好的解决方案。
你能想出一个仅使用常量空间的解决方案吗？

---


---

> 来源：`LeetCode_Collection_Part2.md:2425`

## 54. 螺旋矩阵 (Medium)

给你一个  `m`  行  `n`  列的矩阵  `matrix`  ，请按照 顺时针螺旋顺序 ，返回矩阵中的所有元素。
 
示例 1：

```text
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[1,2,3,6,9,8,7,4,5]
```

示例 2：

```text
输入：matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
输出：[1,2,3,4,8,12,11,10,9,5,6,7]
```

 
提示：

 `m == matrix.length` 
 `n == matrix[i].length` 
 `1 <= m, n <= 10` 
 `-100 <= matrix[i][j] <= 100`


---

> 来源：`LeetCode_Collection_Part2.md:1839`

## 48. 旋转图像 (Medium)

给定一个 n × n 的二维矩阵  `matrix`  表示一个图像。请你将图像顺时针旋转 90 度。
你必须在 原地 旋转图像，这意味着你需要直接修改输入的二维矩阵。请不要 使用另一个矩阵来旋转图像。
 
示例 1：

```text
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[[7,4,1],[8,5,2],[9,6,3]]
```

示例 2：

```text
输入：matrix = [[5,1,9,11],[2,4,8,10],[13,3,6,7],[15,14,12,16]]
输出：[[15,13,2,5],[14,3,4,1],[12,6,8,9],[16,7,10,11]]
```

 
提示：

 `n == matrix.length == matrix[i].length` 
 `1 <= n <= 20` 
 `-1000 <= matrix[i][j] <= 1000`


---

> 来源：`LeetCode_Collection_Part_8.md:1061`

## 240. 搜索二维矩阵 II (Medium)

编写一个高效的算法来搜索  `m x n`  矩阵  `matrix`  中的一个目标值  `target`  。该矩阵具有以下特性：

每行的元素从左到右升序排列。
每列的元素从上到下升序排列。

 
 **示例 1：** 

```text
输入：matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 5
输出：true
```

 **示例 2：** 

```text
输入：matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 20
输出：false
```

 
 **提示：** 

 `m == matrix.length` 
 `n == matrix[i].length` 
 `1 <= n, m <= 300` 
 `-109 <= matrix[i][j] <= 109` 
每行的所有元素从左到右升序排列
每列的所有元素从上到下升序排列
`-109 <= target <= 109`

---

# Java 解法补充附录（211-240）

### 211. 添加与搜索单词 - 数据结构设计

**基础解法：** 用列表保存单词，搜索时逐个匹配 `.` 通配符，查询代价高。

**资深解法：** Trie 加 DFS，普通字符沿对应分支走，`.` 枚举所有子节点。

```java
class WordDictionary {
    private static class Node {
        Node[] next = new Node[26];
        boolean word;
    }
    private Node root = new Node();

    public void addWord(String word) {
        Node cur = root;
        for (char c : word.toCharArray()) {
            int i = c - 'a';
            if (cur.next[i] == null) cur.next[i] = new Node();
            cur = cur.next[i];
        }
        cur.word = true;
    }

    public boolean search(String word) {
        return dfs(word, 0, root);
    }

    private boolean dfs(String word, int i, Node node) {
        if (node == null) return false;
        if (i == word.length()) return node.word;
        char c = word.charAt(i);
        if (c != '.') return dfs(word, i + 1, node.next[c - 'a']);
        for (Node child : node.next) {
            if (dfs(word, i + 1, child)) return true;
        }
        return false;
    }
}
```

**基础语法与思想：** 递归参数携带当前下标和 Trie 节点；通配符搜索是分支 DFS。

### 212. 单词搜索 II

**基础解法：** 对每个单词单独在棋盘上 DFS，重复搜索大量前缀。

**资深解法：** 把所有单词放入 Trie，从每个格子出发 DFS，走到单词结尾即收集答案。

```java
class Solution {
    private static class Node {
        Node[] next = new Node[26];
        String word;
    }
    private List<String> ans = new ArrayList<>();

    public List<String> findWords(char[][] board, String[] words) {
        Node root = new Node();
        for (String w : words) insert(root, w);
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) dfs(board, i, j, root);
        }
        return ans;
    }

    private void insert(Node root, String word) {
        Node cur = root;
        for (char c : word.toCharArray()) {
            int i = c - 'a';
            if (cur.next[i] == null) cur.next[i] = new Node();
            cur = cur.next[i];
        }
        cur.word = word;
    }

    private void dfs(char[][] b, int i, int j, Node node) {
        if (i < 0 || i == b.length || j < 0 || j == b[0].length || b[i][j] == '#') return;
        char c = b[i][j];
        Node next = node.next[c - 'a'];
        if (next == null) return;
        if (next.word != null) {
            ans.add(next.word);
            next.word = null;
        }
        b[i][j] = '#';
        dfs(b, i + 1, j, next); dfs(b, i - 1, j, next);
        dfs(b, i, j + 1, next); dfs(b, i, j - 1, next);
        b[i][j] = c;
    }
}
```

**基础语法与思想：** 原地标记访问格；Trie 把多个单词的共同前缀合并，减少重复搜索。

### 213. 打家劫舍 II

**基础解法：** 环形房屋不能同时抢首尾，可拆成“不抢最后一间”和“不抢第一间”两个线性问题。

**资深解法：** 复用 198 的滚动 DP，分别计算 `[0,n-2]` 与 `[1,n-1]`。

```java
class Solution {
    public int rob(int[] nums) {
        int n = nums.length;
        if (n == 1) return nums[0];
        return Math.max(robLine(nums, 0, n - 2), robLine(nums, 1, n - 1));
    }

    private int robLine(int[] nums, int l, int r) {
        int prev2 = 0, prev1 = 0;
        for (int i = l; i <= r; i++) {
            int cur = Math.max(prev1, prev2 + nums[i]);
            prev2 = prev1;
            prev1 = cur;
        }
        return prev1;
    }
}
```

**基础语法与思想：** 环形约束通过分类拆除；每个线性段只需两个状态。

### 214. 最短回文串

**基础解法：** 从长到短枚举前缀是否为回文，把剩余后缀反转到前面。

**资深解法：** KMP 在 `s + "#" + reverse(s)` 中求最长回文前缀。

```java
class Solution {
    public String shortestPalindrome(String s) {
        String r = new StringBuilder(s).reverse().toString();
        String t = s + "#" + r;
        int[] lps = new int[t.length()];
        for (int i = 1; i < t.length(); i++) {
            int j = lps[i - 1];
            while (j > 0 && t.charAt(i) != t.charAt(j)) j = lps[j - 1];
            if (t.charAt(i) == t.charAt(j)) j++;
            lps[i] = j;
        }
        return r.substring(0, s.length() - lps[t.length() - 1]) + s;
    }
}
```

**基础语法与思想：** `lps` 表示最长相等前后缀；分隔符避免左右串交叉匹配。

### 215. 数组中的第 K 个最大元素

**基础解法：** 排序后返回 `nums[n-k]`，时间 `O(n log n)`。

**资深解法：** 小根堆维护最大的 `k` 个元素，堆顶即第 `k` 大。

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        PriorityQueue<Integer> heap = new PriorityQueue<>();
        for (int x : nums) {
            heap.offer(x);
            if (heap.size() > k) heap.poll();
        }
        return heap.peek();
    }
}
```

**基础语法与思想：** `PriorityQueue` 默认小根堆；也可用快速选择做到平均 `O(n)`。

### 216. 组合总和 III

**基础解法：** 回溯枚举 1 到 9 的选择，路径长度为 `k` 且和为 `n` 时收集。

**资深解法：** 利用 `start` 和剩余和剪枝，超过目标立即停止。

```java
class Solution {
    public List<List<Integer>> combinationSum3(int k, int n) {
        List<List<Integer>> ans = new ArrayList<>();
        dfs(1, k, n, new ArrayList<>(), ans);
        return ans;
    }

    private void dfs(int start, int k, int remain, List<Integer> path, List<List<Integer>> ans) {
        if (path.size() == k) {
            if (remain == 0) ans.add(new ArrayList<>(path));
            return;
        }
        for (int x = start; x <= 9 && x <= remain; x++) {
            path.add(x);
            dfs(x + 1, k, remain - x, path, ans);
            path.remove(path.size() - 1);
        }
    }
}
```

**基础语法与思想：** 回溯撤销用 `remove(size - 1)`；每个数字最多使用一次。

### 217. 存在重复元素

**基础解法：** 排序后检查相邻元素是否相等。

**资深解法：** 哈希集合，加入失败说明已出现。

```java
class Solution {
    public boolean containsDuplicate(int[] nums) {
        Set<Integer> seen = new HashSet<>();
        for (int x : nums) if (!seen.add(x)) return true;
        return false;
    }
}
```

**基础语法与思想：** `Set.add` 返回是否成功新增；哈希查重均摊 `O(n)`。

### 218. 天际线问题

**基础解法：** 扫描每个关键横坐标并计算当前最高楼，复杂度较高。

**资深解法：** 扫描线。左边界加入高度，右边界移除高度；当前最大高度变化时生成关键点。

```java
class Solution {
    public List<List<Integer>> getSkyline(int[][] buildings) {
        List<int[]> events = new ArrayList<>();
        for (int[] b : buildings) {
            events.add(new int[]{b[0], -b[2]});
            events.add(new int[]{b[1], b[2]});
        }
        events.sort((a, b) -> a[0] != b[0] ? a[0] - b[0] : a[1] - b[1]);
        TreeMap<Integer, Integer> heights = new TreeMap<>();
        heights.put(0, 1);
        int prev = 0;
        List<List<Integer>> ans = new ArrayList<>();
        for (int[] e : events) {
            int h = e[1];
            if (h < 0) heights.put(-h, heights.getOrDefault(-h, 0) + 1);
            else {
                int c = heights.get(h);
                if (c == 1) heights.remove(h);
                else heights.put(h, c - 1);
            }
            int cur = heights.lastKey();
            if (cur != prev) {
                ans.add(Arrays.asList(e[0], cur));
                prev = cur;
            }
        }
        return ans;
    }
}
```

**基础语法与思想：** `TreeMap.lastKey()` 取当前最高高度；负高度让同一横坐标的左边界优先处理。

### 219. 存在重复元素 II

**基础解法：** 对每个位置向前检查最多 `k` 个元素。

**资深解法：** 哈希表记录每个值最近出现位置，距离不超过 `k` 即成功。

```java
class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        Map<Integer, Integer> last = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            if (last.containsKey(nums[i]) && i - last.get(nums[i]) <= k) return true;
            last.put(nums[i], i);
        }
        return false;
    }
}
```

**基础语法与思想：** 最近位置足够判断最小距离；旧位置可被覆盖。

### 220. 存在重复元素 III

**基础解法：** 滑动窗口内两两比较，时间 `O(nk)`。

**资深解法：** 桶。宽度为 `valueDiff + 1`，同桶或相邻桶可能满足差值要求。

```java
class Solution {
    public boolean containsNearbyAlmostDuplicate(int[] nums, int indexDiff, int valueDiff) {
        Map<Long, Long> buckets = new HashMap<>();
        long w = (long) valueDiff + 1;
        for (int i = 0; i < nums.length; i++) {
            long id = getId(nums[i], w);
            if (buckets.containsKey(id)) return true;
            if (buckets.containsKey(id - 1) && Math.abs(nums[i] - buckets.get(id - 1)) < w) return true;
            if (buckets.containsKey(id + 1) && Math.abs(nums[i] - buckets.get(id + 1)) < w) return true;
            buckets.put(id, (long) nums[i]);
            if (i >= indexDiff) buckets.remove(getId(nums[i - indexDiff], w));
        }
        return false;
    }

    private long getId(long x, long w) {
        return x >= 0 ? x / w : (x + 1) / w - 1;
    }
}
```

**基础语法与思想：** 用 `long` 防止差值溢出；桶编号要正确处理负数。

### 221. 最大正方形

**基础解法：** 枚举每个左上角和边长检查是否全为 1。

**资深解法：** DP。若当前位置为 1，则最大边长取左、上、左上三者最小值加一。

```java
class Solution {
    public int maximalSquare(char[][] matrix) {
        int m = matrix.length, n = matrix[0].length, best = 0;
        int[][] dp = new int[m + 1][n + 1];
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (matrix[i - 1][j - 1] == '1') {
                    dp[i][j] = Math.min(Math.min(dp[i - 1][j], dp[i][j - 1]), dp[i - 1][j - 1]) + 1;
                    best = Math.max(best, dp[i][j]);
                }
            }
        }
        return best * best;
    }
}
```

**基础语法与思想：** 多一圈 DP 边界可省去越界判断；返回面积不是边长。

### 222. 完全二叉树的节点个数

**基础解法：** 普通 DFS 统计所有节点，时间 `O(n)`。

**资深解法：** 比较左右子树最左深度；深度相同则左子树满，否则右子树满。

```java
class Solution {
    public int countNodes(TreeNode root) {
        if (root == null) return 0;
        int left = depth(root.left), right = depth(root.right);
        if (left == right) return (1 << left) + countNodes(root.right);
        return (1 << right) + countNodes(root.left);
    }

    private int depth(TreeNode node) {
        int d = 0;
        while (node != null) {
            d++;
            node = node.left;
        }
        return d;
    }
}
```

**基础语法与思想：** 满二叉树节点数为 `2^h - 1`，加根后可写成 `1 << h`。

### 223. 矩形面积

**基础解法：** 两个矩形面积相加，再减去重叠部分。

**资深解法：** 重叠宽高分别由交集边界计算，若为负则按 0 处理。

```java
class Solution {
    public int computeArea(int ax1, int ay1, int ax2, int ay2, int bx1, int by1, int bx2, int by2) {
        int area1 = (ax2 - ax1) * (ay2 - ay1);
        int area2 = (bx2 - bx1) * (by2 - by1);
        int w = Math.max(0, Math.min(ax2, bx2) - Math.max(ax1, bx1));
        int h = Math.max(0, Math.min(ay2, by2) - Math.max(ay1, by1));
        return area1 + area2 - w * h;
    }
}
```

**基础语法与思想：** 一维区间交集长度模板是 `min(right) - max(left)`。

### 224. 基本计算器

**基础解法：** 递归处理括号内表达式，遇到 `)` 返回当前结果。

**资深解法：** 栈保存括号前的结果和符号，扫描一次完成加减与括号。

```java
class Solution {
    public int calculate(String s) {
        Deque<Integer> stack = new ArrayDeque<>();
        int ans = 0, sign = 1, n = s.length();
        for (int i = 0; i < n; i++) {
            char c = s.charAt(i);
            if (Character.isDigit(c)) {
                int num = 0;
                while (i < n && Character.isDigit(s.charAt(i))) num = num * 10 + s.charAt(i++) - '0';
                i--;
                ans += sign * num;
            } else if (c == '+') sign = 1;
            else if (c == '-') sign = -1;
            else if (c == '(') {
                stack.push(ans);
                stack.push(sign);
                ans = 0;
                sign = 1;
            } else if (c == ')') {
                ans = stack.pop() * ans + stack.pop();
            }
        }
        return ans;
    }
}
```

**基础语法与思想：** 栈里先放旧结果再放旧符号；遇到右括号时恢复上下文。

### 225. 用队列实现栈

**基础解法：** 两个队列搬运元素，让最后入队的元素先出。

**资深解法：** 一个队列，入队后把前面的元素轮转到队尾，使队头始终是栈顶。

```java
class MyStack {
    private Queue<Integer> q = new ArrayDeque<>();

    public void push(int x) {
        q.offer(x);
        for (int i = 0, n = q.size(); i < n - 1; i++) q.offer(q.poll());
    }

    public int pop() { return q.poll(); }
    public int top() { return q.peek(); }
    public boolean empty() { return q.isEmpty(); }
}
```

**基础语法与思想：** 队列 FIFO，通过旋转队列模拟 LIFO。

### 226. 翻转二叉树

**基础解法：** 递归交换每个节点的左右子树。

**资深解法：** BFS/DFS 迭代也可；递归最短且结构清晰。

```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if (root == null) return null;
        TreeNode left = invertTree(root.left);
        TreeNode right = invertTree(root.right);
        root.left = right;
        root.right = left;
        return root;
    }
}
```

**基础语法与思想：** 树问题常用“处理左右子树后回到当前节点”的递归模型。

### 227. 基本计算器 II

**基础解法：** 栈保存每段带符号数字，遇到乘除立即合并栈顶。

**资深解法：** 只用 `last` 保存上一段乘除链的值，`ans` 保存已确定的加减结果。

```java
class Solution {
    public int calculate(String s) {
        int ans = 0, last = 0, num = 0;
        char op = '+';
        for (int i = 0; i <= s.length(); i++) {
            char c = i == s.length() ? '+' : s.charAt(i);
            if (i < s.length() && c == ' ') continue;
            if (i < s.length() && Character.isDigit(c)) {
                num = num * 10 + c - '0';
            } else {
                if (op == '+') { ans += last; last = num; }
                else if (op == '-') { ans += last; last = -num; }
                else if (op == '*') last *= num;
                else if (op == '/') last /= num;
                op = c;
                num = 0;
            }
        }
        return ans + last;
    }
}
```

**基础语法与思想：** 乘除优先级通过延迟把 `last` 加入总和实现。

### 228. 汇总区间

**基础解法：** 双指针找到每段连续区间，单点和范围分别格式化。

**资深解法：** 一次扫描，每段起点固定，终点尽量向右扩展。

```java
class Solution {
    public List<String> summaryRanges(int[] nums) {
        List<String> ans = new ArrayList<>();
        for (int i = 0; i < nums.length; i++) {
            int start = nums[i];
            while (i + 1 < nums.length && nums[i + 1] == nums[i] + 1) i++;
            if (start == nums[i]) ans.add(String.valueOf(start));
            else ans.add(start + "->" + nums[i]);
        }
        return ans;
    }
}
```

**基础语法与思想：** `String.valueOf` 生成单点字符串；连续段判断只需看相邻差为 1。

### 229. 多数元素 II

**基础解法：** 哈希表计数，找出现次数大于 `n/3` 的元素。

**资深解法：** Boyer-Moore 扩展，最多只有两个超过 `n/3` 的候选。

```java
class Solution {
    public List<Integer> majorityElement(int[] nums) {
        int a = 0, b = 1, ca = 0, cb = 0;
        for (int x : nums) {
            if (x == a) ca++;
            else if (x == b) cb++;
            else if (ca == 0) { a = x; ca = 1; }
            else if (cb == 0) { b = x; cb = 1; }
            else { ca--; cb--; }
        }
        ca = cb = 0;
        for (int x : nums) {
            if (x == a) ca++;
            else if (x == b) cb++;
        }
        List<Integer> ans = new ArrayList<>();
        if (ca > nums.length / 3) ans.add(a);
        if (cb > nums.length / 3) ans.add(b);
        return ans;
    }
}
```

**基础语法与思想：** 候选阶段后必须二次计数验证，因为题目不保证答案存在。

### 230. 二叉搜索树中第 K 小的元素

**基础解法：** 中序遍历收集列表后取第 `k` 个。

**资深解法：** 迭代中序，访问到第 `k` 个节点立即返回。

```java
class Solution {
    public int kthSmallest(TreeNode root, int k) {
        Deque<TreeNode> stack = new ArrayDeque<>();
        while (root != null || !stack.isEmpty()) {
            while (root != null) {
                stack.push(root);
                root = root.left;
            }
            root = stack.pop();
            if (--k == 0) return root.val;
            root = root.right;
        }
        return -1;
    }
}
```

**基础语法与思想：** BST 中序有序；显式栈模拟递归左根右。

### 231. 2 的幂

**基础解法：** 不断除以 2，最后是否等于 1。

**资深解法：** 正的 2 的幂二进制只有一个 1。

```java
class Solution {
    public boolean isPowerOfTwo(int n) {
        return n > 0 && (n & (n - 1)) == 0;
    }
}
```

**基础语法与思想：** `n & (n - 1)` 删除最低位的 1。

### 232. 用栈实现队列

**基础解法：** 每次 `push` 时搬运两个栈，让输出栈顶是队头。

**资深解法：** 懒搬运：输入栈收元素，只有输出栈为空时才整体倒过去。

```java
class MyQueue {
    private Deque<Integer> in = new ArrayDeque<>();
    private Deque<Integer> out = new ArrayDeque<>();

    public void push(int x) { in.push(x); }
    public int pop() { move(); return out.pop(); }
    public int peek() { move(); return out.peek(); }
    public boolean empty() { return in.isEmpty() && out.isEmpty(); }

    private void move() {
        if (out.isEmpty()) while (!in.isEmpty()) out.push(in.pop());
    }
}
```

**基础语法与思想：** 两次栈反转恢复队列 FIFO 顺序；均摊 `O(1)`。

### 233. 数字 1 的个数

**基础解法：** 枚举 1 到 n 并逐位统计 1，时间较高。

**资深解法：** 按每一位的高位、当前位、低位贡献统计。

```java
class Solution {
    public int countDigitOne(int n) {
        long factor = 1;
        int ans = 0;
        while (factor <= n) {
            long high = n / (factor * 10);
            long cur = (n / factor) % 10;
            long low = n % factor;
            if (cur == 0) ans += high * factor;
            else if (cur == 1) ans += high * factor + low + 1;
            else ans += (high + 1) * factor;
            factor *= 10;
        }
        return ans;
    }
}
```

**基础语法与思想：** `long factor` 防止乘 10 溢出；数位 DP 常按位贡献拆分。

### 234. 回文链表

**基础解法：** 把链表值放入数组后双指针判断回文。

**资深解法：** 快慢指针找中点，反转后半段，再逐个比较。

```java
class Solution {
    public boolean isPalindrome(ListNode head) {
        ListNode slow = head, fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        ListNode second = reverse(slow);
        while (second != null) {
            if (head.val != second.val) return false;
            head = head.next;
            second = second.next;
        }
        return true;
    }

    private ListNode reverse(ListNode head) {
        ListNode prev = null;
        while (head != null) {
            ListNode next = head.next;
            head.next = prev;
            prev = head;
            head = next;
        }
        return prev;
    }
}
```

**基础语法与思想：** 后半段反转后可线性比较；若需要保持原链表，可最后再反转回来。

### 235. 二叉搜索树的最近公共祖先

**基础解法：** 分别记录根到两个节点的路径，再找最后一个相同节点。

**资深解法：** 利用 BST 有序性：两个值都小于当前节点就向左，都大于就向右，否则当前节点是 LCA。

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        while (root != null) {
            if (p.val < root.val && q.val < root.val) root = root.left;
            else if (p.val > root.val && q.val > root.val) root = root.right;
            else return root;
        }
        return null;
    }
}
```

**基础语法与思想：** BST 的左右子树值域能直接决定搜索方向。

### 236. 二叉树的最近公共祖先

**基础解法：** 建父指针表，从一个节点向上走并用集合记录祖先。

**资深解法：** 后序递归，左右子树分别找到目标时当前节点就是 LCA。

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null || root == p || root == q) return root;
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if (left != null && right != null) return root;
        return left != null ? left : right;
    }
}
```

**基础语法与思想：** 递归返回“当前子树是否含目标以及找到的祖先”。

### 237. 删除链表中的节点

**基础解法：** 若有头节点，可找到前驱后删除；本题只给待删节点，不能访问前驱。

**资深解法：** 把下一个节点的值复制到当前节点，再删除下一个节点。

```java
class Solution {
    public void deleteNode(ListNode node) {
        node.val = node.next.val;
        node.next = node.next.next;
    }
}
```

**基础语法与思想：** 本题保证待删节点不是尾节点；删除的是“下一个节点的实体”，但效果等价。

### 238. 除了自身以外数组的乘积

**基础解法：** 对每个位置计算左侧乘积和右侧乘积，使用两个额外数组。

**资深解法：** 答案数组先存前缀积，再用一个变量从右向左累乘后缀积。

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int[] ans = new int[n];
        ans[0] = 1;
        for (int i = 1; i < n; i++) ans[i] = ans[i - 1] * nums[i - 1];
        int right = 1;
        for (int i = n - 1; i >= 0; i--) {
            ans[i] *= right;
            right *= nums[i];
        }
        return ans;
    }
}
```

**基础语法与思想：** 不使用除法；当前位置答案等于左侧所有数乘右侧所有数。

### 239. 滑动窗口最大值

**基础解法：** 每个窗口遍历求最大值，时间 `O(nk)`。

**资深解法：** 单调队列保存下标，队首始终是当前窗口最大值。

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int[] ans = new int[nums.length - k + 1];
        Deque<Integer> dq = new ArrayDeque<>();
        for (int i = 0; i < nums.length; i++) {
            while (!dq.isEmpty() && dq.peekFirst() <= i - k) dq.pollFirst();
            while (!dq.isEmpty() && nums[dq.peekLast()] <= nums[i]) dq.pollLast();
            dq.offerLast(i);
            if (i >= k - 1) ans[i - k + 1] = nums[dq.peekFirst()];
        }
        return ans;
    }
}
```

**基础语法与思想：** 队列中下标对应的值保持递减；过期下标从队首移除。

### 240. 搜索二维矩阵 II

**基础解法：** 对每一行二分搜索目标，时间 `O(m log n)`。

**资深解法：** 从右上角出发，当前值大于目标则左移，小于目标则下移。

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int i = 0, j = matrix[0].length - 1;
        while (i < matrix.length && j >= 0) {
            if (matrix[i][j] == target) return true;
            if (matrix[i][j] > target) j--;
            else i++;
        }
        return false;
    }
}
```

**基础语法与思想：** 右上角同时是所在行较大值、所在列较小值，能单调排除一行或一列。


---

## 链表

> 来源：`LeetCode_Collection_Part_6.md:340`

## 160. 相交链表 (Easy)

给你两个单链表的头节点  `headA`  和  `headB`  ，请你找出并返回两个单链表相交的起始节点。如果两个链表不存在相交节点，返回  `null`  。
图示两个链表在节点  `c1`  开始相交 **：**

题目数据  **保证**  整个链式结构中不存在环。
 **注意** ，函数返回结果后，链表必须  **保持其原始结构**  。
 **自定义评测：**
 **评测系统**  的输入如下（你设计的程序  **不适用**  此输入）：

 `intersectVal`  - 相交的起始节点的值。如果不存在相交节点，这一值为  `0`
 `listA`  - 第一个链表
 `listB`  - 第二个链表
 `skipA`  - 在  `listA`  中（从头节点开始）跳到交叉节点的节点数
 `skipB`  - 在  `listB`  中（从头节点开始）跳到交叉节点的节点数

评测系统将根据这些输入创建链式数据结构，并将两个头节点  `headA`  和  `headB`  传递给你的程序。如果程序能够正确返回相交节点，那么你的解决方案将被  **视作正确答案**  。

 **示例 1：**

```text
输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,6,1,8,4,5], skipA = 2, skipB = 3
输出：Intersected at '8'
解释：相交节点的值为 8 （注意，如果两个链表相交则不能为 0）。
从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,6,1,8,4,5]。
在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。
— 请注意相交节点的值不为 1，因为在链表 A 和链表 B 之中值为 1 的节点 (A 中第二个节点和 B 中第三个节点) 是不同的节点。换句话说，它们在内存中指向两个不同的位置，而链表 A 和链表 B 中值为 8 的节点 (A 中第三个节点，B 中第四个节点) 在内存中指向相同的位置。
```


 **示例 2：**

```text
输入：intersectVal = 2, listA = [1,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
输出：Intersected at '2'
解释：相交节点的值为 2 （注意，如果两个链表相交则不能为 0）。
从各自的表头开始算起，链表 A 为 [1,9,1,2,4]，链表 B 为 [3,2,4]。
在 A 中，相交节点前有 3 个节点；在 B 中，相交节点前有 1 个节点。
```

 **示例 3：**

```text
输入：intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
输出：No intersection
解释：从各自的表头开始算起，链表 A 为 [2,6,4]，链表 B 为 [1,5]。
由于这两个链表不相交，所以 intersectVal 必须为 0，而 skipA 和 skipB 可以是任意值。
这两个链表不相交，因此返回 null 。
```


 **提示：**

 `listA`  中节点数目为  `m`
 `listB`  中节点数目为  `n`
 `1 <= m, n <= 3 * 104`
 `1 <= Node.val <= 105`
 `0 <= skipA <= m`
 `0 <= skipB <= n`
如果  `listA`  和  `listB`  没有交点， `intersectVal`  为  `0`
如果  `listA`  和  `listB`  有交点， `intersectVal == listA[skipA] == listB[skipB]`


 **进阶：** 你能否设计一个时间复杂度  `O(m + n)`  、仅用  `O(1)`  内存的解决方案？

---


---

> 来源：`LeetCode_Collection_Part_7.md:1010`

## 206. 反转链表 (Easy)

给你单链表的头节点  `head`  ，请你反转链表，并返回反转后的链表。


 **示例 1：**

```text
输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]
```

 **示例 2：**

```text
输入：head = [1,2]
输出：[2,1]
```

 **示例 3：**

```text
输入：head = []
输出：[]
```


 **提示：**

链表中节点的数目范围是  `[0, 5000]`
 `-5000 <= Node.val <= 5000`


 **进阶：** 链表可以选用迭代或递归方式完成反转。你能否用两种方法解决这道题？

---


---

> 来源：`LeetCode_Collection_Part_8.md:848`

## 234. 回文链表 (Easy)

给你一个单链表的头节点  `head`  ，请你判断该链表是否为回文链表。如果是，返回  `true`  ；否则，返回  `false`  。
 
 **示例 1：** 

```text
输入：head = [1,2,2,1]
输出：true
```

 **示例 2：** 

```text
输入：head = [1,2]
输出：false
```

 
 **提示：** 

链表中节点数目在范围 `[1, 105]`  内
 `0 <= Node.val <= 9` 

 
 **进阶：** 你能否用  `O(n)`  时间复杂度和  `O(1)`  空间复杂度解决此题？

---


---

> 来源：`LeetCode_Collection_Part_5.md:766`

## 141. 环形链表 (Easy)

给你一个链表的头节点  `head`  ，判断链表中是否有环。
如果链表中有某个节点，可以通过连续跟踪  `next`  指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数  `pos`  来表示链表尾连接到链表中的位置（索引从 0 开始）。 **注意： `pos`  不作为参数进行传递** 。仅仅是为了标识链表的实际情况。
如果链表中存在环 ，则返回  `true`  。 否则，返回  `false`  。
 
 **示例 1：** 

```text
输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。
```

 **示例 2：** 

```text
输入：head = [1,2], pos = 0
输出：true
解释：链表中有一个环，其尾部连接到第一个节点。
```

 **示例 3：** 

```text
输入：head = [1], pos = -1
输出：false
解释：链表中没有环。
```

 
 **提示：** 

链表中节点的数目范围是  `[0, 104]` 
 `-105 <= Node.val <= 105` 
 `pos`  为  `-1`  或者链表中的一个  **有效索引**  。

 
 **进阶：** 你能用  `O(1)` （即，常量）内存解决此问题吗？

---


---

> 来源：`LeetCode_Collection_Part_5.md:808`

## 142. 环形链表 II (Medium)

给定一个链表的头节点   `head`  ，返回链表开始入环的第一个节点。 如果链表无环，则返回  `null` 。
如果链表中有某个节点，可以通过连续跟踪  `next`  指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数  `pos`  来表示链表尾连接到链表中的位置（ **索引从 0 开始** ）。如果  `pos`  是  `-1` ，则在该链表中没有环。 **注意： `pos`  不作为参数进行传递** ，仅仅是为了标识链表的实际情况。
 **不允许修改** 链表。

 
 **示例 1：** 

```text
输入：head = [3,2,0,-4], pos = 1
输出：返回索引为 1 的链表节点
解释：链表中有一个环，其尾部连接到第二个节点。
```

 **示例 2：** 

```text
输入：head = [1,2], pos = 0
输出：返回索引为 0 的链表节点
解释：链表中有一个环，其尾部连接到第一个节点。
```

 **示例 3：** 

```text
输入：head = [1], pos = -1
输出：返回 null
解释：链表中没有环。
```

 
 **提示：** 

链表中节点的数目范围在范围  `[0, 104]`  内
 `-105 <= Node.val <= 105` 
 `pos`  的值为  `-1`  或者链表中的一个有效索引

 
 **进阶：** 你是否可以使用  `O(1)`  空间解决此题？

---


---

> 来源：`LeetCode_Collection_Part1.md:2327`

## 21. 合并两个有序链表 (Easy)

将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 
 
示例 1：

```text
输入：l1 = [1,2,4], l2 = [1,3,4]
输出：[1,1,2,3,4,4]
```

示例 2：

```text
输入：l1 = [], l2 = []
输出：[]
```

示例 3：

```text
输入：l1 = [], l2 = [0]
输出：[0]
```

 
提示：

两个链表的节点数目范围是  `[0, 50]` 
 `-100 <= Node.val <= 100` 
 `l1`  和  `l2`  均按 非递减顺序 排列


---

> 来源：`LeetCode_Collection_Part1.md:97`

## 2. 两数相加 (Medium)

给你两个 非空 的链表，表示两个非负的整数。它们每位数字都是按照 逆序 的方式存储的，并且每个节点只能存储 一位 数字。
请你将两个数相加，并以相同形式返回一个表示和的链表。
你可以假设除了数字 0 之外，这两个数都不会以 0 开头。
 
示例 1：

```text
输入：l1 = [2,4,3], l2 = [5,6,4]
输出：[7,0,8]
解释：342 + 465 = 807.
```

示例 2：

```text
输入：l1 = [0], l2 = [0]
输出：[0]
```

示例 3：

```text
输入：l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
输出：[8,9,9,9,0,0,0,1]
```

 
提示：

每个链表中的节点数在范围  `[1, 100]`  内
 `0 <= Node.val <= 9` 
题目数据保证列表表示的数字不含前导零


---

> 来源：`LeetCode_Collection_Part1.md:2115`

## 19. 删除链表的倒数第 N 个结点 (Medium)

给你一个链表，删除链表的倒数第  `n`  个结点，并且返回链表的头结点。
 
示例 1：

```text
输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]
```

示例 2：

```text
输入：head = [1], n = 1
输出：[]
```

示例 3：

```text
输入：head = [1,2], n = 1
输出：[1]
```

 
提示：

链表中结点的数目为  `sz` 
 `1 <= sz <= 30` 
 `0 <= Node.val <= 100` 
 `1 <= n <= sz` 

 
进阶：你能尝试使用一趟扫描实现吗？


---

> 来源：`LeetCode_Collection_Part1.md:2647`

## 24. 两两交换链表中的节点 (Medium)

给你一个链表，两两交换其中相邻的节点，并返回交换后链表的头节点。你必须在不修改节点内部的值的情况下完成本题（即，只能进行节点交换）。
 
示例 1：

```text
输入：head = [1,2,3,4]
输出：[2,1,4,3]
```

示例 2：

```text
输入：head = []
输出：[]
```

示例 3：

```text
输入：head = [1]
输出：[1]
```

 
提示：

链表中节点的数目在范围  `[0, 100]`  内
 `0 <= Node.val <= 100`


---

> 来源：`LeetCode_Collection_Part1.md:2735`

## 25. K 个一组翻转链表 (Hard)

给你链表的头节点  `head`  ，每  `k`  个节点一组进行翻转，请你返回修改后的链表。
 `k`  是一个正整数，它的值小于或等于链表的长度。如果节点总数不是  `k`  的整数倍，那么请将最后剩余的节点保持原有顺序。
你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。
 
示例 1：

```text
输入：head = [1,2,3,4,5], k = 2
输出：[2,1,4,3,5]
```

示例 2：

```text
输入：head = [1,2,3,4,5], k = 3
输出：[3,2,1,4,5]
```

 
提示：

链表中的节点数目为  `n` 
 `1 <= k <= n <= 5000` 
 `0 <= Node.val <= 1000` 

 
进阶：你可以设计一个只用  `O(1)`  额外内存空间的算法解决此问题吗？


---

> 来源：`LeetCode_Collection_Part_5.md:644`

## 138. 随机链表的复制 (Medium)

给你一个长度为  `n`  的链表，每个节点包含一个额外增加的随机指针  `random`  ，该指针可以指向链表中的任何节点或空节点。
构造这个链表的  **深拷贝** 。 深拷贝应该正好由  `n`  个  **全新**  节点组成，其中每个新节点的值都设为其对应的原节点的值。新节点的  `next`  指针和  `random`  指针也都应指向复制链表中的新节点，并使原链表和复制链表中的这些指针能够表示相同的链表状态。 **复制链表中的指针都不应指向原链表中的节点** 。
例如，如果原链表中有  `X`  和  `Y`  两个节点，其中  `X.random --> Y`  。那么在复制链表中对应的两个节点  `x`  和  `y`  ，同样有  `x.random --> y`  。
返回复制链表的头节点。
用一个由  `n`  个节点组成的链表来表示输入/输出中的链表。每个节点用一个  `[val, random_index]`  表示：

 `val` ：一个表示  `Node.val`  的整数。
 `random_index` ：随机指针指向的节点索引（范围从  `0`  到  `n-1` ）；如果不指向任何节点，则为   `null`  。

你的代码  **只**  接受原链表的头节点  `head`  作为传入参数。
 
 **示例 1：** 

```text
输入：head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
输出：[[7,null],[13,0],[11,4],[10,2],[1,0]]
```

 **示例 2：** 

```text
输入：head = [[1,1],[2,1]]
输出：[[1,1],[2,1]]
```

 **示例 3：** 
 **** 

```text
输入：head = [[3,null],[3,0],[3,null]]
输出：[[3,null],[3,0],[3,null]]
```

 
 **提示：** 

 `0 <= n <= 1000` 
 `-104 <= Node.val <= 104` 
 `Node.random`  为  `null`  或指向链表中的节点。

---


---

> 来源：`LeetCode_Collection_Part_5.md:1042`

## 148. 排序链表 (Medium)

给你链表的头结点  `head`  ，请将其按  **升序**  排列并返回  **排序后的链表**  。

 
 **示例 1：** 

```text
输入：head = [4,2,1,3]
输出：[1,2,3,4]
```

 **示例 2：** 

```text
输入：head = [-1,5,3,4,0]
输出：[-1,0,3,4,5]
```

 **示例 3：** 

```text
输入：head = []
输出：[]
```

 
 **提示：** 

链表中节点的数目在范围  `[0, 5 * 104]`  内
 `-105 <= Node.val <= 105` 

 
 **进阶：** 你可以在  `O(n log n)`  时间复杂度和常数级空间复杂度下，对链表进行排序吗？

---


---

> 来源：`LeetCode_Collection_Part1.md:2529`

## 23. 合并 K 个升序链表 (Hard)

给你一个链表数组，每个链表都已经按升序排列。
请你将所有链表合并到一个升序链表中，返回合并后的链表。
 
示例 1：

```text
输入：lists = [[1,4,5],[1,3,4],[2,6]]
输出：[1,1,2,3,4,4,5,6]
解释：链表数组如下：
[
  1->4->5,
  1->3->4,
  2->6
]
将它们合并到一个有序链表中得到。
1->1->2->3->4->4->5->6
```

示例 2：

```text
输入：lists = []
输出：[]
```

示例 3：

```text
输入：lists = [[]]
输出：[]
```

 
提示：

 `k == lists.length` 
 `0 <= k <= 10^4` 
 `0 <= lists[i].length <= 500` 
 `-10^4 <= lists[i][j] <= 10^4` 
 `lists[i]`  按 升序 排列
 `lists[i].length`  的总和不超过  `10^4`


---

> 来源：`LeetCode_Collection_Part_5.md:963`

## 146. LRU 缓存 (Medium)

请你设计并实现一个满足  LRU (最近最少使用) 缓存 约束的数据结构。
实现  `LRUCache`  类：

 `LRUCache(int capacity)`  以  **正整数**  作为容量  `capacity`  初始化 LRU 缓存
 `int get(int key)`  如果关键字  `key`  存在于缓存中，则返回关键字的值，否则返回  `-1`  。
 `void put(int key, int value)`  如果关键字  `key`  已经存在，则变更其数据值  `value`  ；如果不存在，则向缓存中插入该组  `key-value`  。如果插入操作导致关键字数量超过  `capacity`  ，则应该  **逐出**  最久未使用的关键字。

函数  `get`  和  `put`  必须以  `O(1)`  的平均时间复杂度运行。

 
 **示例：** 

```text
输入
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
输出
[null, null, null, 1, null, -1, null, -1, 3, 4]

解释
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // 缓存是 {1=1}
lRUCache.put(2, 2); // 缓存是 {1=1, 2=2}
lRUCache.get(1);    // 返回 1
lRUCache.put(3, 3); // 该操作会使得关键字 2 作废，缓存是 {1=1, 3=3}
lRUCache.get(2);    // 返回 -1 (未找到)
lRUCache.put(4, 4); // 该操作会使得关键字 1 作废，缓存是 {4=4, 3=3}
lRUCache.get(1);    // 返回 -1 (未找到)
lRUCache.get(3);    // 返回 3
lRUCache.get(4);    // 返回 4
```

 
 **提示：** 

 `1 <= capacity <= 3000` 
 `0 <= key <= 10000` 
 `0 <= value <= 105` 
最多调用  `2 * 105`  次  `get`  和  `put`

---


---

## 二叉树

> 来源：`LeetCode_Collection_Part4.md:120`

## 94. 二叉树的中序遍历 (Easy)

给定一个二叉树的根节点  `root`  ，返回 它的 中序 遍历 。
 
示例 1：

```text
输入：root = [1,null,2,3]
输出：[1,3,2]
```

示例 2：

```text
输入：root = []
输出：[]
```

示例 3：

```text
输入：root = [1]
输出：[1]
```

 
提示：

树中节点数目在范围  `[0, 100]`  内
 `-100 <= Node.val <= 100` 

 
进阶: 递归算法很简单，你可以通过迭代算法完成吗？

---


---

> 来源：`LeetCode_Collection_Part_4.md:98`

## 104. 二叉树的最大深度 (Easy)

给定一个二叉树  `root`  ，返回其最大深度。
二叉树的  **最大深度**  是指从根节点到最远叶子节点的最长路径上的节点数。
 
 **示例 1：** 

 

```text
输入：root = [3,9,20,null,null,15,7]
输出：3
```

 **示例 2：** 

```text
输入：root = [1,null,2]
输出：2
```

 
 **提示：** 

树中节点的数量在  `[0, 104]`  区间内。
 `-100 <= Node.val <= 100`

---


---

> 来源：`LeetCode_Collection_Part_8.md:554`

## 226. 翻转二叉树 (Easy)

给你一棵二叉树的根节点  `root`  ，翻转这棵二叉树，并返回其根节点。
 
 **示例 1：** 

```text
输入：root = [4,2,7,1,3,6,9]
输出：[4,7,2,9,6,3,1]
```

 **示例 2：** 

```text
输入：root = [2,1,3]
输出：[2,3,1]
```

 **示例 3：** 

```text
输入：root = []
输出：[]
```

 
 **提示：** 

树中节点数目范围在  `[0, 100]`  内
 `-100 <= Node.val <= 100`

---


---

> 来源：`LeetCode_Collection_Part_4.md:3`

## 101. 对称二叉树 (Easy)

给你一个二叉树的根节点  `root`  ， 检查它是否轴对称。
 
 **示例 1：** 

```text
输入：root = [1,2,2,3,4,4,3]
输出：true
```

 **示例 2：** 

```text
输入：root = [1,2,2,null,3,null,3]
输出：false
```

 
 **提示：** 

树中节点数目在范围  `[1, 1000]`  内
 `-100 <= Node.val <= 100` 

 
 **进阶：** 你可以运用递归和迭代两种方法解决这个问题吗？

---


---

> 来源：`LeetCode_Collection_Part_19.md:65`

## 543. 二叉树的直径 (Easy)

给你一棵二叉树的根节点，返回该树的  **直径**  。
二叉树的  **直径**  是指树中任意两个节点之间最长路径的  **长度**  。这条路径可能经过也可能不经过根节点  `root`  。
两节点之间路径的  **长度**  由它们之间边数表示。
 
 **示例 1：** 

```text
输入：root = [1,2,3,4,5]
输出：3
解释：3 ，取路径 [4,2,1,3] 或 [5,2,1,3] 的长度。
```

 **示例 2：** 

```text
输入：root = [1,2]
输出：1
```

 
 **提示：** 

树中节点数目在范围  `[1, 104]`  内
 `-100 <= Node.val <= 100`

---


---

> 来源：`LeetCode_Collection_Part_4.md:32`

## 102. 二叉树的层序遍历 (Medium)

给你二叉树的根节点  `root`  ，返回其节点值的  **层序遍历**  。 （即逐层地，从左到右访问所有节点）。
 
 **示例 1：** 

```text
输入：root = [3,9,20,null,null,15,7]
输出：[[3],[9,20],[15,7]]
```

 **示例 2：** 

```text
输入：root = [1]
输出：[[1]]
```

 **示例 3：** 

```text
输入：root = []
输出：[]
```

 
 **提示：** 

树中节点数目在范围  `[0, 2000]`  内
 `-1000 <= Node.val <= 1000`

---


---

> 来源：`LeetCode_Collection_Part_4.md:222`

## 108. 将有序数组转换为二叉搜索树 (Easy)

给你一个整数数组  `nums`  ，其中元素已经按  **升序**  排列，请你将其转换为一棵 平衡 二叉搜索树。
 
 **示例 1：** 

```text
输入：nums = [-10,-3,0,5,9]
输出：[0,-3,9,-10,null,5]
解释：[0,-10,5,null,-3,null,9] 也将被视为正确答案：
```

 **示例 2：** 

```text
输入：nums = [1,3]
输出：[3,1]
解释：[1,null,3] 和 [3,1] 都是高度平衡二叉搜索树。
```

 
 **提示：** 

 `1 <= nums.length <= 104` 
 `-104 <= nums[i] <= 104` 
 `nums`  按  **严格递增**  顺序排列

---


---

> 来源：`LeetCode_Collection_Part4.md:252`

## 98. 验证二叉搜索树 (Medium)

给你一个二叉树的根节点  `root`  ，判断其是否是一个有效的二叉搜索树。
有效 二叉搜索树定义如下：

节点的左子树只包含 严格小于 当前节点的数。
节点的右子树只包含 严格大于 当前节点的数。
所有左子树和右子树自身必须也是二叉搜索树。

 
示例 1：

```text
输入：root = [2,1,3]
输出：true
```

示例 2：

```text
输入：root = [5,1,4,null,null,3,6]
输出：false
解释：根节点的值是 5 ，但是右子节点的值是 4 。
```

 
提示：

树中节点数目范围在 `[1, 104]`  内
 `-231 <= Node.val <= 231 - 1`

---


---

> 来源：`LeetCode_Collection_Part_8.md:706`

## 230. 二叉搜索树中第 K 小的元素 (Medium)

给定一个二叉搜索树的根节点  `root`  ，和一个整数  `k`  ，请你设计一个算法查找其中第  `k`  **** 小的元素（ `k`  从 1 开始计数）。
 
 **示例 1：** 

```text
输入：root = [3,1,4,null,2], k = 1
输出：1
```

 **示例 2：** 

```text
输入：root = [5,3,6,2,4,null,null,1], k = 3
输出：3
```

 
 
 **提示：** 

树中的节点数为  `n`  。
 `1 <= k <= n <= 104` 
 `0 <= Node.val <= 104` 

 
 **进阶：** 如果二叉搜索树经常被修改（插入/删除操作）并且你需要频繁地查找第  `k`  小的值，你将如何优化算法？

---


---

> 来源：`LeetCode_Collection_Part_7.md:763`

## 199. 二叉树的右视图 (Medium)

给定一个二叉树的  **根节点**   `root` ，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。

 **示例 1：**

 **输入：** root = [1,2,3,null,5,null,4]
 **输出：** [1,3,4]
 **解释：**

 **示例 2：**

 **输入：** root = [1,2,3,4,null,null,null,5]
 **输出：** [1,3,4,5]
 **解释：**

 **示例 3：**

 **输入：** root = [1,null,3]
 **输出：** [1,3]

 **示例 4：**

 **输入：** root = []
 **输出：** []


 **提示:**

二叉树的节点个数的范围是  `[0,100]`
 `-100 <= Node.val <= 100`

---


---

> 来源：`LeetCode_Collection_Part_4.md:416`

## 114. 二叉树展开为链表 (Medium)

给你二叉树的根结点  `root`  ，请你将它展开为一个单链表：

展开后的单链表应该同样使用  `TreeNode`  ，其中  `right`  子指针指向链表中下一个结点，而左子指针始终为  `null`  。
展开后的单链表应该与二叉树  **先序遍历**  顺序相同。

 
 **示例 1：** 

```text
输入：root = [1,2,5,3,4,null,6]
输出：[1,null,2,null,3,null,4,null,5,null,6]
```

 **示例 2：** 

```text
输入：root = []
输出：[]
```

 **示例 3：** 

```text
输入：root = [0]
输出：[0]
```

 
 **提示：** 

树中结点数在范围  `[0, 2000]`  内
 `-100 <= Node.val <= 100` 

 
 **进阶：** 你可以使用原地算法（ `O(1)`  额外空间）展开这棵树吗？

---


---

> 来源：`LeetCode_Collection_Part_4.md:127`

## 105. 从前序与中序遍历序列构造二叉树 (Medium)

给定两个整数数组  `preorder`  和  `inorder`  ，其中  `preorder`  是二叉树的 **先序遍历** ，  `inorder`  是同一棵树的 **中序遍历** ，请构造二叉树并返回其根节点。
 
 **示例 1:** 

```text
输入: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
输出: [3,9,20,null,null,15,7]
```

 **示例 2:** 

```text
输入: preorder = [-1], inorder = [-1]
输出: [-1]
```

 
 **提示:** 

 `1 <= preorder.length <= 3000` 
 `inorder.length == preorder.length` 
 `-3000 <= preorder[i], inorder[i] <= 3000` 
 `preorder`  和  `inorder`  均  **无重复**  元素
 `inorder`  均出现在  `preorder` 
 `preorder`   **保证**  为二叉树的前序遍历序列
 `inorder`   **保证**  为二叉树的中序遍历序列

---


---

> 来源：`LeetCode_Collection_Part_15.md:464`

## 437. 路径总和 III (Medium)

给定一个二叉树的根节点  `root`  ，和一个整数  `targetSum`  ，求该二叉树里节点值之和等于  `targetSum`  的  **路径**  的数目。
 **路径**  不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。
 
 **示例 1：** 

```text
输入：root = [10,5,-3,3,2,null,11,3,-2,null,1], targetSum = 8
输出：3
解释：和等于 8 的路径有 3 条，如图所示。
```

 **示例 2：** 

```text
输入：root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
输出：3
```

 
 **提示:** 

二叉树的节点个数的范围是  `[0,1000]` 
 `-109 <= Node.val <= 109`  
 `-1000 <= targetSum <= 1000`

---


---

> 来源：`LeetCode_Collection_Part_8.md:908`

## 236. 二叉树的最近公共祖先 (Medium)

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。
百度百科中最近公共祖先的定义为：“对于有根树 T 的两个节点 p、q，最近公共祖先表示为一个节点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（ **一个节点也可以是它自己的祖先** ）。”
 
 **示例 1：** 

```text
输入：root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
输出：3
解释：节点 5 和节点 1 的最近公共祖先是节点 3 。
```

 **示例 2：** 

```text
输入：root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
输出：5
解释：节点 5 和节点 4 的最近公共祖先是节点 5 。因为根据定义最近公共祖先节点可以为节点本身。
```

 **示例 3：** 

```text
输入：root = [1,2], p = 1, q = 2
输出：1
```

 
 **提示：** 

树中节点数目在范围  `[2, 105]`  内。
 `-109 <= Node.val <= 109` 
所有  `Node.val`   `互不相同`  。
 `p != q` 
 `p`  和  `q`  均存在于给定的二叉树中。

---


---

> 来源：`LeetCode_Collection_Part_5.md:123`

## 124. 二叉树中的最大路径和 (Hard)

二叉树中的 **路径**  被定义为一条节点序列，序列中每对相邻节点之间都存在一条边。同一个节点在一条路径序列中  **至多出现一次**  。该路径 **至少包含一个** 节点，且不一定经过根节点。
 **路径和**  是路径中各节点值的总和。
给你一个二叉树的根节点  `root`  ，返回其  **最大路径和**  。
 
 **示例 1：** 

```text
输入：root = [1,2,3]
输出：6
解释：最优路径是 2 -> 1 -> 3 ，路径和为 2 + 1 + 3 = 6
```

 **示例 2：** 

```text
输入：root = [-10,9,20,null,null,15,7]
输出：42
解释：最优路径是 15 -> 20 -> 7 ，路径和为 15 + 20 + 7 = 42
```

 
 **提示：** 

树中节点数目范围是  `[1, 3 * 104]` 
 `-1000 <= Node.val <= 1000`

---


---

## 图论

> 来源：`LeetCode_Collection_Part_7.md:797`

## 200. 岛屿数量 (Medium)

给你一个由  `'1'` （陆地）和  `'0'` （水）组成的的二维网格，请你计算网格中岛屿的数量。
岛屿总是被水包围，并且每座岛屿只能由水平方向和/或竖直方向上相邻的陆地连接形成。
此外，你可以假设该网格的四条边均被水包围。

 **示例 1：**

```text
输入：grid = [
  ['1','1','1','1','0'],
  ['1','1','0','1','0'],
  ['1','1','0','0','0'],
  ['0','0','0','0','0']
]
输出：1
```

 **示例 2：**

```text
输入：grid = [
  ['1','1','0','0','0'],
  ['1','1','0','0','0'],
  ['0','0','1','0','0'],
  ['0','0','0','1','1']
]
输出：3
```


 **提示：**

 `m == grid.length`
 `n == grid[i].length`
 `1 <= m, n <= 300`
 `grid[i][j]`  的值为  `'0'`  或  `'1'`

---


---

> 来源：`LeetCode_Collection_Part_34.md:113`

## 994. 腐烂的橘子 (Medium)

在给定的  `m x n`  网格  `grid`  中，每个单元格可以有以下三个值之一：

值  `0`  代表空单元格；
值  `1`  代表新鲜橘子；
值  `2`  代表腐烂的橘子。

每分钟，腐烂的橘子  **周围 4 个方向上相邻**  的新鲜橘子都会腐烂。
返回 直到单元格中没有新鲜橘子为止所必须经过的最小分钟数。如果不可能，返回  `-1`  。
 
 **示例 1：** 
 **** 

```text
输入：grid = [[2,1,1],[1,1,0],[0,1,1]]
输出：4
```

 **示例 2：** 

```text
输入：grid = [[2,1,1],[0,1,1],[1,0,1]]
输出：-1
解释：左下角的橘子（第 2 行， 第 0 列）永远不会腐烂，因为腐烂只会发生在 4 个方向上。
```

 **示例 3：** 

```text
输入：grid = [[0,2]]
输出：0
解释：因为 0 分钟时已经没有新鲜橘子了，所以答案就是 0 。
```

 
 **提示：** 

 `m == grid.length` 
 `n == grid[i].length` 
 `1 <= m, n <= 10` 
 `grid[i][j]`  仅为  `0` 、 `1`  或  `2`

---


---

> 来源：`LeetCode_Collection_Part_7.md:1047`

## 207. 课程表 (Medium)

你这个学期必须选修  `numCourses`  门课程，记为  `0`  到  `numCourses - 1`  。
在选修某些课程之前需要一些先修课程。 先修课程按数组  `prerequisites`  给出，其中  `prerequisites[i] = [ai, bi]`  ，表示如果要学习课程  `ai`  则  **必须**  先学习课程   `bi`  。

例如，先修课程对  `[0, 1]`  表示：想要学习课程  `0`  ，你需要先完成课程  `1`  。

请你判断是否可能完成所有课程的学习？如果可以，返回  `true`  ；否则，返回  `false`  。

 **示例 1：**

```text
输入：numCourses = 2, prerequisites = [[1,0]]
输出：true
解释：总共有 2 门课程。学习课程 1 之前，你需要完成课程 0 。这是可能的。
```

 **示例 2：**

```text
输入：numCourses = 2, prerequisites = [[1,0],[0,1]]
输出：false
解释：总共有 2 门课程。学习课程 1 之前，你需要先完成​课程 0 ；并且学习课程 0 之前，你还应先完成课程 1 。这是不可能的。
```


 **提示：**

 `1 <= numCourses <= 2000`
 `0 <= prerequisites.length <= 5000`
 `prerequisites[i].length == 2`
 `0 <= ai, bi < numCourses`
 `prerequisites[i]`  中的所有课程对  **互不相同**

---


---

> 来源：`LeetCode_Collection_Part_7.md:1083`

## 208. 实现 Trie (前缀树) (Medium)

**Trie** （发音类似 "try"）或者说  **前缀树**  是一种树形数据结构，用于高效地存储和检索字符串数据集中的键。这一数据结构有相当多的应用情景，例如自动补全和拼写检查。
请你实现 Trie 类：

 `Trie()`  初始化前缀树对象。
 `void insert(String word)`  向前缀树中插入字符串  `word`  。
 `boolean search(String word)`  如果字符串  `word`  在前缀树中，返回  `true` （即，在检索之前已经插入）；否则，返回  `false`  。
 `boolean startsWith(String prefix)`  如果之前已经插入的字符串  `word`  的前缀之一为  `prefix`  ，返回  `true`  ；否则，返回  `false`  。


 **示例：**

```text
输入
["Trie", "insert", "search", "search", "startsWith", "insert", "search"]
[[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]
输出
[null, null, true, false, true, null, true]

解释
Trie trie = new Trie();
trie.insert("apple");
trie.search("apple");   // 返回 True
trie.search("app");     // 返回 False
trie.startsWith("app"); // 返回 True
trie.insert("app");
trie.search("app");     // 返回 True
```


 **提示：**

 `1 <= word.length, prefix.length <= 2000`
 `word`  和  `prefix`  仅由小写英文字母组成
 `insert` 、 `search`  和  `startsWith`  调用次数  **总计**  不超过  `3 * 104`  次

---


---

## 回溯

> 来源：`LeetCode_Collection_Part2.md:1614`

## 46. 全排列 (Medium)

给定一个不含重复数字的数组  `nums`  ，返回其 所有可能的全排列 。你可以 按任意顺序 返回答案。
 
示例 1：

```text
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

示例 2：

```text
输入：nums = [0,1]
输出：[[0,1],[1,0]]
```

示例 3：

```text
输入：nums = [1]
输出：[[1]]
```

 
提示：

 `1 <= nums.length <= 6` 
 `-10 <= nums[i] <= 10` 
 `nums`  中的所有整数 互不相同


---

> 来源：`LeetCode_Collection_Part3.md:667`

## 78. 子集 (Medium)

给你一个整数数组  `nums`  ，数组中的元素 互不相同 。返回该数组所有可能的子集（幂集）。
解集 不能 包含重复的子集。你可以按 任意顺序 返回解集。
 
示例 1：

```text
输入：nums = [1,2,3]
输出：[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```

示例 2：

```text
输入：nums = [0]
输出：[[],[0]]
```

 
提示：

 `1 <= nums.length <= 10` 
 `-10 <= nums[i] <= 10` 
 `nums`  中的所有元素 互不相同

---


---

> 来源：`LeetCode_Collection_Part1.md:1882`

## 17. 电话号码的字母组合 (Medium)

给定一个仅包含数字  `2-9`  的字符串，返回所有它能表示的字母组合。答案可以按 任意顺序 返回。
给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

 
示例 1：

```text
输入：digits = "23"
输出：["ad","ae","af","bd","be","bf","cd","ce","cf"]
```

示例 2：

```text
输入：digits = "2"
输出：["a","b","c"]
```

 
提示：

 `1 <= digits.length <= 4` 
 `digits[i]`  是范围  `['2', '9']`  的一个数字。


---

> 来源：`LeetCode_Collection_Part2.md:865`

## 39. 组合总和 (Medium)

给你一个 无重复元素 的整数数组  `candidates`  和一个目标整数  `target`  ，找出  `candidates`  中可以使数字和为目标数  `target`  的 所有 不同组合 ，并以列表形式返回。你可以按 任意顺序 返回这些组合。
 `candidates`  中的 同一个 数字可以 无限制重复被选取 。如果至少一个数字的被选数量不同，则两种组合是不同的。 
对于给定的输入，保证和为  `target`  的不同组合数少于  `150`  个。
 
示例 1：

```text
输入：candidates = [2,3,6,7], target = 7
输出：[[2,2,3],[7]]
解释：
2 和 3 可以形成一组候选，2 + 2 + 3 = 7 。注意 2 可以使用多次。
7 也是一个候选， 7 = 7 。
仅有这两种组合。
```

示例 2：

```text
输入: candidates = [2,3,5], target = 8
输出: [[2,2,2,2],[2,3,3],[3,5]]
```

示例 3：

```text
输入: candidates = [2], target = 1
输出: []
```

 
提示：

 `1 <= candidates.length <= 30` 
 `2 <= candidates[i] <= 40` 
 `candidates`  的所有元素 互不相同
 `1 <= target <= 40`


---

> 来源：`LeetCode_Collection_Part1.md:2419`

## 22. 括号生成 (Medium)

数字  `n`  代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 有效的 括号组合。
 
示例 1：

```text
输入：n = 3
输出：["((()))","(()())","(())()","()(())","()()()"]
```

示例 2：

```text
输入：n = 1
输出：["()"]
```

 
提示：

 `1 <= n <= 8`


---

> 来源：`LeetCode_Collection_Part3.md:695`

## 79. 单词搜索 (Medium)

给定一个  `m x n`  二维字符网格  `board`  和一个字符串单词  `word`  。如果  `word`  存在于网格中，返回  `true`  ；否则，返回  `false`  。
单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。
 
示例 1：

```text
输入：board = [['A','B','C','E'],['S','F','C','S'],['A','D','E','E']], word = "ABCCED"
输出：true
```

示例 2：

```text
输入：board = [['A','B','C','E'],['S','F','C','S'],['A','D','E','E']], word = "SEE"
输出：true
```

示例 3：

```text
输入：board = [['A','B','C','E'],['S','F','C','S'],['A','D','E','E']], word = "ABCB"
输出：false
```

 
提示：

 `m == board.length` 
 `n = board[i].length` 
 `1 <= m, n <= 6` 
 `1 <= word.length <= 15` 
 `board`  和  `word`  仅由大小写英文字母组成

 
进阶：你可以使用搜索剪枝的技术来优化解决方案，使其在  `board`  更大的情况下可以更快解决问题？

---


---

> 来源：`LeetCode_Collection_Part_5.md:385`

## 131. 分割回文串 (Medium)

给你一个字符串  `s` ，请你将  `s`  分割成一些 子串，使每个子串都是  **回文串**  。返回  `s`  所有可能的分割方案。
 
 **示例 1：** 

```text
输入：s = "aab"
输出：[["a","a","b"],["aa","b"]]
```

 **示例 2：** 

```text
输入：s = "a"
输出：[["a"]]
```

 
 **提示：** 

 `1 <= s.length <= 16` 
 `s`  仅由小写英文字母组成

---


---

> 来源：`LeetCode_Collection_Part2.md:2118`

## 51. N 皇后 (Hard)

按照国际象棋的规则，皇后可以攻击与之处在同一行或同一列或同一斜线上的棋子。
n 皇后问题 研究的是如何将  `n`  个皇后放置在  `n×n`  的棋盘上，并且使皇后彼此之间不能相互攻击。
给你一个整数  `n`  ，返回所有不同的 n 皇后问题 的解决方案。

每一种解法包含一个不同的 n 皇后问题 的棋子放置方案，该方案中  `'Q'`  和  `'.'`  分别代表了皇后和空位。
 
示例 1：

```text
输入：n = 4
输出：[[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
解释：如上图所示，4 皇后问题存在两个不同的解法。
```

示例 2：

```text
输入：n = 1
输出：[["Q"]]
```

 
提示：

 `1 <= n <= 9`


---

## 二分查找

> 来源：`LeetCode_Collection_Part2.md:432`

## 35. 搜索插入位置 (Easy)

给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。
请必须使用时间复杂度为  `O(log n)`  的算法。
 
示例 1:

```text
输入: nums = [1,3,5,6], target = 5
输出: 2
```

示例 2:

```text
输入: nums = [1,3,5,6], target = 2
输出: 1
```

示例 3:

```text
输入: nums = [1,3,5,6], target = 7
输出: 4
```

 
提示:

 `1 <= nums.length <= 104` 
 `-104 <= nums[i] <= 104` 
 `nums`  为 无重复元素 的 升序 排列数组
 `-104 <= target <= 104`


---

> 来源：`LeetCode_Collection_Part3.md:521`

## 74. 搜索二维矩阵 (Medium)

给你一个满足下述两条属性的  `m x n`  整数矩阵：

每行中的整数从左到右按非严格递增顺序排列。
每行的第一个整数大于前一行的最后一个整数。

给你一个整数  `target`  ，如果  `target`  在矩阵中，返回  `true`  ；否则，返回  `false`  。
 
示例 1：

```text
输入：matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3
输出：true
```

示例 2：

```text
输入：matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 13
输出：false
```

 
提示：

 `m == matrix.length` 
 `n == matrix[i].length` 
 `1 <= m, n <= 100` 
 `-104 <= matrix[i][j], target <= 104`

---


---

> 来源：`LeetCode_Collection_Part2.md:335`

## 34. 在排序数组中查找元素的第一个和最后一个位置 (Medium)

给你一个按照非递减顺序排列的整数数组  `nums` ，和一个目标值  `target` 。请你找出给定目标值在数组中的开始位置和结束位置。
如果数组中不存在目标值  `target` ，返回  `[-1, -1]` 。
你必须设计并实现时间复杂度为  `O(log n)`  的算法解决此问题。
 
示例 1：

```text
输入：nums = [5,7,7,8,8,10], target = 8
输出：[3,4]
```

示例 2：

```text
输入：nums = [5,7,7,8,8,10], target = 6
输出：[-1,-1]
```

示例 3：

```text
输入：nums = [], target = 0
输出：[-1,-1]
```

 
提示：

 `0 <= nums.length <= 105` 
 `-109 <= nums[i] <= 109` 
 `nums`  是一个非递减数组
 `-109 <= target <= 109`


---

> 来源：`LeetCode_Collection_Part2.md:244`

## 33. 搜索旋转排序数组 (Medium)

整数数组  `nums`  按升序排列，数组中的值 互不相同 。
在传递给函数之前， `nums`  在预先未知的某个下标  `k` （ `0 <= k < nums.length` ）上进行了 向左旋转，使数组变为  `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` （下标 从 0 开始 计数）。例如，  `[0,1,2,4,5,6,7]`  下标  `3`  上向左旋转后可能变为  `[4,5,6,7,0,1,2]`  。
给你 旋转后 的数组  `nums`  和一个整数  `target`  ，如果  `nums`  中存在这个目标值  `target`  ，则返回它的下标，否则返回  `-1`  。
你必须设计一个时间复杂度为  `O(log n)`  的算法解决此问题。
 
示例 1：

```text
输入：nums = [4,5,6,7,0,1,2], target = 0
输出：4
```

示例 2：

```text
输入：nums = [4,5,6,7,0,1,2], target = 3
输出：-1
```

示例 3：

```text
输入：nums = [1], target = 0
输出：-1
```

 
提示：

 `1 <= nums.length <= 5000` 
 `-104 <= nums[i] <= 104` 
 `nums`  中的每个值都 独一无二
题目数据保证  `nums`  在预先未知的某个下标上进行了旋转
 `-104 <= target <= 104`


---

> 来源：`LeetCode_Collection_Part_6.md:76`

## 153. 寻找旋转排序数组中的最小值 (Medium)

已知一个长度为  `n`  的数组，预先按照升序排列，经由  `1`  到  `n`  次  **旋转**  后，得到输入数组。例如，原数组  `nums = [0,1,2,4,5,6,7]`  在变化后可能得到：

若旋转  `4`  次，则可以得到  `[4,5,6,7,0,1,2]`
若旋转  `7`  次，则可以得到  `[0,1,2,4,5,6,7]`

注意，数组  `[a[0], a[1], a[2], ..., a[n-1]]`   **旋转一次**  的结果为数组  `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]`  。
给你一个元素值  **互不相同**  的数组  `nums`  ，它原来是一个升序排列的数组，并按上述情形进行了多次旋转。请你找出并返回数组中的  **最小元素**  。
你必须设计一个时间复杂度为  `O(log n)`  的算法解决此问题。

 **示例 1：**

```text
输入：nums = [3,4,5,1,2]
输出：1
解释：原数组为 [1,2,3,4,5] ，旋转 3 次得到输入数组。
```

 **示例 2：**

```text
输入：nums = [4,5,6,7,0,1,2]
输出：0
解释：原数组为 [0,1,2,4,5,6,7] ，旋转 4 次得到输入数组。
```

 **示例 3：**

```text
输入：nums = [11,13,15,17]
输出：11
解释：原数组为 [11,13,15,17] ，旋转 4 次得到输入数组。
```


 **提示：**

 `n == nums.length`
 `1 <= n <= 5000`
 `-5000 <= nums[i] <= 5000`
 `nums`  中的所有整数  **互不相同**
 `nums`  原来是一个升序排序的数组，并进行了  `1`  至  `n`  次旋转

---


---

> 来源：`LeetCode_Collection_Part1.md:311`

## 4. 寻找两个正序数组的中位数 (Hard)

给定两个大小分别为  `m`  和  `n`  的正序（从小到大）数组  `nums1`  和  `nums2` 。请你找出并返回这两个正序数组的 中位数 。
算法的时间复杂度应该为  `O(log (m+n))`  。
 
示例 1：

```text
输入：nums1 = [1,3], nums2 = [2]
输出：2.00000
解释：合并数组 = [1,2,3] ，中位数 2
```

示例 2：

```text
输入：nums1 = [1,2], nums2 = [3,4]
输出：2.50000
解释：合并数组 = [1,2,3,4] ，中位数 (2 + 3) / 2 = 2.5
```

 
 
提示：

 `nums1.length == m` 
 `nums2.length == n` 
 `0 <= m <= 1000` 
 `0 <= n <= 1000` 
 `1 <= m + n <= 2000` 
 `-106 <= nums1[i], nums2[i] <= 106`


---

## 栈

> 来源：`LeetCode_Collection_Part1.md:2219`

## 20. 有效的括号 (Easy)

给定一个只包括  `'('` ， `')'` ， `'{'` ， `'}'` ， `'['` ， `']'`  的字符串  `s`  ，判断字符串是否有效。
有效字符串需满足：

左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
每个右括号都有一个对应的相同类型的左括号。

 
示例 1：

输入：s = "()"
输出：true

示例 2：

输入：s = "()[]{}"
输出：true

示例 3：

输入：s = "(]"
输出：false

示例 4：

输入：s = "([])"
输出：true

示例 5：

输入：s = "([)]"
输出：false

 
提示：

 `1 <= s.length <= 104` 
 `s`  仅由括号  `'()[]{}'`  组成


---

> 来源：`LeetCode_Collection_Part_6.md:160`

## 155. 最小栈 (Medium)

设计一个支持  `push`  ， `pop`  ， `top`  操作，并能在常数时间内检索到最小元素的栈。
实现  `MinStack`  类:

 `MinStack()`  初始化堆栈对象。
 `void push(int val)`  将元素val推入堆栈。
 `void pop()`  删除堆栈顶部的元素。
 `int top()`  获取堆栈顶部的元素。
 `int getMin()`  获取堆栈中的最小元素。


 **示例 1:**

```text
输入：
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

输出：
[null,null,null,null,-3,null,0,-2]

解释：
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.getMin();   --> 返回 -2.
```


 **提示：**

 `-231 <= val <= 231 - 1`
 `pop` 、 `top`  和  `getMin`  操作总是在  **非空栈**  上调用
 `push` ,  `pop` ,  `top` , and  `getMin` 最多被调用  `3 * 104`  次

---


---

> 来源：`LeetCode_Collection_Part_14.md:126`

## 394. 字符串解码 (Medium)

给定一个经过编码的字符串，返回它解码后的字符串。
编码规则为:  `k[encoded_string]` ，表示其中方括号内部的  `encoded_string`  正好重复  `k`  次。注意  `k`  保证为正整数。
你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。
此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数  `k`  ，例如不会出现像  `3a`  或  `2[4]`  的输入。
测试用例保证输出的长度不会超过  `105` 。
 
 **示例 1：** 

```text
输入：s = "3[a]2[bc]"
输出："aaabcbc"
```

 **示例 2：** 

```text
输入：s = "3[a2[c]]"
输出："accaccacc"
```

 **示例 3：** 

```text
输入：s = "2[abc]3[cd]ef"
输出："abcabccdcdcdef"
```

 **示例 4：** 

```text
输入：s = "abc3[cd]xyz"
输出："abccdcdcdxyz"
```

 
 **提示：** 

 `1 <= s.length <= 30` 
 `s`  由小写英文字母、数字和方括号  `'[]'`  组成
 `s`  保证是一个  **有效**  的输入。
 `s`  中所有整数的取值范围为  `[1, 300]`

---


---

> 来源：`LeetCode_Collection_Part_25.md:613`

## 739. 每日温度 (Medium)

给定一个整数数组  `temperatures`  ，表示每天的温度，返回一个数组  `answer`  ，其中  `answer[i]`  是指对于第  `i`  天，下一个更高温度出现在几天后。如果气温在这之后都不会升高，请在该位置用  `0`  来代替。
 
 **示例 1:** 

```text
输入: temperatures = [73,74,75,71,69,72,76,73]
输出: [1,1,4,2,1,1,0,0]
```

 **示例 2:** 

```text
输入: temperatures = [30,40,50,60]
输出: [1,1,1,0]
```

 **示例 3:** 

```text
输入: temperatures = [30,60,90]
输出: [1,1,0]
```

 
 **提示：** 

 `1 <= temperatures.length <= 105` 
 `30 <= temperatures[i] <= 100`

---


---

> 来源：`LeetCode_Collection_Part3.md:872`

## 84. 柱状图中最大的矩形 (Hard)

给定 n 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。
求在该柱状图中，能够勾勒出来的矩形的最大面积。
 
示例 1:

```text
输入：heights = [2,1,5,6,2,3]
输出：10
解释：最大的矩形为图中红色区域，面积为 10
```

示例 2：

```text
输入： heights = [2,4]
输出： 4
```

 
提示：

 `1 <= heights.length <=105` 
 `0 <= heights[i] <= 104`

---


---

## 堆

> 来源：`LeetCode_Collection_Part_8.md:139`

## 215. 数组中的第K个最大元素 (Medium)

给定整数数组  `nums`  和整数  `k` ，请返回数组中第  `k`  个最大的元素。
请注意，你需要找的是数组排序后的第  `k`  个最大的元素，而不是第  `k`  个不同的元素。
你必须设计并实现时间复杂度为  `O(n)`  的算法解决此问题。
 
 **示例 1:** 

```text
输入: [3,2,1,5,6,4], k = 2
输出: 5
```

 **示例 2:** 

```text
输入: [3,2,3,1,2,4,5,5,6], k = 4
输出: 4
```

 
 **提示：** 

 `1 <= k <= nums.length <= 105` 
 `-104 <= nums[i] <= 104`

---


---

> 来源：`LeetCode_Collection_Part_12.md:459`

## 347. 前 K 个高频元素 (Medium)

给你一个整数数组  `nums`  和一个整数  `k`  ，请你返回其中出现频率前  `k`  高的元素。你可以按  **任意顺序**  返回答案。
 
 **示例 1：** 

 **输入：** nums = [1,1,1,2,2,3], k = 2
 **输出：** [1,2]

 **示例 2：** 

 **输入：** nums = [1], k = 1
 **输出：** [1]

 **示例 3：** 

 **输入：** nums = [1,2,1,2,1,2,3,1,3,2], k = 2
 **输出：** [1,2]

 
 **提示：** 

 `1 <= nums.length <= 105` 
 `-104 <= nums[i] <= 104` 
 `k`  的取值范围是  `[1, 数组中不相同的元素的个数]` 
题目数据保证答案唯一，换句话说，数组中前  `k`  个高频元素的集合是唯一的

 
 **进阶：** 你所设计算法的时间复杂度  **必须**  优于  `O(n log n)`  ，其中  `n`  是数组大小。

---


---

> 来源：`LeetCode_Collection_Part_10.md:542`

## 295. 数据流的中位数 (Hard)

**中位数** 是有序整数列表中的中间值。如果列表的大小是偶数，则没有中间值，中位数是两个中间值的平均值。

例如  `arr = [2,3,4]`  的中位数是  `3`  。
例如  `arr = [2,3]`  的中位数是  `(2 + 3) / 2 = 2.5`  。

实现 MedianFinder 类:

 `MedianFinder()`  初始化  `MedianFinder`  对象。

 `void addNum(int num)`  将数据流中的整数  `num`  添加到数据结构中。

 `double findMedian()`  返回到目前为止所有元素的中位数。与实际答案相差  `10-5`  以内的答案将被接受。

 **示例 1：**

```text
输入
["MedianFinder", "addNum", "addNum", "findMedian", "addNum", "findMedian"]
[[], [1], [2], [], [3], []]
输出
[null, null, null, 1.5, null, 2.0]

解释
MedianFinder medianFinder = new MedianFinder();
medianFinder.addNum(1);    // arr = [1]
medianFinder.addNum(2);    // arr = [1, 2]
medianFinder.findMedian(); // 返回 1.5 ((1 + 2) / 2)
medianFinder.addNum(3);    // arr[1, 2, 3]
medianFinder.findMedian(); // return 2.0
```

 **提示:**

 `-105 <= num <= 105`
在调用  `findMedian`  之前，数据结构中至少有一个元素
最多  `5 * 104`  次调用  `addNum`  和  `findMedian`

---


---

## 贪心

> 来源：`LeetCode_Collection_Part_5.md:3`

## 121. 买卖股票的最佳时机 (Easy)

给定一个数组  `prices`  ，它的第  `i`  个元素  `prices[i]`  表示一支给定股票第  `i`  天的价格。
你只能选择  **某一天**  买入这只股票，并选择在  **未来的某一个不同的日子**  卖出该股票。设计一个算法来计算你所能获取的最大利润。
返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回  `0`  。
 
 **示例 1：** 

```text
输入：[7,1,5,3,6,4]
输出：5
解释：在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
```

 **示例 2：** 

```text
输入：prices = [7,6,4,3,1]
输出：0
解释：在这种情况下, 没有交易完成, 所以最大利润为 0。
```

 
 **提示：** 

 `1 <= prices.length <= 105` 
 `0 <= prices[i] <= 104`

---


---

> 来源：`LeetCode_Collection_Part2.md:2533`

## 55. 跳跃游戏 (Medium)

给你一个非负整数数组  `nums`  ，你最初位于数组的 第一个下标 。数组中的每个元素代表你在该位置可以跳跃的最大长度。
判断你是否能够到达最后一个下标，如果可以，返回  `true`  ；否则，返回  `false`  。
 
示例 1：

```text
输入：nums = [2,3,1,1,4]
输出：true
解释：可以先跳 1 步，从下标 0 到达下标 1, 然后再从下标 1 跳 3 步到达最后一个下标。
```

示例 2：

```text
输入：nums = [3,2,1,0,4]
输出：false
解释：无论怎样，总会到达下标为 3 的位置。但该下标的最大跳跃长度是 0 ， 所以永远不可能到达最后一个下标。
```

 
提示：

 `1 <= nums.length <= 104` 
 `0 <= nums[i] <= 105`


---

> 来源：`LeetCode_Collection_Part2.md:1520`

## 45. 跳跃游戏 II (Medium)

给定一个长度为  `n`  的 0 索引整数数组  `nums` 。初始位置在下标 0。
每个元素  `nums[i]`  表示从索引  `i`  向后跳转的最大长度。换句话说，如果你在索引  `i`  处，你可以跳转到任意  `(i + j)`  处：

 `0 <= j <= nums[i]`  且
 `i + j < n` 

返回到达  `n - 1`  的最小跳跃次数。测试用例保证可以到达  `n - 1` 。
 
示例 1:

```text
输入: nums = [2,3,1,1,4]
输出: 2
解释: 跳到最后一个位置的最小跳跃数是 2。
     从下标为 0 跳到下标为 1 的位置，跳 1 步，然后跳 3 步到达数组的最后一个位置。
```

示例 2:

```text
输入: nums = [2,3,0,1,4]
输出: 2
```

 
提示:

 `1 <= nums.length <= 104` 
 `0 <= nums[i] <= 1000` 
题目保证可以到达  `n - 1`


---

> 来源：`LeetCode_Collection_Part_26.md:330`

## 763. 划分字母区间 (Medium)

给你一个字符串  `s`  。我们要把这个字符串划分为尽可能多的片段，同一字母最多出现在一个片段中。例如，字符串  `"ababcc"`  能够被分为  `["abab", "cc"]` ，但类似  `["aba", "bcc"]`  或  `["ab", "ab", "cc"]`  的划分是非法的。
注意，划分结果需要满足：将所有划分结果按顺序连接，得到的字符串仍然是  `s`  。
返回一个表示每个字符串片段的长度的列表。
 
 **示例 1：** 

```text
输入：s = "ababcbacadefegdehijhklij"
输出：[9,7,8]
解释：
划分结果为 "ababcbaca"、"defegde"、"hijhklij" 。
每个字母最多出现在一个片段中。
像 "ababcbacadefegde", "hijhklij" 这样的划分是错误的，因为划分的片段数较少。
```

 **示例 2：** 

```text
输入：s = "eccbbbbdec"
输出：[10]
```

 
 **提示：** 

 `1 <= s.length <= 500` 
 `s`  仅由小写英文字母组成

---


---

## 动态规划

> 来源：`LeetCode_Collection_Part3.md:347`

## 70. 爬楼梯 (Easy)

假设你正在爬楼梯。需要  `n`  阶你才能到达楼顶。
每次你可以爬  `1`  或  `2`  个台阶。你有多少种不同的方法可以爬到楼顶呢？
 
示例 1：

```text
输入：n = 2
输出：2
解释：有两种方法可以爬到楼顶。
1. 1 阶 + 1 阶
2. 2 阶
```

示例 2：

```text
输入：n = 3
输出：3
解释：有三种方法可以爬到楼顶。
1. 1 阶 + 1 阶 + 1 阶
2. 1 阶 + 2 阶
3. 2 阶 + 1 阶
```

 
提示：

 `1 <= n <= 45`

---


---

> 来源：`LeetCode_Collection_Part_4.md:584`

## 118. 杨辉三角 (Easy)

给定一个非负整数  `numRows` ，生成「杨辉三角」的前  `numRows`  行。
在 **「杨辉三角」** 中，每个数是它左上方和右上方的数的和。

 
 **示例 1:** 

```text
输入: numRows = 5
输出: [[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]]
```

 **示例 2:** 

```text
输入: numRows = 1
输出: [[1]]
```

 
 **提示:** 

 `1 <= numRows <= 30`

---


---

> 来源：`LeetCode_Collection_Part_7.md:732`

## 198. 打家劫舍 (Medium)

你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统， **如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警** 。
给定一个代表每个房屋存放金额的非负整数数组，计算你 **不触动警报装置的情况下** ，一夜之内能够偷窃到的最高金额。

 **示例 1：**

```text
输入：[1,2,3,1]
输出：4
解释：偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
     偷窃到的最高金额 = 1 + 3 = 4 。
```

 **示例 2：**

```text
输入：[2,7,9,3,1]
输出：12
解释：偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋 (金额 = 9)，接着偷窃 5 号房屋 (金额 = 1)。
     偷窃到的最高金额 = 2 + 9 + 1 = 12 。
```


 **提示：**

 `1 <= nums.length <= 100`
 `0 <= nums[i] <= 400`

---


---

> 来源：`LeetCode_Collection_Part_10.md:162`

## 279. 完全平方数 (Medium)

给你一个整数  `n`  ，返回 和为  `n`  的完全平方数的最少数量 。
 **完全平方数**  是一个整数，其值等于另一个整数的平方；换句话说，其值等于一个整数自乘的积。例如， `1` 、 `4` 、 `9`  和  `16`  都是完全平方数，而  `3`  和  `11`  不是。

 **示例 1：**

```text
输入：n = 12
输出：3
解释：12 = 4 + 4 + 4
```

 **示例 2：**

```text
输入：n = 13
输出：2
解释：13 = 4 + 9
```



 **提示：**

 `1 <= n <= 104`

---


---

> 来源：`LeetCode_Collection_Part_11.md:554`

## 322. 零钱兑换 (Medium)

给你一个整数数组  `coins`  ，表示不同面额的硬币；以及一个整数  `amount`  ，表示总金额。
计算并返回可以凑成总金额所需的  **最少的硬币个数**  。如果没有任何一种硬币组合能组成总金额，返回  `-1`  。
你可以认为每种硬币的数量是无限的。
 
 **示例 1：** 

```text
输入：coins = [1, 2, 5], amount = 11
输出：3 
解释：11 = 5 + 5 + 1
```

 **示例 2：** 

```text
输入：coins = [2], amount = 3
输出：-1
```

 **示例 3：** 

```text
输入：coins = [1], amount = 0
输出：0
```

 
 **提示：** 

 `1 <= coins.length <= 12` 
 `1 <= coins[i] <= 231 - 1` 
 `0 <= amount <= 104`

---


---

> 来源：`LeetCode_Collection_Part_5.md:688`

## 139. 单词拆分 (Medium)

给你一个字符串  `s`  和一个字符串列表  `wordDict`  作为字典。如果可以利用字典中出现的一个或多个单词拼接出  `s`  则返回  `true` 。
 **注意：** 不要求字典中出现的单词全部都使用，并且字典中的单词可以重复使用。
 
 **示例 1：** 

```text
输入: s = "leetcode", wordDict = ["leet", "code"]
输出: true
解释: 返回 true 因为 "leetcode" 可以由 "leet" 和 "code" 拼接成。
```

 **示例 2：** 

```text
输入: s = "applepenapple", wordDict = ["apple", "pen"]
输出: true
解释: 返回 true 因为 "applepenapple" 可以由 "apple" "pen" "apple" 拼接成。
     注意，你可以重复使用字典中的单词。
```

 **示例 3：** 

```text
输入: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
输出: false
```

 
 **提示：** 

 `1 <= s.length <= 300` 
 `1 <= wordDict.length <= 1000` 
 `1 <= wordDict[i].length <= 20` 
 `s`  和  `wordDict[i]`  仅由小写英文字母组成
 `wordDict`  中的所有字符串  **互不相同**

---


---

> 来源：`LeetCode_Collection_Part_10.md:685`

## 300. 最长递增子序列 (Medium)

给你一个整数数组  `nums`  ，找到其中最长严格递增子序列的长度。
 **子序列** 是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。例如， `[3,6,2,7]`  是数组  `[0,3,1,6,2,2,7]`  的子序列。


 **示例 1：**

```text
输入：nums = [10,9,2,5,3,7,101,18]
输出：4
解释：最长递增子序列是 [2,3,7,101]，因此长度为 4 。
```

 **示例 2：**

```text
输入：nums = [0,1,0,3,2,3]
输出：4
```

 **示例 3：**

```text
输入：nums = [7,7,7,7,7,7,7]
输出：1
```


 **提示：**

 `1 <= nums.length <= 2500`
 `-104 <= nums[i] <= 104`


 **进阶：**

你能将算法的时间复杂度降低到  `O(n log(n))`  吗?

---

# Java 解法补充附录（271-300）

### 271. 字符串的编码与解码

**基础解法：** 用特殊分隔符拼接字符串，但当原字符串包含分隔符时会歧义。

**资深解法：** 长度前缀编码：`长度#内容`，解码时先读长度再截取固定字符数。

```java
public class Codec {
    public String encode(List<String> strs) {
        StringBuilder sb = new StringBuilder();
        for (String s : strs) sb.append(s.length()).append('#').append(s);
        return sb.toString();
    }

    public List<String> decode(String s) {
        List<String> ans = new ArrayList<>();
        int i = 0;
        while (i < s.length()) {
            int j = i;
            while (s.charAt(j) != '#') j++;
            int len = Integer.parseInt(s.substring(i, j));
            ans.add(s.substring(j + 1, j + 1 + len));
            i = j + 1 + len;
        }
        return ans;
    }
}
```

**基础语法与思想：** 自描述格式比转义分隔符更稳；`substring` 按长度截取可保留任意字符。

### 272. 最接近的二叉搜索树值 II

**基础解法：** 中序遍历得到有序数组，按与 `target` 的距离排序取前 `k` 个。

**资深解法：** 维护大小为 `k` 的队列，中序有序遍历中若新值更接近就弹出最远旧值。

```java
class Solution {
    public List<Integer> closestKValues(TreeNode root, double target, int k) {
        LinkedList<Integer> ans = new LinkedList<>();
        inorder(root, target, k, ans);
        return ans;
    }

    private void inorder(TreeNode node, double target, int k, LinkedList<Integer> ans) {
        if (node == null) return;
        inorder(node.left, target, k, ans);
        if (ans.size() < k) ans.add(node.val);
        else if (Math.abs(ans.peekFirst() - target) > Math.abs(node.val - target)) {
            ans.pollFirst();
            ans.add(node.val);
        }
        inorder(node.right, target, k, ans);
    }
}
```

**基础语法与思想：** BST 中序递增；固定大小队列保存当前最接近的 `k` 个值。

### 273. 整数转换英文表示

**基础解法：** 按十亿、百万、千分块，每个三位数转英文。

**资深解法：** 递归处理 `<20`、`<100`、`<1000` 和大单位。

```java
class Solution {
    private final String[] below20 = {"", "One", "Two", "Three", "Four", "Five", "Six", "Seven", "Eight", "Nine", "Ten", "Eleven", "Twelve", "Thirteen", "Fourteen", "Fifteen", "Sixteen", "Seventeen", "Eighteen", "Nineteen"};
    private final String[] tens = {"", "", "Twenty", "Thirty", "Forty", "Fifty", "Sixty", "Seventy", "Eighty", "Ninety"};

    public String numberToWords(int num) {
        if (num == 0) return "Zero";
        return helper(num).trim();
    }

    private String helper(int num) {
        if (num == 0) return "";
        if (num < 20) return below20[num] + " ";
        if (num < 100) return tens[num / 10] + " " + helper(num % 10);
        if (num < 1000) return below20[num / 100] + " Hundred " + helper(num % 100);
        if (num < 1_000_000) return helper(num / 1000) + "Thousand " + helper(num % 1000);
        if (num < 1_000_000_000) return helper(num / 1_000_000) + "Million " + helper(num % 1_000_000);
        return helper(num / 1_000_000_000) + "Billion " + helper(num % 1_000_000_000);
    }
}
```

**基础语法与思想：** 英文数字按三位分组；递归返回尾部带空格，最终 `trim()` 收尾。

### 274. H 指数

**基础解法：** 排序后找到最大的 `h`，使至少 `h` 篇论文引用数不小于 `h`。

**资深解法：** 计数桶统计引用数，超过 `n` 的都放入 `n` 桶，从高到低累计。

```java
class Solution {
    public int hIndex(int[] citations) {
        int n = citations.length;
        int[] count = new int[n + 1];
        for (int c : citations) count[Math.min(c, n)]++;
        int papers = 0;
        for (int h = n; h >= 0; h--) {
            papers += count[h];
            if (papers >= h) return h;
        }
        return 0;
    }
}
```

**基础语法与思想：** H 指数只关心引用数是否达到 `h`，超过论文总数可合并统计。

### 275. H 指数 II

**基础解法：** 直接按 274 扫描有序数组，时间 `O(n)`。

**资深解法：** 引用数组升序，二分找第一个 `citations[i] >= n - i` 的位置。

```java
class Solution {
    public int hIndex(int[] citations) {
        int n = citations.length, l = 0, r = n;
        while (l < r) {
            int m = l + (r - l) / 2;
            if (citations[m] >= n - m) r = m;
            else l = m + 1;
        }
        return n - l;
    }
}
```

**基础语法与思想：** 位置 `i` 右侧有 `n-i` 篇论文；二分单调条件。

### 276. 栅栏涂色

**基础解法：** DP 记录最后两个柱子同色或不同色的方案数。

**资深解法：** 滚动变量：`same` 表示当前最后两根同色，`diff` 表示不同色。

```java
class Solution {
    public int numWays(int n, int k) {
        if (n == 0) return 0;
        if (n == 1) return k;
        int same = k, diff = k * (k - 1);
        for (int i = 3; i <= n; i++) {
            int nsame = diff;
            int ndiff = (same + diff) * (k - 1);
            same = nsame;
            diff = ndiff;
        }
        return same + diff;
    }
}
```

**基础语法与思想：** 当前同色只能来自上一状态不同色；当前不同色可从任意上一状态换颜色。

### 277. 搜寻名人

**基础解法：** 对每个人检查入度为 `n-1`、出度为 0，需要 `O(n^2)` 次查询。

**资深解法：** 先用一轮淘汰找候选人，再验证候选人。

```java
public class Solution extends Relation {
    public int findCelebrity(int n) {
        int cand = 0;
        for (int i = 1; i < n; i++) {
            if (knows(cand, i)) cand = i;
        }
        for (int i = 0; i < n; i++) {
            if (i == cand) continue;
            if (knows(cand, i) || !knows(i, cand)) return -1;
        }
        return cand;
    }
}
```

**基础语法与思想：** 若 `cand` 认识 `i`，`cand` 不可能是名人；一轮后只剩一个可能候选。

### 278. 第一个错误的版本

**基础解法：** 从版本 1 开始逐个调用 `isBadVersion`。

**资深解法：** 二分查找第一个满足 `isBadVersion(version)` 的版本。

```java
public class Solution extends VersionControl {
    public int firstBadVersion(int n) {
        int l = 1, r = n;
        while (l < r) {
            int m = l + (r - l) / 2;
            if (isBadVersion(m)) r = m;
            else l = m + 1;
        }
        return l;
    }
}
```

**基础语法与思想：** “第一个 true” 是二分边界题；中点计算要防溢出。

### 279. 完全平方数

**基础解法：** `dp[i]` 表示组成 `i` 的最少完全平方数个数，枚举所有平方数转移。

**资深解法：** 可用数学四平方定理优化；DP 更通用。

```java
class Solution {
    public int numSquares(int n) {
        int[] dp = new int[n + 1];
        Arrays.fill(dp, Integer.MAX_VALUE / 2);
        dp[0] = 0;
        for (int i = 1; i * i <= n; i++) {
            int sq = i * i;
            for (int x = sq; x <= n; x++) {
                dp[x] = Math.min(dp[x], dp[x - sq] + 1);
            }
        }
        return dp[n];
    }
}
```

**基础语法与思想：** 完全背包：每个平方数可以重复使用。

### 280. 摆动排序

**基础解法：** 排序后交错放置小值和大值。

**资深解法：** 一次扫描，偶数位应 `<=` 后一位，奇数位应 `>=` 后一位，不满足就交换。

```java
class Solution {
    public void wiggleSort(int[] nums) {
        for (int i = 0; i + 1 < nums.length; i++) {
            if ((i % 2 == 0 && nums[i] > nums[i + 1]) ||
                (i % 2 == 1 && nums[i] < nums[i + 1])) {
                int t = nums[i];
                nums[i] = nums[i + 1];
                nums[i + 1] = t;
            }
        }
    }
}
```

**基础语法与思想：** 局部交换不会破坏前一个位置已满足的摆动关系。

### 281. 锯齿迭代器

**基础解法：** 构造时把两个向量交替展开到队列。

**资深解法：** 队列保存非空迭代器，每取一个元素后若仍有剩余则放回队尾。

```java
public class ZigzagIterator {
    private Queue<Iterator<Integer>> queue = new ArrayDeque<>();

    public ZigzagIterator(List<Integer> v1, List<Integer> v2) {
        if (!v1.isEmpty()) queue.offer(v1.iterator());
        if (!v2.isEmpty()) queue.offer(v2.iterator());
    }

    public int next() {
        Iterator<Integer> it = queue.poll();
        int val = it.next();
        if (it.hasNext()) queue.offer(it);
        return val;
    }

    public boolean hasNext() {
        return !queue.isEmpty();
    }
}
```

**基础语法与思想：** 队列轮转实现多路交替；扩展到多个向量也自然成立。

### 282. 给表达式添加运算符

**基础解法：** 回溯枚举每个数字切分和每个运算符，最终计算表达式值。

**资深解法：** 回溯时携带当前值和上一项 `last`，乘法通过回滚上一项处理优先级。

```java
class Solution {
    public List<String> addOperators(String num, int target) {
        List<String> ans = new ArrayList<>();
        dfs(num, target, 0, 0, 0, new StringBuilder(), ans);
        return ans;
    }

    private void dfs(String num, int target, int pos, long value, long last, StringBuilder expr, List<String> ans) {
        if (pos == num.length()) {
            if (value == target) ans.add(expr.toString());
            return;
        }
        int len = expr.length();
        for (int i = pos; i < num.length(); i++) {
            if (i > pos && num.charAt(pos) == '0') break;
            long cur = Long.parseLong(num.substring(pos, i + 1));
            if (pos == 0) {
                expr.append(cur);
                dfs(num, target, i + 1, cur, cur, expr, ans);
                expr.setLength(len);
            } else {
                expr.append('+').append(cur);
                dfs(num, target, i + 1, value + cur, cur, expr, ans);
                expr.setLength(len);
                expr.append('-').append(cur);
                dfs(num, target, i + 1, value - cur, -cur, expr, ans);
                expr.setLength(len);
                expr.append('*').append(cur);
                dfs(num, target, i + 1, value - last + last * cur, last * cur, expr, ans);
                expr.setLength(len);
            }
        }
    }
}
```

**基础语法与思想：** `last` 保存最近一个乘法项；前导零数字只能是单个 `0`。

### 283. 移动零

**基础解法：** 额外数组先放非零元素，再补零。

**资深解法：** 双指针原地写入非零元素，最后补零。

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int write = 0;
        for (int x : nums) if (x != 0) nums[write++] = x;
        while (write < nums.length) nums[write++] = 0;
    }
}
```

**基础语法与思想：** 稳定保留非零元素相对顺序；写指针表示下一个非零落点。

### 284. 窥视迭代器

**基础解法：** 每次 `peek` 都复制迭代器不可行。

**资深解法：** 缓存下一个元素，`peek` 返回缓存，`next` 消费缓存并预取。

```java
class PeekingIterator implements Iterator<Integer> {
    private Iterator<Integer> iterator;
    private Integer next;

    public PeekingIterator(Iterator<Integer> iterator) {
        this.iterator = iterator;
        if (iterator.hasNext()) next = iterator.next();
    }

    public Integer peek() {
        return next;
    }

    @Override
    public Integer next() {
        Integer ans = next;
        next = iterator.hasNext() ? iterator.next() : null;
        return ans;
    }

    @Override
    public boolean hasNext() {
        return next != null;
    }
}
```

**基础语法与思想：** 预读一个元素让 `peek` 不推进底层迭代器。

### 285. 二叉搜索树中的中序后继

**基础解法：** 中序遍历列表，找到 `p` 后返回下一个节点。

**资深解法：** 利用 BST：若当前值大于 `p`，当前可能是后继并向左找更小候选；否则向右。

```java
class Solution {
    public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
        TreeNode ans = null;
        while (root != null) {
            if (root.val > p.val) {
                ans = root;
                root = root.left;
            } else root = root.right;
        }
        return ans;
    }
}
```

**基础语法与思想：** 后继是“大于 p 的最小节点”，正好是 BST 搜索过程中的候选。

### 286. 墙与门

**基础解法：** 从每个空房间 BFS 找最近门，重复遍历严重。

**资深解法：** 多源 BFS：所有门同时入队，向外扩散距离。

```java
class Solution {
    public void wallsAndGates(int[][] rooms) {
        int m = rooms.length, n = rooms[0].length;
        Queue<int[]> q = new ArrayDeque<>();
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) {
            if (rooms[i][j] == 0) q.offer(new int[]{i, j});
        }
        int[][] dirs = {{1,0},{-1,0},{0,1},{0,-1}};
        while (!q.isEmpty()) {
            int[] cur = q.poll();
            for (int[] d : dirs) {
                int x = cur[0] + d[0], y = cur[1] + d[1];
                if (x < 0 || x == m || y < 0 || y == n || rooms[x][y] != Integer.MAX_VALUE) continue;
                rooms[x][y] = rooms[cur[0]][cur[1]] + 1;
                q.offer(new int[]{x, y});
            }
        }
    }
}
```

**基础语法与思想：** 多源 BFS 第一次到达某房间就是最近门距离。

### 287. 寻找重复数

**基础解法：** 排序后找相邻相等值，或哈希集合查重。

**资深解法：** 把数组值看作链表指针，重复数是环入口，用 Floyd 判圈。

```java
class Solution {
    public int findDuplicate(int[] nums) {
        int slow = nums[0], fast = nums[nums[0]];
        while (slow != fast) {
            slow = nums[slow];
            fast = nums[nums[fast]];
        }
        slow = 0;
        while (slow != fast) {
            slow = nums[slow];
            fast = nums[fast];
        }
        return slow;
    }
}
```

**基础语法与思想：** 数值范围 `1..n` 可作为下标跳转；重复值导致多个入口指向同一节点形成环。

### 288. 单词的唯一缩写

**基础解法：** 每次查询时扫描词典，比较同缩写单词是否只有自身。

**资深解法：** 构造时把缩写映射到唯一单词；若冲突则标记为空。

```java
class ValidWordAbbr {
    private Map<String, String> map = new HashMap<>();

    public ValidWordAbbr(String[] dictionary) {
        for (String w : new HashSet<>(Arrays.asList(dictionary))) {
            String a = abbr(w);
            map.put(a, map.containsKey(a) ? "" : w);
        }
    }

    public boolean isUnique(String word) {
        String a = abbr(word);
        return !map.containsKey(a) || map.get(a).equals(word);
    }

    private String abbr(String w) {
        int n = w.length();
        return n <= 2 ? w : "" + w.charAt(0) + (n - 2) + w.charAt(n - 1);
    }
}
```

**基础语法与思想：** 词典去重后再建表；同缩写多个不同单词时查询只能失败。

### 289. 生命游戏

**基础解法：** 复制一份原矩阵，根据原状态计算下一代。

**资深解法：** 原地用额外状态编码：`2` 表示活变死，`3` 表示死变活。

```java
class Solution {
    public void gameOfLife(int[][] board) {
        int m = board.length, n = board[0].length;
        int[] dirs = {-1, 0, 1};
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                int live = 0;
                for (int dx : dirs) for (int dy : dirs) {
                    if (dx == 0 && dy == 0) continue;
                    int x = i + dx, y = j + dy;
                    if (x >= 0 && x < m && y >= 0 && y < n && (board[x][y] == 1 || board[x][y] == 2)) live++;
                }
                if (board[i][j] == 1 && (live < 2 || live > 3)) board[i][j] = 2;
                if (board[i][j] == 0 && live == 3) board[i][j] = 3;
            }
        }
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) board[i][j] %= 2;
    }
}
```

**基础语法与思想：** 过渡状态同时保留旧状态和新状态；最后取模落盘。

### 290. 单词规律

**基础解法：** 双向哈希表保证模式字符和单词一一对应。

**资深解法：** 记录上次出现位置，两个序列同构时当前位置历史必须一致。

```java
class Solution {
    public boolean wordPattern(String pattern, String s) {
        String[] words = s.split(" ");
        if (pattern.length() != words.length) return false;
        Map<Character, Integer> a = new HashMap<>();
        Map<String, Integer> b = new HashMap<>();
        for (int i = 0; i < words.length; i++) {
            int p = a.getOrDefault(pattern.charAt(i), -1);
            int q = b.getOrDefault(words[i], -1);
            if (p != q) return false;
            a.put(pattern.charAt(i), i);
            b.put(words[i], i);
        }
        return true;
    }
}
```

**基础语法与思想：** 位置签名一致即可表达双射关系。

### 291. 单词规律 II

**基础解法：** 回溯枚举每个模式字符映射到目标串的所有非空前缀。

**资深解法：** 用映射表和已占用字符串集合同时维护双射，并剪枝剩余长度。

```java
class Solution {
    public boolean wordPatternMatch(String pattern, String s) {
        return dfs(pattern, 0, s, 0, new HashMap<>(), new HashSet<>());
    }

    private boolean dfs(String p, int i, String s, int j, Map<Character, String> map, Set<String> used) {
        if (i == p.length() && j == s.length()) return true;
        if (i == p.length() || j == s.length()) return false;
        char c = p.charAt(i);
        if (map.containsKey(c)) {
            String w = map.get(c);
            return s.startsWith(w, j) && dfs(p, i + 1, s, j + w.length(), map, used);
        }
        for (int end = j + 1; end <= s.length(); end++) {
            String w = s.substring(j, end);
            if (used.contains(w)) continue;
            map.put(c, w); used.add(w);
            if (dfs(p, i + 1, s, end, map, used)) return true;
            map.remove(c); used.remove(w);
        }
        return false;
    }
}
```

**基础语法与思想：** `startsWith(prefix, offset)` 可从指定位置匹配；回溯负责尝试映射长度。

### 292. Nim 游戏

**基础解法：** DP 判断每个石子数是否为必胜态。

**资深解法：** 4 的倍数是必败态，其余都是必胜态。

```java
class Solution {
    public boolean canWinNim(int n) {
        return n % 4 != 0;
    }
}
```

**基础语法与思想：** 每次可拿 1-3 个，先手只要把对手留在 4 的倍数即可。

### 293. 翻转游戏

**基础解法：** 扫描所有相邻位置，遇到 `++` 就构造一个新字符串。

```java
class Solution {
    public List<String> generatePossibleNextMoves(String currentState) {
        List<String> ans = new ArrayList<>();
        char[] arr = currentState.toCharArray();
        for (int i = 0; i + 1 < arr.length; i++) {
            if (arr[i] == '+' && arr[i + 1] == '+') {
                arr[i] = arr[i + 1] = '-';
                ans.add(new String(arr));
                arr[i] = arr[i + 1] = '+';
            }
        }
        return ans;
    }
}
```

**资深解法：** 字符数组原地改再恢复，比多次 `substring` 拼接更直接。
**基础语法与思想：** 每个可行动作只影响两个相邻字符。

### 294. 翻转游戏 II

**基础解法：** 递归枚举所有走法，若存在一步让对手失败，则当前必胜。

**资深解法：** 记忆化当前字符串状态，避免重复博弈搜索。

```java
class Solution {
    private Map<String, Boolean> memo = new HashMap<>();

    public boolean canWin(String currentState) {
        if (memo.containsKey(currentState)) return memo.get(currentState);
        char[] arr = currentState.toCharArray();
        for (int i = 0; i + 1 < arr.length; i++) {
            if (arr[i] == '+' && arr[i + 1] == '+') {
                arr[i] = arr[i + 1] = '-';
                boolean win = !canWin(new String(arr));
                arr[i] = arr[i + 1] = '+';
                if (win) {
                    memo.put(currentState, true);
                    return true;
                }
            }
        }
        memo.put(currentState, false);
        return false;
    }
}
```

**基础语法与思想：** 博弈题常用“当前存在一步让对手输”定义必胜态。

### 295. 数据流的中位数

**基础解法：** 每次插入后排序或插入有序列表，查询中位数。

**资深解法：** 双堆：大根堆保存较小一半，小根堆保存较大一半。

```java
class MedianFinder {
    private PriorityQueue<Integer> small = new PriorityQueue<>(Collections.reverseOrder());
    private PriorityQueue<Integer> large = new PriorityQueue<>();

    public void addNum(int num) {
        small.offer(num);
        large.offer(small.poll());
        if (large.size() > small.size()) small.offer(large.poll());
    }

    public double findMedian() {
        if (small.size() > large.size()) return small.peek();
        return ((double) small.peek() + large.peek()) / 2.0;
    }
}
```

**基础语法与思想：** 保持 `small.size() >= large.size()` 且两堆有序分割。

### 296. 最佳的碰头地点

**基础解法：** 枚举每个网格点作为碰头点，计算所有人的曼哈顿距离总和。

**资深解法：** 曼哈顿距离行列独立，最优点是所有人行坐标和列坐标的中位数。

```java
class Solution {
    public int minTotalDistance(int[][] grid) {
        List<Integer> rows = new ArrayList<>(), cols = new ArrayList<>();
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) if (grid[i][j] == 1) rows.add(i);
        }
        for (int j = 0; j < grid[0].length; j++) {
            for (int i = 0; i < grid.length; i++) if (grid[i][j] == 1) cols.add(j);
        }
        return dist(rows) + dist(cols);
    }

    private int dist(List<Integer> list) {
        int l = 0, r = list.size() - 1, ans = 0;
        while (l < r) ans += list.get(r--) - list.get(l++);
        return ans;
    }
}
```

**基础语法与思想：** 按行、按列扫描天然得到有序坐标；中位数最小化绝对距离和。

### 297. 二叉树的序列化与反序列化

**基础解法：** 层序遍历带 `null` 标记序列化。

**资深解法：** 前序 DFS 加 `#` 空节点标记，递归反序列化。

```java
public class Codec {
    public String serialize(TreeNode root) {
        StringBuilder sb = new StringBuilder();
        ser(root, sb);
        return sb.toString();
    }

    private void ser(TreeNode node, StringBuilder sb) {
        if (node == null) {
            sb.append("#,");
            return;
        }
        sb.append(node.val).append(',');
        ser(node.left, sb);
        ser(node.right, sb);
    }

    public TreeNode deserialize(String data) {
        Queue<String> q = new ArrayDeque<>(Arrays.asList(data.split(",")));
        return de(q);
    }

    private TreeNode de(Queue<String> q) {
        String s = q.poll();
        if (s.equals("#")) return null;
        TreeNode node = new TreeNode(Integer.parseInt(s));
        node.left = de(q);
        node.right = de(q);
        return node;
    }
}
```

**基础语法与思想：** 空节点标记让树结构可唯一还原；前序反序列化天然递归消费队列。

### 298. 二叉树最长连续序列

**基础解法：** 从每个节点出发向下 DFS 找连续路径，重复遍历。

**资深解法：** DFS 携带父节点值和当前连续长度。

```java
class Solution {
    private int ans = 0;

    public int longestConsecutive(TreeNode root) {
        dfs(root, 0, 0);
        return ans;
    }

    private void dfs(TreeNode node, int parent, int len) {
        if (node == null) return;
        int cur = node.val == parent + 1 ? len + 1 : 1;
        ans = Math.max(ans, cur);
        dfs(node.left, node.val, cur);
        dfs(node.right, node.val, cur);
    }
}
```

**基础语法与思想：** 路径方向必须从父到子；状态随 DFS 向下传递。

### 299. 猜数字游戏

**基础解法：** 先统计公牛，再对非公牛数字做频次匹配得到奶牛。

**资深解法：** 用一个计数数组在一次扫描中处理非公牛字符的互补关系。

```java
class Solution {
    public String getHint(String secret, String guess) {
        int bulls = 0, cows = 0;
        int[] count = new int[10];
        for (int i = 0; i < secret.length(); i++) {
            int a = secret.charAt(i) - '0', b = guess.charAt(i) - '0';
            if (a == b) bulls++;
            else {
                if (count[a] < 0) cows++;
                if (count[b] > 0) cows++;
                count[a]++;
                count[b]--;
            }
        }
        return bulls + "A" + cows + "B";
    }
}
```

**基础语法与思想：** 正数表示 secret 多出的数字，负数表示 guess 多出的数字。

### 300. 最长递增子序列

**基础解法：** `dp[i]` 表示以 `nums[i]` 结尾的 LIS 长度，枚举前驱，时间 `O(n^2)`。

**资深解法：** 维护 `tails[len]` 为长度 `len+1` 递增子序列的最小结尾，二分更新。

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int[] tails = new int[nums.length];
        int size = 0;
        for (int x : nums) {
            int l = 0, r = size;
            while (l < r) {
                int m = l + (r - l) / 2;
                if (tails[m] < x) l = m + 1;
                else r = m;
            }
            tails[l] = x;
            if (l == size) size++;
        }
        return size;
    }
}
```

**基础语法与思想：** `tails` 不一定是真实序列，但它的长度代表当前可达到的 LIS 长度；二分找第一个大于等于 `x` 的位置。


---

> 来源：`LeetCode_Collection_Part_6.md:45`

## 152. 乘积最大子数组 (Medium)

给你一个整数数组  `nums`  ，请你找出数组中乘积最大的非空连续 子数组（该子数组中至少包含一个数字），并返回该子数组所对应的乘积。
测试用例的答案是一个  **32-位**  整数。
 **请注意** ，一个只包含一个元素的数组的乘积是这个元素的值。

 **示例 1:**

```text
输入: nums = [2,3,-2,4]
输出: 6
解释: 子数组 [2,3] 有最大乘积 6。
```

 **示例 2:**

```text
输入: nums = [-2,0,-1]
输出: 0
解释: 结果不能为 2, 因为 [-2,-1] 不是子数组。
```


 **提示:**

 `1 <= nums.length <= 2 * 104`
 `-10 <= nums[i] <= 10`
 `nums`  的任何子数组的乘积都  **保证**  是一个  **32-位**  整数

---


---

> 来源：`LeetCode_Collection_Part_14.md:848`

## 416. 分割等和子集 (Medium)

给你一个  **只包含正整数** 的  **非空** 数组  `nums`  。请你判断是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。
 
 **示例 1：** 

```text
输入：nums = [1,5,11,5]
输出：true
解释：数组可以分割成 [1, 5, 5] 和 [11] 。
```

 **示例 2：** 

```text
输入：nums = [1,2,3,5]
输出：false
解释：数组不能分割成两个元素和相等的子集。
```

 
 **提示：** 

 `1 <= nums.length <= 200` 
 `1 <= nums[i] <= 100`

---


---

> 来源：`LeetCode_Collection_Part2.md:138`

## 32. 最长有效括号 (Hard)

给你一个只包含  `'('`  和  `')'`  的字符串，找出最长有效（格式正确且连续）括号 子串 的长度。
左右括号匹配，即每个左括号都有对应的右括号将其闭合的字符串是格式正确的，比如  `"(()())"` 。
 

示例 1：

```text
输入：s = "(()"
输出：2
解释：最长有效括号子串是 "()"
```

示例 2：

```text
输入：s = ")()())"
输出：4
解释：最长有效括号子串是 "()()"
```

示例 3：

```text
输入：s = ""
输出：0
```

 
提示：

 `0 <= s.length <= 3 * 104` 
 `s[i]`  为  `'('`  或  `')'`


---

## 多维动态规划

> 来源：`LeetCode_Collection_Part3.md:28`

## 62. 不同路径 (Medium)

一个机器人位于一个  `m x n`  网格的左上角 （起始点在下图中标记为 “Start” ）。
机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish” ）。
问总共有多少条不同的路径？
 
示例 1：

```text
输入：m = 3, n = 7
输出：28
```

示例 2：

```text
输入：m = 3, n = 2
输出：3
解释：
从左上角开始，总共有 3 条路径可以到达右下角。
1. 向右 -> 向下 -> 向下
2. 向下 -> 向下 -> 向右
3. 向下 -> 向右 -> 向下
```

示例 3：

```text
输入：m = 7, n = 3
输出：28
```

示例 4：

```text
输入：m = 3, n = 3
输出：6
```

 
提示：

 `1 <= m, n <= 100` 
题目数据保证答案小于等于  `2 * 109`

---


---

> 来源：`LeetCode_Collection_Part3.md:110`

## 64. 最小路径和 (Medium)

给定一个包含非负整数的  `m x n`  网格  `grid`  ，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。
说明：每次只能向下或者向右移动一步。
 
示例 1：

```text
输入：grid = [[1,3,1],[1,5,1],[4,2,1]]
输出：7
解释：因为路径 1→3→1→1→1 的总和最小。
```

示例 2：

```text
输入：grid = [[1,2,3],[4,5,6]]
输出：12
```

 
提示：

 `m == grid.length` 
 `n == grid[i].length` 
 `1 <= m, n <= 200` 
 `0 <= grid[i][j] <= 200`

---


---

> 来源：`LeetCode_Collection_Part1.md:434`

## 5. 最长回文子串 (Medium)

给你一个字符串  `s` ，找到  `s`  中最长的 回文 子串。
 
示例 1：

```text
输入：s = "babad"
输出："bab"
解释："aba" 同样是符合题意的答案。
```

示例 2：

```text
输入：s = "cbbd"
输出："bb"
```

 
提示：

 `1 <= s.length <= 1000` 
 `s`  仅由数字和英文字母组成


---

> 来源：`LeetCode_Collection_Part_39.md:66`

## 1143. 最长公共子序列 (Medium)

给定两个字符串  `text1`  和  `text2` ，返回这两个字符串的最长  **公共子序列**  的长度。如果不存在  **公共子序列**  ，返回  `0`  。
一个字符串的  **子序列**  是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。

例如， `"ace"`  是  `"abcde"`  的子序列，但  `"aec"`  不是  `"abcde"`  的子序列。

两个字符串的  **公共子序列**  是这两个字符串所共同拥有的子序列。
 
 **示例 1：** 

```text
输入：text1 = "abcde", text2 = "ace" 
输出：3  
解释：最长公共子序列是 "ace" ，它的长度为 3 。
```

 **示例 2：** 

```text
输入：text1 = "abc", text2 = "abc"
输出：3
解释：最长公共子序列是 "abc" ，它的长度为 3 。
```

 **示例 3：** 

```text
输入：text1 = "abc", text2 = "def"
输出：0
解释：两个字符串没有公共子序列，返回 0 。
```

 
 **提示：** 

 `1 <= text1.length, text2.length <= 1000` 
 `text1`  和  `text2`  仅由小写英文字符组成。

---


---

> 来源：`LeetCode_Collection_Part3.md:443`

## 72. 编辑距离 (Medium)

给你两个单词  `word1`  和  `word2` ， 请返回将  `word1`  转换成  `word2`  所使用的最少操作数  。
你可以对一个单词进行如下三种操作：

插入一个字符
删除一个字符
替换一个字符

 
示例 1：

```text
输入：word1 = "horse", word2 = "ros"
输出：3
解释：
horse -> rorse (将 'h' 替换为 'r')
rorse -> rose (删除 'r')
rose -> ros (删除 'e')
```

示例 2：

```text
输入：word1 = "intention", word2 = "execution"
输出：5
解释：
intention -> inention (删除 't')
inention -> enention (将 'i' 替换为 'e')
enention -> exention (将 'n' 替换为 'x')
exention -> exection (将 'n' 替换为 'c')
exection -> execution (插入 'u')
```

 
提示：

 `0 <= word1.length, word2.length <= 500` 
 `word1`  和  `word2`  由小写英文字母组成

---


---

## 技巧

> 来源：`LeetCode_Collection_Part_5.md:586`

## 136. 只出现一次的数字 (Easy)

给你一个  **非空**  整数数组  `nums`  ，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。
你必须设计并实现线性时间复杂度的算法来解决此问题，且该算法只使用常量额外空间。

 
 **示例 1 ：** 

 **输入：** nums = [2,2,1]
 **输出：** 1

 **示例 2 ：** 

 **输入：** nums = [4,1,2,1,2]
 **输出：** 4

 **示例 3 ：** 

 **输入：** nums = [1]
 **输出：** 1

 
 **提示：** 

 `1 <= nums.length <= 3 * 104` 
 `-3 * 104 <= nums[i] <= 3 * 104` 
除了某个元素只出现一次以外，其余每个元素均出现两次。

---


---

> 来源：`LeetCode_Collection_Part_6.md:711`

## 169. 多数元素 (Easy)

给定一个大小为  `n`  的数组  `nums`  ，返回其中的多数元素。多数元素是指在数组中出现次数  **大于**   `⌊ n/2 ⌋`  的元素。
你可以假设数组是非空的，并且给定的数组总是存在多数元素。

 **示例 1：**

```text
输入：nums = [3,2,3]
输出：3
```

 **示例 2：**

```text
输入：nums = [2,2,1,1,1,2,2]
输出：2
```


 **提示：**

 `n == nums.length`
 `1 <= n <= 5 * 104`
 `-109 <= nums[i] <= 109`
输入保证数组中一定有一个多数元素。


 **进阶：** 尝试设计时间复杂度为 O(n)、空间复杂度为 O(1) 的算法解决此问题。

---


---

> 来源：`LeetCode_Collection_Part3.md:554`

## 75. 颜色分类 (Medium)

给定一个包含红色、白色和蓝色、共  `n`  个元素的数组  `nums`  ，原地 对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。
我们使用整数  `0` 、  `1`  和  `2`  分别表示红色、白色和蓝色。

必须在不使用库内置的 sort 函数的情况下解决这个问题。
 
示例 1：

```text
输入：nums = [2,0,2,1,1,0]
输出：[0,0,1,1,2,2]
```

示例 2：

```text
输入：nums = [2,0,1]
输出：[0,1,2]
```

 
提示：

 `n == nums.length` 
 `1 <= n <= 300` 
 `nums[i]`  为  `0` 、 `1`  或  `2` 

 
进阶：

你能想出一个仅使用常数空间的一趟扫描算法吗？

---


---

> 来源：`LeetCode_Collection_Part2.md:1`

## 31. 下一个排列 (Medium)

整数数组的一个 排列  就是将其所有成员以序列或线性顺序排列。

例如， `arr = [1,2,3]`  ，以下这些都可以视作  `arr`  的排列： `[1,2,3]` 、 `[1,3,2]` 、 `[3,1,2]` 、 `[2,3,1]`  。

整数数组的 下一个排列 是指其整数的下一个字典序更大的排列。更正式地，如果数组的所有排列根据其字典顺序从小到大排列在一个容器中，那么数组的 下一个排列 就是在这个有序容器中排在它后面的那个排列。如果不存在下一个更大的排列，那么这个数组必须重排为字典序最小的排列（即，其元素按升序排列）。

例如， `arr = [1,2,3]`  的下一个排列是  `[1,3,2]`  。
类似地， `arr = [2,3,1]`  的下一个排列是  `[3,1,2]`  。
而  `arr = [3,2,1]`  的下一个排列是  `[1,2,3]`  ，因为  `[3,2,1]`  不存在一个字典序更大的排列。

给你一个整数数组  `nums`  ，找出  `nums`  的下一个排列。
必须 原地 修改，只允许使用额外常数空间。
 
示例 1：

```text
输入：nums = [1,2,3]
输出：[1,3,2]
```

示例 2：

```text
输入：nums = [3,2,1]
输出：[1,2,3]
```

示例 3：

```text
输入：nums = [1,1,5]
输出：[1,5,1]
```

 
提示：

 `1 <= nums.length <= 100` 
 `0 <= nums[i] <= 100`


---

> 来源：`LeetCode_Collection_Part_10.md:335`

## 287. 寻找重复数 (Medium)

给定一个包含  `n + 1`  个整数的数组  `nums`  ，其数字都在  `[1, n]`  范围内（包括  `1`  和  `n` ），可知至少存在一个重复的整数。
假设  `nums`  只有  **一个重复的整数**  ，返回  **这个重复的数**  。
你设计的解决方案必须  **不修改**  数组  `nums`  且只用常量级  `O(1)`  的额外空间。

 **示例 1：**

```text
输入：nums = [1,3,4,2,2]
输出：2
```

 **示例 2：**

```text
输入：nums = [3,1,3,4,2]
输出：3
```

 **示例 3 :**

```text
输入：nums = [3,3,3,3,3]
输出：3
```



 **提示：**

 `1 <= n <= 105`
 `nums.length == n + 1`
 `1 <= nums[i] <= n`
 `nums`  中  **只有一个整数**  出现  **两次或多次**  ，其余整数均只出现  **一次**


 **进阶：**

如何证明  `nums`  中至少存在一个重复的数字?
你可以设计一个线性级时间复杂度  `O(n)`  的解决方案吗？

---


---
