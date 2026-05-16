# Java 语法整理与索引

本索引记录刷题过程中反复出现的 Java 语法、标准库和易错边界。题目文档里写局部解释，这里做集中复盘。

## 数组

| 语法/API | 用法 | 关联题 |
|---|---|---|
| `nums.length` | 读取数组长度，循环边界常用。 | 1, 3, 4 |
| `new int[]{a, b}` | 直接创建并返回匿名数组。 | 1 |
| `int[] merged = new int[m + n]` | 创建固定长度数组保存归并结果。 | 4 |

## 集合

| 语法/API | 用法 | 关联题 |
|---|---|---|
| `Map<Integer, Integer> map = new HashMap<>()` | 哈希表，记录值到下标或频次的映射。 | 1 |
| `map.containsKey(key)` | 判断键是否存在。 | 1 |
| `map.get(key)` | 读取键对应的值。 | 1 |
| `HashMap<String, Boolean>` | 记忆化递归可用字符串键缓存状态。 | 10 |
| `Set<List<Integer>> seen = new HashSet<>()` | 保存已出现的组合，用于去重。 | 15, 18 |
| `new ArrayList<>(seen)` | 把集合转换为列表返回。 | 15, 18 |
| `Deque<Character> stack = new ArrayDeque<>()` | 用双端队列实现栈。 | 20 |
| `PriorityQueue<ListNode>` | 小根堆，动态取最小链表节点。 | 23 |
| `computeIfAbsent(key, k -> new ArrayList<>())` | 分组时按需创建列表。 | 49 |
| `Map<String, Boolean>` | 记忆化递归缓存字符串状态。 | 87 |
| `Queue<TreeNode> queue = new ArrayDeque<>()` | 队列实现二叉树层序遍历。 | 102, 103, 107, 111, 199 |
| `Map<Character, Integer>` | 统计窗口中字符频次。 | 159 |
| `Map<Long, Integer>` | 记录余数第一次出现的小数位置。 | 166 |
| `Map<Node, Node>` | 复制带随机指针链表时建立旧节点到新节点的映射。 | 138 |
| `LinkedHashMap<K,V>` | 结合哈希表与双向链表维护访问顺序，可实现 LRU。 | 146 |
| `Set.add(value)` | 返回是否成功新增，可直接用于查重。 | 217, 266 |
| `TreeMap.lastKey()` | 在有序映射中取当前最大键，适合维护动态最大高度。 | 218 |
| `PriorityQueue<Integer>` | 小根堆维护动态最小结束时间或第 k 大候选。 | 215, 253, 295 |
| `PriorityQueue<>(Collections.reverseOrder())` | 创建大根堆。 | 295 |
| `Queue<Iterator<Integer>>` | 队列轮转多个迭代器，实现交替输出。 | 281 |

## 字符串

| 语法/API | 用法 | 关联题 |
|---|---|---|
| `s.charAt(i)` | 读取第 `i` 个字符。 | 3, 5, 8, 10 |
| `s.substring(l, r)` | 截取左闭右开区间 `[l, r)`。 | 5, 7 |
| `StringBuilder` | 可变字符串，适合频繁追加或反转。 | 6, 7 |
| `Integer.toString(x)` | 把整数转为字符串。 | 7, 9 |
| `Integer.parseInt(str)` | 把字符串解析为 `int`，溢出会抛 `NumberFormatException`。 | 7 |
| `startsWith(prefix)` | 判断字符串是否以指定前缀开头。 | 14 |
| `isEmpty()` | 判断字符串或集合是否为空。 | 14, 20 |
| `String.replace(old, new)` | 替换字符串片段。 | 20 |
| `deleteCharAt(index)` | 删除 `StringBuilder` 指定位置字符，常用于回溯撤销。 | 17 |
| `split("/")` / `split("\\s+")` | 按路径分隔符或连续空白分割字符串。 | 58, 71 |
| `String.join(".", list)` | 用分隔符拼接字符串列表。 | 93 |
| `toCharArray()` / `new String(chars)` | 字符串和字符数组互转。 | 49, 51 |
| `split("\\.")` | 正则中点号要转义，才能按字面量 `.` 切分。 | 165 |
| `String.valueOf(num)` | 把数字转成字符串参与拼接比较。 | 179 |
| `StringBuilder.insert(index, value)` | 在指定位置插入字符，常用于补循环节括号。 | 166 |
| `startsWith(prefix, offset)` | 从指定位置判断是否匹配前缀。 | 291 |
| `StringBuilder.setLength(len)` | 回溯时恢复到进入递归前的长度。 | 257, 282 |
| `StringBuilder.reverse()` | 反转字符串，用于最短回文和镜像生成。 | 214, 267 |
| `Character.isDigit(c)` | 判断字符是否为数字。 | 224, 227 |

## 链表

| 语法/API | 用法 | 关联题 |
|---|---|---|
| `ListNode dummy = new ListNode(0)` | 虚拟头结点，统一处理头结点变化。 | 2 |
| `cur.next = new ListNode(value)` | 创建新节点并接到当前链表尾部。 | 2 |
| `node == null ? 0 : node.val` | 读取可空节点值的常见写法。 | 2 |
| `prev.next = prev.next.next` | 删除链表中 `prev` 后面的节点。 | 19 |
| `new ListNode(0, head)` | 创建带 `next` 的虚拟头结点。 | 82, 92 |
| `tail.next = head` | 成环或拼接链表时连接尾节点。 | 61 |
| `slow` / `fast` 双指针 | 快慢指针找中点、判环或定位入环点。 | 109, 141, 142, 143, 148 |
| `RandomNode random` | 随机指针链表要同时复制 `next` 与 `random` 引用关系。 | 138 |
| `node.val = node.next.val` | 在只给待删节点时复制后继值完成等效删除。 | 237 |

## 控制流与边界

| 语法/API | 用法 | 关联题 |
|---|---|---|
| `for` 双循环 | 最直观地枚举二元组、区间或中心。 | 1, 5 |
| 三重/四重循环 | 暴力枚举组合，常作为基础解法。 | 15, 16, 18 |
| `while (x != 0)` | 逐位处理整数。 | 7, 9 |
| `try/catch` | 捕获解析异常，处理溢出。 | 7 |
| `Integer.MAX_VALUE` / `Integer.MIN_VALUE` | `int` 上下界，常用于溢出判断。 | 4, 7, 8 |
| `continue` | 跳过本轮循环剩余逻辑，常用于排序后去重。 | 15, 18 |
| `break` | 直接结束当前循环，常用于剪枝。 | 15 |
| `for (char[] row : board)` | 遍历二维字符数组的每一行。 | 51 |
| `value++` | 使用当前值后自增，适合连续填数。 | 59 |
| `left + (right - left) / 2` | 安全计算二分中点，避免 `left + right` 溢出。 | 153, 154, 162 |
| `k %= n` | 处理轮转次数超过数组长度的情况。 | 189 |
| `Math.abs((long) x)` | 先转 `long` 再取绝对值，避免 `Integer.MIN_VALUE` 溢出。 | 166 |
| `for (int i = 0; i <= s.length(); i++)` | 多扫一轮哨兵运算符，统一处理表达式末尾数字。 | 227 |
| `new int[]{i, j}` | 快速创建坐标数组入队。 | 286 |

## 排序与查表

| 语法/API | 用法 | 关联题 |
|---|---|---|
| `Arrays.sort(nums)` | 对数组原地升序排序。 | 15, 16, 18 |
| `Arrays.asList(a, b, c)` | 快速生成固定元素列表。 | 15, 18 |
| `String[] map = {...}` | 用数组下标映射固定字符集合。 | 12, 17 |
| `switch (c)` | 把少量固定字符映射成不同值。 | 13 |
| `Integer.compare(a, b)` | 比较两个整数，避免直接相减潜在溢出。 | 56 |
| `Arrays.fill(arr, value)` | 用同一个值初始化数组或 DP 边界。 | 164, 174, 188 |
| `Arrays.sort(arr, (a, b) -> ...)` | 用 Lambda 自定义对象数组排序规则。 | 179 |
| `Collections.reverse(list)` | 反转列表，适合层序结果倒序输出。 | 107 |
| `Comparator.comparingInt(a -> a[0])` | 按数组的指定字段排序。 | 252, 253 |
| `List.of(...)` | 创建不可变小列表，适合递归基准返回固定候选。 | 247, 248 |
| `Arrays.asList(data.split(","))` | 把分割结果包装成列表再构造队列。 | 297 |

## 位运算

| 语法/API | 用法 | 关联题 |
|---|---|---|
| `value << 1` / `value >>= 1` | 左移乘 2，右移除 2。 | 29, 50 |
| `(exp & 1) == 1` | 判断二进制最低位是否为 1。 | 50 |
| `bit = available & -available` | 取最低位的 1。 | 52 |
| `i ^ (i >> 1)` | 生成格雷码。 | 89 |
| `n >>>= 1` | 无符号右移，适合逐位处理二进制表示。 | 190 |
| `n &= n - 1` | 删除最低位的 1。 | 191 |
| `(char) ('A' + x)` | 把 0-25 的数字映射成大写字母。 | 168 |
| `xor & -xor` | 取最低位的 1，用于按差异位分组。 | 260 |
| `(n & (n - 1)) == 0` | 判断正数是否只有一个二进制 1。 | 231 |

## 树与矩阵

| 语法/API | 用法 | 关联题 |
|---|---|---|
| `TreeNode left/right` | 二叉树左右子节点引用。 | 94-100 |
| `boolean[][] rows` | 二维标记表，可记录行/列/宫状态。 | 36, 37 |
| `matrix[idx / n][idx % n]` | 一维下标映射二维坐标。 | 74 |
| `Node next` | 完美二叉树/普通二叉树中连接同层右侧节点。 | 116, 117 |
| `pushLeft(root)` | 把 BST 当前路径的左链压栈，支持惰性中序迭代。 | 173 |
| `grid[i][j] = '0'` | 网格 DFS 中原地标记访问状态。 | 200 |
| `TreeNode == p` | 树节点身份比较要用引用相等。 | 236 |
| `Queue<int[]>` | BFS 中保存二维坐标。 | 286 |
| `board[i][j] %= 2` | 原地状态编码后落盘到新状态。 | 289 |

## 动态规划

| 语法/API | 用法 | 关联题 |
|---|---|---|
| `boolean[][] dp = new boolean[m + 1][n + 1]` | 二维 DP 表，记录两个前缀或后缀的关系。 | 10 |
| `Boolean[][] memo` | 三态缓存：`null` 表示未计算，`true/false` 表示结果。 | 10 |
| `int[] dp = new int[n + 1]` | 一维滚动数组压缩路径或序列 DP。 | 120, 198 |
| `int[] buy/sell` | 股票状态机中分别保存持股和不持股最大收益。 | 123, 188 |
| `Integer.MAX_VALUE / 2` | 初始化不可达状态，避免后续加减时溢出。 | 174 |
| `int[][] dp = new int[m + 1][n + 1]` | 多一圈哨兵边界简化二维 DP。 | 221 |
| `tails[l] = x` | LIS 贪心数组用二分位置更新最小结尾。 | 300 |
| `same/diff` | 滚动维护最后两项关系的 DP 状态。 | 276 |

## SQL 与 Shell

| 语法/API | 用法 | 关联题 |
|---|---|---|
| `LEFT JOIN ... ON ...` | 保留左表记录并补充右表匹配信息。 | 175, 183 |
| `GROUP BY ... HAVING COUNT(*)` | 对分组后的统计结果做过滤。 | 182 |
| `DENSE_RANK() OVER (...)` | 不跳号排名，适合并列排名题。 | 178, 185 |
| `LIMIT 1 OFFSET n` | 取排序后的第 `n + 1` 条记录。 | 176, 177 |
| `DELETE p1 FROM ... JOIN ...` | MySQL 自连接删除重复记录。 | 196 |
| `tr | sort | uniq -c | awk` | Shell 管道完成词频统计与格式化输出。 | 192 |
| `grep -E` | 使用扩展正则筛选合法文本行。 | 193 |
| `awk 'NR == 10'` | 按行号处理文本。 | 195 |
| `SUM(condition)` | MySQL 中布尔表达式可参与聚合统计满足条件的行数。 | 262 |
| `ROUND(value, 2)` | SQL 中保留两位小数。 | 262 |

## 易错点

| 场景 | 处理方式 | 关联题 |
|---|---|---|
| 整数反转溢出 | 在 `ans * 10 + digit` 之前判断边界。 | 7 |
| `atoi` 正负边界不对称 | 正数最大是 `2147483647`，负数最小是 `-2147483648`。 | 8 |
| 偶数长度回文 | 中心为 `(i, i + 1)`，不能只检查单点中心。 | 5 |
| 正则 `*` | 只能作用于前一个字符，且可表示 0 次。 | 10 |
| 版本号切分 | `.` 是正则元字符，按字面量切分必须写成 `"\\."`。 | 165 |
| 分数转小数溢出 | `Integer.MIN_VALUE` 取绝对值前必须转成 `long`。 | 166 |
| Read4 多次调用 | 多读出的字符要保存在对象字段中，不能在本次调用结束时丢弃。 | 158 |
| 前导零数字 | 表达式回溯中 `"05"` 不能作为合法数字，只允许单个 `"0"`。 | 282 |
| `HashMap` 双射 | 只做单向映射会漏掉两个键映射到同一个值的冲突。 | 205, 290, 291 |
| Trie 通配符 | `.` 需要枚举所有非空子节点，不能按普通字符下标访问。 | 211 |
