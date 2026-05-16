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

### Java 解法补充

#### 基础解法：枚举排列后找下一个

算法思想：回溯生成所有排列并排序，找到当前排列后面的一个排列。这个方法只适合理解“字典序下一个”的含义，实际会超时。

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

class Solution {
    public void nextPermutation(int[] nums) {
        List<String> all = new ArrayList<>();
        boolean[] used = new boolean[nums.length];
        build(nums, used, new ArrayList<>(), all);
        Collections.sort(all);
        String cur = encode(nums);
        for (int i = 0; i < all.size(); i++) {
            if (all.get(i).equals(cur)) {
                decode(all.get((i + 1) % all.size()), nums);
                return;
            }
        }
    }

    private void build(int[] nums, boolean[] used, List<Integer> path, List<String> all) {
        if (path.size() == nums.length) {
            all.add(path.toString());
            return;
        }
        for (int i = 0; i < nums.length; i++) {
            if (used[i]) continue;
            used[i] = true;
            path.add(nums[i]);
            build(nums, used, path, all);
            path.remove(path.size() - 1);
            used[i] = false;
        }
    }

    private String encode(int[] nums) {
        List<Integer> list = new ArrayList<>();
        for (int x : nums) list.add(x);
        return list.toString();
    }

    private void decode(String s, int[] nums) {
        String[] parts = s.substring(1, s.length() - 1).split(", ");
        for (int i = 0; i < nums.length; i++) nums[i] = Integer.parseInt(parts[i]);
    }
}
```

复杂度：时间 `O(n! * n log(n!))`，空间 `O(n! * n)`。

#### 资深解法：从右向左找下降点

算法思想：从右往左找第一个 `nums[i] < nums[i + 1]` 的位置；再从右侧找第一个比它大的数交换；最后反转右侧后缀，让后缀变成最小字典序。

```java
class Solution {
    public void nextPermutation(int[] nums) {
        int i = nums.length - 2;
        while (i >= 0 && nums[i] >= nums[i + 1]) i--;
        if (i >= 0) {
            int j = nums.length - 1;
            while (nums[j] <= nums[i]) j--;
            swap(nums, i, j);
        }
        reverse(nums, i + 1, nums.length - 1);
    }

    private void reverse(int[] nums, int left, int right) {
        while (left < right) swap(nums, left++, right--);
    }

    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- `swap` 是数组原地交换的常用辅助函数。
- 后缀降序说明它已经是最大排列，反转后变成最小排列。
- 核心思想：下一个排列只需要改动最靠右的可增大位置。

---

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

### Java 解法补充

#### 基础解法：枚举子串并校验

算法思想：枚举所有偶数长度子串，用计数器判断括号是否合法，并更新最大长度。

```java
class Solution {
    public int longestValidParentheses(String s) {
        int ans = 0;
        for (int i = 0; i < s.length(); i++) {
            for (int j = i + 2; j <= s.length(); j += 2) {
                if (isValid(s, i, j)) ans = Math.max(ans, j - i);
            }
        }
        return ans;
    }

    private boolean isValid(String s, int left, int right) {
        int balance = 0;
        for (int i = left; i < right; i++) {
            balance += s.charAt(i) == '(' ? 1 : -1;
            if (balance < 0) return false;
        }
        return balance == 0;
    }
}
```

复杂度：时间 `O(n^3)`，空间 `O(1)`。

#### 资深解法：栈保存边界下标

算法思想：栈中保存未匹配的下标，先放 `-1` 作为哨兵。遇到 `(` 入栈；遇到 `)` 出栈，若栈空则把当前下标作为新边界，否则用当前下标减栈顶得到有效长度。

```java
import java.util.ArrayDeque;
import java.util.Deque;

class Solution {
    public int longestValidParentheses(String s) {
        Deque<Integer> stack = new ArrayDeque<>();
        stack.push(-1);
        int ans = 0;
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '(') {
                stack.push(i);
            } else {
                stack.pop();
                if (stack.isEmpty()) {
                    stack.push(i);
                } else {
                    ans = Math.max(ans, i - stack.peek());
                }
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 基础语法与算法思想

- `peek()` 只看栈顶但不弹出。
- 哨兵 `-1` 让从下标 0 开始的合法串也能统一计算。
- 核心思想：无效右括号是连续合法区间的分割边界。

---

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

### Java 解法补充

#### 基础解法：线性扫描

算法思想：直接遍历数组，找到 `target` 就返回下标，否则返回 `-1`。

```java
class Solution {
    public int search(int[] nums, int target) {
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == target) return i;
        }
        return -1;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 资深解法：二分判断有序半边

算法思想：旋转数组任意二分后，至少有一边是有序的。判断 `target` 是否在有序半边内，决定保留哪一边。

```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] == target) return mid;
            if (nums[left] <= nums[mid]) {
                if (nums[left] <= target && target < nums[mid]) right = mid - 1;
                else left = mid + 1;
            } else {
                if (nums[mid] < target && target <= nums[right]) left = mid + 1;
                else right = mid - 1;
            }
        }
        return -1;
    }
}
```

复杂度：时间 `O(log n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 二分循环常用 `left <= right` 表示闭区间。
- `nums[left] <= nums[mid]` 判断左半边是否有序。
- 核心思想：旋转不会破坏“至少一半有序”的性质。

---

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

### Java 解法补充

#### 基础解法：线性找左右边界

算法思想：遍历数组，第一次遇到目标值记录左边界，每次遇到目标值更新右边界。

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int first = -1;
        int last = -1;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == target) {
                if (first == -1) first = i;
                last = i;
            }
        }
        return new int[]{first, last};
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 资深解法：两次二分找边界

算法思想：用 `lowerBound(nums, target)` 找第一个大于等于 `target` 的位置；再找第一个大于等于 `target + 1` 的位置，右边界就是它前一位。

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int left = lowerBound(nums, target);
        if (left == nums.length || nums[left] != target) {
            return new int[]{-1, -1};
        }
        int right = lowerBound(nums, target + 1) - 1;
        return new int[]{left, right};
    }

    private int lowerBound(int[] nums, int target) {
        int left = 0;
        int right = nums.length;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] >= target) right = mid;
            else left = mid + 1;
        }
        return left;
    }
}
```

复杂度：时间 `O(log n)`，空间 `O(1)`。

#### 基础语法与算法思想

- `[left, right)` 是左闭右开二分写法。
- `lowerBound` 是查找插入位置的通用模板。
- 核心思想：边界二分不是找等于，而是找第一个满足条件的位置。

---

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

### Java 解法补充

#### 基础解法：顺序扫描

算法思想：从左到右找第一个大于等于 `target` 的位置；如果没有，就插入到数组末尾。

```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] >= target) return i;
        }
        return nums.length;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 资深解法：二分插入点

算法思想：查找第一个大于等于 `target` 的位置。二分结束时 `left` 就是插入位置。

```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        int left = 0;
        int right = nums.length;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] >= target) right = mid;
            else left = mid + 1;
        }
        return left;
    }
}
```

复杂度：时间 `O(log n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 右边界取 `nums.length` 时代表插入末尾也是合法答案。
- 本题是 `lowerBound` 模板的最直接应用。
- 核心思想：有序数组里找位置优先二分。

---

## 36. 有效的数独 (Medium)

请你判断一个  `9 x 9`  的数独是否有效。只需要 根据以下规则 ，验证已经填入的数字是否有效即可。

数字  `1-9`  在每一行只能出现一次。
数字  `1-9`  在每一列只能出现一次。
数字  `1-9`  在每一个以粗实线分隔的  `3x3`  宫内只能出现一次。（请参考示例图）

 
注意：

一个有效的数独（部分已被填充）不一定是可解的。
只需要根据以上规则，验证已经填入的数字是否有效即可。
空白格用  `'.'`  表示。

 
示例 1：

```text
输入：board = 
[["5","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
输出：true
```

示例 2：

```text
输入：board = 
[["8","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
输出：false
解释：除了第一行的第一个数字从 5 改为 8 以外，空格内其他数字均与 示例1 相同。 但由于位于左上角的 3x3 宫内有两个 8 存在, 因此这个数独是无效的。
```

 
提示：

 `board.length == 9` 
 `board[i].length == 9` 
 `board[i][j]`  是一位数字（ `1-9` ）或者  `'.'`

### Java 解法补充

#### 基础解法：用集合记录出现位置

算法思想：每个数字会属于一行、一列、一个宫。把 `数字@行`、`数字@列`、`数字@宫` 作为字符串键放入集合，若重复出现则无效。

```java
import java.util.HashSet;
import java.util.Set;

class Solution {
    public boolean isValidSudoku(char[][] board) {
        Set<String> seen = new HashSet<>();
        for (int r = 0; r < 9; r++) {
            for (int c = 0; c < 9; c++) {
                char ch = board[r][c];
                if (ch == '.') continue;
                int box = r / 3 * 3 + c / 3;
                if (!seen.add(ch + " in row " + r) ||
                        !seen.add(ch + " in col " + c) ||
                        !seen.add(ch + " in box " + box)) {
                    return false;
                }
            }
        }
        return true;
    }
}
```

复杂度：时间 `O(1)`，空间 `O(1)`，棋盘固定 81 格。

#### 资深解法：布尔数组

算法思想：用三个二维布尔数组分别记录行、列、宫内某个数字是否出现过，避免字符串创建。

```java
class Solution {
    public boolean isValidSudoku(char[][] board) {
        boolean[][] rows = new boolean[9][9];
        boolean[][] cols = new boolean[9][9];
        boolean[][] boxes = new boolean[9][9];

        for (int r = 0; r < 9; r++) {
            for (int c = 0; c < 9; c++) {
                if (board[r][c] == '.') continue;
                int digit = board[r][c] - '1';
                int box = r / 3 * 3 + c / 3;
                if (rows[r][digit] || cols[c][digit] || boxes[box][digit]) {
                    return false;
                }
                rows[r][digit] = cols[c][digit] = boxes[box][digit] = true;
            }
        }
        return true;
    }
}
```

复杂度：时间 `O(1)`，空间 `O(1)`。

#### 基础语法与算法思想

- `set.add(x)`：添加成功返回 `true`，已存在返回 `false`。
- `board[r][c] - '1'` 把字符 `'1'..'9'` 映射到下标 `0..8`。
- 核心思想：数独校验只检查已有数字是否冲突，不需要求解。

---

## 37. 解数独 (Hard)

编写一个程序，通过填充空格来解决数独问题。
数独的解法需 遵循如下规则：

数字  `1-9`  在每一行只能出现一次。
数字  `1-9`  在每一列只能出现一次。
数字  `1-9`  在每一个以粗实线分隔的  `3x3`  宫内只能出现一次。（请参考示例图）

数独部分空格内已填入了数字，空白格用  `'.'`  表示。
 

示例 1：

```text
输入：board = [["5","3",".",".","7",".",".",".","."],["6",".",".","1","9","5",".",".","."],[".","9","8",".",".",".",".","6","."],["8",".",".",".","6",".",".",".","3"],["4",".",".","8",".","3",".",".","1"],["7",".",".",".","2",".",".",".","6"],[".","6",".",".",".",".","2","8","."],[".",".",".","4","1","9",".",".","5"],[".",".",".",".","8",".",".","7","9"]]
输出：[["5","3","4","6","7","8","9","1","2"],["6","7","2","1","9","5","3","4","8"],["1","9","8","3","4","2","5","6","7"],["8","5","9","7","6","1","4","2","3"],["4","2","6","8","5","3","7","9","1"],["7","1","3","9","2","4","8","5","6"],["9","6","1","5","3","7","2","8","4"],["2","8","7","4","1","9","6","3","5"],["3","4","5","2","8","6","1","7","9"]]
解释：输入的数独如上图所示，唯一有效的解决方案如下所示：
```

 
提示：

 `board.length == 9` 
 `board[i].length == 9` 
 `board[i][j]`  是一位数字或者  `'.'` 
题目数据 保证 输入数独仅有一个解

### Java 解法补充

#### 基础解法：回溯逐格尝试

算法思想：从左到右、从上到下寻找空格，尝试填入 `1..9`，若当前数字不冲突就递归处理下一个空格。

```java
class Solution {
    public void solveSudoku(char[][] board) {
        solve(board);
    }

    private boolean solve(char[][] board) {
        for (int r = 0; r < 9; r++) {
            for (int c = 0; c < 9; c++) {
                if (board[r][c] != '.') continue;
                for (char ch = '1'; ch <= '9'; ch++) {
                    if (valid(board, r, c, ch)) {
                        board[r][c] = ch;
                        if (solve(board)) return true;
                        board[r][c] = '.';
                    }
                }
                return false;
            }
        }
        return true;
    }

    private boolean valid(char[][] board, int r, int c, char ch) {
        for (int i = 0; i < 9; i++) {
            if (board[r][i] == ch || board[i][c] == ch) return false;
            if (board[r / 3 * 3 + i / 3][c / 3 * 3 + i % 3] == ch) return false;
        }
        return true;
    }
}
```

复杂度：最坏指数级，空间 `O(81)` 递归栈。

#### 资深解法：布尔标记剪枝

算法思想：预处理行、列、宫已用数字，回溯时 `O(1)` 判断冲突，并只遍历空格列表。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    private final boolean[][] rows = new boolean[9][9];
    private final boolean[][] cols = new boolean[9][9];
    private final boolean[][] boxes = new boolean[9][9];
    private final List<int[]> blanks = new ArrayList<>();

    public void solveSudoku(char[][] board) {
        for (int r = 0; r < 9; r++) {
            for (int c = 0; c < 9; c++) {
                if (board[r][c] == '.') {
                    blanks.add(new int[]{r, c});
                } else {
                    int d = board[r][c] - '1';
                    rows[r][d] = cols[c][d] = boxes[box(r, c)][d] = true;
                }
            }
        }
        dfs(board, 0);
    }

    private boolean dfs(char[][] board, int idx) {
        if (idx == blanks.size()) return true;
        int r = blanks.get(idx)[0];
        int c = blanks.get(idx)[1];
        int b = box(r, c);
        for (int d = 0; d < 9; d++) {
            if (rows[r][d] || cols[c][d] || boxes[b][d]) continue;
            rows[r][d] = cols[c][d] = boxes[b][d] = true;
            board[r][c] = (char) ('1' + d);
            if (dfs(board, idx + 1)) return true;
            board[r][c] = '.';
            rows[r][d] = cols[c][d] = boxes[b][d] = false;
        }
        return false;
    }

    private int box(int r, int c) {
        return r / 3 * 3 + c / 3;
    }
}
```

复杂度：最坏指数级，但常数更小；空间 `O(81)`。

#### 基础语法与算法思想

- `List<int[]>` 可保存空格坐标。
- 回溯中设置标记、递归、撤销标记三步必须对称。
- 核心思想：约束满足问题用“状态标记 + 回溯剪枝”。

---

## 38. 外观数列 (Medium)

「外观数列」是一个数位字符串序列，由递归公式定义：

 `countAndSay(1) = "1"` 
 `countAndSay(n)`  是  `countAndSay(n-1)`  的行程长度编码。

 

行程长度编码（RLE）是一种字符串压缩方法，其工作原理是通过将连续相同字符（重复两次或更多次）替换为字符重复次数（运行长度）和字符的串联。例如，要压缩字符串  `"3322251"`  ，我们将  `"33"`  用  `"23"`  替换，将  `"222"`  用  `"32"`  替换，将  `"5"`  用  `"15"`  替换并将  `"1"`  用  `"11"`  替换。因此压缩后字符串变为  `"23321511"` 。
给定一个整数  `n`  ，返回 外观数列 的第  `n`  个元素。
示例 1：

输入：n = 4
输出："1211"
解释：
countAndSay(1) = "1"
countAndSay(2) = "1" 的行程长度编码 = "11"
countAndSay(3) = "11" 的行程长度编码 = "21"
countAndSay(4) = "21" 的行程长度编码 = "1211"

示例 2：

输入：n = 1
输出："1"
解释：
这是基本情况。

 
提示：

 `1 <= n <= 30` 

 
进阶：你能迭代解决该问题吗？

### Java 解法补充

#### 基础解法：递归生成上一项

算法思想：第 `n` 项由第 `n - 1` 项读数得到，递归拿到上一项后统计连续相同字符。

```java
class Solution {
    public String countAndSay(int n) {
        if (n == 1) return "1";
        return describe(countAndSay(n - 1));
    }

    private String describe(String s) {
        StringBuilder ans = new StringBuilder();
        for (int i = 0; i < s.length();) {
            int j = i;
            while (j < s.length() && s.charAt(j) == s.charAt(i)) j++;
            ans.append(j - i).append(s.charAt(i));
            i = j;
        }
        return ans.toString();
    }
}
```

复杂度：时间与生成字符串总长度成正比，空间为递归栈和字符串长度。

#### 资深解法：迭代滚动生成

算法思想：从 `"1"` 开始迭代 `n - 1` 次，每次把当前字符串压缩描述成下一项，避免递归栈。

```java
class Solution {
    public String countAndSay(int n) {
        String cur = "1";
        for (int step = 2; step <= n; step++) {
            StringBuilder next = new StringBuilder();
            for (int i = 0; i < cur.length();) {
                int j = i;
                while (j < cur.length() && cur.charAt(j) == cur.charAt(i)) j++;
                next.append(j - i).append(cur.charAt(i));
                i = j;
            }
            cur = next.toString();
        }
        return cur;
    }
}
```

复杂度：时间与生成字符串总长度成正比，空间为当前项长度。

#### 基础语法与算法思想

- `append(int).append(char)` 可以链式追加。
- 双指针 `i/j` 适合统计连续相同段。
- 核心思想：递推字符串题先明确“上一项到下一项”的转换函数。

---

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

### Java 解法补充

#### 基础解法：回溯选择每个候选数次数

算法思想：对每个候选数，可以选择 0 次、1 次或多次，只要剩余目标不为负就继续搜索。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> ans = new ArrayList<>();
        dfs(candidates, 0, target, new ArrayList<>(), ans);
        return ans;
    }

    private void dfs(int[] nums, int index, int remain, List<Integer> path, List<List<Integer>> ans) {
        if (remain == 0) {
            ans.add(new ArrayList<>(path));
            return;
        }
        if (index == nums.length || remain < 0) return;
        dfs(nums, index + 1, remain, path, ans);
        path.add(nums[index]);
        dfs(nums, index, remain - nums[index], path, ans);
        path.remove(path.size() - 1);
    }
}
```

复杂度：指数级，空间为递归深度和答案。

#### 资深解法：排序后从当前位置枚举

算法思想：排序后在同一层从 `start` 向后选择，递归仍传当前下标 `i`，表示数字可以重复使用；超过剩余值时剪枝。

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        Arrays.sort(candidates);
        List<List<Integer>> ans = new ArrayList<>();
        backtrack(candidates, 0, target, new ArrayList<>(), ans);
        return ans;
    }

    private void backtrack(int[] nums, int start, int remain, List<Integer> path, List<List<Integer>> ans) {
        if (remain == 0) {
            ans.add(new ArrayList<>(path));
            return;
        }
        for (int i = start; i < nums.length && nums[i] <= remain; i++) {
            path.add(nums[i]);
            backtrack(nums, i, remain - nums[i], path, ans);
            path.remove(path.size() - 1);
        }
    }
}
```

复杂度：指数级，排序额外 `O(n log n)`。

#### 基础语法与算法思想

- `new ArrayList<>(path)`：收集答案时复制路径，避免后续回溯修改。
- `start` 控制组合顺序，避免 `[2,3]` 和 `[3,2]` 重复。
- 核心思想：组合题关心选择集合，不关心排列顺序。

---

## 40. 组合总和 II (Medium)

给定一个候选人编号的集合  `candidates`  和一个目标数  `target`  ，找出  `candidates`  中所有可以使数字和为  `target`  的组合。
 `candidates`  中的每个数字在每个组合中只能使用 一次 。
注意：解集不能包含重复的组合。 
 
示例 1:

```text
输入: candidates = [10,1,2,7,6,1,5], target = 8,
输出:
[
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]
```

示例 2:

```text
输入: candidates = [2,5,2,1,2], target = 5,
输出:
[
[1,2,2],
[5]
]
```

 
提示:

 `1 <= candidates.length <= 100` 
 `1 <= candidates[i] <= 50` 
 `1 <= target <= 30`

### Java 解法补充

#### 基础解法：枚举子集并用集合去重

算法思想：每个数只能用一次，递归决定选或不选，命中目标时把路径放入集合去重。

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashSet;
import java.util.List;
import java.util.Set;

class Solution {
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        Arrays.sort(candidates);
        Set<List<Integer>> set = new HashSet<>();
        dfs(candidates, 0, target, new ArrayList<>(), set);
        return new ArrayList<>(set);
    }

    private void dfs(int[] nums, int index, int remain, List<Integer> path, Set<List<Integer>> set) {
        if (remain == 0) {
            set.add(new ArrayList<>(path));
            return;
        }
        if (index == nums.length || remain < 0) return;
        dfs(nums, index + 1, remain, path, set);
        path.add(nums[index]);
        dfs(nums, index + 1, remain - nums[index], path, set);
        path.remove(path.size() - 1);
    }
}
```

复杂度：时间 `O(2^n * n)`，空间 `O(2^n)`。

#### 资深解法：排序回溯并同层跳重

算法思想：排序后同一层如果当前数等于前一个数，就跳过，避免生成重复组合；递归时传 `i + 1`，保证每个数只用一次。

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

class Solution {
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        Arrays.sort(candidates);
        List<List<Integer>> ans = new ArrayList<>();
        backtrack(candidates, 0, target, new ArrayList<>(), ans);
        return ans;
    }

    private void backtrack(int[] nums, int start, int remain, List<Integer> path, List<List<Integer>> ans) {
        if (remain == 0) {
            ans.add(new ArrayList<>(path));
            return;
        }
        for (int i = start; i < nums.length && nums[i] <= remain; i++) {
            if (i > start && nums[i] == nums[i - 1]) continue;
            path.add(nums[i]);
            backtrack(nums, i + 1, remain - nums[i], path, ans);
            path.remove(path.size() - 1);
        }
    }
}
```

复杂度：指数级，空间为递归深度和答案。

#### 基础语法与算法思想

- `i > start && nums[i] == nums[i - 1]` 是同层去重模板。
- `i + 1` 表示当前元素不能被重复使用。
- 核心思想：排序把重复元素放在一起，使去重规则可局部判断。

---

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

### Java 解法补充

#### 基础解法：哈希集合

算法思想：把所有正数放入集合，然后从 `1` 开始递增查找第一个不存在的正整数。

```java
import java.util.HashSet;
import java.util.Set;

class Solution {
    public int firstMissingPositive(int[] nums) {
        Set<Integer> set = new HashSet<>();
        for (int num : nums) {
            if (num > 0) set.add(num);
        }
        int ans = 1;
        while (set.contains(ans)) ans++;
        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 资深解法：原地哈希

算法思想：长度为 `n` 的数组只需要关心 `1..n+1`。把值 `x` 放到下标 `x - 1`，最后第一个位置不匹配的下标就是缺失答案。

```java
class Solution {
    public int firstMissingPositive(int[] nums) {
        int n = nums.length;
        for (int i = 0; i < n; i++) {
            while (nums[i] >= 1 && nums[i] <= n && nums[nums[i] - 1] != nums[i]) {
                swap(nums, i, nums[i] - 1);
            }
        }
        for (int i = 0; i < n; i++) {
            if (nums[i] != i + 1) return i + 1;
        }
        return n + 1;
    }

    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- `nums[nums[i] - 1]` 是把值映射到它应该在的位置。
- `while` 交换要防止重复值导致死循环。
- 核心思想：把数组本身当哈希表，值 `x` 应放在下标 `x - 1`。

---

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

### Java 解法补充

#### 基础解法：每列找左右最高柱

算法思想：第 `i` 列能接的水由左侧最高柱和右侧最高柱中的较小值决定，再减去当前高度。

```java
class Solution {
    public int trap(int[] height) {
        int ans = 0;
        for (int i = 0; i < height.length; i++) {
            int leftMax = 0;
            int rightMax = 0;
            for (int l = 0; l <= i; l++) leftMax = Math.max(leftMax, height[l]);
            for (int r = i; r < height.length; r++) rightMax = Math.max(rightMax, height[r]);
            ans += Math.min(leftMax, rightMax) - height[i];
        }
        return ans;
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(1)`。

#### 资深解法：双指针

算法思想：左右指针向中间收缩，维护 `leftMax` 和 `rightMax`。较低的一侧可以确定当前接水量，因为它的短板已确定。

```java
class Solution {
    public int trap(int[] height) {
        int left = 0;
        int right = height.length - 1;
        int leftMax = 0;
        int rightMax = 0;
        int ans = 0;

        while (left < right) {
            leftMax = Math.max(leftMax, height[left]);
            rightMax = Math.max(rightMax, height[right]);
            if (leftMax < rightMax) {
                ans += leftMax - height[left];
                left++;
            } else {
                ans += rightMax - height[right];
                right--;
            }
        }

        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 每列接水量不能为负，因为当前高度一定不超过自身参与的最大值。
- 双指针适合左右边界共同决定答案的问题。
- 核心思想：接水由较低侧短板决定。

---

## 43. 字符串相乘 (Medium)

给定两个以字符串形式表示的非负整数  `num1`  和  `num2` ，返回  `num1`  和  `num2`  的乘积，它们的乘积也表示为字符串形式。
注意：不能使用任何内置的 BigInteger 库或直接将输入转换为整数。
 
示例 1:

```text
输入: num1 = "2", num2 = "3"
输出: "6"
```

示例 2:

```text
输入: num1 = "123", num2 = "456"
输出: "56088"
```

 
提示：

 `1 <= num1.length, num2.length <= 200` 
 `num1`  和  `num2`  只能由数字组成。
 `num1`  和  `num2`  都不包含任何前导零，除了数字0本身。

### Java 解法补充

#### 基础解法：逐位乘一个数再累加

算法思想：用 `num2` 的每一位去乘 `num1`，得到一行部分积，再按位数补零，最后用字符串加法累加所有部分积。

```java
class Solution {
    public String multiply(String num1, String num2) {
        if (num1.equals("0") || num2.equals("0")) return "0";
        String ans = "0";
        for (int i = num2.length() - 1; i >= 0; i--) {
            String part = multiplyOne(num1, num2.charAt(i) - '0', num2.length() - 1 - i);
            ans = add(ans, part);
        }
        return ans;
    }

    private String multiplyOne(String num, int digit, int zeros) {
        StringBuilder sb = new StringBuilder();
        int carry = 0;
        for (int i = num.length() - 1; i >= 0; i--) {
            int product = (num.charAt(i) - '0') * digit + carry;
            sb.append(product % 10);
            carry = product / 10;
        }
        if (carry > 0) sb.append(carry);
        sb.reverse();
        while (zeros-- > 0) sb.append('0');
        return sb.toString();
    }

    private String add(String a, String b) {
        StringBuilder sb = new StringBuilder();
        int i = a.length() - 1;
        int j = b.length() - 1;
        int carry = 0;
        while (i >= 0 || j >= 0 || carry != 0) {
            int sum = carry;
            if (i >= 0) sum += a.charAt(i--) - '0';
            if (j >= 0) sum += b.charAt(j--) - '0';
            sb.append(sum % 10);
            carry = sum / 10;
        }
        return sb.reverse().toString();
    }
}
```

复杂度：时间 `O(mn + n(m+n))`，空间 `O(m+n)`。

#### 资深解法：数组模拟竖式乘法

算法思想：`num1[i] * num2[j]` 的结果会贡献到答案数组的 `i + j + 1` 位，并把进位传到 `i + j`。

```java
class Solution {
    public String multiply(String num1, String num2) {
        if (num1.equals("0") || num2.equals("0")) return "0";
        int m = num1.length();
        int n = num2.length();
        int[] res = new int[m + n];

        for (int i = m - 1; i >= 0; i--) {
            for (int j = n - 1; j >= 0; j--) {
                int product = (num1.charAt(i) - '0') * (num2.charAt(j) - '0');
                int sum = product + res[i + j + 1];
                res[i + j + 1] = sum % 10;
                res[i + j] += sum / 10;
            }
        }

        StringBuilder sb = new StringBuilder();
        int i = 0;
        while (i < res.length && res[i] == 0) i++;
        while (i < res.length) sb.append(res[i++]);
        return sb.toString();
    }
}
```

复杂度：时间 `O(mn)`，空间 `O(m+n)`。

#### 基础语法与算法思想

- 字符数字转整数使用 `c - '0'`。
- `StringBuilder.reverse()` 可以反转构造出的低位到高位结果。
- 核心思想：字符串大数题按小学竖式模拟，避免整数溢出。

---

## 44. 通配符匹配 (Hard)

给你一个输入字符串 ( `s` ) 和一个字符模式 ( `p` ) ，请你实现一个支持  `'?'`  和  `'*'`  匹配规则的通配符匹配：

 `'?'`  可以匹配任何单个字符。
 `'*'`  可以匹配任意字符序列（包括空字符序列）。

判定匹配成功的充要条件是：字符模式必须能够 完全匹配 输入字符串（而不是部分匹配）。

 

示例 1：

```text
输入：s = "aa", p = "a"
输出：false
解释："a" 无法匹配 "aa" 整个字符串。
```

示例 2：

```text
输入：s = "aa", p = "*"
输出：true
解释：'*' 可以匹配任意字符串。
```

示例 3：

```text
输入：s = "cb", p = "?a"
输出：false
解释：'?' 可以匹配 'c', 但第二个 'a' 无法匹配 'b'。
```

 
提示：

 `0 <= s.length, p.length <= 2000` 
 `s`  仅由小写英文字母组成
 `p`  仅由小写英文字母、 `'?'`  或  `'*'`  组成

### Java 解法补充

#### 基础解法：记忆化递归

算法思想：定义 `dfs(i, j)` 表示 `s[i:]` 能否匹配 `p[j:]`。普通字符和 `?` 消耗一个字符；`*` 可以匹配空串，也可以匹配一个字符后继续停在 `*`。

```java
class Solution {
    private Boolean[][] memo;

    public boolean isMatch(String s, String p) {
        memo = new Boolean[s.length() + 1][p.length() + 1];
        return dfs(s, p, 0, 0);
    }

    private boolean dfs(String s, String p, int i, int j) {
        if (memo[i][j] != null) return memo[i][j];
        boolean ans;
        if (j == p.length()) {
            ans = i == s.length();
        } else if (p.charAt(j) == '*') {
            ans = dfs(s, p, i, j + 1) || (i < s.length() && dfs(s, p, i + 1, j));
        } else {
            ans = i < s.length() &&
                    (p.charAt(j) == '?' || p.charAt(j) == s.charAt(i)) &&
                    dfs(s, p, i + 1, j + 1);
        }
        memo[i][j] = ans;
        return ans;
    }
}
```

复杂度：时间 `O(mn)`，空间 `O(mn)`。

#### 资深解法：贪心双指针

算法思想：记录最近一个 `*` 的位置和它匹配到的字符串位置。普通匹配失败时，如果前面有 `*`，就让该 `*` 多吞一个字符后继续尝试。

```java
class Solution {
    public boolean isMatch(String s, String p) {
        int i = 0;
        int j = 0;
        int star = -1;
        int match = 0;

        while (i < s.length()) {
            if (j < p.length() && (p.charAt(j) == '?' || p.charAt(j) == s.charAt(i))) {
                i++;
                j++;
            } else if (j < p.length() && p.charAt(j) == '*') {
                star = j++;
                match = i;
            } else if (star != -1) {
                j = star + 1;
                match++;
                i = match;
            } else {
                return false;
            }
        }

        while (j < p.length() && p.charAt(j) == '*') j++;
        return j == p.length();
    }
}
```

复杂度：时间 `O(m+n)`，空间 `O(1)`。

#### 基础语法与算法思想

- `Boolean[][]` 可用 `null` 区分未计算状态。
- `*` 的特殊性在于它可以匹配任意长度，包括 0。
- 核心思想：通配符里的 `*` 可以用贪心回退点覆盖大量 DP 状态。

---

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

### Java 解法补充

#### 基础解法：动态规划

算法思想：`dp[i]` 表示到达下标 `i` 的最少跳数。枚举所有能从 `j` 跳到 `i` 的位置，更新最小值。

```java
import java.util.Arrays;

class Solution {
    public int jump(int[] nums) {
        int n = nums.length;
        int[] dp = new int[n];
        Arrays.fill(dp, n);
        dp[0] = 0;
        for (int i = 1; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (j + nums[j] >= i) {
                    dp[i] = Math.min(dp[i], dp[j] + 1);
                }
            }
        }
        return dp[n - 1];
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(n)`。

#### 资深解法：贪心按层扩张

算法思想：把每次跳跃能到达的区间看成一层。遍历当前层内所有位置，维护下一层最远位置；走到当前层末尾时，跳数加一。

```java
class Solution {
    public int jump(int[] nums) {
        int steps = 0;
        int end = 0;
        int farthest = 0;
        for (int i = 0; i < nums.length - 1; i++) {
            farthest = Math.max(farthest, i + nums[i]);
            if (i == end) {
                steps++;
                end = farthest;
            }
        }
        return steps;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- `Arrays.fill(dp, n)` 用较大值初始化 DP 数组。
- `end` 表示当前跳数能覆盖的最远边界。
- 核心思想：最少跳数等价于 BFS 层数，贪心维护每层最远覆盖。

---

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

### Java 解法补充

#### 基础解法：路径加 used 数组

算法思想：每一层选择一个还没使用过的数字加入路径，路径长度等于数组长度时收集答案。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> ans = new ArrayList<>();
        backtrack(nums, new boolean[nums.length], new ArrayList<>(), ans);
        return ans;
    }

    private void backtrack(int[] nums, boolean[] used, List<Integer> path, List<List<Integer>> ans) {
        if (path.size() == nums.length) {
            ans.add(new ArrayList<>(path));
            return;
        }
        for (int i = 0; i < nums.length; i++) {
            if (used[i]) continue;
            used[i] = true;
            path.add(nums[i]);
            backtrack(nums, used, path, ans);
            path.remove(path.size() - 1);
            used[i] = false;
        }
    }
}
```

复杂度：时间 `O(n! * n)`，空间 `O(n)`，不计答案。

#### 资深解法：原地交换

算法思想：固定前缀位置 `index`，把后面的每个数交换到当前位置，递归处理下一位，返回后交换回来。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> ans = new ArrayList<>();
        backtrack(nums, 0, ans);
        return ans;
    }

    private void backtrack(int[] nums, int index, List<List<Integer>> ans) {
        if (index == nums.length) {
            List<Integer> one = new ArrayList<>();
            for (int num : nums) one.add(num);
            ans.add(one);
            return;
        }
        for (int i = index; i < nums.length; i++) {
            swap(nums, index, i);
            backtrack(nums, index + 1, ans);
            swap(nums, index, i);
        }
    }

    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

复杂度：时间 `O(n! * n)`，空间 `O(n)`。

#### 基础语法与算法思想

- `boolean[] used` 记录路径中已经选择的元素。
- 原地交换可以减少额外路径数组。
- 核心思想：排列题每一层关心“当前位置放哪个数”。

---

## 47. 全排列 II (Medium)

给定一个可包含重复数字的序列  `nums`  ，按任意顺序 返回所有不重复的全排列。
 
示例 1：

```text
输入：nums = [1,1,2]
输出：
[[1,1,2],
 [1,2,1],
 [2,1,1]]
```

示例 2：

```text
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

 
提示：

 `1 <= nums.length <= 8` 
 `-10 <= nums[i] <= 10`

### Java 解法补充

#### 基础解法：生成后用集合去重

算法思想：按普通全排列生成所有结果，把每个排列放进集合去重。

```java
import java.util.ArrayList;
import java.util.HashSet;
import java.util.List;
import java.util.Set;

class Solution {
    public List<List<Integer>> permuteUnique(int[] nums) {
        Set<List<Integer>> set = new HashSet<>();
        backtrack(nums, new boolean[nums.length], new ArrayList<>(), set);
        return new ArrayList<>(set);
    }

    private void backtrack(int[] nums, boolean[] used, List<Integer> path, Set<List<Integer>> set) {
        if (path.size() == nums.length) {
            set.add(new ArrayList<>(path));
            return;
        }
        for (int i = 0; i < nums.length; i++) {
            if (used[i]) continue;
            used[i] = true;
            path.add(nums[i]);
            backtrack(nums, used, path, set);
            path.remove(path.size() - 1);
            used[i] = false;
        }
    }
}
```

复杂度：时间 `O(n! * n)`，空间 `O(n! * n)`。

#### 资深解法：排序后同层跳重

算法思想：排序后，相同数字在同一层只允许使用第一个还没被跳过的副本。若 `nums[i] == nums[i - 1]` 且前一个还未使用，则跳过当前。

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

class Solution {
    public List<List<Integer>> permuteUnique(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> ans = new ArrayList<>();
        backtrack(nums, new boolean[nums.length], new ArrayList<>(), ans);
        return ans;
    }

    private void backtrack(int[] nums, boolean[] used, List<Integer> path, List<List<Integer>> ans) {
        if (path.size() == nums.length) {
            ans.add(new ArrayList<>(path));
            return;
        }
        for (int i = 0; i < nums.length; i++) {
            if (used[i]) continue;
            if (i > 0 && nums[i] == nums[i - 1] && !used[i - 1]) continue;
            used[i] = true;
            path.add(nums[i]);
            backtrack(nums, used, path, ans);
            path.remove(path.size() - 1);
            used[i] = false;
        }
    }
}
```

复杂度：时间 `O(n! * n)`，空间 `O(n)`，不计答案。

#### 基础语法与算法思想

- `!used[i - 1]` 表示前一个相同数字没有在当前路径中使用。
- 排序是去重的前置条件。
- 核心思想：重复元素的排列要限制同层选择顺序。

---

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

### Java 解法补充

#### 基础解法：辅助矩阵

算法思想：旋转后，原位置 `(r, c)` 会去到 `(c, n - 1 - r)`。先写入辅助矩阵，再复制回原矩阵。

```java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;
        int[][] copy = new int[n][n];
        for (int r = 0; r < n; r++) {
            for (int c = 0; c < n; c++) {
                copy[c][n - 1 - r] = matrix[r][c];
            }
        }
        for (int r = 0; r < n; r++) {
            for (int c = 0; c < n; c++) {
                matrix[r][c] = copy[r][c];
            }
        }
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(n^2)`。

#### 资深解法：转置加翻转每行

算法思想：顺时针旋转 90 度等价于先沿主对角线转置，再反转每一行。

```java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;
        for (int r = 0; r < n; r++) {
            for (int c = r + 1; c < n; c++) {
                int temp = matrix[r][c];
                matrix[r][c] = matrix[c][r];
                matrix[c][r] = temp;
            }
        }
        for (int r = 0; r < n; r++) {
            int left = 0;
            int right = n - 1;
            while (left < right) {
                int temp = matrix[r][left];
                matrix[r][left++] = matrix[r][right];
                matrix[r][right--] = temp;
            }
        }
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(1)`。

#### 基础语法与算法思想

- 二维数组访问格式是 `matrix[row][col]`。
- 转置只遍历对角线上方，避免交换两次。
- 核心思想：矩阵旋转可拆成简单的对称变换。

---

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

### Java 解法补充

#### 基础解法：排序字符串作键

算法思想：互为字母异位词的字符串排序后完全相同。把排序后的字符串作为哈希表键，收集同组单词。

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> groups = new HashMap<>();
        for (String s : strs) {
            char[] chars = s.toCharArray();
            Arrays.sort(chars);
            String key = new String(chars);
            groups.computeIfAbsent(key, k -> new ArrayList<>()).add(s);
        }
        return new ArrayList<>(groups.values());
    }
}
```

复杂度：时间 `O(n * k log k)`，空间 `O(nk)`。

#### 资深解法：字符计数作键

算法思想：小写字母只有 26 个，用每个字符的出现次数构造键，避免对每个字符串排序。

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> groups = new HashMap<>();
        for (String s : strs) {
            int[] count = new int[26];
            for (int i = 0; i < s.length(); i++) {
                count[s.charAt(i) - 'a']++;
            }
            StringBuilder key = new StringBuilder();
            for (int c : count) key.append('#').append(c);
            groups.computeIfAbsent(key.toString(), k -> new ArrayList<>()).add(s);
        }
        return new ArrayList<>(groups.values());
    }
}
```

复杂度：时间 `O(nk)`，空间 `O(nk)`。

#### 基础语法与算法思想

- `toCharArray()` 把字符串转成字符数组。
- `computeIfAbsent`：键不存在时创建默认值并返回。
- 核心思想：分组题关键是设计能唯一表示同类元素的键。

---

## 50. Pow(x, n) (Medium)

实现 pow(x, n) ，即计算  `x`  的整数  `n`  次幂函数（即， `xn`  ）。
 
示例 1：

```text
输入：x = 2.00000, n = 10
输出：1024.00000
```

示例 2：

```text
输入：x = 2.10000, n = 3
输出：9.26100
```

示例 3：

```text
输入：x = 2.00000, n = -2
输出：0.25000
解释：2-2 = 1/22 = 1/4 = 0.25
```

 
提示：

 `-100.0 < x < 100.0` 
 `-231 <= n <= 231-1` 
 `n`  是一个整数
要么  `x`  不为零，要么  `n > 0`  。
 `-104 <= xn <= 104`

### Java 解法补充

#### 基础解法：循环相乘

算法思想：把指数转成 `long` 防止 `Integer.MIN_VALUE` 取反溢出，循环乘 `|n|` 次；若原指数为负则返回倒数。

```java
class Solution {
    public double myPow(double x, int n) {
        long exp = n;
        if (exp < 0) exp = -exp;
        double ans = 1.0;
        for (long i = 0; i < exp; i++) {
            ans *= x;
        }
        return n < 0 ? 1.0 / ans : ans;
    }
}
```

复杂度：时间 `O(|n|)`，空间 `O(1)`。

#### 资深解法：快速幂

算法思想：指数二进制拆分。若当前最低位是 1，就把当前底数乘入答案；每轮底数平方，指数右移。

```java
class Solution {
    public double myPow(double x, int n) {
        long exp = n;
        if (exp < 0) {
            x = 1.0 / x;
            exp = -exp;
        }

        double ans = 1.0;
        while (exp > 0) {
            if ((exp & 1) == 1) ans *= x;
            x *= x;
            exp >>= 1;
        }
        return ans;
    }
}
```

复杂度：时间 `O(log |n|)`，空间 `O(1)`。

#### 基础语法与算法思想

- `long exp = n`：避免 `-Integer.MIN_VALUE` 仍溢出。
- `& 1` 判断二进制最低位是否为 1。
- 核心思想：快速幂用平方把线性乘法压缩为对数轮。

---

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

### Java 解法补充

#### 基础解法：回溯并扫描冲突

算法思想：每一行放一个皇后，尝试每一列；判断当前位置是否与之前行的皇后同列或同斜线冲突。

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

class Solution {
    public List<List<String>> solveNQueens(int n) {
        List<List<String>> ans = new ArrayList<>();
        char[][] board = new char[n][n];
        for (char[] row : board) Arrays.fill(row, '.');
        backtrack(board, 0, ans);
        return ans;
    }

    private void backtrack(char[][] board, int row, List<List<String>> ans) {
        if (row == board.length) {
            List<String> one = new ArrayList<>();
            for (char[] r : board) one.add(new String(r));
            ans.add(one);
            return;
        }
        for (int col = 0; col < board.length; col++) {
            if (!valid(board, row, col)) continue;
            board[row][col] = 'Q';
            backtrack(board, row + 1, ans);
            board[row][col] = '.';
        }
    }

    private boolean valid(char[][] board, int row, int col) {
        for (int r = 0; r < row; r++) if (board[r][col] == 'Q') return false;
        for (int r = row - 1, c = col - 1; r >= 0 && c >= 0; r--, c--) if (board[r][c] == 'Q') return false;
        for (int r = row - 1, c = col + 1; r >= 0 && c < board.length; r--, c++) if (board[r][c] == 'Q') return false;
        return true;
    }
}
```

复杂度：时间约 `O(n!)`，空间 `O(n^2)`。

#### 资深解法：列和对角线布尔标记

算法思想：用 `cols`、`diag1(row-col+n-1)`、`diag2(row+col)` 三组布尔数组 `O(1)` 判断冲突。

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

class Solution {
    public List<List<String>> solveNQueens(int n) {
        List<List<String>> ans = new ArrayList<>();
        char[][] board = new char[n][n];
        for (char[] row : board) Arrays.fill(row, '.');
        backtrack(0, board, new boolean[n], new boolean[2 * n - 1], new boolean[2 * n - 1], ans);
        return ans;
    }

    private void backtrack(int row, char[][] board, boolean[] cols, boolean[] d1, boolean[] d2, List<List<String>> ans) {
        int n = board.length;
        if (row == n) {
            List<String> one = new ArrayList<>();
            for (char[] r : board) one.add(new String(r));
            ans.add(one);
            return;
        }
        for (int col = 0; col < n; col++) {
            int a = row - col + n - 1;
            int b = row + col;
            if (cols[col] || d1[a] || d2[b]) continue;
            cols[col] = d1[a] = d2[b] = true;
            board[row][col] = 'Q';
            backtrack(row + 1, board, cols, d1, d2, ans);
            board[row][col] = '.';
            cols[col] = d1[a] = d2[b] = false;
        }
    }
}
```

复杂度：时间约 `O(n!)`，空间 `O(n^2)`。

#### 基础语法与算法思想

- `Arrays.fill(row, '.')` 初始化棋盘行。
- 两条对角线可用 `row - col` 与 `row + col` 唯一表示。
- 核心思想：每行一个皇后，把二维放置问题降成逐行选择列。

---

## 52. N 皇后 II (Hard)

n 皇后问题 研究的是如何将  `n`  个皇后放置在  `n × n`  的棋盘上，并且使皇后彼此之间不能相互攻击。
给你一个整数  `n`  ，返回 n 皇后问题 不同的解决方案的数量。
 

示例 1：

```text
输入：n = 4
输出：2
解释：如上图所示，4 皇后问题存在两个不同的解法。
```

示例 2：

```text
输入：n = 1
输出：1
```

 
提示：

 `1 <= n <= 9`

### Java 解法补充

#### 基础解法：复用回溯计数

算法思想：和 N 皇后 I 一样逐行放置，只是不保存棋盘，找到一种合法方案就让计数加一。

```java
class Solution {
    private int count;

    public int totalNQueens(int n) {
        count = 0;
        backtrack(0, n, new boolean[n], new boolean[2 * n - 1], new boolean[2 * n - 1]);
        return count;
    }

    private void backtrack(int row, int n, boolean[] cols, boolean[] d1, boolean[] d2) {
        if (row == n) {
            count++;
            return;
        }
        for (int col = 0; col < n; col++) {
            int a = row - col + n - 1;
            int b = row + col;
            if (cols[col] || d1[a] || d2[b]) continue;
            cols[col] = d1[a] = d2[b] = true;
            backtrack(row + 1, n, cols, d1, d2);
            cols[col] = d1[a] = d2[b] = false;
        }
    }
}
```

复杂度：时间约 `O(n!)`，空间 `O(n)`。

#### 资深解法：位运算回溯

算法思想：用二进制位表示可用列。`cols | diagonals` 标记被攻击位置，每层取最低可用位放皇后，递归时更新左右斜线攻击位。

```java
class Solution {
    public int totalNQueens(int n) {
        return dfs(n, 0, 0, 0, 0);
    }

    private int dfs(int n, int row, int cols, int d1, int d2) {
        if (row == n) return 1;
        int available = ((1 << n) - 1) & ~(cols | d1 | d2);
        int count = 0;
        while (available != 0) {
            int bit = available & -available;
            available -= bit;
            count += dfs(n, row + 1, cols | bit, (d1 | bit) << 1, (d2 | bit) >> 1);
        }
        return count;
    }
}
```

复杂度：时间约 `O(n!)`，空间 `O(n)`。

#### 基础语法与算法思想

- `available & -available` 取最低位的 1。
- `1 << n` 表示二进制第 `n` 位。
- 核心思想：位运算把列集合压缩到整数中，提高回溯常数效率。

---

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

### Java 解法补充

#### 基础解法：枚举子数组

算法思想：枚举每个起点，向右累加形成所有连续子数组和，记录最大值。

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int ans = nums[0];
        for (int i = 0; i < nums.length; i++) {
            int sum = 0;
            for (int j = i; j < nums.length; j++) {
                sum += nums[j];
                ans = Math.max(ans, sum);
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(1)`。

#### 资深解法：Kadane 动态规划

算法思想：`cur` 表示必须以当前位置结尾的最大子数组和，要么接上前一段，要么从当前元素重新开始。

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int cur = nums[0];
        int ans = nums[0];
        for (int i = 1; i < nums.length; i++) {
            cur = Math.max(nums[i], cur + nums[i]);
            ans = Math.max(ans, cur);
        }
        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 初始化最大值要用数组元素，避免全负数时出错。
- `cur` 是局部最优，`ans` 是全局最优。
- 核心思想：连续子数组的状态常定义为“以 i 结尾”。

---

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

### Java 解法补充

#### 基础解法：方向数组加 visited

算法思想：按右、下、左、上的方向前进，下一格越界或已访问就转向，直到访问完所有格子。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        int m = matrix.length;
        int n = matrix[0].length;
        boolean[][] seen = new boolean[m][n];
        int[] dr = {0, 1, 0, -1};
        int[] dc = {1, 0, -1, 0};
        int r = 0, c = 0, dir = 0;
        List<Integer> ans = new ArrayList<>();
        for (int step = 0; step < m * n; step++) {
            ans.add(matrix[r][c]);
            seen[r][c] = true;
            int nr = r + dr[dir];
            int nc = c + dc[dir];
            if (nr < 0 || nr == m || nc < 0 || nc == n || seen[nr][nc]) {
                dir = (dir + 1) % 4;
                nr = r + dr[dir];
                nc = c + dc[dir];
            }
            r = nr;
            c = nc;
        }
        return ans;
    }
}
```

复杂度：时间 `O(mn)`，空间 `O(mn)`。

#### 资深解法：四边界收缩

算法思想：维护上、下、左、右四条边界，每轮依次遍历上边、右边、下边、左边，然后收缩边界。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> ans = new ArrayList<>();
        int top = 0, bottom = matrix.length - 1;
        int left = 0, right = matrix[0].length - 1;

        while (top <= bottom && left <= right) {
            for (int c = left; c <= right; c++) ans.add(matrix[top][c]);
            top++;
            for (int r = top; r <= bottom; r++) ans.add(matrix[r][right]);
            right--;
            if (top <= bottom) {
                for (int c = right; c >= left; c--) ans.add(matrix[bottom][c]);
                bottom--;
            }
            if (left <= right) {
                for (int r = bottom; r >= top; r--) ans.add(matrix[r][left]);
                left++;
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(mn)`，空间 `O(1)`，不计答案。

#### 基础语法与算法思想

- 方向数组能减少重复代码。
- 边界收缩时要检查是否还有行或列未遍历。
- 核心思想：螺旋遍历就是不断剥掉矩阵外层。

---

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

### Java 解法补充

#### 基础解法：动态规划可达性

算法思想：`dp[i]` 表示下标 `i` 是否可达。若某个可达位置 `j` 能跳到 `i`，则 `i` 可达。

```java
class Solution {
    public boolean canJump(int[] nums) {
        boolean[] dp = new boolean[nums.length];
        dp[0] = true;
        for (int i = 1; i < nums.length; i++) {
            for (int j = 0; j < i; j++) {
                if (dp[j] && j + nums[j] >= i) {
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp[nums.length - 1];
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(n)`。

#### 资深解法：维护最远可达位置

算法思想：从左到右扫描，如果当前位置超过最远可达位置就失败；否则用 `i + nums[i]` 更新最远可达范围。

```java
class Solution {
    public boolean canJump(int[] nums) {
        int farthest = 0;
        for (int i = 0; i < nums.length; i++) {
            if (i > farthest) return false;
            farthest = Math.max(farthest, i + nums[i]);
            if (farthest >= nums.length - 1) return true;
        }
        return true;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- `break` 用于找到一个可达来源后提前结束内层循环。
- `farthest` 是当前所有可达位置能扩展出的最远边界。
- 核心思想：只要当前位置在可达范围内，就能继续扩张范围。

---

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

### Java 解法补充

#### 基础解法：排序后逐个合并

算法思想：先按左端点排序，维护当前合并区间。若下一个区间与当前区间重叠，就更新右端点；否则保存当前区间并开启新区间。

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

class Solution {
    public int[][] merge(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
        List<int[]> ans = new ArrayList<>();
        int start = intervals[0][0];
        int end = intervals[0][1];

        for (int i = 1; i < intervals.length; i++) {
            if (intervals[i][0] <= end) {
                end = Math.max(end, intervals[i][1]);
            } else {
                ans.add(new int[]{start, end});
                start = intervals[i][0];
                end = intervals[i][1];
            }
        }
        ans.add(new int[]{start, end});
        return ans.toArray(new int[ans.size()][]);
    }
}
```

复杂度：时间 `O(n log n)`，空间 `O(n)`。

#### 资深解法：直接合并进结果尾部

算法思想：排序后，结果列表最后一个区间就是唯一可能与新区间重叠的区间；若不重叠则追加，若重叠则更新尾区间右端点。

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

class Solution {
    public int[][] merge(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
        List<int[]> merged = new ArrayList<>();
        for (int[] interval : intervals) {
            if (merged.isEmpty() || merged.get(merged.size() - 1)[1] < interval[0]) {
                merged.add(interval);
            } else {
                int[] last = merged.get(merged.size() - 1);
                last[1] = Math.max(last[1], interval[1]);
            }
        }
        return merged.toArray(new int[merged.size()][]);
    }
}
```

复杂度：时间 `O(n log n)`，空间 `O(n)`。

#### 基础语法与算法思想

- `Arrays.sort(intervals, comparator)` 对二维数组排序。
- `toArray(new int[size][])` 把列表转成二维数组。
- 核心思想：区间合并先排序，重叠关系就只需和当前尾区间比较。

---

## 57. 插入区间 (Medium)

给你一个 无重叠的 ，按照区间起始端点排序的区间列表  `intervals` ，其中  `intervals[i] = [starti, endi]`  表示第  `i`  个区间的开始和结束，并且  `intervals`  按照  `starti`  升序排列。同样给定一个区间  `newInterval = [start, end]`  表示另一个区间的开始和结束。
在  `intervals`  中插入区间  `newInterval` ，使得  `intervals`  依然按照  `starti`  升序排列，且区间之间不重叠（如果有必要的话，可以合并区间）。
返回插入之后的  `intervals` 。
注意 你不需要原地修改  `intervals` 。你可以创建一个新数组然后返回它。
 
示例 1：

```text
输入：intervals = [[1,3],[6,9]], newInterval = [2,5]
输出：[[1,5],[6,9]]
```

示例 2：

```text
输入：intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
输出：[[1,2],[3,10],[12,16]]
解释：这是因为新的区间 [4,8] 与 [3,5],[6,7],[8,10] 重叠。
```

 
提示：

 `0 <= intervals.length <= 104` 
 `intervals[i].length == 2` 
 `0 <= starti <= endi <= 105` 
 `intervals`  根据  `starti`  按 升序 排列
 `newInterval.length == 2` 
 `0 <= start <= end <= 105`

### Java 解法补充

#### 基础解法：加入新区间后统一合并

算法思想：把新区间加入列表，再按第 56 题的方式排序合并。

```java
import java.util.Arrays;

class Solution {
    public int[][] insert(int[][] intervals, int[] newInterval) {
        int[][] all = new int[intervals.length + 1][2];
        for (int i = 0; i < intervals.length; i++) all[i] = intervals[i];
        all[intervals.length] = newInterval;
        Arrays.sort(all, (a, b) -> a[0] - b[0]);

        java.util.List<int[]> ans = new java.util.ArrayList<>();
        for (int[] cur : all) {
            if (ans.isEmpty() || ans.get(ans.size() - 1)[1] < cur[0]) ans.add(cur);
            else ans.get(ans.size() - 1)[1] = Math.max(ans.get(ans.size() - 1)[1], cur[1]);
        }
        return ans.toArray(new int[ans.size()][]);
    }
}
```

复杂度：时间 `O(n log n)`，空间 `O(n)`。

#### 资深解法：利用原本有序无重叠

算法思想：先加入所有在新区间左侧且不重叠的区间；再合并所有与新区间重叠的区间；最后加入右侧剩余区间。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public int[][] insert(int[][] intervals, int[] newInterval) {
        List<int[]> ans = new ArrayList<>();
        int i = 0;
        while (i < intervals.length && intervals[i][1] < newInterval[0]) {
            ans.add(intervals[i++]);
        }
        while (i < intervals.length && intervals[i][0] <= newInterval[1]) {
            newInterval[0] = Math.min(newInterval[0], intervals[i][0]);
            newInterval[1] = Math.max(newInterval[1], intervals[i][1]);
            i++;
        }
        ans.add(newInterval);
        while (i < intervals.length) {
            ans.add(intervals[i++]);
        }
        return ans.toArray(new int[ans.size()][]);
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 基础语法与算法思想

- 有序无重叠条件可以省掉排序。
- 区间关系分三类：在左侧、重叠、在右侧。
- 核心思想：利用输入已有结构，避免重复做通用合并。

---

## 58. 最后一个单词的长度 (Easy)

给你一个字符串  `s` ，由若干单词组成，单词前后用一些空格字符隔开。返回字符串中 最后一个 单词的长度。
单词 是指仅由字母组成、不包含任何空格字符的最大子字符串。
 
示例 1：

```text
输入：s = "Hello World"
输出：5
解释：最后一个单词是“World”，长度为 5。
```

示例 2：

```text
输入：s = "   fly me   to   the moon  "
输出：4
解释：最后一个单词是“moon”，长度为 4。
```

示例 3：

```text
输入：s = "luffy is still joyboy"
输出：6
解释：最后一个单词是长度为 6 的“joyboy”。
```

 
提示：

 `1 <= s.length <= 104` 
 `s`  仅有英文字母和空格  `' '`  组成
 `s`  中至少存在一个单词

### Java 解法补充

#### 基础解法：分割字符串

算法思想：去掉首尾空格后按一个或多个空格切分，最后一个片段就是最后一个单词。

```java
class Solution {
    public int lengthOfLastWord(String s) {
        String[] parts = s.trim().split("\\s+");
        return parts[parts.length - 1].length();
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 资深解法：从右向左扫描

算法思想：先跳过尾部空格，再统计连续非空格字符数量。

```java
class Solution {
    public int lengthOfLastWord(String s) {
        int i = s.length() - 1;
        while (i >= 0 && s.charAt(i) == ' ') i--;
        int len = 0;
        while (i >= 0 && s.charAt(i) != ' ') {
            len++;
            i--;
        }
        return len;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- `trim()` 去掉字符串首尾空格。
- `split("\\s+")` 按连续空白字符分割。
- 核心思想：只关心最后一个单词时，从右往左扫描更直接。

---

## 59. 螺旋矩阵 II (Medium)

给你一个正整数  `n`  ，生成一个包含  `1`  到  `n2`  所有元素，且元素按顺时针顺序螺旋排列的  `n x n`  正方形矩阵  `matrix`  。
 
示例 1：

```text
输入：n = 3
输出：[[1,2,3],[8,9,4],[7,6,5]]
```

示例 2：

```text
输入：n = 1
输出：[[1]]
```

 
提示：

 `1 <= n <= 20`

### Java 解法补充

#### 基础解法：方向数组模拟

算法思想：从左上角开始按右、下、左、上走，越界或遇到已填数字就转向，依次填入 `1..n^2`。

```java
class Solution {
    public int[][] generateMatrix(int n) {
        int[][] ans = new int[n][n];
        int[] dr = {0, 1, 0, -1};
        int[] dc = {1, 0, -1, 0};
        int r = 0, c = 0, dir = 0;
        for (int value = 1; value <= n * n; value++) {
            ans[r][c] = value;
            int nr = r + dr[dir];
            int nc = c + dc[dir];
            if (nr < 0 || nr == n || nc < 0 || nc == n || ans[nr][nc] != 0) {
                dir = (dir + 1) % 4;
                nr = r + dr[dir];
                nc = c + dc[dir];
            }
            r = nr;
            c = nc;
        }
        return ans;
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(1)`，不计返回矩阵。

#### 资深解法：四边界填充

算法思想：维护四条边界，每轮填满上、右、下、左四条边，之后向内收缩。

```java
class Solution {
    public int[][] generateMatrix(int n) {
        int[][] ans = new int[n][n];
        int top = 0, bottom = n - 1, left = 0, right = n - 1;
        int value = 1;
        while (top <= bottom && left <= right) {
            for (int c = left; c <= right; c++) ans[top][c] = value++;
            top++;
            for (int r = top; r <= bottom; r++) ans[r][right] = value++;
            right--;
            for (int c = right; c >= left && top <= bottom; c--) ans[bottom][c] = value++;
            bottom--;
            for (int r = bottom; r >= top && left <= right; r--) ans[r][left] = value++;
            left++;
        }
        return ans;
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(1)`，不计返回矩阵。

#### 基础语法与算法思想

- 未填过的 `int` 数组格子默认是 `0`。
- `value++` 先使用当前值，再自增。
- 核心思想：螺旋生成和螺旋遍历是同一个边界收缩模型。

---

## 60. 排列序列 (Hard)

给出集合  `[1,2,3,...,n]` ，其所有元素共有  `n!`  种排列。
按大小顺序列出所有排列情况，并一一标记，当  `n = 3`  时, 所有排列如下：

 `"123"` 
 `"132"` 
 `"213"` 
 `"231"` 
 `"312"` 
 `"321"` 

给定  `n`  和  `k` ，返回第  `k`  个排列。
 
示例 1：

```text
输入：n = 3, k = 3
输出："213"
```

示例 2：

```text
输入：n = 4, k = 9
输出："2314"
```

示例 3：

```text
输入：n = 3, k = 1
输出："123"
```

 
提示：

 `1 <= n <= 9` 
 `1 <= k <= n!`

### Java 解法补充

#### 基础解法：回溯生成到第 k 个

算法思想：按字典序回溯生成排列，计数到第 `k` 个时记录答案。由于 `n <= 9`，能理解但最坏仍会生成大量排列。

```java
class Solution {
    private int count;
    private String ans;

    public String getPermutation(int n, int k) {
        count = 0;
        ans = "";
        backtrack(n, k, new boolean[n + 1], new StringBuilder());
        return ans;
    }

    private void backtrack(int n, int k, boolean[] used, StringBuilder path) {
        if (!ans.isEmpty()) return;
        if (path.length() == n) {
            count++;
            if (count == k) ans = path.toString();
            return;
        }
        for (int num = 1; num <= n; num++) {
            if (used[num]) continue;
            used[num] = true;
            path.append(num);
            backtrack(n, k, used, path);
            path.deleteCharAt(path.length() - 1);
            used[num] = false;
        }
    }
}
```

复杂度：时间 `O(k * n)` 到 `O(n! * n)`，空间 `O(n)`。

#### 资深解法：阶乘定位

算法思想：固定首位后，剩余 `(n-1)!` 个排列为一组。把 `k` 改成 0 基下标，每次用 `k / factorial` 定位要取的数字，再用 `k % factorial` 进入下一位。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public String getPermutation(int n, int k) {
        int[] fact = new int[n + 1];
        fact[0] = 1;
        List<Integer> nums = new ArrayList<>();
        for (int i = 1; i <= n; i++) {
            fact[i] = fact[i - 1] * i;
            nums.add(i);
        }

        k--;
        StringBuilder ans = new StringBuilder();
        for (int pos = n; pos >= 1; pos--) {
            int block = fact[pos - 1];
            int index = k / block;
            ans.append(nums.remove(index));
            k %= block;
        }
        return ans.toString();
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(n)`。

#### 基础语法与算法思想

- `List.remove(index)` 删除并返回指定下标元素。
- 把 `k--` 转为 0 基更方便做整除定位。
- 核心思想：字典序排列可以按阶乘分块直接跳过整组。

---
