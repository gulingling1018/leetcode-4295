## 91. 解码方法 (Medium)

一条包含字母  `A-Z`  的消息通过以下映射进行了 编码 ：
 `"1" -> 'A'
"2" -> 'B'
...
"25" -> 'Y'
"26" -> 'Z'` 
然而，在 解码 已编码的消息时，你意识到有许多不同的方式来解码，因为有些编码被包含在其它编码当中（ `"2"`  和  `"5"`  与  `"25"` ）。
例如， `"11106"`  可以映射为：

 `"AAJF"`  ，将消息分组为  `(1, 1, 10, 6)` 
 `"KJF"`  ，将消息分组为  `(11, 10, 6)` 
消息不能分组为   `(1, 11, 06)`  ，因为  `"06"`  不是一个合法编码（只有 "6" 是合法的）。

注意，可能存在无法解码的字符串。
给你一个只含数字的 非空 字符串  `s`  ，请计算并返回 解码 方法的 总数 。如果没有合法的方式解码整个字符串，返回  `0` 。
题目数据保证答案肯定是一个 32 位 的整数。
 
示例 1：

```text
输入：s = "12"
输出：2
解释：它可以解码为 "AB"（1 2）或者 "L"（12）。
```

示例 2：

```text
输入：s = "226"
输出：3
解释：它可以解码为 "BZ" (2 26), "VF" (22 6), 或者 "BBF" (2 2 6) 。
```

示例 3：

```text
输入：s = "06"
输出：0
解释："06" 无法映射到 "F" ，因为存在前导零（"6" 和 "06" 并不等价）。
```

 
提示：

 `1 <= s.length <= 100` 
 `s`  只包含数字，并且可能包含前导零。

### Java 解法补充

#### 基础解法：递归尝试取 1 位或 2 位

算法思想：递归尝试取 1 位或 2 位，遇到非法前导 0 返回 0。


```java
class Solution {
    public int numDecodings(String s) {
        return dfs(s, 0);
    }

    private int dfs(String s, int i) {
        if (i == s.length()) return 1;
        if (s.charAt(i) == '0') return 0;
        int ans = dfs(s, i + 1);
        if (i + 1 < s.length()) {
            int two = (s.charAt(i) - '0') * 10 + (s.charAt(i + 1) - '0');
            if (two >= 10 && two <= 26) ans += dfs(s, i + 2);
        }
        return ans;
    }
}
```

#### 资深解法：动态规划

算法思想：动态规划，`dp[i]` 表示前 `i` 个字符的解码数。


```java
class Solution {
    public int numDecodings(String s) {
        int n = s.length();
        int[] dp = new int[n + 1];
        dp[0] = 1;
        dp[1] = s.charAt(0) == '0' ? 0 : 1;
        for (int i = 2; i <= n; i++) {
            int one = s.charAt(i - 1) - '0';
            int two = Integer.parseInt(s.substring(i - 2, i));
            if (one >= 1) dp[i] += dp[i - 1];
            if (two >= 10 && two <= 26) dp[i] += dp[i - 2];
        }
        return dp[n];
    }
}
```


#### 基础语法与算法思想

- `substring(l,r)` 左闭右开；解码题要特别处理 `'0'`。

---

## 92. 反转链表 II (Medium)

给你单链表的头指针  `head`  和两个整数  `left`  和  `right`  ，其中  `left <= right`  。请你反转从位置  `left`  到位置  `right`  的链表节点，返回 反转后的链表 。
 
示例 1：

```text
输入：head = [1,2,3,4,5], left = 2, right = 4
输出：[1,4,3,2,5]
```

示例 2：

```text
输入：head = [5], left = 1, right = 1
输出：[5]
```

 
提示：

链表中节点数目为  `n` 
 `1 <= n <= 500` 
 `-500 <= Node.val <= 500` 
 `1 <= left <= right <= n` 

 
进阶： 你可以使用一趟扫描完成反转吗？

### Java 解法补充

#### 基础解法：把链表值放入数组

算法思想：把链表值放入数组，反转 `[left,right]` 的值后写回。


```java
class Solution {
    public ListNode reverseBetween(ListNode head, int left, int right) {
        if (head == null || left == right) return head;
        java.util.List<Integer> values = new java.util.ArrayList<>();
        for (ListNode cur = head; cur != null; cur = cur.next) values.add(cur.val);
        int l = left - 1, r = right - 1;
        while (l < r) {
            int tmp = values.get(l);
            values.set(l, values.get(r));
            values.set(r, tmp);
            l++;
            r--;
        }
        ListNode cur = head;
        int idx = 0;
        while (cur != null) {
            cur.val = values.get(idx++);
            cur = cur.next;
        }
        return head;
    }
}
```

#### 资深解法：虚拟头结点定位区间前驱

算法思想：虚拟头结点定位区间前驱，头插法原地反转区间。


```java
class Solution {
    public ListNode reverseBetween(ListNode head, int left, int right) {
        ListNode dummy = new ListNode(0, head);
        ListNode prev = dummy;
        for (int i = 1; i < left; i++) prev = prev.next;
        ListNode cur = prev.next;
        for (int i = 0; i < right - left; i++) {
            ListNode next = cur.next;
            cur.next = next.next;
            next.next = prev.next;
            prev.next = next;
        }
        return dummy.next;
    }
}
```


#### 基础语法与算法思想

- 头插法每次把 `cur.next` 移到区间最前面；`dummy` 统一处理反转从头开始的情况。

---

## 93. 复原 IP 地址 (Medium)

有效 IP 地址 正好由四个整数（每个整数位于  `0`  到  `255`  之间组成，且不能含有前导  `0` ），整数之间用  `'.'`  分隔。

例如： `"0.1.2.201"`  和 ` "192.168.1.1"`  是 有效 IP 地址，但是  `"0.011.255.245"` 、 `"192.168.1.312"`  和  `"192.168@1.1"`  是 无效 IP 地址。

给定一个只包含数字的字符串  `s`  ，用以表示一个 IP 地址，返回所有可能的有效 IP 地址，这些地址可以通过在  `s`  中插入  `'.'`  来形成。你 不能 重新排序或删除  `s`  中的任何数字。你可以按 任何 顺序返回答案。
 
示例 1：

```text
输入：s = "25525511135"
输出：["255.255.11.135","255.255.111.35"]
```

示例 2：

```text
输入：s = "0000"
输出：["0.0.0.0"]
```

示例 3：

```text
输入：s = "101023"
输出：["1.0.10.23","1.0.102.3","10.1.0.23","10.10.2.3","101.0.2.3"]
```

 
提示：

 `1 <= s.length <= 20` 
 `s`  仅由数字组成

### Java 解法补充

#### 基础解法：三重循环枚举三个切点

算法思想：三重循环枚举三个切点，检查四段是否合法。


```java
class Solution {
    public java.util.List<String> restoreIpAddresses(String s) {
        java.util.List<String> ans = new java.util.ArrayList<>();
        int n = s.length();
        for (int i = 1; i <= 3 && i < n; i++) {
            for (int j = i + 1; j <= i + 3 && j < n; j++) {
                for (int k = j + 1; k <= j + 3 && k < n; k++) {
                    String a = s.substring(0, i);
                    String b = s.substring(i, j);
                    String c = s.substring(j, k);
                    String d = s.substring(k);
                    if (valid(a) && valid(b) && valid(c) && valid(d)) {
                        ans.add(a + "." + b + "." + c + "." + d);
                    }
                }
            }
        }
        return ans;
    }

    private boolean valid(String seg) {
        if (seg.length() == 0 || seg.length() > 3) return false;
        if (seg.length() > 1 && seg.charAt(0) == '0') return false;
        int val = Integer.parseInt(seg);
        return val >= 0 && val <= 255;
    }
}
```

#### 资深解法：回溯选择 1 到 3 位作为下一段

算法思想：回溯选择 1 到 3 位作为下一段，剪枝非法前导 0 和大于 255。


```java
class Solution {
    public java.util.List<String> restoreIpAddresses(String s) {
        java.util.List<String> ans = new java.util.ArrayList<>();
        dfs(s, 0, 0, new java.util.ArrayList<>(), ans);
        return ans;
    }
    private void dfs(String s, int index, int part, java.util.List<String> path, java.util.List<String> ans) {
        if (part == 4) {
            if (index == s.length()) ans.add(String.join(".", path));
            return;
        }
        for (int len = 1; len <= 3 && index + len <= s.length(); len++) {
            String seg = s.substring(index, index + len);
            if (seg.length() > 1 && seg.charAt(0) == '0') break;
            if (Integer.parseInt(seg) > 255) break;
            path.add(seg);
            dfs(s, index + len, part + 1, path, ans);
            path.remove(path.size() - 1);
        }
    }
}
```


#### 基础语法与算法思想

- `String.join(".", list)` 拼接 IP 段；回溯适合切分字符串。

---

## 94. 二叉树的中序遍历 (Easy)

给定一个二叉树的根节点  `root`  ，返回 它的 中序 遍历 。
 
示例 1：

```text
输入：root = [1,null,2,3]
输出：[1,3,2]
```

示例 2：

```text
输入：root = []
输出：[]
```

示例 3：

```text
输入：root = [1]
输出：[1]
```

 
提示：

树中节点数目在范围  `[0, 100]`  内
 `-100 <= Node.val <= 100` 

 
进阶: 递归算法很简单，你可以通过迭代算法完成吗？

### Java 解法补充

#### 基础解法：递归左、根、右

算法思想：递归左、根、右。


```java
class Solution {
    public java.util.List<Integer> inorderTraversal(TreeNode root) {
        java.util.List<Integer> ans = new java.util.ArrayList<>();
        dfs(root, ans);
        return ans;
    }

    private void dfs(TreeNode node, java.util.List<Integer> ans) {
        if (node == null) return;
        dfs(node.left, ans);
        ans.add(node.val);
        dfs(node.right, ans);
    }
}
```

#### 资深解法：栈模拟递归

算法思想：栈模拟递归，持续压入左链。


```java
class Solution {
    public java.util.List<Integer> inorderTraversal(TreeNode root) {
        java.util.List<Integer> ans = new java.util.ArrayList<>();
        java.util.Deque<TreeNode> stack = new java.util.ArrayDeque<>();
        TreeNode cur = root;
        while (cur != null || !stack.isEmpty()) {
            while (cur != null) {
                stack.push(cur);
                cur = cur.left;
            }
            cur = stack.pop();
            ans.add(cur.val);
            cur = cur.right;
        }
        return ans;
    }
}
```


#### 基础语法与算法思想

- 树遍历递归本质就是系统栈；中序顺序是左子树、当前节点、右子树。

---

## 95. 不同的二叉搜索树 II (Medium)

给你一个整数  `n`  ，请你生成并返回所有由  `n`  个节点组成且节点值从  `1`  到  `n`  互不相同的不同 二叉搜索树 。可以按 任意顺序 返回答案。
 

示例 1：

```text
输入：n = 3
输出：[[1,null,2,null,3],[1,null,3,2],[2,1,3],[3,1,null,null,2],[3,2,null,1]]
```

示例 2：

```text
输入：n = 1
输出：[[1]]
```

 
提示：

 `1 <= n <= 8`

### Java 解法补充

#### 基础解法：枚举根节点

算法思想：枚举根节点，递归生成左右子树后两两组合。


```java
class Solution {
    public java.util.List<TreeNode> generateTrees(int n) {
        java.util.List<Integer> vals = new java.util.ArrayList<>();
        for (int i = 1; i <= n; i++) vals.add(i);
        return build(vals);
    }

    private java.util.List<TreeNode> build(java.util.List<Integer> vals) {
        java.util.List<TreeNode> ans = new java.util.ArrayList<>();
        if (vals.isEmpty()) {
            ans.add(null);
            return ans;
        }
        for (int i = 0; i < vals.size(); i++) {
            int rootVal = vals.get(i);
            java.util.List<Integer> leftVals = new java.util.ArrayList<>(vals.subList(0, i));
            java.util.List<Integer> rightVals = new java.util.ArrayList<>(vals.subList(i + 1, vals.size()));
            for (TreeNode left : build(leftVals)) {
                for (TreeNode right : build(rightVals)) {
                    TreeNode root = new TreeNode(rootVal);
                    root.left = left;
                    root.right = right;
                    ans.add(root);
                }
            }
        }
        return ans;
    }
}
```

#### 资深解法：用区间 `[lo, hi]` 表示可用值范围

算法思想：用区间 `[lo, hi]` 表示可用值范围，空区间返回 `null` 作为可组合子树。


```java
class Solution {
    public java.util.List<TreeNode> generateTrees(int n) {
        return build(1, n);
    }
    private java.util.List<TreeNode> build(int lo, int hi) {
        java.util.List<TreeNode> ans = new java.util.ArrayList<>();
        if (lo > hi) {
            ans.add(null);
            return ans;
        }
        for (int root = lo; root <= hi; root++) {
            for (TreeNode left : build(lo, root - 1)) {
                for (TreeNode right : build(root + 1, hi)) {
                    TreeNode node = new TreeNode(root);
                    node.left = left;
                    node.right = right;
                    ans.add(node);
                }
            }
        }
        return ans;
    }
}
```


#### 基础语法与算法思想

- BST 左小右大，因此选定根后左右值域固定。

---

## 96. 不同的二叉搜索树 (Medium)

给你一个整数  `n`  ，求恰由  `n`  个节点组成且节点值从  `1`  到  `n`  互不相同的 二叉搜索树 有多少种？返回满足题意的二叉搜索树的种数。
 
示例 1：

```text
输入：n = 3
输出：5
```

示例 2：

```text
输入：n = 1
输出：1
```

 
提示：

 `1 <= n <= 19`

### Java 解法补充

#### 基础解法：递归枚举根并计算左右方案乘积

算法思想：递归枚举根并计算左右方案乘积。


```java
class Solution {
    public int numTrees(int n) {
        return count(n);
    }

    private int count(int n) {
        if (n <= 1) return 1;
        int ans = 0;
        for (int root = 1; root <= n; root++) {
            ans += count(root - 1) * count(n - root);
        }
        return ans;
    }
}
```

#### 资深解法：Catalan DP

算法思想：Catalan DP，`dp[n] = sum(dp[left] * dp[right])`。


```java
class Solution {
    public int numTrees(int n) {
        int[] dp = new int[n + 1];
        dp[0] = dp[1] = 1;
        for (int nodes = 2; nodes <= n; nodes++) {
            for (int left = 0; left < nodes; left++) {
                dp[nodes] += dp[left] * dp[nodes - 1 - left];
            }
        }
        return dp[n];
    }
}
```


#### 基础语法与算法思想

- 左右子树方案独立，组合数相乘。

---

## 97. 交错字符串 (Medium)

给定三个字符串  `s1` 、 `s2` 、 `s3` ，请你帮忙验证  `s3`  是否是由  `s1`  和  `s2`  交错 组成的。
两个字符串  `s`  和  `t`  交错 的定义与过程如下，其中每个字符串都会被分割成若干 非空 子字符串：

 `s = s1 + s2 + ... + sn` 
 `t = t1 + t2 + ... + tm` 
 `|n - m| <= 1` 
交错 是  `s1 + t1 + s2 + t2 + s3 + t3 + ...`  或者  `t1 + s1 + t2 + s2 + t3 + s3 + ...` 

注意： `a + b`  意味着字符串  `a`  和  `b`  连接。
 
示例 1：

```text
输入：s1 = "aabcc", s2 = "dbbca", s3 = "aadbbcbcac"
输出：true
```

示例 2：

```text
输入：s1 = "aabcc", s2 = "dbbca", s3 = "aadbbbaccc"
输出：false
```

示例 3：

```text
输入：s1 = "", s2 = "", s3 = ""
输出：true
```

 
提示：

 `0 <= s1.length, s2.length <= 100` 
 `0 <= s3.length <= 200` 
 `s1` 、 `s2` 、和  `s3`  都由小写英文字母组成

 
进阶：您能否仅使用  `O(s2.length)`  额外的内存空间来解决它?

### Java 解法补充

#### 基础解法：递归选择从 `s1` 或 `s2` 消耗一个字符

算法思想：递归选择从 `s1` 或 `s2` 消耗一个字符，并记忆化。


```java
class Solution {
    public boolean isInterleave(String s1, String s2, String s3) {
        if (s1.length() + s2.length() != s3.length()) return false;
        return dfs(s1, s2, s3, 0, 0, 0);
    }

    private boolean dfs(String s1, String s2, String s3, int i, int j, int k) {
        if (k == s3.length()) return i == s1.length() && j == s2.length();
        boolean ok = false;
        if (i < s1.length() && s1.charAt(i) == s3.charAt(k)) ok = dfs(s1, s2, s3, i + 1, j, k + 1);
        if (!ok && j < s2.length() && s2.charAt(j) == s3.charAt(k)) ok = dfs(s1, s2, s3, i, j + 1, k + 1);
        return ok;
    }
}
```

#### 资深解法：二维 DP

算法思想：二维 DP，`dp[i][j]` 表示 `s1` 前 `i` 个和 `s2` 前 `j` 个能否组成 `s3` 前 `i+j` 个。


```java
class Solution {
    public boolean isInterleave(String s1, String s2, String s3) {
        int m = s1.length(), n = s2.length();
        if (m + n != s3.length()) return false;
        boolean[][] dp = new boolean[m + 1][n + 1];
        dp[0][0] = true;
        for (int i = 0; i <= m; i++) {
            for (int j = 0; j <= n; j++) {
                int k = i + j - 1;
                if (i > 0) dp[i][j] |= dp[i - 1][j] && s1.charAt(i - 1) == s3.charAt(k);
                if (j > 0) dp[i][j] |= dp[i][j - 1] && s2.charAt(j - 1) == s3.charAt(k);
            }
        }
        return dp[m][n];
    }
}
```


#### 基础语法与算法思想

- `|=` 可把新条件并入布尔状态；双字符串交错常用二维 DP。

---

## 98. 验证二叉搜索树 (Medium)

给你一个二叉树的根节点  `root`  ，判断其是否是一个有效的二叉搜索树。
有效 二叉搜索树定义如下：

节点的左子树只包含 严格小于 当前节点的数。
节点的右子树只包含 严格大于 当前节点的数。
所有左子树和右子树自身必须也是二叉搜索树。

 
示例 1：

```text
输入：root = [2,1,3]
输出：true
```

示例 2：

```text
输入：root = [5,1,4,null,null,3,6]
输出：false
解释：根节点的值是 5 ，但是右子节点的值是 4 。
```

 
提示：

树中节点数目范围在 `[1, 104]`  内
 `-231 <= Node.val <= 231 - 1`

### Java 解法补充

#### 基础解法：中序遍历成数组后检查是否严格递增

算法思想：中序遍历成数组后检查是否严格递增。


```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        java.util.List<Integer> vals = new java.util.ArrayList<>();
        inorder(root, vals);
        for (int i = 1; i < vals.size(); i++) {
            if (vals.get(i) <= vals.get(i - 1)) return false;
        }
        return true;
    }

    private void inorder(TreeNode node, java.util.List<Integer> vals) {
        if (node == null) return;
        inorder(node.left, vals);
        vals.add(node.val);
        inorder(node.right, vals);
    }
}
```

#### 资深解法：递归传上下界

算法思想：递归传上下界，节点值必须在 `(low, high)` 内。


```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        return valid(root, null, null);
    }
    private boolean valid(TreeNode node, Long low, Long high) {
        if (node == null) return true;
        long v = node.val;
        if (low != null && v <= low) return false;
        if (high != null && v >= high) return false;
        return valid(node.left, low, v) && valid(node.right, v, high);
    }
}
```


#### 基础语法与算法思想

- 用 `Long` 上下界避免 `int` 边界溢出；BST 是全局约束，不只是父子关系。

---

## 99. 恢复二叉搜索树 (Medium)

给你二叉搜索树的根节点  `root`  ，该树中的 恰好 两个节点的值被错误地交换。请在不改变其结构的情况下，恢复这棵树 。
 
示例 1：

```text
输入：root = [1,3,null,null,2]
输出：[3,1,null,null,2]
解释：3 不能是 1 的左孩子，因为 3 > 1 。交换 1 和 3 使二叉搜索树有效。
```

示例 2：

```text
输入：root = [3,1,4,null,null,2]
输出：[2,1,4,null,null,3]
解释：2 不能在 3 的右子树中，因为 2 < 3 。交换 2 和 3 使二叉搜索树有效。
```

 
提示：

树上节点的数目在范围  `[2, 1000]`  内
 `-231 <= Node.val <= 231 - 1` 

 
进阶：使用  `O(n)`  空间复杂度的解法很容易实现。你能想出一个只使用  `O(1)`  空间的解决方案吗？

### Java 解法补充

#### 基础解法：中序取出节点和值

算法思想：中序取出节点和值，排序值后写回。


```java
class Solution {
    public void recoverTree(TreeNode root) {
        java.util.List<TreeNode> nodes = new java.util.ArrayList<>();
        java.util.List<Integer> vals = new java.util.ArrayList<>();
        inorder(root, nodes, vals);
        java.util.Collections.sort(vals);
        for (int i = 0; i < nodes.size(); i++) nodes.get(i).val = vals.get(i);
    }

    private void inorder(TreeNode node, java.util.List<TreeNode> nodes, java.util.List<Integer> vals) {
        if (node == null) return;
        inorder(node.left, nodes, vals);
        nodes.add(node);
        vals.add(node.val);
        inorder(node.right, nodes, vals);
    }
}
```

#### 资深解法：中序遍历中找到两个逆序位置对应的错误节点并交换值

算法思想：中序遍历中找到两个逆序位置对应的错误节点并交换值。


```java
class Solution {
    private TreeNode first, second, prev;
    public void recoverTree(TreeNode root) {
        inorder(root);
        int temp = first.val;
        first.val = second.val;
        second.val = temp;
    }
    private void inorder(TreeNode node) {
        if (node == null) return;
        inorder(node.left);
        if (prev != null && prev.val > node.val) {
            if (first == null) first = prev;
            second = node;
        }
        prev = node;
        inorder(node.right);
    }
}
```


#### 基础语法与算法思想

- BST 中序应递增；两个节点交换会造成一次或两次逆序。

---

## 100. 相同的树 (Easy)

给你两棵二叉树的根节点  `p`  和  `q`  ，编写一个函数来检验这两棵树是否相同。
如果两个树在结构上相同，并且节点具有相同的值，则认为它们是相同的。
 
示例 1：

```text
输入：p = [1,2,3], q = [1,2,3]
输出：true
```

示例 2：

```text
输入：p = [1,2], q = [1,null,2]
输出：false
```

示例 3：

```text
输入：p = [1,2,1], q = [1,1,2]
输出：false
```

 
提示：

两棵树上的节点数目都在范围  `[0, 100]`  内
 `-104 <= Node.val <= 104`

### Java 解法补充

#### 基础解法：层序遍历同时比较节点结构和值

算法思想：层序遍历同时比较节点结构和值。


```java
class Solution {
    public boolean isSameTree(TreeNode p, TreeNode q) {
        java.util.ArrayDeque<TreeNode> qp = new java.util.ArrayDeque<>();
        java.util.ArrayDeque<TreeNode> qq = new java.util.ArrayDeque<>();
        qp.offer(p);
        qq.offer(q);
        while (!qp.isEmpty() && !qq.isEmpty()) {
            TreeNode a = qp.poll();
            TreeNode b = qq.poll();
            if (a == null || b == null) {
                if (a != b) return false;
                continue;
            }
            if (a.val != b.val) return false;
            qp.offer(a.left);
            qp.offer(a.right);
            qq.offer(b.left);
            qq.offer(b.right);
        }
        return qp.isEmpty() && qq.isEmpty();
    }
}
```

#### 资深解法：递归比较当前节点、左子树、右子树

算法思想：递归比较当前节点、左子树、右子树。


```java
class Solution {
    public boolean isSameTree(TreeNode p, TreeNode q) {
        if (p == null || q == null) return p == q;
        return p.val == q.val && isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
    }
}
```


#### 基础语法与算法思想

- `p == q` 在两者同为 `null` 时为 `true`；树结构题先处理空节点。

---
