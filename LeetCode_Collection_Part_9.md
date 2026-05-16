# LeetCode 题目合集 Part 9

## 241. 为运算表达式设计优先级 (Medium)

给你一个由数字和运算符组成的字符串  `expression`  ，按不同优先级组合数字和运算符，计算并返回所有可能组合的结果。你可以  **按任意顺序**  返回答案。
生成的测试用例满足其对应输出值符合 32 位整数范围，不同结果的数量不超过  `104`  。
 
 **示例 1：** 

```text
输入：expression = "2-1-1"
输出：[0,2]
解释：
((2-1)-1) = 0 
(2-(1-1)) = 2
```

 **示例 2：** 

```text
输入：expression = "2*3-4*5"
输出：[-34,-14,-10,-10,10]
解释：
(2*(3-(4*5))) = -34 
((2*3)-(4*5)) = -14 
((2*(3-4))*5) = -10 
(2*((3-4)*5)) = -10 
(((2*3)-4)*5) = 10
```

 
 **提示：** 

 `1 <= expression.length <= 20` 
 `expression`  由数字和算符  `'+'` 、 `'-'`  和  `'*'`  组成。
输入表达式中的所有整数值在范围  `[0, 99]`  
输入表达式中的所有整数都没有前导  `'-'`  或  `'+'`  表示符号。

---

## 242. 有效的字母异位词 (Easy)

给定两个字符串  `s`  和  `t`  ，编写一个函数来判断  `t`  是否是  `s`  的 字母异位词。
 
 **示例 1:** 

```text
输入: s = "anagram", t = "nagaram"
输出: true
```

 **示例 2:** 

```text
输入: s = "rat", t = "car"
输出: false
```

 
 **提示:** 

 `1 <= s.length, t.length <= 5 * 104` 
 `s`  和  `t`  仅包含小写字母

 
 **进阶:** 如果输入字符串包含 unicode 字符怎么办？你能否调整你的解法来应对这种情况？

---

## 243. 最短单词距离 (Easy)

给定字符串数组 `wordsDict` 和两个不同的字符串 `word1`、`word2`，返回这两个单词在数组中出现位置的最小距离。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

---

## 244. 最短单词距离 II (Medium)

设计一个类，构造时接收字符串数组 `wordsDict`；实现 `shortest(word1, word2)`，多次查询两个不同单词在数组中出现位置的最小距离。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

---

## 245. 最短单词距离 III (Medium)

给定字符串数组 `wordsDict` 和两个字符串 `word1`、`word2`，返回它们在数组中出现位置的最小距离；`word1` 和 `word2` 可能相同，相同时需要选择两个不同位置。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

---

## 246. 中心对称数 (Easy)

给定字符串 `num`，判断它旋转 180 度后是否仍然表示同一个数字。合法旋转映射包括 `0-0`、`1-1`、`6-9`、`8-8`、`9-6`。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

---

## 247. 中心对称数 II (Medium)

给定整数 `n`，返回所有长度为 `n` 的中心对称数。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

---

## 248. 中心对称数 III (Hard)

给定两个表示非负整数的字符串 `low` 和 `high`，返回闭区间 `[low, high]` 内中心对称数的个数。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

---

## 249. 移位字符串分组 (Medium)

字符串可以通过把每个字母同时向后移动若干位得到同组字符串，例如 `abc -> bcd`。给定字符串数组，将所有属于同一移位序列的字符串分组返回。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

---

## 250. 统计同值子树 (Medium)

给定二叉树根节点，统计同值子树数量。同值子树表示该子树中的所有节点值都相同。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

---

## 251. 展开二维向量 (Medium)

设计一个迭代器，将二维向量 `vec` 按行展开；实现 `next()` 返回下一个整数，`hasNext()` 判断是否还有元素。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

---

## 252. 会议室 (Easy)

给定会议时间区间数组 `intervals`，判断一个人是否能够参加所有会议。若任意两个会议时间重叠，则无法参加所有会议。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

---

## 253. 会议室 II (Medium)

给定会议时间区间数组 `intervals`，返回完成所有会议所需的最少会议室数量。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

---

## 254. 因子的组合 (Medium)

给定整数 `n`，返回它所有可能的因子组合。组合中的因子必须大于 1 且小于 `n`，每个组合中因子乘积等于 `n`。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

---

## 255. 验证二叉搜索树的前序遍历序列 (Medium)

给定整数数组 `preorder`，判断它是否可以表示某棵二叉搜索树的前序遍历序列。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

---

## 256. 粉刷房子 (Medium)

有一排房子，每间可刷成红、蓝、绿三种颜色，费用由 `costs[i][j]` 给出。相邻房子颜色不能相同，返回粉刷所有房子的最小总费用。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

---

## 257. 二叉树的所有路径 (Easy)

给你一个二叉树的根节点  `root`  ，按  **任意顺序**  ，返回所有从根节点到叶子节点的路径。
 **叶子节点**  是指没有子节点的节点。
 

 **示例 1：** 

```text
输入：root = [1,2,3,null,5]
输出：["1->2->5","1->3"]
```

 **示例 2：** 

```text
输入：root = [1]
输出：["1"]
```

 
 **提示：** 

树中节点的数目在范围  `[1, 100]`  内
 `-100 <= Node.val <= 100`

---

## 258. 各位相加 (Easy)

给定一个非负整数  `num` ，反复将各个位上的数字相加，直到结果为一位数。返回这个结果。
 
 **示例 1:** 

```text
输入: num = 38
输出: 2 
解释: 各位相加的过程为：
38 --> 3 + 8 --> 11
11 --> 1 + 1 --> 2
由于 2 是一位数，所以返回 2。
```

 **示例 2:** 

```text
输入: num = 0
输出: 0
```

 
 **提示：** 

 `0 <= num <= 231 - 1` 

 
 **进阶：** 你可以不使用循环或者递归，在  `O(1)`  时间复杂度内解决这个问题吗？

---

## 259. 较小的三数之和 (Medium)

给定整数数组 `nums` 和目标值 `target`，返回满足 `nums[i] + nums[j] + nums[k] < target` 且 `i < j < k` 的三元组数量。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

---

## 260. 只出现一次的数字 III (Medium)

给你一个整数数组  `nums` ，其中恰好有两个元素只出现一次，其余所有元素均出现两次。 找出只出现一次的那两个元素。你可以按  **任意顺序**  返回答案。
你必须设计并实现线性时间复杂度的算法且仅使用常量额外空间来解决此问题。
 
 **示例 1：** 

```text
输入：nums = [1,2,1,3,2,5]
输出：[3,5]
解释：[5, 3] 也是有效的答案。
```

 **示例 2：** 

```text
输入：nums = [-1,0]
输出：[-1,0]
```

 **示例 3：** 

```text
输入：nums = [0,1]
输出：[1,0]
```

 
 **提示：** 

 `2 <= nums.length <= 3 * 104` 
 `-231 <= nums[i] <= 231 - 1` 
除两个只出现一次的整数外， `nums`  中的其他数字都出现两次

---

## 261. 以图判树 (Medium)

给定 `n` 个节点和无向边数组 `edges`，判断这些边是否构成一棵合法树。合法树需要连通且无环。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

---

## 262. 行程和用户 (Hard)

表： `Trips` 

```text
+-------------+----------+
| Column Name | Type     |
+-------------+----------+
| id          | int      |
| client_id   | int      |
| driver_id   | int      |
| city_id     | int      |
| status      | enum     |
| request_at  | varchar  |     
+-------------+----------+
id 是这张表的主键（具有唯一值的列）。
这张表中存所有出租车的行程信息。每段行程有唯一 id ，其中 client_id 和 driver_id 是 Users 表中 users_id 的外键。
status 是一个表示行程状态的枚举类型，枚举成员为(‘completed’, ‘cancelled_by_driver’, ‘cancelled_by_client’) 。
```

表： `Users` 

```text
+-------------+----------+
| Column Name | Type     |
+-------------+----------+
| users_id    | int      |
| banned      | enum     |
| role        | enum     |
+-------------+----------+
users_id 是这张表的主键（具有唯一值的列）。
这张表中存所有用户，每个用户都有一个唯一的 users_id ，role 是一个表示用户身份的枚举类型，枚举成员为 (‘client’, ‘driver’, ‘partner’) 。
banned 是一个表示用户是否被禁止的枚举类型，枚举成员为 (‘Yes’, ‘No’) 。
```

 **取消率**  的计算方式如下：(被司机或乘客取消的非禁止用户生成的订单数量) / (非禁止用户生成的订单总数)。
编写解决方案找出  `"2013-10-01"`  **** 至  `"2013-10-03"`  **** 期间有  **至少** 一次行程的非禁止用户（ **乘客和司机都必须未被禁止** ）的  **取消率** 。非禁止用户即 banned 为 No 的用户，禁止用户即 banned 为 Yes 的用户。其中取消率  `Cancellation Rate`  需要四舍五入保留  **两位小数**  。
返回结果表中的数据  **无顺序要求**  。
结果格式如下例所示。
 
 **示例 1：** 

```text
输入： 
Trips 表：
+----+-----------+-----------+---------+---------------------+------------+
| id | client_id | driver_id | city_id | status              | request_at |
+----+-----------+-----------+---------+---------------------+------------+
| 1  | 1         | 10        | 1       | completed           | 2013-10-01 |
| 2  | 2         | 11        | 1       | cancelled_by_driver | 2013-10-01 |
| 3  | 3         | 12        | 6       | completed           | 2013-10-01 |
| 4  | 4         | 13        | 6       | cancelled_by_client | 2013-10-01 |
| 5  | 1         | 10        | 1       | completed           | 2013-10-02 |
| 6  | 2         | 11        | 6       | completed           | 2013-10-02 |
| 7  | 3         | 12        | 6       | completed           | 2013-10-02 |
| 8  | 2         | 12        | 12      | completed           | 2013-10-03 |
| 9  | 3         | 10        | 12      | completed           | 2013-10-03 |
| 10 | 4         | 13        | 12      | cancelled_by_driver | 2013-10-03 |
+----+-----------+-----------+---------+---------------------+------------+
Users 表：
+----------+--------+--------+
| users_id | banned | role   |
+----------+--------+--------+
| 1        | No     | client |
| 2        | Yes    | client |
| 3        | No     | client |
| 4        | No     | client |
| 10       | No     | driver |
| 11       | No     | driver |
| 12       | No     | driver |
| 13       | No     | driver |
+----------+--------+--------+
输出：
+------------+-------------------+
| Day        | Cancellation Rate |
+------------+-------------------+
| 2013-10-01 | 0.33              |
| 2013-10-02 | 0.00              |
| 2013-10-03 | 0.50              |
+------------+-------------------+
解释：
2013-10-01：
  - 共有 4 条请求，其中 2 条取消。
  - 然而，id=2 的请求是由禁止用户（user_id=2）发出的，所以计算时应当忽略它。
  - 因此，总共有 3 条非禁止请求参与计算，其中 1 条取消。
  - 取消率为 (1 / 3) = 0.33
2013-10-02：
  - 共有 3 条请求，其中 0 条取消。
  - 然而，id=6 的请求是由禁止用户发出的，所以计算时应当忽略它。
  - 因此，总共有 2 条非禁止请求参与计算，其中 0 条取消。
  - 取消率为 (0 / 2) = 0.00
2013-10-03：
  - 共有 3 条请求，其中 1 条取消。
  - 然而，id=8 的请求是由禁止用户发出的，所以计算时应当忽略它。
  - 因此，总共有 2 条非禁止请求参与计算，其中 1 条取消。
  - 取消率为 (1 / 2) = 0.50
```

---

## 263. 丑数 (Easy)

**丑数** 就是只包含质因数  `2` 、 `3`  和  `5`  的 正 整数。
给你一个整数  `n`  ，请你判断  `n`  是否为  **丑数**  。如果是，返回  `true`  ；否则，返回  `false`  。
 
 **示例 1：** 

```text
输入：n = 6
输出：true
解释：6 = 2 × 3
```

 **示例 2：** 

```text
输入：n = 1
输出：true
解释：1 没有质因数。
```

 **示例 3：** 

```text
输入：n = 14
输出：false
解释：14 不是丑数，因为它包含了另外一个质因数 7 。
```

 
 **提示：** 

 `-231 <= n <= 231 - 1`

---

## 264. 丑数 II (Medium)

给你一个整数  `n`  ，请你找出并返回第  `n`  个  **丑数**  。
 **丑数** 就是质因子只包含  `2` 、 `3`  和  `5`  的正整数。
 
 **示例 1：** 

```text
输入：n = 10
输出：12
解释：[1, 2, 3, 4, 5, 6, 8, 9, 10, 12] 是由前 10 个丑数组成的序列。
```

 **示例 2：** 

```text
输入：n = 1
输出：1
解释：1 通常被视为丑数。
```

 
 **提示：** 

 `1 <= n <= 1690`

---

## 265. 粉刷房子 II (Hard)

有一排房子和 `k` 种颜色，费用由 `costs[i][j]` 给出。相邻房子颜色不能相同，返回粉刷所有房子的最小总费用。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

---

## 266. 回文排列 (Easy)

给定字符串 `s`，判断它的某个排列是否可以组成回文串。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

---

## 267. 回文排列 II (Medium)

给定字符串 `s`，返回它所有可以组成回文串的不同排列；如果不存在，返回空列表。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

---

## 268. 丢失的数字 (Easy)

给定一个包含  `[0, n]`  中  `n`  个数的数组  `nums`  ，找出  `[0, n]`  这个范围内没有出现在数组中的那个数。

 
 **示例 1：** 

 **输入：** nums = [3,0,1]
 **输出：** 2
 **解释：**  `n = 3` ，因为有 3 个数字，所以所有的数字都在范围  `[0,3]`  内。2 是丢失的数字，因为它没有出现在  `nums`  中。

 **示例 2：** 

 **输入：** nums = [0,1]
 **输出：** 2
 **解释：**  `n = 2` ，因为有 2 个数字，所以所有的数字都在范围  `[0,2]`  内。2 是丢失的数字，因为它没有出现在  `nums`  中。

 **示例 3：** 

 **输入：** nums = [9,6,4,2,3,5,7,0,1]
 **输出：** 8
 **解释：**  `n = 9` ，因为有 9 个数字，所以所有的数字都在范围  `[0,9]`  内。8 是丢失的数字，因为它没有出现在  `nums`  中。

 **提示：** 

 `n == nums.length` 
 `1 <= n <= 104` 
 `0 <= nums[i] <= n` 
 `nums`  中的所有数字都  **独一无二** 

 
 **进阶：** 你能否实现线性时间复杂度、仅使用额外常数空间的算法解决此问题?

---

## 269. 火星词典 (Hard)

给定按外星语言字典序排序的单词列表 `words`，推断该语言中字母的顺序。若不存在合法顺序，返回空字符串；若有多个合法顺序，返回任意一个。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

---

## 270. 最接近的二叉搜索树值 (Easy)

给定二叉搜索树根节点和目标值 `target`，返回树中最接近 `target` 的节点值。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

---

# Java/SQL 解法补充附录（241-270）

### 241. 为运算表达式设计优先级

**基础解法：** 枚举每个运算符作为最后一次计算，递归计算左右两边所有可能结果。

**资深解法：** 分治加记忆化，避免重复计算同一子表达式。

```java
class Solution {
    private Map<String, List<Integer>> memo = new HashMap<>();

    public List<Integer> diffWaysToCompute(String expression) {
        if (memo.containsKey(expression)) return memo.get(expression);
        List<Integer> ans = new ArrayList<>();
        for (int i = 0; i < expression.length(); i++) {
            char c = expression.charAt(i);
            if (c == '+' || c == '-' || c == '*') {
                for (int a : diffWaysToCompute(expression.substring(0, i))) {
                    for (int b : diffWaysToCompute(expression.substring(i + 1))) {
                        ans.add(c == '+' ? a + b : c == '-' ? a - b : a * b);
                    }
                }
            }
        }
        if (ans.isEmpty()) ans.add(Integer.parseInt(expression));
        memo.put(expression, ans);
        return ans;
    }
}
```

**基础语法与思想：** `substring` 切分子表达式；表达式加括号的本质是选择最后计算的运算符。

### 242. 有效的字母异位词

**基础解法：** 排序两个字符串后比较。

**资深解法：** 计数字母频次，一个字符串加、另一个字符串减。

```java
class Solution {
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) return false;
        int[] count = new int[26];
        for (int i = 0; i < s.length(); i++) {
            count[s.charAt(i) - 'a']++;
            count[t.charAt(i) - 'a']--;
        }
        for (int c : count) if (c != 0) return false;
        return true;
    }
}
```

**基础语法与思想：** 固定小写字母表可用数组替代哈希表。

### 243. 最短单词距离

**基础解法：** 分别收集两个单词所有位置，再双重循环比较距离。

**资深解法：** 一次扫描记录两个单词最近位置，实时更新最小距离。

```java
class Solution {
    public int shortestDistance(String[] wordsDict, String word1, String word2) {
        int a = -1, b = -1, ans = Integer.MAX_VALUE;
        for (int i = 0; i < wordsDict.length; i++) {
            if (wordsDict[i].equals(word1)) a = i;
            if (wordsDict[i].equals(word2)) b = i;
            if (a != -1 && b != -1) ans = Math.min(ans, Math.abs(a - b));
        }
        return ans;
    }
}
```

**基础语法与思想：** 最近出现位置足以产生当前最小距离候选。

### 244. 最短单词距离 II

**基础解法：** 每次查询都扫描整个数组，适合查询很少的场景。

**资深解法：** 构造时把每个单词出现位置存入列表；查询时双指针合并两个有序位置表。

```java
class WordDistance {
    private Map<String, List<Integer>> pos = new HashMap<>();

    public WordDistance(String[] wordsDict) {
        for (int i = 0; i < wordsDict.length; i++) {
            pos.computeIfAbsent(wordsDict[i], k -> new ArrayList<>()).add(i);
        }
    }

    public int shortest(String word1, String word2) {
        List<Integer> a = pos.get(word1), b = pos.get(word2);
        int i = 0, j = 0, ans = Integer.MAX_VALUE;
        while (i < a.size() && j < b.size()) {
            ans = Math.min(ans, Math.abs(a.get(i) - b.get(j)));
            if (a.get(i) < b.get(j)) i++;
            else j++;
        }
        return ans;
    }
}
```

**基础语法与思想：** `computeIfAbsent` 按需建表；多次查询题通常要预处理。

### 245. 最短单词距离 III

**基础解法：** 收集位置后分类处理两个单词是否相同。

**资深解法：** 一次扫描；相同单词时记录前一个出现位置，不同单词时记录各自最近位置。

```java
class Solution {
    public int shortestWordDistance(String[] wordsDict, String word1, String word2) {
        int a = -1, b = -1, ans = Integer.MAX_VALUE;
        boolean same = word1.equals(word2);
        for (int i = 0; i < wordsDict.length; i++) {
            if (same && wordsDict[i].equals(word1)) {
                if (a != -1) ans = Math.min(ans, i - a);
                a = i;
            } else {
                if (wordsDict[i].equals(word1)) a = i;
                if (wordsDict[i].equals(word2)) b = i;
                if (a != -1 && b != -1) ans = Math.min(ans, Math.abs(a - b));
            }
        }
        return ans;
    }
}
```

**基础语法与思想：** `word1 == word2` 时必须使用两个不同下标。

### 246. 中心对称数

**基础解法：** 构造旋转后的字符串再与原字符串比较。

**资深解法：** 双指针同时检查左右字符是否为合法旋转映射。

```java
class Solution {
    public boolean isStrobogrammatic(String num) {
        Map<Character, Character> map = Map.of('0','0','1','1','6','9','8','8','9','6');
        int l = 0, r = num.length() - 1;
        while (l <= r) {
            char a = num.charAt(l++), b = num.charAt(r--);
            if (!map.containsKey(a) || map.get(a) != b) return false;
        }
        return true;
    }
}
```

**基础语法与思想：** 中心对称是“旋转后左右互换”的字符映射问题。

### 247. 中心对称数 II

**基础解法：** 生成所有长度为 `n` 的数字再逐个判断，会爆炸。

**资深解法：** 从中间向两边递归扩展合法数对，外层不能放 `00`。

```java
class Solution {
    public List<String> findStrobogrammatic(int n) {
        return build(n, n);
    }

    private List<String> build(int len, int total) {
        if (len == 0) return List.of("");
        if (len == 1) return List.of("0", "1", "8");
        List<String> inner = build(len - 2, total);
        List<String> ans = new ArrayList<>();
        for (String s : inner) {
            if (len != total) ans.add("0" + s + "0");
            ans.add("1" + s + "1");
            ans.add("6" + s + "9");
            ans.add("8" + s + "8");
            ans.add("9" + s + "6");
        }
        return ans;
    }
}
```

**基础语法与思想：** 递归长度每次减 2；首位不能为 0。

### 248. 中心对称数 III

**基础解法：** 枚举区间内每个数并判断，字符串范围大时不可行。

**资深解法：** 按长度生成所有中心对称数，再用字符串比较过滤上下界。

```java
class Solution {
    public int strobogrammaticInRange(String low, String high) {
        int ans = 0;
        for (int len = low.length(); len <= high.length(); len++) {
            for (String s : build(len, len)) {
                if ((s.length() == low.length() && s.compareTo(low) < 0) ||
                    (s.length() == high.length() && s.compareTo(high) > 0)) continue;
                ans++;
            }
        }
        return ans;
    }

    private List<String> build(int len, int total) {
        if (len == 0) return List.of("");
        if (len == 1) return List.of("0", "1", "8");
        List<String> ans = new ArrayList<>();
        for (String s : build(len - 2, total)) {
            if (len != total) ans.add("0" + s + "0");
            ans.add("1" + s + "1"); ans.add("6" + s + "9");
            ans.add("8" + s + "8"); ans.add("9" + s + "6");
        }
        return ans;
    }
}
```

**基础语法与思想：** 同长度数字可用字典序比较大小；生成候选比枚举区间更高效。

### 249. 移位字符串分组

**基础解法：** 两两判断是否属于同一移位序列，再并入分组。

**资深解法：** 用相邻字符差值作为规范化 key。

```java
class Solution {
    public List<List<String>> groupStrings(String[] strings) {
        Map<String, List<String>> map = new HashMap<>();
        for (String s : strings) {
            StringBuilder key = new StringBuilder();
            for (int i = 1; i < s.length(); i++) {
                int diff = (s.charAt(i) - s.charAt(i - 1) + 26) % 26;
                key.append(diff).append('#');
            }
            map.computeIfAbsent(key.toString(), k -> new ArrayList<>()).add(s);
        }
        return new ArrayList<>(map.values());
    }
}
```

**基础语法与思想：** 环形字母差值用 `+26` 后取模；规范化 key 把同类字符串聚到一起。

### 250. 统计同值子树

**基础解法：** 对每个节点单独检查子树是否同值，重复遍历较多。

**资深解法：** 后序 DFS 返回当前子树是否同值，并顺手计数。

```java
class Solution {
    private int ans = 0;

    public int countUnivalSubtrees(TreeNode root) {
        dfs(root);
        return ans;
    }

    private boolean dfs(TreeNode node) {
        if (node == null) return true;
        boolean left = dfs(node.left), right = dfs(node.right);
        if (!left || !right) return false;
        if (node.left != null && node.left.val != node.val) return false;
        if (node.right != null && node.right.val != node.val) return false;
        ans++;
        return true;
    }
}
```

**基础语法与思想：** 后序先知道左右子树结论，再判断当前子树。

### 251. 展开二维向量

**基础解法：** 构造时把所有元素展开到一维列表。

**资深解法：** 两个下标按需跳过空行，惰性返回元素。

```java
class Vector2D {
    private int[][] vec;
    private int row = 0, col = 0;

    public Vector2D(int[][] vec) {
        this.vec = vec;
    }

    public int next() {
        hasNext();
        return vec[row][col++];
    }

    public boolean hasNext() {
        while (row < vec.length && col == vec[row].length) {
            row++;
            col = 0;
        }
        return row < vec.length;
    }
}
```

**基础语法与思想：** `hasNext()` 负责推进到下一个有效位置，可让 `next()` 简洁。

### 252. 会议室

**基础解法：** 两两检查会议区间是否重叠。

**资深解法：** 按开始时间排序，只需检查相邻会议是否冲突。

```java
class Solution {
    public boolean canAttendMeetings(int[][] intervals) {
        Arrays.sort(intervals, Comparator.comparingInt(a -> a[0]));
        for (int i = 1; i < intervals.length; i++) {
            if (intervals[i][0] < intervals[i - 1][1]) return false;
        }
        return true;
    }
}
```

**基础语法与思想：** 排序后若有重叠，一定出现在相邻区间之间。

### 253. 会议室 II

**基础解法：** 扫描每个会议开始时有多少会议尚未结束。

**资深解法：** 按开始时间排序，小根堆保存当前占用会议室的结束时间。

```java
class Solution {
    public int minMeetingRooms(int[][] intervals) {
        Arrays.sort(intervals, Comparator.comparingInt(a -> a[0]));
        PriorityQueue<Integer> heap = new PriorityQueue<>();
        for (int[] in : intervals) {
            if (!heap.isEmpty() && heap.peek() <= in[0]) heap.poll();
            heap.offer(in[1]);
        }
        return heap.size();
    }
}
```

**基础语法与思想：** 最早结束的会议室若能复用，就弹出它；堆大小是所需房间数。

### 254. 因子的组合

**基础解法：** 枚举所有组合并检查乘积。

**资深解法：** 回溯从当前最小因子开始试除，保证组合非降序以去重。

```java
class Solution {
    public List<List<Integer>> getFactors(int n) {
        List<List<Integer>> ans = new ArrayList<>();
        dfs(n, 2, new ArrayList<>(), ans);
        return ans;
    }

    private void dfs(int n, int start, List<Integer> path, List<List<Integer>> ans) {
        for (int f = start; f * f <= n; f++) {
            if (n % f == 0) {
                path.add(f);
                path.add(n / f);
                ans.add(new ArrayList<>(path));
                path.remove(path.size() - 1);
                dfs(n / f, f, path, ans);
                path.remove(path.size() - 1);
            }
        }
    }
}
```

**基础语法与思想：** `start` 保证组合顺序；先收集当前因子对，再继续拆右侧因子。

### 255. 验证二叉搜索树的前序遍历序列

**基础解法：** 递归按 BST 前序性质切分左右子序列。

**资深解法：** 单调栈维护祖先路径，`lower` 表示当前节点必须大于的下界。

```java
class Solution {
    public boolean verifyPreorder(int[] preorder) {
        Deque<Integer> stack = new ArrayDeque<>();
        int lower = Integer.MIN_VALUE;
        for (int x : preorder) {
            if (x < lower) return false;
            while (!stack.isEmpty() && x > stack.peek()) lower = stack.pop();
            stack.push(x);
        }
        return true;
    }
}
```

**基础语法与思想：** 一旦进入某个祖先的右子树，后续值都必须大于该祖先。

### 256. 粉刷房子

**基础解法：** `dp[i][color]` 表示刷到第 `i` 间且当前颜色为 `color` 的最小费用。

**资深解法：** 原地更新 `costs`，每行只依赖上一行其他两个颜色。

```java
class Solution {
    public int minCost(int[][] costs) {
        for (int i = 1; i < costs.length; i++) {
            costs[i][0] += Math.min(costs[i - 1][1], costs[i - 1][2]);
            costs[i][1] += Math.min(costs[i - 1][0], costs[i - 1][2]);
            costs[i][2] += Math.min(costs[i - 1][0], costs[i - 1][1]);
        }
        int[] last = costs[costs.length - 1];
        return Math.min(last[0], Math.min(last[1], last[2]));
    }
}
```

**基础语法与思想：** 相邻颜色不同，所以当前颜色只能接上一行另外两种颜色。

### 257. 二叉树的所有路径

**基础解法：** DFS 携带路径字符串，到叶子节点收集。

**资深解法：** 用 `StringBuilder` 回溯，减少中间字符串创建。

```java
class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> ans = new ArrayList<>();
        dfs(root, new StringBuilder(), ans);
        return ans;
    }

    private void dfs(TreeNode node, StringBuilder path, List<String> ans) {
        if (node == null) return;
        int len = path.length();
        if (len > 0) path.append("->");
        path.append(node.val);
        if (node.left == null && node.right == null) ans.add(path.toString());
        else {
            dfs(node.left, path, ans);
            dfs(node.right, path, ans);
        }
        path.setLength(len);
    }
}
```

**基础语法与思想：** `setLength` 是 `StringBuilder` 回溯撤销的常用手法。

### 258. 各位相加

**基础解法：** 循环求各位数字和，直到结果小于 10。

**资深解法：** 数根公式：除 0 外答案为 `1 + (num - 1) % 9`。

```java
class Solution {
    public int addDigits(int num) {
        return num == 0 ? 0 : 1 + (num - 1) % 9;
    }
}
```

**基础语法与思想：** 十进制数对 9 同余于其各位数字和。

### 259. 较小的三数之和

**基础解法：** 三重循环枚举所有三元组。

**资深解法：** 排序后固定一个数，双指针统计小于目标的组合数。

```java
class Solution {
    public int threeSumSmaller(int[] nums, int target) {
        Arrays.sort(nums);
        int ans = 0;
        for (int i = 0; i < nums.length - 2; i++) {
            int l = i + 1, r = nums.length - 1;
            while (l < r) {
                if (nums[i] + nums[l] + nums[r] < target) {
                    ans += r - l;
                    l++;
                } else r--;
            }
        }
        return ans;
    }
}
```

**基础语法与思想：** 当前和小于目标时，`l` 到 `r` 之间任意右端点都成立。

### 260. 只出现一次的数字 III

**基础解法：** 哈希表计数，找出现一次的两个数。

**资深解法：** 全体异或得到 `a ^ b`，取最低位 1 把两个数分到不同组。

```java
class Solution {
    public int[] singleNumber(int[] nums) {
        int xor = 0;
        for (int x : nums) xor ^= x;
        int bit = xor & -xor;
        int a = 0, b = 0;
        for (int x : nums) {
            if ((x & bit) == 0) a ^= x;
            else b ^= x;
        }
        return new int[]{a, b};
    }
}
```

**基础语法与思想：** 异或可消去成对数字；区分位保证两个唯一数分到不同组。

### 261. 以图判树

**基础解法：** 树必须边数为 `n - 1`，再 DFS 检查连通性。

**资深解法：** 并查集，若合并时发现同根说明有环。

```java
class Solution {
    public boolean validTree(int n, int[][] edges) {
        if (edges.length != n - 1) return false;
        int[] parent = new int[n];
        for (int i = 0; i < n; i++) parent[i] = i;
        for (int[] e : edges) {
            int a = find(parent, e[0]), b = find(parent, e[1]);
            if (a == b) return false;
            parent[a] = b;
        }
        return true;
    }

    private int find(int[] parent, int x) {
        if (parent[x] != x) parent[x] = find(parent, parent[x]);
        return parent[x];
    }
}
```

**基础语法与思想：** 无向图是树等价于连通且无环；`n-1` 条边加无环即可保证连通。

### 262. 行程和用户

**基础解法：** 过滤未被封禁的乘客和司机，再按日期统计取消率。

```sql
SELECT request_at AS Day,
       ROUND(SUM(status != 'completed') / COUNT(*), 2) AS `Cancellation Rate`
FROM Trips
WHERE request_at BETWEEN '2013-10-01' AND '2013-10-03'
  AND client_id NOT IN (SELECT users_id FROM Users WHERE banned = 'Yes')
  AND driver_id NOT IN (SELECT users_id FROM Users WHERE banned = 'Yes')
GROUP BY request_at;
```

**资深解法：** SQL 题不用 Java；核心是先过滤有效用户，再条件聚合。`SUM(boolean)` 在 MySQL 中可统计满足条件的行数。

### 263. 丑数

**基础解法：** 不断尝试除以 2、3、5，最后是否等于 1。

**资深解法：** 丑数定义只允许这些质因子，任何其他剩余因子都不合法。

```java
class Solution {
    public boolean isUgly(int n) {
        if (n <= 0) return false;
        for (int p : new int[]{2, 3, 5}) {
            while (n % p == 0) n /= p;
        }
        return n == 1;
    }
}
```

**基础语法与思想：** `while` 连续去除同一质因子；1 通常视为丑数。

### 264. 丑数 II

**基础解法：** 逐个判断每个整数是否为丑数，直到找到第 `n` 个。

**资深解法：** 三指针 DP，每次取 `2/3/5` 倍候选中的最小值。

```java
class Solution {
    public int nthUglyNumber(int n) {
        int[] dp = new int[n];
        dp[0] = 1;
        int a = 0, b = 0, c = 0;
        for (int i = 1; i < n; i++) {
            int next = Math.min(dp[a] * 2, Math.min(dp[b] * 3, dp[c] * 5));
            dp[i] = next;
            if (next == dp[a] * 2) a++;
            if (next == dp[b] * 3) b++;
            if (next == dp[c] * 5) c++;
        }
        return dp[n - 1];
    }
}
```

**基础语法与思想：** 三个 `if` 都要执行，避免重复丑数。

### 265. 粉刷房子 II

**基础解法：** 对每个房子颜色枚举上一间所有不同颜色，时间 `O(nk^2)`。

**资深解法：** 记录上一行最小值和次小值；当前颜色若等于最小值颜色，就只能接次小值。

```java
class Solution {
    public int minCostII(int[][] costs) {
        if (costs.length == 0) return 0;
        int k = costs[0].length;
        int min1 = 0, min2 = 0, idx1 = -1;
        for (int[] row : costs) {
            int nMin1 = Integer.MAX_VALUE, nMin2 = Integer.MAX_VALUE, nIdx1 = -1;
            for (int c = 0; c < k; c++) {
                int val = row[c] + (c == idx1 ? min2 : min1);
                if (val < nMin1) {
                    nMin2 = nMin1;
                    nMin1 = val;
                    nIdx1 = c;
                } else if (val < nMin2) nMin2 = val;
            }
            min1 = nMin1; min2 = nMin2; idx1 = nIdx1;
        }
        return min1;
    }
}
```

**基础语法与思想：** 最小/次小值优化把颜色转移从 `O(k^2)` 降到 `O(k)`。

### 266. 回文排列

**基础解法：** 统计每个字符频次，奇数频次最多只能有一个。

**资深解法：** 用集合切换奇偶状态，出现一次加入、再出现移除。

```java
class Solution {
    public boolean canPermutePalindrome(String s) {
        Set<Character> odd = new HashSet<>();
        for (char c : s.toCharArray()) {
            if (!odd.add(c)) odd.remove(c);
        }
        return odd.size() <= 1;
    }
}
```

**基础语法与思想：** 回文排列只关心字符频次奇偶性。

### 267. 回文排列 II

**基础解法：** 全排列后过滤回文，重复多且复杂度高。

**资深解法：** 统计字符，只对半边字符串做去重回溯，再镜像生成回文。

```java
class Solution {
    public List<String> generatePalindromes(String s) {
        int[] count = new int[128];
        for (char c : s.toCharArray()) count[c]++;
        int odd = 0;
        char mid = 0;
        StringBuilder half = new StringBuilder();
        for (int i = 0; i < count.length; i++) {
            if ((count[i] & 1) == 1) { odd++; mid = (char) i; }
            for (int j = 0; j < count[i] / 2; j++) half.append((char) i);
        }
        if (odd > 1) return new ArrayList<>();
        List<String> ans = new ArrayList<>();
        boolean[] used = new boolean[half.length()];
        char[] arr = half.toString().toCharArray();
        dfs(arr, used, new StringBuilder(), mid == 0 ? "" : String.valueOf(mid), ans);
        return ans;
    }

    private void dfs(char[] arr, boolean[] used, StringBuilder path, String mid, List<String> ans) {
        if (path.length() == arr.length) {
            ans.add(path + mid + path.reverse().toString());
            path.reverse();
            return;
        }
        for (int i = 0; i < arr.length; i++) {
            if (used[i] || (i > 0 && arr[i] == arr[i - 1] && !used[i - 1])) continue;
            used[i] = true;
            path.append(arr[i]);
            dfs(arr, used, path, mid, ans);
            path.deleteCharAt(path.length() - 1);
            used[i] = false;
        }
    }
}
```

**基础语法与思想：** 半边去重回溯即可生成全部不同回文；镜像时要恢复 `StringBuilder`。

### 268. 丢失的数字

**基础解法：** 排序后找第一个下标和值不同的位置。

**资深解法：** 利用等差和减去数组和，或异或下标和值。

```java
class Solution {
    public int missingNumber(int[] nums) {
        int n = nums.length, ans = n;
        for (int i = 0; i < n; i++) ans ^= i ^ nums[i];
        return ans;
    }
}
```

**基础语法与思想：** 相同数字异或为 0，剩下的就是缺失值。

### 269. 火星词典

**基础解法：** 比较相邻单词的第一个不同字符，建立字母先后约束，然后拓扑排序。

**资深解法：** 同时处理非法前缀情况，如 `"abc"` 排在 `"ab"` 前面应返回空串。

```java
class Solution {
    public String alienOrder(String[] words) {
        Map<Character, Set<Character>> graph = new HashMap<>();
        int[] indegree = new int[26];
        Arrays.fill(indegree, -1);
        for (String w : words) for (char c : w.toCharArray()) {
            graph.putIfAbsent(c, new HashSet<>());
            indegree[c - 'a'] = 0;
        }
        for (int i = 1; i < words.length; i++) {
            String a = words[i - 1], b = words[i];
            if (a.length() > b.length() && a.startsWith(b)) return "";
            int len = Math.min(a.length(), b.length());
            for (int j = 0; j < len; j++) {
                char x = a.charAt(j), y = b.charAt(j);
                if (x != y) {
                    if (graph.get(x).add(y)) indegree[y - 'a']++;
                    break;
                }
            }
        }
        Queue<Character> q = new ArrayDeque<>();
        for (int i = 0; i < 26; i++) if (indegree[i] == 0) q.offer((char) ('a' + i));
        StringBuilder ans = new StringBuilder();
        while (!q.isEmpty()) {
            char c = q.poll();
            ans.append(c);
            for (char next : graph.get(c)) if (--indegree[next - 'a'] == 0) q.offer(next);
        }
        return ans.length() == graph.size() ? ans.toString() : "";
    }
}
```

**基础语法与思想：** 相邻单词提供最小必要约束；字母序推断是有向图拓扑排序。

### 270. 最接近的二叉搜索树值

**基础解法：** 中序遍历所有节点，找与目标差最小的值。

**资深解法：** 利用 BST 搜索路径，边向下走边更新最接近值。

```java
class Solution {
    public int closestValue(TreeNode root, double target) {
        int ans = root.val;
        while (root != null) {
            if (Math.abs(root.val - target) < Math.abs(ans - target)) ans = root.val;
            root = target < root.val ? root.left : root.right;
        }
        return ans;
    }
}
```

**基础语法与思想：** BST 的搜索路径已经覆盖最可能接近目标的方向，空间 `O(1)`。
