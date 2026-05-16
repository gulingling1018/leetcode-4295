# LeetCode 题目合集 Part 7

## 181. 超过经理收入的员工 (Easy)

表： `Employee`

```text
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
| salary      | int     |
| managerId   | int     |
+-------------+---------+
id 是该表的主键（具有唯一值的列）。
该表的每一行都表示雇员的ID、姓名、工资和经理的ID。
```


编写解决方案，找出收入比经理高的员工。
以  **任意顺序**  返回结果表。
结果格式如下所示。

 **示例 1:**

```text
输入:
Employee 表:
+----+-------+--------+-----------+
| id | name  | salary | managerId |
+----+-------+--------+-----------+
| 1  | Joe   | 70000  | 3         |
| 2  | Henry | 80000  | 4         |
| 3  | Sam   | 60000  | Null      |
| 4  | Max   | 90000  | Null      |
+----+-------+--------+-----------+
输出:
+----------+
| Employee |
+----------+
| Joe      |
+----------+
解释: Joe 是唯一挣得比经理多的雇员。
```

---

## 182. 查找重复的电子邮箱 (Easy)

表:  `Person`

```text
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| email       | varchar |
+-------------+---------+
id 是该表的主键（具有唯一值的列）。
此表的每一行都包含一封电子邮件。电子邮件不包含大写字母。
```


编写解决方案来报告所有重复的电子邮件。 请注意，可以保证电子邮件字段不为 NULL。
以  **任意顺序** 返回结果表。
结果格式如下例。

 **示例 1:**

```text
输入:
Person 表:
+----+---------+
| id | email   |
+----+---------+
| 1  | a@b.com |
| 2  | c@d.com |
| 3  | a@b.com |
+----+---------+
输出:
+---------+
| Email   |
+---------+
| a@b.com |
+---------+
解释: a@b.com 出现了两次。
```

---

## 183. 从不订购的客户 (Easy)

`Customers`  表：

```text
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
+-------------+---------+
在 SQL 中，id 是该表的主键。
该表的每一行都表示客户的 ID 和名称。
```

 `Orders`  表：

```text
+-------------+------+
| Column Name | Type |
+-------------+------+
| id          | int  |
| customerId  | int  |
+-------------+------+
在 SQL 中，id 是该表的主键。
customerId 是 Customers 表中 ID 的外键( Pandas 中的连接键)。
该表的每一行都表示订单的 ID 和订购该订单的客户的 ID。
```


找出所有从不点任何东西的顾客。
以  **任意顺序**  返回结果表。
结果格式如下所示。

 **示例 1：**

```text
输入：
Customers table:
+----+-------+
| id | name  |
+----+-------+
| 1  | Joe   |
| 2  | Henry |
| 3  | Sam   |
| 4  | Max   |
+----+-------+
Orders table:
+----+------------+
| id | customerId |
+----+------------+
| 1  | 3          |
| 2  | 1          |
+----+------------+
输出：
+-----------+
| Customers |
+-----------+
| Henry     |
| Max       |
+-----------+
```

---

## 184. 部门工资最高的员工 (Medium)

表：  `Employee`

```text
+--------------+---------+
| 列名          | 类型    |
+--------------+---------+
| id           | int     |
| name         | varchar |
| salary       | int     |
| departmentId | int     |
+--------------+---------+
在 SQL 中，id是此表的主键。
departmentId 是 Department 表中 id 的外键（在 Pandas 中称为 join key）。
此表的每一行都表示员工的 id、姓名和工资。它还包含他们所在部门的 id。
```


表：  `Department`

```text
+-------------+---------+
| 列名         | 类型    |
+-------------+---------+
| id          | int     |
| name        | varchar |
+-------------+---------+
在 SQL 中，id 是此表的主键列。
此表的每一行都表示一个部门的 id 及其名称。
```


查找出每个部门中薪资最高的员工。
按  **任意顺序**  返回结果表。
查询结果格式如下例所示。

 **示例 1:**

```text
输入：
Employee 表:
+----+-------+--------+--------------+
| id | name  | salary | departmentId |
+----+-------+--------+--------------+
| 1  | Joe   | 70000  | 1            |
| 2  | Jim   | 90000  | 1            |
| 3  | Henry | 80000  | 2            |
| 4  | Sam   | 60000  | 2            |
| 5  | Max   | 90000  | 1            |
+----+-------+--------+--------------+
Department 表:
+----+-------+
| id | name  |
+----+-------+
| 1  | IT    |
| 2  | Sales |
+----+-------+
输出：
+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| IT         | Jim      | 90000  |
| Sales      | Henry    | 80000  |
| IT         | Max      | 90000  |
+------------+----------+--------+
解释：Max 和 Jim 在 IT 部门的工资都是最高的，Henry 在销售部的工资最高。
```

---

## 185. 部门工资前三高的所有员工 (Hard)

表:  `Employee`

```text
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| id           | int     |
| name         | varchar |
| salary       | int     |
| departmentId | int     |
+--------------+---------+
id 是该表的主键列(具有唯一值的列)。
departmentId 是 Department 表中 ID 的外键（reference 列）。
该表的每一行都表示员工的ID、姓名和工资。它还包含了他们部门的ID。
```


表:  `Department`

```text
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
+-------------+---------+
id 是该表的主键列(具有唯一值的列)。
该表的每一行表示部门ID和部门名。
```


公司的主管们感兴趣的是公司每个部门中谁赚的钱最多。一个部门的  **高收入者**  是指一个员工的工资在该部门的  **不同**  工资中  **排名前三**  。
编写解决方案，找出每个部门中  **收入高的员工**  。
以  **任意顺序**  返回结果表。
返回结果格式如下所示。

 **示例 1:**

```text
输入:
Employee 表:
+----+-------+--------+--------------+
| id | name  | salary | departmentId |
+----+-------+--------+--------------+
| 1  | Joe   | 85000  | 1            |
| 2  | Henry | 80000  | 2            |
| 3  | Sam   | 60000  | 2            |
| 4  | Max   | 90000  | 1            |
| 5  | Janet | 69000  | 1            |
| 6  | Randy | 85000  | 1            |
| 7  | Will  | 70000  | 1            |
+----+-------+--------+--------------+
Department  表:
+----+-------+
| id | name  |
+----+-------+
| 1  | IT    |
| 2  | Sales |
+----+-------+
输出:
+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| IT         | Max      | 90000  |
| IT         | Joe      | 85000  |
| IT         | Randy    | 85000  |
| IT         | Will     | 70000  |
| Sales      | Henry    | 80000  |
| Sales      | Sam      | 60000  |
+------------+----------+--------+
解释:
在IT部门:
- Max的工资最高
- 兰迪和乔都赚取第二高的独特的薪水
- 威尔的薪水是第三高的

在销售部:
- 亨利的工资最高
- 山姆的薪水第二高
- 没有第三高的工资，因为只有两名员工
```


 **提示：**

没有姓名、薪资和部门  **完全**  相同的员工。

---

## 186. 反转字符串中的单词 II (Medium)

给定一个字符数组 `s`，请原地反转字符串中的单词顺序。

单词定义为不包含空格的字符序列。输入字符串不包含前导或尾随空格，单词之间总是由单个空格分隔。

示例：

```text
输入：["t","h","e"," ","s","k","y"," ","i","s"," ","b","l","u","e"]
输出：["b","l","u","e"," ","i","s"," ","s","k","y"," ","t","h","e"]
```

进阶：你能否不分配额外空间，原地完成？

题面补充来源：leetcode.ca，核对日期：2026-05-15。

---

## 187. 重复的DNA序列 (Medium)

**DNA序列**  由一系列核苷酸组成，缩写为  `'A'` ,  `'C'` ,  `'G'`  和  `'T'` .。

例如， `"ACGAATTCCG"`  是一个  **DNA序列**  。

在研究  **DNA**  时，识别 DNA 中的重复序列非常有用。
给定一个表示  **DNA序列**  的字符串  `s`  ，返回所有在 DNA 分子中出现不止一次的  **长度为  `10`**  的序列(子字符串)。你可以按  **任意顺序**  返回答案。

 **示例 1：**

```text
输入：s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT"
输出：["AAAAACCCCC","CCCCCAAAAA"]
```

 **示例 2：**

```text
输入：s = "AAAAAAAAAAAAA"
输出：["AAAAAAAAAA"]
```


 **提示：**

 `0 <= s.length <= 105`
 `s[i]`  `==`  `'A'` 、 `'C'` 、 `'G'`  or  `'T'`

---

## 188. 买卖股票的最佳时机 IV (Hard)

给你一个整数数组  `prices`  和一个整数  `k`  ，其中  `prices[i]`  是某支给定的股票在第  `i`  天的价格。
设计一个算法来计算你所能获取的最大利润。你最多可以完成  `k`  笔交易。也就是说，你最多可以买  `k`  次，卖  `k`  次。
 **注意：** 你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

 **示例 1：**

```text
输入：k = 2, prices = [2,4,1]
输出：2
解释：在第 1 天 (股票价格 = 2) 的时候买入，在第 2 天 (股票价格 = 4) 的时候卖出，这笔交易所能获得利润 = 4-2 = 2 。
```

 **示例 2：**

```text
输入：k = 2, prices = [3,2,6,5,0,3]
输出：7
解释：在第 2 天 (股票价格 = 2) 的时候买入，在第 3 天 (股票价格 = 6) 的时候卖出, 这笔交易所能获得利润 = 6-2 = 4 。
     随后，在第 5 天 (股票价格 = 0) 的时候买入，在第 6 天 (股票价格 = 3) 的时候卖出, 这笔交易所能获得利润 = 3-0 = 3 。
```


 **提示：**

 `1 <= k <= 100`
 `1 <= prices.length <= 1000`
 `0 <= prices[i] <= 1000`

---

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

## 190. 颠倒二进制位 (Easy)

颠倒给定的 32 位有符号整数的二进制位。

 **示例 1：**

 **输入：** n = 43261596
 **输出：** 964176192
 **解释：**

整数
二进制

43261596
00000010100101000001111010011100

964176192
00111001011110000010100101000000

 **示例 2：**

 **输入：** n = 2147483644
 **输出：** 1073741822
 **解释：**

整数
二进制

2147483644
01111111111111111111111111111100

1073741822
00111111111111111111111111111110


 **提示：**

 `0 <= n <= 231 - 2`
 `n`  为偶数


 **进阶** : 如果多次调用这个函数，你将如何优化你的算法？

---

## 191. 位1的个数 (Easy)

给定一个正整数  `n` ，编写一个函数，获取一个正整数的二进制形式并返回其二进制表达式中 设置位 的个数（也被称为汉明重量）。

 **示例 1：**

```text
输入：n = 11
输出：3
解释：输入的二进制串 1011 中，共有 3 个设置位。
```

 **示例 2：**

```text
输入：n = 128
输出：1
解释：输入的二进制串 10000000 中，共有 1 个设置位。
```

 **示例 3：**

```text
输入：n = 2147483645
输出：30
解释：输入的二进制串 1111111111111111111111111111101 中，共有 30 个设置位。
```


 **提示：**

 `1 <= n <= 231 - 1`


 **进阶** ：

如果多次调用这个函数，你将如何优化你的算法？

---

## 192. 统计词频 (Medium)

写一个 bash 脚本以统计一个文本文件  `words.txt`  中每个单词出现的频率。
为了简单起见，你可以假设：

 `words.txt` 只包括小写字母和  `' '`  。
每个单词只由小写字母组成。
单词间由一个或多个空格字符分隔。

 **示例:**
假设  `words.txt`  内容如下：

```text
the day is sunny the the
the sunny is is
```

你的脚本应当输出（以词频降序排列）：

```text
the 4
is 3
sunny 2
day 1
```

 **说明:**

不要担心词频相同的单词的排序问题，每个单词出现的频率都是唯一的。
你可以使用一行 Unix pipes 实现吗？

---

## 193. 有效电话号码 (Easy)

给定一个包含电话号码列表（一行一个电话号码）的文本文件  `file.txt` ，写一个单行 bash 脚本输出所有有效的电话号码。
你可以假设一个有效的电话号码必须满足以下两种格式： (xxx) xxx-xxxx 或 xxx-xxx-xxxx。（x 表示一个数字）
你也可以假设每行前后没有多余的空格字符。

 **示例：**
假设  `file.txt`  内容如下：

```text
987-123-4567
123 456 7890
(123) 456-7890
```

你的脚本应当输出下列有效的电话号码：

```text
987-123-4567
(123) 456-7890
```

---

## 194. 转置文件 (Medium)

给定一个文件  `file.txt` ，转置它的内容。
你可以假设每行列数相同，并且每个字段由  `' '`  分隔。

 **示例：**
假设  `file.txt`  文件内容如下：

```text
name age
alice 21
ryan 30
```

应当输出：

```text
name alice ryan
age 21 30
```

---

## 195. 第十行 (Easy)

给定一个文本文件  `file.txt` ，请只打印这个文件中的第十行。
 **示例:**
假设  `file.txt`  有如下内容：

```text
Line 1
Line 2
Line 3
Line 4
Line 5
Line 6
Line 7
Line 8
Line 9
Line 10
```

你的脚本应当显示第十行：

```text
Line 10
```

 **说明:**
1. 如果文件少于十行，你应当输出什么？
2. 至少有三种不同的解法，请尝试尽可能多的方法来解题。

---

## 196. 删除重复的电子邮箱 (Easy)

表:  `Person`

```text
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| email       | varchar |
+-------------+---------+
id 是该表的主键列(具有唯一值的列)。
该表的每一行包含一封电子邮件。电子邮件将不包含大写字母。
```


编写解决方案 **删除**  所有重复的电子邮件，只保留一个具有最小  `id`  的唯一电子邮件。
（对于 SQL 用户，请注意你应该编写一个  `DELETE`  语句而不是  `SELECT`  语句。）
（对于 Pandas 用户，请注意你应该直接修改  `Person`  表。）
运行脚本后，显示的答案是  `Person`  表。驱动程序将首先编译并运行您的代码片段，然后再显示  `Person`  表。 `Person`  表的最终顺序  **无关紧要**  。
返回结果格式如下示例所示。

 **示例 1:**

```text
输入:
Person 表:
+----+------------------+
| id | email            |
+----+------------------+
| 1  | john@example.com |
| 2  | bob@example.com  |
| 3  | john@example.com |
+----+------------------+
输出:
+----+------------------+
| id | email            |
+----+------------------+
| 1  | john@example.com |
| 2  | bob@example.com  |
+----+------------------+
解释: john@example.com重复两次。我们保留最小的Id = 1。
```

---

## 197. 上升的温度 (Easy)

表：  `Weather`

```text
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| recordDate    | date    |
| temperature   | int     |
+---------------+---------+
id 是该表具有唯一值的列。
没有具有相同 recordDate 的不同行。
该表包含特定日期的温度信息
```


编写解决方案，找出与之前（昨天的）日期相比温度更高的所有日期的  `id`  。
返回结果  **无顺序要求**  。
结果格式如下例子所示。

 **示例 1：**

```text
输入：
Weather 表：
+----+------------+-------------+
| id | recordDate | Temperature |
+----+------------+-------------+
| 1  | 2015-01-01 | 10          |
| 2  | 2015-01-02 | 25          |
| 3  | 2015-01-03 | 20          |
| 4  | 2015-01-04 | 30          |
+----+------------+-------------+
输出：
+----+
| id |
+----+
| 2  |
| 4  |
+----+
解释：
2015-01-02 的温度比前一天高（10 -> 25）
2015-01-04 的温度比前一天高（20 -> 30）
```

---

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

## 201. 数字范围按位与 (Medium)

给你两个整数  `left`  和  `right`  ，表示区间  `[left, right]`  ，返回此区间内所有数字  **按位与**  的结果（包含  `left`  、 `right`  端点）。

 **示例 1：**

```text
输入：left = 5, right = 7
输出：4
```

 **示例 2：**

```text
输入：left = 0, right = 0
输出：0
```

 **示例 3：**

```text
输入：left = 1, right = 2147483647
输出：0
```


 **提示：**

 `0 <= left <= right <= 231 - 1`

---

## 202. 快乐数 (Easy)

编写一个算法来判断一个数  `n`  是不是快乐数。
 **「快乐数」**  定义为：

对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和。
然后重复这个过程直到这个数变为 1，也可能是  **无限循环**  但始终变不到 1。
如果这个过程  **结果为**  1，那么这个数就是快乐数。

如果  `n`  是 快乐数 就返回  `true`  ；不是，则返回  `false`  。

 **示例 1：**

```text
输入：n = 19
输出：true
解释：
12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1
```

 **示例 2：**

```text
输入：n = 2
输出：false
```


 **提示：**

 `1 <= n <= 231 - 1`

---

## 203. 移除链表元素 (Easy)

给你一个链表的头节点  `head`  和一个整数  `val`  ，请你删除链表中所有满足  `Node.val == val`  的节点，并返回  **新的头节点**  。

 **示例 1：**

```text
输入：head = [1,2,6,3,4,5,6], val = 6
输出：[1,2,3,4,5]
```

 **示例 2：**

```text
输入：head = [], val = 1
输出：[]
```

 **示例 3：**

```text
输入：head = [7,7,7,7], val = 7
输出：[]
```


 **提示：**

列表中的节点数目在范围  `[0, 104]`  内
 `1 <= Node.val <= 50`
 `0 <= val <= 50`

---

## 204. 计数质数 (Medium)

给定整数  `n`  ，返回 所有小于非负整数  `n`  的质数的数量 。

 **示例 1：**

```text
输入：n = 10
输出：4
解释：小于 10 的质数一共有 4 个, 它们是 2, 3, 5, 7 。
```

 **示例 2：**

```text
输入：n = 0
输出：0
```

 **示例 3：**

```text
输入：n = 1
输出：0
```


 **提示：**

 `0 <= n <= 5 * 106`

---

## 205. 同构字符串 (Easy)

给定两个字符串  `s`  和  `t`  ，判断它们是否是同构的。
如果  `s`  中的字符可以按某种映射关系替换得到  `t`  ，那么这两个字符串是同构的。
每个出现的字符都应当映射到另一个字符，同时不改变字符的顺序。不同字符不能映射到同一个字符上，相同字符只能映射到同一个字符上，字符可以映射到自己本身。

 **示例 1：**

 **输入：** s = "egg", t = "add"
 **输出：** true
 **解释：**
字符串  `s`  和  `t`  可以通过以下方式变得相同：

将  `'e'`  映射为  `'a'` 。
将  `'g'`  映射为  `'d'` 。

 **示例 2：**

 **输入：** s = "f11", t = "b23"
 **输出：** false
 **解释：**
字符串  `s`  和  `t`  无法变得相同，因为  `'1'`  需要同时映射到  `'2'`  和  `'3'` 。

 **示例 3：**

 **输入：** s = "paper", t = "title"
 **输出：** true


 **提示：**

 `1 <= s.length <= 5 * 104`
 `t.length == s.length`
 `s`  和  `t`  由任意有效的 ASCII 字符组成

---

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

## 209. 长度最小的子数组 (Medium)

给定一个含有  `n`  **** 个正整数的数组和一个正整数  `target`  **。**
找出该数组中满足其总和大于等于 ****  `target`  **** 的长度最小的  **子数组**   `[numsl, numsl+1, ..., numsr-1, numsr]`  ，并返回其长度 **。** 如果不存在符合条件的子数组，返回  `0`  。

 **示例 1：**

```text
输入：target = 7, nums = [2,3,1,2,4,3]
输出：2
解释：子数组 [4,3] 是该条件下的长度最小的子数组。
```

 **示例 2：**

```text
输入：target = 4, nums = [1,4,4]
输出：1
```

 **示例 3：**

```text
输入：target = 11, nums = [1,1,1,1,1,1,1,1]
输出：0
```


 **提示：**

 `1 <= target <= 109`
 `1 <= nums.length <= 105`
 `1 <= nums[i] <= 104`


 **进阶：**

如果你已经实现  `O(n)`  时间复杂度的解法, 请尝试设计一个  `O(n log(n))`  时间复杂度的解法。

---

## 210. 课程表 II (Medium)

现在你总共有  `numCourses`  门课需要选，记为  `0`  到  `numCourses - 1` 。给你一个数组  `prerequisites`  ，其中  `prerequisites[i] = [ai, bi]`  ，表示在选修课程  `ai`  前  **必须**  先选修  `bi`  。

例如，想要学习课程  `0`  ，你需要先完成课程  `1`  ，我们用一个匹配来表示： `[0,1]`  。

返回你为了学完所有课程所安排的学习顺序。可能会有多个正确的顺序，你只要返回  **任意一种**  就可以了。如果不可能完成所有课程，返回  **一个空数组**  。

 **示例 1：**

```text
输入：numCourses = 2, prerequisites = [[1,0]]
输出：[0,1]
解释：总共有 2 门课程。要学习课程 1，你需要先完成课程 0。因此，正确的课程顺序为 [0,1] 。
```

 **示例 2：**

```text
输入：numCourses = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]]
输出：[0,2,1,3]
解释：总共有 4 门课程。要学习课程 3，你应该先完成课程 1 和课程 2。并且课程 1 和课程 2 都应该排在课程 0 之后。
因此，一个正确的课程顺序是 [0,1,2,3] 。另一个正确的排序是 [0,2,1,3] 。
```

 **示例 3：**

```text
输入：numCourses = 1, prerequisites = []
输出：[0]
```


 **提示：**

 `1 <= numCourses <= 2000`
 `0 <= prerequisites.length <= numCourses * (numCourses - 1)`
 `prerequisites[i].length == 2`
 `0 <= ai, bi < numCourses`
 `ai != bi`
所有 `[ai, bi]`   **互不相同**

---

# SQL/Shell/Java 解法补充附录（181-200）

### 181. 超过经理收入的员工

**基础解法：** 员工表自连接，用员工的 `managerId` 匹配经理的 `id`，再比较薪水。

```sql
SELECT e.name AS Employee
FROM Employee e
JOIN Employee m ON e.managerId = m.id
WHERE e.salary > m.salary;
```

**资深解法：** 这是典型“同表两种角色”的自连接；别名 `e/m` 分别代表员工和经理。数据库题按 SQL 提交，Java 侧重点是理解结果集关系。

### 182. 查找重复的电子邮箱

**基础解法：** 按邮箱分组并统计数量，数量大于 1 的邮箱即重复。

```sql
SELECT email AS Email
FROM Person
GROUP BY email
HAVING COUNT(*) > 1;
```

**资深解法：** `WHERE` 过滤行，`HAVING` 过滤分组结果；聚合判断重复值是 SQL 高频模板。

### 183. 从不订购的客户

**基础解法：** 左连接订单表，未匹配到订单的客户其订单字段为 `NULL`。

```sql
SELECT c.name AS Customers
FROM Customers c
LEFT JOIN Orders o ON c.id = o.customerId
WHERE o.id IS NULL;
```

**资深解法：** 也可用 `NOT EXISTS`，通常语义更直接；左连接版本更适合入门理解“反连接”。

### 184. 部门工资最高的员工

**基础解法：** 先按部门求最高薪水，再连接员工表找出薪水等于最高值的员工。

```sql
SELECT d.name AS Department, e.name AS Employee, e.salary AS Salary
FROM Employee e
JOIN Department d ON e.departmentId = d.id
JOIN (
    SELECT departmentId, MAX(salary) AS maxSalary
    FROM Employee
    GROUP BY departmentId
) x ON e.departmentId = x.departmentId AND e.salary = x.maxSalary;
```

**资深解法：** 窗口函数可用 `DENSE_RANK() OVER (PARTITION BY departmentId ORDER BY salary DESC)`，排名为 1 的员工即答案。

### 185. 部门工资前三高的所有员工

**基础解法：** 对每个员工统计本部门有多少个不同薪水比它高，小于 3 则属于前三高。

**资深解法：** 窗口函数按部门分区排名，保留 `DENSE_RANK <= 3`。

```sql
SELECT Department, Employee, Salary
FROM (
    SELECT d.name AS Department,
           e.name AS Employee,
           e.salary AS Salary,
           DENSE_RANK() OVER (
               PARTITION BY e.departmentId
               ORDER BY e.salary DESC
           ) AS rk
    FROM Employee e
    JOIN Department d ON e.departmentId = d.id
) t
WHERE rk <= 3;
```

**基础语法与思想：** `PARTITION BY` 在每个部门内部单独排名；`DENSE_RANK` 能正确处理并列薪水。

### 186. 反转字符串中的单词 II

**基础解法：** 按空格切分后反向拼接，再写回字符数组。

**资深解法：** 原地先整体反转，再逐个单词反转，空间 `O(1)`。

```java
class Solution {
    public void reverseWords(char[] s) {
        reverse(s, 0, s.length - 1);
        int start = 0;
        for (int i = 0; i <= s.length; i++) {
            if (i == s.length || s[i] == ' ') {
                reverse(s, start, i - 1);
                start = i + 1;
            }
        }
    }

    private void reverse(char[] s, int left, int right) {
        while (left < right) {
            char tmp = s[left];
            s[left++] = s[right];
            s[right--] = tmp;
        }
    }
}
```

**基础语法与思想：** `char[]` 可原地修改；“整体反转 + 局部反转”能把单词顺序反转且保持单词内部字符正常。

### 187. 重复的 DNA 序列

**基础解法：** 枚举所有长度为 10 的子串，用集合统计出现次数。

```java
class Solution {
    public List<String> findRepeatedDnaSequences(String s) {
        Set<String> seen = new HashSet<>();
        Set<String> repeated = new HashSet<>();
        for (int i = 0; i + 10 <= s.length(); i++) {
            String sub = s.substring(i, i + 10);
            if (!seen.add(sub)) repeated.add(sub);
        }
        return new ArrayList<>(repeated);
    }
}
```

**资深解法：** 可把 A/C/G/T 编码成 2 位，滚动维护 20 位整数窗口，降低字符串创建开销。
**基础语法与思想：** `Set.add` 返回是否首次加入；固定长度窗口适合滑动哈希。

### 188. 买卖股票的最佳时机 IV

**基础解法：** 三维 DP：天数、交易次数、是否持股，表达清楚但空间较大。

**资深解法：** 压缩为 `buy[t]` 和 `sell[t]`；当 `k >= n/2` 时退化为不限交易次数。

```java
class Solution {
    public int maxProfit(int k, int[] prices) {
        int n = prices.length;
        if (n == 0 || k == 0) return 0;
        if (k >= n / 2) {
            int ans = 0;
            for (int i = 1; i < n; i++) {
                if (prices[i] > prices[i - 1]) ans += prices[i] - prices[i - 1];
            }
            return ans;
        }
        int[] buy = new int[k + 1];
        int[] sell = new int[k + 1];
        Arrays.fill(buy, Integer.MIN_VALUE / 2);
        for (int price : prices) {
            for (int t = 1; t <= k; t++) {
                buy[t] = Math.max(buy[t], sell[t - 1] - price);
                sell[t] = Math.max(sell[t], buy[t] + price);
            }
        }
        return sell[k];
    }
}
```

**基础语法与思想：** `buy[t]` 表示完成不超过 `t` 次交易且持股的最大收益；状态机 DP 是股票题通用模型。

### 189. 轮转数组

**基础解法：** 使用额外数组，把 `nums[i]` 放到 `(i + k) % n`。

**资深解法：** 三次反转：整体反转、反转前 `k` 个、反转剩余元素。

```java
class Solution {
    public void rotate(int[] nums, int k) {
        int n = nums.length;
        k %= n;
        reverse(nums, 0, n - 1);
        reverse(nums, 0, k - 1);
        reverse(nums, k, n - 1);
    }

    private void reverse(int[] nums, int left, int right) {
        while (left < right) {
            int tmp = nums[left];
            nums[left++] = nums[right];
            nums[right--] = tmp;
        }
    }
}
```

**基础语法与思想：** `k %= n` 处理轮转超过数组长度；原地反转空间 `O(1)`。

### 190. 颠倒二进制位

**基础解法：** 循环 32 次，每次取 `n` 的最低位并追加到答案低位。

```java
public class Solution {
    public int reverseBits(int n) {
        int ans = 0;
        for (int i = 0; i < 32; i++) {
            ans = (ans << 1) | (n & 1);
            n >>>= 1;
        }
        return ans;
    }
}
```

**资深解法：** 可用分治掩码交换 16/8/4/2/1 位块；循环版更易掌握。
**基础语法与思想：** `>>>` 是无符号右移，适合处理二进制位而不受符号位影响。

### 191. 位1的个数

**基础解法：** 循环 32 次检查最低位是否为 1。

**资深解法：** 每次执行 `n &= n - 1` 都会删除最低位的 1，循环次数等于 1 的个数。

```java
class Solution {
    public int hammingWeight(int n) {
        int ans = 0;
        while (n != 0) {
            n &= n - 1;
            ans++;
        }
        return ans;
    }
}
```

**基础语法与思想：** Java `int` 是有符号的，但位运算按二进制补码执行；该技巧对负数同样有效。

### 192. 统计词频

**基础解法：** 将空白转换为换行，排序后统计重复行，再按次数降序。

```bash
tr -s ' ' '\n' < words.txt | sort | uniq -c | sort -nr | awk '{print $2, $1}'
```

**资深解法：** 这是 Shell 题，不用 Java 提交；管道把“分词、排序、计数、重排输出”拆成一串小工具。

### 193. 有效电话号码

**基础解法：** 用正则匹配两种合法格式：`xxx-xxx-xxxx` 和 `(xxx) xxx-xxxx`。

```bash
grep -E '^([0-9]{3}-|\([0-9]{3}\) )[0-9]{3}-[0-9]{4}$' file.txt
```

**资深解法：** `^` 与 `$` 保证整行匹配；括号在扩展正则中要转义。

### 194. 转置文件

**基础解法：** 用 `awk` 按行读取字段，把同一列的内容累加到同一个输出行。

```bash
awk '{
    for (i = 1; i <= NF; i++) {
        a[i] = a[i] (NR == 1 ? "" : " ") $i
    }
}
END {
    for (i = 1; i <= NF; i++) print a[i]
}' file.txt
```

**资深解法：** `NR` 是当前行号，`NF` 是当前行字段数；转置的本质是把第 `i` 列聚合成第 `i` 行。

### 195. 第十行

**基础解法：** 用 `awk` 输出行号为 10 的行。

```bash
awk 'NR == 10' file.txt
```

**资深解法：** 也可用 `sed -n '10p' file.txt`；Shell 题的重点是熟悉文本流按行处理。

### 196. 删除重复的电子邮箱

**基础解法：** 自连接删除邮箱相同且 `id` 更大的记录，保留最小 `id`。

```sql
DELETE p1
FROM Person p1
JOIN Person p2
  ON p1.email = p2.email AND p1.id > p2.id;
```

**资深解法：** 删除题要先明确保留规则；这里用 `p1.id > p2.id` 精确表示“删掉重复组中较晚的记录”。

### 197. 上升的温度

**基础解法：** 自连接相邻日期记录，找今天温度高于昨天的 `id`。

```sql
SELECT w1.id
FROM Weather w1
JOIN Weather w2
  ON DATEDIFF(w1.recordDate, w2.recordDate) = 1
WHERE w1.temperature > w2.temperature;
```

**资深解法：** 支持日期加减的数据库也可写成 `w1.recordDate = DATE_ADD(w2.recordDate, INTERVAL 1 DAY)`。

### 198. 打家劫舍

**基础解法：** `dp[i]` 表示前 `i` 间房能抢到的最大金额，转移为抢当前或不抢当前。

**资深解法：** 只依赖前两个状态，用两个变量压缩空间。

```java
class Solution {
    public int rob(int[] nums) {
        int prev2 = 0, prev1 = 0;
        for (int x : nums) {
            int cur = Math.max(prev1, prev2 + x);
            prev2 = prev1;
            prev1 = cur;
        }
        return prev1;
    }
}
```

**基础语法与思想：** 相邻房屋不能同时选择，所以每一步只比较“不抢当前”和“抢当前加前前状态”。

### 199. 二叉树的右视图

**基础解法：** BFS 层序遍历，每层最后一个节点进入答案。

```java
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> ans = new ArrayList<>();
        if (root == null) return ans;
        Queue<TreeNode> queue = new ArrayDeque<>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                if (i == size - 1) ans.add(node.val);
                if (node.left != null) queue.offer(node.left);
                if (node.right != null) queue.offer(node.right);
            }
        }
        return ans;
    }
}
```

**资深解法：** DFS 先访问右子树，当深度首次出现时记录该节点。
**基础语法与思想：** `Queue.offer/poll` 完成层序遍历；右视图就是每一层最右侧节点。

### 200. 岛屿数量

**基础解法：** 遍历网格，遇到陆地就 DFS/BFS 把整座岛标记为水，岛屿数加一。

```java
class Solution {
    public int numIslands(char[][] grid) {
        int m = grid.length, n = grid[0].length, ans = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '1') {
                    ans++;
                    dfs(grid, i, j);
                }
            }
        }
        return ans;
    }

    private void dfs(char[][] grid, int i, int j) {
        if (i < 0 || i == grid.length || j < 0 || j == grid[0].length || grid[i][j] != '1') {
            return;
        }
        grid[i][j] = '0';
        dfs(grid, i + 1, j);
        dfs(grid, i - 1, j);
        dfs(grid, i, j + 1);
        dfs(grid, i, j - 1);
    }
}
```

**资深解法：** 并查集也可把相邻陆地合并，适合扩展到动态图；静态网格 DFS 最直接。
**基础语法与思想：** 原地改写 `grid` 作为访问标记；这是连通块计数模板。

# Java 解法补充附录（201-210）

### 201. 数字范围按位与

**基础解法：** 从 `left` 到 `right` 逐个做按位与，区间很大时会超时。

**资深解法：** 不断右移两端点，直到它们相等，保留公共二进制前缀。

```java
class Solution {
    public int rangeBitwiseAnd(int left, int right) {
        int shift = 0;
        while (left < right) {
            left >>= 1;
            right >>= 1;
            shift++;
        }
        return left << shift;
    }
}
```

**基础语法与思想：** `>>` 和 `<<` 移动二进制位；连续区间按位与只会留下所有数共同的高位前缀。

### 202. 快乐数

**基础解法：** 用 `HashSet` 记录出现过的数字，循环则不是快乐数，到 1 则是。

**资深解法：** 快慢指针检测数值变换链是否成环，空间 `O(1)`。

```java
class Solution {
    public boolean isHappy(int n) {
        int slow = n, fast = next(n);
        while (fast != 1 && slow != fast) {
            slow = next(slow);
            fast = next(next(fast));
        }
        return fast == 1;
    }

    private int next(int n) {
        int sum = 0;
        while (n > 0) {
            int d = n % 10;
            sum += d * d;
            n /= 10;
        }
        return sum;
    }
}
```

**基础语法与思想：** `%` 和 `/` 拆数字位；重复状态意味着进入循环。

### 203. 移除链表元素

**基础解法：** 先处理所有头节点等于 `val` 的情况，再遍历删除后续节点。

**资深解法：** 虚拟头结点统一头删和中间删除。

```java
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        ListNode dummy = new ListNode(0, head);
        ListNode cur = dummy;
        while (cur.next != null) {
            if (cur.next.val == val) cur.next = cur.next.next;
            else cur = cur.next;
        }
        return dummy.next;
    }
}
```

**基础语法与思想：** 删除节点时不移动 `cur`，因为新接上的节点仍需检查。

### 204. 计数质数

**基础解法：** 对每个数试除到平方根，整体约 `O(n sqrt n)`。

**资深解法：** 埃氏筛，从每个质数的平方开始标记倍数。

```java
class Solution {
    public int countPrimes(int n) {
        boolean[] composite = new boolean[n];
        int ans = 0;
        for (int i = 2; i < n; i++) {
            if (!composite[i]) {
                ans++;
                if ((long) i * i < n) {
                    for (int j = i * i; j < n; j += i) composite[j] = true;
                }
            }
        }
        return ans;
    }
}
```

**基础语法与思想：** `long` 防止 `i * i` 溢出；质数筛用已知质数排除合数。

### 205. 同构字符串

**基础解法：** 双向哈希表维护 `s -> t` 与 `t -> s` 的一致映射。

**资深解法：** 用两个长度 256 的数组记录字符上次出现位置，位置不同则映射不一致。

```java
class Solution {
    public boolean isIsomorphic(String s, String t) {
        int[] a = new int[256], b = new int[256];
        for (int i = 0; i < s.length(); i++) {
            char x = s.charAt(i), y = t.charAt(i);
            if (a[x] != b[y]) return false;
            a[x] = b[y] = i + 1;
        }
        return true;
    }
}
```

**基础语法与思想：** 用 `i + 1` 区分“未出现”的 0；同构要求双射。

### 206. 反转链表

**基础解法：** 把节点值放入数组反向写回，改变值不改变节点。

**资深解法：** 三指针原地反转 `next` 指向。

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode prev = null, cur = head;
        while (cur != null) {
            ListNode next = cur.next;
            cur.next = prev;
            prev = cur;
            cur = next;
        }
        return prev;
    }
}
```

**基础语法与思想：** 改指针前保存 `next`；链表反转模板会反复出现。

### 207. 课程表

**基础解法：** DFS 三色标记检测有向图是否存在环。

**资深解法：** 拓扑排序统计入度，能修完所有课程说明无环。

```java
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        List<Integer>[] graph = new ArrayList[numCourses];
        for (int i = 0; i < numCourses; i++) graph[i] = new ArrayList<>();
        int[] indegree = new int[numCourses];
        for (int[] p : prerequisites) {
            graph[p[1]].add(p[0]);
            indegree[p[0]]++;
        }
        Queue<Integer> queue = new ArrayDeque<>();
        for (int i = 0; i < numCourses; i++) if (indegree[i] == 0) queue.offer(i);
        int seen = 0;
        while (!queue.isEmpty()) {
            int cur = queue.poll();
            seen++;
            for (int next : graph[cur]) {
                if (--indegree[next] == 0) queue.offer(next);
            }
        }
        return seen == numCourses;
    }
}
```

**基础语法与思想：** 邻接表表达先修依赖；入度为 0 的课程可先学习。

### 208. 实现 Trie（前缀树）

**基础解法：** 用 `HashSet` 保存所有单词，前缀查询时遍历所有单词判断。

**资深解法：** Trie 每个节点保存 26 个子节点和单词结束标记。

```java
class Trie {
    private static class Node {
        Node[] next = new Node[26];
        boolean word;
    }

    private Node root = new Node();

    public void insert(String word) {
        Node cur = root;
        for (char c : word.toCharArray()) {
            int i = c - 'a';
            if (cur.next[i] == null) cur.next[i] = new Node();
            cur = cur.next[i];
        }
        cur.word = true;
    }

    public boolean search(String word) {
        Node node = find(word);
        return node != null && node.word;
    }

    public boolean startsWith(String prefix) {
        return find(prefix) != null;
    }

    private Node find(String s) {
        Node cur = root;
        for (char c : s.toCharArray()) {
            cur = cur.next[c - 'a'];
            if (cur == null) return null;
        }
        return cur;
    }
}
```

**基础语法与思想：** `Node[] next` 是固定字母表的分支表；Trie 用路径共享压缩公共前缀。

### 209. 长度最小的子数组

**基础解法：** 枚举左端点并向右累加，时间 `O(n^2)`。

**资深解法：** 正整数数组可用滑动窗口，和达到目标后尽量收缩左端点。

```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int left = 0, sum = 0, ans = Integer.MAX_VALUE;
        for (int right = 0; right < nums.length; right++) {
            sum += nums[right];
            while (sum >= target) {
                ans = Math.min(ans, right - left + 1);
                sum -= nums[left++];
            }
        }
        return ans == Integer.MAX_VALUE ? 0 : ans;
    }
}
```

**基础语法与思想：** 数组元素全为正数，窗口扩大和变大、收缩和变小，因此双指针成立。

### 210. 课程表 II

**基础解法：** DFS 后序加入课程，遇到环返回空数组。

**资深解法：** BFS 拓扑排序，出队顺序就是一种合法学习顺序。

```java
class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        List<Integer>[] graph = new ArrayList[numCourses];
        for (int i = 0; i < numCourses; i++) graph[i] = new ArrayList<>();
        int[] indegree = new int[numCourses];
        for (int[] p : prerequisites) {
            graph[p[1]].add(p[0]);
            indegree[p[0]]++;
        }
        Queue<Integer> queue = new ArrayDeque<>();
        for (int i = 0; i < numCourses; i++) if (indegree[i] == 0) queue.offer(i);
        int[] order = new int[numCourses];
        int idx = 0;
        while (!queue.isEmpty()) {
            int cur = queue.poll();
            order[idx++] = cur;
            for (int next : graph[cur]) {
                if (--indegree[next] == 0) queue.offer(next);
            }
        }
        return idx == numCourses ? order : new int[0];
    }
}
```

**基础语法与思想：** 拓扑排序不仅能判环，还能输出满足依赖的线性顺序。
