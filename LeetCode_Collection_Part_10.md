## 271. 字符串的编码与解码 (Medium)

设计一组算法，将字符串列表编码成一个字符串，并能将该字符串解码回原列表。字符串可能包含任意字符，因此编码格式必须能无歧义还原。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

### Java 解法补充

#### 基础解法：用特殊分隔符拼接字符串

算法思想：用特殊分隔符拼接字符串，但当原字符串包含分隔符时会歧义。

```java
public class Codec {
    public String encode(java.util.List<String> strs) {
        return String.join("#", strs);
    }

    public java.util.List<String> decode(String s) {
        if (s.isEmpty()) {
            return new java.util.ArrayList<>();
        }
        return new java.util.ArrayList<>(java.util.Arrays.asList(s.split("#", -1)));
    }
}
```



#### 资深解法：长度前缀编码：`长度#内容`

算法思想：长度前缀编码：`长度#内容`，解码时先读长度再截取固定字符数。


```java
public class Codec {
    public String encode(List<String> strs) {
        StringBuilder sb = new StringBuilder();
        for (String s : strs) sb.append(s.length()).append('#').append(s);
        return sb.toString();
    }

    public List<String> decode(String s) {
        List<String> ans = new ArrayList<>();
        int i = 0;
        while (i < s.length()) {
            int j = i;
            while (s.charAt(j) != '#') j++;
            int len = Integer.parseInt(s.substring(i, j));
            ans.add(s.substring(j + 1, j + 1 + len));
            i = j + 1 + len;
        }
        return ans;
    }
}
```


#### 基础语法与算法思想

- 自描述格式比转义分隔符更稳；`substring` 按长度截取可保留任意字符。

---

## 272. 最接近的二叉搜索树值 II (Hard)

给定二叉搜索树根节点、目标值 `target` 和整数 `k`，返回树中最接近 `target` 的 `k` 个节点值。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

### Java 解法补充

#### 基础解法：中序遍历得到有序数组

算法思想：中序遍历得到有序数组，按与 `target` 的距离排序取前 `k` 个。

```java
class Solution {
    public java.util.List<Integer> closestKValues(TreeNode root, double target, int k) {
        java.util.List<Integer> values = new java.util.ArrayList<>();
        inorder(root, values);
        values.sort((a, b) -> Double.compare(Math.abs(a - target), Math.abs(b - target)));
        return new java.util.ArrayList<>(values.subList(0, k));
    }

    private void inorder(TreeNode node, java.util.List<Integer> values) {
        if (node == null) {
            return;
        }
        inorder(node.left, values);
        values.add(node.val);
        inorder(node.right, values);
    }
}
```



#### 资深解法：维护大小为 `k` 的队列

算法思想：维护大小为 `k` 的队列，中序有序遍历中若新值更接近就弹出最远旧值。


```java
class Solution {
    public List<Integer> closestKValues(TreeNode root, double target, int k) {
        LinkedList<Integer> ans = new LinkedList<>();
        inorder(root, target, k, ans);
        return ans;
    }

    private void inorder(TreeNode node, double target, int k, LinkedList<Integer> ans) {
        if (node == null) return;
        inorder(node.left, target, k, ans);
        if (ans.size() < k) ans.add(node.val);
        else if (Math.abs(ans.peekFirst() - target) > Math.abs(node.val - target)) {
            ans.pollFirst();
            ans.add(node.val);
        }
        inorder(node.right, target, k, ans);
    }
}
```


#### 基础语法与算法思想

- BST 中序递增；固定大小队列保存当前最接近的 `k` 个值。

---

## 273. 整数转换英文表示 (Hard)

将非负整数  `num`  转换为其对应的英文表示。

 **示例 1：**

```text
输入：num = 123
输出："One Hundred Twenty Three"
```

 **示例 2：**

```text
输入：num = 12345
输出："Twelve Thousand Three Hundred Forty Five"
```

 **示例 3：**

```text
输入：num = 1234567
输出："One Million Two Hundred Thirty Four Thousand Five Hundred Sixty Seven"
```


 **提示：**

 `0 <= num <= 231 - 1`

### Java 解法补充

#### 基础解法：按十亿、百万、千分块

算法思想：按十亿、百万、千分块，每个三位数转英文。

```java
class Solution {
    private final String[] below20 = {"", "One", "Two", "Three", "Four", "Five", "Six",
            "Seven", "Eight", "Nine", "Ten", "Eleven", "Twelve", "Thirteen",
            "Fourteen", "Fifteen", "Sixteen", "Seventeen", "Eighteen", "Nineteen"};
    private final String[] tens = {"", "", "Twenty", "Thirty", "Forty", "Fifty",
            "Sixty", "Seventy", "Eighty", "Ninety"};

    public String numberToWords(int num) {
        if (num == 0) {
            return "Zero";
        }
        int[] values = {1000000000, 1000000, 1000, 1};
        String[] names = {"Billion", "Million", "Thousand", ""};
        StringBuilder ans = new StringBuilder();

        for (int i = 0; i < values.length; i++) {
            int part = num / values[i];
            if (part > 0) {
                ans.append(convertLessThanThousand(part)).append(names[i]).append(" ");
                num %= values[i];
            }
        }
        return ans.toString().trim();
    }

    private String convertLessThanThousand(int num) {
        StringBuilder part = new StringBuilder();
        if (num >= 100) {
            part.append(below20[num / 100]).append(" Hundred ");
            num %= 100;
        }
        if (num >= 20) {
            part.append(tens[num / 10]).append(" ");
            num %= 10;
        }
        if (num > 0) {
            part.append(below20[num]).append(" ");
        }
        return part.toString();
    }
}
```



#### 资深解法：递归处理 `<20`、`<100`、`<1000` 和大单位

算法思想：递归处理 `<20`、`<100`、`<1000` 和大单位。


```java
class Solution {
    private final String[] below20 = {"", "One", "Two", "Three", "Four", "Five", "Six", "Seven", "Eight", "Nine", "Ten", "Eleven", "Twelve", "Thirteen", "Fourteen", "Fifteen", "Sixteen", "Seventeen", "Eighteen", "Nineteen"};
    private final String[] tens = {"", "", "Twenty", "Thirty", "Forty", "Fifty", "Sixty", "Seventy", "Eighty", "Ninety"};

    public String numberToWords(int num) {
        if (num == 0) return "Zero";
        return helper(num).trim();
    }

    private String helper(int num) {
        if (num == 0) return "";
        if (num < 20) return below20[num] + " ";
        if (num < 100) return tens[num / 10] + " " + helper(num % 10);
        if (num < 1000) return below20[num / 100] + " Hundred " + helper(num % 100);
        if (num < 1_000_000) return helper(num / 1000) + "Thousand " + helper(num % 1000);
        if (num < 1_000_000_000) return helper(num / 1_000_000) + "Million " + helper(num % 1_000_000);
        return helper(num / 1_000_000_000) + "Billion " + helper(num % 1_000_000_000);
    }
}
```


#### 基础语法与算法思想

- 英文数字按三位分组；递归返回尾部带空格，最终 `trim()` 收尾。

---

## 274. H 指数 (Medium)

给你一个整数数组  `citations`  ，其中  `citations[i]`  表示研究者的第  `i`  篇论文被引用的次数。计算并返回该研究者的  **`h`  指数** 。
根据维基百科上 h 指数的定义： `h`  代表“高引用次数” ，一名科研人员的  `h`  **指数** 是指他（她）至少发表了  `h`  篇论文，并且  **至少** 有  `h`  篇论文被引用次数大于等于  `h`  。如果  `h`  有多种可能的值， **`h`  指数** 是其中最大的那个。

 **示例 1：**

```text
输入：citations = [3,0,6,1,5]
输出：3
解释：给定数组表示研究者总共有 5 篇论文，每篇论文相应的被引用了 3, 0, 6, 1, 5 次。
     由于研究者有 3 篇论文每篇 至少 被引用了 3 次，其余两篇论文每篇被引用 不多于 3 次，所以她的 h 指数是 3。
```

 **示例 2：**

```text
输入：citations = [1,3,1]
输出：1
```


 **提示：**

 `n == citations.length`
 `1 <= n <= 5000`
 `0 <= citations[i] <= 1000`

### Java 解法补充

#### 基础解法：排序后找到最大的 `h`

算法思想：排序后找到最大的 `h`，使至少 `h` 篇论文引用数不小于 `h`。

```java
class Solution {
    public int hIndex(int[] citations) {
        java.util.Arrays.sort(citations);
        int n = citations.length;
        int ans = 0;
        for (int i = 0; i < n; i++) {
            int papers = n - i;
            if (citations[i] >= papers) {
                ans = Math.max(ans, papers);
            }
        }
        return ans;
    }
}
```



#### 资深解法：计数桶统计引用数

算法思想：计数桶统计引用数，超过 `n` 的都放入 `n` 桶，从高到低累计。


```java
class Solution {
    public int hIndex(int[] citations) {
        int n = citations.length;
        int[] count = new int[n + 1];
        for (int c : citations) count[Math.min(c, n)]++;
        int papers = 0;
        for (int h = n; h >= 0; h--) {
            papers += count[h];
            if (papers >= h) return h;
        }
        return 0;
    }
}
```


#### 基础语法与算法思想

- H 指数只关心引用数是否达到 `h`，超过论文总数可合并统计。

---

## 275. H 指数 II (Medium)

给你一个整数数组  `citations`  ，其中  `citations[i]`  表示研究者的第  `i`  篇论文被引用的次数， `citations`  已经按照  **非降序排列** 。计算并返回该研究者的 h **** 指数。
h 指数的定义：h 代表“高引用次数”（high citations），一名科研人员的  `h`  指数是指他（她）的 （ `n`  篇论文中） **至少** 有  `h`  篇论文分别被引用了 **至少**   `h`  次。
请你设计并实现对数时间复杂度的算法解决此问题。

 **示例 1：**

```text
输入：citations = [0,1,3,5,6]
输出：3
解释：给定数组表示研究者总共有 5 篇论文，每篇论文相应的被引用了 0, 1, 3, 5, 6 次。
     由于研究者有3篇论文每篇 至少 被引用了 3 次，其余两篇论文每篇被引用 不多于 3 次，所以她的 h 指数是 3 。
```

 **示例 2：**

```text
输入：citations = [1,2,100]
输出：2
```


 **提示：**

 `n == citations.length`
 `1 <= n <= 105`
 `0 <= citations[i] <= 1000`
 `citations`  按  **升序排列**

### Java 解法补充

#### 基础解法：直接按 274 扫描有序数组

算法思想：直接按 274 扫描有序数组，时间 `O(n)`。

```java
class Solution {
    public int hIndex(int[] citations) {
        int n = citations.length;
        for (int i = 0; i < n; i++) {
            if (citations[i] >= n - i) {
                return n - i;
            }
        }
        return 0;
    }
}
```



#### 资深解法：引用数组升序

算法思想：引用数组升序，二分找第一个 `citations[i] >= n - i` 的位置。


```java
class Solution {
    public int hIndex(int[] citations) {
        int n = citations.length, l = 0, r = n;
        while (l < r) {
            int m = l + (r - l) / 2;
            if (citations[m] >= n - m) r = m;
            else l = m + 1;
        }
        return n - l;
    }
}
```


#### 基础语法与算法思想

- 位置 `i` 右侧有 `n-i` 篇论文；二分单调条件。

---

## 276. 栅栏涂色 (Medium)

有 `n` 个栅栏柱和 `k` 种颜色，要求不能有超过两个相邻柱子颜色相同。返回所有合法涂色方案数。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

### Java 解法补充

#### 基础解法：DP 记录最后两个柱子同色或不同色的方案数

算法思想：DP 记录最后两个柱子同色或不同色的方案数。

```java
class Solution {
    public int numWays(int n, int k) {
        if (n == 0) {
            return 0;
        }
        if (n == 1) {
            return k;
        }

        int[] same = new int[n + 1];
        int[] diff = new int[n + 1];
        same[2] = k;
        diff[2] = k * (k - 1);
        for (int i = 3; i <= n; i++) {
            same[i] = diff[i - 1];
            diff[i] = (same[i - 1] + diff[i - 1]) * (k - 1);
        }
        return same[n] + diff[n];
    }
}
```



#### 资深解法：滚动变量：`same` 表示当前最后两根同色

算法思想：滚动变量：`same` 表示当前最后两根同色，`diff` 表示不同色。


```java
class Solution {
    public int numWays(int n, int k) {
        if (n == 0) return 0;
        if (n == 1) return k;
        int same = k, diff = k * (k - 1);
        for (int i = 3; i <= n; i++) {
            int nsame = diff;
            int ndiff = (same + diff) * (k - 1);
            same = nsame;
            diff = ndiff;
        }
        return same + diff;
    }
}
```


#### 基础语法与算法思想

- 当前同色只能来自上一状态不同色；当前不同色可从任意上一状态换颜色。

---

## 277. 搜寻名人 (Medium)

聚会中有 `n` 个人，名人定义为所有人都认识他，而他不认识任何其他人。只能通过 `knows(a, b)` API 查询，返回名人编号；若不存在则返回 `-1`。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

### Java 解法补充

#### 基础解法：对每个人检查入度为 `n-1`、出度为 0

算法思想：对每个人检查入度为 `n-1`、出度为 0，需要 `O(n^2)` 次查询。

```java
public class Solution extends Relation {
    public int findCelebrity(int n) {
        for (int person = 0; person < n; person++) {
            boolean ok = true;
            for (int other = 0; other < n; other++) {
                if (person == other) {
                    continue;
                }
                if (knows(person, other) || !knows(other, person)) {
                    ok = false;
                    break;
                }
            }
            if (ok) {
                return person;
            }
        }
        return -1;
    }
}
```



#### 资深解法：先用一轮淘汰找候选人

算法思想：先用一轮淘汰找候选人，再验证候选人。


```java
public class Solution extends Relation {
    public int findCelebrity(int n) {
        int cand = 0;
        for (int i = 1; i < n; i++) {
            if (knows(cand, i)) cand = i;
        }
        for (int i = 0; i < n; i++) {
            if (i == cand) continue;
            if (knows(cand, i) || !knows(i, cand)) return -1;
        }
        return cand;
    }
}
```


#### 基础语法与算法思想

- 若 `cand` 认识 `i`，`cand` 不可能是名人；一轮后只剩一个可能候选。

---

## 278. 第一个错误的版本 (Easy)

你是产品经理，目前正在带领一个团队开发新的产品。不幸的是，你的产品的最新版本没有通过质量检测。由于每个版本都是基于之前的版本开发的，所以错误的版本之后的所有版本都是错的。
假设你有  `n`  个版本  `[1, 2, ..., n]` ，你想找出导致之后所有版本出错的第一个错误的版本。
你可以通过调用  `bool isBadVersion(version)`  接口来判断版本号  `version`  是否在单元测试中出错。实现一个函数来查找第一个错误的版本。你应该尽量减少对调用 API 的次数。


 **示例 1：**

```text
输入：n = 5, bad = 4
输出：4
解释：
调用 isBadVersion(3) -> false
调用 isBadVersion(5) -> true
调用 isBadVersion(4) -> true
所以，4 是第一个错误的版本。
```

 **示例 2：**

```text
输入：n = 1, bad = 1
输出：1
```


 **提示：**

 `1 <= bad <= n <= 231 - 1`

### Java 解法补充

#### 基础解法：从版本 1 开始逐个调用 `isBadVersion`

算法思想：从版本 1 开始逐个调用 `isBadVersion`。

```java
public class Solution extends VersionControl {
    public int firstBadVersion(int n) {
        for (int version = 1; version <= n; version++) {
            if (isBadVersion(version)) {
                return version;
            }
        }
        return -1;
    }
}
```



#### 资深解法：二分查找第一个满足 `isBadVersion(version)` 的版本

算法思想：二分查找第一个满足 `isBadVersion(version)` 的版本。


```java
public class Solution extends VersionControl {
    public int firstBadVersion(int n) {
        int l = 1, r = n;
        while (l < r) {
            int m = l + (r - l) / 2;
            if (isBadVersion(m)) r = m;
            else l = m + 1;
        }
        return l;
    }
}
```


#### 基础语法与算法思想

- “第一个 true” 是二分边界题；中点计算要防溢出。

---

## 279. 完全平方数 (Medium)

给你一个整数  `n`  ，返回 和为  `n`  的完全平方数的最少数量 。
 **完全平方数**  是一个整数，其值等于另一个整数的平方；换句话说，其值等于一个整数自乘的积。例如， `1` 、 `4` 、 `9`  和  `16`  都是完全平方数，而  `3`  和  `11`  不是。

 **示例 1：**

```text
输入：n = 12
输出：3
解释：12 = 4 + 4 + 4
```

 **示例 2：**

```text
输入：n = 13
输出：2
解释：13 = 4 + 9
```



 **提示：**

 `1 <= n <= 104`

### Java 解法补充

#### 基础解法：`dp[i]` 表示组成 `i` 的最少完全平方数个数

算法思想：`dp[i]` 表示组成 `i` 的最少完全平方数个数，枚举所有平方数转移。

```java
class Solution {
    public int numSquares(int n) {
        int[] dp = new int[n + 1];
        java.util.Arrays.fill(dp, n + 1);
        dp[0] = 0;

        for (int i = 1; i <= n; i++) {
            for (int square = 1; square * square <= i; square++) {
                dp[i] = Math.min(dp[i], dp[i - square * square] + 1);
            }
        }
        return dp[n];
    }
}
```



#### 资深解法：可用数学四平方定理优化；DP 更通用

算法思想：可用数学四平方定理优化；DP 更通用。


```java
class Solution {
    public int numSquares(int n) {
        int[] dp = new int[n + 1];
        Arrays.fill(dp, Integer.MAX_VALUE / 2);
        dp[0] = 0;
        for (int i = 1; i * i <= n; i++) {
            int sq = i * i;
            for (int x = sq; x <= n; x++) {
                dp[x] = Math.min(dp[x], dp[x - sq] + 1);
            }
        }
        return dp[n];
    }
}
```


#### 基础语法与算法思想

- 完全背包：每个平方数可以重复使用。

---

## 280. 摆动排序 (Medium)

给定整数数组 `nums`，原地重新排列，使其满足 `nums[0] <= nums[1] >= nums[2] <= nums[3]...` 的摆动顺序。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

### Java 解法补充

#### 基础解法：排序后交错放置小值和大值

算法思想：排序后交错放置小值和大值。

```java
class Solution {
    public void wiggleSort(int[] nums) {
        int[] sorted = nums.clone();
        java.util.Arrays.sort(sorted);
        int left = 0;
        int right = nums.length - 1;

        for (int i = 0; i < nums.length; i++) {
            if (i % 2 == 0) {
                nums[i] = sorted[left++];
            } else {
                nums[i] = sorted[right--];
            }
        }
    }
}
```



#### 资深解法：一次扫描

算法思想：一次扫描，偶数位应 `<=` 后一位，奇数位应 `>=` 后一位，不满足就交换。


```java
class Solution {
    public void wiggleSort(int[] nums) {
        for (int i = 0; i + 1 < nums.length; i++) {
            if ((i % 2 == 0 && nums[i] > nums[i + 1]) ||
                (i % 2 == 1 && nums[i] < nums[i + 1])) {
                int t = nums[i];
                nums[i] = nums[i + 1];
                nums[i + 1] = t;
            }
        }
    }
}
```


#### 基础语法与算法思想

- 局部交换不会破坏前一个位置已满足的摆动关系。

---

## 281. 锯齿迭代器 (Medium)

给定两个一维向量，设计迭代器交替返回两个向量中的元素；当某个向量耗尽时，继续返回另一个向量剩余元素。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

### Java 解法补充

#### 基础解法：构造时把两个向量交替展开到队列

算法思想：构造时把两个向量交替展开到队列。

```java
public class ZigzagIterator {
    private java.util.Queue<Integer> queue = new java.util.ArrayDeque<>();

    public ZigzagIterator(java.util.List<Integer> v1, java.util.List<Integer> v2) {
        int i = 0;
        while (i < v1.size() || i < v2.size()) {
            if (i < v1.size()) {
                queue.offer(v1.get(i));
            }
            if (i < v2.size()) {
                queue.offer(v2.get(i));
            }
            i++;
        }
    }

    public int next() {
        return queue.poll();
    }

    public boolean hasNext() {
        return !queue.isEmpty();
    }
}
```



#### 资深解法：队列保存非空迭代器

算法思想：队列保存非空迭代器，每取一个元素后若仍有剩余则放回队尾。


```java
public class ZigzagIterator {
    private Queue<Iterator<Integer>> queue = new ArrayDeque<>();

    public ZigzagIterator(List<Integer> v1, List<Integer> v2) {
        if (!v1.isEmpty()) queue.offer(v1.iterator());
        if (!v2.isEmpty()) queue.offer(v2.iterator());
    }

    public int next() {
        Iterator<Integer> it = queue.poll();
        int val = it.next();
        if (it.hasNext()) queue.offer(it);
        return val;
    }

    public boolean hasNext() {
        return !queue.isEmpty();
    }
}
```


#### 基础语法与算法思想

- 队列轮转实现多路交替；扩展到多个向量也自然成立。

---

## 282. 给表达式添加运算符 (Hard)

给定一个仅包含数字  `0-9`  的字符串  `num`  和一个目标值整数  `target`  ，在  `num`  的数字之间添加  **二元** 运算符（不是一元） `+` 、 `-`  或  `*`  ，返回  **所有**  能够得到  `target`  的表达式。
注意，返回表达式中的操作数  **不应该**  包含前导零。
 **注意** ，一个数字可以包含多个数位。

 **示例 1:**

```text
输入: num = "123", target = 6
输出: ["1+2+3", "1*2*3"]
解释: “1*2*3” 和 “1+2+3” 的值都是6。
```

 **示例 2:**

```text
输入: num = "232", target = 8
输出: ["2*3+2", "2+3*2"]
解释: “2*3+2” 和 “2+3*2” 的值都是8。
```

 **示例 3:**

```text
输入: num = "3456237490", target = 9191
输出: []
解释: 表达式 “3456237490” 无法得到 9191 。
```


 **提示：**

 `1 <= num.length <= 10`
 `num`  仅含数字
 `-231 <= target <= 231 - 1`

### Java 解法补充

#### 基础解法：回溯枚举每个数字切分和每个运算符

算法思想：回溯枚举每个数字切分和每个运算符，最终计算表达式值。

```java
class Solution {
    public java.util.List<String> addOperators(String num, int target) {
        java.util.List<String> ans = new java.util.ArrayList<>();
        dfs(num, target, 0, new StringBuilder(), ans);
        return ans;
    }

    private void dfs(String num, int target, int index, StringBuilder expr,
            java.util.List<String> ans) {
        if (index == num.length()) {
            if (eval(expr.toString()) == target) {
                ans.add(expr.toString());
            }
            return;
        }

        int len = expr.length();
        for (int end = index; end < num.length(); end++) {
            if (end > index && num.charAt(index) == '0') {
                break;
            }
            String part = num.substring(index, end + 1);
            if (index == 0) {
                expr.append(part);
                dfs(num, target, end + 1, expr, ans);
                expr.setLength(len);
            } else {
                for (char op : new char[]{'+', '-', '*'}) {
                    expr.append(op).append(part);
                    dfs(num, target, end + 1, expr, ans);
                    expr.setLength(len);
                }
            }
        }
    }

    private long eval(String expr) {
        java.util.List<Long> nums = new java.util.ArrayList<>();
        java.util.List<Character> ops = new java.util.ArrayList<>();
        int i = 0;
        while (i < expr.length()) {
            if (expr.charAt(i) == '+' || expr.charAt(i) == '-' || expr.charAt(i) == '*') {
                ops.add(expr.charAt(i++));
            } else {
                long value = 0;
                while (i < expr.length() && Character.isDigit(expr.charAt(i))) {
                    value = value * 10 + expr.charAt(i++) - '0';
                }
                nums.add(value);
            }
        }

        for (int p = 0; p < ops.size(); p++) {
            if (ops.get(p) == '*') {
                nums.set(p, nums.get(p) * nums.remove(p + 1));
                ops.remove(p--);
            }
        }
        long ans = nums.get(0);
        for (int p = 0; p < ops.size(); p++) {
            ans = ops.get(p) == '+' ? ans + nums.get(p + 1) : ans - nums.get(p + 1);
        }
        return ans;
    }
}
```



#### 资深解法：回溯时携带当前值和上一项 `last`

算法思想：回溯时携带当前值和上一项 `last`，乘法通过回滚上一项处理优先级。


```java
class Solution {
    public List<String> addOperators(String num, int target) {
        List<String> ans = new ArrayList<>();
        dfs(num, target, 0, 0, 0, new StringBuilder(), ans);
        return ans;
    }

    private void dfs(String num, int target, int pos, long value, long last, StringBuilder expr, List<String> ans) {
        if (pos == num.length()) {
            if (value == target) ans.add(expr.toString());
            return;
        }
        int len = expr.length();
        for (int i = pos; i < num.length(); i++) {
            if (i > pos && num.charAt(pos) == '0') break;
            long cur = Long.parseLong(num.substring(pos, i + 1));
            if (pos == 0) {
                expr.append(cur);
                dfs(num, target, i + 1, cur, cur, expr, ans);
                expr.setLength(len);
            } else {
                expr.append('+').append(cur);
                dfs(num, target, i + 1, value + cur, cur, expr, ans);
                expr.setLength(len);
                expr.append('-').append(cur);
                dfs(num, target, i + 1, value - cur, -cur, expr, ans);
                expr.setLength(len);
                expr.append('*').append(cur);
                dfs(num, target, i + 1, value - last + last * cur, last * cur, expr, ans);
                expr.setLength(len);
            }
        }
    }
}
```


#### 基础语法与算法思想

- `last` 保存最近一个乘法项；前导零数字只能是单个 `0`。

---

## 283. 移动零 (Easy)

给定一个数组  `nums` ，编写一个函数将所有  `0`  移动到数组的末尾，同时保持非零元素的相对顺序。
 **请注意**  ，必须在不复制数组的情况下原地对数组进行操作。

 **示例 1:**

```text
输入: nums = [0,1,0,3,12]
输出: [1,3,12,0,0]
```

 **示例 2:**

```text
输入: nums = [0]
输出: [0]
```


 **提示** :

 `1 <= nums.length <= 104`
 `-231 <= nums[i] <= 231 - 1`


 **进阶：** 你能尽量减少完成的操作次数吗？

### Java 解法补充

#### 基础解法：额外数组先放非零元素

算法思想：额外数组先放非零元素，再补零。

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int[] copy = new int[nums.length];
        int index = 0;
        for (int num : nums) {
            if (num != 0) {
                copy[index++] = num;
            }
        }
        for (int i = 0; i < nums.length; i++) {
            nums[i] = copy[i];
        }
    }
}
```



#### 资深解法：双指针原地写入非零元素

算法思想：双指针原地写入非零元素，最后补零。


```java
class Solution {
    public void moveZeroes(int[] nums) {
        int write = 0;
        for (int x : nums) if (x != 0) nums[write++] = x;
        while (write < nums.length) nums[write++] = 0;
    }
}
```


#### 基础语法与算法思想

- 稳定保留非零元素相对顺序；写指针表示下一个非零落点。

---

## 284. 窥视迭代器 (Medium)

请你在设计一个迭代器，在集成现有迭代器拥有的  `hasNext`  和  `next`  操作的基础上，还额外支持  `peek`  操作。
实现  `PeekingIterator`  类：

 `PeekingIterator(Iterator<int> nums)`  使用指定整数迭代器  `nums`  初始化迭代器。
 `int next()`  返回数组中的下一个元素，并将指针移动到下个元素处。
 `bool hasNext()`  如果数组中存在下一个元素，返回  `true`  ；否则，返回  `false`  。
 `int peek()`  返回数组中的下一个元素，但  **不**  移动指针。

 **注意：** 每种语言可能有不同的构造函数和迭代器  `Iterator` ，但均支持  `int next()`  和  `boolean hasNext()`  函数。

 **示例 1：**

```text
输入：
["PeekingIterator", "next", "peek", "next", "next", "hasNext"]
[[[1, 2, 3]], [], [], [], [], []]
输出：
[null, 1, 2, 2, 3, false]

解释：
PeekingIterator peekingIterator = new PeekingIterator([1, 2, 3]); // [1,2,3]
peekingIterator.next();    // 返回 1 ，指针移动到下一个元素 [1,2,3]
peekingIterator.peek();    // 返回 2 ，指针未发生移动 [1,2,3]
peekingIterator.next();    // 返回 2 ，指针移动到下一个元素 [1,2,3]
peekingIterator.next();    // 返回 3 ，指针移动到下一个元素 [1,2,3]
peekingIterator.hasNext(); // 返回 False
```


 **提示：**

 `1 <= nums.length <= 1000`
 `1 <= nums[i] <= 1000`
对  `next`  和  `peek`  的调用均有效
 `next` 、 `hasNext`  和  `peek` 最多调用   `1000`  次


 **进阶：** 你将如何拓展你的设计？使之变得通用化，从而适应所有的类型，而不只是整数型？

### Java 解法补充

#### 基础解法：每次 `peek` 都复制迭代器不可行

算法思想：每次 `peek` 都复制迭代器不可行。

```java
class PeekingIterator implements java.util.Iterator<Integer> {
    private java.util.List<Integer> data = new java.util.ArrayList<>();
    private int index = 0;

    public PeekingIterator(java.util.Iterator<Integer> iterator) {
        while (iterator.hasNext()) {
            data.add(iterator.next());
        }
    }

    public Integer peek() {
        return data.get(index);
    }

    @Override
    public Integer next() {
        return data.get(index++);
    }

    @Override
    public boolean hasNext() {
        return index < data.size();
    }
}
```



#### 资深解法：缓存下一个元素

算法思想：缓存下一个元素，`peek` 返回缓存，`next` 消费缓存并预取。


```java
class PeekingIterator implements Iterator<Integer> {
    private Iterator<Integer> iterator;
    private Integer next;

    public PeekingIterator(Iterator<Integer> iterator) {
        this.iterator = iterator;
        if (iterator.hasNext()) next = iterator.next();
    }

    public Integer peek() {
        return next;
    }

    @Override
    public Integer next() {
        Integer ans = next;
        next = iterator.hasNext() ? iterator.next() : null;
        return ans;
    }

    @Override
    public boolean hasNext() {
        return next != null;
    }
}
```


#### 基础语法与算法思想

- 预读一个元素让 `peek` 不推进底层迭代器。

---

## 285. 二叉搜索树中的中序后继 (Medium)

给定二叉搜索树根节点和节点 `p`，返回 `p` 在中序遍历中的后继节点；若不存在后继，返回 `null`。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

### Java 解法补充

#### 基础解法：中序遍历列表

算法思想：中序遍历列表，找到 `p` 后返回下一个节点。

```java
class Solution {
    public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
        java.util.List<TreeNode> nodes = new java.util.ArrayList<>();
        inorder(root, nodes);
        for (int i = 0; i + 1 < nodes.size(); i++) {
            if (nodes.get(i) == p) {
                return nodes.get(i + 1);
            }
        }
        return null;
    }

    private void inorder(TreeNode node, java.util.List<TreeNode> nodes) {
        if (node == null) {
            return;
        }
        inorder(node.left, nodes);
        nodes.add(node);
        inorder(node.right, nodes);
    }
}
```



#### 资深解法：利用 BST：若当前值大于 `p`

算法思想：利用 BST：若当前值大于 `p`，当前可能是后继并向左找更小候选；否则向右。


```java
class Solution {
    public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
        TreeNode ans = null;
        while (root != null) {
            if (root.val > p.val) {
                ans = root;
                root = root.left;
            } else root = root.right;
        }
        return ans;
    }
}
```


#### 基础语法与算法思想

- 后继是“大于 p 的最小节点”，正好是 BST 搜索过程中的候选。

---

## 286. 墙与门 (Medium)

给定二维网格，`-1` 表示墙，`0` 表示门，`INF` 表示空房间。请把每个空房间填成到最近门的距离，无法到达门的房间保持 `INF`。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

### Java 解法补充

#### 基础解法：从每个空房间 BFS 找最近门

算法思想：从每个空房间 BFS 找最近门，重复遍历严重。

```java
class Solution {
    private final int[][] dirs = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};

    public void wallsAndGates(int[][] rooms) {
        int m = rooms.length;
        int n = rooms[0].length;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (rooms[i][j] == Integer.MAX_VALUE) {
                    rooms[i][j] = nearestGate(rooms, i, j);
                }
            }
        }
    }

    private int nearestGate(int[][] rooms, int startX, int startY) {
        int m = rooms.length;
        int n = rooms[0].length;
        boolean[][] seen = new boolean[m][n];
        java.util.Queue<int[]> queue = new java.util.ArrayDeque<>();
        queue.offer(new int[]{startX, startY, 0});
        seen[startX][startY] = true;

        while (!queue.isEmpty()) {
            int[] cur = queue.poll();
            if (rooms[cur[0]][cur[1]] == 0) {
                return cur[2];
            }
            for (int[] dir : dirs) {
                int x = cur[0] + dir[0];
                int y = cur[1] + dir[1];
                if (x < 0 || x >= m || y < 0 || y >= n || seen[x][y] || rooms[x][y] == -1) {
                    continue;
                }
                seen[x][y] = true;
                queue.offer(new int[]{x, y, cur[2] + 1});
            }
        }
        return Integer.MAX_VALUE;
    }
}
```



#### 资深解法：多源 BFS：所有门同时入队

算法思想：多源 BFS：所有门同时入队，向外扩散距离。


```java
class Solution {
    public void wallsAndGates(int[][] rooms) {
        int m = rooms.length, n = rooms[0].length;
        Queue<int[]> q = new ArrayDeque<>();
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) {
            if (rooms[i][j] == 0) q.offer(new int[]{i, j});
        }
        int[][] dirs = {{1,0},{-1,0},{0,1},{0,-1}};
        while (!q.isEmpty()) {
            int[] cur = q.poll();
            for (int[] d : dirs) {
                int x = cur[0] + d[0], y = cur[1] + d[1];
                if (x < 0 || x == m || y < 0 || y == n || rooms[x][y] != Integer.MAX_VALUE) continue;
                rooms[x][y] = rooms[cur[0]][cur[1]] + 1;
                q.offer(new int[]{x, y});
            }
        }
    }
}
```


#### 基础语法与算法思想

- 多源 BFS 第一次到达某房间就是最近门距离。

---

## 287. 寻找重复数 (Medium)

给定一个包含  `n + 1`  个整数的数组  `nums`  ，其数字都在  `[1, n]`  范围内（包括  `1`  和  `n` ），可知至少存在一个重复的整数。
假设  `nums`  只有  **一个重复的整数**  ，返回  **这个重复的数**  。
你设计的解决方案必须  **不修改**  数组  `nums`  且只用常量级  `O(1)`  的额外空间。

 **示例 1：**

```text
输入：nums = [1,3,4,2,2]
输出：2
```

 **示例 2：**

```text
输入：nums = [3,1,3,4,2]
输出：3
```

 **示例 3 :**

```text
输入：nums = [3,3,3,3,3]
输出：3
```



 **提示：**

 `1 <= n <= 105`
 `nums.length == n + 1`
 `1 <= nums[i] <= n`
 `nums`  中  **只有一个整数**  出现  **两次或多次**  ，其余整数均只出现  **一次**


 **进阶：**

如何证明  `nums`  中至少存在一个重复的数字?
你可以设计一个线性级时间复杂度  `O(n)`  的解决方案吗？

### Java 解法补充

#### 基础解法：排序后找相邻相等值

算法思想：排序后找相邻相等值，或哈希集合查重。

```java
class Solution {
    public int findDuplicate(int[] nums) {
        int[] copy = nums.clone();
        java.util.Arrays.sort(copy);
        for (int i = 1; i < copy.length; i++) {
            if (copy[i] == copy[i - 1]) {
                return copy[i];
            }
        }
        return -1;
    }
}
```



#### 资深解法：把数组值看作链表指针

算法思想：把数组值看作链表指针，重复数是环入口，用 Floyd 判圈。


```java
class Solution {
    public int findDuplicate(int[] nums) {
        int slow = nums[0], fast = nums[nums[0]];
        while (slow != fast) {
            slow = nums[slow];
            fast = nums[nums[fast]];
        }
        slow = 0;
        while (slow != fast) {
            slow = nums[slow];
            fast = nums[fast];
        }
        return slow;
    }
}
```


#### 基础语法与算法思想

- 数值范围 `1..n` 可作为下标跳转；重复值导致多个入口指向同一节点形成环。

---

## 288. 单词的唯一缩写 (Medium)

设计 `ValidWordAbbr`，给定词典后判断某个单词的缩写在词典中是否唯一。缩写规则为首字母 + 中间字符数量 + 尾字母，长度小于等于 2 的单词缩写为自身。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

### Java 解法补充

#### 基础解法：每次查询时扫描词典

算法思想：每次查询时扫描词典，比较同缩写单词是否只有自身。

```java
class ValidWordAbbr {
    private String[] dictionary;

    public ValidWordAbbr(String[] dictionary) {
        this.dictionary = dictionary;
    }

    public boolean isUnique(String word) {
        String target = abbr(word);
        for (String dictWord : dictionary) {
            if (!dictWord.equals(word) && abbr(dictWord).equals(target)) {
                return false;
            }
        }
        return true;
    }

    private String abbr(String word) {
        int n = word.length();
        if (n <= 2) {
            return word;
        }
        return "" + word.charAt(0) + (n - 2) + word.charAt(n - 1);
    }
}
```



#### 资深解法：构造时把缩写映射到唯一单词；若冲突则标记为空

算法思想：构造时把缩写映射到唯一单词；若冲突则标记为空。


```java
class ValidWordAbbr {
    private Map<String, String> map = new HashMap<>();

    public ValidWordAbbr(String[] dictionary) {
        for (String w : new HashSet<>(Arrays.asList(dictionary))) {
            String a = abbr(w);
            map.put(a, map.containsKey(a) ? "" : w);
        }
    }

    public boolean isUnique(String word) {
        String a = abbr(word);
        return !map.containsKey(a) || map.get(a).equals(word);
    }

    private String abbr(String w) {
        int n = w.length();
        return n <= 2 ? w : "" + w.charAt(0) + (n - 2) + w.charAt(n - 1);
    }
}
```


#### 基础语法与算法思想

- 词典去重后再建表；同缩写多个不同单词时查询只能失败。

---

## 289. 生命游戏 (Medium)

根据 百度百科 ，  **生命游戏**  ，简称为  **生命**  ，是英国数学家约翰·何顿·康威在 1970 年发明的细胞自动机。
给定一个包含  `m × n`  个格子的面板，每一个格子都可以看成是一个细胞。每个细胞都具有一个初始状态：  `1`  即为  **活细胞**  （live），或  `0`  即为  **死细胞**  （dead）。每个细胞与其八个相邻位置（水平，垂直，对角线）的细胞都遵循以下四条生存定律：

如果活细胞周围八个位置的活细胞数少于两个，则该位置活细胞死亡；
如果活细胞周围八个位置有两个或三个活细胞，则该位置活细胞仍然存活；
如果活细胞周围八个位置有超过三个活细胞，则该位置活细胞死亡；
如果死细胞周围正好有三个活细胞，则该位置死细胞复活；

下一个状态是通过将上述规则同时应用于当前状态下的每个细胞所形成的，其中细胞的出生和死亡是  **同时**  发生的。给你  `m x n`  网格面板  `board`  的当前状态，返回下一个状态。
给定当前  `board`  的状态， **更新**   `board`  到下一个状态。
 **注意**  你不需要返回任何东西。

 **示例 1：**

```text
输入：board = [[0,1,0],[0,0,1],[1,1,1],[0,0,0]]
输出：[[0,0,0],[1,0,1],[0,1,1],[0,1,0]]
```

 **示例 2：**

```text
输入：board = [[1,1],[1,0]]
输出：[[1,1],[1,1]]
```


 **提示：**

 `m == board.length`
 `n == board[i].length`
 `1 <= m, n <= 25`
 `board[i][j]`  为  `0`  或  `1`


 **进阶：**

你可以使用原地算法解决本题吗？请注意，面板上所有格子需要同时被更新：你不能先更新某些格子，然后使用它们的更新后的值再更新其他格子。
本题中，我们使用二维数组来表示面板。原则上，面板是无限的，但当活细胞侵占了面板边界时会造成问题。你将如何解决这些问题？

### Java 解法补充

#### 基础解法：复制一份原矩阵

算法思想：复制一份原矩阵，根据原状态计算下一代。

```java
class Solution {
    public void gameOfLife(int[][] board) {
        int m = board.length;
        int n = board[0].length;
        int[][] old = new int[m][n];
        for (int i = 0; i < m; i++) {
            old[i] = board[i].clone();
        }

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                int live = countLive(old, i, j);
                if (old[i][j] == 1) {
                    board[i][j] = live == 2 || live == 3 ? 1 : 0;
                } else {
                    board[i][j] = live == 3 ? 1 : 0;
                }
            }
        }
    }

    private int countLive(int[][] board, int row, int col) {
        int count = 0;
        for (int dx = -1; dx <= 1; dx++) {
            for (int dy = -1; dy <= 1; dy++) {
                if (dx == 0 && dy == 0) {
                    continue;
                }
                int x = row + dx;
                int y = col + dy;
                if (x >= 0 && x < board.length && y >= 0 && y < board[0].length) {
                    count += board[x][y];
                }
            }
        }
        return count;
    }
}
```



#### 资深解法：原地用额外状态编码：`2` 表示活变死

算法思想：原地用额外状态编码：`2` 表示活变死，`3` 表示死变活。


```java
class Solution {
    public void gameOfLife(int[][] board) {
        int m = board.length, n = board[0].length;
        int[] dirs = {-1, 0, 1};
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                int live = 0;
                for (int dx : dirs) for (int dy : dirs) {
                    if (dx == 0 && dy == 0) continue;
                    int x = i + dx, y = j + dy;
                    if (x >= 0 && x < m && y >= 0 && y < n && (board[x][y] == 1 || board[x][y] == 2)) live++;
                }
                if (board[i][j] == 1 && (live < 2 || live > 3)) board[i][j] = 2;
                if (board[i][j] == 0 && live == 3) board[i][j] = 3;
            }
        }
        for (int i = 0; i < m; i++) for (int j = 0; j < n; j++) board[i][j] %= 2;
    }
}
```


#### 基础语法与算法思想

- 过渡状态同时保留旧状态和新状态；最后取模落盘。

---

## 290. 单词规律 (Easy)

给定一种规律  `pattern`  和一个字符串  `s`  ，判断  `s`  是否遵循相同的规律。
这里的  **遵循** 指完全匹配，例如，  `pattern`  里的每个字母和字符串  `s`  **** 中的每个非空单词之间存在着双向连接的对应规律。具体来说：

 `pattern`  中的每个字母都  **恰好**  映射到  `s`  中的一个唯一单词。
 `s`  中的每个唯一单词都  **恰好**  映射到  `pattern`  中的一个字母。
没有两个字母映射到同一个单词，也没有两个单词映射到同一个字母。


 **示例1:**

```text
输入: pattern = "abba", s = "dog cat cat dog"
输出: true
```

 **示例 2:**

```text
输入:pattern = "abba", s = "dog cat cat fish"
输出: false
```

 **示例 3:**

```text
输入: pattern = "aaaa", s = "dog cat cat dog"
输出: false
```


 **提示:**

 `1 <= pattern.length <= 300`
 `pattern`  只包含小写英文字母
 `1 <= s.length <= 3000`
 `s`  只包含小写英文字母和  `' '`
 `s`   **不包含**  任何前导或尾随对空格
 `s`  中每个单词都被  **单个空格** 分隔

### Java 解法补充

#### 基础解法：双向哈希表保证模式字符和单词一一对应

算法思想：双向哈希表保证模式字符和单词一一对应。

```java
class Solution {
    public boolean wordPattern(String pattern, String s) {
        String[] words = s.split(" ");
        if (pattern.length() != words.length) {
            return false;
        }

        java.util.Map<Character, String> charToWord = new java.util.HashMap<>();
        java.util.Map<String, Character> wordToChar = new java.util.HashMap<>();
        for (int i = 0; i < pattern.length(); i++) {
            char c = pattern.charAt(i);
            String word = words[i];
            if (charToWord.containsKey(c) && !charToWord.get(c).equals(word)) {
                return false;
            }
            if (wordToChar.containsKey(word) && wordToChar.get(word) != c) {
                return false;
            }
            charToWord.put(c, word);
            wordToChar.put(word, c);
        }
        return true;
    }
}
```



#### 资深解法：记录上次出现位置

算法思想：记录上次出现位置，两个序列同构时当前位置历史必须一致。


```java
class Solution {
    public boolean wordPattern(String pattern, String s) {
        String[] words = s.split(" ");
        if (pattern.length() != words.length) return false;
        Map<Character, Integer> a = new HashMap<>();
        Map<String, Integer> b = new HashMap<>();
        for (int i = 0; i < words.length; i++) {
            int p = a.getOrDefault(pattern.charAt(i), -1);
            int q = b.getOrDefault(words[i], -1);
            if (p != q) return false;
            a.put(pattern.charAt(i), i);
            b.put(words[i], i);
        }
        return true;
    }
}
```


#### 基础语法与算法思想

- 位置签名一致即可表达双射关系。

---

## 291. 单词规律 II (Medium)

给定模式串 `pattern` 和目标串 `s`，判断是否存在从模式字符到非空字符串的双射映射，使得按模式拼接后等于 `s`。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

### Java 解法补充

#### 基础解法：回溯枚举每个模式字符映射到目标串的所有非空前缀

算法思想：回溯枚举每个模式字符映射到目标串的所有非空前缀。

```java
class Solution {
    public boolean wordPatternMatch(String pattern, String s) {
        return dfs(pattern, 0, s, 0, new java.util.HashMap<>(), new java.util.HashSet<>());
    }

    private boolean dfs(String pattern, int pi, String s, int si,
            java.util.Map<Character, String> map, java.util.Set<String> used) {
        if (pi == pattern.length() && si == s.length()) {
            return true;
        }
        if (pi == pattern.length() || si == s.length()) {
            return false;
        }

        char c = pattern.charAt(pi);
        if (map.containsKey(c)) {
            String word = map.get(c);
            return s.startsWith(word, si) && dfs(pattern, pi + 1, s, si + word.length(), map, used);
        }

        for (int end = si + 1; end <= s.length(); end++) {
            String word = s.substring(si, end);
            if (used.contains(word)) {
                continue;
            }
            map.put(c, word);
            used.add(word);
            if (dfs(pattern, pi + 1, s, end, map, used)) {
                return true;
            }
            map.remove(c);
            used.remove(word);
        }
        return false;
    }
}
```



#### 资深解法：用映射表和已占用字符串集合同时维护双射

算法思想：用映射表和已占用字符串集合同时维护双射，并剪枝剩余长度。


```java
class Solution {
    public boolean wordPatternMatch(String pattern, String s) {
        return dfs(pattern, 0, s, 0, new HashMap<>(), new HashSet<>());
    }

    private boolean dfs(String p, int i, String s, int j, Map<Character, String> map, Set<String> used) {
        if (i == p.length() && j == s.length()) return true;
        if (i == p.length() || j == s.length()) return false;
        char c = p.charAt(i);
        if (map.containsKey(c)) {
            String w = map.get(c);
            return s.startsWith(w, j) && dfs(p, i + 1, s, j + w.length(), map, used);
        }
        for (int end = j + 1; end <= s.length(); end++) {
            String w = s.substring(j, end);
            if (used.contains(w)) continue;
            map.put(c, w); used.add(w);
            if (dfs(p, i + 1, s, end, map, used)) return true;
            map.remove(c); used.remove(w);
        }
        return false;
    }
}
```


#### 基础语法与算法思想

- `startsWith(prefix, offset)` 可从指定位置匹配；回溯负责尝试映射长度。

---

## 292. Nim 游戏 (Easy)

你和你的朋友，两个人一起玩 Nim 游戏：

桌子上有一堆石头。
你们轮流进行自己的回合，  **你作为先手** 。
每一回合，轮到的人拿掉 1 - 3 块石头。
拿掉最后一块石头的人就是获胜者。

假设你们每一步都是最优解。请编写一个函数，来判断你是否可以在给定石头数量为  `n`  的情况下赢得游戏。如果可以赢，返回  `true` ；否则，返回  `false`  。

 **示例 1：**

```text
输入：n = 4
输出：false
解释：以下是可能的结果:
1. 移除1颗石头。你的朋友移走了3块石头，包括最后一块。你的朋友赢了。
2. 移除2个石子。你的朋友移走2块石头，包括最后一块。你的朋友赢了。
3.你移走3颗石子。你的朋友移走了最后一块石头。你的朋友赢了。
在所有结果中，你的朋友是赢家。
```

 **示例 2：**

```text
输入：n = 1
输出：true
```

 **示例 3：**

```text
输入：n = 2
输出：true
```


 **提示：**

 `1 <= n <= 231 - 1`

### Java 解法补充

#### 基础解法：DP 判断每个石子数是否为必胜态

算法思想：DP 判断每个石子数是否为必胜态。

```java
class Solution {
    public boolean canWinNim(int n) {
        if (n <= 3) {
            return true;
        }
        boolean[] win = new boolean[n + 1];
        win[1] = win[2] = win[3] = true;
        for (int stones = 4; stones <= n; stones++) {
            win[stones] = !win[stones - 1] || !win[stones - 2] || !win[stones - 3];
        }
        return win[n];
    }
}
```



#### 资深解法：4 的倍数是必败态

算法思想：4 的倍数是必败态，其余都是必胜态。


```java
class Solution {
    public boolean canWinNim(int n) {
        return n % 4 != 0;
    }
}
```


#### 基础语法与算法思想

- 每次可拿 1-3 个，先手只要把对手留在 4 的倍数即可。

---

## 293. 翻转游戏 (Easy)

给定只包含 `+` 和 `-` 的字符串，返回所有把连续两个 `++` 翻转成 `--` 后可能得到的状态。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

### Java 解法补充

#### 基础解法：扫描所有相邻位置

算法思想：扫描所有相邻位置，遇到 `++` 就构造一个新字符串。


```java
class Solution {
    public List<String> generatePossibleNextMoves(String currentState) {
        List<String> ans = new ArrayList<>();
        char[] arr = currentState.toCharArray();
        for (int i = 0; i + 1 < arr.length; i++) {
            if (arr[i] == '+' && arr[i + 1] == '+') {
                arr[i] = arr[i + 1] = '-';
                ans.add(new String(arr));
                arr[i] = arr[i + 1] = '+';
            }
        }
        return ans;
    }
}
```


#### 资深解法：字符数组原地改再恢复

算法思想：字符数组原地改再恢复，比多次 `substring` 拼接更直接。

```java
class Solution {
    public java.util.List<String> generatePossibleNextMoves(String currentState) {
        java.util.List<String> ans = new java.util.ArrayList<>();
        char[] chars = currentState.toCharArray();
        for (int i = 0; i + 1 < chars.length; i++) {
            if (chars[i] == '+' && chars[i + 1] == '+') {
                chars[i] = '-';
                chars[i + 1] = '-';
                ans.add(new String(chars));
                chars[i] = '+';
                chars[i + 1] = '+';
            }
        }
        return ans;
    }
}
```

#### 基础语法与算法思想

- 每个可行动作只影响两个相邻字符。

---

## 294. 翻转游戏 II (Medium)

给定只包含 `+` 和 `-` 的字符串，两名玩家轮流把连续两个 `++` 翻转成 `--`，无法行动者失败。判断先手是否必胜。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

### Java 解法补充

#### 基础解法：递归枚举所有走法

算法思想：递归枚举所有走法，若存在一步让对手失败，则当前必胜。

```java
class Solution {
    public boolean canWin(String currentState) {
        char[] chars = currentState.toCharArray();
        for (int i = 0; i + 1 < chars.length; i++) {
            if (chars[i] == '+' && chars[i + 1] == '+') {
                chars[i] = '-';
                chars[i + 1] = '-';
                boolean opponentCanWin = canWin(new String(chars));
                chars[i] = '+';
                chars[i + 1] = '+';
                if (!opponentCanWin) {
                    return true;
                }
            }
        }
        return false;
    }
}
```



#### 资深解法：记忆化当前字符串状态

算法思想：记忆化当前字符串状态，避免重复博弈搜索。


```java
class Solution {
    private Map<String, Boolean> memo = new HashMap<>();

    public boolean canWin(String currentState) {
        if (memo.containsKey(currentState)) return memo.get(currentState);
        char[] arr = currentState.toCharArray();
        for (int i = 0; i + 1 < arr.length; i++) {
            if (arr[i] == '+' && arr[i + 1] == '+') {
                arr[i] = arr[i + 1] = '-';
                boolean win = !canWin(new String(arr));
                arr[i] = arr[i + 1] = '+';
                if (win) {
                    memo.put(currentState, true);
                    return true;
                }
            }
        }
        memo.put(currentState, false);
        return false;
    }
}
```


#### 基础语法与算法思想

- 博弈题常用“当前存在一步让对手输”定义必胜态。

---

## 295. 数据流的中位数 (Hard)

**中位数** 是有序整数列表中的中间值。如果列表的大小是偶数，则没有中间值，中位数是两个中间值的平均值。

例如  `arr = [2,3,4]`  的中位数是  `3`  。
例如  `arr = [2,3]`  的中位数是  `(2 + 3) / 2 = 2.5`  。

实现 MedianFinder 类:

 `MedianFinder()`  初始化  `MedianFinder`  对象。

 `void addNum(int num)`  将数据流中的整数  `num`  添加到数据结构中。

 `double findMedian()`  返回到目前为止所有元素的中位数。与实际答案相差  `10-5`  以内的答案将被接受。

 **示例 1：**

```text
输入
["MedianFinder", "addNum", "addNum", "findMedian", "addNum", "findMedian"]
[[], [1], [2], [], [3], []]
输出
[null, null, null, 1.5, null, 2.0]

解释
MedianFinder medianFinder = new MedianFinder();
medianFinder.addNum(1);    // arr = [1]
medianFinder.addNum(2);    // arr = [1, 2]
medianFinder.findMedian(); // 返回 1.5 ((1 + 2) / 2)
medianFinder.addNum(3);    // arr[1, 2, 3]
medianFinder.findMedian(); // return 2.0
```

 **提示:**

 `-105 <= num <= 105`
在调用  `findMedian`  之前，数据结构中至少有一个元素
最多  `5 * 104`  次调用  `addNum`  和  `findMedian`

### Java 解法补充

#### 基础解法：每次插入后排序或插入有序列表

算法思想：每次插入后排序或插入有序列表，查询中位数。

```java
class MedianFinder {
    private java.util.List<Integer> nums = new java.util.ArrayList<>();

    public void addNum(int num) {
        nums.add(num);
        java.util.Collections.sort(nums);
    }

    public double findMedian() {
        int n = nums.size();
        if (n % 2 == 1) {
            return nums.get(n / 2);
        }
        return (nums.get(n / 2 - 1) + nums.get(n / 2)) / 2.0;
    }
}
```



#### 资深解法：双堆：大根堆保存较小一半

算法思想：双堆：大根堆保存较小一半，小根堆保存较大一半。


```java
class MedianFinder {
    private PriorityQueue<Integer> small = new PriorityQueue<>(Collections.reverseOrder());
    private PriorityQueue<Integer> large = new PriorityQueue<>();

    public void addNum(int num) {
        small.offer(num);
        large.offer(small.poll());
        if (large.size() > small.size()) small.offer(large.poll());
    }

    public double findMedian() {
        if (small.size() > large.size()) return small.peek();
        return ((double) small.peek() + large.peek()) / 2.0;
    }
}
```


#### 基础语法与算法思想

- 保持 `small.size() >= large.size()` 且两堆有序分割。

---

## 296. 最佳的碰头地点 (Hard)

给定由 `0` 和 `1` 组成的二维网格，`1` 表示某人的家。选择一个碰头点，使所有人到该点的曼哈顿距离之和最小，返回最小距离。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

### Java 解法补充

#### 基础解法：枚举每个网格点作为碰头点

算法思想：枚举每个网格点作为碰头点，计算所有人的曼哈顿距离总和。

```java
class Solution {
    public int minTotalDistance(int[][] grid) {
        int ans = Integer.MAX_VALUE;
        for (int row = 0; row < grid.length; row++) {
            for (int col = 0; col < grid[0].length; col++) {
                int dist = 0;
                for (int i = 0; i < grid.length; i++) {
                    for (int j = 0; j < grid[0].length; j++) {
                        if (grid[i][j] == 1) {
                            dist += Math.abs(row - i) + Math.abs(col - j);
                        }
                    }
                }
                ans = Math.min(ans, dist);
            }
        }
        return ans;
    }
}
```



#### 资深解法：曼哈顿距离行列独立

算法思想：曼哈顿距离行列独立，最优点是所有人行坐标和列坐标的中位数。


```java
class Solution {
    public int minTotalDistance(int[][] grid) {
        List<Integer> rows = new ArrayList<>(), cols = new ArrayList<>();
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) if (grid[i][j] == 1) rows.add(i);
        }
        for (int j = 0; j < grid[0].length; j++) {
            for (int i = 0; i < grid.length; i++) if (grid[i][j] == 1) cols.add(j);
        }
        return dist(rows) + dist(cols);
    }

    private int dist(List<Integer> list) {
        int l = 0, r = list.size() - 1, ans = 0;
        while (l < r) ans += list.get(r--) - list.get(l++);
        return ans;
    }
}
```


#### 基础语法与算法思想

- 按行、按列扫描天然得到有序坐标；中位数最小化绝对距离和。

---

## 297. 二叉树的序列化与反序列化 (Hard)

序列化是将一个数据结构或者对象转换为连续的比特位的操作，进而可以将转换后的数据存储在一个文件或者内存中，同时也可以通过网络传输到另一个计算机环境，采取相反方式重构得到原数据。
请设计一个算法来实现二叉树的序列化与反序列化。这里不限定你的序列 / 反序列化算法执行逻辑，你只需要保证一个二叉树可以被序列化为一个字符串并且将这个字符串反序列化为原始的树结构。
 **提示:** 输入输出格式与 LeetCode 目前使用的方式一致，详情请参阅 LeetCode 序列化二叉树的格式。你并非必须采取这种方式，你也可以采用其他的方法解决这个问题。

 **示例 1：**

```text
输入：root = [1,2,3,null,null,4,5]
输出：[1,2,3,null,null,4,5]
```

 **示例 2：**

```text
输入：root = []
输出：[]
```

 **示例 3：**

```text
输入：root = [1]
输出：[1]
```

 **示例 4：**

```text
输入：root = [1,2]
输出：[1,2]
```


 **提示：**

树中结点数在范围  `[0, 104]`  内
 `-1000 <= Node.val <= 1000`

### Java 解法补充

#### 基础解法：层序遍历带 `null` 标记序列化

算法思想：层序遍历带 `null` 标记序列化。

```java
public class Codec {
    public String serialize(TreeNode root) {
        if (root == null) {
            return "";
        }
        StringBuilder ans = new StringBuilder();
        java.util.Queue<TreeNode> queue = new java.util.LinkedList<>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            TreeNode node = queue.poll();
            if (node == null) {
                ans.append("null,");
                continue;
            }
            ans.append(node.val).append(',');
            queue.offer(node.left);
            queue.offer(node.right);
        }
        return ans.toString();
    }

    public TreeNode deserialize(String data) {
        if (data.isEmpty()) {
            return null;
        }
        String[] values = data.split(",");
        TreeNode root = new TreeNode(Integer.parseInt(values[0]));
        java.util.Queue<TreeNode> queue = new java.util.ArrayDeque<>();
        queue.offer(root);
        int index = 1;
        while (!queue.isEmpty() && index < values.length) {
            TreeNode node = queue.poll();
            if (!values[index].equals("null")) {
                node.left = new TreeNode(Integer.parseInt(values[index]));
                queue.offer(node.left);
            }
            index++;
            if (index < values.length && !values[index].equals("null")) {
                node.right = new TreeNode(Integer.parseInt(values[index]));
                queue.offer(node.right);
            }
            index++;
        }
        return root;
    }
}
```



#### 资深解法：前序 DFS 加 `#` 空节点标记

算法思想：前序 DFS 加 `#` 空节点标记，递归反序列化。


```java
public class Codec {
    public String serialize(TreeNode root) {
        StringBuilder sb = new StringBuilder();
        ser(root, sb);
        return sb.toString();
    }

    private void ser(TreeNode node, StringBuilder sb) {
        if (node == null) {
            sb.append("#,");
            return;
        }
        sb.append(node.val).append(',');
        ser(node.left, sb);
        ser(node.right, sb);
    }

    public TreeNode deserialize(String data) {
        Queue<String> q = new ArrayDeque<>(Arrays.asList(data.split(",")));
        return de(q);
    }

    private TreeNode de(Queue<String> q) {
        String s = q.poll();
        if (s.equals("#")) return null;
        TreeNode node = new TreeNode(Integer.parseInt(s));
        node.left = de(q);
        node.right = de(q);
        return node;
    }
}
```


#### 基础语法与算法思想

- 空节点标记让树结构可唯一还原；前序反序列化天然递归消费队列。

---

## 298. 二叉树最长连续序列 (Medium)

给定二叉树根节点，返回最长的父子连续递增路径长度。路径必须从父节点到子节点，且子节点值等于父节点值加一。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

### Java 解法补充

#### 基础解法：从每个节点出发向下 DFS 找连续路径

算法思想：从每个节点出发向下 DFS 找连续路径，重复遍历。

```java
class Solution {
    public int longestConsecutive(TreeNode root) {
        if (root == null) {
            return 0;
        }
        return Math.max(down(root, root.val - 1),
                Math.max(longestConsecutive(root.left), longestConsecutive(root.right)));
    }

    private int down(TreeNode node, int prev) {
        if (node == null || node.val != prev + 1) {
            return 0;
        }
        return 1 + Math.max(down(node.left, node.val), down(node.right, node.val));
    }
}
```



#### 资深解法：DFS 携带父节点值和当前连续长度

算法思想：DFS 携带父节点值和当前连续长度。


```java
class Solution {
    private int ans = 0;

    public int longestConsecutive(TreeNode root) {
        dfs(root, 0, 0);
        return ans;
    }

    private void dfs(TreeNode node, int parent, int len) {
        if (node == null) return;
        int cur = node.val == parent + 1 ? len + 1 : 1;
        ans = Math.max(ans, cur);
        dfs(node.left, node.val, cur);
        dfs(node.right, node.val, cur);
    }
}
```


#### 基础语法与算法思想

- 路径方向必须从父到子；状态随 DFS 向下传递。

---

## 299. 猜数字游戏 (Medium)

你在和朋友一起玩 猜数字（Bulls and Cows）游戏，该游戏规则如下：
写出一个秘密数字，并请朋友猜这个数字是多少。朋友每猜测一次，你就会给他一个包含下述信息的提示：

猜测数字中有多少位属于数字和确切位置都猜对了（称为 "Bulls"，公牛），
有多少位属于数字猜对了但是位置不对（称为 "Cows"，奶牛）。也就是说，这次猜测中有多少位非公牛数字可以通过重新排列转换成公牛数字。

给你一个秘密数字  `secret`  和朋友猜测的数字  `guess`  ，请你返回对朋友这次猜测的提示。
提示的格式为  `"xAyB"`  ， `x`  是公牛个数，  `y`  是奶牛个数， `A`  表示公牛， `B`  表示奶牛。
请注意秘密数字和朋友猜测的数字都可能含有重复数字。

 **示例 1：**

```text
输入：secret = "1807", guess = "7810"
输出："1A3B"
解释：数字和位置都对（公牛）用 '|' 连接，数字猜对位置不对（奶牛）的采用斜体加粗标识。
"1807"
  |
"7810"
```

 **示例 2：**

```text
输入：secret = "1123", guess = "0111"
输出："1A1B"
解释：数字和位置都对（公牛）用 '|' 连接，数字猜对位置不对（奶牛）的采用斜体加粗标识。
"1123"        "1123"
  |      or     |
"0111"        "0111"
注意，两个不匹配的 1 中，只有一个会算作奶牛（数字猜对位置不对）。通过重新排列非公牛数字，其中仅有一个 1 可以成为公牛数字。
```


 **提示：**

 `1 <= secret.length, guess.length <= 1000`
 `secret.length == guess.length`
 `secret`  和  `guess`  仅由数字组成

### Java 解法补充

#### 基础解法：先统计公牛

算法思想：先统计公牛，再对非公牛数字做频次匹配得到奶牛。

```java
class Solution {
    public String getHint(String secret, String guess) {
        int bulls = 0;
        int[] secretCount = new int[10];
        int[] guessCount = new int[10];

        for (int i = 0; i < secret.length(); i++) {
            char a = secret.charAt(i);
            char b = guess.charAt(i);
            if (a == b) {
                bulls++;
            } else {
                secretCount[a - '0']++;
                guessCount[b - '0']++;
            }
        }

        int cows = 0;
        for (int digit = 0; digit < 10; digit++) {
            cows += Math.min(secretCount[digit], guessCount[digit]);
        }
        return bulls + "A" + cows + "B";
    }
}
```



#### 资深解法：用一个计数数组在一次扫描中处理非公牛字符的互补关系

算法思想：用一个计数数组在一次扫描中处理非公牛字符的互补关系。


```java
class Solution {
    public String getHint(String secret, String guess) {
        int bulls = 0, cows = 0;
        int[] count = new int[10];
        for (int i = 0; i < secret.length(); i++) {
            int a = secret.charAt(i) - '0', b = guess.charAt(i) - '0';
            if (a == b) bulls++;
            else {
                if (count[a] < 0) cows++;
                if (count[b] > 0) cows++;
                count[a]++;
                count[b]--;
            }
        }
        return bulls + "A" + cows + "B";
    }
}
```


#### 基础语法与算法思想

- 正数表示 secret 多出的数字，负数表示 guess 多出的数字。

---

## 300. 最长递增子序列 (Medium)

给你一个整数数组  `nums`  ，找到其中最长严格递增子序列的长度。
 **子序列** 是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。例如， `[3,6,2,7]`  是数组  `[0,3,1,6,2,2,7]`  的子序列。


 **示例 1：**

```text
输入：nums = [10,9,2,5,3,7,101,18]
输出：4
解释：最长递增子序列是 [2,3,7,101]，因此长度为 4 。
```

 **示例 2：**

```text
输入：nums = [0,1,0,3,2,3]
输出：4
```

 **示例 3：**

```text
输入：nums = [7,7,7,7,7,7,7]
输出：1
```


 **提示：**

 `1 <= nums.length <= 2500`
 `-104 <= nums[i] <= 104`


 **进阶：**

你能将算法的时间复杂度降低到  `O(n log(n))`  吗?

### Java 解法补充

#### 基础解法：`dp[i]` 表示以 `nums[i]` 结尾的 LIS 长度

算法思想：`dp[i]` 表示以 `nums[i]` 结尾的 LIS 长度，枚举前驱，时间 `O(n^2)`。

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int[] dp = new int[nums.length];
        java.util.Arrays.fill(dp, 1);
        int ans = 1;

        for (int i = 0; i < nums.length; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[j] < nums[i]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
            ans = Math.max(ans, dp[i]);
        }
        return ans;
    }
}
```



#### 资深解法：维护 `tails[len]` 为长度 `len+1` 递增子序列的最小结尾

算法思想：维护 `tails[len]` 为长度 `len+1` 递增子序列的最小结尾，二分更新。


```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int[] tails = new int[nums.length];
        int size = 0;
        for (int x : nums) {
            int l = 0, r = size;
            while (l < r) {
                int m = l + (r - l) / 2;
                if (tails[m] < x) l = m + 1;
                else r = m;
            }
            tails[l] = x;
            if (l == size) size++;
        }
        return size;
    }
}
```


#### 基础语法与算法思想

- `tails` 不一定是真实序列，但它的长度代表当前可达到的 LIS 长度；二分找第一个大于等于 `x` 的位置。

---
