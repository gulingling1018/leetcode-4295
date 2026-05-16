# LeetCode 题目合集 Part 6

## 151. 反转字符串中的单词 (Medium)

给你一个字符串  `s`  ，请你反转字符串中  **单词**  的顺序。
 **单词**  是由非空格字符组成的字符串。 `s`  中使用至少一个空格将字符串中的  **单词**  分隔开。
返回  **单词**  顺序颠倒且  **单词**  之间用单个空格连接的结果字符串。
 **注意：** 输入字符串  `s` 中可能会存在前导空格、尾随空格或者单词间的多个空格。返回的结果字符串中，单词间应当仅用单个空格分隔，且不包含任何额外的空格。

 **示例 1：**

```text
输入：s = "the sky is blue"
输出："blue is sky the"
```

 **示例 2：**

```text
输入：s = "  hello world  "
输出："world hello"
解释：反转后的字符串中不能存在前导空格和尾随空格。
```

 **示例 3：**

```text
输入：s = "a good   example"
输出："example good a"
解释：如果两个单词间有多余的空格，反转后的字符串需要将单词间的空格减少到仅有一个。
```


 **提示：**

 `1 <= s.length <= 104`
 `s`  包含英文大小写字母、数字和空格  `' '`
 `s`  中  **至少存在一个**  单词


 **进阶：** 如果字符串在你使用的编程语言中是一种可变数据类型，请尝试使用  `O(1)`  额外空间复杂度的  **原地**  解法。

---

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

## 154. 寻找旋转排序数组中的最小值 II (Hard)

已知一个长度为  `n`  的数组，预先按照升序排列，经由  `1`  到  `n`  次  **旋转**  后，得到输入数组。例如，原数组  `nums = [0,1,4,4,5,6,7]`  在变化后可能得到：

若旋转  `4`  次，则可以得到  `[4,5,6,7,0,1,4]`
若旋转  `7`  次，则可以得到  `[0,1,4,4,5,6,7]`

注意，数组  `[a[0], a[1], a[2], ..., a[n-1]]`   **旋转一次**  的结果为数组  `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]`  。
给你一个可能存在  **重复**  元素值的数组  `nums`  ，它原来是一个升序排列的数组，并按上述情形进行了多次旋转。请你找出并返回数组中的  **最小元素**  。
你必须尽可能减少整个过程的操作步骤。

 **示例 1：**

```text
输入：nums = [1,3,5]
输出：1
```

 **示例 2：**

```text
输入：nums = [2,2,2,0,1]
输出：0
```


 **提示：**

 `n == nums.length`
 `1 <= n <= 5000`
 `-5000 <= nums[i] <= 5000`
 `nums`  原来是一个升序排序的数组，并进行了  `1`  至  `n`  次旋转


 **进阶：** 这道题与 寻找旋转排序数组中的最小值 类似，但  `nums`  可能包含重复元素。允许重复会影响算法的时间复杂度吗？会如何影响，为什么？

---

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

## 156. 上下翻转二叉树 (Medium)

给你一棵二叉树的根节点 `root`，请将这棵树上下翻转，并返回新的根节点。
翻转规则按层执行：

1. 原来的左子节点变成新的根节点。
2. 原来的根节点变成新的右子节点。
3. 原来的右子节点变成新的左子节点。

题目保证每个右子节点都有一个同父节点的左兄弟，并且每个右子节点都没有子节点。

示例 1：

```text
输入：root = [1,2,3,4,5]
输出：[4,5,2,null,null,3,1]
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

`0 <= 节点数 <= 10`
`1 <= Node.val <= 10`

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

---

## 157. 用 Read4 读取 N 个字符 (Easy)

给定一个文件，并假设只能通过给定的 `read4` 方法读取文件，请实现一个方法 `read` 读取 `n` 个字符。

`read4` API 每次从文件中读取连续的最多 4 个字符，并写入缓冲数组 `buf4`，返回实际读取的字符数量。`read4` 有自己的文件指针。

请实现：

```text
int read(char[] buf, int n)
```

它应使用 `read4` 从文件中读取最多 `n` 个字符并写入 `buf`，返回实际读取的字符数。你不能直接操作文件本身。每个测试用例中 `read` 只会调用一次，并且 `buf` 有足够空间保存 `n` 个字符。

示例 1：

```text
输入：file = "abc", n = 4
输出：3
解释：buf 中应包含 "abc"，实际读取 3 个字符。
```

示例 2：

```text
输入：file = "abcde", n = 5
输出：5
```

示例 3：

```text
输入：file = "abcdABCD1234", n = 12
输出：12
```

提示：

`1 <= file.length <= 500`
`file` 由英文字母和数字组成
`1 <= n <= 1000`

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

---

## 158. 用 Read4 读取 N 个字符 II - 多次调用 (Hard)

给定一个文件，并假设只能通过给定的 `read4` 方法读取文件，请实现一个方法 `read` 读取 `n` 个字符。不同之处在于：你的 `read` 方法可能被多次调用。

`read4` API 每次从文件中读取连续的最多 4 个字符，并写入缓冲数组 `buf4`，返回实际读取的字符数量。`read4` 有自己的文件指针。

请实现：

```text
int read(char[] buf, int n)
```

它应使用 `read4` 从文件中读取最多 `n` 个字符并写入 `buf`，返回实际读取的字符数。因为 `read` 会被多次调用，你需要保存上一次 `read4` 读多但尚未消费的字符。

注意：

`read` 可能被多次调用；`buf` 有足够空间保存 `n` 个字符；每个测试用例之间应重置类变量。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

---

## 159. 至多包含两个不同字符的最长子串 (Medium)

给你一个字符串 `s`，请返回至多包含两个不同字符的最长子串长度。

示例 1：

```text
输入：s = "eceba"
输出：3
解释：最长子串是 "ece"，长度为 3。
```

示例 2：

```text
输入：s = "ccaabbb"
输出：5
解释：最长子串是 "aabbb"，长度为 5。
```

提示：

`1 <= s.length <= 10^5`
`s` 由英文字母组成。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

---

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

## 161. 相隔为 1 的编辑距离 (Medium)

给定两个字符串 `s` 和 `t`，判断它们是否恰好相隔一个编辑距离。

一次编辑可以是以下三种操作之一：

1. 向 `s` 插入一个字符得到 `t`。
2. 从 `s` 删除一个字符得到 `t`。
3. 替换 `s` 中的一个字符得到 `t`。

示例 1：

```text
输入：s = "ab", t = "acb"
输出：true
解释：可以向 s 插入 'c' 得到 t。
```

示例 2：

```text
输入：s = "cab", t = "ad"
输出：false
```

示例 3：

```text
输入：s = "1203", t = "1213"
输出：true
解释：可以将 '0' 替换为 '1'。
```

题面补充来源：leetcode.ca，核对日期：2026-05-15。

---

## 162. 寻找峰值 (Medium)

峰值元素是指其值严格大于左右相邻值的元素。
给你一个整数数组  `nums` ，找到峰值元素并返回其索引。数组可能包含多个峰值，在这种情况下，返回  **任何一个峰值**  所在位置即可。
你可以假设  `nums[-1] = nums[n] = -∞`  。
你必须实现时间复杂度为  `O(log n)`  的算法来解决此问题。

 **示例 1：**

```text
输入：nums = [1,2,3,1]
输出：2
解释：3 是峰值元素，你的函数应该返回其索引 2。
```

 **示例 2：**

```text
输入：nums = [1,2,1,3,5,6,4]
输出：1 或 5
解释：你的函数可以返回索引 1，其峰值元素为 2；
     或者返回索引 5， 其峰值元素为 6。
```


 **提示：**

 `1 <= nums.length <= 1000`
 `-231 <= nums[i] <= 231 - 1`
对于所有有效的  `i`  都有  `nums[i] != nums[i + 1]`

---

## 163. 缺失的区间 (Easy)

给你一个闭区间 `[lower, upper]` 和一个升序且元素唯一的整数数组 `nums`，数组中的所有元素都在该闭区间内。

如果一个数 `x` 在 `[lower, upper]` 范围内，但不在 `nums` 中，则认为它缺失。请返回最短的、按升序排列的区间列表，恰好覆盖所有缺失数字。

示例 1：

```text
输入：nums = [0,1,3,50,75], lower = 0, upper = 99
输出：[[2,2],[4,49],[51,74],[76,99]]
```

示例 2：

```text
输入：nums = [-1], lower = -1, upper = -1
输出：[]
```

提示：

`-10^9 <= lower <= upper <= 10^9`
`0 <= nums.length <= 100`
`lower <= nums[i] <= upper`
`nums` 中所有值互不相同。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

---

## 164. 最大间距 (Medium)

给定一个无序的数组  `nums` ，返回 数组在排序之后，相邻元素之间最大的差值 。如果数组元素个数小于 2，则返回  `0`  。
您必须编写一个在「线性时间」内运行并使用「线性额外空间」的算法。

 **示例 1:**

```text
输入: nums = [3,6,9,1]
输出: 3
解释: 排序后的数组是 [1,3,6,9], 其中相邻元素 (3,6) 和 (6,9) 之间都存在最大差值 3。
```

 **示例 2:**

```text
输入: nums = [10]
输出: 0
解释: 数组元素个数小于 2，因此返回 0。
```


 **提示:**

 `1 <= nums.length <= 105`
 `0 <= nums[i] <= 109`

---

## 165. 比较版本号 (Medium)

给你两个  **版本号字符串**   `version1`  和  `version2`  ，请你比较它们。版本号由被点  `'.'`  分开的修订号组成。 **修订号的值**  是它  **转换为整数**  并忽略前导零。
比较版本号时，请按  **从左到右的顺序**  依次比较它们的修订号。如果其中一个版本字符串的修订号较少，则将缺失的修订号视为  `0` 。
返回规则如下：

如果  `version1 < version2`  返回  `-1` ，
如果  `version1 > version2`  返回  `1` ，
除此之外返回  `0` 。


 **示例 1：**

 **输入：** version1 = "1.2", version2 = "1.10"
 **输出：** -1
 **解释：**
version1 的第二个修订号为 "2"，version2 的第二个修订号为 "10"：2 < 10，所以 version1 < version2。

 **示例 2：**

 **输入：** version1 = "1.01", version2 = "1.001"
 **输出：** 0
 **解释：**
忽略前导零，"01" 和 "001" 都代表相同的整数 "1"。

 **示例 3：**

 **输入：** version1 = "1.0", version2 = "1.0.0.0"
 **输出：** 0
 **解释：**
version1 有更少的修订号，每个缺失的修订号按 "0" 处理。


 **提示：**

 `1 <= version1.length, version2.length <= 500`
 `version1`  和  `version2`  仅包含数字和  `'.'`
 `version1`  和  `version2`  都是  **有效版本号**
 `version1`  和  `version2`  的所有修订号都可以存储在  **32 位整数**  中

---

## 166. 分数到小数 (Medium)

给定两个整数，分别表示分数的分子  `numerator`  和分母  `denominator` ，以  **字符串形式返回小数**  。
如果小数部分为循环小数，则将循环的部分括在括号内。
如果存在多个答案，只需返回  **任意一个**  。
对于所有给定的输入， **保证**  答案字符串的长度小于  `104`  。
 **注意** ，如果分数可以表示为有限长度的字符串，则  **必须**  返回它。

 **示例 1：**

```text
输入：numerator = 1, denominator = 2
输出："0.5"
```

 **示例 2：**

```text
输入：numerator = 2, denominator = 1
输出："2"
```

 **示例 3：**

```text
输入：numerator = 4, denominator = 333
输出："0.(012)"
```


 **提示：**

 `-231 <= numerator, denominator <= 231 - 1`
 `denominator != 0`

---

## 167. 两数之和 II - 输入有序数组 (Medium)

给你一个下标从  **1**  开始的整数数组  `numbers`  ，该数组已按 **非递减顺序排列** ，请你从数组中找出满足相加之和等于目标数  `target`  的两个数。如果设这两个数分别是  `numbers[index1]`  和  `numbers[index2]`  ，则  `1 <= index1 < index2 <= numbers.length`  。
以长度为 2 的整数数组  `[index1, index2]`  的形式返回这两个整数的下标  `index1`  和  `index2` 。
你可以假设每个输入  **只对应唯一的答案**  ，而且你  **不可以**  重复使用相同的元素。
你所设计的解决方案必须只使用常量级的额外空间。


 **示例 1：**

```text
输入：numbers = [2,7,11,15], target = 9
输出：[1,2]
解释：2 与 7 之和等于目标数 9 。因此 index1 = 1, index2 = 2 。返回 [1, 2] 。
```

 **示例 2：**

```text
输入：numbers = [2,3,4], target = 6
输出：[1,3]
解释：2 与 4 之和等于目标数 6 。因此 index1 = 1, index2 = 3 。返回 [1, 3] 。
```

 **示例 3：**

```text
输入：numbers = [-1,0], target = -1
输出：[1,2]
解释：-1 与 0 之和等于目标数 -1 。因此 index1 = 1, index2 = 2 。返回 [1, 2] 。
```


 **提示：**

 `2 <= numbers.length <= 3 * 104`
 `-1000 <= numbers[i] <= 1000`
 `numbers`  按  **非递减顺序**  排列
 `-1000 <= target <= 1000`
 **仅存在一个有效答案**

---

## 168. Excel 表列名称 (Easy)

给你一个整数  `columnNumber`  ，返回它在 Excel 表中相对应的列名称。
例如：

```text
A -> 1
B -> 2
C -> 3
...
Z -> 26
AA -> 27
AB -> 28
...
```


 **示例 1：**

```text
输入：columnNumber = 1
输出："A"
```

 **示例 2：**

```text
输入：columnNumber = 28
输出："AB"
```

 **示例 3：**

```text
输入：columnNumber = 701
输出："ZY"
```

 **示例 4：**

```text
输入：columnNumber = 2147483647
输出："FXSHRXW"
```


 **提示：**

 `1 <= columnNumber <= 231 - 1`

---

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

## 170. 两数之和 III - 数据结构设计 (Easy)

请设计一个数据结构，它接收一个整数数据流，并能检查其中是否存在两个整数之和等于某个给定值。

实现 `TwoSum` 类：

```text
TwoSum() 初始化对象，初始为空。
void add(int number) 向数据结构中添加 number。
boolean find(int value) 如果存在两个数之和等于 value，返回 true；否则返回 false。
```

示例：

```text
输入：
["TwoSum", "add", "add", "add", "find", "find"]
[[], [1], [3], [5], [4], [7]]
输出：
[null, null, null, null, true, false]

解释：
TwoSum twoSum = new TwoSum();
twoSum.add(1);
twoSum.add(3);
twoSum.add(5);
twoSum.find(4); // 1 + 3 = 4，返回 true
twoSum.find(7); // 不存在两个数之和为 7，返回 false
```

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

---

## 171. Excel 表列序号 (Easy)

给你一个字符串  `columnTitle`  ，表示 Excel 表格中的列名称。返回 该列名称对应的列序号 。
例如：

```text
A -> 1
B -> 2
C -> 3
...
Z -> 26
AA -> 27
AB -> 28
...
```


 **示例 1:**

```text
输入: columnTitle = "A"
输出: 1
```

 **示例 2:**

```text
输入: columnTitle = "AB"
输出: 28
```

 **示例 3:**

```text
输入: columnTitle = "ZY"
输出: 701
```


 **提示：**

 `1 <= columnTitle.length <= 7`
 `columnTitle`  仅由大写英文组成
 `columnTitle`  在范围  `["A", "FXSHRXW"]`  内

---

## 172. 阶乘后的零 (Medium)

给定一个整数  `n`  ，返回  `n!`  结果中尾随零的数量。
提示  `n! = n * (n - 1) * (n - 2) * ... * 3 * 2 * 1`

 **示例 1：**

```text
输入：n = 3
输出：0
解释：3! = 6 ，不含尾随 0
```

 **示例 2：**

```text
输入：n = 5
输出：1
解释：5! = 120 ，有一个尾随 0
```

 **示例 3：**

```text
输入：n = 0
输出：0
```


 **提示：**

 `0 <= n <= 104`


 **进阶：** 你可以设计并实现对数时间复杂度的算法来解决此问题吗？

---

## 173. 二叉搜索树迭代器 (Medium)

实现一个二叉搜索树迭代器类 `BSTIterator`  ，表示一个按中序遍历二叉搜索树（BST）的迭代器：

 `BSTIterator(TreeNode root)`  初始化  `BSTIterator`  类的一个对象。BST 的根节点  `root`  会作为构造函数的一部分给出。指针应初始化为一个不存在于 BST 中的数字，且该数字小于 BST 中的任何元素。
 `boolean hasNext()`  如果向指针右侧遍历存在数字，则返回  `true`  ；否则返回  `false`  。
 `int next()` 将指针向右移动，然后返回指针处的数字。

注意，指针初始化为一个不存在于 BST 中的数字，所以对  `next()`  的首次调用将返回 BST 中的最小元素。

你可以假设  `next()`  调用总是有效的，也就是说，当调用  `next()`  时，BST 的中序遍历中至少存在一个下一个数字。

 **示例：**

```text
输入
["BSTIterator", "next", "next", "hasNext", "next", "hasNext", "next", "hasNext", "next", "hasNext"]
[[[7, 3, 15, null, null, 9, 20]], [], [], [], [], [], [], [], [], []]
输出
[null, 3, 7, true, 9, true, 15, true, 20, false]

解释
BSTIterator bSTIterator = new BSTIterator([7, 3, 15, null, null, 9, 20]);
bSTIterator.next();    // 返回 3
bSTIterator.next();    // 返回 7
bSTIterator.hasNext(); // 返回 True
bSTIterator.next();    // 返回 9
bSTIterator.hasNext(); // 返回 True
bSTIterator.next();    // 返回 15
bSTIterator.hasNext(); // 返回 True
bSTIterator.next();    // 返回 20
bSTIterator.hasNext(); // 返回 False
```


 **提示：**

树中节点的数目在范围  `[1, 105]`  内
 `0 <= Node.val <= 106`
最多调用  `105`  次  `hasNext`  和  `next`  操作


 **进阶：**

你可以设计一个满足下述条件的解决方案吗？ `next()`  和  `hasNext()`  操作均摊时间复杂度为  `O(1)`  ，并使用  `O(h)`  内存。其中  `h`  是树的高度。

---

## 174. 地下城游戏 (Hard)

恶魔们抓住了公主并将她关在了地下城  `dungeon`  的  **右下角**  。地下城是由  `m x n`  个房间组成的二维网格。我们英勇的骑士最初被安置在  **左上角**  的房间里，他必须穿过地下城并通过对抗恶魔来拯救公主。
骑士的初始健康点数为一个正整数。如果他的健康点数在某一时刻降至 0 或以下，他会立即死亡。
有些房间由恶魔守卫，因此骑士在进入这些房间时会失去健康点数（若房间里的值为负整数，则表示骑士将损失健康点数）；其他房间要么是空的（房间里的值为 0），要么包含增加骑士健康点数的魔法球（若房间里的值为正整数，则表示骑士将增加健康点数）。
为了尽快解救公主，骑士决定每次只  **向右**  或  **向下**  移动一步。
返回确保骑士能够拯救到公主所需的最低初始健康点数。
 **注意：** 任何房间都可能对骑士的健康点数造成威胁，也可能增加骑士的健康点数，包括骑士进入的左上角房间以及公主被监禁的右下角房间。

 **示例 1：**

```text
输入：dungeon = [[-2,-3,3],[-5,-10,1],[10,30,-5]]
输出：7
解释：如果骑士遵循最佳路径：右 -> 右 -> 下 -> 下 ，则骑士的初始健康点数至少为 7 。
```

 **示例 2：**

```text
输入：dungeon = [[0]]
输出：1
```


 **提示：**

 `m == dungeon.length`
 `n == dungeon[i].length`
 `1 <= m, n <= 200`
 `-1000 <= dungeon[i][j] <= 1000`

---

## 175. 组合两个表 (Easy)

表:  `Person`

```text
+-------------+---------+
| 列名         | 类型     |
+-------------+---------+
| PersonId    | int     |
| FirstName   | varchar |
| LastName    | varchar |
+-------------+---------+
personId 是该表的主键（具有唯一值的列）。
该表包含一些人的 ID 和他们的姓和名的信息。
```


表:  `Address`

```text
+-------------+---------+
| 列名         | 类型    |
+-------------+---------+
| AddressId   | int     |
| PersonId    | int     |
| City        | varchar |
| State       | varchar |
+-------------+---------+
addressId 是该表的主键（具有唯一值的列）。
该表的每一行都包含一个 ID = PersonId 的人的城市和州的信息。
```


编写解决方案，报告  `Person`  表中每个人的姓、名、城市和州。如果  `personId`  的地址不在  `Address`  表中，则报告为  `null`  。
以  **任意顺序**  返回结果表。
结果格式如下所示。

 **示例 1:**

```text
输入:
Person表:
+----------+----------+-----------+
| personId | lastName | firstName |
+----------+----------+-----------+
| 1        | Wang     | Allen     |
| 2        | Alice    | Bob       |
+----------+----------+-----------+
Address表:
+-----------+----------+---------------+------------+
| addressId | personId | city          | state      |
+-----------+----------+---------------+------------+
| 1         | 2        | New York City | New York   |
| 2         | 3        | Leetcode      | California |
+-----------+----------+---------------+------------+
输出:
+-----------+----------+---------------+----------+
| firstName | lastName | city          | state    |
+-----------+----------+---------------+----------+
| Allen     | Wang     | Null          | Null     |
| Bob       | Alice    | New York City | New York |
+-----------+----------+---------------+----------+
解释:
地址表中没有 personId = 1 的地址，所以它们的城市和州返回 null。
addressId = 1 包含了 personId = 2 的地址信息。
```

---

## 176. 第二高的薪水 (Medium)

`Employee`  表：

```text
+-------------+------+
| Column Name | Type |
+-------------+------+
| id          | int  |
| salary      | int  |
+-------------+------+
id 是这个表的主键。
表的每一行包含员工的工资信息。
```


查询并返回  `Employee`  表中第二高的  **不同**  薪水 。如果不存在第二高的薪水，查询应该返回  `null(Pandas 则返回 None)`  。
查询结果如下例所示。

 **示例 1：**

```text
输入：
Employee 表：
+----+--------+
| id | salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
输出：
+---------------------+
| SecondHighestSalary |
+---------------------+
| 200                 |
+---------------------+
```

 **示例 2：**

```text
输入：
Employee 表：
+----+--------+
| id | salary |
+----+--------+
| 1  | 100    |
+----+--------+
输出：
+---------------------+
| SecondHighestSalary |
+---------------------+
| null                |
+---------------------+
```

---

## 177. 第N高的薪水 (Medium)

表:  `Employee`

```text
+-------------+------+
| Column Name | Type |
+-------------+------+
| id          | int  |
| salary      | int  |
+-------------+------+
id 是该表的主键（列中的值互不相同）。
该表的每一行都包含有关员工工资的信息。
```


编写一个解决方案查询  `Employee`  表中第  `n`  高的  **不同**  工资。如果少于  `n`  个不同工资，查询结果应该为  `null`  。
查询结果格式如下所示。

 **示例 1:**

```text
输入:
Employee table:
+----+--------+
| id | salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
n = 2
输出:
+------------------------+
| getNthHighestSalary(2) |
+------------------------+
| 200                    |
+------------------------+
```

 **示例 2:**

```text
输入:
Employee 表:
+----+--------+
| id | salary |
+----+--------+
| 1  | 100    |
+----+--------+
n = 2
输出:
+------------------------+
| getNthHighestSalary(2) |
+------------------------+
| null                   |
+------------------------+
```

---

## 178. 分数排名 (Medium)

表:  `Scores`

```text
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| score       | decimal |
+-------------+---------+
id 是该表的主键（有不同值的列）。
该表的每一行都包含了一场比赛的分数。Score 是一个有两位小数点的浮点值。
```


编写一个解决方案来查询分数的排名。排名按以下规则计算:

分数应按从高到低排列。
如果两个分数相等，那么两个分数的排名应该相同。
在排名相同的分数后，排名数应该是下一个连续的整数。换句话说，排名之间不应该有空缺的数字。

按  `score`  降序返回结果表。
查询结果格式如下所示。

 **示例 1:**

```text
输入:
Scores 表:
+----+-------+
| id | score |
+----+-------+
| 1  | 3.50  |
| 2  | 3.65  |
| 3  | 4.00  |
| 4  | 3.85  |
| 5  | 4.00  |
| 6  | 3.65  |
+----+-------+
输出:
+-------+------+
| score | rank |
+-------+------+
| 4.00  | 1    |
| 4.00  | 1    |
| 3.85  | 2    |
| 3.65  | 3    |
| 3.65  | 3    |
| 3.50  | 4    |
+-------+------+
```

---

## 179. 最大数 (Medium)

给定一组非负整数  `nums` ，重新排列每个数的顺序（每个数不可拆分）使之组成一个最大的整数。
 **注意：** 输出结果可能非常大，所以你需要返回一个字符串而不是整数。

 **示例 1：**

```text
输入：nums = [10,2]
输出："210"
```

 **示例 2：**

```text
输入：nums = [3,30,34,5,9]
输出："9534330"
```


 **提示：**

 `1 <= nums.length <= 100`
 `0 <= nums[i] <= 109`

---

## 180. 连续出现的数字 (Medium)

表： `Logs`

```text
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| num         | varchar |
+-------------+---------+
在 SQL 中，id 是该表的主键。
id 是一个自增列。
```


找出所有至少连续出现三次的数字。
返回的结果表中的数据可以按  **任意顺序**  排列。
结果格式如下面的例子所示：

 **示例 1:**

```text
输入：
Logs 表：
+----+-----+
| id | num |
+----+-----+
| 1  | 1   |
| 2  | 1   |
| 3  | 1   |
| 4  | 2   |
| 5  | 1   |
| 6  | 2   |
| 7  | 2   |
+----+-----+
输出：
Result 表：
+-----------------+
| ConsecutiveNums |
+-----------------+
| 1               |
+-----------------+
解释：1 是唯一连续出现至少三次的数字。
```

---

# Java/SQL 解法补充附录（151-180）

### 151. 反转字符串中的单词

**基础解法：** `trim + split + 反向拼接`，先去掉首尾空格，再按连续空白切分单词。

```java
class Solution {
    public String reverseWords(String s) {
        String[] words = s.trim().split("\\s+");
        StringBuilder ans = new StringBuilder();
        for (int i = words.length - 1; i >= 0; i--) {
            if (ans.length() > 0) ans.append(' ');
            ans.append(words[i]);
        }
        return ans.toString();
    }
}
```

**资深解法：** 若输入是可变字符数组，可整体反转再逐词反转；本题 Java `String` 不可变，实际提交以切分法最简洁。
**基础语法与思想：** `split("\\s+")` 使用正则匹配连续空白；算法核心是“单词级别反转”，时间 `O(n)`，空间 `O(n)`。

### 152. 乘积最大子数组

**基础解法：** 暴力枚举每个起点并持续累乘，维护最大值，时间 `O(n^2)`。

**资深解法：** 动态规划同时维护以当前位置结尾的最大积和最小积；遇到负数时最大/最小会互换。

```java
class Solution {
    public int maxProduct(int[] nums) {
        int max = nums[0], min = nums[0], ans = nums[0];
        for (int i = 1; i < nums.length; i++) {
            int x = nums[i];
            if (x < 0) {
                int t = max;
                max = min;
                min = t;
            }
            max = Math.max(x, max * x);
            min = Math.min(x, min * x);
            ans = Math.max(ans, max);
        }
        return ans;
    }
}
```

**基础语法与思想：** `Math.max/min` 滚动更新状态；负数会改变乘积符号，所以要保留最小积。时间 `O(n)`，空间 `O(1)`。

### 153. 寻找旋转排序数组中的最小值

**基础解法：** 线性扫描找到第一个下降点或全局最小值，时间 `O(n)`。

**资深解法：** 二分比较 `nums[mid]` 与 `nums[right]`；右侧有序且 `mid` 更大时最小值在右边，否则在左边含 `mid`。

```java
class Solution {
    public int findMin(int[] nums) {
        int left = 0, right = nums.length - 1;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] > nums[right]) left = mid + 1;
            else right = mid;
        }
        return nums[left];
    }
}
```

**基础语法与思想：** 二分循环条件用 `left < right`，收敛后 `left` 即答案；无重复元素时判断明确，时间 `O(log n)`。

### 154. 寻找旋转排序数组中的最小值 II

**基础解法：** 直接遍历求最小值，时间 `O(n)`。

**资深解法：** 含重复元素时仍二分；当 `nums[mid] == nums[right]` 无法判断方向，只能安全地 `right--`。

```java
class Solution {
    public int findMin(int[] nums) {
        int left = 0, right = nums.length - 1;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] > nums[right]) left = mid + 1;
            else if (nums[mid] < nums[right]) right = mid;
            else right--;
        }
        return nums[left];
    }
}
```

**基础语法与思想：** `right--` 不会丢失唯一最小值，因为 `nums[mid] == nums[right]` 时右端点可被同值替代；最坏时间 `O(n)`，平均接近 `O(log n)`。

### 155. 最小栈

**基础解法：** 普通栈保存元素，`getMin` 时遍历栈求最小值，查询 `O(n)`。

**资深解法：** 双栈：数据栈保存所有值，最小栈同步保存当前位置的最小值。

```java
class MinStack {
    private Deque<Integer> stack = new ArrayDeque<>();
    private Deque<Integer> mins = new ArrayDeque<>();

    public void push(int val) {
        stack.push(val);
        mins.push(mins.isEmpty() ? val : Math.min(val, mins.peek()));
    }

    public void pop() {
        stack.pop();
        mins.pop();
    }

    public int top() {
        return stack.peek();
    }

    public int getMin() {
        return mins.peek();
    }
}
```

**基础语法与思想：** `Deque` 的 `push/pop/peek` 可当栈用；用空间换 `getMin` 的 `O(1)` 查询。

### 156. 上下翻转二叉树

**基础解法：** 递归到底部最左节点作为新根，回溯时重连当前节点、左子节点、右子节点。

```java
class Solution {
    public TreeNode upsideDownBinaryTree(TreeNode root) {
        if (root == null || root.left == null) return root;
        TreeNode newRoot = upsideDownBinaryTree(root.left);
        root.left.left = root.right;
        root.left.right = root;
        root.left = null;
        root.right = null;
        return newRoot;
    }
}
```

**资深解法：** 迭代维护 `parent` 与原右子树，沿左链向下翻转，空间 `O(1)`。

```java
class Solution {
    public TreeNode upsideDownBinaryTree(TreeNode root) {
        TreeNode parent = null, parentRight = null;
        while (root != null) {
            TreeNode next = root.left;
            root.left = parentRight;
            parentRight = root.right;
            root.right = parent;
            parent = root;
            root = next;
        }
        return parent;
    }
}
```

**基础语法与思想：** 重连指针前必须先保存 `next`；树形翻转本质是把左链改造成新右链。

### 157. 用 Read4 读取 N 个字符

**基础解法：** 每次调用 `read4` 读取最多 4 个字符，再拷贝到目标缓冲区，直到读够或文件结束。

```java
public class Solution extends Reader4 {
    public int read(char[] buf, int n) {
        char[] tmp = new char[4];
        int total = 0;
        while (total < n) {
            int cnt = read4(tmp);
            if (cnt == 0) break;
            for (int i = 0; i < cnt && total < n; i++) {
                buf[total++] = tmp[i];
            }
        }
        return total;
    }
}
```

**资深解法：** 单次调用场景不需要跨调用缓存；关键是最后一批可能读到超过 `n` 的字符，只复制需要的部分。
**基础语法与思想：** `char[]` 是可变缓冲区；`read4` 返回实际读取量，返回 0 表示文件结束。

### 158. 用 Read4 读取 N 个字符 II - 多次调用

**基础解法：** 每次都调用 `read4` 会丢失上次多读的字符，不能通过多次调用测试。

**资深解法：** 对象字段保存 `read4` 的剩余字符，多次 `read` 共享这段缓存。

```java
public class Solution extends Reader4 {
    private char[] cache = new char[4];
    private int size = 0;
    private int index = 0;

    public int read(char[] buf, int n) {
        int total = 0;
        while (total < n) {
            if (index == size) {
                size = read4(cache);
                index = 0;
                if (size == 0) break;
            }
            while (total < n && index < size) {
                buf[total++] = cache[index++];
            }
        }
        return total;
    }
}
```

**基础语法与思想：** 类成员变量跨方法调用保留状态；这是“流式读取”的典型缓存设计。

### 159. 至多包含两个不同字符的最长子串

**基础解法：** 枚举左端点，向右扩展并用集合统计不同字符，超过 2 个停止，时间 `O(n^2)`。

**资深解法：** 滑动窗口维护字符频次，窗口中不同字符超过 2 时移动左端点。

```java
class Solution {
    public int lengthOfLongestSubstringTwoDistinct(String s) {
        Map<Character, Integer> count = new HashMap<>();
        int left = 0, ans = 0;
        for (int right = 0; right < s.length(); right++) {
            char c = s.charAt(right);
            count.put(c, count.getOrDefault(c, 0) + 1);
            while (count.size() > 2) {
                char d = s.charAt(left++);
                count.put(d, count.get(d) - 1);
                if (count.get(d) == 0) count.remove(d);
            }
            ans = Math.max(ans, right - left + 1);
        }
        return ans;
    }
}
```

**基础语法与思想：** `getOrDefault` 简化计数；窗口满足约束时更新答案，不满足时收缩。

### 160. 相交链表

**基础解法：** 用 `HashSet` 保存链表 A 的节点引用，再扫描 B 找第一个已出现节点。

**资深解法：** 双指针走完自己的链表后切到另一条链表，长度差会被第二段抵消，最终在交点或 `null` 相遇。

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode a = headA, b = headB;
        while (a != b) {
            a = a == null ? headB : a.next;
            b = b == null ? headA : b.next;
        }
        return a;
    }
}
```

**基础语法与思想：** 比较节点是否相交要用引用相等 `a == b`，不是比较节点值。时间 `O(m+n)`，空间 `O(1)`。

### 161. 相隔为 1 的编辑距离

**基础解法：** 递归/DP 判断编辑距离是否为 1，较重但直观。

**资深解法：** 双指针扫描，第一次不同后根据长度关系跳过一个字符或同时跳过。

```java
class Solution {
    public boolean isOneEditDistance(String s, String t) {
        int m = s.length(), n = t.length();
        if (Math.abs(m - n) > 1) return false;
        for (int i = 0; i < Math.min(m, n); i++) {
            if (s.charAt(i) != t.charAt(i)) {
                if (m == n) return s.substring(i + 1).equals(t.substring(i + 1));
                if (m < n) return s.substring(i).equals(t.substring(i + 1));
                return s.substring(i + 1).equals(t.substring(i));
            }
        }
        return Math.abs(m - n) == 1;
    }
}
```

**基础语法与思想：** `substring` 比较剩余后缀；必须排除完全相同的字符串，因为题目要求“恰好一次编辑”。

### 162. 寻找峰值

**基础解法：** 扫描数组，找到第一个大于左右相邻元素的位置。

**资深解法：** 二分坡度：若 `nums[mid] < nums[mid + 1]`，右侧一定存在峰值；否则左侧含 `mid` 存在峰值。

```java
class Solution {
    public int findPeakElement(int[] nums) {
        int left = 0, right = nums.length - 1;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] < nums[mid + 1]) left = mid + 1;
            else right = mid;
        }
        return left;
    }
}
```

**基础语法与思想：** 循环中访问 `mid + 1`，所以条件必须是 `left < right`；峰值可不唯一，返回任一即可。

### 163. 缺失的区间

**基础解法：** 在首尾加哨兵，逐段检查相邻数字之间是否有空缺。

```java
class Solution {
    public List<List<Integer>> findMissingRanges(int[] nums, int lower, int upper) {
        List<List<Integer>> ans = new ArrayList<>();
        long prev = (long) lower - 1;
        for (int i = 0; i <= nums.length; i++) {
            long cur = i == nums.length ? (long) upper + 1 : nums[i];
            if (cur - prev >= 2) {
                ans.add(Arrays.asList((int) (prev + 1), (int) (cur - 1)));
            }
            prev = cur;
        }
        return ans;
    }
}
```

**资深解法：** 使用 `long` 做哨兵，避免 `lower - 1` 或 `upper + 1` 在整数边界溢出。
**基础语法与思想：** `Arrays.asList(a, b)` 生成闭区间；思想是检查两个已存在边界之间的空档。

### 164. 最大间距

**基础解法：** 排序后扫描相邻差值，时间 `O(n log n)`。

**资深解法：** 桶排序思想。若最大间距存在，一定出现在非空桶之间；桶内最大差不会超过桶宽。

```java
class Solution {
    public int maximumGap(int[] nums) {
        int n = nums.length;
        if (n < 2) return 0;
        int min = nums[0], max = nums[0];
        for (int x : nums) {
            min = Math.min(min, x);
            max = Math.max(max, x);
        }
        if (min == max) return 0;
        int size = Math.max(1, (max - min) / (n - 1));
        int count = (max - min) / size + 1;
        int[] bucketMin = new int[count];
        int[] bucketMax = new int[count];
        Arrays.fill(bucketMin, Integer.MAX_VALUE);
        Arrays.fill(bucketMax, Integer.MIN_VALUE);
        for (int x : nums) {
            int idx = (x - min) / size;
            bucketMin[idx] = Math.min(bucketMin[idx], x);
            bucketMax[idx] = Math.max(bucketMax[idx], x);
        }
        int ans = 0, prev = min;
        for (int i = 0; i < count; i++) {
            if (bucketMin[i] == Integer.MAX_VALUE) continue;
            ans = Math.max(ans, bucketMin[i] - prev);
            prev = bucketMax[i];
        }
        return ans;
    }
}
```

**基础语法与思想：** `Arrays.fill` 初始化桶；鸽巢原理保证线性桶方案可找到最大相邻差。

### 165. 比较版本号

**基础解法：** `split("\\.")` 切分版本段，逐段转整数比较。

```java
class Solution {
    public int compareVersion(String version1, String version2) {
        String[] a = version1.split("\\.");
        String[] b = version2.split("\\.");
        int n = Math.max(a.length, b.length);
        for (int i = 0; i < n; i++) {
            int x = i < a.length ? Integer.parseInt(a[i]) : 0;
            int y = i < b.length ? Integer.parseInt(b[i]) : 0;
            if (x != y) return x < y ? -1 : 1;
        }
        return 0;
    }
}
```

**资深解法：** 双指针逐字符解析数字段，可避免创建数组；本题段值范围可用 `int`。
**基础语法与思想：** 正则中点号需要写成 `"\\."`；版本比较忽略前导零和缺失尾段零。

### 166. 分数到小数

**基础解法：** 模拟长除法，把每次余数映射到小数位置；重复余数代表循环节开始。

```java
class Solution {
    public String fractionToDecimal(int numerator, int denominator) {
        if (numerator == 0) return "0";
        StringBuilder ans = new StringBuilder();
        if ((numerator < 0) ^ (denominator < 0)) ans.append('-');
        long num = Math.abs((long) numerator);
        long den = Math.abs((long) denominator);
        ans.append(num / den);
        long rem = num % den;
        if (rem == 0) return ans.toString();
        ans.append('.');
        Map<Long, Integer> seen = new HashMap<>();
        while (rem != 0) {
            if (seen.containsKey(rem)) {
                ans.insert(seen.get(rem).intValue(), "(");
                ans.append(')');
                break;
            }
            seen.put(rem, ans.length());
            rem *= 10;
            ans.append(rem / den);
            rem %= den;
        }
        return ans.toString();
    }
}
```

**资深解法：** 用 `long` 处理 `Integer.MIN_VALUE` 的绝对值，避免 `Math.abs(int)` 溢出。
**基础语法与思想：** `StringBuilder.insert` 可在循环节起点插入括号；核心是余数状态有限。

### 167. 两数之和 II - 输入有序数组

**基础解法：** 固定一个数后二分查找另一个数，时间 `O(n log n)`。

**资深解法：** 有序数组双指针，小了左移，大了右移。

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int left = 0, right = numbers.length - 1;
        while (left < right) {
            int sum = numbers[left] + numbers[right];
            if (sum == target) return new int[]{left + 1, right + 1};
            if (sum < target) left++;
            else right--;
        }
        return new int[0];
    }
}
```

**基础语法与思想：** 返回下标从 1 开始；双指针利用排序性质，时间 `O(n)`。

### 168. Excel 表列名称

**基础解法：** 类似进制转换，但 Excel 是 1 到 26 的映射，需要先 `columnNumber--` 再取模。

```java
class Solution {
    public String convertToTitle(int columnNumber) {
        StringBuilder ans = new StringBuilder();
        while (columnNumber > 0) {
            columnNumber--;
            ans.append((char) ('A' + columnNumber % 26));
            columnNumber /= 26;
        }
        return ans.reverse().toString();
    }
}
```

**资深解法：** 先减一把 1-indexed 转为 0-indexed，避免 `Z` 对应余数 0 的特殊分支。
**基础语法与思想：** `(char)('A' + k)` 把数字转字母；字符串从低位生成，最后反转。

### 169. 多数元素

**基础解法：** 哈希表计数，某个数次数超过 `n/2` 即返回。

**资深解法：** Boyer-Moore 投票法，候选数与其他数抵消，最后剩下的一定是多数元素。

```java
class Solution {
    public int majorityElement(int[] nums) {
        int candidate = 0, vote = 0;
        for (int x : nums) {
            if (vote == 0) candidate = x;
            vote += x == candidate ? 1 : -1;
        }
        return candidate;
    }
}
```

**基础语法与思想：** 三元表达式更新票数；题目保证多数元素存在，故无需二次验证。

### 170. 两数之和 III - 数据结构设计

**基础解法：** 保存所有数字，`find` 时双重循环找和，`add O(1)`、`find O(n^2)`。

**资深解法：** 哈希表保存数字频次；`find` 枚举一个数并检查补数是否存在。

```java
class TwoSum {
    private Map<Integer, Integer> count = new HashMap<>();

    public void add(int number) {
        count.put(number, count.getOrDefault(number, 0) + 1);
    }

    public boolean find(int value) {
        for (int x : count.keySet()) {
            int y = value - x;
            if (y != x && count.containsKey(y)) return true;
            if (y == x && count.get(x) > 1) return true;
        }
        return false;
    }
}
```

**基础语法与思想：** 频次表能处理两个相同数相加的情况；设计题要明确各操作的时间复杂度。

### 171. Excel 表列序号

**基础解法：** 从左到右按 26 进制累加。

```java
class Solution {
    public int titleToNumber(String columnTitle) {
        int ans = 0;
        for (int i = 0; i < columnTitle.length(); i++) {
            ans = ans * 26 + (columnTitle.charAt(i) - 'A' + 1);
        }
        return ans;
    }
}
```

**资深解法：** 与 168 互为逆过程；每读一个字符就把已有高位左移一位 26 进制。
**基础语法与思想：** 字符相减得到字母序号；时间 `O(n)`。

### 172. 阶乘后的零

**基础解法：** 计算阶乘再数末尾零不可行，数值会极快溢出。

**资深解法：** 末尾零由因子 `2 * 5` 产生，2 的数量远多于 5，只需统计 5 的因子数量。

```java
class Solution {
    public int trailingZeroes(int n) {
        int ans = 0;
        while (n > 0) {
            n /= 5;
            ans += n;
        }
        return ans;
    }
}
```

**基础语法与思想：** `25、125` 等贡献多个 5，所以循环不断除以 5。时间 `O(log n)`。

### 173. 二叉搜索树迭代器

**基础解法：** 中序遍历整棵树存入列表，`next` 按下标返回。

**资深解法：** 栈模拟中序遍历，只保存当前路径，均摊 `O(1)` 获取下一个最小值。

```java
class BSTIterator {
    private Deque<TreeNode> stack = new ArrayDeque<>();

    public BSTIterator(TreeNode root) {
        pushLeft(root);
    }

    public int next() {
        TreeNode node = stack.pop();
        pushLeft(node.right);
        return node.val;
    }

    public boolean hasNext() {
        return !stack.isEmpty();
    }

    private void pushLeft(TreeNode node) {
        while (node != null) {
            stack.push(node);
            node = node.left;
        }
    }
}
```

**基础语法与思想：** BST 中序遍历递增；把递归调用栈显式化，就是迭代器的核心。

### 174. 地下城游戏

**基础解法：** 从起点正向 DP 会同时受当前血量与最低血量约束，状态较难。

**资深解法：** 反向 DP：`dp[i][j]` 表示进入该格前至少需要多少生命值。

```java
class Solution {
    public int calculateMinimumHP(int[][] dungeon) {
        int m = dungeon.length, n = dungeon[0].length;
        int[][] dp = new int[m + 1][n + 1];
        for (int[] row : dp) Arrays.fill(row, Integer.MAX_VALUE / 2);
        dp[m][n - 1] = 1;
        dp[m - 1][n] = 1;
        for (int i = m - 1; i >= 0; i--) {
            for (int j = n - 1; j >= 0; j--) {
                int need = Math.min(dp[i + 1][j], dp[i][j + 1]) - dungeon[i][j];
                dp[i][j] = Math.max(1, need);
            }
        }
        return dp[0][0];
    }
}
```

**基础语法与思想：** 使用哨兵边界简化终点处理；生命值最低为 1，不能降到 0。

### 175. 组合两个表

**基础解法：** 左连接保留所有 Person，即使没有地址也返回 `null`。

```sql
SELECT p.firstName, p.lastName, a.city, a.state
FROM Person p
LEFT JOIN Address a ON p.personId = a.personId;
```

**资深解法：** 数据库题不使用 Java 提交；核心语法是 `LEFT JOIN ... ON ...`，它表达“主表全保留，匹配表尽量补充”。

### 176. 第二高的薪水

**基础解法：** 去重排序后取第二条；外层子查询保证没有第二高薪水时返回 `null`。

```sql
SELECT (
    SELECT DISTINCT salary
    FROM Employee
    ORDER BY salary DESC
    LIMIT 1 OFFSET 1
) AS SecondHighestSalary;
```

**资深解法：** `DISTINCT` 去掉重复薪水，`OFFSET 1` 跳过最高薪水；这是“排名后取第 N 个值”的基础模板。

### 177. 第 N 高的薪水

**基础解法：** 与 176 一样先去重降序，再跳过 `N - 1` 条。

```sql
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
    SET N = N - 1;
    RETURN (
        SELECT DISTINCT salary
        FROM Employee
        ORDER BY salary DESC
        LIMIT 1 OFFSET N
    );
END
```

**资深解法：** 窗口函数版本可用 `DENSE_RANK()` 计算薪水排名，再筛选第 `N` 名。
**基础语法与思想：** `LIMIT 1 OFFSET k` 取排序后的第 `k+1` 条；函数题要注意返回单个标量。

### 178. 分数排名

**基础解法：** 对每条分数统计有多少个不同分数比它高，排名为数量加一。

**资深解法：** 使用窗口函数 `DENSE_RANK`，并按题目要求把列名命名为 `rank`。

```sql
SELECT score, DENSE_RANK() OVER (ORDER BY score DESC) AS `rank`
FROM Scores
ORDER BY score DESC;
```

**基础语法与思想：** `DENSE_RANK` 不跳号，适合“并列后下一名连续”的排名定义。

### 179. 最大数

**基础解法：** 枚举排序规则较难直观；关键是比较两个数拼接后的大小。

**资深解法：** 把数字转字符串，按 `(b+a)` 与 `(a+b)` 的字典序降序排序。

```java
class Solution {
    public String largestNumber(int[] nums) {
        String[] arr = new String[nums.length];
        for (int i = 0; i < nums.length; i++) arr[i] = String.valueOf(nums[i]);
        Arrays.sort(arr, (a, b) -> (b + a).compareTo(a + b));
        if (arr[0].equals("0")) return "0";
        StringBuilder ans = new StringBuilder();
        for (String s : arr) ans.append(s);
        return ans.toString();
    }
}
```

**基础语法与思想：** `Arrays.sort` 可传 Lambda 比较器；若最大字符串是 `"0"`，说明所有数都是 0。

### 180. 连续出现的数字

**基础解法：** 自连接三次，找 `id` 连续且 `num` 相同的记录。

```sql
SELECT DISTINCT l1.num AS ConsecutiveNums
FROM Logs l1
JOIN Logs l2 ON l2.id = l1.id + 1
JOIN Logs l3 ON l3.id = l1.id + 2
WHERE l1.num = l2.num AND l2.num = l3.num;
```

**资深解法：** 支持窗口函数时可用 `LAG/LEAD` 比较前后行；自连接版本兼容性好。
**基础语法与思想：** SQL 的连续性依赖 `id + 1` 关系；`DISTINCT` 去掉重复命中的数字。
