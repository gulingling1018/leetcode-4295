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

### Java 解法补充

#### 基础解法：枚举每个运算符作为最后一次计算

算法思想：枚举每个运算符作为最后一次计算，递归计算左右两边所有可能结果。

```java
class Solution {
    public java.util.List<Integer> diffWaysToCompute(String expression) {
        java.util.List<Integer> ans = new java.util.ArrayList<>();
        for (int i = 0; i < expression.length(); i++) {
            char op = expression.charAt(i);
            if (op == '+' || op == '-' || op == '*') {
                java.util.List<Integer> left = diffWaysToCompute(expression.substring(0, i));
                java.util.List<Integer> right = diffWaysToCompute(expression.substring(i + 1));
                for (int a : left) {
                    for (int b : right) {
                        if (op == '+') ans.add(a + b);
                        else if (op == '-') ans.add(a - b);
                        else ans.add(a * b);
                    }
                }
            }
        }
        if (ans.isEmpty()) ans.add(Integer.parseInt(expression));
        return ans;
    }
}
```


#### 资深解法：分治加记忆化

算法思想：分治加记忆化，避免重复计算同一子表达式。


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


#### 基础语法与算法思想

- `substring` 切分子表达式；表达式加括号的本质是选择最后计算的运算符。

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

### Java 解法补充

#### 基础解法：排序两个字符串后比较

算法思想：排序两个字符串后比较。

```java
class Solution {
    public boolean isAnagram(String s, String t) {
        char[] a = s.toCharArray();
        char[] b = t.toCharArray();
        java.util.Arrays.sort(a);
        java.util.Arrays.sort(b);
        return java.util.Arrays.equals(a, b);
    }
}
```



#### 资深解法：计数字母频次

算法思想：计数字母频次，一个字符串加、另一个字符串减。


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


#### 基础语法与算法思想

- 固定小写字母表可用数组替代哈希表。

---

## 243. 最短单词距离 (Easy)

给定字符串数组 `wordsDict` 和两个不同的字符串 `word1`、`word2`，返回这两个单词在数组中出现位置的最小距离。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

### Java 解法补充

#### 基础解法：分别收集两个单词所有位置

算法思想：分别收集两个单词所有位置，再双重循环比较距离。

```java
class Solution {
    public int shortestDistance(String[] wordsDict, String word1, String word2) {
        java.util.List<Integer> a = new java.util.ArrayList<>();
        java.util.List<Integer> b = new java.util.ArrayList<>();
        for (int i = 0; i < wordsDict.length; i++) {
            if (wordsDict[i].equals(word1)) a.add(i);
            if (wordsDict[i].equals(word2)) b.add(i);
        }
        int ans = Integer.MAX_VALUE;
        for (int i : a) {
            for (int j : b) ans = Math.min(ans, Math.abs(i - j));
        }
        return ans;
    }
}
```



#### 资深解法：一次扫描记录两个单词最近位置

算法思想：一次扫描记录两个单词最近位置，实时更新最小距离。


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


#### 基础语法与算法思想

- 最近出现位置足以产生当前最小距离候选。

---

## 244. 最短单词距离 II (Medium)

设计一个类，构造时接收字符串数组 `wordsDict`；实现 `shortest(word1, word2)`，多次查询两个不同单词在数组中出现位置的最小距离。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

### Java 解法补充

#### 基础解法：每次查询都扫描整个数组

算法思想：每次查询都扫描整个数组，适合查询很少的场景。

```java
class WordDistance {
    private String[] words;

    public WordDistance(String[] wordsDict) {
        words = wordsDict;
    }

    public int shortest(String word1, String word2) {
        int p1 = -1, p2 = -1, ans = Integer.MAX_VALUE;
        for (int i = 0; i < words.length; i++) {
            if (words[i].equals(word1)) p1 = i;
            if (words[i].equals(word2)) p2 = i;
            if (p1 != -1 && p2 != -1) ans = Math.min(ans, Math.abs(p1 - p2));
        }
        return ans;
    }
}
```



#### 资深解法：构造时把每个单词出现位置存入列表；查询时双指针合并两个有序位置表

算法思想：构造时把每个单词出现位置存入列表；查询时双指针合并两个有序位置表。


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


#### 基础语法与算法思想

- `computeIfAbsent` 按需建表；多次查询题通常要预处理。

---

## 245. 最短单词距离 III (Medium)

给定字符串数组 `wordsDict` 和两个字符串 `word1`、`word2`，返回它们在数组中出现位置的最小距离；`word1` 和 `word2` 可能相同，相同时需要选择两个不同位置。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

### Java 解法补充

#### 基础解法：收集位置后分类处理两个单词是否相同

算法思想：收集位置后分类处理两个单词是否相同。

```java
class Solution {
    public int shortestWordDistance(String[] wordsDict, String word1, String word2) {
        int ans = Integer.MAX_VALUE;
        if (word1.equals(word2)) {
            int prev = -1;
            for (int i = 0; i < wordsDict.length; i++) {
                if (wordsDict[i].equals(word1)) {
                    if (prev != -1) ans = Math.min(ans, i - prev);
                    prev = i;
                }
            }
            return ans;
        }
        int p1 = -1, p2 = -1;
        for (int i = 0; i < wordsDict.length; i++) {
            if (wordsDict[i].equals(word1)) p1 = i;
            if (wordsDict[i].equals(word2)) p2 = i;
            if (p1 != -1 && p2 != -1) ans = Math.min(ans, Math.abs(p1 - p2));
        }
        return ans;
    }
}
```



#### 资深解法：一次扫描；相同单词时记录前一个出现位置

算法思想：一次扫描；相同单词时记录前一个出现位置，不同单词时记录各自最近位置。


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


#### 基础语法与算法思想

- `word1 == word2` 时必须使用两个不同下标。

---

## 246. 中心对称数 (Easy)

给定字符串 `num`，判断它旋转 180 度后是否仍然表示同一个数字。合法旋转映射包括 `0-0`、`1-1`、`6-9`、`8-8`、`9-6`。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

### Java 解法补充

#### 基础解法：构造旋转后的字符串再与原字符串比较

算法思想：构造旋转后的字符串再与原字符串比较。

```java
class Solution {
    public boolean isStrobogrammatic(String num) {
        java.util.Map<Character, Character> map = new java.util.HashMap<>();
        map.put('0', '0');
        map.put('1', '1');
        map.put('6', '9');
        map.put('8', '8');
        map.put('9', '6');

        StringBuilder rotated = new StringBuilder();
        for (int i = num.length() - 1; i >= 0; i--) {
            char c = num.charAt(i);
            if (!map.containsKey(c)) {
                return false;
            }
            rotated.append(map.get(c));
        }
        return rotated.toString().equals(num);
    }
}
```



#### 资深解法：双指针同时检查左右字符是否为合法旋转映射

算法思想：双指针同时检查左右字符是否为合法旋转映射。


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


#### 基础语法与算法思想

- 中心对称是“旋转后左右互换”的字符映射问题。

---

## 247. 中心对称数 II (Medium)

给定整数 `n`，返回所有长度为 `n` 的中心对称数。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

### Java 解法补充

#### 基础解法：生成所有长度为 `n` 的数字再逐个判断

算法思想：生成所有长度为 `n` 的数字再逐个判断，会爆炸。

```java
class Solution {
    public java.util.List<String> findStrobogrammatic(int n) {
        java.util.List<String> ans = new java.util.ArrayList<>();
        int start = n == 1 ? 0 : (int) Math.pow(10, n - 1);
        int end = (int) Math.pow(10, n);

        for (int x = start; x < end; x++) {
            String s = String.valueOf(x);
            if (s.length() == n && isStrobogrammatic(s)) {
                ans.add(s);
            }
        }
        return ans;
    }

    private boolean isStrobogrammatic(String s) {
        String pairs = "00 11 69 88 96";
        int left = 0;
        int right = s.length() - 1;
        while (left <= right) {
            String pair = "" + s.charAt(left) + s.charAt(right);
            if (!pairs.contains(pair)) {
                return false;
            }
            left++;
            right--;
        }
        return true;
    }
}
```



#### 资深解法：从中间向两边递归扩展合法数对

算法思想：从中间向两边递归扩展合法数对，外层不能放 `00`。


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


#### 基础语法与算法思想

- 递归长度每次减 2；首位不能为 0。

---

## 248. 中心对称数 III (Hard)

给定两个表示非负整数的字符串 `low` 和 `high`，返回闭区间 `[low, high]` 内中心对称数的个数。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

### Java 解法补充

#### 基础解法：枚举区间内每个数并判断

算法思想：枚举区间内每个数并判断，字符串范围大时不可行。

```java
class Solution {
    public int strobogrammaticInRange(String low, String high) {
        long start = Long.parseLong(low);
        long end = Long.parseLong(high);
        int ans = 0;

        for (long x = start; x <= end; x++) {
            if (isStrobogrammatic(String.valueOf(x))) {
                ans++;
            }
        }
        return ans;
    }

    private boolean isStrobogrammatic(String s) {
        String pairs = "00 11 69 88 96";
        int left = 0;
        int right = s.length() - 1;
        while (left <= right) {
            String pair = "" + s.charAt(left) + s.charAt(right);
            if (!pairs.contains(pair)) {
                return false;
            }
            left++;
            right--;
        }
        return true;
    }
}
```



#### 资深解法：按长度生成所有中心对称数

算法思想：按长度生成所有中心对称数，再用字符串比较过滤上下界。


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


#### 基础语法与算法思想

- 同长度数字可用字典序比较大小；生成候选比枚举区间更高效。

---

## 249. 移位字符串分组 (Medium)

字符串可以通过把每个字母同时向后移动若干位得到同组字符串，例如 `abc -> bcd`。给定字符串数组，将所有属于同一移位序列的字符串分组返回。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

### Java 解法补充

#### 基础解法：两两判断是否属于同一移位序列

算法思想：两两判断是否属于同一移位序列，再并入分组。

```java
class Solution {
    public java.util.List<java.util.List<String>> groupStrings(String[] strings) {
        java.util.List<java.util.List<String>> ans = new java.util.ArrayList<>();
        boolean[] used = new boolean[strings.length];

        for (int i = 0; i < strings.length; i++) {
            if (used[i]) {
                continue;
            }
            java.util.List<String> group = new java.util.ArrayList<>();
            for (int j = i; j < strings.length; j++) {
                if (!used[j] && sameGroup(strings[i], strings[j])) {
                    used[j] = true;
                    group.add(strings[j]);
                }
            }
            ans.add(group);
        }
        return ans;
    }

    private boolean sameGroup(String a, String b) {
        if (a.length() != b.length()) {
            return false;
        }
        int diff = (b.charAt(0) - a.charAt(0) + 26) % 26;
        for (int i = 1; i < a.length(); i++) {
            int cur = (b.charAt(i) - a.charAt(i) + 26) % 26;
            if (cur != diff) {
                return false;
            }
        }
        return true;
    }
}
```



#### 资深解法：用相邻字符差值作为规范化 key

算法思想：用相邻字符差值作为规范化 key。


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


#### 基础语法与算法思想

- 环形字母差值用 `+26` 后取模；规范化 key 把同类字符串聚到一起。

---

## 250. 统计同值子树 (Medium)

给定二叉树根节点，统计同值子树数量。同值子树表示该子树中的所有节点值都相同。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

### Java 解法补充

#### 基础解法：对每个节点单独检查子树是否同值

算法思想：对每个节点单独检查子树是否同值，重复遍历较多。

```java
class Solution {
    public int countUnivalSubtrees(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int left = countUnivalSubtrees(root.left);
        int right = countUnivalSubtrees(root.right);
        return left + right + (isUnival(root, root.val) ? 1 : 0);
    }

    private boolean isUnival(TreeNode node, int value) {
        if (node == null) {
            return true;
        }
        return node.val == value
                && isUnival(node.left, value)
                && isUnival(node.right, value);
    }
}
```



#### 资深解法：后序 DFS 返回当前子树是否同值

算法思想：后序 DFS 返回当前子树是否同值，并顺手计数。


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


#### 基础语法与算法思想

- 后序先知道左右子树结论，再判断当前子树。

---

## 251. 展开二维向量 (Medium)

设计一个迭代器，将二维向量 `vec` 按行展开；实现 `next()` 返回下一个整数，`hasNext()` 判断是否还有元素。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

### Java 解法补充

#### 基础解法：构造时把所有元素展开到一维列表

算法思想：构造时把所有元素展开到一维列表。

```java
class Vector2D {
    private java.util.List<Integer> data = new java.util.ArrayList<>();
    private int index = 0;

    public Vector2D(int[][] vec) {
        for (int[] row : vec) {
            for (int value : row) {
                data.add(value);
            }
        }
    }

    public int next() {
        return data.get(index++);
    }

    public boolean hasNext() {
        return index < data.size();
    }
}
```



#### 资深解法：两个下标按需跳过空行

算法思想：两个下标按需跳过空行，惰性返回元素。


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


#### 基础语法与算法思想

- `hasNext()` 负责推进到下一个有效位置，可让 `next()` 简洁。

---

## 252. 会议室 (Easy)

给定会议时间区间数组 `intervals`，判断一个人是否能够参加所有会议。若任意两个会议时间重叠，则无法参加所有会议。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

### Java 解法补充

#### 基础解法：两两检查会议区间是否重叠

算法思想：两两检查会议区间是否重叠。

```java
class Solution {
    public boolean canAttendMeetings(int[][] intervals) {
        for (int i = 0; i < intervals.length; i++) {
            for (int j = i + 1; j < intervals.length; j++) {
                int start = Math.max(intervals[i][0], intervals[j][0]);
                int end = Math.min(intervals[i][1], intervals[j][1]);
                if (start < end) {
                    return false;
                }
            }
        }
        return true;
    }
}
```



#### 资深解法：按开始时间排序

算法思想：按开始时间排序，只需检查相邻会议是否冲突。


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


#### 基础语法与算法思想

- 排序后若有重叠，一定出现在相邻区间之间。

---

## 253. 会议室 II (Medium)

给定会议时间区间数组 `intervals`，返回完成所有会议所需的最少会议室数量。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

### Java 解法补充

#### 基础解法：扫描每个会议开始时有多少会议尚未结束

算法思想：扫描每个会议开始时有多少会议尚未结束。

```java
class Solution {
    public int minMeetingRooms(int[][] intervals) {
        int ans = 0;
        for (int[] current : intervals) {
            int rooms = 0;
            for (int[] other : intervals) {
                if (other[0] <= current[0] && current[0] < other[1]) {
                    rooms++;
                }
            }
            ans = Math.max(ans, rooms);
        }
        return ans;
    }
}
```



#### 资深解法：按开始时间排序

算法思想：按开始时间排序，小根堆保存当前占用会议室的结束时间。


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


#### 基础语法与算法思想

- 最早结束的会议室若能复用，就弹出它；堆大小是所需房间数。

---

## 254. 因子的组合 (Medium)

给定整数 `n`，返回它所有可能的因子组合。组合中的因子必须大于 1 且小于 `n`，每个组合中因子乘积等于 `n`。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

### Java 解法补充

#### 基础解法：枚举所有组合并检查乘积

算法思想：枚举所有组合并检查乘积。

```java
class Solution {
    public java.util.List<java.util.List<Integer>> getFactors(int n) {
        java.util.List<java.util.List<Integer>> ans = new java.util.ArrayList<>();
        dfs(n, 2, new java.util.ArrayList<>(), ans);
        return ans;
    }

    private void dfs(int rest, int start, java.util.List<Integer> path,
            java.util.List<java.util.List<Integer>> ans) {
        if (rest == 1) {
            if (path.size() > 1) {
                ans.add(new java.util.ArrayList<>(path));
            }
            return;
        }

        for (int factor = start; factor <= rest; factor++) {
            if (rest % factor == 0) {
                path.add(factor);
                dfs(rest / factor, factor, path, ans);
                path.remove(path.size() - 1);
            }
        }
    }
}
```



#### 资深解法：回溯从当前最小因子开始试除

算法思想：回溯从当前最小因子开始试除，保证组合非降序以去重。


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


#### 基础语法与算法思想

- `start` 保证组合顺序；先收集当前因子对，再继续拆右侧因子。

---

## 255. 验证二叉搜索树的前序遍历序列 (Medium)

给定整数数组 `preorder`，判断它是否可以表示某棵二叉搜索树的前序遍历序列。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

### Java 解法补充

#### 基础解法：递归按 BST 前序性质切分左右子序列

算法思想：递归按 BST 前序性质切分左右子序列。

```java
class Solution {
    public boolean verifyPreorder(int[] preorder) {
        return check(preorder, 0, preorder.length - 1, Integer.MIN_VALUE, Integer.MAX_VALUE);
    }

    private boolean check(int[] preorder, int left, int right, int low, int high) {
        if (left > right) {
            return true;
        }
        int root = preorder[left];
        if (root <= low || root >= high) {
            return false;
        }

        int split = left + 1;
        while (split <= right && preorder[split] < root) {
            split++;
        }
        for (int i = split; i <= right; i++) {
            if (preorder[i] < root) {
                return false;
            }
        }

        return check(preorder, left + 1, split - 1, low, root)
                && check(preorder, split, right, root, high);
    }
}
```



#### 资深解法：单调栈维护祖先路径

算法思想：单调栈维护祖先路径，`lower` 表示当前节点必须大于的下界。


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


#### 基础语法与算法思想

- 一旦进入某个祖先的右子树，后续值都必须大于该祖先。

---

## 256. 粉刷房子 (Medium)

有一排房子，每间可刷成红、蓝、绿三种颜色，费用由 `costs[i][j]` 给出。相邻房子颜色不能相同，返回粉刷所有房子的最小总费用。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

### Java 解法补充

#### 基础解法：`dp[i][color]` 表示刷到第 `i` 间且当前颜色为 `color` 的最小费用

算法思想：`dp[i][color]` 表示刷到第 `i` 间且当前颜色为 `color` 的最小费用。

```java
class Solution {
    public int minCost(int[][] costs) {
        int n = costs.length;
        int[][] dp = new int[n][3];
        for (int color = 0; color < 3; color++) {
            dp[0][color] = costs[0][color];
        }

        for (int i = 1; i < n; i++) {
            dp[i][0] = costs[i][0] + Math.min(dp[i - 1][1], dp[i - 1][2]);
            dp[i][1] = costs[i][1] + Math.min(dp[i - 1][0], dp[i - 1][2]);
            dp[i][2] = costs[i][2] + Math.min(dp[i - 1][0], dp[i - 1][1]);
        }

        return Math.min(dp[n - 1][0], Math.min(dp[n - 1][1], dp[n - 1][2]));
    }
}
```



#### 资深解法：原地更新 `costs`

算法思想：原地更新 `costs`，每行只依赖上一行其他两个颜色。


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


#### 基础语法与算法思想

- 相邻颜色不同，所以当前颜色只能接上一行另外两种颜色。

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

### Java 解法补充

#### 基础解法：DFS 携带路径字符串

算法思想：DFS 携带路径字符串，到叶子节点收集。

```java
class Solution {
    public java.util.List<String> binaryTreePaths(TreeNode root) {
        java.util.List<String> ans = new java.util.ArrayList<>();
        dfs(root, "", ans);
        return ans;
    }

    private void dfs(TreeNode node, String path, java.util.List<String> ans) {
        if (node == null) {
            return;
        }
        String current = path.isEmpty() ? String.valueOf(node.val) : path + "->" + node.val;
        if (node.left == null && node.right == null) {
            ans.add(current);
            return;
        }
        dfs(node.left, current, ans);
        dfs(node.right, current, ans);
    }
}
```



#### 资深解法：用 `StringBuilder` 回溯

算法思想：用 `StringBuilder` 回溯，减少中间字符串创建。


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


#### 基础语法与算法思想

- `setLength` 是 `StringBuilder` 回溯撤销的常用手法。

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

### Java 解法补充

#### 基础解法：循环求各位数字和

算法思想：循环求各位数字和，直到结果小于 10。

```java
class Solution {
    public int addDigits(int num) {
        while (num >= 10) {
            int sum = 0;
            while (num > 0) {
                sum += num % 10;
                num /= 10;
            }
            num = sum;
        }
        return num;
    }
}
```



#### 资深解法：数根公式：除 0 外答案为 `1 + (num - 1) % 9`

算法思想：数根公式：除 0 外答案为 `1 + (num - 1) % 9`。


```java
class Solution {
    public int addDigits(int num) {
        return num == 0 ? 0 : 1 + (num - 1) % 9;
    }
}
```


#### 基础语法与算法思想

- 十进制数对 9 同余于其各位数字和。

---

## 259. 较小的三数之和 (Medium)

给定整数数组 `nums` 和目标值 `target`，返回满足 `nums[i] + nums[j] + nums[k] < target` 且 `i < j < k` 的三元组数量。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

### Java 解法补充

#### 基础解法：三重循环枚举所有三元组

算法思想：三重循环枚举所有三元组。

```java
class Solution {
    public int threeSumSmaller(int[] nums, int target) {
        int ans = 0;
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                for (int k = j + 1; k < nums.length; k++) {
                    if (nums[i] + nums[j] + nums[k] < target) {
                        ans++;
                    }
                }
            }
        }
        return ans;
    }
}
```



#### 资深解法：排序后固定一个数

算法思想：排序后固定一个数，双指针统计小于目标的组合数。


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


#### 基础语法与算法思想

- 当前和小于目标时，`l` 到 `r` 之间任意右端点都成立。

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

### Java 解法补充

#### 基础解法：哈希表计数

算法思想：哈希表计数，找出现一次的两个数。

```java
class Solution {
    public int[] singleNumber(int[] nums) {
        java.util.Map<Integer, Integer> count = new java.util.HashMap<>();
        for (int num : nums) {
            count.put(num, count.getOrDefault(num, 0) + 1);
        }

        int[] ans = new int[2];
        int index = 0;
        for (int num : nums) {
            if (count.get(num) == 1) {
                ans[index++] = num;
            }
        }
        return ans;
    }
}
```



#### 资深解法：全体异或得到 `a ^ b`

算法思想：全体异或得到 `a ^ b`，取最低位 1 把两个数分到不同组。


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


#### 基础语法与算法思想

- 异或可消去成对数字；区分位保证两个唯一数分到不同组。

---

## 261. 以图判树 (Medium)

给定 `n` 个节点和无向边数组 `edges`，判断这些边是否构成一棵合法树。合法树需要连通且无环。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

### Java 解法补充

#### 基础解法：树必须边数为 `n - 1`

算法思想：树必须边数为 `n - 1`，再 DFS 检查连通性。

```java
class Solution {
    public boolean validTree(int n, int[][] edges) {
        if (edges.length != n - 1) {
            return false;
        }

        java.util.List<Integer>[] graph = new java.util.ArrayList[n];
        for (int i = 0; i < n; i++) {
            graph[i] = new java.util.ArrayList<>();
        }
        for (int[] edge : edges) {
            graph[edge[0]].add(edge[1]);
            graph[edge[1]].add(edge[0]);
        }

        boolean[] seen = new boolean[n];
        dfs(0, graph, seen);
        for (boolean visited : seen) {
            if (!visited) {
                return false;
            }
        }
        return true;
    }

    private void dfs(int node, java.util.List<Integer>[] graph, boolean[] seen) {
        seen[node] = true;
        for (int next : graph[node]) {
            if (!seen[next]) {
                dfs(next, graph, seen);
            }
        }
    }
}
```



#### 资深解法：并查集

算法思想：并查集，若合并时发现同根说明有环。


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


#### 基础语法与算法思想

- 无向图是树等价于连通且无环；`n-1` 条边加无环即可保证连通。

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

### SQL/Shell 解法补充
#### 基础解法：过滤未被封禁的乘客和司机

算法思想：过滤未被封禁的乘客和司机，再按日期统计取消率。


```sql
SELECT request_at AS Day,
       ROUND(SUM(status != 'completed') / COUNT(*), 2) AS `Cancellation Rate`
FROM Trips
WHERE request_at BETWEEN '2013-10-01' AND '2013-10-03'
  AND client_id NOT IN (SELECT users_id FROM Users WHERE banned = 'Yes')
  AND driver_id NOT IN (SELECT users_id FROM Users WHERE banned = 'Yes')
GROUP BY request_at;
```


#### 资深解法：SQL 题不用 Java；核心是先过滤有效用户

算法思想：SQL 题不用 Java；核心是先过滤有效用户，再条件聚合。`SUM(boolean)` 在 MySQL 中可统计满足条件的行数。

```sql
SELECT t.request_at AS Day,
       ROUND(SUM(t.status != 'completed') / COUNT(*), 2) AS `Cancellation Rate`
FROM Trips t
JOIN Users c ON t.client_id = c.users_id AND c.banned = 'No'
JOIN Users d ON t.driver_id = d.users_id AND d.banned = 'No'
WHERE t.request_at BETWEEN '2013-10-01' AND '2013-10-03'
GROUP BY t.request_at;
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

### Java 解法补充

#### 基础解法：不断尝试除以 2、3、5

算法思想：不断尝试除以 2、3、5，最后是否等于 1。

```java
class Solution {
    public boolean isUgly(int n) {
        if (n <= 0) {
            return false;
        }
        int[] factors = {2, 3, 5};
        for (int factor : factors) {
            while (n % factor == 0) {
                n /= factor;
            }
        }
        return n == 1;
    }
}
```



#### 资深解法：丑数定义只允许这些质因子

算法思想：丑数定义只允许这些质因子，任何其他剩余因子都不合法。


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


#### 基础语法与算法思想

- `while` 连续去除同一质因子；1 通常视为丑数。

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

### Java 解法补充

#### 基础解法：逐个判断每个整数是否为丑数

算法思想：逐个判断每个整数是否为丑数，直到找到第 `n` 个。

```java
class Solution {
    public int nthUglyNumber(int n) {
        int count = 0;
        int num = 0;
        while (count < n) {
            num++;
            if (isUgly(num)) {
                count++;
            }
        }
        return num;
    }

    private boolean isUgly(int num) {
        for (int factor : new int[]{2, 3, 5}) {
            while (num % factor == 0) {
                num /= factor;
            }
        }
        return num == 1;
    }
}
```



#### 资深解法：三指针 DP

算法思想：三指针 DP，每次取 `2/3/5` 倍候选中的最小值。


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


#### 基础语法与算法思想

- 三个 `if` 都要执行，避免重复丑数。

---

## 265. 粉刷房子 II (Hard)

有一排房子和 `k` 种颜色，费用由 `costs[i][j]` 给出。相邻房子颜色不能相同，返回粉刷所有房子的最小总费用。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

### Java 解法补充

#### 基础解法：对每个房子颜色枚举上一间所有不同颜色

算法思想：对每个房子颜色枚举上一间所有不同颜色，时间 `O(nk^2)`。

```java
class Solution {
    public int minCostII(int[][] costs) {
        int n = costs.length;
        int k = costs[0].length;
        int[][] dp = new int[n][k];

        for (int color = 0; color < k; color++) {
            dp[0][color] = costs[0][color];
        }

        for (int i = 1; i < n; i++) {
            for (int color = 0; color < k; color++) {
                int best = Integer.MAX_VALUE;
                for (int prev = 0; prev < k; prev++) {
                    if (prev != color) {
                        best = Math.min(best, dp[i - 1][prev]);
                    }
                }
                dp[i][color] = costs[i][color] + best;
            }
        }

        int ans = Integer.MAX_VALUE;
        for (int value : dp[n - 1]) {
            ans = Math.min(ans, value);
        }
        return ans;
    }
}
```



#### 资深解法：记录上一行最小值和次小值；当前颜色若等于最小值颜色

算法思想：记录上一行最小值和次小值；当前颜色若等于最小值颜色，就只能接次小值。


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


#### 基础语法与算法思想

- 最小/次小值优化把颜色转移从 `O(k^2)` 降到 `O(k)`。

---

## 266. 回文排列 (Easy)

给定字符串 `s`，判断它的某个排列是否可以组成回文串。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

### Java 解法补充

#### 基础解法：统计每个字符频次

算法思想：统计每个字符频次，奇数频次最多只能有一个。

```java
class Solution {
    public boolean canPermutePalindrome(String s) {
        java.util.Map<Character, Integer> count = new java.util.HashMap<>();
        for (char c : s.toCharArray()) {
            count.put(c, count.getOrDefault(c, 0) + 1);
        }

        int odd = 0;
        for (int value : count.values()) {
            if (value % 2 == 1) {
                odd++;
            }
        }
        return odd <= 1;
    }
}
```



#### 资深解法：用集合切换奇偶状态

算法思想：用集合切换奇偶状态，出现一次加入、再出现移除。


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


#### 基础语法与算法思想

- 回文排列只关心字符频次奇偶性。

---

## 267. 回文排列 II (Medium)

给定字符串 `s`，返回它所有可以组成回文串的不同排列；如果不存在，返回空列表。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

### Java 解法补充

#### 基础解法：全排列后过滤回文

算法思想：全排列后过滤回文，重复多且复杂度高。

```java
class Solution {
    public java.util.List<String> generatePalindromes(String s) {
        java.util.Set<String> seen = new java.util.HashSet<>();
        boolean[] used = new boolean[s.length()];
        dfs(s.toCharArray(), used, new StringBuilder(), seen);
        return new java.util.ArrayList<>(seen);
    }

    private void dfs(char[] chars, boolean[] used, StringBuilder path, java.util.Set<String> seen) {
        if (path.length() == chars.length) {
            String candidate = path.toString();
            if (isPalindrome(candidate)) {
                seen.add(candidate);
            }
            return;
        }

        for (int i = 0; i < chars.length; i++) {
            if (used[i]) {
                continue;
            }
            used[i] = true;
            path.append(chars[i]);
            dfs(chars, used, path, seen);
            path.deleteCharAt(path.length() - 1);
            used[i] = false;
        }
    }

    private boolean isPalindrome(String s) {
        int left = 0;
        int right = s.length() - 1;
        while (left < right) {
            if (s.charAt(left++) != s.charAt(right--)) {
                return false;
            }
        }
        return true;
    }
}
```



#### 资深解法：统计字符

算法思想：统计字符，只对半边字符串做去重回溯，再镜像生成回文。


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


#### 基础语法与算法思想

- 半边去重回溯即可生成全部不同回文；镜像时要恢复 `StringBuilder`。

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

### Java 解法补充

#### 基础解法：排序后找第一个下标和值不同的位置

算法思想：排序后找第一个下标和值不同的位置。

```java
class Solution {
    public int missingNumber(int[] nums) {
        java.util.Arrays.sort(nums);
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != i) {
                return i;
            }
        }
        return nums.length;
    }
}
```



#### 资深解法：利用等差和减去数组和

算法思想：利用等差和减去数组和，或异或下标和值。


```java
class Solution {
    public int missingNumber(int[] nums) {
        int n = nums.length, ans = n;
        for (int i = 0; i < n; i++) ans ^= i ^ nums[i];
        return ans;
    }
}
```


#### 基础语法与算法思想

- 相同数字异或为 0，剩下的就是缺失值。

---

## 269. 火星词典 (Hard)

给定按外星语言字典序排序的单词列表 `words`，推断该语言中字母的顺序。若不存在合法顺序，返回空字符串；若有多个合法顺序，返回任意一个。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

### Java 解法补充

#### 基础解法：比较相邻单词的第一个不同字符

算法思想：比较相邻单词的第一个不同字符，建立字母先后约束，然后拓扑排序。

```java
class Solution {
    public String alienOrder(String[] words) {
        java.util.Map<Character, java.util.Set<Character>> graph = new java.util.HashMap<>();
        java.util.Map<Character, Integer> indegree = new java.util.HashMap<>();

        for (String word : words) {
            for (char c : word.toCharArray()) {
                graph.putIfAbsent(c, new java.util.HashSet<>());
                indegree.putIfAbsent(c, 0);
            }
        }

        for (int i = 0; i + 1 < words.length; i++) {
            String a = words[i];
            String b = words[i + 1];
            int len = Math.min(a.length(), b.length());
            int j = 0;
            while (j < len && a.charAt(j) == b.charAt(j)) {
                j++;
            }
            if (j == len) {
                if (a.length() > b.length()) {
                    return "";
                }
                continue;
            }
            char from = a.charAt(j);
            char to = b.charAt(j);
            if (graph.get(from).add(to)) {
                indegree.put(to, indegree.get(to) + 1);
            }
        }

        java.util.Queue<Character> queue = new java.util.ArrayDeque<>();
        for (char c : indegree.keySet()) {
            if (indegree.get(c) == 0) {
                queue.offer(c);
            }
        }

        StringBuilder ans = new StringBuilder();
        while (!queue.isEmpty()) {
            char c = queue.poll();
            ans.append(c);
            for (char next : graph.get(c)) {
                indegree.put(next, indegree.get(next) - 1);
                if (indegree.get(next) == 0) {
                    queue.offer(next);
                }
            }
        }
        return ans.length() == indegree.size() ? ans.toString() : "";
    }
}
```



#### 资深解法：同时处理非法前缀情况

算法思想：同时处理非法前缀情况，如 `"abc"` 排在 `"ab"` 前面应返回空串。


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


#### 基础语法与算法思想

- 相邻单词提供最小必要约束；字母序推断是有向图拓扑排序。

---

## 270. 最接近的二叉搜索树值 (Easy)

给定二叉搜索树根节点和目标值 `target`，返回树中最接近 `target` 的节点值。

题面补充来源：LeetCode Wiki，核对日期：2026-05-15。

### Java 解法补充

#### 基础解法：中序遍历所有节点

算法思想：中序遍历所有节点，找与目标差最小的值。

```java
class Solution {
    public int closestValue(TreeNode root, double target) {
        java.util.List<Integer> values = new java.util.ArrayList<>();
        inorder(root, values);

        int ans = values.get(0);
        for (int value : values) {
            if (Math.abs(value - target) < Math.abs(ans - target)) {
                ans = value;
            }
        }
        return ans;
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



#### 资深解法：利用 BST 搜索路径

算法思想：利用 BST 搜索路径，边向下走边更新最接近值。


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


#### 基础语法与算法思想

- BST 的搜索路径已经覆盖最可能接近目标的方向，空间 `O(1)`。

---
