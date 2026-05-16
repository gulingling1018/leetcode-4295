# LeetCode 题目合集 Part 8

## 211. 添加与搜索单词 - 数据结构设计 (Medium)

请你设计一个数据结构，支持 添加新单词 和 查找字符串是否与任何先前添加的字符串匹配 。
实现词典类  `WordDictionary`  ：

 `WordDictionary()`  初始化词典对象
 `void addWord(word)`  将  `word`  添加到数据结构中，之后可以对它进行匹配
 `bool search(word)`  如果数据结构中存在字符串与  `word`  匹配，则返回  `true`  ；否则，返回   `false`  。 `word`  中可能包含一些  `'.'`  ，每个  `.`  都可以表示任何一个字母。

 
 **示例：** 

```text
输入：
["WordDictionary","addWord","addWord","addWord","search","search","search","search"]
[[],["bad"],["dad"],["mad"],["pad"],["bad"],[".ad"],["b.."]]
输出：
[null,null,null,null,false,true,true,true]

解释：
WordDictionary wordDictionary = new WordDictionary();
wordDictionary.addWord("bad");
wordDictionary.addWord("dad");
wordDictionary.addWord("mad");
wordDictionary.search("pad"); // 返回 False
wordDictionary.search("bad"); // 返回 True
wordDictionary.search(".ad"); // 返回 True
wordDictionary.search("b.."); // 返回 True
```

 
 **提示：** 

 `1 <= word.length <= 25` 
 `addWord`  中的  `word`  由小写英文字母组成
 `search`  中的  `word`  由 '.' 或小写英文字母组成
最多调用  `104`  次  `addWord`  和  `search`

---

## 212. 单词搜索 II (Hard)

给定一个  `m x n`  二维字符网格  `board`  **** 和一个单词（字符串）列表  `words` ， 返回所有二维网格上的单词 。
单词必须按照字母顺序，通过  **相邻的单元格**  内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母在一个单词中不允许被重复使用。
 
 **示例 1：** 

```text
输入：board = [["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]], words = ["oath","pea","eat","rain"]
输出：["eat","oath"]
```

 **示例 2：** 

```text
输入：board = [["a","b"],["c","d"]], words = ["abcb"]
输出：[]
```

 
 **提示：** 

 `m == board.length` 
 `n == board[i].length` 
 `1 <= m, n <= 12` 
 `board[i][j]`  是一个小写英文字母
 `1 <= words.length <= 3 * 104` 
 `1 <= words[i].length <= 10` 
 `words[i]`  由小写英文字母组成
 `words`  中的所有字符串互不相同

---

## 213. 打家劫舍 II (Medium)

你是一个专业的小偷，计划偷窃沿街的房屋，每间房内都藏有一定的现金。这个地方所有的房屋都  **围成一圈**  ，这意味着第一个房屋和最后一个房屋是紧挨着的。同时，相邻的房屋装有相互连通的防盗系统， **如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警**  。
给定一个代表每个房屋存放金额的非负整数数组，计算你  **在不触动警报装置的情况下**  ，今晚能够偷窃到的最高金额。
 
 **示例 1：** 

```text
输入：nums = [2,3,2]
输出：3
解释：你不能先偷窃 1 号房屋（金额 = 2），然后偷窃 3 号房屋（金额 = 2）, 因为他们是相邻的。
```

 **示例 2：** 

```text
输入：nums = [1,2,3,1]
输出：4
解释：你可以先偷窃 1 号房屋（金额 = 1），然后偷窃 3 号房屋（金额 = 3）。
     偷窃到的最高金额 = 1 + 3 = 4 。
```

 **示例 3：** 

```text
输入：nums = [1,2,3]
输出：3
```

 
 **提示：** 

 `1 <= nums.length <= 100` 
 `0 <= nums[i] <= 1000`

---

## 214. 最短回文串 (Hard)

给定一个字符串  **s** ，你可以通过在字符串前面添加字符将其转换为回文串。找到并返回可以用这种方式转换的最短回文串。
 
 **示例 1：** 

```text
输入：s = "aacecaaa"
输出："aaacecaaa"
```

 **示例 2：** 

```text
输入：s = "abcd"
输出："dcbabcd"
```

 
 **提示：** 

 `0 <= s.length <= 5 * 104` 
 `s`  仅由小写英文字母组成

---

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

## 216. 组合总和 III (Medium)

找出所有相加之和为  `n`  的  `k`  **** 个数的组合，且满足下列条件：

只使用数字1到9
每个数字  **最多使用一次**  

返回 所有可能的有效组合的列表 。该列表不能包含相同的组合两次，组合可以以任何顺序返回。
 
 **示例 1:** 

```text
输入: k = 3, n = 7
输出: [[1,2,4]]
解释:
1 + 2 + 4 = 7
没有其他符合的组合了。
```

 **示例 2:** 

```text
输入: k = 3, n = 9
输出: [[1,2,6], [1,3,5], [2,3,4]]
解释:
1 + 2 + 6 = 9
1 + 3 + 5 = 9
2 + 3 + 4 = 9
没有其他符合的组合了。
```

 **示例 3:** 

```text
输入: k = 4, n = 1
输出: []
解释: 不存在有效的组合。
在[1,9]范围内使用4个不同的数字，我们可以得到的最小和是1+2+3+4 = 10，因为10 > 1，没有有效的组合。
```

 
 **提示:** 

 `2 <= k <= 9` 
 `1 <= n <= 60`

---

## 217. 存在重复元素 (Easy)

给你一个整数数组  `nums`  。如果任一值在数组中出现  **至少两次**  ，返回  `true`  ；如果数组中每个元素互不相同，返回  `false`  。
 
 **示例 1：** 

 **输入：** nums = [1,2,3,1]
 **输出：** true
 **解释：** 
元素 1 在下标 0 和 3 出现。

 **示例 2：** 

 **输入：** nums = [1,2,3,4]
 **输出：** false
 **解释：** 
所有元素都不同。

 **示例 3：** 

 **输入：** nums = [1,1,1,3,3,4,3,2,4,2]
 **输出：** true

 
 **提示：** 

 `1 <= nums.length <= 105` 
 `-109 <= nums[i] <= 109`

---

## 218. 天际线问题 (Hard)

城市的  **天际线**  是从远处观看该城市中所有建筑物形成的轮廓的外部轮廓。给你所有建筑物的位置和高度，请返回 由这些建筑物形成的 **天际线**  。
每个建筑物的几何信息由数组  `buildings`  表示，其中三元组  `buildings[i] = [lefti, righti, heighti]`  表示：

 `lefti`  是第  `i`  座建筑物左边缘的  `x`  坐标。
 `righti`  是第  `i`  座建筑物右边缘的  `x`  坐标。
 `heighti`  是第  `i`  座建筑物的高度。

你可以假设所有的建筑都是完美的长方形，在高度为  `0`  的绝对平坦的表面上。
 **天际线**  应该表示为由 “关键点” 组成的列表，格式  `[[x1,y1],[x2,y2],...]`  ，并按  **x 坐标** 进行  **排序**  。 **关键点是水平线段的左端点** 。列表中最后一个点是最右侧建筑物的终点， `y`  坐标始终为  `0`  ，仅用于标记天际线的终点。此外，任何两个相邻建筑物之间的地面都应被视为天际线轮廓的一部分。
 **注意：** 输出天际线中不得有连续的相同高度的水平线。例如  `[...[2 3], [4 5], [7 5], [11 5], [12 7]...]`  是不正确的答案；三条高度为 5 的线应该在最终输出中合并为一个： `[...[2 3], [4 5], [12 7], ...]` 
 
 **示例 1：** 

```text
输入：buildings = [[2,9,10],[3,7,15],[5,12,12],[15,20,10],[19,24,8]]
输出：[[2,10],[3,15],[7,12],[12,0],[15,10],[20,8],[24,0]]
解释：
图 A 显示输入的所有建筑物的位置和高度，
图 B 显示由这些建筑物形成的天际线。图 B 中的红点表示输出列表中的关键点。
```

 **示例 2：** 

```text
输入：buildings = [[0,2,3],[2,5,3]]
输出：[[0,3],[5,0]]
```

 
 **提示：** 

 `1 <= buildings.length <= 104` 
 `0 <= lefti < righti <= 231 - 1` 
 `1 <= heighti <= 231 - 1` 
 `buildings`  按  `lefti`  非递减排序

---

## 219. 存在重复元素 II (Easy)

给你一个整数数组  `nums`  和一个整数  `k`  ，判断数组中是否存在两个  **不同的索引**   `i`  和  `j`  ，满足  `nums[i] == nums[j]`  且  `abs(i - j) <= k`  。如果存在，返回  `true`  ；否则，返回  `false`  。
 
 **示例 1：** 

```text
输入：nums = [1,2,3,1], k = 3
输出：true
```

 **示例 2：** 

```text
输入：nums = [1,0,1,1], k = 1
输出：true
```

 **示例 3：** 

```text
输入：nums = [1,2,3,1,2,3], k = 2
输出：false
```

 
 
 **提示：** 

 `1 <= nums.length <= 105` 
 `-109 <= nums[i] <= 109` 
 `0 <= k <= 105`

---

## 220. 存在重复元素 III (Hard)

给你一个整数数组  `nums`  和两个整数  `indexDiff`  和  `valueDiff`  。
找出满足下述条件的下标对  `(i, j)` ：

 `i != j` ,
 `abs(i - j) <= indexDiff` 
 `abs(nums[i] - nums[j]) <= valueDiff` 

如果存在，返回  `true`  ；否则，返回  `false`  。
 
 **示例 1：** 

```text
输入：nums = [1,2,3,1], indexDiff = 3, valueDiff = 0
输出：true
解释：可以找出 (i, j) = (0, 3) 。
满足下述 3 个条件：
i != j --> 0 != 3
abs(i - j) <= indexDiff --> abs(0 - 3) <= 3
abs(nums[i] - nums[j]) <= valueDiff --> abs(1 - 1) <= 0
```

 **示例 2：** 

```text
输入：nums = [1,5,9,1,5,9], indexDiff = 2, valueDiff = 3
输出：false
解释：尝试所有可能的下标对 (i, j) ，均无法满足这 3 个条件，因此返回 false 。
```

 
 **提示：** 

 `2 <= nums.length <= 105` 
 `-109 <= nums[i] <= 109` 
 `1 <= indexDiff <= nums.length` 
 `0 <= valueDiff <= 109`

---

## 221. 最大正方形 (Medium)

在一个由  `'0'`  和  `'1'`  组成的二维矩阵内，找到只包含  `'1'`  的最大正方形，并返回其面积。
 
 **示例 1：** 

```text
输入：matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]
输出：4
```

 **示例 2：** 

```text
输入：matrix = [["0","1"],["1","0"]]
输出：1
```

 **示例 3：** 

```text
输入：matrix = [["0"]]
输出：0
```

 
 **提示：** 

 `m == matrix.length` 
 `n == matrix[i].length` 
 `1 <= m, n <= 300` 
 `matrix[i][j]`  为  `'0'`  或  `'1'`

---

## 222. 完全二叉树的节点个数 (Easy)

给你一棵 **完全二叉树**  的根节点  `root`  ，求出该树的节点个数。
完全二叉树 的定义如下：在完全二叉树中，除了最底层节点可能没填满外，其余每层节点数都达到最大值，并且最下面一层的节点都集中在该层最左边的若干位置。若最底层为第  `h`  层（从第 0 层开始），则该层包含  `1~ 2h`  个节点。
 
 **示例 1：** 

```text
输入：root = [1,2,3,4,5,6]
输出：6
```

 **示例 2：** 

```text
输入：root = []
输出：0
```

 **示例 3：** 

```text
输入：root = [1]
输出：1
```

 
 **提示：** 

树中节点的数目范围是 `[0, 5 * 104]` 
 `0 <= Node.val <= 5 * 104` 
题目数据保证输入的树是  **完全二叉树** 

 
 **进阶：** 遍历树来统计节点是一种时间复杂度为  `O(n)`  的简单解决方案。你可以设计一个更快的算法吗？

---

## 223. 矩形面积 (Medium)

给你  **二维**  平面上两个  **由直线构成且边与坐标轴平行/垂直**  的矩形，请你计算并返回两个矩形覆盖的总面积。
每个矩形由其  **左下**  顶点和  **右上**  顶点坐标表示：

第一个矩形由其左下顶点  `(ax1, ay1)`  和右上顶点  `(ax2, ay2)`  定义。
第二个矩形由其左下顶点  `(bx1, by1)`  和右上顶点  `(bx2, by2)`  定义。

 
 **示例 1：** 

```text
输入：ax1 = -3, ay1 = 0, ax2 = 3, ay2 = 4, bx1 = 0, by1 = -1, bx2 = 9, by2 = 2
输出：45
```

 **示例 2：** 

```text
输入：ax1 = -2, ay1 = -2, ax2 = 2, ay2 = 2, bx1 = -2, by1 = -2, bx2 = 2, by2 = 2
输出：16
```

 
 **提示：** 

 `-104 <= ax1 <= ax2 <= 104` 
 `-104 <= ay1 <= ay2 <= 104` 
 `-104 <= bx1 <= bx2 <= 104` 
 `-104 <= by1 <= by2 <= 104`

---

## 224. 基本计算器 (Hard)

给你一个字符串表达式  `s`  ，请你实现一个基本计算器来计算并返回它的值。
注意:不允许使用任何将字符串作为数学表达式计算的内置函数，比如  `eval()`  。
 
 **示例 1：** 

```text
输入：s = "1 + 1"
输出：2
```

 **示例 2：** 

```text
输入：s = " 2-1 + 2 "
输出：3
```

 **示例 3：** 

```text
输入：s = "(1+(4+5+2)-3)+(6+8)"
输出：23
```

 
 **提示：** 

 `1 <= s.length <= 3 * 105` 
 `s`  由数字、 `'+'` 、 `'-'` 、 `'('` 、 `')'` 、和  `' '`  组成
 `s`  表示一个有效的表达式
 `'+'`  不能用作一元运算(例如，  `"+1"`  和  `"+(2 + 3)"`  无效)
 `'-'`  可以用作一元运算(即  `"-1"`  和  `"-(2 + 3)"`  是有效的)
输入中不存在两个连续的操作符
每个数字和运行的计算将适合于一个有符号的 32位 整数

---

## 225. 用队列实现栈 (Easy)

请你仅使用两个队列实现一个后入先出（LIFO）的栈，并支持普通栈的全部四种操作（ `push` 、 `top` 、 `pop`  和  `empty` ）。
实现  `MyStack`  类：

 `void push(int x)`  将元素 x 压入栈顶。
 `int pop()`  移除并返回栈顶元素。
 `int top()`  返回栈顶元素。
 `boolean empty()`  如果栈是空的，返回  `true`  ；否则，返回  `false`  。

 
 **注意：** 

你只能使用队列的标准操作 —— 也就是  `push to back` 、 `peek/pop from front` 、 `size`  和  `is empty`  这些操作。
你所使用的语言也许不支持队列。 你可以使用 list （列表）或者 deque（双端队列）来模拟一个队列 , 只要是标准的队列操作即可。

 
 **示例：** 

```text
输入：
["MyStack", "push", "push", "top", "pop", "empty"]
[[], [1], [2], [], [], []]
输出：
[null, null, null, 2, 2, false]

解释：
MyStack myStack = new MyStack();
myStack.push(1);
myStack.push(2);
myStack.top(); // 返回 2
myStack.pop(); // 返回 2
myStack.empty(); // 返回 False
```

 
 **提示：** 

 `1 <= x <= 9` 
最多调用 `100`  次  `push` 、 `pop` 、 `top`  和  `empty` 
每次调用  `pop`  和  `top`  都保证栈不为空

 
 **进阶：** 你能否仅用一个队列来实现栈。

---

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

## 227. 基本计算器 II (Medium)

给你一个字符串表达式  `s`  ，请你实现一个基本计算器来计算并返回它的值。
整数除法仅保留整数部分。
你可以假设给定的表达式总是有效的。所有中间结果将在  `[-231, 231 - 1]`  的范围内。
 **注意：** 不允许使用任何将字符串作为数学表达式计算的内置函数，比如  `eval()`  。
 
 **示例 1：** 

```text
输入：s = "3+2*2"
输出：7
```

 **示例 2：** 

```text
输入：s = " 3/2 "
输出：1
```

 **示例 3：** 

```text
输入：s = " 3+5 / 2 "
输出：5
```

 
 **提示：** 

 `1 <= s.length <= 3 * 105` 
 `s`  由整数和算符  `('+', '-', '*', '/')`  组成，中间由一些空格隔开
 `s`  表示一个  **有效表达式** 
表达式中的所有整数都是非负整数，且在范围  `[0, 231 - 1]`  内
题目数据保证答案是一个  **32-bit 整数**

---

## 228. 汇总区间 (Easy)

给定一个   **无重复元素**  的  **有序**  整数数组  `nums`  。
区间  `[a,b]`  是从  `a`  到  `b` （包含）的所有整数的集合。
返回  **恰好覆盖数组中所有数字**  的  **最小有序**  区间范围列表 。也就是说， `nums`  的每个元素都恰好被某个区间范围所覆盖，并且不存在属于某个区间但不属于  `nums`  的数字  `x`  。
列表中的每个区间范围  `[a,b]`  应该按如下格式输出：

 `"a->b"`  ，如果  `a != b` 
 `"a"`  ，如果  `a == b` 

 
 **示例 1：** 

```text
输入：nums = [0,1,2,4,5,7]
输出：["0->2","4->5","7"]
解释：区间范围是：
[0,2] --> "0->2"
[4,5] --> "4->5"
[7,7] --> "7"
```

 **示例 2：** 

```text
输入：nums = [0,2,3,4,6,8,9]
输出：["0","2->4","6","8->9"]
解释：区间范围是：
[0,0] --> "0"
[2,4] --> "2->4"
[6,6] --> "6"
[8,9] --> "8->9"
```

 
 **提示：** 

 `0 <= nums.length <= 20` 
 `-231 <= nums[i] <= 231 - 1` 
 `nums`  中的所有值都  **互不相同** 
 `nums`  按升序排列

---

## 229. 多数元素 II (Medium)

给定一个大小为 n 的整数数组，找出其中所有出现超过  `⌊ n/3 ⌋`  次的元素。
 
 **示例 1：** 

```text
输入：nums = [3,2,3]
输出：[3]
```

 **示例 2：** 

```text
输入：nums = [1]
输出：[1]
```

 **示例 3：** 

```text
输入：nums = [1,2]
输出：[1,2]
```

 
 **提示：** 

 `1 <= nums.length <= 5 * 104` 
 `-109 <= nums[i] <= 109` 

 
 **进阶：** 尝试设计时间复杂度为 O(n)、空间复杂度为 O(1)的算法解决此问题。

---

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

## 231. 2 的幂 (Easy)

给你一个整数  `n` ，请你判断该整数是否是 2 的幂次方。如果是，返回  `true`  ；否则，返回  `false`  。
如果存在一个整数  `x`  使得  `n == 2x`  ，则认为  `n`  是 2 的幂次方。
 
 **示例 1：** 

```text
输入：n = 1
输出：true
解释：20 = 1
```

 **示例 2：** 

```text
输入：n = 16
输出：true
解释：24 = 16
```

 **示例 3：** 

```text
输入：n = 3
输出：false
```

 
 **提示：** 

 `-231 <= n <= 231 - 1` 

 
 **进阶：** 你能够不使用循环/递归解决此问题吗？

---

## 232. 用栈实现队列 (Easy)

请你仅使用两个栈实现先入先出队列。队列应当支持一般队列支持的所有操作（ `push` 、 `pop` 、 `peek` 、 `empty` ）：
实现  `MyQueue`  类：

 `void push(int x)`  将元素 x 推到队列的末尾
 `int pop()`  从队列的开头移除并返回元素
 `int peek()`  返回队列开头的元素
 `boolean empty()`  如果队列为空，返回  `true`  ；否则，返回  `false` 

 **说明：** 

你  **只能**  使用标准的栈操作 —— 也就是只有  `push to top` ,  `peek/pop from top` ,  `size` , 和  `is empty`  操作是合法的。
你所使用的语言也许不支持栈。你可以使用 list 或者 deque（双端队列）来模拟一个栈，只要是标准的栈操作即可。

 
 **示例 1：** 

```text
输入：
["MyQueue", "push", "push", "peek", "pop", "empty"]
[[], [1], [2], [], [], []]
输出：
[null, null, null, 1, 1, false]

解释：
MyQueue myQueue = new MyQueue();
myQueue.push(1); // queue is: [1]
myQueue.push(2); // queue is: [1, 2] (leftmost is front of the queue)
myQueue.peek(); // return 1
myQueue.pop(); // return 1, queue is [2]
myQueue.empty(); // return false
```

 
 **提示：** 

 `1 <= x <= 9` 
最多调用  `100`  次  `push` 、 `pop` 、 `peek`  和  `empty` 
假设所有操作都是有效的 （例如，一个空的队列不会调用  `pop`  或者  `peek`  操作）

 
 **进阶：** 

你能否实现每个操作均摊时间复杂度为  `O(1)`  的队列？换句话说，执行  `n`  个操作的总时间复杂度为  `O(n)`  ，即使其中一个操作可能花费较长时间。

---

## 233. 数字 1 的个数 (Hard)

给定一个整数  `n` ，计算所有小于等于  `n`  的非负整数中数字  `1`  出现的个数。
 
 **示例 1：** 

```text
输入：n = 13
输出：6
```

 **示例 2：** 

```text
输入：n = 0
输出：0
```

 
 **提示：** 

 `0 <= n <= 109`

---

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

## 235. 二叉搜索树的最近公共祖先 (Medium)

给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。
百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（ **一个节点也可以是它自己的祖先** ）。”
例如，给定如下二叉搜索树:  root = [6,2,8,0,4,7,9,null,null,3,5]

 
 **示例 1:** 

```text
输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
输出: 6 
解释: 节点 2 和节点 8 的最近公共祖先是 6。
```

 **示例 2:** 

```text
输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
输出: 2
解释: 节点 2 和节点 4 的最近公共祖先是 2, 因为根据定义最近公共祖先节点可以为节点本身。
```

 
 **说明:** 

所有节点的值都是唯一的。
p、q 为不同节点且均存在于给定的二叉搜索树中。

---

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

## 237. 删除链表中的节点 (Medium)

有一个单链表的  `head` ，我们想删除它其中的一个节点  `node` 。
给你一个需要删除的节点  `node`  。你将  **无法访问**  第一个节点   `head` 。
链表的所有值都是  **唯一的** ，并且保证给定的节点  `node`  不是链表中的最后一个节点。
删除给定的节点。注意，删除节点并不是指从内存中删除它。这里的意思是：

给定节点的值不应该存在于链表中。
链表中的节点数应该减少 1。
 `node`  前面的所有值顺序相同。
 `node`  后面的所有值顺序相同。

 **自定义测试：** 

对于输入，你应该提供整个链表  `head`  和要给出的节点  `node` 。 `node`  不应该是链表的最后一个节点，而应该是链表中的一个实际节点。
我们将构建链表，并将节点传递给你的函数。
输出将是调用你函数后的整个链表。

 
 **示例 1：** 

```text
输入：head = [4,5,1,9], node = 5
输出：[4,1,9]
解释：指定链表中值为 5 的第二个节点，那么在调用了你的函数之后，该链表应变为 4 -> 1 -> 9
```

 **示例 2：** 

```text
输入：head = [4,5,1,9], node = 1
输出：[4,5,9]
解释：指定链表中值为 1 的第三个节点，那么在调用了你的函数之后，该链表应变为 4 -> 5 -> 9
```

 
 **提示：** 

链表中节点的数目范围是  `[2, 1000]` 
 `-1000 <= Node.val <= 1000` 
链表中每个节点的值都是  **唯一**  的
需要删除的节点  `node`  是  **链表中的节点**  ，且  **不是末尾节点**

---

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
