# 数据结构算法学习手册（Java 版，0 基础）

## 1. 先掌握的 Java 语法

必须会：

- 变量与类型：`int long double boolean char String`
- 数组：`int[] a = new int[n];`
- 集合：
  - `ArrayList<Integer>`
  - `HashMap<Integer, Integer>`
  - `HashSet<Integer>`
  - `Deque<Integer> q = new ArrayDeque<>();`
- 控制流：`if/for/while`
- 函数：`public int f(int x) { ... }`
- 类与节点定义（链表/树）

常用函数（高频）：

- `map.put(k, v)`：写入键值
- `map.getOrDefault(k, 0)`：读键，不存在给默认值
- `set.contains(x)`：判重
- `list.add(x)`：追加
- `deque.offerLast(x) / pollFirst()`：队列
- `Arrays.sort(arr)`：排序
- `Collections.sort(list)`：集合排序

## 2. 模块一：数组/字符串/哈希

代表题：`1 3 14 49 209`

### 基础解法

- 暴力双循环，先保证正确性。
- 时间复杂度常是 `O(n^2)`。

### 熟练解法

- 用哈希表把查询降到 `O(1)` 平均。
- 双指针处理有序数组，窗口处理连续子数组。

### 资深解法

- 抽象“状态定义 + 转移”，写成可复用模板。
- 明确何时用 `HashMap`，何时用数组计数（字符题更快）。

示例（两数之和）：

```java
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        int need = target - nums[i];
        if (map.containsKey(need)) return new int[]{map.get(need), i};
        map.put(nums[i], i);
    }
    return new int[]{-1, -1};
}
```

语法与函数详解：

- `Map<Integer, Integer>`：泛型，键值都为 `Integer`。
- `containsKey`：判断键是否存在，返回 `boolean`。
- `return new int[]{...}`：返回匿名数组对象。

## 3. 模块二：链表/栈/队列

代表题：`2 19 21 206 232`

### 基础解法

- 画指针变化图，先写 `prev/curr/next` 三指针。

### 熟练解法

- 使用“虚拟头结点 `dummy`”统一边界。
- 括号、单调栈题先写入栈规则再写弹栈规则。

### 资深解法

- 链表题优先抽出通用子函数（反转区间、合并有序链表）。
- 栈队列题关注不变量（单调性、下标范围）。

## 4. 模块三：树/图/搜索

代表题：`94 102 104 199 200 207`

### 基础解法

- 树先递归：前中后序。
- 图先 DFS/BFS，先能遍历全图。

### 熟练解法

- 二叉树层序遍历用 `Queue<TreeNode>`。
- 图建邻接表：`List<List<Integer>> g`。

### 资深解法

- 明确遍历顺序与状态含义（入度、访问标记、父节点）。
- 拓扑排序、最短路、并查集按场景切换。

## 5. 模块四：二分/回溯/动态规划

代表题：`33 39 46 70 198 300`

### 基础解法

- 二分牢记区间语义：`[l, r]` 或 `[l, r)`，不要混用。
- 回溯先写“路径 + 选择列表 + 终止条件”。
- DP 先写 `dp[i]` 定义。

### 熟练解法

- 二分模板化：找左边界/右边界。
- 回溯做剪枝（排序后去重）。
- DP 写状态转移并压缩空间。

### 资深解法

- 能解释“为什么这个状态定义最小且完整”。
- 对比贪心与 DP 的可证明性。

## 6. 语法与函数专项（Java 必背）

### 数组与字符串

- `s.charAt(i)`：取字符
- `s.substring(l, r)`：左闭右开
- `StringBuilder sb = new StringBuilder();`
- `sb.append(x)` / `sb.toString()`

### 排序与比较器

- `Arrays.sort(arr)`：基本类型升序
- `Arrays.sort(a, (x, y) -> x[0] - y[0])`：二维数组按列排序

### 优先队列

- `PriorityQueue<Integer> min = new PriorityQueue<>();`
- 大根堆：`new PriorityQueue<>((a, b) -> b - a);`

## 7. 每题作答模板（固定流程）

1. 问题定义：输入/输出/约束
2. 基础解法：最直接正确方案
3. 熟练解法：主流最优方案
4. 资深解法：工程化与可扩展方案
5. 语法与函数详解：逐条解释 API
6. 复杂度：时间/空间
7. 边界用例：空输入、重复值、极值

## 8. Java 阶段目标

- 第 1 阶段：`Easy 120+`，独立 AC 率 `>= 85%`
- 第 2 阶段：`Medium 220+`，独立 AC 率 `>= 70%`
- 第 3 阶段：核心 Hard `30+`，能解释解法与取舍

