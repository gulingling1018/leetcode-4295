# LeetCode 题目合集 Part 13

## 361. 轰炸敌人 (Medium)

暂无内容描述。

### Java 解法补充

#### 基础解法：逐格四向扫描

算法思想：只在空格 `'0'` 上放炸弹，向上下左右四个方向一直扫到墙 `'W'`，统计能消灭的敌人 `'E'`，维护最大值。这个写法最直观，适合理解题意。

```java
class Solution {
    public int maxKilledEnemies(char[][] grid) {
        if (grid.length == 0 || grid[0].length == 0) return 0;
        int m = grid.length;
        int n = grid[0].length;
        int ans = 0;
        int[][] dirs = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] != '0') continue;
                int count = 0;
                for (int[] dir : dirs) {
                    int x = i + dir[0];
                    int y = j + dir[1];
                    while (x >= 0 && x < m && y >= 0 && y < n && grid[x][y] != 'W') {
                        if (grid[x][y] == 'E') count++;
                        x += dir[0];
                        y += dir[1];
                    }
                }
                ans = Math.max(ans, count);
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(mn(m+n))`，空间 `O(1)`。

#### 资深解法：按墙分段复用统计

算法思想：同一段没有墙隔开的行/列中，敌人数对该段所有空格都相同。扫描矩阵时，只在行段或列段起点重新统计，避免重复四向扫描。

```java
class Solution {
    public int maxKilledEnemies(char[][] grid) {
        if (grid.length == 0 || grid[0].length == 0) return 0;
        int m = grid.length;
        int n = grid[0].length;
        int ans = 0;
        int rowHits = 0;
        int[] colHits = new int[n];

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (j == 0 || grid[i][j - 1] == 'W') {
                    rowHits = 0;
                    for (int k = j; k < n && grid[i][k] != 'W'; k++) {
                        if (grid[i][k] == 'E') rowHits++;
                    }
                }
                if (i == 0 || grid[i - 1][j] == 'W') {
                    colHits[j] = 0;
                    for (int k = i; k < m && grid[k][j] != 'W'; k++) {
                        if (grid[k][j] == 'E') colHits[j]++;
                    }
                }
                if (grid[i][j] == '0') {
                    ans = Math.max(ans, rowHits + colHits[j]);
                }
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(mn)`，空间 `O(n)`。

#### 基础语法与算法思想

- `char[][]` 表示二维字符网格。
- `continue` 可以跳过不适合作为炸弹位置的格子。
- 核心思想：网格题基础版按方向模拟；优化版找可复用的行段、列段状态。

---

## 362. 敲击计数器 (Medium)

暂无内容描述。

### Java 解法补充

#### 基础解法：保存所有时间戳

算法思想：每次 `hit` 把时间戳加入列表；查询时遍历所有记录，统计 `[timestamp - 299, timestamp]` 范围内的点击次数。

```java
import java.util.ArrayList;
import java.util.List;

class HitCounter {
    private final List<Integer> hits = new ArrayList<>();

    public void hit(int timestamp) {
        hits.add(timestamp);
    }

    public int getHits(int timestamp) {
        int count = 0;
        for (int time : hits) {
            if (time > timestamp - 300 && time <= timestamp) {
                count++;
            }
        }
        return count;
    }
}
```

复杂度：`hit` 时间 `O(1)`，`getHits` 时间 `O(n)`，空间 `O(n)`。

#### 资深解法：队列清理过期点击

算法思想：时间戳按调用顺序递增，用队列保存最近点击。查询或写入时把超过 300 秒窗口的旧数据从队头弹出，队列大小就是答案。

```java
import java.util.ArrayDeque;
import java.util.Queue;

class HitCounter {
    private final Queue<Integer> queue = new ArrayDeque<>();

    public void hit(int timestamp) {
        queue.offer(timestamp);
    }

    public int getHits(int timestamp) {
        while (!queue.isEmpty() && queue.peek() <= timestamp - 300) {
            queue.poll();
        }
        return queue.size();
    }
}
```

复杂度：均摊时间 `O(1)`，空间 `O(k)`，`k` 为最近 300 秒内点击数。

#### 基础语法与算法思想

- `Queue<Integer>` 表示先进先出的队列。
- `offer` 入队，`peek` 查看队头，`poll` 出队。
- 核心思想：只关心最近窗口时，队列天然适合删除最老数据。

---

## 363. 矩形区域不超过 K 的最大数值和 (Hard)

给你一个  `m x n`  的矩阵  `matrix`  和一个整数  `k`  ，找出并返回矩阵内部矩形区域的不超过  `k`  的最大数值和。
题目数据保证总会存在一个数值和不超过  `k`  的矩形区域。
 
 **示例 1：** 

```text
输入：matrix = [[1,0,1],[0,-2,3]], k = 2
输出：2
解释：蓝色边框圈出来的矩形区域 [[0, 1], [-2, 3]] 的数值和是 2，且 2 是不超过 k 的最大数字（k = 2）。
```

 **示例 2：** 

```text
输入：matrix = [[2,2,-1]], k = 3
输出：3
```

 
 **提示：** 

 `m == matrix.length` 
 `n == matrix[i].length` 
 `1 <= m, n <= 100` 
 `-100 <= matrix[i][j] <= 100` 
 `-105 <= k <= 105` 

 
 **进阶：** 如果行数远大于列数，该如何设计解决方案？

### Java 解法补充

#### 基础解法：二维前缀和枚举矩形

算法思想：先构造二维前缀和，之后枚举矩形的上下左右边界，用 `O(1)` 算出矩形和，挑选不超过 `k` 的最大值。

```java
class Solution {
    public int maxSumSubmatrix(int[][] matrix, int k) {
        int m = matrix.length;
        int n = matrix[0].length;
        int[][] prefix = new int[m + 1][n + 1];
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                prefix[i][j] = matrix[i - 1][j - 1] + prefix[i - 1][j]
                        + prefix[i][j - 1] - prefix[i - 1][j - 1];
            }
        }

        int ans = Integer.MIN_VALUE;
        for (int top = 0; top < m; top++) {
            for (int bottom = top; bottom < m; bottom++) {
                for (int left = 0; left < n; left++) {
                    for (int right = left; right < n; right++) {
                        int sum = prefix[bottom + 1][right + 1] - prefix[top][right + 1]
                                - prefix[bottom + 1][left] + prefix[top][left];
                        if (sum <= k) ans = Math.max(ans, sum);
                    }
                }
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(m^2n^2)`，空间 `O(mn)`。

#### 资深解法：压缩行加有序集合

算法思想：固定上下边界，把这几行压缩成一维列和。问题转成“一维数组中不超过 `k` 的最大子数组和”，用 `TreeSet` 保存前缀和并查找不小于 `prefix - k` 的最小前缀。

```java
import java.util.TreeSet;

class Solution {
    public int maxSumSubmatrix(int[][] matrix, int k) {
        int m = matrix.length;
        int n = matrix[0].length;
        int ans = Integer.MIN_VALUE;

        for (int top = 0; top < m; top++) {
            int[] cols = new int[n];
            for (int bottom = top; bottom < m; bottom++) {
                for (int c = 0; c < n; c++) {
                    cols[c] += matrix[bottom][c];
                }
                TreeSet<Integer> prefixes = new TreeSet<>();
                prefixes.add(0);
                int prefix = 0;
                for (int value : cols) {
                    prefix += value;
                    Integer need = prefixes.ceiling(prefix - k);
                    if (need != null) {
                        ans = Math.max(ans, prefix - need);
                    }
                    prefixes.add(prefix);
                }
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(m^2 n log n)`，空间 `O(n)`。如果行数远大于列数，可以交换枚举方向，固定左右边界压缩行。

#### 基础语法与算法思想

- `prefix[i][j]` 常表示左上角到当前位置之前的矩形和。
- `TreeSet.ceiling(x)` 返回集合中大于等于 `x` 的最小值。
- 核心思想：二维求和题常先压缩一个维度，再套一维最优子数组算法。

---

## 364. 嵌套列表加权和 II (Medium)

暂无内容描述。

### Java 解法补充

#### 基础解法：先求最大深度再 DFS

算法思想：权重从底层开始最大，等价于节点权重为 `maxDepth - depth + 1`。先递归求最大深度，再递归累加加权和。

```java
import java.util.List;

class Solution {
    public int depthSumInverse(List<NestedInteger> nestedList) {
        int maxDepth = maxDepth(nestedList, 1);
        return sum(nestedList, 1, maxDepth);
    }

    private int maxDepth(List<NestedInteger> list, int depth) {
        int ans = depth;
        for (NestedInteger item : list) {
            if (!item.isInteger()) {
                ans = Math.max(ans, maxDepth(item.getList(), depth + 1));
            }
        }
        return ans;
    }

    private int sum(List<NestedInteger> list, int depth, int maxDepth) {
        int ans = 0;
        for (NestedInteger item : list) {
            if (item.isInteger()) {
                ans += item.getInteger() * (maxDepth - depth + 1);
            } else {
                ans += sum(item.getList(), depth + 1, maxDepth);
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(d)`，`d` 为嵌套深度。

#### 资深解法：层序累加未加权和

算法思想：从外到内 BFS。每一层把当前整数加到 `levelSum`，再把 `levelSum` 累加到答案。外层整数会被累加更多次，正好形成反向权重。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public int depthSumInverse(List<NestedInteger> nestedList) {
        int levelSum = 0;
        int ans = 0;
        List<NestedInteger> level = nestedList;

        while (!level.isEmpty()) {
            List<NestedInteger> next = new ArrayList<>();
            for (NestedInteger item : level) {
                if (item.isInteger()) {
                    levelSum += item.getInteger();
                } else {
                    next.addAll(item.getList());
                }
            }
            ans += levelSum;
            level = next;
        }
        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(w)`，`w` 为某一层最大元素数。

#### 基础语法与算法思想

- `NestedInteger` 是题目提供的接口，整数和列表要分开判断。
- `addAll` 可以把一个列表中的元素批量加入另一个列表。
- 核心思想：反向权重题可以用“每下沉一层，之前的整数再加一次”来避免先求深度。

---

## 365. 水壶问题 (Medium)

有两个水壶，容量分别为  `x`  和  `y`  升。水的供应是无限的。确定是否有可能使用这两个壶准确得到  `target`  升。
你可以：

装满任意一个水壶
清空任意一个水壶
将水从一个水壶倒入另一个水壶，直到接水壶已满，或倒水壶已空。

 
 **示例 1:**  

```text
输入: x = 3,y = 5,target = 4
输出: true
解释：
按照以下步骤操作，以达到总共 4 升水：
1. 装满 5 升的水壶(0, 5)。
2. 把 5 升的水壶倒进 3 升的水壶，留下 2 升(3, 2)。
3. 倒空 3 升的水壶(0, 2)。
4. 把 2 升水从 5 升的水壶转移到 3 升的水壶(2, 0)。
5. 再次加满 5 升的水壶(2, 5)。
6. 从 5 升的水壶向 3 升的水壶倒水直到 3 升的水壶倒满。5 升的水壶里留下了 4 升水(3, 4)。
7. 倒空 3 升的水壶。现在，5 升的水壶里正好有 4 升水(0, 4)。
参考：来自著名的 "Die Hard"
```

 **示例 2:** 

```text
输入: x = 2, y = 6, target = 5
输出: false
```

 **示例 3:** 

```text
输入: x = 1, y = 2, target = 3
输出: true
解释：同时倒满两个水壶。现在两个水壶中水的总量等于 3。
```

 
 **提示:** 

 `1 <= x, y, target <= 103` 

### Java 解法补充

#### 基础解法：BFS 搜索水壶状态

算法思想：状态用 `(a, b)` 表示两个水壶当前水量。每次从一个状态扩展装满、倒空、互倒六种操作，只要出现 `a == target`、`b == target` 或 `a + b == target` 就成功。

```java
import java.util.ArrayDeque;
import java.util.HashSet;
import java.util.Queue;
import java.util.Set;

class Solution {
    public boolean canMeasureWater(int x, int y, int target) {
        Queue<int[]> queue = new ArrayDeque<>();
        Set<String> seen = new HashSet<>();
        queue.offer(new int[]{0, 0});

        while (!queue.isEmpty()) {
            int[] cur = queue.poll();
            int a = cur[0];
            int b = cur[1];
            if (a == target || b == target || a + b == target) return true;
            String key = a + "," + b;
            if (!seen.add(key)) continue;

            int pourToB = Math.min(a, y - b);
            int pourToA = Math.min(b, x - a);
            queue.offer(new int[]{x, b});
            queue.offer(new int[]{a, y});
            queue.offer(new int[]{0, b});
            queue.offer(new int[]{a, 0});
            queue.offer(new int[]{a - pourToB, b + pourToB});
            queue.offer(new int[]{a + pourToA, b - pourToA});
        }
        return false;
    }
}
```

复杂度：时间和空间 `O(xy)`。

#### 资深解法：贝祖定理

算法思想：通过装满、倒空、互倒两个水壶，能量出的水量一定是 `gcd(x, y)` 的倍数；同时目标不能超过总容量。因此只需判断 `target <= x + y` 且 `target` 能被最大公约数整除。

```java
class Solution {
    public boolean canMeasureWater(int x, int y, int target) {
        if (target > x + y) return false;
        if (target == 0) return true;
        return target % gcd(x, y) == 0;
    }

    private int gcd(int a, int b) {
        while (b != 0) {
            int t = a % b;
            a = b;
            b = t;
        }
        return a;
    }
}
```

复杂度：时间 `O(log min(x, y))`，空间 `O(1)`。

#### 基础语法与算法思想

- `Set.add` 返回是否真的插入成功，可用于判重。
- `Math.min` 用来计算互倒时实际能倒多少水。
- 核心思想：基础版把操作当图搜索；资深版用数论直接刻画可达水量。

---

## 366. 寻找二叉树的叶子节点 (Medium)

暂无内容描述。

### Java 解法补充

#### 基础解法：反复删除叶子

算法思想：每一轮 DFS 找到当前叶子节点，把它们加入本轮答案，并在父节点处断开叶子引用。重复直到根节点也被删除。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<List<Integer>> findLeaves(TreeNode root) {
        List<List<Integer>> ans = new ArrayList<>();
        while (root != null) {
            List<Integer> level = new ArrayList<>();
            root = removeLeaves(root, level);
            ans.add(level);
        }
        return ans;
    }

    private TreeNode removeLeaves(TreeNode node, List<Integer> level) {
        if (node == null) return null;
        if (node.left == null && node.right == null) {
            level.add(node.val);
            return null;
        }
        node.left = removeLeaves(node.left, level);
        node.right = removeLeaves(node.right, level);
        return node;
    }
}
```

复杂度：最坏时间 `O(n^2)`，空间 `O(h)`。

#### 资深解法：按节点高度分组

算法思想：叶子节点高度为 0，父节点高度为左右子树最大高度加 1。同一高度的节点会在同一轮被删除，一次 DFS 即可完成分组。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<List<Integer>> findLeaves(TreeNode root) {
        List<List<Integer>> ans = new ArrayList<>();
        height(root, ans);
        return ans;
    }

    private int height(TreeNode node, List<List<Integer>> ans) {
        if (node == null) return -1;
        int h = Math.max(height(node.left, ans), height(node.right, ans)) + 1;
        if (h == ans.size()) {
            ans.add(new ArrayList<>());
        }
        ans.get(h).add(node.val);
        return h;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(h)`，答案空间不计。

#### 基础语法与算法思想

- 返回 `null` 可以表示“这个子树已经被删除”。
- `ans.get(h)` 取出第 `h` 轮对应的列表。
- 核心思想：树的删除轮次等价于节点到最近叶子的高度。

---

## 367. 有效的完全平方数 (Easy)

给你一个正整数  `num`  。如果  `num`  是一个完全平方数，则返回  `true`  ，否则返回  `false`  。
 **完全平方数**  是一个可以写成某个整数的平方的整数。换句话说，它可以写成某个整数和自身的乘积。
不能使用任何内置的库函数，如   `sqrt`  。
 
 **示例 1：** 

```text
输入：num = 16
输出：true
解释：返回 true ，因为 4 * 4 = 16 且 4 是一个整数。
```

 **示例 2：** 

```text
输入：num = 14
输出：false
解释：返回 false ，因为 3.742 * 3.742 = 14 但 3.742 不是一个整数。
```

 
 **提示：** 

 `1 <= num <= 231 - 1`

### Java 解法补充

#### 基础解法：从 1 开始试平方

算法思想：枚举正整数 `i`，计算 `i * i`。如果等于 `num` 就返回 `true`，超过 `num` 后返回 `false`。

```java
class Solution {
    public boolean isPerfectSquare(int num) {
        long i = 1;
        while (i * i <= num) {
            if (i * i == num) return true;
            i++;
        }
        return false;
    }
}
```

复杂度：时间 `O(sqrt(num))`，空间 `O(1)`。

#### 资深解法：二分查找平方根

算法思想：完全平方数的平方根在 `[1, num]` 内。用二分查找中点平方，注意用 `long` 防止 `mid * mid` 溢出。

```java
class Solution {
    public boolean isPerfectSquare(int num) {
        int left = 1;
        int right = num;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            long square = (long) mid * mid;
            if (square == num) return true;
            if (square < num) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return false;
    }
}
```

复杂度：时间 `O(log num)`，空间 `O(1)`。

#### 基础语法与算法思想

- `(long) mid * mid` 会先把 `mid` 转成长整型再乘。
- `left + (right - left) / 2` 是更稳妥的二分中点写法。
- 核心思想：答案具有单调性时，优先考虑二分。

---

## 368. 最大整除子集 (Medium)

给你一个由  **无重复**  正整数组成的集合  `nums`  ，请你找出并返回其中最大的整除子集  `answer`  ，子集中每一元素对  `(answer[i], answer[j])`  都应当满足：

 `answer[i] % answer[j] == 0`  ，或
 `answer[j] % answer[i] == 0` 

如果存在多个有效解子集，返回其中任何一个均可。
 
 **示例 1：** 

```text
输入：nums = [1,2,3]
输出：[1,2]
解释：[1,3] 也会被视为正确答案。
```

 **示例 2：** 

```text
输入：nums = [1,2,4,8]
输出：[1,2,4,8]
```

 
 **提示：** 

 `1 <= nums.length <= 1000` 
 `1 <= nums[i] <= 2 * 109` 
 `nums`  中的所有整数  **互不相同**

### Java 解法补充

#### 基础解法：回溯枚举子集

算法思想：先排序，再递归决定每个数选或不选。因为已排序，只要当前数能被路径最后一个数整除，就可以加入路径。这个方法直观但会超时。

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

class Solution {
    private List<Integer> best = new ArrayList<>();

    public List<Integer> largestDivisibleSubset(int[] nums) {
        Arrays.sort(nums);
        backtrack(nums, 0, new ArrayList<>());
        return best;
    }

    private void backtrack(int[] nums, int index, List<Integer> path) {
        if (index == nums.length) {
            if (path.size() > best.size()) best = new ArrayList<>(path);
            return;
        }
        if (path.isEmpty() || nums[index] % path.get(path.size() - 1) == 0) {
            path.add(nums[index]);
            backtrack(nums, index + 1, path);
            path.remove(path.size() - 1);
        }
        backtrack(nums, index + 1, path);
    }
}
```

复杂度：最坏时间 `O(2^n)`，空间 `O(n)`。

#### 资深解法：排序加动态规划

算法思想：排序后，如果 `nums[i] % nums[j] == 0`，以 `j` 结尾的整除子集可以接上 `i`。用 `dp[i]` 记录以 `i` 结尾的最大长度，用 `prev[i]` 还原路径。

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

class Solution {
    public List<Integer> largestDivisibleSubset(int[] nums) {
        Arrays.sort(nums);
        int n = nums.length;
        int[] dp = new int[n];
        int[] prev = new int[n];
        Arrays.fill(dp, 1);
        Arrays.fill(prev, -1);

        int best = 0;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] % nums[j] == 0 && dp[j] + 1 > dp[i]) {
                    dp[i] = dp[j] + 1;
                    prev[i] = j;
                }
            }
            if (dp[i] > dp[best]) best = i;
        }

        List<Integer> ans = new ArrayList<>();
        while (best != -1) {
            ans.add(nums[best]);
            best = prev[best];
        }
        return ans;
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(n)`。

#### 基础语法与算法思想

- `Arrays.sort` 让整除关系只需要从小到大检查。
- `prev` 数组常用于从 DP 状态中还原具体方案。
- 核心思想：排序后，“两两可整除”的子集可以转成最长链问题。

---

## 369. 给单链表加一 (Medium)

暂无内容描述。

### Java 解法补充

#### 基础解法：反转链表后加一

算法思想：单链表从高位到低位存储，直接加一不方便。先反转链表，从低位开始处理进位，再反转回来。

```java
class Solution {
    public ListNode plusOne(ListNode head) {
        head = reverse(head);
        ListNode cur = head;
        int carry = 1;
        while (cur != null && carry > 0) {
            int sum = cur.val + carry;
            cur.val = sum % 10;
            carry = sum / 10;
            if (cur.next == null && carry > 0) {
                cur.next = new ListNode(0);
            }
            cur = cur.next;
        }
        return reverse(head);
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

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 资深解法：找到最后一个非 9 节点

算法思想：使用哨兵节点处理 `999` 变 `1000` 的情况。找到最后一个值不为 9 的节点，让它加一，并把它后面的所有节点置为 0。

```java
class Solution {
    public ListNode plusOne(ListNode head) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode lastNotNine = dummy;

        for (ListNode cur = head; cur != null; cur = cur.next) {
            if (cur.val != 9) {
                lastNotNine = cur;
            }
        }

        lastNotNine.val++;
        for (ListNode cur = lastNotNine.next; cur != null; cur = cur.next) {
            cur.val = 0;
        }
        return dummy.val == 0 ? dummy.next : dummy;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 哨兵节点可以统一处理头节点发生变化的情况。
- 链表反转常用 `prev / cur / next` 三指针。
- 核心思想：加一只会影响最后一段连续的 9。

---

## 370. 区间加法 (Medium)

暂无内容描述。

### Java 解法补充

#### 基础解法：逐个区间直接加

算法思想：每条更新 `[start, end, inc]` 都直接遍历区间，把数组对应位置加上 `inc`。

```java
class Solution {
    public int[] getModifiedArray(int length, int[][] updates) {
        int[] ans = new int[length];
        for (int[] update : updates) {
            int start = update[0];
            int end = update[1];
            int inc = update[2];
            for (int i = start; i <= end; i++) {
                ans[i] += inc;
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(length * updates.length)`，空间 `O(length)`。

#### 资深解法：差分数组

算法思想：区间 `[l, r]` 加 `inc`，只需要在差分数组里 `diff[l] += inc`、`diff[r + 1] -= inc`。最后做一次前缀和得到真实数组。

```java
class Solution {
    public int[] getModifiedArray(int length, int[][] updates) {
        int[] diff = new int[length];
        for (int[] update : updates) {
            int start = update[0];
            int end = update[1];
            int inc = update[2];
            diff[start] += inc;
            if (end + 1 < length) {
                diff[end + 1] -= inc;
            }
        }

        for (int i = 1; i < length; i++) {
            diff[i] += diff[i - 1];
        }
        return diff;
    }
}
```

复杂度：时间 `O(length + updates.length)`，空间 `O(length)`。

#### 基础语法与算法思想

- 二维数组 `updates` 中每一行保存一次操作。
- 差分数组记录“变化量”，前缀和还原“真实值”。
- 核心思想：大量区间加法时，不逐点更新，而是在边界打标记。

---

## 371. 两整数之和 (Medium)

给你两个整数  `a`  和  `b`  ， **不使用** 运算符  `+`  和  `-`  ​​​​​​​，计算并返回两整数之和。
 
 **示例 1：** 

```text
输入：a = 1, b = 2
输出：3
```

 **示例 2：** 

```text
输入：a = 2, b = 3
输出：5
```

 
 **提示：** 

 `-1000 <= a, b <= 1000`

### Java 解法补充

#### 基础解法：拆成无进位和进位

算法思想：异或 `a ^ b` 可以得到不考虑进位的和，与运算后左移 `(a & b) << 1` 可以得到进位。不断重复，直到进位为 0。

```java
class Solution {
    public int getSum(int a, int b) {
        while (b != 0) {
            int sum = a ^ b;
            int carry = (a & b) << 1;
            a = sum;
            b = carry;
        }
        return a;
    }
}
```

复杂度：时间 `O(1)`，空间 `O(1)`，`int` 固定 32 位。

#### 资深解法：递归表达位加法

算法思想：递归版本把“当前无进位和”和“下一轮进位”作为下一次参数，直到没有进位。逻辑更贴近位运算公式。

```java
class Solution {
    public int getSum(int a, int b) {
        if (b == 0) return a;
        return getSum(a ^ b, (a & b) << 1);
    }
}
```

复杂度：时间 `O(1)`，空间 `O(1)`，递归深度受 32 位限制。

#### 基础语法与算法思想

- `^` 是按位异或，表示不同为 1。
- `&` 是按位与，可找出需要进位的位置。
- 核心思想：整数加法可以拆成“无进位相加”和“进位继续加”。

---

## 372. 超级次方 (Medium)

你的任务是计算  `ab`  对  `1337`  取模， `a`  是一个正整数， `b`  是一个非常大的正整数且会以数组形式给出。
 
 **示例 1：** 

```text
输入：a = 2, b = [3]
输出：8
```

 **示例 2：** 

```text
输入：a = 2, b = [1,0]
输出：1024
```

 **示例 3：** 

```text
输入：a = 1, b = [4,3,3,8,5,2]
输出：1
```

 **示例 4：** 

```text
输入：a = 2147483647, b = [2,0,0]
输出：1198
```

 
 **提示：** 

 `1 <= a <= 231 - 1` 
 `1 <= b.length <= 2000` 
 `0 <= b[i] <= 9` 
 `b`  不含前导 0

### Java 解法补充

#### 基础解法：按十进制指数逐步滚动

算法思想：数组 `b` 是十进制指数。扫描每一位时，旧结果要整体 10 次方，再乘上 `a^digit`，每一步都对 1337 取模。

```java
class Solution {
    private static final int MOD = 1337;

    public int superPow(int a, int[] b) {
        int ans = 1;
        a %= MOD;
        for (int digit : b) {
            ans = pow(ans, 10) * pow(a, digit) % MOD;
        }
        return ans;
    }

    private int pow(int base, int exp) {
        int ans = 1;
        for (int i = 0; i < exp; i++) {
            ans = ans * base % MOD;
        }
        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 资深解法：快速幂封装

算法思想：仍使用十进制滚动公式，但 `pow` 使用二进制快速幂。这样实现更通用，适合指数范围变大或复用到其他模幂场景。

```java
class Solution {
    private static final int MOD = 1337;

    public int superPow(int a, int[] b) {
        int ans = 1;
        int base = a % MOD;
        for (int digit : b) {
            ans = modPow(ans, 10) * modPow(base, digit) % MOD;
        }
        return ans;
    }

    private int modPow(int base, int exp) {
        int ans = 1;
        while (exp > 0) {
            if ((exp & 1) == 1) {
                ans = ans * base % MOD;
            }
            base = base * base % MOD;
            exp >>= 1;
        }
        return ans;
    }
}
```

复杂度：时间 `O(n log 10)`，空间 `O(1)`。

#### 基础语法与算法思想

- `(exp & 1) == 1` 判断指数当前最低位是否为 1。
- 取模乘法满足 `(x * y) % mod` 可分步计算。
- 核心思想：大指数不能还原成整数，应在扫描指数位时同步维护模结果。

---

## 373. 查找和最小的 K 对数字 (Medium)

给定两个以  **非递减顺序排列**  的整数数组  `nums1`  和 ****  `nums2`  **** , 以及一个整数  `k`  **** 。
定义一对值  `(u,v)` ，其中第一个元素来自  `nums1` ，第二个元素来自  `nums2`  **** 。
请找到和最小的  `k`  个数对  `(u1,v1)` ,  `(u2,v2)`   ...   `(uk,vk)`  。
 
 **示例 1:** 

```text
输入: nums1 = [1,7,11], nums2 = [2,4,6], k = 3
输出: [[1,2],[1,4],[1,6]]
解释: 返回序列中的前 3 对数：
     [1,2],[1,4],[1,6],[7,2],[7,4],[11,2],[7,6],[11,4],[11,6]
```

 **示例 2:** 

```text
输入: nums1 = [1,1,2], nums2 = [1,2,3], k = 2
输出: [[1,1],[1,1]]
解释: 返回序列中的前 2 对数：
     [1,1],[1,1],[1,2],[2,1],[1,2],[2,2],[1,3],[1,3],[2,3]
```

 
 **提示:** 

 `1 <= nums1.length, nums2.length <= 105` 
 `-109 <= nums1[i], nums2[i] <= 109` 
 `nums1`  和  `nums2`  均为  **升序排列** 
 `1 <= k <= 104` 
 `k <= nums1.length * nums2.length`

### Java 解法补充

#### 基础解法：生成所有数对后排序

算法思想：枚举 `nums1` 和 `nums2` 的所有数对，按两数之和排序，再取前 `k` 个。

```java
import java.util.ArrayList;
import java.util.Comparator;
import java.util.List;

class Solution {
    public List<List<Integer>> kSmallestPairs(int[] nums1, int[] nums2, int k) {
        List<List<Integer>> pairs = new ArrayList<>();
        for (int a : nums1) {
            for (int b : nums2) {
                List<Integer> pair = new ArrayList<>();
                pair.add(a);
                pair.add(b);
                pairs.add(pair);
            }
        }
        pairs.sort(Comparator.comparingInt(p -> p.get(0) + p.get(1)));
        return pairs.subList(0, Math.min(k, pairs.size()));
    }
}
```

复杂度：时间 `O(mn log(mn))`，空间 `O(mn)`。

#### 资深解法：小根堆按列推进

算法思想：因为数组有序，先把每个 `nums1[i]` 与 `nums2[0]` 的数对放入堆。每弹出 `(i, j)`，就把同一行的下一个 `(i, j + 1)` 放入堆。

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.PriorityQueue;

class Solution {
    public List<List<Integer>> kSmallestPairs(int[] nums1, int[] nums2, int k) {
        List<List<Integer>> ans = new ArrayList<>();
        PriorityQueue<int[]> heap = new PriorityQueue<>(
                (a, b) -> nums1[a[0]] + nums2[a[1]] - nums1[b[0]] - nums2[b[1]]);

        for (int i = 0; i < nums1.length && i < k; i++) {
            heap.offer(new int[]{i, 0});
        }

        while (k > 0 && !heap.isEmpty()) {
            int[] cur = heap.poll();
            int i = cur[0];
            int j = cur[1];
            ans.add(Arrays.asList(nums1[i], nums2[j]));
            if (j + 1 < nums2.length) {
                heap.offer(new int[]{i, j + 1});
            }
            k--;
        }
        return ans;
    }
}
```

复杂度：时间 `O(k log min(k, m))`，空间 `O(min(k, m))`。

#### 基础语法与算法思想

- `PriorityQueue` 可以按自定义比较器维护当前最小候选。
- `Arrays.asList(a, b)` 快速创建二元列表。
- 核心思想：多个有序序列取前 `k` 小时，用堆做多路归并。

---

## 374. 猜数字大小 (Easy)

我们正在玩猜数字游戏。猜数字游戏的规则如下：
我会从  `1`  到  `n`  随机选择一个数字。 请你猜选出的是哪个数字。（我选的数字在整个游戏中保持不变）。
如果你猜错了，我会告诉你，我选出的数字比你猜测的数字大了还是小了。
你可以通过调用一个预先定义好的接口  `int guess(int num)`  来获取猜测结果，返回值一共有三种可能的情况：

 `-1` ：你猜的数字比我选出的数字大 （即  `num > pick` ）。
 `1` ：你猜的数字比我选出的数字小 （即  `num < pick` ）。
 `0` ：你猜的数字与我选出的数字相等。（即  `num == pick` ）。

返回我选出的数字。
 
 **示例 1：** 

```text
输入：n = 10, pick = 6
输出：6
```

 **示例 2：** 

```text
输入：n = 1, pick = 1
输出：1
```

 **示例 3：** 

```text
输入：n = 2, pick = 1
输出：1
```

 
 **提示：** 

 `1 <= n <= 231 - 1` 
 `1 <= pick <= n`

### Java 解法补充

#### 基础解法：从小到大试

算法思想：从 `1` 到 `n` 依次调用 `guess`，返回 0 时就是答案。

```java
public class Solution extends GuessGame {
    public int guessNumber(int n) {
        for (int i = 1; i <= n; i++) {
            if (guess(i) == 0) {
                return i;
            }
        }
        return -1;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 资深解法：二分查找

算法思想：`guess(mid)` 会告诉答案在左侧还是右侧，区间具有单调方向，因此用二分缩小范围。

```java
public class Solution extends GuessGame {
    public int guessNumber(int n) {
        int left = 1;
        int right = n;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            int result = guess(mid);
            if (result == 0) return mid;
            if (result < 0) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        return -1;
    }
}
```

复杂度：时间 `O(log n)`，空间 `O(1)`。

#### 基础语法与算法思想

- `extends GuessGame` 表示继承题目提供的 API。
- API 返回 `-1` 代表猜大了，返回 `1` 代表猜小了。
- 核心思想：有大小反馈的猜数题就是二分查找。

---

## 375. 猜数字大小 II (Medium)

我们正在玩一个猜数游戏，游戏规则如下：

我从  `1`  **** 到  `n`  之间选择一个数字。
你来猜我选了哪个数字。
如果你猜到正确的数字，就会  **赢得游戏**  。
如果你猜错了，那么我会告诉你，我选的数字比你的  **更大或者更小**  ，并且你需要继续猜数。
每当你猜了数字  `x`  并且猜错了的时候，你需要支付金额为  `x`  的现金。如果你花光了钱，就会 **输掉游戏**  。

给你一个特定的数字  `n`  ，返回能够  **确保你获胜**  的最小现金数， **不管我选择那个数字**  。
 
 **示例 1：** 

```text
输入：n = 10
输出：16
解释：制胜策略如下：
- 数字范围是 [1,10] 。你先猜测数字为 7 。
    - 如果这是我选中的数字，你的总费用为 $0 。否则，你需要支付 $7 。
    - 如果我的数字更大，则下一步需要猜测的数字范围是 [8,10] 。你可以猜测数字为 9 。
        - 如果这是我选中的数字，你的总费用为 $7 。否则，你需要支付 $9 。
        - 如果我的数字更大，那么这个数字一定是 10 。你猜测数字为 10 并赢得游戏，总费用为 $7 + $9 = $16 。
        - 如果我的数字更小，那么这个数字一定是 8 。你猜测数字为 8 并赢得游戏，总费用为 $7 + $9 = $16 。
    - 如果我的数字更小，则下一步需要猜测的数字范围是 [1,6] 。你可以猜测数字为 3 。
        - 如果这是我选中的数字，你的总费用为 $7 。否则，你需要支付 $3 。
        - 如果我的数字更大，则下一步需要猜测的数字范围是 [4,6] 。你可以猜测数字为 5 。
            - 如果这是我选中的数字，你的总费用为 $7 + $3 = $10 。否则，你需要支付 $5 。
            - 如果我的数字更大，那么这个数字一定是 6 。你猜测数字为 6 并赢得游戏，总费用为 $7 + $3 + $5 = $15 。
            - 如果我的数字更小，那么这个数字一定是 4 。你猜测数字为 4 并赢得游戏，总费用为 $7 + $3 + $5 = $15 。
        - 如果我的数字更小，则下一步需要猜测的数字范围是 [1,2] 。你可以猜测数字为 1 。
            - 如果这是我选中的数字，你的总费用为 $7 + $3 = $10 。否则，你需要支付 $1 。
            - 如果我的数字更大，那么这个数字一定是 2 。你猜测数字为 2 并赢得游戏，总费用为 $7 + $3 + $1 = $11 。
在最糟糕的情况下，你需要支付 $16 。因此，你只需要 $16 就可以确保自己赢得游戏。
```

 **示例 2：** 

```text
输入：n = 1
输出：0
解释：只有一个可能的数字，所以你可以直接猜 1 并赢得游戏，无需支付任何费用。
```

 **示例 3：** 

```text
输入：n = 2
输出：1
解释：有两个可能的数字 1 和 2 。
- 你可以先猜 1 。
    - 如果这是我选中的数字，你的总费用为 $0 。否则，你需要支付 $1 。
    - 如果我的数字更大，那么这个数字一定是 2 。你猜测数字为 2 并赢得游戏，总费用为 $1 。
最糟糕的情况下，你需要支付 $1 。
```

 
 **提示：** 

 `1 <= n <= 200`

### Java 解法补充

#### 基础解法：递归尝试每个猜测

算法思想：定义 `cost(l, r)` 表示保证猜中 `[l, r]` 的最小费用。枚举第一次猜的数字 `x`，最坏情况费用为 `x + max(cost(l, x - 1), cost(x + 1, r))`。

```java
class Solution {
    public int getMoneyAmount(int n) {
        return cost(1, n);
    }

    private int cost(int left, int right) {
        if (left >= right) return 0;
        int ans = Integer.MAX_VALUE;
        for (int pick = left; pick <= right; pick++) {
            int worst = pick + Math.max(cost(left, pick - 1), cost(pick + 1, right));
            ans = Math.min(ans, worst);
        }
        return ans;
    }
}
```

复杂度：指数级，空间 `O(n)`，只适合理解状态。

#### 资深解法：区间动态规划

算法思想：把递归状态落表。按区间长度从小到大计算 `dp[left][right]`，每个区间枚举第一次猜测点，取所有最坏费用中的最小值。

```java
class Solution {
    public int getMoneyAmount(int n) {
        int[][] dp = new int[n + 2][n + 2];
        for (int len = 2; len <= n; len++) {
            for (int left = 1; left + len - 1 <= n; left++) {
                int right = left + len - 1;
                dp[left][right] = Integer.MAX_VALUE;
                for (int pick = left; pick <= right; pick++) {
                    int worst = pick + Math.max(dp[left][pick - 1], dp[pick + 1][right]);
                    dp[left][right] = Math.min(dp[left][right], worst);
                }
            }
        }
        return dp[1][n];
    }
}
```

复杂度：时间 `O(n^3)`，空间 `O(n^2)`。

#### 基础语法与算法思想

- `max` 表示对手选择更贵的分支，`min` 表示我们选择更好的策略。
- 区间 DP 常用 `left/right` 表示子问题边界。
- 核心思想：要求“保证获胜”的题，要按最坏情况做决策。

---

## 376. 摆动序列 (Medium)

如果连续数字之间的差严格地在正数和负数之间交替，则数字序列称为 **摆动序列 。** 第一个差（如果存在的话）可能是正数或负数。仅有一个元素或者含两个不等元素的序列也视作摆动序列。

例如，  `[1, 7, 4, 9, 2, 5]`  是一个  **摆动序列**  ，因为差值  `(6, -3, 5, -7, 3)`  是正负交替出现的。

相反， `[1, 4, 7, 2, 5]`  和  `[1, 7, 4, 5, 5]`  不是摆动序列，第一个序列是因为它的前两个差值都是正数，第二个序列是因为它的最后一个差值为零。

 **子序列**  可以通过从原始序列中删除一些（也可以不删除）元素来获得，剩下的元素保持其原始顺序。
给你一个整数数组  `nums`  ，返回  `nums`  中作为  **摆动序列** 的  **最长子序列的长度**  。
 
 **示例 1：** 

```text
输入：nums = [1,7,4,9,2,5]
输出：6
解释：整个序列均为摆动序列，各元素之间的差值为 (6, -3, 5, -7, 3) 。
```

 **示例 2：** 

```text
输入：nums = [1,17,5,10,13,15,10,5,16,8]
输出：7
解释：这个序列包含几个长度为 7 摆动序列。
其中一个是 [1, 17, 10, 13, 10, 16, 8] ，各元素之间的差值为 (16, -7, 3, -3, 6, -8) 。
```

 **示例 3：** 

```text
输入：nums = [1,2,3,4,5,6,7,8,9]
输出：2
```

 
 **提示：** 

 `1 <= nums.length <= 1000` 
 `0 <= nums[i] <= 1000` 

 
 **进阶：** 你能否用  `O(n)`  时间复杂度完成此题?

### Java 解法补充

#### 基础解法：动态规划数组

算法思想：`up[i]` 表示以 `i` 结尾且最后一个差为正的最长摆动长度，`down[i]` 表示最后一个差为负的最长摆动长度。枚举前一个位置转移。

```java
import java.util.Arrays;

class Solution {
    public int wiggleMaxLength(int[] nums) {
        int n = nums.length;
        int[] up = new int[n];
        int[] down = new int[n];
        Arrays.fill(up, 1);
        Arrays.fill(down, 1);

        int ans = 1;
        for (int i = 1; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    up[i] = Math.max(up[i], down[j] + 1);
                } else if (nums[i] < nums[j]) {
                    down[i] = Math.max(down[i], up[j] + 1);
                }
            }
            ans = Math.max(ans, Math.max(up[i], down[i]));
        }
        return ans;
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(n)`。

#### 资深解法：滚动维护上升和下降

算法思想：只关心当前能形成的最长上升摆动和下降摆动。遇到上升差时更新 `up = down + 1`，遇到下降差时更新 `down = up + 1`。

```java
class Solution {
    public int wiggleMaxLength(int[] nums) {
        int up = 1;
        int down = 1;
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] > nums[i - 1]) {
                up = down + 1;
            } else if (nums[i] < nums[i - 1]) {
                down = up + 1;
            }
        }
        return Math.max(up, down);
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 差值为 0 不会形成摆动，直接忽略。
- `up/down` 是成对状态，分别记录最后一步方向。
- 核心思想：摆动序列只需要保留方向变化的峰谷。

---

## 377. 组合总和 Ⅳ (Medium)

给你一个由  **不同**  整数组成的数组  `nums`  ，和一个目标整数  `target`  。请你从  `nums`  中找出并返回总和为  `target`  的元素排列的个数。
题目数据保证答案符合 32 位整数范围。
 
 **示例 1：** 

```text
输入：nums = [1,2,3], target = 4
输出：7
解释：
所有可能的组合为：
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)
请注意，顺序不同的序列被视作不同的组合。
```

 **示例 2：** 

```text
输入：nums = [9], target = 3
输出：0
```

 
 **提示：** 

 `1 <= nums.length <= 200` 
 `1 <= nums[i] <= 1000` 
 `nums`  中的所有元素  **互不相同** 
 `1 <= target <= 1000` 

 
 **进阶：** 如果给定的数组中含有负数会发生什么？问题会产生何种变化？如果允许负数出现，需要向题目中添加哪些限制条件？

### Java 解法补充

#### 基础解法：递归枚举排列

算法思想：当前剩余目标为 `remain`，每一步都可以选择任意一个不超过 `remain` 的数。剩余为 0 时找到一种排列。

```java
class Solution {
    public int combinationSum4(int[] nums, int target) {
        return dfs(nums, target);
    }

    private int dfs(int[] nums, int remain) {
        if (remain == 0) return 1;
        int ans = 0;
        for (int num : nums) {
            if (num <= remain) {
                ans += dfs(nums, remain - num);
            }
        }
        return ans;
    }
}
```

复杂度：指数级，空间 `O(target)`。

#### 资深解法：完全背包排列 DP

算法思想：`dp[i]` 表示和为 `i` 的排列数。因为顺序不同算不同排列，外层枚举目标值，内层枚举最后一个选择的数字。

```java
class Solution {
    public int combinationSum4(int[] nums, int target) {
        int[] dp = new int[target + 1];
        dp[0] = 1;
        for (int sum = 1; sum <= target; sum++) {
            for (int num : nums) {
                if (sum >= num) {
                    dp[sum] += dp[sum - num];
                }
            }
        }
        return dp[target];
    }
}
```

复杂度：时间 `O(target * nums.length)`，空间 `O(target)`。

#### 基础语法与算法思想

- `dp[0] = 1` 表示凑出 0 有一种空方案。
- 组合和排列的 DP 循环顺序不同；本题顺序敏感，要先枚举金额。
- 核心思想：把“最后一个数是谁”作为转移来源。

---

## 378. 有序矩阵中第 K 小的元素 (Medium)

给你一个  `n x n`  矩阵  `matrix`  ，其中每行和每列元素均按升序排序，找到矩阵中第  `k`  小的元素。
请注意，它是  **排序后**  的第  `k`  小元素，而不是第  `k`  个  **不同**  的元素。
你必须找到一个内存复杂度优于  `O(n2)`  的解决方案。
 
 **示例 1：** 

```text
输入：matrix = [[1,5,9],[10,11,13],[12,13,15]], k = 8
输出：13
解释：矩阵中的元素为 [1,5,9,10,11,12,13,13,15]，第 8 小元素是 13
```

 **示例 2：** 

```text
输入：matrix = [[-5]], k = 1
输出：-5
```

 
 **提示：** 

 `n == matrix.length` 
 `n == matrix[i].length` 
 `1 <= n <= 300` 
 `-109 <= matrix[i][j] <= 109` 
题目数据  **保证**   `matrix`  中的所有行和列都按  **非递减顺序**  排列
 `1 <= k <= n2` 

 
 **进阶：** 

你能否用一个恒定的内存(即  `O(1)`  内存复杂度)来解决这个问题?
你能在  `O(n)`  的时间复杂度下解决这个问题吗?这个方法对于面试来说可能太超前了，但是你会发现阅读这篇文章（ this paper ）很有趣。

### Java 解法补充

#### 基础解法：展开后排序

算法思想：把矩阵所有元素放入一维数组，排序后返回下标 `k - 1` 的元素。

```java
import java.util.Arrays;

class Solution {
    public int kthSmallest(int[][] matrix, int k) {
        int n = matrix.length;
        int[] nums = new int[n * n];
        int index = 0;
        for (int[] row : matrix) {
            for (int value : row) {
                nums[index++] = value;
            }
        }
        Arrays.sort(nums);
        return nums[k - 1];
    }
}
```

复杂度：时间 `O(n^2 log n)`，空间 `O(n^2)`。

#### 资深解法：按值域二分

算法思想：答案一定在矩阵最小值和最大值之间。二分一个值 `mid`，从左下角统计矩阵中小于等于 `mid` 的元素个数，若数量不少于 `k`，答案在左半边。

```java
class Solution {
    public int kthSmallest(int[][] matrix, int k) {
        int n = matrix.length;
        int left = matrix[0][0];
        int right = matrix[n - 1][n - 1];
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (countLessOrEqual(matrix, mid) >= k) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }

    private int countLessOrEqual(int[][] matrix, int target) {
        int n = matrix.length;
        int row = n - 1;
        int col = 0;
        int count = 0;
        while (row >= 0 && col < n) {
            if (matrix[row][col] <= target) {
                count += row + 1;
                col++;
            } else {
                row--;
            }
        }
        return count;
    }
}
```

复杂度：时间 `O(n log(valueRange))`，空间 `O(1)`。

#### 基础语法与算法思想

- `k - 1` 是第 `k` 小在 0 下标数组中的位置。
- 有序矩阵可以从左下角或右上角做线性计数。
- 核心思想：第 `k` 小不一定要枚举元素，可以二分答案值。

---

## 379. 电话目录管理系统 (Medium)

暂无内容描述。

### Java 解法补充

#### 基础解法：布尔数组线性查找

算法思想：用 `used[i]` 表示号码 `i` 是否被占用。`get` 时从头扫描第一个空闲号码并标记占用。

```java
class PhoneDirectory {
    private final boolean[] used;

    public PhoneDirectory(int maxNumbers) {
        used = new boolean[maxNumbers];
    }

    public int get() {
        for (int i = 0; i < used.length; i++) {
            if (!used[i]) {
                used[i] = true;
                return i;
            }
        }
        return -1;
    }

    public boolean check(int number) {
        return number >= 0 && number < used.length && !used[number];
    }

    public void release(int number) {
        if (number >= 0 && number < used.length) {
            used[number] = false;
        }
    }
}
```

复杂度：`get` 时间 `O(n)`，`check/release` 时间 `O(1)`，空间 `O(n)`。

#### 资深解法：空闲号码队列

算法思想：初始化时把所有号码放入队列。`get` 从队头取空闲号码，`release` 只在号码已占用时放回队列，避免重复释放导致重复分配。

```java
import java.util.ArrayDeque;
import java.util.Queue;

class PhoneDirectory {
    private final Queue<Integer> free = new ArrayDeque<>();
    private final boolean[] used;

    public PhoneDirectory(int maxNumbers) {
        used = new boolean[maxNumbers];
        for (int i = 0; i < maxNumbers; i++) {
            free.offer(i);
        }
    }

    public int get() {
        if (free.isEmpty()) return -1;
        int number = free.poll();
        used[number] = true;
        return number;
    }

    public boolean check(int number) {
        return number >= 0 && number < used.length && !used[number];
    }

    public void release(int number) {
        if (number >= 0 && number < used.length && used[number]) {
            used[number] = false;
            free.offer(number);
        }
    }
}
```

复杂度：各操作时间 `O(1)`，空间 `O(n)`。

#### 基础语法与算法思想

- 布尔数组适合表示编号是否被使用。
- 队列适合管理可复用资源池。
- 核心思想：设计题要先明确每个操作的复杂度目标和状态一致性。

---

## 380. O(1) 时间插入、删除和获取随机元素 (Medium)

实现 `RandomizedSet`  类：

 `RandomizedSet()`  初始化  `RandomizedSet`  对象
 `bool insert(int val)`  当元素  `val`  不存在时，向集合中插入该项，并返回  `true`  ；否则，返回  `false`  。
 `bool remove(int val)`  当元素  `val`  存在时，从集合中移除该项，并返回  `true`  ；否则，返回  `false`  。
 `int getRandom()`  随机返回现有集合中的一项（测试用例保证调用此方法时集合中至少存在一个元素）。每个元素应该有  **相同的概率**  被返回。

你必须实现类的所有函数，并满足每个函数的  **平均**  时间复杂度为  `O(1)`  。
 
 **示例：** 

```text
输入
["RandomizedSet", "insert", "remove", "insert", "getRandom", "remove", "insert", "getRandom"]
[[], [1], [2], [2], [], [1], [2], []]
输出
[null, true, false, true, 2, true, false, 2]

解释
RandomizedSet randomizedSet = new RandomizedSet();
randomizedSet.insert(1); // 向集合中插入 1 。返回 true 表示 1 被成功地插入。
randomizedSet.remove(2); // 返回 false ，表示集合中不存在 2 。
randomizedSet.insert(2); // 向集合中插入 2 。返回 true 。集合现在包含 [1,2] 。
randomizedSet.getRandom(); // getRandom 应随机返回 1 或 2 。
randomizedSet.remove(1); // 从集合中移除 1 ，返回 true 。集合现在包含 [2] 。
randomizedSet.insert(2); // 2 已在集合中，所以返回 false 。
randomizedSet.getRandom(); // 由于 2 是集合中唯一的数字，getRandom 总是返回 2 。
```

 
 **提示：** 

 `-231 <= val <= 231 - 1` 
最多调用  `insert` 、 `remove`  和  `getRandom`  函数  `2 *`  `105`  次
在调用  `getRandom`  方法时，数据结构中  **至少存在一个**  元素。

### Java 解法补充

#### 基础解法：列表顺序存储

算法思想：用列表保存元素。插入前用 `contains` 检查是否存在；删除用 `remove`；随机时按随机下标取值。写法简单但插入、删除都可能线性扫描。

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Random;

class RandomizedSet {
    private final List<Integer> nums = new ArrayList<>();
    private final Random random = new Random();

    public boolean insert(int val) {
        if (nums.contains(val)) return false;
        nums.add(val);
        return true;
    }

    public boolean remove(int val) {
        return nums.remove(Integer.valueOf(val));
    }

    public int getRandom() {
        return nums.get(random.nextInt(nums.size()));
    }
}
```

复杂度：`insert/remove` 时间 `O(n)`，`getRandom` 时间 `O(1)`，空间 `O(n)`。

#### 资深解法：数组列表加下标哈希表

算法思想：用 `ArrayList` 支持随机访问，用哈希表记录值到下标。删除时把最后一个元素换到待删位置，再删除尾部，从而保持 `O(1)`。

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Random;

class RandomizedSet {
    private final List<Integer> nums = new ArrayList<>();
    private final Map<Integer, Integer> index = new HashMap<>();
    private final Random random = new Random();

    public boolean insert(int val) {
        if (index.containsKey(val)) return false;
        index.put(val, nums.size());
        nums.add(val);
        return true;
    }

    public boolean remove(int val) {
        Integer i = index.get(val);
        if (i == null) return false;

        int last = nums.get(nums.size() - 1);
        nums.set(i, last);
        index.put(last, i);
        nums.remove(nums.size() - 1);
        index.remove(val);
        return true;
    }

    public int getRandom() {
        return nums.get(random.nextInt(nums.size()));
    }
}
```

复杂度：各操作均摊时间 `O(1)`，空间 `O(n)`。

#### 基础语法与算法思想

- `Random.nextInt(size)` 返回 `[0, size)` 的随机整数。
- `Integer.valueOf(val)` 可让 `List.remove` 按对象删除，而不是按下标删除。
- 核心思想：想同时支持随机和删除，常用“数组 + 下标表 + 尾部交换”。

---

## 381. O(1) 时间插入、删除和获取随机元素 - 允许重复 (Hard)

`RandomizedCollection`  是一种包含数字集合(可能是重复的)的数据结构。它应该支持插入和删除特定元素，以及删除随机元素。
实现  `RandomizedCollection`  类:

 `RandomizedCollection()` 初始化空的  `RandomizedCollection`  对象。
 `bool insert(int val)`  将一个  `val`  项插入到集合中，即使该项已经存在。如果该项不存在，则返回  `true`  ，否则返回  `false`  。
 `bool remove(int val)`  如果存在，从集合中移除一个  `val`  项。如果该项存在，则返回  `true`  ，否则返回  `false`  。注意，如果  `val`  在集合中出现多次，我们只删除其中一个。
 `int getRandom()`  从当前的多个元素集合中返回一个随机元素。每个元素被返回的概率与集合中包含的相同值的数量  **线性相关**  。

您必须实现类的函数，使每个函数的  **平均**  时间复杂度为  `O(1)`  。
 **注意：** 生成测试用例时，只有在  `RandomizedCollection`  中  **至少有一项**  时，才会调用  `getRandom`  。
 
 **示例 1:** 

```text
输入
["RandomizedCollection", "insert", "insert", "insert", "getRandom", "remove", "getRandom"]
[[], [1], [1], [2], [], [1], []]
输出
[null, true, false, true, 2, true, 1]

解释
RandomizedCollection collection = new RandomizedCollection();// 初始化一个空的集合。
collection.insert(1);   // 返回 true，因为集合不包含 1。
                        // 将 1 插入到集合中。
collection.insert(1);   // 返回 false，因为集合包含 1。
                        // 将另一个 1 插入到集合中。集合现在包含 [1,1]。
collection.insert(2);   // 返回 true，因为集合不包含 2。
                        // 将 2 插入到集合中。集合现在包含 [1,1,2]。
collection.getRandom(); // getRandom 应当:
                        // 有 2/3 的概率返回 1,
                        // 1/3 的概率返回 2。
collection.remove(1);   // 返回 true，因为集合包含 1。
                        // 从集合中移除 1。集合现在包含 [1,2]。
collection.getRandom(); // getRandom 应该返回 1 或 2，两者的可能性相同。
```

 
 **提示:** 

 `-231 <= val <= 231 - 1` 
 `insert` ,  `remove`  和  `getRandom`  最多  **总共**  被调用  `2 * 105`  次
当调用  `getRandom`  时，数据结构中  **至少有一个**  元素

### Java 解法补充

#### 基础解法：列表保存所有元素

算法思想：允许重复时，插入直接加入列表；删除时线性查找第一个等于 `val` 的位置并删除；随机按下标取值。概率天然按元素出现次数分布。

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Random;

class RandomizedCollection {
    private final List<Integer> nums = new ArrayList<>();
    private final Random random = new Random();

    public boolean insert(int val) {
        boolean notExists = !nums.contains(val);
        nums.add(val);
        return notExists;
    }

    public boolean remove(int val) {
        return nums.remove(Integer.valueOf(val));
    }

    public int getRandom() {
        return nums.get(random.nextInt(nums.size()));
    }
}
```

复杂度：`insert/remove` 时间 `O(n)`，`getRandom` 时间 `O(1)`，空间 `O(n)`。

#### 资深解法：列表加值到下标集合

算法思想：数组列表存所有元素，哈希表把每个值映射到它出现的所有下标。删除任意一个下标时，仍用尾部元素交换到被删位置，并同步更新下标集合。

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.HashSet;
import java.util.List;
import java.util.Map;
import java.util.Random;
import java.util.Set;

class RandomizedCollection {
    private final List<Integer> nums = new ArrayList<>();
    private final Map<Integer, Set<Integer>> indexes = new HashMap<>();
    private final Random random = new Random();

    public boolean insert(int val) {
        boolean notExists = !indexes.containsKey(val);
        indexes.computeIfAbsent(val, key -> new HashSet<>()).add(nums.size());
        nums.add(val);
        return notExists;
    }

    public boolean remove(int val) {
        Set<Integer> set = indexes.get(val);
        if (set == null || set.isEmpty()) return false;

        int removeIndex = set.iterator().next();
        set.remove(removeIndex);
        int lastIndex = nums.size() - 1;
        int lastValue = nums.get(lastIndex);

        nums.set(removeIndex, lastValue);
        Set<Integer> lastSet = indexes.get(lastValue);
        lastSet.remove(lastIndex);
        if (removeIndex < lastIndex) {
            lastSet.add(removeIndex);
        }

        nums.remove(lastIndex);
        if (set.isEmpty()) {
            indexes.remove(val);
        }
        return true;
    }

    public int getRandom() {
        return nums.get(random.nextInt(nums.size()));
    }
}
```

复杂度：各操作均摊时间 `O(1)`，空间 `O(n)`。

#### 基础语法与算法思想

- `Map<Integer, Set<Integer>>` 可以保存一个值对应的多个下标。
- `computeIfAbsent` 常用于“没有就创建容器”的场景。
- 核心思想：允许重复时，哈希表的值不再是单个下标，而是一组下标。

---

## 382. 链表随机节点 (Medium)

给你一个单链表，随机选择链表的一个节点，并返回相应的节点值。每个节点 **被选中的概率一样**  。
实现  `Solution`  类：

 `Solution(ListNode head)`  使用整数数组初始化对象。
 `int getRandom()`  从链表中随机选择一个节点并返回该节点的值。链表中所有节点被选中的概率相等。

 
 **示例：** 

```text
输入
["Solution", "getRandom", "getRandom", "getRandom", "getRandom", "getRandom"]
[[[1, 2, 3]], [], [], [], [], []]
输出
[null, 1, 3, 2, 2, 3]

解释
Solution solution = new Solution([1, 2, 3]);
solution.getRandom(); // 返回 1
solution.getRandom(); // 返回 3
solution.getRandom(); // 返回 2
solution.getRandom(); // 返回 2
solution.getRandom(); // 返回 3
// getRandom() 方法应随机返回 1、2、3中的一个，每个元素被返回的概率相等。
```

 
 **提示：** 

链表中的节点数在范围  `[1, 104]`  内
 `-104 <= Node.val <= 104` 
至多调用  `getRandom`  方法  `104`  次

 
 **进阶：** 

如果链表非常大且长度未知，该怎么处理？
你能否在不使用额外空间的情况下解决此问题？

### Java 解法补充

#### 基础解法：复制到数组列表

算法思想：构造对象时遍历链表，把所有节点值放进列表。随机时从列表中等概率选择一个下标。

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Random;

class Solution {
    private final List<Integer> values = new ArrayList<>();
    private final Random random = new Random();

    public Solution(ListNode head) {
        while (head != null) {
            values.add(head.val);
            head = head.next;
        }
    }

    public int getRandom() {
        return values.get(random.nextInt(values.size()));
    }
}
```

复杂度：初始化时间 `O(n)`，空间 `O(n)`；查询时间 `O(1)`。

#### 资深解法：蓄水池抽样

算法思想：每次查询重新遍历链表。遍历到第 `count` 个节点时，以 `1 / count` 的概率替换答案，最终每个节点被选中的概率相同。

```java
import java.util.Random;

class Solution {
    private final ListNode head;
    private final Random random = new Random();

    public Solution(ListNode head) {
        this.head = head;
    }

    public int getRandom() {
        int ans = head.val;
        int count = 0;
        for (ListNode cur = head; cur != null; cur = cur.next) {
            count++;
            if (random.nextInt(count) == 0) {
                ans = cur.val;
            }
        }
        return ans;
    }
}
```

复杂度：查询时间 `O(n)`，额外空间 `O(1)`。

#### 基础语法与算法思想

- `this.head = head` 把构造参数保存为对象字段。
- `random.nextInt(count) == 0` 表示 `1/count` 的概率命中。
- 核心思想：长度未知的数据流等概率抽样，优先考虑蓄水池抽样。

---

## 383. 赎金信 (Easy)

给你两个字符串： `ransomNote`  和  `magazine`  ，判断  `ransomNote`  能不能由  `magazine`  里面的字符构成。
如果可以，返回  `true`  ；否则返回  `false`  。
 `magazine`  中的每个字符只能在  `ransomNote`  中使用一次。
 
 **示例 1：** 

```text
输入：ransomNote = "a", magazine = "b"
输出：false
```

 **示例 2：** 

```text
输入：ransomNote = "aa", magazine = "ab"
输出：false
```

 **示例 3：** 

```text
输入：ransomNote = "aa", magazine = "aab"
输出：true
```

 
 **提示：** 

 `1 <= ransomNote.length, magazine.length <= 105` 
 `ransomNote`  和  `magazine`  由小写英文字母组成

### Java 解法补充

#### 基础解法：哈希表统计字符

算法思想：先统计 `magazine` 中每个字符出现次数，再逐个消耗 `ransomNote` 的字符；如果某个字符数量不够，返回 `false`。

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        Map<Character, Integer> count = new HashMap<>();
        for (int i = 0; i < magazine.length(); i++) {
            char c = magazine.charAt(i);
            count.put(c, count.getOrDefault(c, 0) + 1);
        }
        for (int i = 0; i < ransomNote.length(); i++) {
            char c = ransomNote.charAt(i);
            int left = count.getOrDefault(c, 0);
            if (left == 0) return false;
            count.put(c, left - 1);
        }
        return true;
    }
}
```

复杂度：时间 `O(m+n)`，空间 `O(1)`，字符集固定。

#### 资深解法：定长计数数组

算法思想：题目只有小写字母，用长度为 26 的数组比哈希表更轻量。杂志字符加一，勒索信字符减一，出现负数说明不够。

```java
class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        int[] count = new int[26];
        for (int i = 0; i < magazine.length(); i++) {
            count[magazine.charAt(i) - 'a']++;
        }
        for (int i = 0; i < ransomNote.length(); i++) {
            int index = ransomNote.charAt(i) - 'a';
            count[index]--;
            if (count[index] < 0) return false;
        }
        return true;
    }
}
```

复杂度：时间 `O(m+n)`，空间 `O(1)`。

#### 基础语法与算法思想

- `char - 'a'` 可以把小写字母映射到 `0..25`。
- 字符频次题先看字符集大小，固定字符集优先用数组。
- 核心思想：每个字符只能用一次，本质是库存扣减。

---

## 384. 打乱数组 (Medium)

给你一个整数数组  `nums`  ，设计算法来打乱一个没有重复元素的数组。打乱后，数组的所有排列应该是  **等可能**  的。
实现  `Solution`  class:

 `Solution(int[] nums)`  使用整数数组  `nums`  初始化对象
 `int[] reset()`  重设数组到它的初始状态并返回
 `int[] shuffle()`  返回数组随机打乱后的结果

 
 **示例 1：** 

```text
输入
["Solution", "shuffle", "reset", "shuffle"]
[[[1, 2, 3]], [], [], []]
输出
[null, [3, 1, 2], [1, 2, 3], [1, 3, 2]]

解释
Solution solution = new Solution([1, 2, 3]);
solution.shuffle();    // 打乱数组 [1,2,3] 并返回结果。任何 [1,2,3]的排列返回的概率应该相同。例如，返回 [3, 1, 2]
solution.reset();      // 重设数组到它的初始状态 [1, 2, 3] 。返回 [1, 2, 3]
solution.shuffle();    // 随机返回数组 [1, 2, 3] 打乱后的结果。例如，返回 [1, 3, 2]
```

 
 **提示：** 

 `1 <= nums.length <= 50` 
 `-106 <= nums[i] <= 106` 
 `nums`  中的所有元素都是  **唯一的** 
最多可以调用  `104`  次  `reset`  和  `shuffle`

### Java 解法补充

#### 基础解法：随机抽取到新数组

算法思想：每次打乱时把原数组放入列表，然后反复随机选一个元素加入结果并从列表删除，直到列表为空。

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Random;

class Solution {
    private final int[] original;
    private final Random random = new Random();

    public Solution(int[] nums) {
        original = nums.clone();
    }

    public int[] reset() {
        return original.clone();
    }

    public int[] shuffle() {
        List<Integer> pool = new ArrayList<>();
        for (int num : original) {
            pool.add(num);
        }
        int[] ans = new int[original.length];
        for (int i = 0; i < ans.length; i++) {
            int index = random.nextInt(pool.size());
            ans[i] = pool.remove(index);
        }
        return ans;
    }
}
```

复杂度：`shuffle` 时间 `O(n^2)`，空间 `O(n)`。

#### 资深解法：Fisher-Yates 洗牌

算法思想：从左到右遍历位置 `i`，在 `[i, n - 1]` 中随机选一个位置与 `i` 交换。每个排列出现概率相同。

```java
import java.util.Random;

class Solution {
    private final int[] original;
    private final Random random = new Random();

    public Solution(int[] nums) {
        original = nums.clone();
    }

    public int[] reset() {
        return original.clone();
    }

    public int[] shuffle() {
        int[] ans = original.clone();
        for (int i = 0; i < ans.length; i++) {
            int j = i + random.nextInt(ans.length - i);
            int temp = ans[i];
            ans[i] = ans[j];
            ans[j] = temp;
        }
        return ans;
    }
}
```

复杂度：`shuffle` 时间 `O(n)`，空间 `O(n)`，返回数组本身不计可视为额外 `O(n)`。

#### 基础语法与算法思想

- `clone()` 创建数组副本，避免修改原始数组。
- 洗牌必须保证每个排列等概率，不能简单按随机比较器排序。
- 核心思想：Fisher-Yates 每一步固定一个位置，概率可证明均匀。

---

## 385. 迷你语法分析器 (Medium)

给定一个字符串 s 表示一个整数嵌套列表，实现一个解析它的语法分析器并返回解析的结果  `NestedInteger`  。
列表中的每个元素只可能是整数或整数嵌套列表
 
 **示例 1：** 

```text
输入：s = "324",
输出：324
解释：你应该返回一个 NestedInteger 对象，其中只包含整数值 324。
```

 **示例 2：** 

```text
输入：s = "[123,[456,[789]]]",
输出：[123,[456,[789]]]
解释：返回一个 NestedInteger 对象包含一个有两个元素的嵌套列表：
1. 一个 integer 包含值 123
2. 一个包含两个元素的嵌套列表：
    i.  一个 integer 包含值 456
    ii. 一个包含一个元素的嵌套列表
         a. 一个 integer 包含值 789
```

 
 **提示：** 

 `1 <= s.length <= 5 * 104` 
 `s`  由数字、方括号  `"[]"` 、负号  `'-'`  、逗号  `','` 组成
用例保证  `s`  是可解析的  `NestedInteger` 
输入中的所有值的范围是  `[-106, 106]`

### Java 解法补充

#### 基础解法：递归解析

算法思想：用下标指针从左到右解析。如果当前位置不是 `'['`，就解析一个整数；如果是列表，就递归解析内部元素，直到遇到 `']'`。

```java
class Solution {
    public NestedInteger deserialize(String s) {
        int[] index = {0};
        return parse(s, index);
    }

    private NestedInteger parse(String s, int[] index) {
        if (s.charAt(index[0]) != '[') {
            int sign = 1;
            if (s.charAt(index[0]) == '-') {
                sign = -1;
                index[0]++;
            }
            int value = 0;
            while (index[0] < s.length() && Character.isDigit(s.charAt(index[0]))) {
                value = value * 10 + s.charAt(index[0]) - '0';
                index[0]++;
            }
            return new NestedInteger(sign * value);
        }

        NestedInteger list = new NestedInteger();
        index[0]++;
        while (s.charAt(index[0]) != ']') {
            list.add(parse(s, index));
            if (s.charAt(index[0]) == ',') {
                index[0]++;
            }
        }
        index[0]++;
        return list;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(d)`。

#### 资深解法：栈解析列表边界

算法思想：遇到 `'['` 创建新列表入栈；遇到数字解析完整整数并加入栈顶；遇到 `']'` 时弹出当前列表，加入上层列表。

```java
import java.util.ArrayDeque;
import java.util.Deque;

class Solution {
    public NestedInteger deserialize(String s) {
        if (s.charAt(0) != '[') {
            return new NestedInteger(Integer.parseInt(s));
        }

        Deque<NestedInteger> stack = new ArrayDeque<>();
        NestedInteger current = null;
        int i = 0;
        while (i < s.length()) {
            char c = s.charAt(i);
            if (c == '[') {
                if (current != null) stack.push(current);
                current = new NestedInteger();
                i++;
            } else if (c == ']') {
                if (!stack.isEmpty()) {
                    NestedInteger parent = stack.pop();
                    parent.add(current);
                    current = parent;
                }
                i++;
            } else if (c == ',') {
                i++;
            } else {
                int start = i;
                if (s.charAt(i) == '-') i++;
                while (i < s.length() && Character.isDigit(s.charAt(i))) i++;
                current.add(new NestedInteger(Integer.parseInt(s.substring(start, i))));
            }
        }
        return current;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(d)`。

#### 基础语法与算法思想

- `int[] index` 可在递归中共享并修改当前下标。
- `Integer.parseInt` 可解析带负号的整数字符串。
- 核心思想：嵌套结构可以用递归自然表达，也可以用栈模拟括号层级。

---

## 386. 字典序排数 (Medium)

给你一个整数  `n`  ，按字典序返回范围  `[1, n]`  内所有整数。
你必须设计一个时间复杂度为  `O(n)`  且使用  `O(1)`  额外空间的算法。
 
 **示例 1：** 

```text
输入：n = 13
输出：[1,10,11,12,13,2,3,4,5,6,7,8,9]
```

 **示例 2：** 

```text
输入：n = 2
输出：[1,2]
```

 
 **提示：** 

 `1 <= n <= 5 * 104`

### Java 解法补充

#### 基础解法：转字符串排序

算法思想：把 `1..n` 全部放进列表，按字符串字典序排序，再返回整数列表。

```java
import java.util.ArrayList;
import java.util.Comparator;
import java.util.List;

class Solution {
    public List<Integer> lexicalOrder(int n) {
        List<Integer> ans = new ArrayList<>();
        for (int i = 1; i <= n; i++) {
            ans.add(i);
        }
        ans.sort(Comparator.comparing(String::valueOf));
        return ans;
    }
}
```

复杂度：时间 `O(n log n)`，空间 `O(n)`。

#### 资深解法：按字典树 DFS

算法思想：数字字典序等价于一棵十叉树的先序遍历。先访问当前数字，再依次尝试追加 `0..9`。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<Integer> lexicalOrder(int n) {
        List<Integer> ans = new ArrayList<>();
        for (int first = 1; first <= 9; first++) {
            dfs(first, n, ans);
        }
        return ans;
    }

    private void dfs(int cur, int n, List<Integer> ans) {
        if (cur > n) return;
        ans.add(cur);
        for (int digit = 0; digit <= 9; digit++) {
            int next = cur * 10 + digit;
            if (next > n) break;
            dfs(next, n, ans);
        }
    }
}
```

复杂度：时间 `O(n)`，空间 `O(log n)`，不计答案空间。

#### 基础语法与算法思想

- `Comparator.comparing(String::valueOf)` 可按数字的字符串形式排序。
- DFS 前序遍历会先得到父节点，再得到以它为前缀的数字。
- 核心思想：字典序本质是前缀树顺序。

---

## 387. 字符串中的第一个唯一字符 (Easy)

给定一个字符串  `s`  ，找到 它的第一个不重复的字符，并返回它的索引 。如果不存在，则返回  `-1`  。
 
 **示例 1：** 

```text
输入: s = "leetcode"
输出: 0
```

 **示例 2:** 

```text
输入: s = "loveleetcode"
输出: 2
```

 **示例 3:** 

```text
输入: s = "aabb"
输出: -1
```

 
 **提示:** 

 `1 <= s.length <= 105` 
 `s`  只包含小写字母

### Java 解法补充

#### 基础解法：逐个字符检查全串

算法思想：对每个位置 `i`，再遍历整个字符串统计 `s[i]` 出现次数。第一个次数为 1 的位置就是答案。

```java
class Solution {
    public int firstUniqChar(String s) {
        for (int i = 0; i < s.length(); i++) {
            int count = 0;
            for (int j = 0; j < s.length(); j++) {
                if (s.charAt(i) == s.charAt(j)) {
                    count++;
                }
            }
            if (count == 1) return i;
        }
        return -1;
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(1)`。

#### 资深解法：计数数组

算法思想：先统计每个小写字母出现次数，再从左到右找到第一个计数为 1 的字符。

```java
class Solution {
    public int firstUniqChar(String s) {
        int[] count = new int[26];
        for (int i = 0; i < s.length(); i++) {
            count[s.charAt(i) - 'a']++;
        }
        for (int i = 0; i < s.length(); i++) {
            if (count[s.charAt(i) - 'a'] == 1) {
                return i;
            }
        }
        return -1;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 小写字母可以用长度 26 的数组计数。
- “第一个”要求第二次遍历仍按原字符串顺序扫描。
- 核心思想：频次统计先解决“唯一”，原顺序扫描再解决“第一个”。

---

## 388. 文件的最长绝对路径 (Medium)

假设有一个同时存储文件和目录的文件系统。下图展示了文件系统的一个示例：

这里将  `dir`  作为根目录中的唯一目录。 `dir`  包含两个子目录  `subdir1`  和  `subdir2`  。 `subdir1`  包含文件  `file1.ext`  和子目录  `subsubdir1` ； `subdir2`  包含子目录  `subsubdir2` ，该子目录下包含文件  `file2.ext`  。
在文本格式中，如下所示(⟶表示制表符)：

```text
dir
⟶ subdir1
⟶ ⟶ file1.ext
⟶ ⟶ subsubdir1
⟶ subdir2
⟶ ⟶ subsubdir2
⟶ ⟶ ⟶ file2.ext
```

如果是代码表示，上面的文件系统可以写为  `"dir\n\tsubdir1\n\t\tfile1.ext\n\t\tsubsubdir1\n\tsubdir2\n\t\tsubsubdir2\n\t\t\tfile2.ext"`  。 `'\n'`  和  `'\t'`  分别是换行符和制表符。
文件系统中的每个文件和文件夹都有一个唯一的  **绝对路径**  ，即必须打开才能到达文件/目录所在位置的目录顺序，所有路径用  `'/'`  连接。上面例子中，指向  `file2.ext`  的  **绝对路径**  是  `"dir/subdir2/subsubdir2/file2.ext"`  。每个目录名由字母、数字和/或空格组成，每个文件名遵循  `name.extension`  的格式，其中  `name`  和  `extension` 由字母、数字和/或空格组成。
给定一个以上述格式表示文件系统的字符串  `input`  ，返回文件系统中 指向  **文件**  的  **最长绝对路径**  的长度 。 如果系统中没有文件，返回  `0` 。
 
 **示例 1：** 

```text
输入：input = "dir\n\tsubdir1\n\tsubdir2\n\t\tfile.ext"
输出：20
解释：只有一个文件，绝对路径为 "dir/subdir2/file.ext" ，路径长度 20
```

 **示例 2：** 

```text
输入：input = "dir\n\tsubdir1\n\t\tfile1.ext\n\t\tsubsubdir1\n\tsubdir2\n\t\tsubsubdir2\n\t\t\tfile2.ext"
输出：32
解释：存在两个文件：
"dir/subdir1/file1.ext" ，路径长度 21
"dir/subdir2/subsubdir2/file2.ext" ，路径长度 32
返回 32 ，因为这是最长的路径
```

 **示例 3：** 

```text
输入：input = "a"
输出：0
解释：不存在任何文件
```

 **示例 4：** 

```text
输入：input = "file1.txt\nfile2.txt\nlongfile.txt"
输出：12
解释：根目录下有 3 个文件。
因为根目录中任何东西的绝对路径只是名称本身，所以答案是 "longfile.txt" ，路径长度为 12
```

 
 **提示：** 

 `1 <= input.length <= 104` 
 `input`  可能包含小写或大写的英文字母，一个换行符  `'\n'` ，一个制表符  `'\t'` ，一个点  `'.'` ，一个空格  `' '` ，和数字。

### Java 解法补充

#### 基础解法：保存每层路径字符串

算法思想：按换行拆分每一项，统计前导制表符数量得到深度。用数组保存每一层完整路径，遇到文件时更新最大长度。

```java
class Solution {
    public int lengthLongestPath(String input) {
        String[] lines = input.split("\n");
        String[] path = new String[lines.length + 1];
        int ans = 0;

        for (String line : lines) {
            int depth = 0;
            while (depth < line.length() && line.charAt(depth) == '\t') {
                depth++;
            }
            String name = line.substring(depth);
            if (depth == 0) {
                path[depth] = name;
            } else {
                path[depth] = path[depth - 1] + "/" + name;
            }
            if (name.contains(".")) {
                ans = Math.max(ans, path[depth].length());
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 资深解法：栈保存每层长度

算法思想：无需保存完整路径，只保存每一层到当前目录的长度。当前项深度为 `depth`，其路径长度等于上一层长度加当前名字长度和一个斜杠。

```java
class Solution {
    public int lengthLongestPath(String input) {
        String[] lines = input.split("\n");
        int[] lengthAtDepth = new int[lines.length + 1];
        int ans = 0;

        for (String line : lines) {
            int depth = 0;
            while (depth < line.length() && line.charAt(depth) == '\t') {
                depth++;
            }
            String name = line.substring(depth);
            int curLength = lengthAtDepth[depth] + name.length();
            if (name.contains(".")) {
                ans = Math.max(ans, curLength);
            } else {
                lengthAtDepth[depth + 1] = curLength + 1;
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(d)`。

#### 基础语法与算法思想

- `split("\n")` 按行解析文件系统文本。
- 前导 `\t` 的个数就是当前文件或目录的深度。
- 核心思想：路径长度只依赖父目录长度，不必真的拼出每条路径。

---

## 389. 找不同 (Easy)

给定两个字符串  `s`  和  `t`  ，它们只包含小写字母。
字符串  `t`  由字符串  `s`  随机重排，然后在随机位置添加一个字母。
请找出在  `t`  中被添加的字母。
 
 **示例 1：** 

```text
输入：s = "abcd", t = "abcde"
输出："e"
解释：'e' 是那个被添加的字母。
```

 **示例 2：** 

```text
输入：s = "", t = "y"
输出："y"
```

 
 **提示：** 

 `0 <= s.length <= 1000` 
 `t.length == s.length + 1` 
 `s`  和  `t`  只包含小写字母

### Java 解法补充

#### 基础解法：计数数组

算法思想：统计 `s` 中每个字符数量，再用 `t` 中字符逐个抵消。第一个抵消后为负的字符就是新增字符。

```java
class Solution {
    public char findTheDifference(String s, String t) {
        int[] count = new int[26];
        for (int i = 0; i < s.length(); i++) {
            count[s.charAt(i) - 'a']++;
        }
        for (int i = 0; i < t.length(); i++) {
            int index = t.charAt(i) - 'a';
            count[index]--;
            if (count[index] < 0) {
                return t.charAt(i);
            }
        }
        return ' ';
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 资深解法：异或抵消

算法思想：相同字符异或后会抵消为 0。把 `s` 和 `t` 的所有字符异或起来，最后剩下的就是被添加的字符。

```java
class Solution {
    public char findTheDifference(String s, String t) {
        char ans = 0;
        for (int i = 0; i < s.length(); i++) {
            ans ^= s.charAt(i);
        }
        for (int i = 0; i < t.length(); i++) {
            ans ^= t.charAt(i);
        }
        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 字符也可以参与异或运算。
- `x ^ x == 0`，`x ^ 0 == x`。
- 核心思想：找一个多出来的元素，异或是很轻量的抵消工具。

---

## 390. 消除游戏 (Medium)

列表  `arr`  由在范围  `[1, n]`  中的所有整数组成，并按严格递增排序。请你对  `arr`  应用下述算法：

从左到右，删除第一个数字，然后每隔一个数字删除一个，直到到达列表末尾。
重复上面的步骤，但这次是从右到左。也就是，删除最右侧的数字，然后剩下的数字每隔一个删除一个。
不断重复这两步，从左到右和从右到左交替进行，直到只剩下一个数字。

给你整数  `n`  ，返回  `arr`  最后剩下的数字。
 
 **示例 1：** 

```text
输入：n = 9
输出：6
解释：
arr = [1, 2, 3, 4, 5, 6, 7, 8, 9]
arr = [2, 4, 6, 8]
arr = [2, 6]
arr = [6]
```

 **示例 2：** 

```text
输入：n = 1
输出：1
```

 
 **提示：** 

 `1 <= n <= 109`

### Java 解法补充

#### 基础解法：列表模拟删除

算法思想：把 `1..n` 放入列表，按方向每隔一个删除。每轮后反转删除方向，直到只剩一个数。这个写法直观但无法通过大数据。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public int lastRemaining(int n) {
        List<Integer> nums = new ArrayList<>();
        for (int i = 1; i <= n; i++) {
            nums.add(i);
        }
        boolean leftToRight = true;
        while (nums.size() > 1) {
            List<Integer> next = new ArrayList<>();
            if (leftToRight) {
                for (int i = 1; i < nums.size(); i += 2) next.add(nums.get(i));
            } else {
                for (int i = nums.size() - 2; i >= 0; i -= 2) next.add(0, nums.get(i));
            }
            nums = next;
            leftToRight = !leftToRight;
        }
        return nums.get(0);
    }
}
```

复杂度：时间最坏 `O(n^2)`，空间 `O(n)`。

#### 资深解法：维护首项和步长

算法思想：只维护当前序列的首项 `head`、步长 `step`、剩余数量 `remaining` 和方向。每轮删除后，如果从左删，或从右删且数量为奇数，首项都会前进一个步长。

```java
class Solution {
    public int lastRemaining(int n) {
        int head = 1;
        int step = 1;
        int remaining = n;
        boolean leftToRight = true;

        while (remaining > 1) {
            if (leftToRight || remaining % 2 == 1) {
                head += step;
            }
            remaining /= 2;
            step *= 2;
            leftToRight = !leftToRight;
        }
        return head;
    }
}
```

复杂度：时间 `O(log n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 模拟完整列表会受 `n` 的上限限制。
- `step` 表示当前序列相邻两个保留数字的差。
- 核心思想：当序列始终是等差数列时，只维护等差数列参数即可。

---

