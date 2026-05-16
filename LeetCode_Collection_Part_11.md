# LeetCode 题目合集 Part 11

## 301. 删除无效的括号 (Hard)

给你一个由若干括号和字母组成的字符串  `s`  ，删除最小数量的无效括号，使得输入的字符串有效。
返回所有可能的结果。答案可以按  **任意顺序**  返回。
 
 **示例 1：** 

```text
输入：s = "()())()"
输出：["(())()","()()()"]
```

 **示例 2：** 

```text
输入：s = "(a)())()"
输出：["(a())()","(a)()()"]
```

 **示例 3：** 

```text
输入：s = ")("
输出：[""]
```

 
 **提示：** 

 `1 <= s.length <= 25` 
 `s`  由小写英文字母以及括号  `'('`  和  `')'`  组成
 `s`  中至多含  `20`  个括号

### Java 解法补充

#### 基础解法：BFS 按层删除括号

算法思想：每一层删除一个括号，第一次遇到合法字符串时，这一层就是最少删除次数。

```java
class Solution {
    public List<String> removeInvalidParentheses(String s) {
        List<String> ans = new ArrayList<>();
        Set<String> seen = new HashSet<>();
        Queue<String> queue = new ArrayDeque<>();
        queue.offer(s);
        seen.add(s);
        boolean found = false;
        while (!queue.isEmpty()) {
            String cur = queue.poll();
            if (valid(cur)) {
                ans.add(cur);
                found = true;
            }
            if (found) continue;
            for (int i = 0; i < cur.length(); i++) {
                char c = cur.charAt(i);
                if (c != '(' && c != ')') continue;
                String next = cur.substring(0, i) + cur.substring(i + 1);
                if (seen.add(next)) queue.offer(next);
            }
        }
        return ans;
    }

    private boolean valid(String s) {
        int balance = 0;
        for (char c : s.toCharArray()) {
            if (c == '(') balance++;
            else if (c == ')' && --balance < 0) return false;
        }
        return balance == 0;
    }
}
```

复杂度：时间最坏 `O(2^n * n)`，空间 `O(2^n)`。

#### 资深解法：先统计必须删除的左右括号再回溯剪枝

算法思想：先算出最少要删的左括号和右括号，回溯时只删除必要数量，并用集合去重。

```java
class Solution {
    public List<String> removeInvalidParentheses(String s) {
        int leftRemove = 0, rightRemove = 0;
        for (char c : s.toCharArray()) {
            if (c == '(') leftRemove++;
            else if (c == ')') {
                if (leftRemove == 0) rightRemove++;
                else leftRemove--;
            }
        }
        Set<String> ans = new HashSet<>();
        dfs(s, 0, leftRemove, rightRemove, 0, new StringBuilder(), ans);
        return new ArrayList<>(ans);
    }

    private void dfs(String s, int index, int leftRemove, int rightRemove, int balance,
            StringBuilder path, Set<String> ans) {
        if (index == s.length()) {
            if (leftRemove == 0 && rightRemove == 0 && balance == 0) ans.add(path.toString());
            return;
        }
        char c = s.charAt(index);
        int len = path.length();
        if (c == '(' && leftRemove > 0) dfs(s, index + 1, leftRemove - 1, rightRemove, balance, path, ans);
        if (c == ')' && rightRemove > 0) dfs(s, index + 1, leftRemove, rightRemove - 1, balance, path, ans);
        if (c != '(' && c != ')') {
            path.append(c);
            dfs(s, index + 1, leftRemove, rightRemove, balance, path, ans);
        } else if (c == '(') {
            path.append(c);
            dfs(s, index + 1, leftRemove, rightRemove, balance + 1, path, ans);
        } else if (balance > 0) {
            path.append(c);
            dfs(s, index + 1, leftRemove, rightRemove, balance - 1, path, ans);
        }
        path.setLength(len);
    }
}
```

复杂度：时间最坏 `O(2^n * n)`，空间 `O(n)`，不计答案。

#### 基础语法与算法思想

- `balance` 表示当前未匹配的左括号数量。
- BFS 的“第一次合法层”天然满足最少删除。

---

## 302. 包含全部黑色像素的最小矩形 (Hard)

暂无内容描述。

### Java 解法补充

#### 基础解法：扫描整个图片找黑色像素边界

算法思想：遍历所有格子，记录黑色像素的最小行、最大行、最小列、最大列。

```java
class Solution {
    public int minArea(char[][] image, int x, int y) {
        int top = image.length, bottom = -1, left = image[0].length, right = -1;
        for (int i = 0; i < image.length; i++) {
            for (int j = 0; j < image[0].length; j++) {
                if (image[i][j] == '1') {
                    top = Math.min(top, i);
                    bottom = Math.max(bottom, i);
                    left = Math.min(left, j);
                    right = Math.max(right, j);
                }
            }
        }
        return (bottom - top + 1) * (right - left + 1);
    }
}
```

复杂度：时间 `O(mn)`，空间 `O(1)`。

#### 资深解法：二分查找上下左右边界

算法思想：黑色像素连通，行列是否含黑点具有可二分的边界。

```java
class Solution {
    public int minArea(char[][] image, int x, int y) {
        int m = image.length, n = image[0].length;
        int top = searchRows(image, 0, x, true);
        int bottom = searchRows(image, x + 1, m, false);
        int left = searchCols(image, 0, y, true);
        int right = searchCols(image, y + 1, n, false);
        return (bottom - top) * (right - left);
    }

    private int searchRows(char[][] image, int low, int high, boolean firstBlack) {
        while (low < high) {
            int mid = low + (high - low) / 2;
            boolean hasBlack = false;
            for (int c = 0; c < image[0].length && !hasBlack; c++) hasBlack = image[mid][c] == '1';
            if (hasBlack == firstBlack) high = mid;
            else low = mid + 1;
        }
        return low;
    }

    private int searchCols(char[][] image, int low, int high, boolean firstBlack) {
        while (low < high) {
            int mid = low + (high - low) / 2;
            boolean hasBlack = false;
            for (int r = 0; r < image.length && !hasBlack; r++) hasBlack = image[r][mid] == '1';
            if (hasBlack == firstBlack) high = mid;
            else low = mid + 1;
        }
        return low;
    }
}
```

复杂度：时间 `O(m log n + n log m)`，空间 `O(1)`。

#### 基础语法与算法思想

- 面积由边界差计算。
- 有序边界问题优先考虑二分。

---

## 303. 区域和检索 - 数组不可变 (Easy)

给定一个整数数组   `nums` ，处理以下类型的多个查询:

计算索引  `left`  和  `right`  （包含  `left`  和  `right` ）之间的  `nums`  元素的  **和**  ，其中  `left <= right` 

实现  `NumArray`  类：

 `NumArray(int[] nums)`  使用数组  `nums`  初始化对象
 `int sumRange(int left, int right)`  返回数组  `nums`  中索引  `left`  和  `right`  之间的元素的  **总和**  ，包含  `left`  和  `right`  两点（也就是  `nums[left] + nums[left + 1] + ... + nums[right]`  )

 
 **示例 1：** 

```text
输入：
["NumArray", "sumRange", "sumRange", "sumRange"]
[[[-2, 0, 3, -5, 2, -1]], [0, 2], [2, 5], [0, 5]]
输出：
[null, 1, -1, -3]

解释：
NumArray numArray = new NumArray([-2, 0, 3, -5, 2, -1]);
numArray.sumRange(0, 2); // return 1 ((-2) + 0 + 3)
numArray.sumRange(2, 5); // return -1 (3 + (-5) + 2 + (-1)) 
numArray.sumRange(0, 5); // return -3 ((-2) + 0 + 3 + (-5) + 2 + (-1))
```

 
 **提示：** 

 `1 <= nums.length <= 104` 
 `-105 <= nums[i] <= 105` 
 `0 <= left <= right < nums.length` 
最多调用  `104`  次  `sumRange`  **** 方法

### Java 解法补充

#### 基础解法：每次查询直接累加区间

算法思想：保存原数组，`sumRange` 时从 `left` 加到 `right`。

```java
class NumArray {
    private int[] nums;

    public NumArray(int[] nums) {
        this.nums = nums;
    }

    public int sumRange(int left, int right) {
        int sum = 0;
        for (int i = left; i <= right; i++) sum += nums[i];
        return sum;
    }
}
```

复杂度：构造 `O(1)`，查询 `O(n)`，空间 `O(1)`。

#### 资深解法：前缀和

算法思想：预处理 `prefix[i + 1]` 表示前 `i` 个数之和，区间和一次相减得到。

```java
class NumArray {
    private int[] prefix;

    public NumArray(int[] nums) {
        prefix = new int[nums.length + 1];
        for (int i = 0; i < nums.length; i++) prefix[i + 1] = prefix[i] + nums[i];
    }

    public int sumRange(int left, int right) {
        return prefix[right + 1] - prefix[left];
    }
}
```

复杂度：构造 `O(n)`，查询 `O(1)`，空间 `O(n)`。

#### 基础语法与算法思想

- 前缀和适合数组不变、区间查询很多的场景。

---

## 304. 二维区域和检索 - 矩阵不可变 (Medium)

给定一个二维矩阵  `matrix` ，以下类型的多个请求：

计算其子矩形范围内元素的总和，该子矩阵的  **左上角**  为  `(row1, col1)`  ， **右下角**  为  `(row2, col2)`  。

实现  `NumMatrix`  类：

 `NumMatrix(int[][] matrix)`  给定整数矩阵  `matrix`  进行初始化
 `int sumRegion(int row1, int col1, int row2, int col2)`  返回  **左上角**   `(row1, col1)`  、 **右下角**   `(row2, col2)`  所描述的子矩阵的元素  **总和**  。

 
 **示例 1：** 

```text
输入: 
["NumMatrix","sumRegion","sumRegion","sumRegion"]
[[[[3,0,1,4,2],[5,6,3,2,1],[1,2,0,1,5],[4,1,0,1,7],[1,0,3,0,5]]],[2,1,4,3],[1,1,2,2],[1,2,2,4]]
输出: 
[null, 8, 11, 12]

解释:
NumMatrix numMatrix = new NumMatrix([[3,0,1,4,2],[5,6,3,2,1],[1,2,0,1,5],[4,1,0,1,7],[1,0,3,0,5]]);
numMatrix.sumRegion(2, 1, 4, 3); // return 8 (红色矩形框的元素总和)
numMatrix.sumRegion(1, 1, 2, 2); // return 11 (绿色矩形框的元素总和)
numMatrix.sumRegion(1, 2, 2, 4); // return 12 (蓝色矩形框的元素总和)
```

 
 **提示：** 

 `m == matrix.length` 
 `n == matrix[i].length` 
 `1 <= m, n <= 200` 
 `-105 <= matrix[i][j] <= 105` 
 `0 <= row1 <= row2 < m` 
 `0 <= col1 <= col2 < n` 
最多调用  `104`  次  `sumRegion`  方法

### Java 解法补充

#### 基础解法：每次查询扫描子矩形

算法思想：保存矩阵，查询时双重循环累加目标区域。

```java
class NumMatrix {
    private int[][] matrix;

    public NumMatrix(int[][] matrix) {
        this.matrix = matrix;
    }

    public int sumRegion(int row1, int col1, int row2, int col2) {
        int sum = 0;
        for (int i = row1; i <= row2; i++) {
            for (int j = col1; j <= col2; j++) sum += matrix[i][j];
        }
        return sum;
    }
}
```

复杂度：查询 `O(mn)`，空间 `O(1)`。

#### 资深解法：二维前缀和

算法思想：`sum[i+1][j+1]` 表示左上角到 `(i,j)` 的区域和，查询用容斥。

```java
class NumMatrix {
    private int[][] sum;

    public NumMatrix(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        sum = new int[m + 1][n + 1];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                sum[i + 1][j + 1] = matrix[i][j] + sum[i][j + 1] + sum[i + 1][j] - sum[i][j];
            }
        }
    }

    public int sumRegion(int row1, int col1, int row2, int col2) {
        return sum[row2 + 1][col2 + 1] - sum[row1][col2 + 1]
                - sum[row2 + 1][col1] + sum[row1][col1];
    }
}
```

复杂度：构造 `O(mn)`，查询 `O(1)`，空间 `O(mn)`。

#### 基础语法与算法思想

- 二维前缀和的核心是容斥。

---

## 305. 岛屿数量 II (Hard)

暂无内容描述。

### Java 解法补充

#### 基础解法：每次加陆地后重新 BFS 统计岛屿

算法思想：维护网格，每次操作后扫描整张图，用 BFS 统计连通块。

```java
class Solution {
    public List<Integer> numIslands2(int m, int n, int[][] positions) {
        int[][] grid = new int[m][n];
        List<Integer> ans = new ArrayList<>();
        for (int[] p : positions) {
            grid[p[0]][p[1]] = 1;
            ans.add(count(grid));
        }
        return ans;
    }

    private int count(int[][] grid) {
        int m = grid.length, n = grid[0].length, ans = 0;
        boolean[][] seen = new boolean[m][n];
        int[][] dirs = {{1,0},{-1,0},{0,1},{0,-1}};
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) {
            if (grid[i][j] == 0 || seen[i][j]) continue;
            ans++;
            Queue<int[]> q = new ArrayDeque<>();
            q.offer(new int[]{i, j});
            seen[i][j] = true;
            while (!q.isEmpty()) {
                int[] cur = q.poll();
                for (int[] d : dirs) {
                    int x = cur[0] + d[0], y = cur[1] + d[1];
                    if (x < 0 || x == m || y < 0 || y == n || seen[x][y] || grid[x][y] == 0) continue;
                    seen[x][y] = true;
                    q.offer(new int[]{x, y});
                }
            }
        }
        return ans;
    }
}
```

复杂度：每次操作 `O(mn)`，空间 `O(mn)`。

#### 资深解法：并查集动态合并陆地

算法思想：新增陆地岛屿数加一，与四邻陆地合并时岛屿数减一。

```java
class Solution {
    public List<Integer> numIslands2(int m, int n, int[][] positions) {
        int[] parent = new int[m * n];
        Arrays.fill(parent, -1);
        List<Integer> ans = new ArrayList<>();
        int count = 0;
        int[][] dirs = {{1,0},{-1,0},{0,1},{0,-1}};
        for (int[] p : positions) {
            int id = p[0] * n + p[1];
            if (parent[id] != -1) {
                ans.add(count);
                continue;
            }
            parent[id] = id;
            count++;
            for (int[] d : dirs) {
                int x = p[0] + d[0], y = p[1] + d[1];
                if (x < 0 || x == m || y < 0 || y == n) continue;
                int next = x * n + y;
                if (parent[next] == -1) continue;
                int a = find(parent, id), b = find(parent, next);
                if (a != b) {
                    parent[a] = b;
                    count--;
                }
            }
            ans.add(count);
        }
        return ans;
    }

    private int find(int[] parent, int x) {
        if (parent[x] != x) parent[x] = find(parent, parent[x]);
        return parent[x];
    }
}
```

复杂度：近似 `O(k α(mn))`，空间 `O(mn)`。

#### 基础语法与算法思想

- 动态连通性问题优先考虑并查集。

---

## 306. 累加数 (Medium)

**累加数**  是一个字符串，组成它的数字可以形成累加序列。
一个有效的  **累加序列**  必须 **至少** 包含 3 个数。除了最开始的两个数以外，序列中的每个后续数字必须是它之前两个数字之和。
给你一个只包含数字  `'0'-'9'`  的字符串，编写一个算法来判断给定输入是否是  **累加数**  。如果是，返回  `true`  ；否则，返回  `false`  。
 **说明：** 累加序列里的数，除数字 0 之外， **不会**  以 0 开头，所以不会出现  `1, 2, 03`  或者  `1, 02, 3`  的情况。
 
 **示例 1：** 

```text
输入："112358"
输出：true 
解释：累加序列为: 1, 1, 2, 3, 5, 8 。1 + 1 = 2, 1 + 2 = 3, 2 + 3 = 5, 3 + 5 = 8
```

 **示例 2：** 

```text
输入："199100199"
输出：true 
解释：累加序列为: 1, 99, 100, 199。1 + 99 = 100, 99 + 100 = 199
```

 
 **提示：** 

 `1 <= num.length <= 35` 
 `num`  仅由数字（ `0`  -  `9` ）组成

 
 **进阶：** 你计划如何处理由过大的整数输入导致的溢出?

### Java 解法补充

#### 基础解法：枚举前两个数字并用 BigInteger 校验

算法思想：确定前两个数字后，后续数字必须唯一，用大整数避免溢出。

```java
class Solution {
    public boolean isAdditiveNumber(String num) {
        int n = num.length();
        for (int i = 1; i <= n / 2; i++) {
            if (num.charAt(0) == '0' && i > 1) break;
            for (int j = i + 1; j < n; j++) {
                if (num.charAt(i) == '0' && j - i > 1) break;
                BigInteger a = new BigInteger(num.substring(0, i));
                BigInteger b = new BigInteger(num.substring(i, j));
                if (check(num, j, a, b)) return true;
            }
        }
        return false;
    }

    private boolean check(String num, int index, BigInteger a, BigInteger b) {
        while (index < num.length()) {
            String c = a.add(b).toString();
            if (!num.startsWith(c, index)) return false;
            index += c.length();
            a = b;
            b = new BigInteger(c);
        }
        return true;
    }
}
```

复杂度：时间 `O(n^3)`，空间 `O(n)`。

#### 资深解法：字符串加法校验

算法思想：不用大整数库，直接用字符串模拟加法，更贴近跨语言和超大数场景。

```java
class Solution {
    public boolean isAdditiveNumber(String num) {
        int n = num.length();
        for (int i = 1; i <= n / 2; i++) {
            if (num.charAt(0) == '0' && i > 1) break;
            for (int j = i + 1; j < n; j++) {
                if (num.charAt(i) == '0' && j - i > 1) break;
                if (valid(num.substring(0, i), num.substring(i, j), num.substring(j))) return true;
            }
        }
        return false;
    }

    private boolean valid(String a, String b, String rest) {
        while (!rest.isEmpty()) {
            String c = add(a, b);
            if (!rest.startsWith(c)) return false;
            rest = rest.substring(c.length());
            a = b;
            b = c;
        }
        return true;
    }

    private String add(String a, String b) {
        StringBuilder sb = new StringBuilder();
        int i = a.length() - 1, j = b.length() - 1, carry = 0;
        while (i >= 0 || j >= 0 || carry != 0) {
            int x = i >= 0 ? a.charAt(i--) - '0' : 0;
            int y = j >= 0 ? b.charAt(j--) - '0' : 0;
            int sum = x + y + carry;
            sb.append(sum % 10);
            carry = sum / 10;
        }
        return sb.reverse().toString();
    }
}
```

复杂度：时间 `O(n^3)`，空间 `O(n)`。

#### 基础语法与算法思想

- 前导零要单独处理。
- 大数加法可以逐位从右往左模拟。

---

## 307. 区域和检索 - 数组可修改 (Medium)

给你一个数组  `nums`  ，请你完成两类查询。

其中一类查询要求  **更新**  数组  `nums`  下标对应的值
另一类查询要求返回数组  `nums`  中索引  `left`  和索引  `right`  之间（  **包含** ）的nums元素的  **和**  ，其中  `left <= right` 

实现  `NumArray`  类：

 `NumArray(int[] nums)`  用整数数组  `nums`  初始化对象
 `void update(int index, int val)`  将  `nums[index]`  的值  **更新**  为  `val` 
 `int sumRange(int left, int right)`  返回数组  `nums`  中索引  `left`  和索引  `right`  之间（  **包含** ）的nums元素的  **和**  （即， `nums[left] + nums[left + 1], ..., nums[right]` ）

 
 **示例 1：** 

```text
输入：
["NumArray", "sumRange", "update", "sumRange"]
[[[1, 3, 5]], [0, 2], [1, 2], [0, 2]]
输出：
[null, 9, null, 8]

解释：
NumArray numArray = new NumArray([1, 3, 5]);
numArray.sumRange(0, 2); // 返回 1 + 3 + 5 = 9
numArray.update(1, 2);   // nums = [1,2,5]
numArray.sumRange(0, 2); // 返回 1 + 2 + 5 = 8
```

 
 **提示：** 

 `1 <= nums.length <= 3 * 104` 
 `-100 <= nums[i] <= 100` 
 `0 <= index < nums.length` 
 `-100 <= val <= 100` 
 `0 <= left <= right < nums.length` 
调用  `update`  和  `sumRange`  方法次数不大于  `3 * 104`

### Java 解法补充

#### 基础解法：数组保存原值，查询时扫描区间

算法思想：`update` 直接改数组，`sumRange` 每次循环累加。

```java
class NumArray {
    private int[] nums;

    public NumArray(int[] nums) {
        this.nums = nums;
    }

    public void update(int index, int val) {
        nums[index] = val;
    }

    public int sumRange(int left, int right) {
        int sum = 0;
        for (int i = left; i <= right; i++) sum += nums[i];
        return sum;
    }
}
```

复杂度：更新 `O(1)`，查询 `O(n)`，空间 `O(1)`。

#### 资深解法：树状数组

算法思想：树状数组支持单点更新和前缀和查询，区间和用两个前缀相减。

```java
class NumArray {
    private int[] nums;
    private int[] bit;

    public NumArray(int[] nums) {
        this.nums = nums.clone();
        bit = new int[nums.length + 1];
        for (int i = 0; i < nums.length; i++) add(i + 1, nums[i]);
    }

    public void update(int index, int val) {
        add(index + 1, val - nums[index]);
        nums[index] = val;
    }

    public int sumRange(int left, int right) {
        return sum(right + 1) - sum(left);
    }

    private void add(int i, int delta) {
        while (i < bit.length) {
            bit[i] += delta;
            i += i & -i;
        }
    }

    private int sum(int i) {
        int ans = 0;
        while (i > 0) {
            ans += bit[i];
            i -= i & -i;
        }
        return ans;
    }
}
```

复杂度：构造 `O(n log n)`，更新/查询 `O(log n)`，空间 `O(n)`。

#### 基础语法与算法思想

- `i & -i` 表示树状数组当前节点覆盖区间长度。

---

## 308. 二维区域和检索 - 矩阵可修改 (Medium)

暂无内容描述。

### Java 解法补充

#### 基础解法：直接保存矩阵并扫描子区域

算法思想：更新时改矩阵，查询时双重循环累加。

```java
class NumMatrix {
    private int[][] matrix;

    public NumMatrix(int[][] matrix) {
        this.matrix = matrix;
    }

    public void update(int row, int col, int val) {
        matrix[row][col] = val;
    }

    public int sumRegion(int row1, int col1, int row2, int col2) {
        int sum = 0;
        for (int i = row1; i <= row2; i++) {
            for (int j = col1; j <= col2; j++) sum += matrix[i][j];
        }
        return sum;
    }
}
```

复杂度：更新 `O(1)`，查询 `O(mn)`，空间 `O(1)`。

#### 资深解法：二维树状数组

算法思想：二维 BIT 维护矩阵前缀和，单点更新后影响相关树节点。

```java
class NumMatrix {
    private int[][] nums;
    private int[][] bit;

    public NumMatrix(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        nums = new int[m][n];
        bit = new int[m + 1][n + 1];
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) update(i, j, matrix[i][j]);
    }

    public void update(int row, int col, int val) {
        int delta = val - nums[row][col];
        nums[row][col] = val;
        for (int i = row + 1; i < bit.length; i += i & -i) {
            for (int j = col + 1; j < bit[0].length; j += j & -j) bit[i][j] += delta;
        }
    }

    public int sumRegion(int row1, int col1, int row2, int col2) {
        return sum(row2 + 1, col2 + 1) - sum(row1, col2 + 1)
                - sum(row2 + 1, col1) + sum(row1, col1);
    }

    private int sum(int row, int col) {
        int ans = 0;
        for (int i = row; i > 0; i -= i & -i) {
            for (int j = col; j > 0; j -= j & -j) ans += bit[i][j];
        }
        return ans;
    }
}
```

复杂度：更新/查询 `O(log m log n)`，空间 `O(mn)`。

#### 基础语法与算法思想

- 二维 BIT 是一维 BIT 的嵌套形式。

---

## 309. 买卖股票的最佳时机含冷冻期 (Medium)

给定一个整数数组 `prices` ，其中第   `prices[i]`  表示第  `i`  天的股票价格 。​
设计一个算法计算出最大利润。在满足以下约束条件下，你可以尽可能地完成更多的交易（多次买卖一支股票）:

卖出股票后，你无法在第二天买入股票 (即冷冻期为 1 天)。

 **注意：** 你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。
 
 **示例 1:** 

```text
输入: prices = [1,2,3,0,2]
输出: 3 
解释: 对应的交易状态为: [买入, 卖出, 冷冻期, 买入, 卖出]
```

 **示例 2:** 

```text
输入: prices = [1]
输出: 0
```

 
 **提示：** 

 `1 <= prices.length <= 5000` 
 `0 <= prices[i] <= 1000`

### Java 解法补充

#### 基础解法：递归枚举买卖和冷冻

算法思想：每天可以休息、买入或卖出；卖出后跳到后天，用记忆化避免重复。

```java
class Solution {
    private Integer[][] memo;

    public int maxProfit(int[] prices) {
        memo = new Integer[prices.length][2];
        return dfs(prices, 0, 0);
    }

    private int dfs(int[] prices, int day, int hold) {
        if (day >= prices.length) return 0;
        if (memo[day][hold] != null) return memo[day][hold];
        int ans = dfs(prices, day + 1, hold);
        if (hold == 0) ans = Math.max(ans, -prices[day] + dfs(prices, day + 1, 1));
        else ans = Math.max(ans, prices[day] + dfs(prices, day + 2, 0));
        return memo[day][hold] = ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 资深解法：三状态动态规划

算法思想：维护持股、刚卖出、空仓冷冻后的最大收益。

```java
class Solution {
    public int maxProfit(int[] prices) {
        int hold = -prices[0], sold = 0, rest = 0;
        for (int i = 1; i < prices.length; i++) {
            int prevSold = sold;
            sold = hold + prices[i];
            hold = Math.max(hold, rest - prices[i]);
            rest = Math.max(rest, prevSold);
        }
        return Math.max(sold, rest);
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 冷冻期本质是卖出后买入状态要隔一天。

---

## 310. 最小高度树 (Medium)

树是一个无向图，其中任何两个顶点只通过一条路径连接。 换句话说，任何一个没有简单环路的连通图都是一棵树。
给你一棵包含  `n`  个节点的树，标记为  `0`  到  `n - 1`  。给定数字  `n`  和一个有  `n - 1`  条无向边的  `edges`  列表（每一个边都是一对标签），其中  `edges[i] = [ai, bi]`  表示树中节点  `ai`  和  `bi`  之间存在一条无向边。
可选择树中任何一个节点作为根。当选择节点  `x`  作为根节点时，设结果树的高度为  `h`  。在所有可能的树中，具有最小高度的树（即， `min(h)` ）被称为  **最小高度树**  。
请你找到所有的  **最小高度树**  并按  **任意顺序**  返回它们的根节点标签列表。
树的  **高度**  是指根节点和叶子节点之间最长向下路径上边的数量。

 
 **示例 1：** 

```text
输入：n = 4, edges = [[1,0],[1,2],[1,3]]
输出：[1]
解释：如图所示，当根是标签为 1 的节点时，树的高度是 1 ，这是唯一的最小高度树。
```

 **示例 2：** 

```text
输入：n = 6, edges = [[3,0],[3,1],[3,2],[3,4],[5,4]]
输出：[3,4]
```

 

 **提示：** 

 `1 <= n <= 2 * 104` 
 `edges.length == n - 1` 
 `0 <= ai, bi < n` 
 `ai != bi` 
所有  `(ai, bi)`  互不相同
给定的输入  **保证**  是一棵树，并且  **不会有重复的边**

### Java 解法补充

#### 基础解法：枚举每个节点作为根并 BFS 求高度

算法思想：对每个根跑一次 BFS，取最大层数最小的节点。

```java
class Solution {
    public List<Integer> findMinHeightTrees(int n, int[][] edges) {
        List<Integer>[] graph = build(n, edges);
        int best = Integer.MAX_VALUE;
        List<Integer> ans = new ArrayList<>();
        for (int root = 0; root < n; root++) {
            int height = height(root, graph);
            if (height < best) {
                best = height;
                ans.clear();
            }
            if (height == best) ans.add(root);
        }
        return ans;
    }

    private int height(int root, List<Integer>[] graph) {
        boolean[] seen = new boolean[graph.length];
        Queue<Integer> q = new ArrayDeque<>();
        q.offer(root);
        seen[root] = true;
        int h = -1;
        while (!q.isEmpty()) {
            for (int size = q.size(); size > 0; size--) {
                int cur = q.poll();
                for (int next : graph[cur]) if (!seen[next]) {
                    seen[next] = true;
                    q.offer(next);
                }
            }
            h++;
        }
        return h;
    }

    private List<Integer>[] build(int n, int[][] edges) {
        List<Integer>[] graph = new ArrayList[n];
        for (int i = 0; i < n; i++) graph[i] = new ArrayList<>();
        for (int[] e : edges) {
            graph[e[0]].add(e[1]);
            graph[e[1]].add(e[0]);
        }
        return graph;
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(n)`。

#### 资深解法：拓扑剥叶子

算法思想：最小高度树的根是树中心；不断删除叶子，最后剩下 1 或 2 个中心。

```java
class Solution {
    public List<Integer> findMinHeightTrees(int n, int[][] edges) {
        if (n == 1) return List.of(0);
        List<Set<Integer>> graph = new ArrayList<>();
        for (int i = 0; i < n; i++) graph.add(new HashSet<>());
        for (int[] e : edges) {
            graph.get(e[0]).add(e[1]);
            graph.get(e[1]).add(e[0]);
        }
        List<Integer> leaves = new ArrayList<>();
        for (int i = 0; i < n; i++) if (graph.get(i).size() == 1) leaves.add(i);
        int remain = n;
        while (remain > 2) {
            remain -= leaves.size();
            List<Integer> nextLeaves = new ArrayList<>();
            for (int leaf : leaves) {
                int parent = graph.get(leaf).iterator().next();
                graph.get(parent).remove(leaf);
                if (graph.get(parent).size() == 1) nextLeaves.add(parent);
            }
            leaves = nextLeaves;
        }
        return leaves;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 基础语法与算法思想

- 树中心到最远叶子的距离最短，剥叶子可以从外向内找中心。

---

## 311. 稀疏矩阵的乘法 (Medium)

暂无内容描述。

### Java 解法补充

#### 基础解法：普通三重循环矩阵乘法

算法思想：直接按定义计算 `ans[i][j] = Σ mat1[i][k] * mat2[k][j]`。

```java
class Solution {
    public int[][] multiply(int[][] mat1, int[][] mat2) {
        int m = mat1.length, p = mat1[0].length, n = mat2[0].length;
        int[][] ans = new int[m][n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                for (int k = 0; k < p; k++) ans[i][j] += mat1[i][k] * mat2[k][j];
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(mpn)`，空间 `O(1)`，不计答案。

#### 资深解法：跳过零元素

算法思想：稀疏矩阵大量为 0，只在 `mat1[i][k] != 0` 时贡献整行结果。

```java
class Solution {
    public int[][] multiply(int[][] mat1, int[][] mat2) {
        int m = mat1.length, p = mat1[0].length, n = mat2[0].length;
        int[][] ans = new int[m][n];
        for (int i = 0; i < m; i++) {
            for (int k = 0; k < p; k++) {
                if (mat1[i][k] == 0) continue;
                for (int j = 0; j < n; j++) ans[i][j] += mat1[i][k] * mat2[k][j];
            }
        }
        return ans;
    }
}
```

复杂度：最坏 `O(mpn)`，稀疏场景实际更快，空间 `O(1)`。

#### 基础语法与算法思想

- 乘法中任何一项为 0 都不会贡献结果，稀疏题要主动跳过零。

---

## 312. 戳气球 (Hard)

有  `n`  个气球，编号为 `0`  到  `n - 1` ，每个气球上都标有一个数字，这些数字存在数组  `nums`  中。
现在要求你戳破所有的气球。戳破第  `i`  个气球，你可以获得  `nums[i - 1] * nums[i] * nums[i + 1]`  枚硬币。 这里的  `i - 1`  和  `i + 1`  代表和  `i`  相邻的两个气球的序号。如果  `i - 1` 或  `i + 1`  超出了数组的边界，那么就当它是一个数字为  `1`  的气球。
求所能获得硬币的最大数量。
 
 **示例 1：** 

```text
输入：nums = [3,1,5,8]
输出：167
解释：
nums = [3,1,5,8] --> [3,5,8] --> [3,8] --> [8] --> []
coins =  3*1*5    +   3*5*8   +  1*3*8  + 1*8*1 = 167
```

 **示例 2：** 

```text
输入：nums = [1,5]
输出：10
```

 
 **提示：** 

 `n == nums.length` 
 `1 <= n <= 300` 
 `0 <= nums[i] <= 100`

### Java 解法补充

#### 基础解法：递归枚举当前戳哪个气球

算法思想：每次从当前列表中选择一个气球戳破，递归计算剩余最大收益。

```java
class Solution {
    public int maxCoins(int[] nums) {
        List<Integer> list = new ArrayList<>();
        for (int x : nums) list.add(x);
        return dfs(list);
    }

    private int dfs(List<Integer> nums) {
        if (nums.isEmpty()) return 0;
        int ans = 0;
        for (int i = 0; i < nums.size(); i++) {
            int left = i == 0 ? 1 : nums.get(i - 1);
            int right = i == nums.size() - 1 ? 1 : nums.get(i + 1);
            int cur = left * nums.get(i) * right;
            int removed = nums.remove(i);
            ans = Math.max(ans, cur + dfs(nums));
            nums.add(i, removed);
        }
        return ans;
    }
}
```

复杂度：指数级，空间 `O(n)`。

#### 资深解法：区间 DP 枚举最后戳破的气球

算法思想：给两端补 1，`dp[l][r]` 表示开区间 `(l,r)` 内全部戳完的最大收益。

```java
class Solution {
    public int maxCoins(int[] nums) {
        int n = nums.length;
        int[] arr = new int[n + 2];
        arr[0] = arr[n + 1] = 1;
        for (int i = 0; i < n; i++) arr[i + 1] = nums[i];
        int[][] dp = new int[n + 2][n + 2];
        for (int len = 2; len < n + 2; len++) {
            for (int l = 0; l + len < n + 2; l++) {
                int r = l + len;
                for (int k = l + 1; k < r; k++) {
                    dp[l][r] = Math.max(dp[l][r], dp[l][k] + dp[k][r] + arr[l] * arr[k] * arr[r]);
                }
            }
        }
        return dp[0][n + 1];
    }
}
```

复杂度：时间 `O(n^3)`，空间 `O(n^2)`。

#### 基础语法与算法思想

- 反向思考“最后戳哪个”能让左右区间独立。

---

## 313. 超级丑数 (Medium)

**超级丑数**  是一个正整数，并满足其所有质因数都出现在质数数组  `primes`  中。
给你一个整数  `n`  和一个整数数组  `primes`  ，返回第  `n`  个  **超级丑数**  。
题目数据保证第  `n`  个  **超级丑数**  在  **32-bit**  带符号整数范围内。
 
 **示例 1：** 

```text
输入：n = 12, primes = [2,7,13,19]
输出：32 
解释：给定长度为 4 的质数数组 primes = [2,7,13,19]，前 12 个超级丑数序列为：[1,2,4,7,8,13,14,16,19,26,28,32] 。
```

 **示例 2：** 

```text
输入：n = 1, primes = [2,3,5]
输出：1
解释：1 不含质因数，因此它的所有质因数都在质数数组 primes = [2,3,5] 中。
```

 

 **提示：** 

 `1 <= n <= 105` 
 `1 <= primes.length <= 100` 
 `2 <= primes[i] <= 1000` 
题目数据 **保证**   `primes[i]`  是一个质数
 `primes`  中的所有值都  **互不相同**  ，且按  **递增顺序**  排列

### Java 解法补充

#### 基础解法：小根堆生成候选丑数

算法思想：从 1 出发，每次取最小候选，再乘所有质数生成下一批候选。

```java
class Solution {
    public int nthSuperUglyNumber(int n, int[] primes) {
        PriorityQueue<Long> heap = new PriorityQueue<>();
        Set<Long> seen = new HashSet<>();
        heap.offer(1L);
        seen.add(1L);
        long cur = 1;
        for (int i = 0; i < n; i++) {
            cur = heap.poll();
            for (int p : primes) {
                long next = cur * p;
                if (seen.add(next)) heap.offer(next);
            }
        }
        return (int) cur;
    }
}
```

复杂度：时间 `O(nk log(nk))`，空间 `O(nk)`。

#### 资深解法：多指针 DP

算法思想：每个质数维护一个指针，表示下一个候选为 `ugly[index[p]] * p`。

```java
class Solution {
    public int nthSuperUglyNumber(int n, int[] primes) {
        int k = primes.length;
        int[] idx = new int[k];
        int[] ugly = new int[n];
        ugly[0] = 1;
        for (int i = 1; i < n; i++) {
            long next = Long.MAX_VALUE;
            for (int j = 0; j < k; j++) next = Math.min(next, (long) ugly[idx[j]] * primes[j]);
            ugly[i] = (int) next;
            for (int j = 0; j < k; j++) {
                if ((long) ugly[idx[j]] * primes[j] == next) idx[j]++;
            }
        }
        return ugly[n - 1];
    }
}
```

复杂度：时间 `O(nk)`，空间 `O(n + k)`。

#### 基础语法与算法思想

- 多路有序序列合并可用堆，也可用多指针减少额外开销。

---

## 314. 二叉树的垂直遍历 (Medium)

暂无内容描述。

### Java 解法补充

#### 基础解法：BFS 记录每个节点所在列

算法思想：根节点列为 0，左子列减 1，右子列加 1，用有序映射按列收集。

```java
class Solution {
    public List<List<Integer>> verticalOrder(TreeNode root) {
        List<List<Integer>> ans = new ArrayList<>();
        if (root == null) return ans;
        Map<Integer, List<Integer>> map = new TreeMap<>();
        Queue<TreeNode> nodes = new ArrayDeque<>();
        Queue<Integer> cols = new ArrayDeque<>();
        nodes.offer(root);
        cols.offer(0);
        while (!nodes.isEmpty()) {
            TreeNode node = nodes.poll();
            int col = cols.poll();
            map.computeIfAbsent(col, k -> new ArrayList<>()).add(node.val);
            if (node.left != null) {
                nodes.offer(node.left);
                cols.offer(col - 1);
            }
            if (node.right != null) {
                nodes.offer(node.right);
                cols.offer(col + 1);
            }
        }
        ans.addAll(map.values());
        return ans;
    }
}
```

复杂度：时间 `O(n log n)`，空间 `O(n)`。

#### 资深解法：BFS 同时维护最小列和最大列

算法思想：用哈希表收集列，记录列范围，最后按范围输出，减少 `TreeMap` 的对数开销。

```java
class Solution {
    public List<List<Integer>> verticalOrder(TreeNode root) {
        List<List<Integer>> ans = new ArrayList<>();
        if (root == null) return ans;
        Map<Integer, List<Integer>> map = new HashMap<>();
        Queue<Object[]> q = new ArrayDeque<>();
        q.offer(new Object[]{root, 0});
        int min = 0, max = 0;
        while (!q.isEmpty()) {
            Object[] item = q.poll();
            TreeNode node = (TreeNode) item[0];
            int col = (int) item[1];
            map.computeIfAbsent(col, k -> new ArrayList<>()).add(node.val);
            min = Math.min(min, col);
            max = Math.max(max, col);
            if (node.left != null) q.offer(new Object[]{node.left, col - 1});
            if (node.right != null) q.offer(new Object[]{node.right, col + 1});
        }
        for (int c = min; c <= max; c++) ans.add(map.get(c));
        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 基础语法与算法思想

- BFS 能保证同列节点按从上到下、从左到右的顺序进入结果。

---

## 315. 计算右侧小于当前元素的个数 (Hard)

给你一个整数数组  `nums`  ，按要求返回一个新数组  `counts`  。数组  `counts`  有该性质：  `counts[i]`  的值是   `nums[i]`  右侧小于  `nums[i]`  的元素的数量。
 
 **示例 1：** 

```text
输入：nums = [5,2,6,1]
输出：[2,1,1,0] 
解释：
5 的右侧有 2 个更小的元素 (2 和 1)
2 的右侧仅有 1 个更小的元素 (1)
6 的右侧有 1 个更小的元素 (1)
1 的右侧有 0 个更小的元素
```

 **示例 2：** 

```text
输入：nums = [-1]
输出：[0]
```

 **示例 3：** 

```text
输入：nums = [-1,-1]
输出：[0,0]
```

 
 **提示：** 

 `1 <= nums.length <= 105` 
 `-104 <= nums[i] <= 104`

### Java 解法补充

#### 基础解法：双重循环

算法思想：对每个位置向右扫描，统计比它小的元素个数。

```java
class Solution {
    public List<Integer> countSmaller(int[] nums) {
        List<Integer> ans = new ArrayList<>();
        for (int i = 0; i < nums.length; i++) {
            int count = 0;
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[j] < nums[i]) count++;
            }
            ans.add(count);
        }
        return ans;
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(1)`，不计答案。

#### 资深解法：离散化加树状数组

算法思想：从右往左扫描，树状数组维护已出现数字频次，查询当前值左侧频次和。

```java
class Solution {
    public List<Integer> countSmaller(int[] nums) {
        int[] sorted = nums.clone();
        Arrays.sort(sorted);
        Map<Integer, Integer> rank = new HashMap<>();
        for (int x : sorted) rank.putIfAbsent(x, rank.size() + 1);
        int[] bit = new int[rank.size() + 1];
        Integer[] ans = new Integer[nums.length];
        for (int i = nums.length - 1; i >= 0; i--) {
            int r = rank.get(nums[i]);
            ans[i] = sum(bit, r - 1);
            add(bit, r, 1);
        }
        return Arrays.asList(ans);
    }

    private void add(int[] bit, int i, int delta) {
        while (i < bit.length) {
            bit[i] += delta;
            i += i & -i;
        }
    }

    private int sum(int[] bit, int i) {
        int ans = 0;
        while (i > 0) {
            ans += bit[i];
            i -= i & -i;
        }
        return ans;
    }
}
```

复杂度：时间 `O(n log n)`，空间 `O(n)`。

#### 基础语法与算法思想

- 从右往左时，树状数组里保存的正好是“右侧元素”。

---

## 316. 去除重复字母 (Medium)

给你一个字符串  `s`  ，请你去除字符串中重复的字母，使得每个字母只出现一次。需保证  **返回结果的字典序最小** （要求不能打乱其他字符的相对位置）。
 
 **示例 1：** 

```text
输入：s = "bcabc"
输出："abc"
```

 **示例 2：** 

```text
输入：s = "cbacdcbc"
输出："acdb"
```

 
 **提示：** 

 `1 <= s.length <= 104` 
 `s`  由小写英文字母组成

 
 **注意：** 该题与 1081 https://leetcode.cn/problems/smallest-subsequence-of-distinct-characters 相同

### Java 解法补充

#### 基础解法：递归选择当前能选的最小首字符

算法思想：每次在保证剩余字符仍能覆盖所有不同字母的前缀里，选择最小字符作为答案下一位。

```java
class Solution {
    public String removeDuplicateLetters(String s) {
        if (s.isEmpty()) return "";
        int[] count = new int[26];
        for (char c : s.toCharArray()) count[c - 'a']++;
        int pos = 0;
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) < s.charAt(pos)) pos = i;
            if (--count[s.charAt(i) - 'a'] == 0) break;
        }
        char pick = s.charAt(pos);
        String rest = s.substring(pos + 1).replace(String.valueOf(pick), "");
        return pick + removeDuplicateLetters(rest);
    }
}
```

复杂度：时间 `O(26n)` 级别，空间 `O(n)`。

#### 资深解法：单调栈

算法思想：若栈顶字符比当前字符大且后面还会再出现，就弹出栈顶，以获得更小字典序。

```java
class Solution {
    public String removeDuplicateLetters(String s) {
        int[] last = new int[26];
        for (int i = 0; i < s.length(); i++) last[s.charAt(i) - 'a'] = i;
        boolean[] used = new boolean[26];
        Deque<Character> stack = new ArrayDeque<>();
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (used[c - 'a']) continue;
            while (!stack.isEmpty() && stack.peek() > c && last[stack.peek() - 'a'] > i) {
                used[stack.pop() - 'a'] = false;
            }
            stack.push(c);
            used[c - 'a'] = true;
        }
        StringBuilder ans = new StringBuilder();
        while (!stack.isEmpty()) ans.append(stack.removeLast());
        return ans.toString();
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 单调栈解决“保留相对顺序且字典序最小”的典型题。

---

## 317. 离建筑物最近的距离 (Hard)

暂无内容描述。

### Java 解法补充

#### 基础解法：从每个空地 BFS 到所有建筑

算法思想：枚举每个空地，BFS 计算到所有建筑的距离和，能到达全部建筑才更新答案。

```java
class Solution {
    public int shortestDistance(int[][] grid) {
        int ans = Integer.MAX_VALUE, buildings = 0;
        for (int[] row : grid) for (int x : row) if (x == 1) buildings++;
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (grid[i][j] == 0) {
                    int[] res = bfs(grid, i, j);
                    if (res[1] == buildings) ans = Math.min(ans, res[0]);
                }
            }
        }
        return ans == Integer.MAX_VALUE ? -1 : ans;
    }

    private int[] bfs(int[][] grid, int sx, int sy) {
        int m = grid.length, n = grid[0].length, dist = 0, hit = 0;
        boolean[][] seen = new boolean[m][n];
        Queue<int[]> q = new ArrayDeque<>();
        q.offer(new int[]{sx, sy, 0});
        seen[sx][sy] = true;
        int[][] dirs = {{1,0},{-1,0},{0,1},{0,-1}};
        while (!q.isEmpty()) {
            int[] cur = q.poll();
            if (grid[cur[0]][cur[1]] == 1) {
                dist += cur[2];
                hit++;
                continue;
            }
            for (int[] d : dirs) {
                int x = cur[0] + d[0], y = cur[1] + d[1];
                if (x < 0 || x == m || y < 0 || y == n || seen[x][y] || grid[x][y] == 2) continue;
                seen[x][y] = true;
                q.offer(new int[]{x, y, cur[2] + 1});
            }
        }
        return new int[]{dist, hit};
    }
}
```

复杂度：时间 `O((mn)^2)`，空间 `O(mn)`。

#### 资深解法：从每个建筑 BFS 累加空地距离

算法思想：建筑数量通常少于空地总搜索量，从每栋建筑出发累计距离和到达次数。

```java
class Solution {
    public int shortestDistance(int[][] grid) {
        int m = grid.length, n = grid[0].length, buildings = 0;
        int[][] dist = new int[m][n], reach = new int[m][n];
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) {
            if (grid[i][j] == 1) {
                buildings++;
                bfs(grid, i, j, dist, reach);
            }
        }
        int ans = Integer.MAX_VALUE;
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) {
            if (grid[i][j] == 0 && reach[i][j] == buildings) ans = Math.min(ans, dist[i][j]);
        }
        return ans == Integer.MAX_VALUE ? -1 : ans;
    }

    private void bfs(int[][] grid, int sx, int sy, int[][] dist, int[][] reach) {
        int m = grid.length, n = grid[0].length;
        boolean[][] seen = new boolean[m][n];
        Queue<int[]> q = new ArrayDeque<>();
        q.offer(new int[]{sx, sy, 0});
        seen[sx][sy] = true;
        int[][] dirs = {{1,0},{-1,0},{0,1},{0,-1}};
        while (!q.isEmpty()) {
            int[] cur = q.poll();
            for (int[] d : dirs) {
                int x = cur[0] + d[0], y = cur[1] + d[1];
                if (x < 0 || x == m || y < 0 || y == n || seen[x][y] || grid[x][y] != 0) continue;
                seen[x][y] = true;
                dist[x][y] += cur[2] + 1;
                reach[x][y]++;
                q.offer(new int[]{x, y, cur[2] + 1});
            }
        }
    }
}
```

复杂度：时间 `O(Bmn)`，空间 `O(mn)`。

#### 基础语法与算法思想

- 多源累计类 BFS 常用 `dist` 和 `reach` 两张表。

---

## 318. 最大单词长度乘积 (Medium)

给你一个字符串数组  `words`  ，找出并返回  `length(words[i]) * length(words[j])`  的最大值，并且这两个单词不含有公共字母。如果不存在这样的两个单词，返回  `0`  。
 
 **示例 1：** 

```text
输入：words = ["abcw","baz","foo","bar","xtfn","abcdef"]
输出：16 
解释：这两个单词为 "abcw", "xtfn"。
```

 **示例 2：** 

```text
输入：words = ["a","ab","abc","d","cd","bcd","abcd"]
输出：4 
解释：这两个单词为 "ab", "cd"。
```

 **示例 3：** 

```text
输入：words = ["a","aa","aaa","aaaa"]
输出：0 
解释：不存在这样的两个单词。
```

 
 **提示：** 

 `2 <= words.length <= 1000` 
 `1 <= words[i].length <= 1000` 
 `words[i]`  仅包含小写字母

### Java 解法补充

#### 基础解法：两两比较并用集合判断是否有公共字母

算法思想：枚举两个单词，把第一个单词字符放入集合，再检查第二个单词是否命中。

```java
class Solution {
    public int maxProduct(String[] words) {
        int ans = 0;
        for (int i = 0; i < words.length; i++) {
            for (int j = i + 1; j < words.length; j++) {
                if (!share(words[i], words[j])) ans = Math.max(ans, words[i].length() * words[j].length());
            }
        }
        return ans;
    }

    private boolean share(String a, String b) {
        boolean[] seen = new boolean[26];
        for (char c : a.toCharArray()) seen[c - 'a'] = true;
        for (char c : b.toCharArray()) if (seen[c - 'a']) return true;
        return false;
    }
}
```

复杂度：时间 `O(n^2 * L)`，空间 `O(1)`。

#### 资深解法：位掩码压缩字符集合

算法思想：每个单词用 26 位整数表示字符集合，两个掩码按位与为 0 说明无公共字母。

```java
class Solution {
    public int maxProduct(String[] words) {
        int n = words.length;
        int[] mask = new int[n];
        for (int i = 0; i < n; i++) {
            for (char c : words[i].toCharArray()) mask[i] |= 1 << (c - 'a');
        }
        int ans = 0;
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                if ((mask[i] & mask[j]) == 0) ans = Math.max(ans, words[i].length() * words[j].length());
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(n^2 + nL)`，空间 `O(n)`。

#### 基础语法与算法思想

- 小写字母集合可以用整数位图高效表示。

---

## 319. 灯泡开关 (Medium)

初始时有  `n`  个灯泡处于关闭状态。第一轮，你将会打开所有灯泡。接下来的第二轮，你将会每两个灯泡关闭第二个。
第三轮，你每三个灯泡就切换第三个灯泡的开关（即，打开变关闭，关闭变打开）。第  `i`  轮，你每  `i`  个灯泡就切换第  `i`  个灯泡的开关。直到第  `n`  轮，你只需要切换最后一个灯泡的开关。
找出并返回  `n`  轮后有多少个亮着的灯泡。
 
 **示例 1：** 

```text
输入：n = 3
输出：1 
解释：
初始时, 灯泡状态 [关闭, 关闭, 关闭].
第一轮后, 灯泡状态 [开启, 开启, 开启].
第二轮后, 灯泡状态 [开启, 关闭, 开启].
第三轮后, 灯泡状态 [开启, 关闭, 关闭]. 

你应该返回 1，因为只有一个灯泡还亮着。
```

 **示例 2：** 

```text
输入：n = 0
输出：0
```

 **示例 3：** 

```text
输入：n = 1
输出：1
```

 
 **提示：** 

 `0 <= n <= 109`

### Java 解法补充

#### 基础解法：模拟每一轮开关

算法思想：用布尔数组表示灯泡状态，第 `round` 轮切换所有 `round` 的倍数位置。

```java
class Solution {
    public int bulbSwitch(int n) {
        boolean[] on = new boolean[n + 1];
        for (int round = 1; round <= n; round++) {
            for (int bulb = round; bulb <= n; bulb += round) on[bulb] = !on[bulb];
        }
        int ans = 0;
        for (boolean b : on) if (b) ans++;
        return ans;
    }
}
```

复杂度：时间 `O(n log n)`，空间 `O(n)`。

#### 资深解法：只剩完全平方数会亮

算法思想：灯泡被切换次数等于因子个数，只有完全平方数因子个数为奇数。

```java
class Solution {
    public int bulbSwitch(int n) {
        return (int) Math.sqrt(n);
    }
}
```

复杂度：时间 `O(1)`，空间 `O(1)`。

#### 基础语法与算法思想

- 数学化简常来自观察操作次数和因子数量。

---

## 320. 列举单词的全部缩写 (Medium)

暂无内容描述。

### Java 解法补充

#### 基础解法：回溯选择保留或缩写每个字符

算法思想：每个字符要么保留，要么计入连续缩写数字，递归到末尾收集。

```java
class Solution {
    public List<String> generateAbbreviations(String word) {
        List<String> ans = new ArrayList<>();
        dfs(word, 0, 0, new StringBuilder(), ans);
        return ans;
    }

    private void dfs(String word, int index, int count, StringBuilder path, List<String> ans) {
        int len = path.length();
        if (index == word.length()) {
            if (count > 0) path.append(count);
            ans.add(path.toString());
            path.setLength(len);
            return;
        }
        dfs(word, index + 1, count + 1, path, ans);
        if (count > 0) path.append(count);
        path.append(word.charAt(index));
        dfs(word, index + 1, 0, path, ans);
        path.setLength(len);
    }
}
```

复杂度：时间 `O(2^n * n)`，空间 `O(n)`，不计答案。

#### 资深解法：位掩码枚举

算法思想：二进制位为 1 表示缩写该字符，连续 1 合并成数字。

```java
class Solution {
    public List<String> generateAbbreviations(String word) {
        List<String> ans = new ArrayList<>();
        int total = 1 << word.length();
        for (int mask = 0; mask < total; mask++) ans.add(build(word, mask));
        return ans;
    }

    private String build(String word, int mask) {
        StringBuilder sb = new StringBuilder();
        int count = 0;
        for (int i = 0; i < word.length(); i++) {
            if ((mask & (1 << i)) != 0) count++;
            else {
                if (count > 0) sb.append(count);
                count = 0;
                sb.append(word.charAt(i));
            }
        }
        if (count > 0) sb.append(count);
        return sb.toString();
    }
}
```

复杂度：时间 `O(2^n * n)`，空间 `O(1)`，不计答案。

#### 基础语法与算法思想

- 子集枚举类问题可以用回溯，也可以用位掩码。

---

## 321. 拼接最大数 (Hard)

给你两个整数数组  `nums1`  和  `nums2` ，它们的长度分别为  `m`  和  `n` 。数组  `nums1`  和  `nums2`  分别代表两个数各位上的数字。同时你也会得到一个整数  `k` 。
请你利用这两个数组中的数字创建一个长度为  `k <= m + n`  的最大数。同一数组中数字的相对顺序必须保持不变。
返回代表答案的长度为  `k`  的数组。
 
 **示例 1：** 

```text
输入：nums1 = [3,4,6,5], nums2 = [9,1,2,5,8,3], k = 5
输出：[9,8,6,5,3]
```

 **示例 2：** 

```text
输入：nums1 = [6,7], nums2 = [6,0,4], k = 5
输出：[6,7,6,0,4]
```

 **示例 3：** 

```text
输入：nums1 = [3,9], nums2 = [8,9], k = 3
输出：[9,8,9]
```

 
 **提示：** 

 `m == nums1.length` 
 `n == nums2.length` 
 `1 <= m, n <= 500` 
 `0 <= nums1[i], nums2[i] <= 9` 
 `1 <= k <= m + n` 
 `nums1`  和  `nums2`  没有前导 0。

### Java 解法补充

#### 基础解法：枚举两个数组各取多少位

算法思想：固定从 `nums1` 取 `i` 位、从 `nums2` 取 `k-i` 位，分别取最大子序列后贪心合并。

```java
class Solution {
    public int[] maxNumber(int[] nums1, int[] nums2, int k) {
        int[] best = new int[k];
        for (int i = Math.max(0, k - nums2.length); i <= Math.min(k, nums1.length); i++) {
            int[] candidate = merge(maxSub(nums1, i), maxSub(nums2, k - i));
            if (greater(candidate, 0, best, 0)) best = candidate;
        }
        return best;
    }

    private int[] maxSub(int[] nums, int k) {
        int[] stack = new int[k];
        int top = 0, drop = nums.length - k;
        for (int x : nums) {
            while (top > 0 && drop > 0 && stack[top - 1] < x) {
                top--;
                drop--;
            }
            if (top < k) stack[top++] = x;
            else drop--;
        }
        return stack;
    }

    private int[] merge(int[] a, int[] b) {
        int[] ans = new int[a.length + b.length];
        int i = 0, j = 0, p = 0;
        while (i < a.length || j < b.length) ans[p++] = greater(a, i, b, j) ? a[i++] : b[j++];
        return ans;
    }

    private boolean greater(int[] a, int i, int[] b, int j) {
        while (i < a.length && j < b.length && a[i] == b[j]) {
            i++;
            j++;
        }
        return j == b.length || (i < a.length && a[i] > b[j]);
    }
}
```

复杂度：时间 `O(k(m+n+k))` 级别，空间 `O(k)`。

#### 资深解法：单调栈取子序列加字典序合并

算法思想：生产环境中会拆成“选最大子序列”和“字典序合并”两个可测函数，上面代码已经是该套路的实现。

```java
class Solution {
    public int[] maxNumber(int[] nums1, int[] nums2, int k) {
        int[] best = new int[k];
        for (int left = Math.max(0, k - nums2.length); left <= Math.min(k, nums1.length); left++) {
            int[] cur = merge(pick(nums1, left), pick(nums2, k - left));
            if (greater(cur, 0, best, 0)) best = cur;
        }
        return best;
    }

    private int[] pick(int[] nums, int k) {
        int[] stack = new int[k];
        int top = 0, drop = nums.length - k;
        for (int num : nums) {
            while (top > 0 && drop > 0 && stack[top - 1] < num) {
                top--;
                drop--;
            }
            if (top < k) stack[top++] = num;
            else drop--;
        }
        return stack;
    }

    private int[] merge(int[] a, int[] b) {
        int[] ans = new int[a.length + b.length];
        int i = 0, j = 0;
        for (int p = 0; p < ans.length; p++) {
            ans[p] = greater(a, i, b, j) ? a[i++] : b[j++];
        }
        return ans;
    }

    private boolean greater(int[] a, int i, int[] b, int j) {
        while (i < a.length && j < b.length && a[i] == b[j]) {
            i++;
            j++;
        }
        return j == b.length || (i < a.length && a[i] > b[j]);
    }
}
```

复杂度：时间 `O(k(m+n+k))`，空间 `O(k)`。

#### 基础语法与算法思想

- 单调栈可以在保持相对顺序的前提下删掉较小前缀。

---

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

### Java 解法补充

#### 基础解法：递归尝试每种硬币

算法思想：要凑出 `rest`，可以先拿任意一枚硬币，再递归求剩余金额。

```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int ans = dfs(coins, amount);
        return ans >= 1_000_000 ? -1 : ans;
    }

    private int dfs(int[] coins, int rest) {
        if (rest == 0) return 0;
        if (rest < 0) return 1_000_000;
        int ans = 1_000_000;
        for (int coin : coins) ans = Math.min(ans, dfs(coins, rest - coin) + 1);
        return ans;
    }
}
```

复杂度：指数级，空间 `O(amount)`。

#### 资深解法：一维动态规划

算法思想：`dp[x]` 表示凑出金额 `x` 的最少硬币数，枚举最后一枚硬币。

```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount + 1];
        Arrays.fill(dp, amount + 1);
        dp[0] = 0;
        for (int x = 1; x <= amount; x++) {
            for (int coin : coins) {
                if (x >= coin) dp[x] = Math.min(dp[x], dp[x - coin] + 1);
            }
        }
        return dp[amount] > amount ? -1 : dp[amount];
    }
}
```

复杂度：时间 `O(amount * n)`，空间 `O(amount)`。

#### 基础语法与算法思想

- 完全背包问题常用一维 DP 表示金额状态。

---

## 323. 无向图中连通分量的数目 (Medium)

暂无内容描述。

### Java 解法补充

#### 基础解法：建图后 DFS 统计连通块

算法思想：每遇到一个未访问节点，就从它出发 DFS，连通块数量加一。

```java
class Solution {
    public int countComponents(int n, int[][] edges) {
        List<Integer>[] graph = new ArrayList[n];
        for (int i = 0; i < n; i++) graph[i] = new ArrayList<>();
        for (int[] e : edges) {
            graph[e[0]].add(e[1]);
            graph[e[1]].add(e[0]);
        }
        boolean[] seen = new boolean[n];
        int ans = 0;
        for (int i = 0; i < n; i++) {
            if (!seen[i]) {
                ans++;
                dfs(i, graph, seen);
            }
        }
        return ans;
    }

    private void dfs(int node, List<Integer>[] graph, boolean[] seen) {
        seen[node] = true;
        for (int next : graph[node]) if (!seen[next]) dfs(next, graph, seen);
    }
}
```

复杂度：时间 `O(n + e)`，空间 `O(n + e)`。

#### 资深解法：并查集

算法思想：初始每个点都是一个分量，每成功合并两个不同分量，答案减一。

```java
class Solution {
    public int countComponents(int n, int[][] edges) {
        int[] parent = new int[n];
        for (int i = 0; i < n; i++) parent[i] = i;
        int count = n;
        for (int[] e : edges) {
            int a = find(parent, e[0]), b = find(parent, e[1]);
            if (a != b) {
                parent[a] = b;
                count--;
            }
        }
        return count;
    }

    private int find(int[] parent, int x) {
        if (parent[x] != x) parent[x] = find(parent, parent[x]);
        return parent[x];
    }
}
```

复杂度：近似 `O((n+e)α(n))`，空间 `O(n)`。

#### 基础语法与算法思想

- 静态图可 DFS，动态连通或合并关系可并查集。

---

## 324. 摆动排序 II (Medium)

给你一个整数数组  `nums` ，将它重新排列成  `nums[0] < nums[1] > nums[2] < nums[3]...`  的顺序。
你可以假设所有输入数组都可以得到满足题目要求的结果。
 
 **示例 1：** 

```text
输入：nums = [1,5,1,1,6,4]
输出：[1,6,1,5,1,4]
解释：[1,4,1,5,1,6] 同样是符合题目要求的结果，可以被判题程序接受。
```

 **示例 2：** 

```text
输入：nums = [1,3,2,2,3,1]
输出：[2,3,1,3,1,2]
```

 
 **提示：** 

 `1 <= nums.length <= 5 * 104` 
 `0 <= nums[i] <= 5000` 
题目数据保证，对于给定的输入  `nums`  ，总能产生满足题目要求的结果

 
 **进阶：** 你能用 O(n) 时间复杂度和 / 或原地 O(1) 额外空间来实现吗？

### Java 解法补充

#### 基础解法：排序后把较小半和较大半交错放置

算法思想：排序后从两个半区的末尾取数，偶数位放较小半，奇数位放较大半，减少相等元素相邻。

```java
class Solution {
    public void wiggleSort(int[] nums) {
        int[] sorted = nums.clone();
        Arrays.sort(sorted);
        int small = (nums.length - 1) / 2;
        int large = nums.length - 1;
        for (int i = 0; i < nums.length; i++) {
            nums[i] = (i % 2 == 0) ? sorted[small--] : sorted[large--];
        }
    }
}
```

复杂度：时间 `O(n log n)`，空间 `O(n)`。

#### 资深解法：三向切分快选中位数加虚拟下标

算法思想：先找中位数，再按虚拟下标 `(1 + 2*i) % (n|1)` 做三向切分，把大数放奇数位。

```java
class Solution {
    public void wiggleSort(int[] nums) {
        int[] copy = nums.clone();
        Arrays.sort(copy);
        int median = copy[nums.length / 2];
        int n = nums.length, left = 0, i = 0, right = n - 1;
        while (i <= right) {
            int vi = index(i, n);
            if (nums[vi] > median) swap(nums, index(left++, n), vi);
            else if (nums[vi] < median) swap(nums, vi, index(right--, n));
            else i++;
        }
    }

    private int index(int i, int n) {
        return (1 + 2 * i) % (n | 1);
    }

    private void swap(int[] nums, int i, int j) {
        int t = nums[i];
        nums[i] = nums[j];
        nums[j] = t;
    }
}
```

复杂度：排序取中位数版本时间 `O(n log n)`，切分 `O(n)`，空间 `O(n)`。

#### 基础语法与算法思想

- 摆动排序 II 的关键是把相同中位数分散到两侧位置。

---

## 325. 和等于 k 的最长子数组长度 (Medium)

暂无内容描述。

### Java 解法补充

#### 基础解法：枚举所有子数组

算法思想：固定左端点，向右累加子数组和，命中 `k` 时更新最大长度。

```java
class Solution {
    public int maxSubArrayLen(int[] nums, int k) {
        int ans = 0;
        for (int i = 0; i < nums.length; i++) {
            int sum = 0;
            for (int j = i; j < nums.length; j++) {
                sum += nums[j];
                if (sum == k) ans = Math.max(ans, j - i + 1);
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(1)`。

#### 资深解法：前缀和加最早出现位置

算法思想：若当前前缀和为 `sum`，需要找最早出现的 `sum-k`，长度才最大。

```java
class Solution {
    public int maxSubArrayLen(int[] nums, int k) {
        Map<Integer, Integer> first = new HashMap<>();
        first.put(0, -1);
        int sum = 0, ans = 0;
        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];
            if (first.containsKey(sum - k)) ans = Math.max(ans, i - first.get(sum - k));
            first.putIfAbsent(sum, i);
        }
        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 基础语法与算法思想

- 最长长度需要保留前缀和第一次出现的位置。

---

## 326. 3 的幂 (Easy)

给定一个整数，写一个函数来判断它是否是 3 的幂次方。如果是，返回  `true`  ；否则，返回  `false`  。
整数  `n`  是 3 的幂次方需满足：存在整数  `x`  使得  `n == 3x` 
 
 **示例 1：** 

```text
输入：n = 27
输出：true
```

 **示例 2：** 

```text
输入：n = 0
输出：false
```

 **示例 3：** 

```text
输入：n = 9
输出：true
```

 **示例 4：** 

```text
输入：n = 45
输出：false
```

 
 **提示：** 

 `-231 <= n <= 231 - 1` 

 
 **进阶：** 你能不使用循环或者递归来完成本题吗？

### Java 解法补充

#### 基础解法：不断除以 3

算法思想：正数只要能持续被 3 整除，最后变成 1 就是 3 的幂。

```java
class Solution {
    public boolean isPowerOfThree(int n) {
        if (n <= 0) return false;
        while (n % 3 == 0) n /= 3;
        return n == 1;
    }
}
```

复杂度：时间 `O(log n)`，空间 `O(1)`。

#### 资深解法：最大 3 的幂取模

算法思想：32 位整数内最大的 3 的幂是 `1162261467`，所有较小的 3 的幂都能整除它。

```java
class Solution {
    public boolean isPowerOfThree(int n) {
        return n > 0 && 1162261467 % n == 0;
    }
}
```

复杂度：时间 `O(1)`，空间 `O(1)`。

#### 基础语法与算法思想

- 固定整数范围内可以利用最大合法幂做整除判断。

---

## 327. 区间和的个数 (Hard)

给你一个整数数组  `nums`  以及两个整数  `lower`  和  `upper`  。求数组中，值位于范围  `[lower, upper]`  （包含  `lower`  和  `upper` ）之内的  **区间和的个数**  。
 **区间和**   `S(i, j)`  表示在  `nums`  中，位置从  `i`  到  `j`  的元素之和，包含  `i`  和  `j`  ( `i`  ≤  `j` )。
 
 **示例 1：** 

```text
输入：nums = [-2,5,-1], lower = -2, upper = 2
输出：3
解释：存在三个区间：[0,0]、[2,2] 和 [0,2] ，对应的区间和分别是：-2 、-1 、2 。
```

 **示例 2：** 

```text
输入：nums = [0], lower = 0, upper = 0
输出：1
```

 
 **提示：** 

 `1 <= nums.length <= 105` 
 `-231 <= nums[i] <= 231 - 1` 
 `-105 <= lower <= upper <= 105` 
题目数据保证答案是一个  **32 位**  的整数

### Java 解法补充

#### 基础解法：前缀和后枚举区间

算法思想：先算前缀和，再枚举所有 `(i,j)`，判断 `prefix[j]-prefix[i]` 是否在范围内。

```java
class Solution {
    public int countRangeSum(int[] nums, int lower, int upper) {
        long[] prefix = new long[nums.length + 1];
        for (int i = 0; i < nums.length; i++) prefix[i + 1] = prefix[i] + nums[i];
        int ans = 0;
        for (int i = 0; i < prefix.length; i++) {
            for (int j = i + 1; j < prefix.length; j++) {
                long sum = prefix[j] - prefix[i];
                if (sum >= lower && sum <= upper) ans++;
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(n)`。

#### 资深解法：归并排序计数

算法思想：对前缀和排序归并，统计右半区有多少值落在 `[left+lower, left+upper]`。

```java
class Solution {
    public int countRangeSum(int[] nums, int lower, int upper) {
        long[] prefix = new long[nums.length + 1];
        for (int i = 0; i < nums.length; i++) prefix[i + 1] = prefix[i] + nums[i];
        return sort(prefix, 0, prefix.length, lower, upper);
    }

    private int sort(long[] a, int l, int r, int lower, int upper) {
        if (r - l <= 1) return 0;
        int m = (l + r) / 2;
        int ans = sort(a, l, m, lower, upper) + sort(a, m, r, lower, upper);
        int lo = m, hi = m;
        for (int i = l; i < m; i++) {
            while (lo < r && a[lo] - a[i] < lower) lo++;
            while (hi < r && a[hi] - a[i] <= upper) hi++;
            ans += hi - lo;
        }
        Arrays.sort(a, l, r);
        return ans;
    }
}
```

复杂度：上面写法时间 `O(n log^2 n)`，可用手写归并优化到 `O(n log n)`，空间 `O(n)`。

#### 基础语法与算法思想

- 区间和计数常转成前缀和之间的差值计数。

---

## 328. 奇偶链表 (Medium)

给定单链表的头节点  `head`  ，将所有索引为奇数的节点和索引为偶数的节点分别分组，保持它们原有的相对顺序，然后把偶数索引节点分组连接到奇数索引节点分组之后，返回重新排序的链表。
 **第一个** 节点的索引被认为是  **奇数**  ，  **第二个** 节点的索引为  **偶数**  ，以此类推。
请注意，偶数组和奇数组内部的相对顺序应该与输入时保持一致。
你必须在  `O(1)`  的额外空间复杂度和  `O(n)`  的时间复杂度下解决这个问题。
 
 **示例 1:** 

```text
输入: head = [1,2,3,4,5]
输出: [1,3,5,2,4]
```

 **示例 2:** 

```text
输入: head = [2,1,3,5,6,4,7]
输出: [2,3,6,7,1,5,4]
```

 
 **提示:** 

 `n ==`  链表中的节点数
 `0 <= n <= 104` 
 `-106 <= Node.val <= 106`

### Java 解法补充

#### 基础解法：拆成奇数链表和偶数链表

算法思想：遍历原链表，把奇数位置节点接到奇链，偶数位置节点接到偶链，最后拼接。

```java
class Solution {
    public ListNode oddEvenList(ListNode head) {
        ListNode oddDummy = new ListNode(0), evenDummy = new ListNode(0);
        ListNode odd = oddDummy, even = evenDummy;
        int index = 1;
        while (head != null) {
            if (index % 2 == 1) {
                odd.next = head;
                odd = odd.next;
            } else {
                even.next = head;
                even = even.next;
            }
            head = head.next;
            index++;
        }
        even.next = null;
        odd.next = evenDummy.next;
        return oddDummy.next;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 资深解法：原地维护 odd/even 两条指针

算法思想：`odd` 串奇数位，`even` 串偶数位，`evenHead` 保存偶链头用于最后拼接。

```java
class Solution {
    public ListNode oddEvenList(ListNode head) {
        if (head == null) return null;
        ListNode odd = head, even = head.next, evenHead = even;
        while (even != null && even.next != null) {
            odd.next = even.next;
            odd = odd.next;
            even.next = odd.next;
            even = even.next;
        }
        odd.next = evenHead;
        return head;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 链表重排时先保存关键头节点，避免后续断链后找不回来。

---

## 329. 矩阵中的最长递增路径 (Hard)

给定一个  `m x n`  整数矩阵  `matrix`  ，找出其中  **最长递增路径**  的长度。
对于每个单元格，你可以往上，下，左，右四个方向移动。 你  **不能**  在  **对角线**  方向上移动或移动到  **边界外** （即不允许环绕）。
 
 **示例 1：** 

```text
输入：matrix = [[9,9,4],[6,6,8],[2,1,1]]
输出：4 
解释：最长递增路径为 [1, 2, 6, 9]。
```

 **示例 2：** 

```text
输入：matrix = [[3,4,5],[3,2,6],[2,2,1]]
输出：4 
解释：最长递增路径是 [3, 4, 5, 6]。注意不允许在对角线方向上移动。
```

 **示例 3：** 

```text
输入：matrix = [[1]]
输出：1
```

 
 **提示：** 

 `m == matrix.length` 
 `n == matrix[i].length` 
 `1 <= m, n <= 200` 
 `0 <= matrix[i][j] <= 231 - 1`

### Java 解法补充

#### 基础解法：从每个格子 DFS 搜索

算法思想：每个格子都作为起点向四周更大的值递归搜索，取最大路径长度。

```java
class Solution {
    private final int[][] dirs = {{1,0},{-1,0},{0,1},{0,-1}};

    public int longestIncreasingPath(int[][] matrix) {
        int ans = 0;
        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[0].length; j++) ans = Math.max(ans, dfs(matrix, i, j));
        }
        return ans;
    }

    private int dfs(int[][] matrix, int x, int y) {
        int best = 1;
        for (int[] d : dirs) {
            int nx = x + d[0], ny = y + d[1];
            if (nx < 0 || nx == matrix.length || ny < 0 || ny == matrix[0].length) continue;
            if (matrix[nx][ny] > matrix[x][y]) best = Math.max(best, 1 + dfs(matrix, nx, ny));
        }
        return best;
    }
}
```

复杂度：指数级重复搜索，空间 `O(mn)` 递归栈。

#### 资深解法：记忆化 DFS

算法思想：每个格子的最长递增路径只计算一次，后续直接复用。

```java
class Solution {
    private final int[][] dirs = {{1,0},{-1,0},{0,1},{0,-1}};
    private int[][] memo;

    public int longestIncreasingPath(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length, ans = 0;
        memo = new int[m][n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) ans = Math.max(ans, dfs(matrix, i, j));
        }
        return ans;
    }

    private int dfs(int[][] matrix, int x, int y) {
        if (memo[x][y] != 0) return memo[x][y];
        int best = 1;
        for (int[] d : dirs) {
            int nx = x + d[0], ny = y + d[1];
            if (nx >= 0 && nx < matrix.length && ny >= 0 && ny < matrix[0].length
                    && matrix[nx][ny] > matrix[x][y]) {
                best = Math.max(best, 1 + dfs(matrix, nx, ny));
            }
        }
        return memo[x][y] = best;
    }
}
```

复杂度：时间 `O(mn)`，空间 `O(mn)`。

#### 基础语法与算法思想

- 矩阵递增路径无环，适合 DFS 加记忆化。

---

## 330. 按要求补齐数组 (Hard)

给定一个已排序的正整数数组  `nums`  ，和一个正整数  `n`  。从  `[1, n]`  区间内选取任意个数字补充到 nums 中，使得  `[1, n]`  区间内的任何数字都可以用 nums 中某几个数字的和来表示。
请返回 满足上述要求的最少需要补充的数字个数 。
 
 **示例 1:** 

```text
输入: nums = [1,3], n = 6
输出: 1 
解释:
根据 nums 里现有的组合 [1], [3], [1,3]，可以得出 1, 3, 4。
现在如果我们将 2 添加到 nums 中， 组合变为: [1], [2], [3], [1,3], [2,3], [1,2,3]。
其和可以表示数字 1, 2, 3, 4, 5, 6，能够覆盖 [1, 6] 区间里所有的数。
所以我们最少需要添加一个数字。
```

 **示例 2:** 

```text
输入: nums = [1,5,10], n = 20
输出: 2
解释: 我们需要添加 [2,4]。
```

 **示例 3:** 

```text
输入: nums = [1,2,2], n = 5
输出: 0
```

 
 **提示：** 

 `1 <= nums.length <= 1000` 
 `1 <= nums[i] <= 104` 
 `nums`  按  **升序排列** 
 `1 <= n <= 231 - 1`

### Java 解法补充

#### 基础解法：用可达数组模拟覆盖范围

算法思想：对较小 `n` 可以用布尔数组记录哪些和可达，发现最小不可达数就补它。

```java
class Solution {
    public int minPatches(int[] nums, int n) {
        boolean[] can = new boolean[n + 1];
        can[0] = true;
        int ans = 0, i = 0;
        while (true) {
            int miss = 1;
            while (miss <= n && can[miss]) miss++;
            if (miss > n) return ans;
            int add = i < nums.length && nums[i] <= miss ? nums[i++] : miss;
            if (add == miss) ans++;
            for (int s = n; s >= add; s--) can[s] |= can[s - add];
        }
    }
}
```

复杂度：时间最坏 `O(n * patches)`，空间 `O(n)`。

#### 资深解法：贪心维护 `[1, miss)` 已覆盖

算法思想：若当前数字 `nums[i] <= miss`，覆盖扩展到 `miss + nums[i]`；否则必须补 `miss`，覆盖翻倍。

```java
class Solution {
    public int minPatches(int[] nums, int n) {
        long miss = 1;
        int i = 0, ans = 0;
        while (miss <= n) {
            if (i < nums.length && nums[i] <= miss) {
                miss += nums[i++];
            } else {
                miss += miss;
                ans++;
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(nums.length + log n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 当 `[1, miss)` 都可达时，补上 `miss` 能一次把覆盖扩到 `[1, 2*miss)`。

---

