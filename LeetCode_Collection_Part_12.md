# LeetCode 题目合集 Part 12

## 331. 验证二叉树的前序序列化 (Medium)

序列化二叉树的一种方法是使用  **前序遍历** 。当我们遇到一个非空节点时，我们可以记录下这个节点的值。如果它是一个空节点，我们可以使用一个标记值记录，例如  `#` 。

例如，上面的二叉树可以被序列化为字符串  `"9,3,4,#,#,1,#,#,2,#,6,#,#"` ，其中  `#`  代表一个空节点。
给定一串以逗号分隔的序列，验证它是否是正确的二叉树的前序序列化。编写一个在不重构树的条件下的可行算法。
 **保证**  每个以逗号分隔的字符或为一个整数或为一个表示  `null`  指针的  `'#'`  。
你可以认为输入格式总是有效的

例如它永远不会包含两个连续的逗号，比如  `"1,,3"`  。

 **注意：** 不允许重建树。
 
 **示例 1:** 

```text
输入: preorder = "9,3,4,#,#,1,#,#,2,#,6,#,#"
输出: true
```

 **示例 2:** 

```text
输入: preorder = "1,#"
输出: false
```

 **示例 3:** 

```text
输入: preorder = "9,#,#,1"
输出: false
```

 
 **提示:** 

 `1 <= preorder.length <= 104` 
 `preorder`  由以逗号  `“，”`  分隔的  `[0,100]`  范围内的整数和  `“#”`  组成

### Java 解法补充

#### 基础解法：栈反复归约叶子结构

算法思想：合法序列中子树最终会出现 `数字,#,#`，可把它归约成 `#`，最后只剩一个 `#`。

```java
class Solution {
    public boolean isValidSerialization(String preorder) {
        Deque<String> stack = new ArrayDeque<>();
        for (String token : preorder.split(",")) {
            stack.push(token);
            while (stack.size() >= 3) {
                String a = stack.pop(), b = stack.pop(), c = stack.pop();
                if (a.equals("#") && b.equals("#") && !c.equals("#")) stack.push("#");
                else {
                    stack.push(c);
                    stack.push(b);
                    stack.push(a);
                    break;
                }
            }
        }
        return stack.size() == 1 && stack.peek().equals("#");
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 资深解法：入度出度槽位计数

算法思想：每个节点消耗一个槽位，非空节点额外产生两个槽位；过程中槽位不能为负，结束必须为 0。

```java
class Solution {
    public boolean isValidSerialization(String preorder) {
        int slots = 1;
        for (String token : preorder.split(",")) {
            if (slots == 0) return false;
            if (token.equals("#")) slots--;
            else slots++;
        }
        return slots == 0;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 空节点只消耗槽位；非空节点消耗 1 个槽位并产生 2 个子槽位。

---

## 332. 重新安排行程 (Hard)

给你一份航线列表  `tickets`  ，其中  `tickets[i] = [fromi, toi]`  表示飞机出发和降落的机场地点。请你对该行程进行重新规划排序。
所有这些机票都属于一个从  `JFK` （肯尼迪国际机场）出发的先生，所以该行程必须从  `JFK`  开始。如果存在多种有效的行程，请你按字典排序返回最小的行程组合。

例如，行程  `["JFK", "LGA"]`  与  `["JFK", "LGB"]`  相比就更小，排序更靠前。

假定所有机票至少存在一种合理的行程。且所有的机票 必须都用一次 且 只能用一次。
 
 **示例 1：** 

```text
输入：tickets = [["MUC","LHR"],["JFK","MUC"],["SFO","SJC"],["LHR","SFO"]]
输出：["JFK","MUC","LHR","SFO","SJC"]
```

 **示例 2：** 

```text
输入：tickets = [["JFK","SFO"],["JFK","ATL"],["SFO","ATL"],["ATL","JFK"],["ATL","SFO"]]
输出：["JFK","ATL","JFK","SFO","ATL","SFO"]
解释：另一种有效的行程是 ["JFK","SFO","ATL","JFK","ATL","SFO"] ，但是它字典排序更大更靠后。
```

 
 **提示：** 

 `1 <= tickets.length <= 300` 
 `tickets[i].length == 2` 
 `fromi.length == 3` 
 `toi.length == 3` 
 `fromi`  和  `toi`  由大写英文字母组成
 `fromi != toi`

### Java 解法补充

#### 基础解法：排序后回溯尝试每张票

算法思想：先按目的地字典序排序，再从 `JFK` 出发尝试未使用机票，第一次用完所有票就是答案。

```java
class Solution {
    public List<String> findItinerary(List<List<String>> tickets) {
        tickets.sort((a, b) -> a.get(1).equals(b.get(1)) ? a.get(0).compareTo(b.get(0)) : a.get(1).compareTo(b.get(1)));
        boolean[] used = new boolean[tickets.size()];
        List<String> path = new ArrayList<>();
        path.add("JFK");
        dfs(tickets, used, "JFK", path);
        return path;
    }

    private boolean dfs(List<List<String>> tickets, boolean[] used, String from, List<String> path) {
        if (path.size() == tickets.size() + 1) return true;
        for (int i = 0; i < tickets.size(); i++) {
            if (used[i] || !tickets.get(i).get(0).equals(from)) continue;
            used[i] = true;
            path.add(tickets.get(i).get(1));
            if (dfs(tickets, used, tickets.get(i).get(1), path)) return true;
            path.remove(path.size() - 1);
            used[i] = false;
        }
        return false;
    }
}
```

复杂度：最坏阶乘级，空间 `O(n)`。

#### 资深解法：Hierholzer 欧拉路径

算法思想：每个出发点用小根堆保存目的地，后序加入路径，得到字典序最小欧拉路径。

```java
class Solution {
    public List<String> findItinerary(List<List<String>> tickets) {
        Map<String, PriorityQueue<String>> graph = new HashMap<>();
        for (List<String> t : tickets) graph.computeIfAbsent(t.get(0), k -> new PriorityQueue<>()).offer(t.get(1));
        LinkedList<String> ans = new LinkedList<>();
        dfs("JFK", graph, ans);
        return ans;
    }

    private void dfs(String from, Map<String, PriorityQueue<String>> graph, LinkedList<String> ans) {
        PriorityQueue<String> heap = graph.get(from);
        while (heap != null && !heap.isEmpty()) dfs(heap.poll(), graph, ans);
        ans.addFirst(from);
    }
}
```

复杂度：时间 `O(n log n)`，空间 `O(n)`。

#### 基础语法与算法思想

- 所有边必须恰好使用一次，是典型欧拉路径问题。

---

## 333. 最大二叉搜索子树 (Medium)

暂无内容描述。

### Java 解法补充

#### 基础解法：枚举每个子树并单独判断是否为 BST

算法思想：对每个节点，先判断以它为根是否是 BST，是则统计节点数，否则继续看左右子树。

```java
class Solution {
    public int largestBSTSubtree(TreeNode root) {
        if (root == null) return 0;
        if (isBST(root, Long.MIN_VALUE, Long.MAX_VALUE)) return count(root);
        return Math.max(largestBSTSubtree(root.left), largestBSTSubtree(root.right));
    }

    private boolean isBST(TreeNode node, long low, long high) {
        if (node == null) return true;
        return node.val > low && node.val < high
                && isBST(node.left, low, node.val)
                && isBST(node.right, node.val, high);
    }

    private int count(TreeNode node) {
        return node == null ? 0 : 1 + count(node.left) + count(node.right);
    }
}
```

复杂度：时间最坏 `O(n^2)`，空间 `O(h)`。

#### 资深解法：后序返回子树信息

算法思想：每个节点返回当前子树是否 BST、最小值、最大值、大小，一次后序完成统计。

```java
class Solution {
    private int ans = 0;

    public int largestBSTSubtree(TreeNode root) {
        dfs(root);
        return ans;
    }

    private int[] dfs(TreeNode node) {
        if (node == null) return new int[]{1, Integer.MAX_VALUE, Integer.MIN_VALUE, 0};
        int[] left = dfs(node.left), right = dfs(node.right);
        if (left[0] == 1 && right[0] == 1 && node.val > left[2] && node.val < right[1]) {
            int size = left[3] + right[3] + 1;
            ans = Math.max(ans, size);
            return new int[]{1, Math.min(left[1], node.val), Math.max(right[2], node.val), size};
        }
        return new int[]{0, 0, 0, 0};
    }
}
```

复杂度：时间 `O(n)`，空间 `O(h)`。

#### 基础语法与算法思想

- BST 子树判断需要同时知道左右子树边界。

---

## 334. 递增的三元子序列 (Medium)

给你一个整数数组  `nums`  ，判断这个数组中是否存在长度为  `3`  的递增子序列。
如果存在这样的三元组下标  `(i, j, k)`  且满足  `i < j < k`  ，使得  `nums[i] < nums[j] < nums[k]`  ，返回  `true`  ；否则，返回  `false`  。
 
 **示例 1：** 

```text
输入：nums = [1,2,3,4,5]
输出：true
解释：任何 i < j < k 的三元组都满足题意
```

 **示例 2：** 

```text
输入：nums = [5,4,3,2,1]
输出：false
解释：不存在满足题意的三元组
```

 **示例 3：** 

```text
输入：nums = [2,1,5,0,4,6]
输出：true
解释：其中一个满足题意的三元组是 (1, 4, 5)，因为 nums[1] == 1 < nums[4] == 4 < nums[5] == 6
```

 
 **提示：** 

 `1 <= nums.length <= 5 * 105` 
 `-231 <= nums[i] <= 231 - 1` 

 
 **进阶：** 你能实现时间复杂度为  `O(n)`  ，空间复杂度为  `O(1)`  的解决方案吗？

### Java 解法补充

#### 基础解法：三重循环枚举下标

算法思想：直接枚举 `i < j < k`，判断是否满足严格递增。

```java
class Solution {
    public boolean increasingTriplet(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                for (int k = j + 1; k < nums.length; k++) {
                    if (nums[i] < nums[j] && nums[j] < nums[k]) return true;
                }
            }
        }
        return false;
    }
}
```

复杂度：时间 `O(n^3)`，空间 `O(1)`。

#### 资深解法：维护最小值和第二小值

算法思想：扫描时尽量压低第一位和第二位，只要出现比第二位大的数就存在三元组。

```java
class Solution {
    public boolean increasingTriplet(int[] nums) {
        int first = Integer.MAX_VALUE, second = Integer.MAX_VALUE;
        for (int x : nums) {
            if (x <= first) first = x;
            else if (x <= second) second = x;
            else return true;
        }
        return false;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- `first < second < x` 一旦成立即可提前返回。

---

## 335. 路径交叉 (Hard)

给你一个整数数组  `distance`  。
从  **X-Y**  平面上的点  `(0,0)`  开始，先向北移动  `distance[0]`  米，然后向西移动  `distance[1]`  米，向南移动  `distance[2]`  米，向东移动  `distance[3]`  米，持续移动。也就是说，每次移动后你的方位会发生逆时针变化。
判断你所经过的路径是否相交。如果相交，返回  `true`  ；否则，返回  `false`  。
 
 **示例 1：** 

```text
输入：distance = [2,1,1,2]
输出：true
```

 **示例 2：** 

```text
输入：distance = [1,2,3,4]
输出：false
```

 **示例 3：** 

```text
输入：distance = [1,1,1,1]
输出：true
```

 
 **提示：** 

 `1 <= distance.length <= 105` 
 `1 <= distance[i] <= 105`

### Java 解法补充

#### 基础解法：保存所有线段并两两判断相交

算法思想：按移动方向生成线段，每加入一条新线段就和之前非相邻线段判断是否相交。

```java
class Solution {
    public boolean isSelfCrossing(int[] distance) {
        List<int[]> lines = new ArrayList<>();
        int x = 0, y = 0;
        int[][] dirs = {{0,1},{-1,0},{0,-1},{1,0}};
        for (int i = 0; i < distance.length; i++) {
            int nx = x + dirs[i % 4][0] * distance[i];
            int ny = y + dirs[i % 4][1] * distance[i];
            int[] cur = {x, y, nx, ny};
            for (int j = 0; j + 1 < lines.size(); j++) {
                if (cross(cur, lines.get(j))) return true;
            }
            lines.add(cur);
            x = nx;
            y = ny;
        }
        return false;
    }

    private boolean cross(int[] a, int[] b) {
        return Math.max(Math.min(a[0], a[2]), Math.min(b[0], b[2])) <= Math.min(Math.max(a[0], a[2]), Math.max(b[0], b[2]))
                && Math.max(Math.min(a[1], a[3]), Math.min(b[1], b[3])) <= Math.min(Math.max(a[1], a[3]), Math.max(b[1], b[3]));
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(n)`。

#### 资深解法：只检查三类局部交叉

算法思想：螺旋移动若发生交叉，只会和前三到前五条边形成固定模式。

```java
class Solution {
    public boolean isSelfCrossing(int[] d) {
        for (int i = 3; i < d.length; i++) {
            if (d[i] >= d[i - 2] && d[i - 1] <= d[i - 3]) return true;
            if (i >= 4 && d[i - 1] == d[i - 3] && d[i] + d[i - 4] >= d[i - 2]) return true;
            if (i >= 5 && d[i - 2] >= d[i - 4]
                    && d[i] + d[i - 4] >= d[i - 2]
                    && d[i - 1] <= d[i - 3]
                    && d[i - 1] + d[i - 5] >= d[i - 3]) return true;
        }
        return false;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 路径按固定方向旋转，交叉只需要看有限个历史边。

---

## 336. 回文对 (Hard)

给定一个由唯一字符串构成的  **0 索引** 数组  `words`  。
 **回文对**  是一对整数  `(i, j)`  ，满足以下条件：

 `0 <= i, j < words.length` ，
 `i != j`  ，并且
 `words[i] + words[j]` （两个字符串的连接）是一个回文串。

返回一个数组，它包含  `words`  中所有满足  **回文对**  条件的字符串。
你必须设计一个时间复杂度为  `O(sum of words[i].length)`  的算法。
 
 **示例 1：** 

```text
输入：words = ["abcd","dcba","lls","s","sssll"]
输出：[[0,1],[1,0],[3,2],[2,4]] 
解释：可拼接成的回文串为 ["dcbaabcd","abcddcba","slls","llssssll"]
```

 **示例 2：** 

```text
输入：words = ["bat","tab","cat"]
输出：[[0,1],[1,0]] 
解释：可拼接成的回文串为 ["battab","tabbat"]
```

 **示例 3：** 

```text
输入：words = ["a",""]
输出：[[0,1],[1,0]]
```

 

 **提示：** 

 `1 <= words.length <= 5000` 
 `0 <= words[i].length <= 300` 
 `words[i]`  由小写英文字母组成

### Java 解法补充

#### 基础解法：枚举所有单词对

算法思想：两两拼接后判断是否为回文，写法直观但重复比较多。

```java
class Solution {
    public List<List<Integer>> palindromePairs(String[] words) {
        List<List<Integer>> ans = new ArrayList<>();
        for (int i = 0; i < words.length; i++) {
            for (int j = 0; j < words.length; j++) {
                if (i != j && isPal(words[i] + words[j])) ans.add(Arrays.asList(i, j));
            }
        }
        return ans;
    }

    private boolean isPal(String s) {
        int l = 0, r = s.length() - 1;
        while (l < r) if (s.charAt(l++) != s.charAt(r--)) return false;
        return true;
    }
}
```

复杂度：时间 `O(n^2 * L)`，空间 `O(L)`。

#### 资深解法：哈希表枚举切分

算法思想：若左半是回文，则需要右半反串在前；若右半是回文，则需要左半反串在后。

```java
class Solution {
    public List<List<Integer>> palindromePairs(String[] words) {
        Map<String, Integer> index = new HashMap<>();
        for (int i = 0; i < words.length; i++) index.put(words[i], i);
        List<List<Integer>> ans = new ArrayList<>();
        for (int i = 0; i < words.length; i++) {
            String w = words[i];
            for (int cut = 0; cut <= w.length(); cut++) {
                String left = w.substring(0, cut), right = w.substring(cut);
                if (isPal(left)) add(ans, index, rev(right), i, true);
                if (cut != w.length() && isPal(right)) add(ans, index, rev(left), i, false);
            }
        }
        return ans;
    }

    private void add(List<List<Integer>> ans, Map<String, Integer> index, String need, int i, boolean before) {
        Integer j = index.get(need);
        if (j != null && j != i) ans.add(before ? Arrays.asList(j, i) : Arrays.asList(i, j));
    }

    private String rev(String s) {
        return new StringBuilder(s).reverse().toString();
    }

    private boolean isPal(String s) {
        int l = 0, r = s.length() - 1;
        while (l < r) if (s.charAt(l++) != s.charAt(r--)) return false;
        return true;
    }
}
```

复杂度：时间 `O(n * L^2)`，空间 `O(nL)`。

#### 基础语法与算法思想

- 回文拼接题常通过“某半边已经回文，另一半找反串”拆解。

---

## 337. 打家劫舍 III (Medium)

小偷又发现了一个新的可行窃的地区。这个地区只有一个入口，我们称之为  `root`  。
除了  `root`  之外，每栋房子有且只有一个“父“房子与之相连。一番侦察之后，聪明的小偷意识到“这个地方的所有房屋的排列类似于一棵二叉树”。 如果  **两个直接相连的房子在同一天晚上被打劫**  ，房屋将自动报警。
给定二叉树的  `root`  。返回  **在不触动警报的情况下**  ，小偷能够盗取的最高金额 。
 
 **示例 1:** 

```text
输入: root = [3,2,3,null,3,null,1]
输出: 7 
解释: 小偷一晚能够盗取的最高金额 3 + 3 + 1 = 7
```

 **示例 2:** 

```text
输入: root = [3,4,5,1,3,null,1]
输出: 9
解释: 小偷一晚能够盗取的最高金额 4 + 5 = 9
```

 
 **提示：** 

树的节点数在  `[1, 104]`  范围内
 `0 <= Node.val <= 104`

### Java 解法补充

#### 基础解法：递归分抢当前节点或不抢当前节点

算法思想：抢当前节点就不能抢左右孩子；不抢当前节点就分别求左右子树最优。

```java
class Solution {
    public int rob(TreeNode root) {
        if (root == null) return 0;
        int take = root.val;
        if (root.left != null) take += rob(root.left.left) + rob(root.left.right);
        if (root.right != null) take += rob(root.right.left) + rob(root.right.right);
        int skip = rob(root.left) + rob(root.right);
        return Math.max(take, skip);
    }
}
```

复杂度：时间指数级，空间 `O(h)`。

#### 资深解法：树形 DP 返回抢和不抢

算法思想：后序返回数组 `[不抢当前, 抢当前]`，父节点直接使用左右结果。

```java
class Solution {
    public int rob(TreeNode root) {
        int[] res = dfs(root);
        return Math.max(res[0], res[1]);
    }

    private int[] dfs(TreeNode node) {
        if (node == null) return new int[2];
        int[] left = dfs(node.left), right = dfs(node.right);
        int skip = Math.max(left[0], left[1]) + Math.max(right[0], right[1]);
        int take = node.val + left[0] + right[0];
        return new int[]{skip, take};
    }
}
```

复杂度：时间 `O(n)`，空间 `O(h)`。

#### 基础语法与算法思想

- 树形 DP 的状态通常由“选当前/不选当前”组成。

---

## 338. 比特位计数 (Easy)

给你一个整数  `n`  ，对于  `0 <= i <= n`  中的每个  `i`  ，计算其二进制表示中  **`1`  的个数**  ，返回一个长度为  `n + 1`  的数组  `ans`  作为答案。
 

 **示例 1：** 

```text
输入：n = 2
输出：[0,1,1]
解释：
0 --> 0
1 --> 1
2 --> 10
```

 **示例 2：** 

```text
输入：n = 5
输出：[0,1,1,2,1,2]
解释：
0 --> 0
1 --> 1
2 --> 10
3 --> 11
4 --> 100
5 --> 101
```

 
 **提示：** 

 `0 <= n <= 105` 

 
 **进阶：** 

很容易就能实现时间复杂度为  `O(n log n)`  的解决方案，你可以在线性时间复杂度  `O(n)`  内用一趟扫描解决此问题吗？
你能不使用任何内置函数解决此问题吗？（如，C++ 中的  `__builtin_popcount`  ）

### Java 解法补充

#### 基础解法：逐个数字循环数 1

算法思想：对每个 `i`，不断右移并统计最低位是否为 1。

```java
class Solution {
    public int[] countBits(int n) {
        int[] ans = new int[n + 1];
        for (int i = 0; i <= n; i++) {
            int x = i;
            while (x != 0) {
                ans[i] += x & 1;
                x >>>= 1;
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(n log n)`，空间 `O(1)`，不计答案。

#### 资深解法：动态规划

算法思想：`i` 的 1 个数等于 `i >> 1` 的 1 个数加上最低位。

```java
class Solution {
    public int[] countBits(int n) {
        int[] ans = new int[n + 1];
        for (int i = 1; i <= n; i++) ans[i] = ans[i >> 1] + (i & 1);
        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`，不计答案。

#### 基础语法与算法思想

- 位运算 `i & 1` 可判断最低位是否为 1。

---

## 339. 嵌套列表加权和 (Medium)

暂无内容描述。

### Java 解法补充

#### 基础解法：DFS 携带深度

算法思想：整数贡献为 `value * depth`，列表里的元素深度加一。

```java
class Solution {
    public int depthSum(List<NestedInteger> nestedList) {
        return dfs(nestedList, 1);
    }

    private int dfs(List<NestedInteger> list, int depth) {
        int sum = 0;
        for (NestedInteger ni : list) {
            if (ni.isInteger()) sum += ni.getInteger() * depth;
            else sum += dfs(ni.getList(), depth + 1);
        }
        return sum;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(d)`。

#### 资深解法：队列按层 BFS

算法思想：逐层遍历嵌套列表，当前层所有整数乘以当前深度。

```java
class Solution {
    public int depthSum(List<NestedInteger> nestedList) {
        Queue<NestedInteger> q = new ArrayDeque<>(nestedList);
        int depth = 1, ans = 0;
        while (!q.isEmpty()) {
            for (int size = q.size(); size > 0; size--) {
                NestedInteger cur = q.poll();
                if (cur.isInteger()) ans += cur.getInteger() * depth;
                else q.addAll(cur.getList());
            }
            depth++;
        }
        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 基础语法与算法思想

- 嵌套结构递归最直观；层次权重也可用 BFS 表达。

---

## 340. 至多包含 K 个不同字符的最长子串 (Medium)

暂无内容描述。

### Java 解法补充

#### 基础解法：枚举起点并向右扩展

算法思想：从每个起点开始向右统计字符种类，超过 `k` 就停止。

```java
class Solution {
    public int lengthOfLongestSubstringKDistinct(String s, int k) {
        int ans = 0;
        for (int left = 0; left < s.length(); left++) {
            Map<Character, Integer> count = new HashMap<>();
            for (int right = left; right < s.length(); right++) {
                char c = s.charAt(right);
                count.put(c, count.getOrDefault(c, 0) + 1);
                if (count.size() > k) break;
                ans = Math.max(ans, right - left + 1);
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(k)`。

#### 资深解法：滑动窗口

算法思想：右指针扩展，字符种类超过 `k` 时移动左指针直到合法。

```java
class Solution {
    public int lengthOfLongestSubstringKDistinct(String s, int k) {
        Map<Character, Integer> count = new HashMap<>();
        int left = 0, ans = 0;
        for (int right = 0; right < s.length(); right++) {
            char c = s.charAt(right);
            count.put(c, count.getOrDefault(c, 0) + 1);
            while (count.size() > k) {
                char remove = s.charAt(left++);
                count.put(remove, count.get(remove) - 1);
                if (count.get(remove) == 0) count.remove(remove);
            }
            ans = Math.max(ans, right - left + 1);
        }
        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(k)`。

#### 基础语法与算法思想

- “最长子串 + 至多 K 种”是滑动窗口的经典信号。

---

## 341. 扁平化嵌套列表迭代器 (Medium)

给你一个嵌套的整数列表  `nestedList`  。每个元素要么是一个整数，要么是一个列表；该列表的元素也可能是整数或者是其他列表。请你实现一个迭代器将其扁平化，使之能够遍历这个列表中的所有整数。
实现扁平迭代器类  `NestedIterator`  ：

 `NestedIterator(List<NestedInteger> nestedList)`  用嵌套列表  `nestedList`  初始化迭代器。
 `int next()`  返回嵌套列表的下一个整数。
 `boolean hasNext()`  如果仍然存在待迭代的整数，返回  `true`  ；否则，返回  `false`  。

你的代码将会用下述伪代码检测：

```text
initialize iterator with nestedList
res = []
while iterator.hasNext()
    append iterator.next() to the end of res
return res
```

如果  `res`  与预期的扁平化列表匹配，那么你的代码将会被判为正确。
 
 **示例 1：** 

```text
输入：nestedList = [[1,1],2,[1,1]]
输出：[1,1,2,1,1]
解释：通过重复调用 next 直到 hasNext 返回 false，next 返回的元素的顺序应该是: [1,1,2,1,1]。
```

 **示例 2：** 

```text
输入：nestedList = [1,[4,[6]]]
输出：[1,4,6]
解释：通过重复调用 next 直到 hasNext 返回 false，next 返回的元素的顺序应该是: [1,4,6]。
```

 
 **提示：** 

 `1 <= nestedList.length <= 500` 
嵌套列表中的整数值在范围  `[-106, 106]`  内

### Java 解法补充

#### 基础解法：构造时递归展开所有整数

算法思想：初始化时把嵌套结构全部拍平成列表，`next` 只移动下标。

```java
public class NestedIterator implements Iterator<Integer> {
    private List<Integer> data = new ArrayList<>();
    private int index = 0;

    public NestedIterator(List<NestedInteger> nestedList) {
        flatten(nestedList);
    }

    private void flatten(List<NestedInteger> list) {
        for (NestedInteger ni : list) {
            if (ni.isInteger()) data.add(ni.getInteger());
            else flatten(ni.getList());
        }
    }

    public Integer next() {
        return data.get(index++);
    }

    public boolean hasNext() {
        return index < data.size();
    }
}
```

复杂度：初始化 `O(n)`，`next/hasNext O(1)`，空间 `O(n)`。

#### 资深解法：栈惰性展开

算法思想：栈中保存待处理元素，`hasNext` 只展开到栈顶为整数为止。

```java
public class NestedIterator implements Iterator<Integer> {
    private Deque<NestedInteger> stack = new ArrayDeque<>();

    public NestedIterator(List<NestedInteger> nestedList) {
        for (int i = nestedList.size() - 1; i >= 0; i--) stack.push(nestedList.get(i));
    }

    public Integer next() {
        hasNext();
        return stack.pop().getInteger();
    }

    public boolean hasNext() {
        while (!stack.isEmpty() && !stack.peek().isInteger()) {
            List<NestedInteger> list = stack.pop().getList();
            for (int i = list.size() - 1; i >= 0; i--) stack.push(list.get(i));
        }
        return !stack.isEmpty();
    }
}
```

复杂度：总时间 `O(n)`，空间 `O(d)` 到 `O(n)`。

#### 基础语法与算法思想

- 迭代器设计常用“预展开”和“惰性展开”两种思路。

---

## 342. 4的幂 (Easy)

给定一个整数，写一个函数来判断它是否是 4 的幂次方。如果是，返回  `true`  ；否则，返回  `false`  。
整数  `n`  是 4 的幂次方需满足：存在整数  `x`  使得  `n == 4x` 
 
 **示例 1：** 

```text
输入：n = 16
输出：true
```

 **示例 2：** 

```text
输入：n = 5
输出：false
```

 **示例 3：** 

```text
输入：n = 1
输出：true
```

 
 **提示：** 

 `-231 <= n <= 231 - 1` 

 
 **进阶：** 你能不使用循环或者递归来完成本题吗？

### Java 解法补充

#### 基础解法：不断除以 4

算法思想：正数能一直被 4 整除并最终变成 1，就是 4 的幂。

```java
class Solution {
    public boolean isPowerOfFour(int n) {
        if (n <= 0) return false;
        while (n % 4 == 0) n /= 4;
        return n == 1;
    }
}
```

#### 资深解法：位运算判断唯一 1 是否在偶数位

算法思想：4 的幂必须是 2 的幂，且二进制唯一的 1 位于偶数位，可用掩码 `0x55555555` 判断。

```java
class Solution {
    public boolean isPowerOfFour(int n) {
        return n > 0 && (n & (n - 1)) == 0 && (n & 0x55555555) != 0;
    }
}
```

复杂度：均为 `O(1)` 额外空间；基础版时间 `O(log n)`，资深版时间 `O(1)`。

#### 基础语法与算法思想

- `n & (n - 1)` 可以消掉最低位的 1，常用于判断 2 的幂。

---

## 343. 整数拆分 (Medium)

给定一个正整数  `n`  ，将其拆分为  `k`  个  **正整数**  的和（  `k >= 2`  ），并使这些整数的乘积最大化。
返回 你可以获得的最大乘积 。
 
 **示例 1:** 

```text
输入: n = 2
输出: 1
解释: 2 = 1 + 1, 1 × 1 = 1。
```

 **示例 2:** 

```text
输入: n = 10
输出: 36
解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36。
```

 
 **提示:** 

 `2 <= n <= 58`

### Java 解法补充

#### 基础解法：动态规划枚举最后一次拆分

算法思想：`dp[i]` 表示拆分整数 `i` 的最大乘积，枚举第一段长度。

```java
class Solution {
    public int integerBreak(int n) {
        int[] dp = new int[n + 1];
        dp[1] = 1;
        for (int i = 2; i <= n; i++) {
            for (int j = 1; j < i; j++) {
                dp[i] = Math.max(dp[i], Math.max(j * (i - j), j * dp[i - j]));
            }
        }
        return dp[n];
    }
}
```

#### 资深解法：尽量拆成 3

算法思想：乘积最大时优先拆 3；余数为 1 时把 `3+1` 改成 `2+2`。

```java
class Solution {
    public int integerBreak(int n) {
        if (n <= 3) return n - 1;
        int ans = 1;
        while (n > 4) {
            ans *= 3;
            n -= 3;
        }
        return ans * n;
    }
}
```

复杂度：基础版时间 `O(n^2)`、空间 `O(n)`；资深版时间 `O(n)`、空间 `O(1)`。

#### 基础语法与算法思想

- 整数拆分最大乘积的核心结论是“尽量使用 3”。

---

## 344. 反转字符串 (Easy)

编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组  `s`  的形式给出。
不要给另外的数组分配额外的空间，你必须 **原地修改输入数组** 、使用 O(1) 的额外空间解决这一问题。
 
 **示例 1：** 

```text
输入：s = ["h","e","l","l","o"]
输出：["o","l","l","e","h"]
```

 **示例 2：** 

```text
输入：s = ["H","a","n","n","a","h"]
输出：["h","a","n","n","a","H"]
```

 
 **提示：** 

 `1 <= s.length <= 105` 
 `s[i]`  都是 ASCII 码表中的可打印字符

### Java 解法补充

#### 基础解法：借助新数组反向复制

算法思想：创建一份拷贝，从后往前写回原数组。

```java
class Solution {
    public void reverseString(char[] s) {
        char[] copy = s.clone();
        for (int i = 0; i < s.length; i++) s[i] = copy[s.length - 1 - i];
    }
}
```

#### 资深解法：双指针原地交换

算法思想：左右指针向中间移动，每次交换一对字符。

```java
class Solution {
    public void reverseString(char[] s) {
        int left = 0, right = s.length - 1;
        while (left < right) {
            char temp = s[left];
            s[left++] = s[right];
            s[right--] = temp;
        }
    }
}
```

复杂度：基础版空间 `O(n)`，资深版空间 `O(1)`；时间均为 `O(n)`。

#### 基础语法与算法思想

- 原地反转数组常用左右双指针。

---

## 345. 反转字符串中的元音字母 (Easy)

给你一个字符串  `s`  ，仅反转字符串中的所有元音字母，并返回结果字符串。
元音字母包括  `'a'` 、 `'e'` 、 `'i'` 、 `'o'` 、 `'u'` ，且可能以大小写两种形式出现不止一次。
 
 **示例 1：** 

 **输入：** s = "IceCreAm"
 **输出：** "AceCreIm"
 **解释：** 
 `s`  中的元音是  `['I', 'e', 'e', 'A']` 。反转这些元音， `s`  变为  `"AceCreIm"` .

 **示例 2：** 

 **输入：** s = "leetcode"
 **输出：** "leotcede"
 

 **提示：** 

 `1 <= s.length <= 3 * 105` 
 `s`  由  **可打印的 ASCII**  字符组成

### Java 解法补充

#### 基础解法：收集元音后再回填

算法思想：先按顺序收集所有元音，再从后往前回填到原来的元音位置。

```java
class Solution {
    public String reverseVowels(String s) {
        List<Character> vowels = new ArrayList<>();
        for (char c : s.toCharArray()) if (isVowel(c)) vowels.add(c);
        char[] arr = s.toCharArray();
        int index = vowels.size() - 1;
        for (int i = 0; i < arr.length; i++) {
            if (isVowel(arr[i])) arr[i] = vowels.get(index--);
        }
        return new String(arr);
    }

    private boolean isVowel(char c) {
        return "aeiouAEIOU".indexOf(c) >= 0;
    }
}
```

#### 资深解法：双指针只交换元音

算法思想：左右指针分别跳过非元音，遇到元音就交换。

```java
class Solution {
    public String reverseVowels(String s) {
        char[] arr = s.toCharArray();
        int left = 0, right = arr.length - 1;
        while (left < right) {
            while (left < right && !isVowel(arr[left])) left++;
            while (left < right && !isVowel(arr[right])) right--;
            char temp = arr[left];
            arr[left++] = arr[right];
            arr[right--] = temp;
        }
        return new String(arr);
    }

    private boolean isVowel(char c) {
        return "aeiouAEIOU".indexOf(c) >= 0;
    }
}
```

复杂度：时间 `O(n)`；基础版空间 `O(n)`，资深版空间 `O(n)` 用于字符数组。

#### 基础语法与算法思想

- 字符串不可变，通常转成 `char[]` 再修改。

---

## 346. 数据流中的移动平均值 (Easy)

暂无内容描述。

### Java 解法补充

#### 基础解法：保存窗口内所有值并每次求和

算法思想：列表保存最近元素，超过窗口大小就删除最早值，查询时循环求和。

```java
class MovingAverage {
    private int size;
    private List<Integer> window = new ArrayList<>();

    public MovingAverage(int size) {
        this.size = size;
    }

    public double next(int val) {
        window.add(val);
        if (window.size() > size) window.remove(0);
        int sum = 0;
        for (int x : window) sum += x;
        return (double) sum / window.size();
    }
}
```

#### 资深解法：队列加滚动和

算法思想：队列维护窗口，`sum` 维护当前窗口总和，新增和移除都 `O(1)`。

```java
class MovingAverage {
    private int size;
    private Queue<Integer> queue = new ArrayDeque<>();
    private double sum = 0;

    public MovingAverage(int size) {
        this.size = size;
    }

    public double next(int val) {
        queue.offer(val);
        sum += val;
        if (queue.size() > size) sum -= queue.poll();
        return sum / queue.size();
    }
}
```

复杂度：基础版每次 `O(k)`，资深版每次 `O(1)`；空间 `O(k)`。

#### 基础语法与算法思想

- 滑动窗口聚合值适合用队列和滚动变量维护。

---

## 347. 前 K 个高频元素 (Medium)

给你一个整数数组  `nums`  和一个整数  `k`  ，请你返回其中出现频率前  `k`  高的元素。你可以按  **任意顺序**  返回答案。
 
 **示例 1：** 

 **输入：** nums = [1,1,1,2,2,3], k = 2
 **输出：** [1,2]

 **示例 2：** 

 **输入：** nums = [1], k = 1
 **输出：** [1]

 **示例 3：** 

 **输入：** nums = [1,2,1,2,1,2,3,1,3,2], k = 2
 **输出：** [1,2]

 
 **提示：** 

 `1 <= nums.length <= 105` 
 `-104 <= nums[i] <= 104` 
 `k`  的取值范围是  `[1, 数组中不相同的元素的个数]` 
题目数据保证答案唯一，换句话说，数组中前  `k`  个高频元素的集合是唯一的

 
 **进阶：** 你所设计算法的时间复杂度  **必须**  优于  `O(n log n)`  ，其中  `n`  是数组大小。

### Java 解法补充

#### 基础解法：哈希计数后排序

算法思想：先统计频次，再按频次降序排序取前 `k` 个。

```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> count = new HashMap<>();
        for (int x : nums) count.put(x, count.getOrDefault(x, 0) + 1);
        List<Integer> keys = new ArrayList<>(count.keySet());
        keys.sort((a, b) -> count.get(b) - count.get(a));
        int[] ans = new int[k];
        for (int i = 0; i < k; i++) ans[i] = keys.get(i);
        return ans;
    }
}
```

#### 资深解法：小根堆保留前 K 高频

算法思想：堆里只保留目前最高频的 `k` 个元素，频次更低的会被弹出。

```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> count = new HashMap<>();
        for (int x : nums) count.put(x, count.getOrDefault(x, 0) + 1);
        PriorityQueue<Integer> heap = new PriorityQueue<>((a, b) -> count.get(a) - count.get(b));
        for (int x : count.keySet()) {
            heap.offer(x);
            if (heap.size() > k) heap.poll();
        }
        int[] ans = new int[k];
        for (int i = 0; i < k; i++) ans[i] = heap.poll();
        return ans;
    }
}
```

复杂度：基础版 `O(n log n)`，资深版 `O(n log k)`。

#### 基础语法与算法思想

- Top K 高频常用“计数 + 堆”。

---

## 348. 设计井字棋 (Medium)

暂无内容描述。

### Java 解法补充

#### 基础解法：保存棋盘并每步扫描胜负

算法思想：落子后扫描该玩家是否占满某一行、列或对角线。

```java
class TicTacToe {
    private int[][] board;

    public TicTacToe(int n) {
        board = new int[n][n];
    }

    public int move(int row, int col, int player) {
        board[row][col] = player;
        int n = board.length;
        boolean winRow = true, winCol = true, winDiag = true, winAnti = true;
        for (int i = 0; i < n; i++) {
            winRow &= board[row][i] == player;
            winCol &= board[i][col] == player;
            winDiag &= board[i][i] == player;
            winAnti &= board[i][n - 1 - i] == player;
        }
        return winRow || winCol || winDiag || winAnti ? player : 0;
    }
}
```

#### 资深解法：行列和对角线计数

算法思想：玩家 1 记 `+1`，玩家 2 记 `-1`，某条线绝对值达到 `n` 即获胜。

```java
class TicTacToe {
    private int[] rows, cols;
    private int diag, anti, n;

    public TicTacToe(int n) {
        this.n = n;
        rows = new int[n];
        cols = new int[n];
    }

    public int move(int row, int col, int player) {
        int add = player == 1 ? 1 : -1;
        rows[row] += add;
        cols[col] += add;
        if (row == col) diag += add;
        if (row + col == n - 1) anti += add;
        return Math.abs(rows[row]) == n || Math.abs(cols[col]) == n
                || Math.abs(diag) == n || Math.abs(anti) == n ? player : 0;
    }
}
```

复杂度：基础版每步 `O(n)`，资深版每步 `O(1)`。

#### 基础语法与算法思想

- 设计题要把每次操作的重复扫描转成增量状态。

---

## 349. 两个数组的交集 (Easy)

给定两个数组  `nums1`  和  `nums2`  ，返回 它们的 交集 。输出结果中的每个元素一定是  **唯一**  的。我们可以  **不考虑输出结果的顺序**  。
 
 **示例 1：** 

```text
输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2]
```

 **示例 2：** 

```text
输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出：[9,4]
解释：[4,9] 也是可通过的
```

 
 **提示：** 

 `1 <= nums1.length, nums2.length <= 1000` 
 `0 <= nums1[i], nums2[i] <= 1000`

### Java 解法补充

#### 基础解法：双集合

算法思想：把第一个数组放入集合，遍历第二个数组时命中就加入答案集合。

```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        Set<Integer> set = new HashSet<>();
        for (int x : nums1) set.add(x);
        Set<Integer> ans = new HashSet<>();
        for (int x : nums2) if (set.contains(x)) ans.add(x);
        int[] res = new int[ans.size()];
        int i = 0;
        for (int x : ans) res[i++] = x;
        return res;
    }
}
```

#### 资深解法：遍历较小集合

算法思想：先把较小数组建集合，降低常数开销。

```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        if (nums1.length > nums2.length) return intersection(nums2, nums1);
        Set<Integer> small = new HashSet<>();
        for (int x : nums1) small.add(x);
        Set<Integer> ans = new HashSet<>();
        for (int x : nums2) if (small.remove(x)) ans.add(x);
        return ans.stream().mapToInt(Integer::intValue).toArray();
    }
}
```

复杂度：时间 `O(m+n)`，空间 `O(min(m,n))`。

#### 基础语法与算法思想

- 唯一交集天然适合集合去重。

---

## 350. 两个数组的交集 II (Easy)

给你两个整数数组  `nums1`  和  `nums2`  ，请你以数组形式返回两数组的交集。返回结果中每个元素出现的次数，应与元素在两个数组中都出现的次数一致（如果出现次数不一致，则考虑取较小值）。可以不考虑输出结果的顺序。
 
 **示例 1：** 

```text
输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2,2]
```

 **示例 2:** 

```text
输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出：[4,9]
```

 
 **提示：** 

 `1 <= nums1.length, nums2.length <= 1000` 
 `0 <= nums1[i], nums2[i] <= 1000` 

 
 **进阶：** 

如果给定的数组已经排好序呢？你将如何优化你的算法？
如果  `nums1`  的大小比  `nums2`  小，哪种方法更优？
如果  `nums2`  的元素存储在磁盘上，内存是有限的，并且你不能一次加载所有的元素到内存中，你该怎么办？

### Java 解法补充

#### 基础解法：哈希表计数

算法思想：统计 `nums1` 每个数字出现次数，遍历 `nums2` 时扣减并收集。

```java
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        Map<Integer, Integer> count = new HashMap<>();
        for (int x : nums1) count.put(x, count.getOrDefault(x, 0) + 1);
        List<Integer> ans = new ArrayList<>();
        for (int x : nums2) {
            int c = count.getOrDefault(x, 0);
            if (c > 0) {
                ans.add(x);
                count.put(x, c - 1);
            }
        }
        return ans.stream().mapToInt(Integer::intValue).toArray();
    }
}
```

#### 资深解法：排序加双指针

算法思想：两个数组排序后同时移动，值相等就收集，较小值指针右移。

```java
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        Arrays.sort(nums1);
        Arrays.sort(nums2);
        List<Integer> ans = new ArrayList<>();
        int i = 0, j = 0;
        while (i < nums1.length && j < nums2.length) {
            if (nums1[i] == nums2[j]) {
                ans.add(nums1[i]);
                i++;
                j++;
            } else if (nums1[i] < nums2[j]) i++;
            else j++;
        }
        return ans.stream().mapToInt(Integer::intValue).toArray();
    }
}
```

复杂度：哈希法时间 `O(m+n)`，排序法时间 `O(m log m+n log n)`。

#### 基础语法与算法思想

- 多重交集要保留频次，不能只用集合。

---

## 351. 安卓系统手势解锁 (Medium)

暂无内容描述。

### Java 解法补充

#### 基础解法：回溯枚举每条手势路径

算法思想：用 `skip[a][b]` 表示从 a 到 b 中间必须先经过哪个点，回溯时检查是否可走。

```java
class Solution {
    public int numberOfPatterns(int m, int n) {
        int[][] skip = new int[10][10];
        skip[1][3] = skip[3][1] = 2; skip[1][7] = skip[7][1] = 4;
        skip[3][9] = skip[9][3] = 6; skip[7][9] = skip[9][7] = 8;
        skip[1][9] = skip[9][1] = skip[3][7] = skip[7][3] = 5;
        skip[2][8] = skip[8][2] = skip[4][6] = skip[6][4] = 5;
        boolean[] used = new boolean[10];
        int ans = 0;
        for (int len = m; len <= n; len++) {
            for (int start = 1; start <= 9; start++) ans += dfs(start, len - 1, used, skip);
        }
        return ans;
    }

    private int dfs(int cur, int remain, boolean[] used, int[][] skip) {
        if (remain == 0) return 1;
        used[cur] = true;
        int ans = 0;
        for (int next = 1; next <= 9; next++) {
            int mid = skip[cur][next];
            if (!used[next] && (mid == 0 || used[mid])) ans += dfs(next, remain - 1, used, skip);
        }
        used[cur] = false;
        return ans;
    }
}
```

#### 资深解法：利用九宫格对称性

算法思想：1/3/7/9 等价，2/4/6/8 等价，5 单独计算，可减少重复搜索。

```java
class Solution {
    public int numberOfPatterns(int m, int n) {
        int[][] skip = new int[10][10];
        skip[1][3] = skip[3][1] = 2; skip[1][7] = skip[7][1] = 4;
        skip[3][9] = skip[9][3] = 6; skip[7][9] = skip[9][7] = 8;
        skip[1][9] = skip[9][1] = skip[3][7] = skip[7][3] = 5;
        skip[2][8] = skip[8][2] = skip[4][6] = skip[6][4] = 5;
        boolean[] used = new boolean[10];
        int ans = 0;
        for (int len = m; len <= n; len++) {
            ans += dfs(1, len - 1, used, skip) * 4;
            ans += dfs(2, len - 1, used, skip) * 4;
            ans += dfs(5, len - 1, used, skip);
        }
        return ans;
    }

    private int dfs(int cur, int remain, boolean[] used, int[][] skip) {
        if (remain == 0) return 1;
        used[cur] = true;
        int ans = 0;
        for (int next = 1; next <= 9; next++) {
            int mid = skip[cur][next];
            if (!used[next] && (mid == 0 || used[mid])) ans += dfs(next, remain - 1, used, skip);
        }
        used[cur] = false;
        return ans;
    }
}
```

复杂度：搜索状态有限，空间 `O(1)`。

#### 基础语法与算法思想

- 回溯题的合法性约束可以预处理成表。

---

## 352. 将数据流变为多个不相交区间 (Hard)

给你一个由非负整数组成的数据流输入  `a1, a2, ..., an` ，请你将目前为止看到的数字汇总为一组不相交的区间列表。
实现  `SummaryRanges`  类：

 `SummaryRanges()`  初始化一个空的数据流对象。
 `void addNum(int value)`  将整数  `value`  添加到数据流中。
 `int[][] getIntervals()`  返回当前数据流中的整数汇总为一组不相交的区间列表  `[starti, endi]` 。答案应按  `starti`  升序排序。

 
 **示例 1：** 

```text
输入
["SummaryRanges", "addNum", "getIntervals", "addNum", "getIntervals", "addNum", "getIntervals", "addNum", "getIntervals", "addNum", "getIntervals"]
[[], [1], [], [3], [], [7], [], [2], [], [6], []]
输出
[null, null, [[1, 1]], null, [[1, 1], [3, 3]], null, [[1, 1], [3, 3], [7, 7]], null, [[1, 3], [7, 7]], null, [[1, 3], [6, 7]]]

解释
SummaryRanges summaryRanges = new SummaryRanges();
summaryRanges.addNum(1);      // arr = [1]
summaryRanges.getIntervals(); // 返回 [[1, 1]]
summaryRanges.addNum(3);      // arr = [1, 3]
summaryRanges.getIntervals(); // 返回 [[1, 1], [3, 3]]
summaryRanges.addNum(7);      // arr = [1, 3, 7]
summaryRanges.getIntervals(); // 返回 [[1, 1], [3, 3], [7, 7]]
summaryRanges.addNum(2);      // arr = [1, 2, 3, 7]
summaryRanges.getIntervals(); // 返回 [[1, 3], [7, 7]]
summaryRanges.addNum(6);      // arr = [1, 2, 3, 6, 7]
summaryRanges.getIntervals(); // 返回 [[1, 3], [6, 7]]
```

 
 **提示：** 

 `0 <= value <= 104` 
最多会调用  `addNum`  和  `getIntervals`  方法  `3 * 104`  次。
最多会调用  `getIntervals`  方法  `102`  次。

 
 **进阶：** 如果存在大量合并，并且与数据流的大小相比，不相交区间的数量很小，该怎么办?

### Java 解法补充

#### 基础解法：保存所有数字，查询时排序合并

算法思想：`addNum` 只入集合，`getIntervals` 时排序所有数字并合并连续段。

```java
class SummaryRanges {
    private Set<Integer> set = new HashSet<>();

    public void addNum(int value) {
        set.add(value);
    }

    public int[][] getIntervals() {
        List<Integer> nums = new ArrayList<>(set);
        Collections.sort(nums);
        List<int[]> ans = new ArrayList<>();
        for (int x : nums) {
            if (ans.isEmpty() || ans.get(ans.size() - 1)[1] + 1 < x) ans.add(new int[]{x, x});
            else ans.get(ans.size() - 1)[1] = x;
        }
        return ans.toArray(new int[ans.size()][]);
    }
}
```

#### 资深解法：TreeMap 动态维护区间

算法思想：按左端点存区间，新增数字时检查左右相邻区间是否可合并。

```java
class SummaryRanges {
    private TreeMap<Integer, int[]> map = new TreeMap<>();

    public void addNum(int value) {
        Map.Entry<Integer, int[]> floor = map.floorEntry(value);
        if (floor != null && floor.getValue()[1] >= value) return;
        Integer left = map.floorKey(value), right = map.ceilingKey(value);
        boolean mergeLeft = left != null && map.get(left)[1] + 1 == value;
        boolean mergeRight = right != null && right == value + 1;
        if (mergeLeft && mergeRight) {
            map.get(left)[1] = map.get(right)[1];
            map.remove(right);
        } else if (mergeLeft) map.get(left)[1] = value;
        else if (mergeRight) {
            int end = map.remove(right)[1];
            map.put(value, new int[]{value, end});
        } else map.put(value, new int[]{value, value});
    }

    public int[][] getIntervals() {
        return map.values().toArray(new int[map.size()][]);
    }
}
```

复杂度：基础版查询 `O(n log n)`；资深版新增 `O(log n)`。

#### 基础语法与算法思想

- 有序映射适合查询左右邻居并维护区间。

---

## 353. 贪吃蛇 (Medium)

暂无内容描述。

### Java 解法补充

#### 基础解法：用列表保存蛇身

算法思想：每次移动先计算新头，判断撞墙或撞到身体，再处理是否吃到食物。

```java
class SnakeGame {
    private int width, height, score = 0, foodIndex = 0;
    private int[][] food;
    private LinkedList<int[]> body = new LinkedList<>();

    public SnakeGame(int width, int height, int[][] food) {
        this.width = width;
        this.height = height;
        this.food = food;
        body.add(new int[]{0, 0});
    }

    public int move(String direction) {
        int[] head = body.getFirst();
        int r = head[0], c = head[1];
        if (direction.equals("U")) r--; else if (direction.equals("D")) r++;
        else if (direction.equals("L")) c--; else c++;
        if (r < 0 || r >= height || c < 0 || c >= width) return -1;
        boolean eat = foodIndex < food.length && food[foodIndex][0] == r && food[foodIndex][1] == c;
        if (!eat) body.removeLast();
        for (int[] p : body) if (p[0] == r && p[1] == c) return -1;
        body.addFirst(new int[]{r, c});
        if (eat) { score++; foodIndex++; }
        return score;
    }
}
```

#### 资深解法：Deque 加 HashSet 判断身体碰撞

算法思想：蛇身顺序用双端队列，身体占用格用集合；移动时先移尾再判断新头。

```java
class SnakeGame {
    private int width, height, score, foodIndex;
    private int[][] food;
    private Deque<Integer> body = new ArrayDeque<>();
    private Set<Integer> occupied = new HashSet<>();

    public SnakeGame(int width, int height, int[][] food) {
        this.width = width; this.height = height; this.food = food;
        body.offerFirst(0); occupied.add(0);
    }

    public int move(String direction) {
        int head = body.peekFirst(), r = head / width, c = head % width;
        if (direction.equals("U")) r--; else if (direction.equals("D")) r++;
        else if (direction.equals("L")) c--; else c++;
        if (r < 0 || r >= height || c < 0 || c >= width) return -1;
        boolean eat = foodIndex < food.length && food[foodIndex][0] == r && food[foodIndex][1] == c;
        if (!eat) occupied.remove(body.pollLast());
        int next = r * width + c;
        if (occupied.contains(next)) return -1;
        body.offerFirst(next); occupied.add(next);
        if (eat) { score++; foodIndex++; }
        return score;
    }
}
```

复杂度：基础版碰撞 `O(length)`；资深版每步 `O(1)`。

#### 基础语法与算法思想

- 坐标可压缩成 `row * width + col` 存入集合。

---

## 354. 俄罗斯套娃信封问题 (Hard)

给你一个二维整数数组  `envelopes`  ，其中  `envelopes[i] = [wi, hi]`  ，表示第  `i`  个信封的宽度和高度。
当另一个信封的宽度和高度都比这个信封大的时候，这个信封就可以放进另一个信封里，如同俄罗斯套娃一样。
请计算  **最多能有多少个**  信封能组成一组“俄罗斯套娃”信封（即可以把一个信封放到另一个信封里面）。
 **注意** ：不允许旋转信封。
 

 **示例 1：** 

```text
输入：envelopes = [[5,4],[6,4],[6,7],[2,3]]
输出：3
解释：最多信封的个数为 3, 组合为: [2,3] => [5,4] => [6,7]。
```

 **示例 2：** 

```text
输入：envelopes = [[1,1],[1,1],[1,1]]
输出：1
```

 
 **提示：** 

 `1 <= envelopes.length <= 105` 
 `envelopes[i].length == 2` 
 `1 <= wi, hi <= 105`

### Java 解法补充

#### 基础解法：排序后二维 DP

算法思想：按宽度、高度升序排序，`dp[i]` 表示以第 `i` 个信封结尾的最长套娃数量。

```java
class Solution {
    public int maxEnvelopes(int[][] envelopes) {
        Arrays.sort(envelopes, (a, b) -> a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]);
        int n = envelopes.length, ans = 1;
        int[] dp = new int[n];
        Arrays.fill(dp, 1);
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (envelopes[j][0] < envelopes[i][0] && envelopes[j][1] < envelopes[i][1]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
            ans = Math.max(ans, dp[i]);
        }
        return ans;
    }
}
```

#### 资深解法：排序后转成高度 LIS

算法思想：宽度升序、高度降序排序，避免同宽信封互套，然后对高度求 LIS。

```java
class Solution {
    public int maxEnvelopes(int[][] envelopes) {
        Arrays.sort(envelopes, (a, b) -> a[0] == b[0] ? b[1] - a[1] : a[0] - b[0]);
        int[] tails = new int[envelopes.length];
        int size = 0;
        for (int[] e : envelopes) {
            int l = 0, r = size;
            while (l < r) {
                int m = (l + r) / 2;
                if (tails[m] < e[1]) l = m + 1;
                else r = m;
            }
            tails[l] = e[1];
            if (l == size) size++;
        }
        return size;
    }
}
```

复杂度：基础版 `O(n^2)`；资深版 `O(n log n)`。

#### 基础语法与算法思想

- 同一宽度高度要降序，防止被 LIS 错误串起来。

---

## 355. 设计推特 (Medium)

设计一个简化版的推特(Twitter)，可以让用户实现发送推文，关注/取消关注其他用户，能够看见关注人（包括自己）的最近  `10`  条推文。
实现  `Twitter`  类：

 `Twitter()`  初始化简易版推特对象
 `void postTweet(int userId, int tweetId)`  根据给定的  `tweetId`  和  `userId`  创建一条新推文。每次调用此函数都会使用一个不同的  `tweetId`  。
 `List<Integer> getNewsFeed(int userId)`  检索当前用户新闻推送中最近   `10`  条推文的 ID 。新闻推送中的每一项都必须是由用户关注的人或者是用户自己发布的推文。推文必须  **按照时间顺序由最近到最远排序**  。
 `void follow(int followerId, int followeeId)`  ID 为  `followerId`  的用户开始关注 ID 为  `followeeId`  的用户。
 `void unfollow(int followerId, int followeeId)`  ID 为  `followerId`  的用户不再关注 ID 为  `followeeId`  的用户。

 
 **示例：** 

```text
输入
["Twitter", "postTweet", "getNewsFeed", "follow", "postTweet", "getNewsFeed", "unfollow", "getNewsFeed"]
[[], [1, 5], [1], [1, 2], [2, 6], [1], [1, 2], [1]]
输出
[null, null, [5], null, null, [6, 5], null, [5]]

解释
Twitter twitter = new Twitter();
twitter.postTweet(1, 5); // 用户 1 发送了一条新推文 (用户 id = 1, 推文 id = 5)
twitter.getNewsFeed(1);  // 用户 1 的获取推文应当返回一个列表，其中包含一个 id 为 5 的推文
twitter.follow(1, 2);    // 用户 1 关注了用户 2
twitter.postTweet(2, 6); // 用户 2 发送了一个新推文 (推文 id = 6)
twitter.getNewsFeed(1);  // 用户 1 的获取推文应当返回一个列表，其中包含两个推文，id 分别为 -> [6, 5] 。推文 id 6 应当在推文 id 5 之前，因为它是在 5 之后发送的
twitter.unfollow(1, 2);  // 用户 1 取消关注了用户 2
twitter.getNewsFeed(1);  // 用户 1 获取推文应当返回一个列表，其中包含一个 id 为 5 的推文。因为用户 1 已经不再关注用户 2
```

 
 **提示：** 

 `1 <= userId, followerId, followeeId <= 500` 
 `0 <= tweetId <= 104` 
所有推特的 ID 都互不相同
`postTweet` 、 `getNewsFeed` 、 `follow`  和  `unfollow`  方法最多调用  `3 * 104`  次
用户不能关注自己

### Java 解法补充

#### 基础解法：全局推文列表反向扫描

算法思想：保存所有推文，查询新闻流时从后往前扫描，取自己和关注者的最近 10 条。

```java
class Twitter {
    private List<int[]> tweets = new ArrayList<>();
    private Map<Integer, Set<Integer>> follow = new HashMap<>();
    private int time = 0;

    public void postTweet(int userId, int tweetId) {
        tweets.add(new int[]{time++, userId, tweetId});
    }

    public List<Integer> getNewsFeed(int userId) {
        Set<Integer> users = follow.getOrDefault(userId, new HashSet<>());
        List<Integer> ans = new ArrayList<>();
        for (int i = tweets.size() - 1; i >= 0 && ans.size() < 10; i--) {
            int[] t = tweets.get(i);
            if (t[1] == userId || users.contains(t[1])) ans.add(t[2]);
        }
        return ans;
    }

    public void follow(int followerId, int followeeId) {
        if (followerId != followeeId) follow.computeIfAbsent(followerId, k -> new HashSet<>()).add(followeeId);
    }

    public void unfollow(int followerId, int followeeId) {
        follow.getOrDefault(followerId, new HashSet<>()).remove(followeeId);
    }
}
```

#### 资深解法：按用户存推文并用堆合并

算法思想：每个用户只看自己的推文链，新闻流用大根堆按时间合并关注者的最新推文。

```java
class Twitter {
    private static class Tweet {
        int id, time;
        Tweet next;
        Tweet(int id, int time, Tweet next) { this.id = id; this.time = time; this.next = next; }
    }

    private int time = 0;
    private Map<Integer, Tweet> tweets = new HashMap<>();
    private Map<Integer, Set<Integer>> follow = new HashMap<>();

    public void postTweet(int userId, int tweetId) {
        tweets.put(userId, new Tweet(tweetId, time++, tweets.get(userId)));
    }

    public List<Integer> getNewsFeed(int userId) {
        PriorityQueue<Tweet> heap = new PriorityQueue<>((a, b) -> b.time - a.time);
        Set<Integer> users = new HashSet<>(follow.getOrDefault(userId, new HashSet<>()));
        users.add(userId);
        for (int u : users) if (tweets.get(u) != null) heap.offer(tweets.get(u));
        List<Integer> ans = new ArrayList<>();
        while (!heap.isEmpty() && ans.size() < 10) {
            Tweet t = heap.poll();
            ans.add(t.id);
            if (t.next != null) heap.offer(t.next);
        }
        return ans;
    }

    public void follow(int followerId, int followeeId) {
        if (followerId != followeeId) follow.computeIfAbsent(followerId, k -> new HashSet<>()).add(followeeId);
    }

    public void unfollow(int followerId, int followeeId) {
        Set<Integer> set = follow.get(followerId);
        if (set != null) set.remove(followeeId);
    }
}
```

复杂度：基础版查询 `O(T)`；资深版查询 `O((f+10)log f)`。

#### 基础语法与算法思想

- 多个有序流取最近 10 条，用堆合并比全局扫描更可控。

---

## 356. 直线镜像 (Medium)

暂无内容描述。

### Java 解法补充

#### 基础解法：尝试用最小最大 x 确定镜像线

算法思想：镜像线若存在，一定是 `(minX + maxX) / 2`，每个点都要有对应镜像点。

```java
class Solution {
    public boolean isReflected(int[][] points) {
        int min = Integer.MAX_VALUE, max = Integer.MIN_VALUE;
        Set<String> set = new HashSet<>();
        for (int[] p : points) {
            min = Math.min(min, p[0]);
            max = Math.max(max, p[0]);
            set.add(p[0] + "," + p[1]);
        }
        int sum = min + max;
        for (int[] p : points) {
            if (!set.contains((sum - p[0]) + "," + p[1])) return false;
        }
        return true;
    }
}
```

#### 资深解法：避免浮点镜像线

算法思想：直接用 `minX + maxX` 表示两倍镜像线，避免小数精度问题；实现同上。

```java
class Solution {
    public boolean isReflected(int[][] points) {
        int minX = Integer.MAX_VALUE, maxX = Integer.MIN_VALUE;
        Set<String> seen = new HashSet<>();
        for (int[] p : points) {
            minX = Math.min(minX, p[0]);
            maxX = Math.max(maxX, p[0]);
            seen.add(p[0] + ":" + p[1]);
        }
        int mirror2 = minX + maxX;
        for (int[] p : points) {
            if (!seen.contains((mirror2 - p[0]) + ":" + p[1])) return false;
        }
        return true;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 基础语法与算法思想

- 几何题能用整数表达时尽量避免浮点数。

---

## 357. 统计各位数字都不同的数字个数 (Medium)

给你一个整数  `n`  ，统计并返回各位数字都不同的数字  `x`  的个数，其中  `0 <= x < 10n`  。

 
 **示例 1：** 

```text
输入：n = 2
输出：91
解释：答案应为除去 11、22、33、44、55、66、77、88、99 外，在 0 ≤ x < 100 范围内的所有数字。
```

 **示例 2：** 

```text
输入：n = 0
输出：1
```

 
 **提示：** 

 `0 <= n <= 8`

### Java 解法补充

#### 基础解法：逐个数字判断是否有重复位

算法思想：枚举 `0 <= x < 10^n` 的所有数字，用布尔数组判断每个数字是否有重复位。

```java
class Solution {
    public int countNumbersWithUniqueDigits(int n) {
        int limit = (int) Math.pow(10, n), ans = 0;
        for (int x = 0; x < limit; x++) if (unique(x)) ans++;
        return ans;
    }

    private boolean unique(int x) {
        boolean[] seen = new boolean[10];
        if (x == 0) return true;
        while (x > 0) {
            int d = x % 10;
            if (seen[d]) return false;
            seen[d] = true;
            x /= 10;
        }
        return true;
    }
}
```

#### 资深解法：排列计数

算法思想：长度为 `len` 的数首位 9 种选择，后续依次有 `9,8,7...` 种选择。

```java
class Solution {
    public int countNumbersWithUniqueDigits(int n) {
        if (n == 0) return 1;
        int ans = 10, cur = 9, available = 9;
        for (int len = 2; len <= n; len++) {
            cur *= available--;
            ans += cur;
        }
        return ans;
    }
}
```

复杂度：基础版 `O(10^n * n)`；资深版 `O(n)`。

#### 基础语法与算法思想

- 计数题常从“每一位还有多少选择”入手。

---

## 358. K 距离间隔重排字符串 (Hard)

暂无内容描述。

### Java 解法补充

#### 基础解法：每次选择当前可用且频次最高的字符

算法思想：逐位构造结果，每次扫描所有字符，选择距离上次出现至少 `k` 且剩余最多的字符。

```java
class Solution {
    public String rearrangeString(String s, int k) {
        int[] count = new int[26], last = new int[26];
        Arrays.fill(last, -k);
        for (char c : s.toCharArray()) count[c - 'a']++;
        StringBuilder ans = new StringBuilder();
        for (int i = 0; i < s.length(); i++) {
            int pick = -1;
            for (int c = 0; c < 26; c++) {
                if (count[c] > 0 && i - last[c] >= k && (pick == -1 || count[c] > count[pick])) pick = c;
            }
            if (pick == -1) return "";
            ans.append((char) ('a' + pick));
            count[pick]--;
            last[pick] = i;
        }
        return ans.toString();
    }
}
```

#### 资深解法：大根堆加冷却队列

算法思想：堆中放当前可用字符，队列中保存冷却中的字符和释放位置。

```java
class Solution {
    public String rearrangeString(String s, int k) {
        if (k <= 1) return s;
        int[] count = new int[26];
        for (char c : s.toCharArray()) count[c - 'a']++;
        PriorityQueue<int[]> heap = new PriorityQueue<>((a, b) -> b[1] - a[1]);
        for (int i = 0; i < 26; i++) if (count[i] > 0) heap.offer(new int[]{i, count[i]});
        Queue<int[]> wait = new ArrayDeque<>();
        StringBuilder ans = new StringBuilder();
        for (int pos = 0; pos < s.length(); pos++) {
            if (!wait.isEmpty() && wait.peek()[2] <= pos) heap.offer(wait.poll());
            if (heap.isEmpty()) return "";
            int[] cur = heap.poll();
            ans.append((char) ('a' + cur[0]));
            if (--cur[1] > 0) {
                cur = new int[]{cur[0], cur[1], pos + k};
                wait.offer(cur);
            }
        }
        return ans.toString();
    }
}
```

复杂度：时间 `O(n log 26)`，空间 `O(26)`。

#### 基础语法与算法思想

- 带间隔约束的重排常用“优先队列 + 冷却队列”。

---

## 359. 日志速率限制器 (Easy)

暂无内容描述。

### Java 解法补充

#### 基础解法：哈希表记录上次打印时间

算法思想：如果消息没出现过，或距离上次打印至少 10 秒，就允许打印并更新时间。

```java
class Logger {
    private Map<String, Integer> last = new HashMap<>();

    public boolean shouldPrintMessage(int timestamp, String message) {
        if (!last.containsKey(message) || timestamp - last.get(message) >= 10) {
            last.put(message, timestamp);
            return true;
        }
        return false;
    }
}
```

#### 资深解法：同哈希表，按业务语义封装 TTL

算法思想：将 10 秒窗口视为消息级 TTL，代码中用常量表达规则，方便调整。

```java
class Logger {
    private static final int WINDOW = 10;
    private final Map<String, Integer> nextAllowed = new HashMap<>();

    public boolean shouldPrintMessage(int timestamp, String message) {
        int allowed = nextAllowed.getOrDefault(message, 0);
        if (timestamp < allowed) return false;
        nextAllowed.put(message, timestamp + WINDOW);
        return true;
    }
}
```

复杂度：每次调用时间 `O(1)`，空间 `O(消息种类数)`。

#### 基础语法与算法思想

- 限流器本质是记录某个 key 的下一次允许时间。

---

## 360. 有序转化数组 (Medium)

暂无内容描述。

### Java 解法补充

#### 基础解法：逐个计算后排序

算法思想：把每个数代入二次函数，再对结果排序。

```java
class Solution {
    public int[] sortTransformedArray(int[] nums, int a, int b, int c) {
        int[] ans = new int[nums.length];
        for (int i = 0; i < nums.length; i++) ans[i] = f(nums[i], a, b, c);
        Arrays.sort(ans);
        return ans;
    }

    private int f(int x, int a, int b, int c) {
        return a * x * x + b * x + c;
    }
}
```

#### 资深解法：双指针利用抛物线单调性

算法思想：原数组有序，二次函数最大或最小值会出现在两端；`a >= 0` 时从后往前填较大值，否则从前往后填较小值。

```java
class Solution {
    public int[] sortTransformedArray(int[] nums, int a, int b, int c) {
        int n = nums.length, left = 0, right = n - 1;
        int[] ans = new int[n];
        int index = a >= 0 ? n - 1 : 0;
        while (left <= right) {
            int lv = f(nums[left], a, b, c), rv = f(nums[right], a, b, c);
            if (a >= 0) {
                if (lv > rv) { ans[index--] = lv; left++; }
                else { ans[index--] = rv; right--; }
            } else {
                if (lv < rv) { ans[index++] = lv; left++; }
                else { ans[index++] = rv; right--; }
            }
        }
        return ans;
    }

    private int f(int x, int a, int b, int c) {
        return a * x * x + b * x + c;
    }
}
```

复杂度：基础版 `O(n log n)`；资深版 `O(n)`。

#### 基础语法与算法思想

- 有序数组经过二次函数变换后，可从两端按大小归并。

---

