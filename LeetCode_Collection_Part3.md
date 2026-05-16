## 61. 旋转链表 (Medium)

给你一个链表的头节点  `head`  ，旋转链表，将链表每个节点向右移动  `k`  个位置。
 
示例 1：

```text
输入：head = [1,2,3,4,5], k = 2
输出：[4,5,1,2,3]
```

示例 2：

```text
输入：head = [0,1,2], k = 4
输出：[2,0,1]
```

 
提示：

链表中节点的数目在范围  `[0, 500]`  内
 `-100 <= Node.val <= 100` 
 `0 <= k <= 2 * 109`

---

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

## 63. 不同路径 II (Medium)

给定一个  `m x n`  的整数数组  `grid` 。一个机器人初始位于 左上角（即  `grid[0][0]` ）。机器人尝试移动到 右下角（即  `grid[m - 1][n - 1]` ）。机器人每次只能向下或者向右移动一步。
网格中的障碍物和空位置分别用  `1`  和  `0`  来表示。机器人的移动路径中不能包含 任何 有障碍物的方格。
返回机器人能够到达右下角的不同路径数量。
测试用例保证答案小于等于  `2 * 109` 。
 
示例 1：

```text
输入：obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]
输出：2
解释：3x3 网格的正中间有一个障碍物。
从左上角到右下角一共有 2 条不同的路径：
1. 向右 -> 向右 -> 向下 -> 向下
2. 向下 -> 向下 -> 向右 -> 向右
```

示例 2：

```text
输入：obstacleGrid = [[0,1],[0,0]]
输出：1
```

 
提示：

 `m == obstacleGrid.length` 
 `n == obstacleGrid[i].length` 
 `1 <= m, n <= 100` 
 `obstacleGrid[i][j]`  为  `0`  或  `1`

---

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

## 65. 有效数字 (Hard)

给定一个字符串  `s`  ，返回  `s`  是否是一个 有效数字。
例如，下面的都是有效数字： `"2", "0089", "-0.1", "+3.14", "4.", "-.9", "2e10", "-90E3", "3e+7", "+6e-1", "53.5e93", "-123.456e789"` ，而接下来的不是： `"abc", "1a", "1e", "e3", "99e2.5", "--6", "-+3", "95a54e53"` 。
一般的，一个 有效数字 可以用以下的规则之一定义：

一个 整数 后面跟着一个 可选指数。
一个 十进制数 后面跟着一个 可选指数。

一个 整数 定义为一个 可选符号  `'-'`  或  `'+'`  后面跟着 数字。
一个 十进制数 定义为一个 可选符号  `'-'`  或  `'+'`  后面跟着下述规则：

数字 后跟着一个 小数点  `.` 。
数字 后跟着一个 小数点  `.`  再跟着 数位。
一个 小数点  `.`  后跟着 数位。

指数 定义为指数符号  `'e'`  或  `'E'` ，后面跟着一个 整数。
数字 定义为一个或多个数位。
 
示例 1：

输入：s = "0"
输出：true

示例 2：

输入：s = "e"
输出：false

示例 3：

输入：s = "."
输出：false

 
提示：

 `1 <= s.length <= 20` 
 `s`  仅含英文字母（大写和小写），数字（ `0-9` ），加号  `'+'`  ，减号  `'-'`  ，或者点  `'.'`  。

---

## 66. 加一 (Easy)

给定一个表示 大整数 的整数数组  `digits` ，其中  `digits[i]`  是整数的第  `i`  位数字。这些数字按从左到右，从最高位到最低位排列。这个大整数不包含任何前导  `0` 。
将大整数加 1，并返回结果的数字数组。
 
示例 1：

```text
输入：digits = [1,2,3]
输出：[1,2,4]
解释：输入数组表示数字 123。
加 1 后得到 123 + 1 = 124。
因此，结果应该是 [1,2,4]。
```

示例 2：

```text
输入：digits = [4,3,2,1]
输出：[4,3,2,2]
解释：输入数组表示数字 4321。
加 1 后得到 4321 + 1 = 4322。
因此，结果应该是 [4,3,2,2]。
```

示例 3：

```text
输入：digits = [9]
输出：[1,0]
解释：输入数组表示数字 9。
加 1 得到了 9 + 1 = 10。
因此，结果应该是 [1,0]。
```

 
提示：

 `1 <= digits.length <= 100` 
 `0 <= digits[i] <= 9` 
 `digits`  不包含任何前导  `0` 。

---

## 67. 二进制求和 (Easy)

给你两个二进制字符串  `a`  和  `b`  ，以二进制字符串的形式返回它们的和。
 
示例 1：

```text
输入:a = "11", b = "1"
输出："100"
```

示例 2：

```text
输入：a = "1010", b = "1011"
输出："10101"
```

 
提示：

 `1 <= a.length, b.length <= 104` 
 `a`  和  `b`  仅由字符  `'0'`  或  `'1'`  组成
字符串如果不是  `"0"`  ，就不含前导零

---

## 68. 文本左右对齐 (Hard)

给定一个单词数组  `words`  和一个长度  `maxWidth`  ，重新排版单词，使其成为每行恰好有  `maxWidth`  个字符，且左右两端对齐的文本。
你应该使用 “贪心算法” 来放置给定的单词；也就是说，尽可能多地往每行中放置单词。必要时可用空格  `' '`  填充，使得每行恰好有 maxWidth 个字符。
要求尽可能均匀分配单词间的空格数量。如果某一行单词间的空格不能均匀分配，则左侧放置的空格数要多于右侧的空格数。
文本的最后一行应为左对齐，且单词之间不插入额外的空格。
注意:

单词是指由非空格字符组成的字符序列。
每个单词的长度大于 0，小于等于 maxWidth。
输入单词数组  `words`  至少包含一个单词。

 
示例 1:

```text
输入: words = ["This", "is", "an", "example", "of", "text", "justification."], maxWidth = 16
输出:
[
   "This    is    an",
   "example  of text",
   "justification.  "
]
```

示例 2:

```text
输入:words = ["What","must","be","acknowledgment","shall","be"], maxWidth = 16
输出:
[
  "What   must   be",
  "acknowledgment  ",
  "shall be        "
]
解释: 注意最后一行的格式应为 "shall be    " 而不是 "shall     be",
     因为最后一行应为左对齐，而不是左右两端对齐。       
     第二行同样为左对齐，这是因为这行只包含一个单词。
```

示例 3:

```text
输入:words = ["Science","is","what","we","understand","well","enough","to","explain","to","a","computer.","Art","is","everything","else","we","do"]，maxWidth = 20
输出:
[
  "Science  is  what we",
  "understand      well",
  "enough to explain to",
  "a  computer.  Art is",
  "everything  else  we",
  "do                  "
]
```

 
提示:

 `1 <= words.length <= 300` 
 `1 <= words[i].length <= 20` 
 `words[i]`  由小写英文字母和符号组成
 `1 <= maxWidth <= 100` 
 `words[i].length <= maxWidth`

---

## 69. x 的平方根  (Easy)

给你一个非负整数  `x`  ，计算并返回  `x`  的 算术平方根 。
由于返回类型是整数，结果只保留 整数部分 ，小数部分将被 舍去 。
注意：不允许使用任何内置指数函数和算符，例如  `pow(x, 0.5)`  或者  `x ** 0.5`  。
 
示例 1：

```text
输入：x = 4
输出：2
```

示例 2：

```text
输入：x = 8
输出：2
解释：8 的算术平方根是 2.82842..., 由于返回类型是整数，小数部分将被舍去。
```

 
提示：

 `0 <= x <= 231 - 1`

---

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

## 71. 简化路径 (Medium)

给你一个字符串  `path`  ，表示指向某一文件或目录的 Unix 风格 绝对路径 （以  `'/'`  开头），请你将其转化为 更加简洁的规范路径。
在 Unix 风格的文件系统中规则如下：

一个点  `'.'`  表示当前目录本身。
此外，两个点  `'..'`  表示将目录切换到上一级（指向父目录）。
任意多个连续的斜杠（即， `'//'`  或  `'///'` ）都被视为单个斜杠  `'/'` 。
任何其他格式的点（例如， `'...'`  或  `'....'` ）均被视为有效的文件/目录名称。

返回的 简化路径 必须遵循下述格式：

始终以斜杠  `'/'`  开头。
两个目录名之间必须只有一个斜杠  `'/'`  。
最后一个目录名（如果存在）不能 以  `'/'`  结尾。
此外，路径仅包含从根目录到目标文件或目录的路径上的目录（即，不含  `'.'`  或  `'..'` ）。

返回简化后得到的 规范路径 。
 
示例 1：

输入：path = "/home/"
输出："/home"
解释：
应删除尾随斜杠。

示例 2：

输入：path = "/home//foo/"
输出："/home/foo"
解释：
多个连续的斜杠被单个斜杠替换。

示例 3：

输入：path = "/home/user/Documents/../Pictures"
输出："/home/user/Pictures"
解释：
两个点  `".."`  表示上一级目录（父目录）。

示例 4：

输入：path = "/../"
输出："/"
解释：
不可能从根目录上升一级目录。

示例 5：

输入：path = "/.../a/../b/c/../d/./"
输出："/.../b/d"
解释：
 `"..."`  在这个问题中是一个合法的目录名。

 
提示：

 `1 <= path.length <= 3000` 
 `path`  由英文字母，数字， `'.'` ， `'/'`  或  `'_'`  组成。
 `path`  是一个有效的 Unix 风格绝对路径。

---

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

## 77. 组合 (Medium)

给定两个整数  `n`  和  `k` ，返回范围  `[1, n]`  中所有可能的  `k`  个数的组合。
你可以按 任何顺序 返回答案。
 
示例 1：

```text
输入：n = 4, k = 2
输出：
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```

示例 2：

```text
输入：n = 1, k = 1
输出：[[1]]
```

 
提示：

 `1 <= n <= 20` 
 `1 <= k <= n`

---

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

## 80. 删除有序数组中的重复项 II (Medium)

给你一个有序数组  `nums`  ，请你 原地 删除重复出现的元素，使得出现次数超过两次的元素只出现两次 ，返回删除后数组的新长度。
不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。
 
说明：
为什么返回数值是整数，但输出的答案是数组呢？
请注意，输入数组是以「引用」方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。
你可以想象内部操作如下:

```text
// nums 是以“引用”方式传递的。也就是说，不对实参做任何拷贝
int len = removeDuplicates(nums);

// 在函数里修改输入数组对于调用者是可见的。
// 根据你的函数返回的长度, 它会打印出数组中 该长度范围内 的所有元素。
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

 
示例 1：

```text
输入：nums = [1,1,1,2,2,3]
输出：5, nums = [1,1,2,2,3]
解释：函数应返回新长度 length = 5, 并且原数组的前五个元素被修改为 1, 1, 2, 2, 3。 不需要考虑数组中超出新长度后面的元素。
```

示例 2：

```text
输入：nums = [0,0,1,1,1,1,2,3,3]
输出：7, nums = [0,0,1,1,2,3,3]
解释：函数应返回新长度 length = 7, 并且原数组的前七个元素被修改为 0, 0, 1, 1, 2, 3, 3。不需要考虑数组中超出新长度后面的元素。
```

 
提示：

 `1 <= nums.length <= 3 * 104` 
 `-104 <= nums[i] <= 104` 
 `nums`  已按升序排列

---

## 81. 搜索旋转排序数组 II (Medium)

已知存在一个按非降序排列的整数数组  `nums`  ，数组中的值不必互不相同。
在传递给函数之前， `nums`  在预先未知的某个下标  `k` （ `0 <= k < nums.length` ）上进行了 旋转 ，使数组变为  `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` （下标 从 0 开始 计数）。例如，  `[0,1,2,4,4,4,5,6,6,7]`  在下标  `5`  处经旋转后可能变为  `[4,5,6,6,7,0,1,2,4,4]`  。
给你 旋转后 的数组  `nums`  和一个整数  `target`  ，请你编写一个函数来判断给定的目标值是否存在于数组中。如果  `nums`  中存在这个目标值  `target`  ，则返回  `true`  ，否则返回  `false`  。
你必须尽可能减少整个操作步骤。
 
示例 1：

```text
输入：nums = [2,5,6,0,0,1,2], target = 0
输出：true
```

示例 2：

```text
输入：nums = [2,5,6,0,0,1,2], target = 3
输出：false
```

 
提示：

 `1 <= nums.length <= 5000` 
 `-104 <= nums[i] <= 104` 
题目数据保证  `nums`  在预先未知的某个下标上进行了旋转
 `-104 <= target <= 104` 

 
进阶：

此题与 搜索旋转排序数组 相似，但本题中的  `nums`   可能包含 重复 元素。这会影响到程序的时间复杂度吗？会有怎样的影响，为什么？

---

## 82. 删除排序链表中的重复元素 II (Medium)

给定一个已排序的链表的头  `head`  ， 删除原始链表中所有重复数字的节点，只留下不同的数字 。返回 已排序的链表 。
 
示例 1：

```text
输入：head = [1,2,3,3,4,4,5]
输出：[1,2,5]
```

示例 2：

```text
输入：head = [1,1,1,2,3]
输出：[2,3]
```

 
提示：

链表中节点数目在范围  `[0, 300]`  内
 `-100 <= Node.val <= 100` 
题目数据保证链表已经按升序 排列

---

## 83. 删除排序链表中的重复元素 (Easy)

给定一个已排序的链表的头  `head`  ， 删除所有重复的元素，使每个元素只出现一次 。返回 已排序的链表 。
 
示例 1：

```text
输入：head = [1,1,2]
输出：[1,2]
```

示例 2：

```text
输入：head = [1,1,2,3,3]
输出：[1,2,3]
```

 
提示：

链表中节点数目在范围  `[0, 300]`  内
 `-100 <= Node.val <= 100` 
题目数据保证链表已经按升序 排列

---

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

## 85. 最大矩形 (Hard)

给定一个仅包含  `0`  和  `1`  、大小为  `rows x cols`  的二维二进制矩阵，找出只包含  `1`  的最大矩形，并返回其面积。
 
示例 1：

```text
输入：matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]
输出：6
解释：最大矩形如上图所示。
```

示例 2：

```text
输入：matrix = [["0"]]
输出：0
```

示例 3：

```text
输入：matrix = [["1"]]
输出：1
```

 
提示：

 `rows == matrix.length` 
 `cols == matrix[0].length` 
 `1 <= rows, cols <= 200` 
 `matrix[i][j]`  为  `'0'`  或  `'1'`

---

## 86. 分隔链表 (Medium)

给你一个链表的头节点  `head`  和一个特定值  `x`  ，请你对链表进行分隔，使得所有 小于  `x`  的节点都出现在 大于或等于  `x`  的节点之前。
你应当 保留 两个分区中每个节点的初始相对位置。
 
示例 1：

```text
输入：head = [1,4,3,2,5,2], x = 3
输出：[1,2,2,4,3,5]
```

示例 2：

```text
输入：head = [2,1], x = 2
输出：[1,2]
```

 
提示：

链表中节点的数目在范围  `[0, 200]`  内
 `-100 <= Node.val <= 100` 
 `-200 <= x <= 200`

---

## 87. 扰乱字符串 (Hard)

使用下面描述的算法可以扰乱字符串  `s`  得到字符串  `t`  ：

如果字符串的长度为 1 ，算法停止
如果字符串的长度 > 1 ，执行下述步骤：
	
在一个随机下标处将字符串分割成两个非空的子字符串。即，如果已知字符串  `s`  ，则可以将其分成两个子字符串  `x`  和  `y`  ，且满足  `s = x + y`  。
随机 决定是要「交换两个子字符串」还是要「保持这两个子字符串的顺序不变」。即，在执行这一步骤之后， `s`  可能是  `s = x + y`  或者  `s = y + x`  。
在  `x`  和  `y`  这两个子字符串上继续从步骤 1 开始递归执行此算法。

给你两个 长度相等 的字符串  `s1`  和  `s2` ，判断  `s2`  是否是  `s1`  的扰乱字符串。如果是，返回  `true`  ；否则，返回  `false`  。
 
示例 1：

```text
输入：s1 = "great", s2 = "rgeat"
输出：true
解释：s1 上可能发生的一种情形是：
"great" --> "gr/eat" // 在一个随机下标处分割得到两个子字符串
"gr/eat" --> "gr/eat" // 随机决定：「保持这两个子字符串的顺序不变」
"gr/eat" --> "g/r / e/at" // 在子字符串上递归执行此算法。两个子字符串分别在随机下标处进行一轮分割
"g/r / e/at" --> "r/g / e/at" // 随机决定：第一组「交换两个子字符串」，第二组「保持这两个子字符串的顺序不变」
"r/g / e/at" --> "r/g / e/ a/t" // 继续递归执行此算法，将 "at" 分割得到 "a/t"
"r/g / e/ a/t" --> "r/g / e/ a/t" // 随机决定：「保持这两个子字符串的顺序不变」
算法终止，结果字符串和 s2 相同，都是 "rgeat"
这是一种能够扰乱 s1 得到 s2 的情形，可以认为 s2 是 s1 的扰乱字符串，返回 true
```

示例 2：

```text
输入：s1 = "abcde", s2 = "caebd"
输出：false
```

示例 3：

```text
输入：s1 = "a", s2 = "a"
输出：true
```

 
提示：

 `s1.length == s2.length` 
 `1 <= s1.length <= 30` 
 `s1`  和  `s2`  由小写英文字母组成

---

## 88. 合并两个有序数组 (Easy)

给你两个按 非递减顺序 排列的整数数组  `nums1`  和  `nums2` ，另有两个整数  `m`  和  `n`  ，分别表示  `nums1`  和  `nums2`  中的元素数目。
请你 合并  `nums2`  到  `nums1`  中，使合并后的数组同样按 非递减顺序 排列。
注意：最终，合并后数组不应由函数返回，而是存储在数组  `nums1`  中。为了应对这种情况， `nums1`  的初始长度为  `m + n` ，其中前  `m`  个元素表示应合并的元素，后  `n`  个元素为  `0`  ，应忽略。 `nums2`  的长度为  `n`  。
 
示例 1：

```text
输入：nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
输出：[1,2,2,3,5,6]
解释：需要合并 [1,2,3] 和 [2,5,6] 。
合并结果是 [1,2,2,3,5,6] ，其中斜体加粗标注的为 nums1 中的元素。
```

示例 2：

```text
输入：nums1 = [1], m = 1, nums2 = [], n = 0
输出：[1]
解释：需要合并 [1] 和 [] 。
合并结果是 [1] 。
```

示例 3：

```text
输入：nums1 = [0], m = 0, nums2 = [1], n = 1
输出：[1]
解释：需要合并的数组是 [] 和 [1] 。
合并结果是 [1] 。
注意，因为 m = 0 ，所以 nums1 中没有元素。nums1 中仅存的 0 仅仅是为了确保合并结果可以顺利存放到 nums1 中。
```

 
提示：

 `nums1.length == m + n` 
 `nums2.length == n` 
 `0 <= m, n <= 200` 
 `1 <= m + n <= 200` 
 `-109 <= nums1[i], nums2[j] <= 109` 

 
进阶：你可以设计实现一个时间复杂度为  `O(m + n)`  的算法解决此问题吗？

---

## 89. 格雷编码 (Medium)

n 位格雷码序列 是一个由  `2n`  个整数组成的序列，其中：

每个整数都在范围  `[0, 2n - 1]`  内（含  `0`  和  `2n - 1` ）
第一个整数是  `0` 
一个整数在序列中出现 不超过一次
每对 相邻 整数的二进制表示 恰好一位不同 ，且
第一个 和 最后一个 整数的二进制表示 恰好一位不同

给你一个整数  `n`  ，返回任一有效的 n 位格雷码序列 。
 
示例 1：

```text
输入：n = 2
输出：[0,1,3,2]
解释：
[0,1,3,2] 的二进制表示是 [00,01,11,10] 。
- 00 和 01 有一位不同
- 01 和 11 有一位不同
- 11 和 10 有一位不同
- 10 和 00 有一位不同
[0,2,3,1] 也是一个有效的格雷码序列，其二进制表示是 [00,10,11,01] 。
- 00 和 10 有一位不同
- 10 和 11 有一位不同
- 11 和 01 有一位不同
- 01 和 00 有一位不同
```

示例 2：

```text
输入：n = 1
输出：[0,1]
```

 
提示：

 `1 <= n <= 16`

---

## 90. 子集 II (Medium)

给你一个整数数组  `nums`  ，其中可能包含重复元素，请你返回该数组所有可能的 子集（幂集）。
解集 不能 包含重复的子集。返回的解集中，子集可以按 任意顺序 排列。

 
示例 1：

```text
输入：nums = [1,2,2]
输出：[[],[1],[1,2],[1,2,2],[2],[2,2]]
```

示例 2：

```text
输入：nums = [0]
输出：[[],[0]]
```

 
提示：

 `1 <= nums.length <= 10` 
 `-10 <= nums[i] <= 10`

---

# Java 解法补充附录（61-90）

### 61. 旋转链表

基础解法：把链表节点放进数组，按 `(i + k) % n` 重连；时间 `O(n)`，空间 `O(n)`。
资深解法：先求长度，把链表连成环，再在 `n - k % n` 处断开。

```java
class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        if (head == null || head.next == null || k == 0) return head;
        int n = 1;
        ListNode tail = head;
        while (tail.next != null) {
            tail = tail.next;
            n++;
        }
        tail.next = head;
        int steps = n - k % n;
        while (steps-- > 0) tail = tail.next;
        ListNode ans = tail.next;
        tail.next = null;
        return ans;
    }
}
```

基础语法与思想：链表成环要记得断开；`k % n` 消除无效旋转。

### 62. 不同路径

基础解法：二维 DP，`dp[r][c] = dp[r-1][c] + dp[r][c-1]`。
资深解法：一维滚动数组，当前格只依赖左侧和上方。

```java
class Solution {
    public int uniquePaths(int m, int n) {
        int[] dp = new int[n];
        java.util.Arrays.fill(dp, 1);
        for (int r = 1; r < m; r++) {
            for (int c = 1; c < n; c++) dp[c] += dp[c - 1];
        }
        return dp[n - 1];
    }
}
```

基础语法与思想：`Arrays.fill` 初始化数组；路径计数常用“到达当前格的方法数”。

### 63. 不同路径 II

基础解法：二维 DP，遇到障碍格置 0。
资深解法：一维 DP，障碍清零，否则累加左侧。

```java
class Solution {
    public int uniquePathsWithObstacles(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int[] dp = new int[n];
        dp[0] = grid[0][0] == 1 ? 0 : 1;
        for (int r = 0; r < m; r++) {
            for (int c = 0; c < n; c++) {
                if (grid[r][c] == 1) dp[c] = 0;
                else if (c > 0) dp[c] += dp[c - 1];
            }
        }
        return dp[n - 1];
    }
}
```

基础语法与思想：障碍格不能穿过，DP 状态直接归零。

### 64. 最小路径和

基础解法：二维 DP 保存到每个格子的最小路径和。
资深解法：直接在原矩阵上滚动更新，省空间。

```java
class Solution {
    public int minPathSum(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        for (int r = 0; r < m; r++) {
            for (int c = 0; c < n; c++) {
                if (r == 0 && c == 0) continue;
                int up = r == 0 ? Integer.MAX_VALUE : grid[r - 1][c];
                int left = c == 0 ? Integer.MAX_VALUE : grid[r][c - 1];
                grid[r][c] += Math.min(up, left);
            }
        }
        return grid[m - 1][n - 1];
    }
}
```

基础语法与思想：用哨兵值处理边界；最短路径 DP 取前驱最小值。

### 65. 有效数字

基础解法：用 `Double.parseDouble` 尝试解析，但要注意它接受部分 LeetCode 不允许的格式，面试中只作辅助理解。
资深解法：按字符扫描，分别记录数字、小数点、指数、符号是否合法。

```java
class Solution {
    public boolean isNumber(String s) {
        boolean seenDigit = false, seenDot = false, seenExp = false;
        boolean digitAfterExp = true;
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (Character.isDigit(c)) {
                seenDigit = true;
                if (seenExp) digitAfterExp = true;
            } else if (c == '+' || c == '-') {
                if (i > 0 && s.charAt(i - 1) != 'e' && s.charAt(i - 1) != 'E') return false;
            } else if (c == '.') {
                if (seenDot || seenExp) return false;
                seenDot = true;
            } else if (c == 'e' || c == 'E') {
                if (seenExp || !seenDigit) return false;
                seenExp = true;
                digitAfterExp = false;
            } else {
                return false;
            }
        }
        return seenDigit && digitAfterExp;
    }
}
```

基础语法与思想：`Character.isDigit` 判断数字；解析题适合维护状态标记。

### 66. 加一

基础解法：转成数字再加一会溢出，不推荐。
资深解法：从末位处理进位，遇到非 9 直接加一返回。

```java
class Solution {
    public int[] plusOne(int[] digits) {
        for (int i = digits.length - 1; i >= 0; i--) {
            if (digits[i] != 9) {
                digits[i]++;
                return digits;
            }
            digits[i] = 0;
        }
        int[] ans = new int[digits.length + 1];
        ans[0] = 1;
        return ans;
    }
}
```

基础语法与思想：数组长度固定，需要扩容时创建新数组。

### 67. 二进制求和

基础解法：从右向左模拟加法。
资深解法：统一用 `carry` 同时处理两个字符串和进位。

```java
class Solution {
    public String addBinary(String a, String b) {
        StringBuilder sb = new StringBuilder();
        int i = a.length() - 1, j = b.length() - 1, carry = 0;
        while (i >= 0 || j >= 0 || carry != 0) {
            int sum = carry;
            if (i >= 0) sum += a.charAt(i--) - '0';
            if (j >= 0) sum += b.charAt(j--) - '0';
            sb.append(sum % 2);
            carry = sum / 2;
        }
        return sb.reverse().toString();
    }
}
```

基础语法与思想：字符转数字用 `c - '0'`；低位到高位构造后反转。

### 68. 文本左右对齐

基础解法：逐行贪心装入尽可能多单词，再补空格。
资深解法：普通行平均分配空格，最后一行左对齐。

```java
class Solution {
    public java.util.List<String> fullJustify(String[] words, int maxWidth) {
        java.util.List<String> ans = new java.util.ArrayList<>();
        for (int i = 0; i < words.length;) {
            int j = i, len = 0;
            while (j < words.length && len + words[j].length() + (j - i) <= maxWidth) len += words[j++].length();
            int gaps = j - i - 1;
            StringBuilder line = new StringBuilder();
            if (j == words.length || gaps == 0) {
                for (int k = i; k < j; k++) {
                    if (k > i) line.append(' ');
                    line.append(words[k]);
                }
                while (line.length() < maxWidth) line.append(' ');
            } else {
                int spaces = maxWidth - len;
                int each = spaces / gaps, extra = spaces % gaps;
                for (int k = i; k < j; k++) {
                    line.append(words[k]);
                    if (k < j - 1) {
                        for (int s = 0; s < each + (k - i < extra ? 1 : 0); s++) line.append(' ');
                    }
                }
            }
            ans.add(line.toString());
            i = j;
        }
        return ans;
    }
}
```

基础语法与思想：`StringBuilder` 补空格；贪心按行分组。

### 69. x 的平方根

基础解法：从 1 开始线性试探。
资深解法：二分答案，用 `long` 防止平方溢出。

```java
class Solution {
    public int mySqrt(int x) {
        int left = 0, right = x, ans = 0;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if ((long) mid * mid <= x) {
                ans = mid;
                left = mid + 1;
            } else right = mid - 1;
        }
        return ans;
    }
}
```

基础语法与思想：二分寻找“最后一个平方不超过 x 的数”。

### 70. 爬楼梯

基础解法：递归 `f(n)=f(n-1)+f(n-2)`。
资深解法：滚动变量迭代 Fibonacci。

```java
class Solution {
    public int climbStairs(int n) {
        int a = 1, b = 1;
        for (int i = 2; i <= n; i++) {
            int c = a + b;
            a = b;
            b = c;
        }
        return b;
    }
}
```

基础语法与思想：只依赖前两项时可用滚动变量压缩空间。

### 71. 简化路径

基础解法：按 `/` 分割后用列表模拟进入目录和返回上级。
资深解法：用栈处理目录名，跳过空串和 `.`。

```java
class Solution {
    public String simplifyPath(String path) {
        java.util.Deque<String> stack = new java.util.ArrayDeque<>();
        for (String part : path.split("/")) {
            if (part.equals("") || part.equals(".")) continue;
            if (part.equals("..")) {
                if (!stack.isEmpty()) stack.pollLast();
            } else stack.offerLast(part);
        }
        StringBuilder ans = new StringBuilder();
        for (String dir : stack) ans.append('/').append(dir);
        return ans.length() == 0 ? "/" : ans.toString();
    }
}
```

基础语法与思想：`split("/")` 分割路径；栈适合处理返回上级目录。

### 72. 编辑距离

基础解法：递归尝试插入、删除、替换并加记忆化。
资深解法：二维 DP，`dp[i][j]` 表示前 `i` 个字符变成前 `j` 个字符的最小操作数。

```java
class Solution {
    public int minDistance(String word1, String word2) {
        int m = word1.length(), n = word2.length();
        int[][] dp = new int[m + 1][n + 1];
        for (int i = 0; i <= m; i++) dp[i][0] = i;
        for (int j = 0; j <= n; j++) dp[0][j] = j;
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (word1.charAt(i - 1) == word2.charAt(j - 1)) dp[i][j] = dp[i - 1][j - 1];
                else dp[i][j] = 1 + Math.min(dp[i - 1][j - 1], Math.min(dp[i - 1][j], dp[i][j - 1]));
            }
        }
        return dp[m][n];
    }
}
```

基础语法与思想：二维 DP 初始化空串边界；三种操作对应三个前驱状态。

### 73. 矩阵置零

基础解法：用两个集合记录需要清零的行和列。
资深解法：用第一行和第一列作为标记数组，额外记录首列是否清零。

```java
class Solution {
    public void setZeroes(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        boolean firstColZero = false;
        for (int r = 0; r < m; r++) {
            if (matrix[r][0] == 0) firstColZero = true;
            for (int c = 1; c < n; c++) {
                if (matrix[r][c] == 0) matrix[r][0] = matrix[0][c] = 0;
            }
        }
        for (int r = m - 1; r >= 0; r--) {
            for (int c = n - 1; c >= 1; c--) if (matrix[r][0] == 0 || matrix[0][c] == 0) matrix[r][c] = 0;
            if (firstColZero) matrix[r][0] = 0;
        }
    }
}
```

基础语法与思想：原地标记要注意第一行/列会被复用。

### 74. 搜索二维矩阵

基础解法：逐行二分或直接遍历。
资深解法：把矩阵看作长度 `m*n` 的有序数组二分。

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int m = matrix.length, n = matrix[0].length;
        int left = 0, right = m * n - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            int value = matrix[mid / n][mid % n];
            if (value == target) return true;
            if (value < target) left = mid + 1;
            else right = mid - 1;
        }
        return false;
    }
}
```

基础语法与思想：一维下标映射二维坐标：`row = idx / n`，`col = idx % n`。

### 75. 颜色分类

基础解法：计数 0、1、2 后重写数组。
资深解法：荷兰国旗三指针，`zero` 左侧全 0，`two` 右侧全 2。

```java
class Solution {
    public void sortColors(int[] nums) {
        int zero = 0, i = 0, two = nums.length - 1;
        while (i <= two) {
            if (nums[i] == 0) swap(nums, zero++, i++);
            else if (nums[i] == 2) swap(nums, i, two--);
            else i++;
        }
    }
    private void swap(int[] a, int i, int j) {
        int t = a[i]; a[i] = a[j]; a[j] = t;
    }
}
```

基础语法与思想：交换 2 后 `i` 不前进，因为换来的元素还未检查。

### 76. 最小覆盖子串

基础解法：枚举所有子串并统计是否覆盖。
资深解法：滑动窗口维护所需字符计数，满足覆盖后尽量收缩左边界。

```java
class Solution {
    public String minWindow(String s, String t) {
        int[] need = new int[128];
        for (int i = 0; i < t.length(); i++) need[t.charAt(i)]++;
        int missing = t.length(), left = 0, bestLen = Integer.MAX_VALUE, bestStart = 0;
        for (int right = 0; right < s.length(); right++) {
            if (need[s.charAt(right)]-- > 0) missing--;
            while (missing == 0) {
                if (right - left + 1 < bestLen) {
                    bestLen = right - left + 1;
                    bestStart = left;
                }
                if (++need[s.charAt(left++)] > 0) missing++;
            }
        }
        return bestLen == Integer.MAX_VALUE ? "" : s.substring(bestStart, bestStart + bestLen);
    }
}
```

基础语法与思想：数组可当字符频次表；窗口题常用“扩右、缩左”。

### 77. 组合

基础解法：枚举所有子集并筛选大小为 `k`。
资深解法：回溯从 `start` 起选择下一个数，并用剩余数量剪枝。

```java
class Solution {
    public java.util.List<java.util.List<Integer>> combine(int n, int k) {
        java.util.List<java.util.List<Integer>> ans = new java.util.ArrayList<>();
        dfs(1, n, k, new java.util.ArrayList<>(), ans);
        return ans;
    }
    private void dfs(int start, int n, int k, java.util.List<Integer> path, java.util.List<java.util.List<Integer>> ans) {
        if (path.size() == k) { ans.add(new java.util.ArrayList<>(path)); return; }
        for (int x = start; x <= n - (k - path.size()) + 1; x++) {
            path.add(x); dfs(x + 1, n, k, path, ans); path.remove(path.size() - 1);
        }
    }
}
```

基础语法与思想：`start` 防止组合重复；剪枝减少无效分支。

### 78. 子集

基础解法：每个元素选或不选。
资深解法：迭代扩展已有子集。

```java
class Solution {
    public java.util.List<java.util.List<Integer>> subsets(int[] nums) {
        java.util.List<java.util.List<Integer>> ans = new java.util.ArrayList<>();
        ans.add(new java.util.ArrayList<>());
        for (int num : nums) {
            int size = ans.size();
            for (int i = 0; i < size; i++) {
                java.util.List<Integer> next = new java.util.ArrayList<>(ans.get(i));
                next.add(num);
                ans.add(next);
            }
        }
        return ans;
    }
}
```

基础语法与思想：复制列表再追加，避免修改原子集。

### 79. 单词搜索

基础解法：从每个格子开始 DFS 搜索单词。
资深解法：原地标记访问过的格子，回溯时恢复。

```java
class Solution {
    public boolean exist(char[][] board, String word) {
        for (int r = 0; r < board.length; r++)
            for (int c = 0; c < board[0].length; c++)
                if (dfs(board, word, r, c, 0)) return true;
        return false;
    }
    private boolean dfs(char[][] b, String w, int r, int c, int i) {
        if (i == w.length()) return true;
        if (r < 0 || r == b.length || c < 0 || c == b[0].length || b[r][c] != w.charAt(i)) return false;
        char old = b[r][c];
        b[r][c] = '#';
        boolean ok = dfs(b, w, r + 1, c, i + 1) || dfs(b, w, r - 1, c, i + 1) ||
                dfs(b, w, r, c + 1, i + 1) || dfs(b, w, r, c - 1, i + 1);
        b[r][c] = old;
        return ok;
    }
}
```

基础语法与思想：DFS 越界判断要放在访问数组前；回溯恢复现场。

### 80. 删除有序数组中的重复项 II

基础解法：计数每个值出现次数，最多写两次。
资深解法：慢指针，若 `nums[fast] != nums[slow - 2]` 才写入。

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int slow = 0;
        for (int num : nums) {
            if (slow < 2 || num != nums[slow - 2]) nums[slow++] = num;
        }
        return slow;
    }
}
```

基础语法与思想：允许最多两次时，只需比较新元素和有效区倒数第二个元素。

### 81. 搜索旋转排序数组 II

基础解法：线性扫描。
资深解法：带重复值的二分；当 `left/mid/right` 无法判断有序边时收缩边界。

```java
class Solution {
    public boolean search(int[] nums, int target) {
        int left = 0, right = nums.length - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] == target) return true;
            if (nums[left] == nums[mid] && nums[mid] == nums[right]) { left++; right--; }
            else if (nums[left] <= nums[mid]) {
                if (nums[left] <= target && target < nums[mid]) right = mid - 1; else left = mid + 1;
            } else {
                if (nums[mid] < target && target <= nums[right]) left = mid + 1; else right = mid - 1;
            }
        }
        return false;
    }
}
```

基础语法与思想：重复值会破坏“哪边有序”的明确判断，需要退化收缩。

### 82. 删除排序链表中的重复元素 II

基础解法：用哈希表统计值出现次数，再重建只出现一次的节点。
资深解法：虚拟头结点，遇到重复段就整段跳过。

```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        ListNode dummy = new ListNode(0, head);
        ListNode prev = dummy;
        while (head != null) {
            if (head.next != null && head.val == head.next.val) {
                int val = head.val;
                while (head != null && head.val == val) head = head.next;
                prev.next = head;
            } else {
                prev = head;
                head = head.next;
            }
        }
        return dummy.next;
    }
}
```

基础语法与思想：删除头节点可能变化时使用 `dummy`。

### 83. 删除排序链表中的重复元素

基础解法：用集合记录已出现值并删除重复节点。
资深解法：排序链表重复值相邻，直接跳过相同的 `next`。

```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        ListNode cur = head;
        while (cur != null && cur.next != null) {
            if (cur.val == cur.next.val) cur.next = cur.next.next;
            else cur = cur.next;
        }
        return head;
    }
}
```

基础语法与思想：排序条件让重复检测只需比较相邻节点。

### 84. 柱状图中最大的矩形

基础解法：枚举每个柱子向左右扩展，找不低于它的最大宽度。
资深解法：单调递增栈，遇到更矮柱子时结算栈顶柱子的最大矩形。

```java
class Solution {
    public int largestRectangleArea(int[] heights) {
        int n = heights.length, ans = 0;
        java.util.Deque<Integer> stack = new java.util.ArrayDeque<>();
        for (int i = 0; i <= n; i++) {
            int h = i == n ? 0 : heights[i];
            while (!stack.isEmpty() && heights[stack.peek()] > h) {
                int height = heights[stack.pop()];
                int left = stack.isEmpty() ? -1 : stack.peek();
                ans = Math.max(ans, height * (i - left - 1));
            }
            stack.push(i);
        }
        return ans;
    }
}
```

基础语法与思想：栈里存下标；哨兵高度 0 触发最后结算。

### 85. 最大矩形

基础解法：枚举上下边界后按列判断连续 1。
资深解法：把每一行作为柱状图底部，累积高度后复用第 84 题单调栈。

```java
class Solution {
    public int maximalRectangle(char[][] matrix) {
        int n = matrix[0].length, ans = 0;
        int[] heights = new int[n];
        for (char[] row : matrix) {
            for (int c = 0; c < n; c++) heights[c] = row[c] == '1' ? heights[c] + 1 : 0;
            ans = Math.max(ans, largest(heights));
        }
        return ans;
    }
    private int largest(int[] h) {
        java.util.Deque<Integer> st = new java.util.ArrayDeque<>();
        int ans = 0;
        for (int i = 0; i <= h.length; i++) {
            int cur = i == h.length ? 0 : h[i];
            while (!st.isEmpty() && h[st.peek()] > cur) {
                int height = h[st.pop()];
                int left = st.isEmpty() ? -1 : st.peek();
                ans = Math.max(ans, height * (i - left - 1));
            }
            st.push(i);
        }
        return ans;
    }
}
```

基础语法与思想：二维矩阵可逐行转化为一维柱状图问题。

### 86. 分隔链表

基础解法：把小于 `x` 和大于等于 `x` 的值分别收集后重建链表。
资深解法：两个虚拟头链表分别接小节点和大节点，最后拼接。

```java
class Solution {
    public ListNode partition(ListNode head, int x) {
        ListNode smallDummy = new ListNode(0), bigDummy = new ListNode(0);
        ListNode small = smallDummy, big = bigDummy;
        while (head != null) {
            if (head.val < x) { small.next = head; small = small.next; }
            else { big.next = head; big = big.next; }
            head = head.next;
        }
        big.next = null;
        small.next = bigDummy.next;
        return smallDummy.next;
    }
}
```

基础语法与思想：拆分链表后要把尾节点 `next` 置空，避免形成旧链。

### 87. 扰乱字符串

基础解法：递归枚举切分点和交换/不交换两种情况。
资深解法：记忆化递归，并先用字符频次剪枝。

```java
class Solution {
    private final java.util.Map<String, Boolean> memo = new java.util.HashMap<>();
    public boolean isScramble(String s1, String s2) {
        String key = s1 + "#" + s2;
        if (memo.containsKey(key)) return memo.get(key);
        if (s1.equals(s2)) return true;
        int[] cnt = new int[26];
        for (int i = 0; i < s1.length(); i++) { cnt[s1.charAt(i)-'a']++; cnt[s2.charAt(i)-'a']--; }
        for (int x : cnt) if (x != 0) { memo.put(key, false); return false; }
        int n = s1.length();
        for (int i = 1; i < n; i++) {
            if (isScramble(s1.substring(0,i), s2.substring(0,i)) && isScramble(s1.substring(i), s2.substring(i)) ||
                isScramble(s1.substring(0,i), s2.substring(n-i)) && isScramble(s1.substring(i), s2.substring(0,n-i))) {
                memo.put(key, true); return true;
            }
        }
        memo.put(key, false);
        return false;
    }
}
```

基础语法与思想：`Map<String, Boolean>` 缓存递归状态；递归题先找状态键。

### 88. 合并两个有序数组

基础解法：拷贝后从头归并。
资深解法：从后往前填充，避免覆盖 `nums1` 未处理元素。

```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int i = m - 1, j = n - 1, k = m + n - 1;
        while (j >= 0) {
            if (i >= 0 && nums1[i] > nums2[j]) nums1[k--] = nums1[i--];
            else nums1[k--] = nums2[j--];
        }
    }
}
```

基础语法与思想：从后写入是原地归并的关键。

### 89. 格雷编码

基础解法：回溯找相邻只差一位的序列。
资深解法：公式 `i ^ (i >> 1)` 直接生成第 `i` 个格雷码。

```java
class Solution {
    public java.util.List<Integer> grayCode(int n) {
        java.util.List<Integer> ans = new java.util.ArrayList<>();
        for (int i = 0; i < (1 << n); i++) ans.add(i ^ (i >> 1));
        return ans;
    }
}
```

基础语法与思想：`^` 是异或；右移一位后异或能构造相邻一位差。

### 90. 子集 II

基础解法：生成全部子集后用集合去重。
资深解法：排序后回溯，同一层跳过重复元素。

```java
class Solution {
    public java.util.List<java.util.List<Integer>> subsetsWithDup(int[] nums) {
        java.util.Arrays.sort(nums);
        java.util.List<java.util.List<Integer>> ans = new java.util.ArrayList<>();
        dfs(nums, 0, new java.util.ArrayList<>(), ans);
        return ans;
    }
    private void dfs(int[] nums, int start, java.util.List<Integer> path, java.util.List<java.util.List<Integer>> ans) {
        ans.add(new java.util.ArrayList<>(path));
        for (int i = start; i < nums.length; i++) {
            if (i > start && nums[i] == nums[i - 1]) continue;
            path.add(nums[i]); dfs(nums, i + 1, path, ans); path.remove(path.size() - 1);
        }
    }
}
```

基础语法与思想：子集题每个递归节点都是一个答案；排序后同层去重。
