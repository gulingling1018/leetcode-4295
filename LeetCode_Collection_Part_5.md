## 121. 买卖股票的最佳时机 (Easy)

给定一个数组  `prices`  ，它的第  `i`  个元素  `prices[i]`  表示一支给定股票第  `i`  天的价格。
你只能选择  **某一天**  买入这只股票，并选择在  **未来的某一个不同的日子**  卖出该股票。设计一个算法来计算你所能获取的最大利润。
返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回  `0`  。
 
 **示例 1：** 

```text
输入：[7,1,5,3,6,4]
输出：5
解释：在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
```

 **示例 2：** 

```text
输入：prices = [7,6,4,3,1]
输出：0
解释：在这种情况下, 没有交易完成, 所以最大利润为 0。
```

 
 **提示：** 

 `1 <= prices.length <= 105` 
 `0 <= prices[i] <= 104`

### Java 解法补充

#### 基础解法：枚举买入日和卖出日计算最大利润

算法思想：枚举买入日和卖出日计算最大利润。

```java
class Solution {
    public int maxProfit(int[] prices) {
        int ans = 0;
        for (int buy = 0; buy < prices.length; buy++) {
            for (int sell = buy + 1; sell < prices.length; sell++) {
                ans = Math.max(ans, prices[sell] - prices[buy]);
            }
        }
        return ans;
    }
}
```

#### 资深解法：一次扫描维护历史最低价和当前最大利润

算法思想：一次扫描维护历史最低价和当前最大利润。


```java
class Solution {
    public int maxProfit(int[] prices) {
        int minPrice = Integer.MAX_VALUE, ans = 0;
        for (int p : prices) {
            minPrice = Math.min(minPrice, p);
            ans = Math.max(ans, p - minPrice);
        }
        return ans;
    }
}
```


#### 基础语法与算法思想

- `minPrice` 是到当前日之前的最佳买入价；只允许一次交易。

---
 
## 122. 买卖股票的最佳时机 II (Medium)

给你一个整数数组  `prices`  ，其中  `prices[i]`  表示某支股票第  `i`  天的价格。
在每一天，你可以决定是否购买和/或出售股票。你在任何时候  **最多**  只能持有  **一股**  股票。然而，你可以在  **同一天**  多次买卖该股票，但要确保你持有的股票不超过一股。
返回 你能获得的  **最大**  利润 。
 
 **示例 1：** 

```text
输入：prices = [7,1,5,3,6,4]
输出：7
解释：在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5 - 1 = 4。
随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6 - 3 = 3。
最大总利润为 4 + 3 = 7 。
```

 **示例 2：** 

```text
输入：prices = [1,2,3,4,5]
输出：4
解释：在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5 - 1 = 4。
最大总利润为 4 。
```

 **示例 3：** 

```text
输入：prices = [7,6,4,3,1]
输出：0
解释：在这种情况下, 交易无法获得正利润，所以不参与交易可以获得最大利润，最大利润为 0。
```

 
 **提示：** 

 `1 <= prices.length <= 3 * 104` 
 `0 <= prices[i] <= 104`

### Java 解法补充

#### 基础解法：DP 记录每天持股/不持股的最大收益

算法思想：DP 记录每天持股/不持股的最大收益。

```java
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        int[][] dp = new int[n][2];
        dp[0][0] = 0;
        dp[0][1] = -prices[0];
        for (int i = 1; i < n; i++) {
            dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] + prices[i]);
            dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][0] - prices[i]);
        }
        return dp[n - 1][0];
    }
}
```

#### 资深解法：所有上涨段利润都可累加

算法思想：所有上涨段利润都可累加。


```java
class Solution {
    public int maxProfit(int[] prices) {
        int ans = 0;
        for (int i = 1; i < prices.length; i++) {
            if (prices[i] > prices[i - 1]) ans += prices[i] - prices[i - 1];
        }
        return ans;
    }
}
```


#### 基础语法与算法思想

- 可多次交易且不能同时持多股时，贪心吃掉每段上涨。

---

## 123. 买卖股票的最佳时机 III (Hard)

给定一个数组，它的第  `i`  个元素是一支给定的股票在第  `i`  天的价格。
设计一个算法来计算你所能获取的最大利润。你最多可以完成  **两笔** 交易。
 **注意：** 你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。
 
 **示例 1:** 

```text
输入：prices = [3,3,5,0,0,3,1,4]
输出：6
解释：在第 4 天（股票价格 = 0）的时候买入，在第 6 天（股票价格 = 3）的时候卖出，这笔交易所能获得利润 = 3-0 = 3 。
     随后，在第 7 天（股票价格 = 1）的时候买入，在第 8 天 （股票价格 = 4）的时候卖出，这笔交易所能获得利润 = 4-1 = 3 。
```

 **示例 2：** 

```text
输入：prices = [1,2,3,4,5]
输出：4
解释：在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。   
     注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。   
     因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。
```

 **示例 3：** 

```text
输入：prices = [7,6,4,3,1] 
输出：0 
解释：在这个情况下, 没有交易完成, 所以最大利润为 0。
```

 **示例 4：** 

```text
输入：prices = [1]
输出：0
```

 
 **提示：** 

 `1 <= prices.length <= 105` 
 `0 <= prices[i] <= 105`

### Java 解法补充

#### 基础解法：二维 DP

算法思想：二维 DP，交易次数为 0、1、2，记录持股/不持股状态。

```java
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        int[][][] dp = new int[n][3][2];
        for (int k = 0; k <= 2; k++) dp[0][k][1] = -prices[0];
        for (int i = 1; i < n; i++) {
            for (int k = 0; k <= 2; k++) {
                dp[i][k][0] = dp[i - 1][k][0];
                if (k > 0) dp[i][k][0] = Math.max(dp[i][k][0], dp[i - 1][k][1] + prices[i]);
                dp[i][k][1] = dp[i - 1][k][1];
                if (k > 0) dp[i][k][1] = Math.max(dp[i][k][1], dp[i - 1][k - 1][0] - prices[i]);
            }
        }
        return dp[n - 1][2][0];
    }
}
```

#### 资深解法：四个变量表示两次买入卖出的最优状态

算法思想：四个变量表示两次买入卖出的最优状态。


```java
class Solution {
    public int maxProfit(int[] prices) {
        int buy1 = Integer.MIN_VALUE, sell1 = 0, buy2 = Integer.MIN_VALUE, sell2 = 0;
        for (int p : prices) {
            buy1 = Math.max(buy1, -p);
            sell1 = Math.max(sell1, buy1 + p);
            buy2 = Math.max(buy2, sell1 - p);
            sell2 = Math.max(sell2, buy2 + p);
        }
        return sell2;
    }
}
```


#### 基础语法与算法思想

- 状态机 DP 可压缩成变量；买入是减价格，卖出是加价格。

---

## 124. 二叉树中的最大路径和 (Hard)

二叉树中的 **路径**  被定义为一条节点序列，序列中每对相邻节点之间都存在一条边。同一个节点在一条路径序列中  **至多出现一次**  。该路径 **至少包含一个** 节点，且不一定经过根节点。
 **路径和**  是路径中各节点值的总和。
给你一个二叉树的根节点  `root`  ，返回其  **最大路径和**  。
 
 **示例 1：** 

```text
输入：root = [1,2,3]
输出：6
解释：最优路径是 2 -> 1 -> 3 ，路径和为 2 + 1 + 3 = 6
```

 **示例 2：** 

```text
输入：root = [-10,9,20,null,null,15,7]
输出：42
解释：最优路径是 15 -> 20 -> 7 ，路径和为 15 + 20 + 7 = 42
```

 
 **提示：** 

树中节点数目范围是  `[1, 3 * 104]` 
 `-1000 <= Node.val <= 1000`

### Java 解法补充

#### 基础解法：枚举每个节点作为路径最高点

算法思想：枚举每个节点作为路径最高点，计算左右最大贡献。

```java
class Solution {
    public int maxPathSum(TreeNode root) {
        java.util.List<TreeNode> nodes = new java.util.ArrayList<>();
        collect(root, nodes);
        int ans = Integer.MIN_VALUE;
        for (TreeNode node : nodes) {
            int left = Math.max(0, down(node.left));
            int right = Math.max(0, down(node.right));
            ans = Math.max(ans, node.val + left + right);
        }
        return ans;
    }

    private void collect(TreeNode node, java.util.List<TreeNode> nodes) {
        if (node == null) return;
        nodes.add(node);
        collect(node.left, nodes);
        collect(node.right, nodes);
    }

    private int down(TreeNode node) {
        if (node == null) return 0;
        return node.val + Math.max(0, Math.max(down(node.left), down(node.right)));
    }
}
```

#### 资深解法：后序递归返回单边最大贡献

算法思想：后序递归返回单边最大贡献，全局更新左右加根的路径和。


```java
class Solution {
    private int ans = Integer.MIN_VALUE;
    public int maxPathSum(TreeNode root) {
        gain(root);
        return ans;
    }
    private int gain(TreeNode node) {
        if (node == null) return 0;
        int left = Math.max(0, gain(node.left));
        int right = Math.max(0, gain(node.right));
        ans = Math.max(ans, node.val + left + right);
        return node.val + Math.max(left, right);
    }
}
```


#### 基础语法与算法思想

- 返回给父节点的路径只能选择一边；全局答案可同时使用左右两边。

---

## 125. 验证回文串 (Easy)

如果在将所有大写字符转换为小写字符、并移除所有非字母数字字符之后，短语正着读和反着读都一样。则可以认为该短语是一个  **回文串**  。
字母和数字都属于字母数字字符。
给你一个字符串  `s` ，如果它是  **回文串**  ，返回  `true`  ；否则，返回  `false`  。
 
 **示例 1：** 

```text
输入: s = "A man, a plan, a canal: Panama"
输出：true
解释："amanaplanacanalpanama" 是回文串。
```

 **示例 2：** 

```text
输入：s = "race a car"
输出：false
解释："raceacar" 不是回文串。
```

 **示例 3：** 

```text
输入：s = " "
输出：true
解释：在移除非字母数字字符之后，s 是一个空字符串 "" 。
由于空字符串正着反着读都一样，所以是回文串。
```

 
 **提示：** 

 `1 <= s.length <= 2 * 105` 
 `s`  仅由可打印的 ASCII 字符组成

### Java 解法补充

#### 基础解法：过滤出字母数字并转小写后双指针比较

算法思想：过滤出字母数字并转小写后双指针比较。

```java
class Solution {
    public boolean isPalindrome(String s) {
        StringBuilder clean = new StringBuilder();
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (Character.isLetterOrDigit(c)) clean.append(Character.toLowerCase(c));
        }
        int left = 0, right = clean.length() - 1;
        while (left < right) {
            if (clean.charAt(left) != clean.charAt(right)) return false;
            left++;
            right--;
        }
        return true;
    }
}
```

#### 资深解法：原字符串上双指针跳过非字母数字

算法思想：原字符串上双指针跳过非字母数字。


```java
class Solution {
    public boolean isPalindrome(String s) {
        int l = 0, r = s.length() - 1;
        while (l < r) {
            while (l < r && !Character.isLetterOrDigit(s.charAt(l))) l++;
            while (l < r && !Character.isLetterOrDigit(s.charAt(r))) r--;
            if (Character.toLowerCase(s.charAt(l++)) != Character.toLowerCase(s.charAt(r--))) return false;
        }
        return true;
    }
}
```


#### 基础语法与算法思想

- `Character.isLetterOrDigit` 判断字母数字；回文比较可原地跳过无关字符。

---

## 126. 单词接龙 II (Hard)

按字典  `wordList`  完成从单词  `beginWord`  到单词  `endWord`  转化，一个表示此过程的  **转换序列**  是形式上像  `beginWord -> s1 -> s2 -> ... -> sk`  这样的单词序列，并满足：

每对相邻的单词之间仅有单个字母不同。
转换过程中的每个单词  `si` （ `1 <= i <= k` ）必须是字典  `wordList`  中的单词。注意， `beginWord`  不必是字典  `wordList`  中的单词。
 `sk == endWord` 

给你两个单词  `beginWord`  和  `endWord`  ，以及一个字典  `wordList`  。请你找出并返回所有从  `beginWord`  到  `endWord`  的  **最短转换序列**  ，如果不存在这样的转换序列，返回一个空列表。每个序列都应该以单词列表  `[beginWord, s1, s2, ..., sk]`  的形式返回。
 
 **示例 1：** 

```text
输入：beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
输出：[["hit","hot","dot","dog","cog"],["hit","hot","lot","log","cog"]]
解释：存在 2 种最短的转换序列：
"hit" -> "hot" -> "dot" -> "dog" -> "cog"
"hit" -> "hot" -> "lot" -> "log" -> "cog"
```

 **示例 2：** 

```text
输入：beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]
输出：[]
解释：endWord "cog" 不在字典 wordList 中，所以不存在符合要求的转换序列。
```

 
 **提示：** 

 `1 <= beginWord.length <= 5` 
 `endWord.length == beginWord.length` 
 `1 <= wordList.length <= 500` 
 `wordList[i].length == beginWord.length` 
 `beginWord` 、 `endWord`  和  `wordList[i]`  由小写英文字母组成
 `beginWord != endWord` 
 `wordList`  中的所有单词  **互不相同**

### Java 解法补充

#### 基础解法：DFS 枚举所有转换路径并取最短

算法思想：DFS 枚举所有转换路径并取最短，容易超时。

```java
class Solution {
    private java.util.List<java.util.List<String>> ans = new java.util.ArrayList<>();
    private int best = Integer.MAX_VALUE;

    public java.util.List<java.util.List<String>> findLadders(String beginWord, String endWord, java.util.List<String> wordList) {
        java.util.Set<String> dict = new java.util.HashSet<>(wordList);
        if (!dict.contains(endWord)) return ans;
        java.util.List<String> path = new java.util.ArrayList<>();
        java.util.Set<String> used = new java.util.HashSet<>();
        path.add(beginWord);
        used.add(beginWord);
        dfs(beginWord, endWord, dict, used, path);
        return ans;
    }

    private void dfs(String cur, String end, java.util.Set<String> dict, java.util.Set<String> used, java.util.List<String> path) {
        if (path.size() > best) return;
        if (cur.equals(end)) {
            if (path.size() < best) {
                best = path.size();
                ans.clear();
            }
            ans.add(new java.util.ArrayList<>(path));
            return;
        }
        for (String next : dict) {
            if (!used.contains(next) && oneDiff(cur, next)) {
                used.add(next);
                path.add(next);
                dfs(next, end, dict, used, path);
                path.remove(path.size() - 1);
                used.remove(next);
            }
        }
    }

    private boolean oneDiff(String a, String b) {
        int diff = 0;
        for (int i = 0; i < a.length(); i++) {
            if (a.charAt(i) != b.charAt(i) && ++diff > 1) return false;
        }
        return diff == 1;
    }
}
```

#### 资深解法：BFS 建最短层级和前驱关系

算法思想：BFS 建最短层级和前驱关系，再从终点回溯所有最短路径。


```java
class Solution {
    public java.util.List<java.util.List<String>> findLadders(String beginWord, String endWord, java.util.List<String> wordList) {
        java.util.Set<String> dict = new java.util.HashSet<>(wordList);
        java.util.List<java.util.List<String>> ans = new java.util.ArrayList<>();
        if (!dict.contains(endWord)) return ans;
        java.util.Map<String, java.util.List<String>> prev = new java.util.HashMap<>();
        java.util.Set<String> level = new java.util.HashSet<>();
        level.add(beginWord);
        dict.remove(beginWord);
        boolean found = false;
        while (!level.isEmpty() && !found) {
            java.util.Set<String> nextLevel = new java.util.HashSet<>();
            for (String w : level) dict.remove(w);
            for (String word : level) {
                char[] arr = word.toCharArray();
                for (int i = 0; i < arr.length; i++) {
                    char old = arr[i];
                    for (char c = 'a'; c <= 'z'; c++) {
                        arr[i] = c;
                        String next = new String(arr);
                        if (!dict.contains(next)) continue;
                        if (next.equals(endWord)) found = true;
                        nextLevel.add(next);
                        prev.computeIfAbsent(next, k -> new java.util.ArrayList<>()).add(word);
                    }
                    arr[i] = old;
                }
            }
            level = nextLevel;
        }
        backtrack(endWord, beginWord, prev, new java.util.ArrayList<>(), ans);
        return ans;
    }
    private void backtrack(String word, String begin, java.util.Map<String, java.util.List<String>> prev, java.util.List<String> path, java.util.List<java.util.List<String>> ans) {
        path.add(word);
        if (word.equals(begin)) {
            java.util.List<String> one = new java.util.ArrayList<>(path);
            java.util.Collections.reverse(one);
            ans.add(one);
        } else if (prev.containsKey(word)) for (String p : prev.get(word)) backtrack(p, begin, prev, path, ans);
        path.remove(path.size() - 1);
    }
}
```


#### 基础语法与算法思想

- BFS 保证最短层；前驱表用于回溯所有答案。

---

## 127. 单词接龙 (Hard)

字典  `wordList`  中从单词  `beginWord`  到  `endWord`  的  **转换序列** 是一个按下述规格形成的序列  `beginWord -> s1 -> s2 -> ... -> sk` ：

每一对相邻的单词只差一个字母。
 对于  `1 <= i <= k`  时，每个  `si`  都在  `wordList`  中。注意，  `beginWord`  不需要在  `wordList`  中。
 `sk == endWord` 

给你两个单词  `beginWord`  和  `endWord`  和一个字典  `wordList`  ，返回 从  `beginWord`  到  `endWord`  的  **最短转换序列**  中的  **单词数目**  。如果不存在这样的转换序列，返回  `0`  。
 

 **示例 1：** 

```text
输入：beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
输出：5
解释：一个最短转换序列是 "hit" -> "hot" -> "dot" -> "dog" -> "cog", 返回它的长度 5。
```

 **示例 2：** 

```text
输入：beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]
输出：0
解释：endWord "cog" 不在字典中，所以无法进行转换。
```

 
 **提示：** 

 `1 <= beginWord.length <= 10` 
 `endWord.length == beginWord.length` 
 `1 <= wordList.length <= 5000` 
 `wordList[i].length == beginWord.length` 
 `beginWord` 、 `endWord`  和  `wordList[i]`  由小写英文字母组成
 `beginWord != endWord` 
 `wordList`  中的所有字符串  **互不相同**

### Java 解法补充

#### 基础解法：BFS 每次枚举字典中只差一个字符的单词

算法思想：BFS 每次枚举字典中只差一个字符的单词。

```java
class Solution {
    public int ladderLength(String beginWord, String endWord, java.util.List<String> wordList) {
        java.util.Set<String> dict = new java.util.HashSet<>(wordList);
        if (!dict.contains(endWord)) return 0;
        java.util.Queue<String> q = new java.util.ArrayDeque<>();
        java.util.Set<String> visited = new java.util.HashSet<>();
        q.offer(beginWord);
        visited.add(beginWord);
        int step = 1;
        while (!q.isEmpty()) {
            int size = q.size();
            for (int i = 0; i < size; i++) {
                String cur = q.poll();
                if (cur.equals(endWord)) return step;
                for (String next : dict) {
                    if (!visited.contains(next) && oneDiff(cur, next)) {
                        visited.add(next);
                        q.offer(next);
                    }
                }
            }
            step++;
        }
        return 0;
    }

    private boolean oneDiff(String a, String b) {
        int diff = 0;
        for (int i = 0; i < a.length(); i++) {
            if (a.charAt(i) != b.charAt(i) && ++diff > 1) return false;
        }
        return diff == 1;
    }
}
```

#### 资深解法：对当前单词逐位替换 26 个字母生成邻居

算法思想：对当前单词逐位替换 26 个字母生成邻居。


```java
class Solution {
    public int ladderLength(String beginWord, String endWord, java.util.List<String> wordList) {
        java.util.Set<String> dict = new java.util.HashSet<>(wordList);
        if (!dict.contains(endWord)) return 0;
        java.util.Queue<String> q = new java.util.ArrayDeque<>();
        q.offer(beginWord);
        dict.remove(beginWord);
        int step = 1;
        while (!q.isEmpty()) {
            for (int s = q.size(); s > 0; s--) {
                char[] arr = q.poll().toCharArray();
                String cur = new String(arr);
                if (cur.equals(endWord)) return step;
                for (int i = 0; i < arr.length; i++) {
                    char old = arr[i];
                    for (char c = 'a'; c <= 'z'; c++) {
                        arr[i] = c;
                        String next = new String(arr);
                        if (dict.remove(next)) q.offer(next);
                    }
                    arr[i] = old;
                }
            }
            step++;
        }
        return 0;
    }
}
```


#### 基础语法与算法思想

- `dict.remove(next)` 同时判断存在并标记已访问。

---

## 128. 最长连续序列 (Medium)

给定一个未排序的整数数组  `nums`  ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。
请你设计并实现时间复杂度为  `O(n)`  的算法解决此问题。
 
 **示例 1：** 

```text
输入：nums = [100,4,200,1,3,2]
输出：4
解释：最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。
```

 **示例 2：** 

```text
输入：nums = [0,3,7,2,5,8,4,6,0,1]
输出：9
```

 **示例 3：** 

```text
输入：nums = [1,0,1,2]
输出：3
```

 
 **提示：** 

 `0 <= nums.length <= 105` 
 `-109 <= nums[i] <= 109`

### Java 解法补充

#### 基础解法：排序后统计连续段

算法思想：排序后统计连续段。

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        if (nums.length == 0) return 0;
        java.util.Arrays.sort(nums);
        int ans = 1, cur = 1;
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] == nums[i - 1]) continue;
            if (nums[i] == nums[i - 1] + 1) cur++;
            else cur = 1;
            ans = Math.max(ans, cur);
        }
        return ans;
    }
}
```

#### 资深解法：哈希集合只从序列起点 `x-1` 不存在的位置向后扩展

算法思想：哈希集合只从序列起点 `x-1` 不存在的位置向后扩展。


```java
class Solution {
    public int longestConsecutive(int[] nums) {
        java.util.Set<Integer> set = new java.util.HashSet<>();
        for (int x : nums) set.add(x);
        int ans = 0;
        for (int x : set) {
            if (!set.contains(x - 1)) {
                int y = x;
                while (set.contains(y)) y++;
                ans = Math.max(ans, y - x);
            }
        }
        return ans;
    }
}
```


#### 基础语法与算法思想

- 只从连续段起点扩展，保证总体 `O(n)`。

---

## 129. 求根节点到叶节点数字之和 (Medium)

给你一个二叉树的根节点  `root`  ，树中每个节点都存放有一个  `0`  到  `9`  之间的数字。

每条从根节点到叶节点的路径都代表一个数字：

例如，从根节点到叶节点的路径  `1 -> 2 -> 3`  表示数字  `123`  。

计算从根节点到叶节点生成的  **所有数字之和**  。
 **叶节点**  是指没有子节点的节点。
 
 **示例 1：** 

```text
输入：root = [1,2,3]
输出：25
解释：
从根到叶子节点路径 1->2 代表数字 12
从根到叶子节点路径 1->3 代表数字 13
因此，数字总和 = 12 + 13 = 25
```

 **示例 2：** 

```text
输入：root = [4,9,0,5,1]
输出：1026
解释：
从根到叶子节点路径 4->9->5 代表数字 495
从根到叶子节点路径 4->9->1 代表数字 491
从根到叶子节点路径 4->0 代表数字 40
因此，数字总和 = 495 + 491 + 40 = 1026
```

 
 **提示：** 

树中节点的数目在范围  `[1, 1000]`  内
 `0 <= Node.val <= 9` 
树的深度不超过  `10`

### Java 解法补充

#### 基础解法：DFS 保存路径字符串

算法思想：DFS 保存路径字符串，到叶子后转整数。

```java
class Solution {
    public int sumNumbers(TreeNode root) {
        java.util.List<String> paths = new java.util.ArrayList<>();
        dfs(root, "", paths);
        int ans = 0;
        for (String s : paths) ans += Integer.parseInt(s);
        return ans;
    }

    private void dfs(TreeNode node, String path, java.util.List<String> paths) {
        if (node == null) return;
        String next = path + node.val;
        if (node.left == null && node.right == null) {
            paths.add(next);
            return;
        }
        dfs(node.left, next, paths);
        dfs(node.right, next, paths);
    }
}
```

#### 资深解法：递归传当前数字 `cur = cur * 10 + val`

算法思想：递归传当前数字 `cur = cur * 10 + val`。


```java
class Solution {
    public int sumNumbers(TreeNode root) {
        return dfs(root, 0);
    }
    private int dfs(TreeNode node, int cur) {
        if (node == null) return 0;
        cur = cur * 10 + node.val;
        if (node.left == null && node.right == null) return cur;
        return dfs(node.left, cur) + dfs(node.right, cur);
    }
}
```


#### 基础语法与算法思想

- 根到叶路径题在叶子节点结算。

---

## 130. 被围绕的区域 (Medium)

给你一个  `m x n`  的矩阵  `board`  ，由若干字符  `'X'`  和  `'O'`  组成， **捕获**  所有  **被围绕的区域** ：

 **连接：** 一个单元格与水平或垂直方向上相邻的单元格连接。
 **区域：连接所有**  `'O'`  的单元格来形成一个区域。
 **围绕：** 如果一个区域中的所有  `'O'`  单元格都不在棋盘的边缘，则该区域被包围。这样的区域  **完全**  被  `'X'`  单元格包围。

通过  **原地**  将输入矩阵中的所有  `'O'`  替换为  `'X'`  来  **捕获被围绕的区域** 。你不需要返回任何值。

 
 **示例 1：** 

 **输入：** board = [['X','X','X','X'],['X','O','O','X'],['X','X','O','X'],['X','O','X','X']]
 **输出：** [['X','X','X','X'],['X','X','X','X'],['X','X','X','X'],['X','O','X','X']]
 **解释：** 

在上图中，底部的区域没有被捕获，因为它在 board 的边缘并且不能被围绕。

 **示例 2：** 

 **输入：** board = [['X']]
 **输出：** [['X']]

 
 **提示：** 

 `m == board.length` 
 `n == board[i].length` 
 `1 <= m, n <= 200` 
 `board[i][j]`  为  `'X'`  或  `'O'`

### Java 解法补充

#### 基础解法：对每个 `O` DFS 判断是否连到边界

算法思想：对每个 `O` DFS 判断是否连到边界。

```java
class Solution {
    public void solve(char[][] board) {
        int m = board.length, n = board[0].length;
        boolean[][] seen = new boolean[m][n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] == 'O' && !seen[i][j]) {
                    java.util.List<int[]> cells = new java.util.ArrayList<>();
                    boolean safe = dfs(board, i, j, seen, cells);
                    if (!safe) {
                        for (int[] c : cells) board[c[0]][c[1]] = 'X';
                    }
                }
            }
        }
    }

    private boolean dfs(char[][] board, int r, int c, boolean[][] seen, java.util.List<int[]> cells) {
        int m = board.length, n = board[0].length;
        if (r < 0 || r >= m || c < 0 || c >= n) return false;
        if (board[r][c] != 'O' || seen[r][c]) return false;
        seen[r][c] = true;
        cells.add(new int[]{r, c});
        boolean touchesBorder = r == 0 || r == m - 1 || c == 0 || c == n - 1;
        boolean down = dfs(board, r + 1, c, seen, cells);
        boolean up = dfs(board, r - 1, c, seen, cells);
        boolean right = dfs(board, r, c + 1, seen, cells);
        boolean left = dfs(board, r, c - 1, seen, cells);
        return touchesBorder || down || up || right || left;
    }
}
```

#### 资深解法：从边界 `O` 出发标记安全区域

算法思想：从边界 `O` 出发标记安全区域，剩余 `O` 翻成 `X`。


```java
class Solution {
    public void solve(char[][] board) {
        int m = board.length, n = board[0].length;
        for (int r = 0; r < m; r++) { mark(board, r, 0); mark(board, r, n - 1); }
        for (int c = 0; c < n; c++) { mark(board, 0, c); mark(board, m - 1, c); }
        for (int r = 0; r < m; r++) for (int c = 0; c < n; c++) {
            if (board[r][c] == 'O') board[r][c] = 'X';
            else if (board[r][c] == '#') board[r][c] = 'O';
        }
    }
    private void mark(char[][] b, int r, int c) {
        if (r < 0 || r == b.length || c < 0 || c == b[0].length || b[r][c] != 'O') return;
        b[r][c] = '#';
        mark(b, r + 1, c); mark(b, r - 1, c); mark(b, r, c + 1); mark(b, r, c - 1);
    }
}
```


#### 基础语法与算法思想

- 反向思考，边界连通的 `O` 不会被包围。

---

## 131. 分割回文串 (Medium)

给你一个字符串  `s` ，请你将  `s`  分割成一些 子串，使每个子串都是  **回文串**  。返回  `s`  所有可能的分割方案。
 
 **示例 1：** 

```text
输入：s = "aab"
输出：[["a","a","b"],["aa","b"]]
```

 **示例 2：** 

```text
输入：s = "a"
输出：[["a"]]
```

 
 **提示：** 

 `1 <= s.length <= 16` 
 `s`  仅由小写英文字母组成

### Java 解法补充

#### 基础解法：回溯枚举切分

算法思想：回溯枚举切分，每段实时判断回文。

```java
class Solution {
    public java.util.List<java.util.List<String>> partition(String s) {
        java.util.List<java.util.List<String>> ans = new java.util.ArrayList<>();
        backtrack(s, 0, new java.util.ArrayList<>(), ans);
        return ans;
    }

    private void backtrack(String s, int start, java.util.List<String> path, java.util.List<java.util.List<String>> ans) {
        if (start == s.length()) {
            ans.add(new java.util.ArrayList<>(path));
            return;
        }
        for (int end = start; end < s.length(); end++) {
            if (isPal(s, start, end)) {
                path.add(s.substring(start, end + 1));
                backtrack(s, end + 1, path, ans);
                path.remove(path.size() - 1);
            }
        }
    }

    private boolean isPal(String s, int l, int r) {
        while (l < r) {
            if (s.charAt(l++) != s.charAt(r--)) return false;
        }
        return true;
    }
}
```

#### 资深解法：预处理回文 DP 后回溯切分

算法思想：预处理回文 DP 后回溯切分。


```java
class Solution {
    public java.util.List<java.util.List<String>> partition(String s) {
        java.util.List<java.util.List<String>> ans = new java.util.ArrayList<>();
        dfs(s, 0, new java.util.ArrayList<>(), ans);
        return ans;
    }
    private void dfs(String s, int start, java.util.List<String> path, java.util.List<java.util.List<String>> ans) {
        if (start == s.length()) { ans.add(new java.util.ArrayList<>(path)); return; }
        for (int end = start; end < s.length(); end++) {
            if (!pal(s, start, end)) continue;
            path.add(s.substring(start, end + 1));
            dfs(s, end + 1, path, ans);
            path.remove(path.size() - 1);
        }
    }
    private boolean pal(String s, int l, int r) {
        while (l < r) if (s.charAt(l++) != s.charAt(r--)) return false;
        return true;
    }
}
```


#### 基础语法与算法思想

- 切分题回溯参数通常是下一个起点。

---

## 132. 分割回文串 II (Hard)

给你一个字符串  `s` ，请你将  `s`  分割成一些子串，使每个子串都是回文串。
返回符合要求的  **最少分割次数**  。

 
 **示例 1：** 

```text
输入：s = "aab"
输出：1
解释：只需一次分割就可将 s 分割成 ["aa","b"] 这样两个回文子串。
```

 **示例 2：** 

```text
输入：s = "a"
输出：0
```

 **示例 3：** 

```text
输入：s = "ab"
输出：1
```

 
 **提示：** 

 `1 <= s.length <= 2000` 
 `s`  仅由小写英文字母组成

### Java 解法补充

#### 基础解法：枚举所有回文切分取最少段数

算法思想：枚举所有回文切分取最少段数。

```java
class Solution {
    private int ans;

    public int minCut(String s) {
        ans = s.length() - 1;
        dfs(s, 0, 0);
        return ans;
    }

    private void dfs(String s, int start, int parts) {
        if (parts - 1 >= ans) return;
        if (start == s.length()) {
            ans = Math.min(ans, parts - 1);
            return;
        }
        for (int end = start; end < s.length(); end++) {
            if (isPal(s, start, end)) dfs(s, end + 1, parts + 1);
        }
    }

    private boolean isPal(String s, int l, int r) {
        while (l < r) {
            if (s.charAt(l++) != s.charAt(r--)) return false;
        }
        return true;
    }
}
```

#### 资深解法：预处理回文

算法思想：预处理回文，`dp[i]` 表示前 `i` 个字符最少切割数。


```java
class Solution {
    public int minCut(String s) {
        int n = s.length();
        boolean[][] pal = new boolean[n][n];
        for (int i = n - 1; i >= 0; i--) for (int j = i; j < n; j++)
            pal[i][j] = s.charAt(i) == s.charAt(j) && (j - i < 2 || pal[i + 1][j - 1]);
        int[] dp = new int[n + 1];
        java.util.Arrays.fill(dp, n);
        dp[0] = -1;
        for (int i = 1; i <= n; i++) for (int j = 0; j < i; j++)
            if (pal[j][i - 1]) dp[i] = Math.min(dp[i], dp[j] + 1);
        return dp[n];
    }
}
```


#### 基础语法与算法思想

- `dp[0] = -1` 让整个前缀回文时切割数为 0。

---

## 133. 克隆图 (Medium)

给你无向  **连通** 图中一个节点的引用，请你返回该图的  **深拷贝** （克隆）。
图中的每个节点都包含它的值  `val` （ `int` ） 和其邻居的列表（ `list[Node]` ）。

```text
class Node {
    public int val;
    public List<Node> neighbors;
}
```

 
 **测试用例格式：** 
简单起见，每个节点的值都和它的索引相同。例如，第一个节点值为 1（ `val = 1` ），第二个节点值为 2（ `val = 2` ），以此类推。该图在测试用例中使用邻接列表表示。
 **邻接列表**  是用于表示有限图的无序列表的集合。每个列表都描述了图中节点的邻居集。
给定节点将始终是图中的第一个节点（值为 1）。你必须将  **给定节点的拷贝** 作为对克隆图的引用返回。
 
 **示例 1：** 

```text
输入：adjList = [[2,4],[1,3],[2,4],[1,3]]
输出：[[2,4],[1,3],[2,4],[1,3]]
解释：
图中有 4 个节点。
节点 1 的值是 1，它有两个邻居：节点 2 和 4 。
节点 2 的值是 2，它有两个邻居：节点 1 和 3 。
节点 3 的值是 3，它有两个邻居：节点 2 和 4 。
节点 4 的值是 4，它有两个邻居：节点 1 和 3 。
```

 **示例 2：** 

```text
输入：adjList = [[]]
输出：[[]]
解释：输入包含一个空列表。该图仅仅只有一个值为 1 的节点，它没有任何邻居。
```

 **示例 3：** 

```text
输入：adjList = []
输出：[]
解释：这个图是空的，它不含任何节点。
```

 
 **提示：** 

这张图中的节点数在  `[0, 100]`  之间。
 `1 <= Node.val <= 100` 
每个节点值  `Node.val`  都是唯一的，
图中没有重复的边，也没有自环。
图是连通图，你可以从给定节点访问到所有节点。

### Java 解法补充

#### 基础解法：DFS 递归克隆节点和邻居

算法思想：DFS 递归克隆节点和邻居。

```java
class Solution {
    private Node[] copied = new Node[101];

    public Node cloneGraph(Node node) {
        if (node == null) return null;
        if (copied[node.val] != null) return copied[node.val];
        Node clone = new Node(node.val);
        copied[node.val] = clone;
        for (Node next : node.neighbors) {
            clone.neighbors.add(cloneGraph(next));
        }
        return clone;
    }
}
```

#### 资深解法：哈希表记录原节点到克隆节点映射

算法思想：哈希表记录原节点到克隆节点映射，避免重复克隆和死循环。


```java
class Solution {
    private java.util.Map<Node, Node> map = new java.util.HashMap<>();
    public Node cloneGraph(Node node) {
        if (node == null) return null;
        if (map.containsKey(node)) return map.get(node);
        Node copy = new Node(node.val, new java.util.ArrayList<>());
        map.put(node, copy);
        for (Node nei : node.neighbors) copy.neighbors.add(cloneGraph(nei));
        return copy;
    }
}
```


#### 基础语法与算法思想

- 图可能有环，克隆时必须先放入映射再递归邻居。

---

## 134. 加油站 (Medium)

在一条环路上有  `n`  个加油站，其中第  `i`  个加油站有汽油  `gas[i]`  升。
你有一辆油箱容量无限的的汽车，从第  `i`  个加油站开往第  `i+1`  个加油站需要消耗汽油  `cost[i]`  升。你从其中的一个加油站出发，开始时油箱为空。
给定两个整数数组  `gas`  和  `cost`  ，如果你可以按顺序绕环路行驶一周，则返回出发时加油站的编号，否则返回  `-1`  。如果存在解，则  **保证**  它是  **唯一**  的。
 
 **示例 1:** 

```text
输入: gas = [1,2,3,4,5], cost = [3,4,5,1,2]
输出: 3
解释:
从 3 号加油站(索引为 3 处)出发，可获得 4 升汽油。此时油箱有 = 0 + 4 = 4 升汽油
开往 4 号加油站，此时油箱有 4 - 1 + 5 = 8 升汽油
开往 0 号加油站，此时油箱有 8 - 2 + 1 = 7 升汽油
开往 1 号加油站，此时油箱有 7 - 3 + 2 = 6 升汽油
开往 2 号加油站，此时油箱有 6 - 4 + 3 = 5 升汽油
开往 3 号加油站，你需要消耗 5 升汽油，正好足够你返回到 3 号加油站。
因此，3 可为起始索引。
```

 **示例 2:** 

```text
输入: gas = [2,3,4], cost = [3,4,3]
输出: -1
解释:
你不能从 0 号或 1 号加油站出发，因为没有足够的汽油可以让你行驶到下一个加油站。
我们从 2 号加油站出发，可以获得 4 升汽油。 此时油箱有 = 0 + 4 = 4 升汽油
开往 0 号加油站，此时油箱有 4 - 3 + 2 = 3 升汽油
开往 1 号加油站，此时油箱有 3 - 3 + 3 = 3 升汽油
你无法返回 2 号加油站，因为返程需要消耗 4 升汽油，但是你的油箱只有 3 升汽油。
因此，无论怎样，你都不可能绕环路行驶一周。
```

 
 **提示:** 

 `n == gas.length == cost.length` 
 `1 <= n <= 105` 
 `0 <= gas[i], cost[i] <= 104` 
输入保证答案唯一。

### Java 解法补充

#### 基础解法：枚举每个起点模拟一圈

算法思想：枚举每个起点模拟一圈。

```java
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int n = gas.length;
        for (int start = 0; start < n; start++) {
            int tank = 0, count = 0;
            while (count < n) {
                int i = (start + count) % n;
                tank += gas[i] - cost[i];
                if (tank < 0) break;
                count++;
            }
            if (count == n) return start;
        }
        return -1;
    }
}
```

#### 资深解法：若从 `start` 到 `i` 油量为负

算法思想：若从 `start` 到 `i` 油量为负，则这些点都不能作为起点，从 `i+1` 重来。


```java
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int total = 0, tank = 0, start = 0;
        for (int i = 0; i < gas.length; i++) {
            int diff = gas[i] - cost[i];
            total += diff;
            tank += diff;
            if (tank < 0) { start = i + 1; tank = 0; }
        }
        return total >= 0 ? start : -1;
    }
}
```


#### 基础语法与算法思想

- 总油量不足必无解；局部失败区间内的点都不可能成功。

---

## 135. 分发糖果 (Hard)

`n`  个孩子站成一排。给你一个整数数组  `ratings`  表示每个孩子的评分。
你需要按照以下要求，给这些孩子分发糖果：

每个孩子至少分配到  `1`  个糖果。
相邻两个孩子中，评分更高的那个会获得更多的糖果。

请你给每个孩子分发糖果，计算并返回需要准备的  **最少糖果数目**  。
 
 **示例 1：** 

```text
输入：ratings = [1,0,2]
输出：5
解释：你可以分别给第一个、第二个、第三个孩子分发 2、1、2 颗糖果。
```

 **示例 2：** 

```text
输入：ratings = [1,2,2]
输出：4
解释：你可以分别给第一个、第二个、第三个孩子分发 1、2、1 颗糖果。
     第三个孩子只得到 1 颗糖果，这满足题面中的两个条件。
```

 
 **提示：** 

 `n == ratings.length` 
 `1 <= n <= 2 * 104` 
 `0 <= ratings[i] <= 2 * 104`

### Java 解法补充

#### 基础解法：反复调整不满足条件的孩子糖果数直到稳定

算法思想：反复调整不满足条件的孩子糖果数直到稳定。

```java
class Solution {
    public int candy(int[] ratings) {
        int n = ratings.length;
        int[] candy = new int[n];
        java.util.Arrays.fill(candy, 1);
        boolean changed = true;
        while (changed) {
            changed = false;
            for (int i = 0; i < n; i++) {
                if (i > 0 && ratings[i] > ratings[i - 1] && candy[i] <= candy[i - 1]) {
                    candy[i] = candy[i - 1] + 1;
                    changed = true;
                }
                if (i + 1 < n && ratings[i] > ratings[i + 1] && candy[i] <= candy[i + 1]) {
                    candy[i] = candy[i + 1] + 1;
                    changed = true;
                }
            }
        }
        int ans = 0;
        for (int x : candy) ans += x;
        return ans;
    }
}
```

#### 资深解法：左右两次扫描

算法思想：左右两次扫描，分别满足左邻和右邻约束。


```java
class Solution {
    public int candy(int[] ratings) {
        int n = ratings.length;
        int[] candies = new int[n];
        java.util.Arrays.fill(candies, 1);
        for (int i = 1; i < n; i++) if (ratings[i] > ratings[i - 1]) candies[i] = candies[i - 1] + 1;
        for (int i = n - 2; i >= 0; i--) if (ratings[i] > ratings[i + 1]) candies[i] = Math.max(candies[i], candies[i + 1] + 1);
        int sum = 0;
        for (int c : candies) sum += c;
        return sum;
    }
}
```


#### 基础语法与算法思想

- 两个方向的局部约束分开满足，再取较大值。

---

## 136. 只出现一次的数字 (Easy)

给你一个  **非空**  整数数组  `nums`  ，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。
你必须设计并实现线性时间复杂度的算法来解决此问题，且该算法只使用常量额外空间。

 
 **示例 1 ：** 

 **输入：** nums = [2,2,1]
 **输出：** 1

 **示例 2 ：** 

 **输入：** nums = [4,1,2,1,2]
 **输出：** 4

 **示例 3 ：** 

 **输入：** nums = [1]
 **输出：** 1

 
 **提示：** 

 `1 <= nums.length <= 3 * 104` 
 `-3 * 104 <= nums[i] <= 3 * 104` 
除了某个元素只出现一次以外，其余每个元素均出现两次。

### Java 解法补充

#### 基础解法：哈希表统计频次

算法思想：哈希表统计频次。

```java
class Solution {
    public int singleNumber(int[] nums) {
        java.util.Map<Integer, Integer> count = new java.util.HashMap<>();
        for (int x : nums) count.put(x, count.getOrDefault(x, 0) + 1);
        for (int x : nums) {
            if (count.get(x) == 1) return x;
        }
        return 0;
    }
}
```

#### 资深解法：异或所有数字

算法思想：异或所有数字，成对数字抵消为 0。


```java
class Solution {
    public int singleNumber(int[] nums) {
        int ans = 0;
        for (int x : nums) ans ^= x;
        return ans;
    }
}
```


#### 基础语法与算法思想

- `a ^ a = 0`，`a ^ 0 = a`。

---

## 137. 只出现一次的数字 II (Medium)

给你一个整数数组  `nums`  ，除某个元素仅出现  **一次**  外，其余每个元素都恰出现  **三次 。** 请你找出并返回那个只出现了一次的元素。
你必须设计并实现线性时间复杂度的算法且使用常数级空间来解决此问题。
 
 **示例 1：** 

```text
输入：nums = [2,2,3,2]
输出：3
```

 **示例 2：** 

```text
输入：nums = [0,1,0,1,0,1,99]
输出：99
```

 
 **提示：** 

 `1 <= nums.length <= 3 * 104` 
 `-231 <= nums[i] <= 231 - 1` 
 `nums`  中，除某个元素仅出现  **一次**  外，其余每个元素都恰出现  **三次**

### Java 解法补充

#### 基础解法：哈希表统计频次后找 1 次

算法思想：哈希表统计频次后找 1 次。

```java
class Solution {
    public int singleNumber(int[] nums) {
        java.util.Map<Integer, Integer> count = new java.util.HashMap<>();
        for (int x : nums) count.put(x, count.getOrDefault(x, 0) + 1);
        for (int x : nums) {
            if (count.get(x) == 1) return x;
        }
        return 0;
    }
}
```

#### 资深解法：逐位统计 1 的个数

算法思想：逐位统计 1 的个数，对 3 取余得到答案位。


```java
class Solution {
    public int singleNumber(int[] nums) {
        int ans = 0;
        for (int bit = 0; bit < 32; bit++) {
            int count = 0;
            for (int x : nums) count += (x >> bit) & 1;
            if (count % 3 != 0) ans |= 1 << bit;
        }
        return ans;
    }
}
```


#### 基础语法与算法思想

- 位统计天然支持负数的补码表示。

---

## 138. 随机链表的复制 (Medium)

给你一个长度为  `n`  的链表，每个节点包含一个额外增加的随机指针  `random`  ，该指针可以指向链表中的任何节点或空节点。
构造这个链表的  **深拷贝** 。 深拷贝应该正好由  `n`  个  **全新**  节点组成，其中每个新节点的值都设为其对应的原节点的值。新节点的  `next`  指针和  `random`  指针也都应指向复制链表中的新节点，并使原链表和复制链表中的这些指针能够表示相同的链表状态。 **复制链表中的指针都不应指向原链表中的节点** 。
例如，如果原链表中有  `X`  和  `Y`  两个节点，其中  `X.random --> Y`  。那么在复制链表中对应的两个节点  `x`  和  `y`  ，同样有  `x.random --> y`  。
返回复制链表的头节点。
用一个由  `n`  个节点组成的链表来表示输入/输出中的链表。每个节点用一个  `[val, random_index]`  表示：

 `val` ：一个表示  `Node.val`  的整数。
 `random_index` ：随机指针指向的节点索引（范围从  `0`  到  `n-1` ）；如果不指向任何节点，则为   `null`  。

你的代码  **只**  接受原链表的头节点  `head`  作为传入参数。
 
 **示例 1：** 

```text
输入：head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
输出：[[7,null],[13,0],[11,4],[10,2],[1,0]]
```

 **示例 2：** 

```text
输入：head = [[1,1],[2,1]]
输出：[[1,1],[2,1]]
```

 **示例 3：** 
 **** 

```text
输入：head = [[3,null],[3,0],[3,null]]
输出：[[3,null],[3,0],[3,null]]
```

 
 **提示：** 

 `0 <= n <= 1000` 
 `-104 <= Node.val <= 104` 
 `Node.random`  为  `null`  或指向链表中的节点。

### Java 解法补充

#### 基础解法：哈希表记录原节点到新节点映射

算法思想：哈希表记录原节点到新节点映射。

```java
class Solution {
    public Node copyRandomList(Node head) {
        java.util.Map<Node, Node> map = new java.util.HashMap<>();
        Node cur = head;
        while (cur != null) {
            map.put(cur, new Node(cur.val));
            cur = cur.next;
        }
        cur = head;
        while (cur != null) {
            Node copy = map.get(cur);
            copy.next = map.get(cur.next);
            copy.random = map.get(cur.random);
            cur = cur.next;
        }
        return map.get(head);
    }
}
```

#### 资深解法：原链表中穿插克隆节点

算法思想：原链表中穿插克隆节点，再设置 random，最后拆分。


```java
class Solution {
    public Node copyRandomList(Node head) {
        if (head == null) return null;
        java.util.Map<Node, Node> map = new java.util.HashMap<>();
        for (Node cur = head; cur != null; cur = cur.next) map.put(cur, new Node(cur.val));
        for (Node cur = head; cur != null; cur = cur.next) {
            map.get(cur).next = map.get(cur.next);
            map.get(cur).random = map.get(cur.random);
        }
        return map.get(head);
    }
}
```


#### 基础语法与算法思想

- `HashMap` 可以用对象引用作为键；`map.get(null)` 返回 `null`。

---

## 139. 单词拆分 (Medium)

给你一个字符串  `s`  和一个字符串列表  `wordDict`  作为字典。如果可以利用字典中出现的一个或多个单词拼接出  `s`  则返回  `true` 。
 **注意：** 不要求字典中出现的单词全部都使用，并且字典中的单词可以重复使用。
 
 **示例 1：** 

```text
输入: s = "leetcode", wordDict = ["leet", "code"]
输出: true
解释: 返回 true 因为 "leetcode" 可以由 "leet" 和 "code" 拼接成。
```

 **示例 2：** 

```text
输入: s = "applepenapple", wordDict = ["apple", "pen"]
输出: true
解释: 返回 true 因为 "applepenapple" 可以由 "apple" "pen" "apple" 拼接成。
     注意，你可以重复使用字典中的单词。
```

 **示例 3：** 

```text
输入: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
输出: false
```

 
 **提示：** 

 `1 <= s.length <= 300` 
 `1 <= wordDict.length <= 1000` 
 `1 <= wordDict[i].length <= 20` 
 `s`  和  `wordDict[i]`  仅由小写英文字母组成
 `wordDict`  中的所有字符串  **互不相同**

### Java 解法补充

#### 基础解法：DFS 枚举切分点并记忆化

算法思想：DFS 枚举切分点并记忆化。

```java
class Solution {
    public boolean wordBreak(String s, java.util.List<String> wordDict) {
        java.util.Set<String> dict = new java.util.HashSet<>(wordDict);
        Boolean[] memo = new Boolean[s.length()];
        return dfs(s, 0, dict, memo);
    }

    private boolean dfs(String s, int start, java.util.Set<String> dict, Boolean[] memo) {
        if (start == s.length()) return true;
        if (memo[start] != null) return memo[start];
        for (int end = start + 1; end <= s.length(); end++) {
            if (dict.contains(s.substring(start, end)) && dfs(s, end, dict, memo)) {
                memo[start] = true;
                return true;
            }
        }
        memo[start] = false;
        return false;
    }
}
```

#### 资深解法：DP

算法思想：DP，`dp[i]` 表示前 `i` 个字符能否被字典切分。


```java
class Solution {
    public boolean wordBreak(String s, java.util.List<String> wordDict) {
        java.util.Set<String> set = new java.util.HashSet<>(wordDict);
        boolean[] dp = new boolean[s.length() + 1];
        dp[0] = true;
        for (int i = 1; i <= s.length(); i++) {
            for (int j = 0; j < i; j++) {
                if (dp[j] && set.contains(s.substring(j, i))) { dp[i] = true; break; }
            }
        }
        return dp[s.length()];
    }
}
```


#### 基础语法与算法思想

- 字符串切分 DP 常用前缀可达状态。

---

## 140. 单词拆分 II (Hard)

给定一个字符串  `s`  和一个字符串字典  `wordDict`  ，在字符串  `s`  中增加空格来构建一个句子，使得句子中所有的单词都在词典中。 **以任意顺序**  返回所有这些可能的句子。
 **注意：** 词典中的同一个单词可能在分段中被重复使用多次。
 
 **示例 1：** 

```text
输入:s = "catsanddog", wordDict = ["cat","cats","and","sand","dog"]
输出:["cats and dog","cat sand dog"]
```

 **示例 2：** 

```text
输入:s = "pineapplepenapple", wordDict = ["apple","pen","applepen","pine","pineapple"]
输出:["pine apple pen apple","pineapple pen apple","pine applepen apple"]
解释: 注意你可以重复使用字典中的单词。
```

 **示例 3：** 

```text
输入:s = "catsandog", wordDict = ["cats","dog","sand","and","cat"]
输出:[]
```

 
 **提示：** 

 `1 <= s.length <= 20` 
 `1 <= wordDict.length <= 1000` 
 `1 <= wordDict[i].length <= 10` 
 `s`  和  `wordDict[i]`  仅有小写英文字母组成
 `wordDict`  中所有字符串都  **不同**

### Java 解法补充

#### 基础解法：DFS 枚举所有切分句子

算法思想：DFS 枚举所有切分句子。

```java
class Solution {
    public java.util.List<String> wordBreak(String s, java.util.List<String> wordDict) {
        java.util.List<String> ans = new java.util.ArrayList<>();
        java.util.Set<String> dict = new java.util.HashSet<>(wordDict);
        dfs(s, 0, dict, new java.util.ArrayList<>(), ans);
        return ans;
    }

    private void dfs(String s, int start, java.util.Set<String> dict, java.util.List<String> path, java.util.List<String> ans) {
        if (start == s.length()) {
            ans.add(String.join(" ", path));
            return;
        }
        for (int end = start + 1; end <= s.length(); end++) {
            String word = s.substring(start, end);
            if (dict.contains(word)) {
                path.add(word);
                dfs(s, end, dict, path, ans);
                path.remove(path.size() - 1);
            }
        }
    }
}
```

#### 资深解法：记忆化 `start -> 句子列表`

算法思想：记忆化 `start -> 句子列表`，避免重复求后缀答案。


```java
class Solution {
    private java.util.Set<String> dict;
    private java.util.Map<Integer, java.util.List<String>> memo = new java.util.HashMap<>();
    public java.util.List<String> wordBreak(String s, java.util.List<String> wordDict) {
        dict = new java.util.HashSet<>(wordDict);
        return dfs(s, 0);
    }
    private java.util.List<String> dfs(String s, int start) {
        if (memo.containsKey(start)) return memo.get(start);
        java.util.List<String> ans = new java.util.ArrayList<>();
        if (start == s.length()) ans.add("");
        for (int end = start + 1; end <= s.length(); end++) {
            String word = s.substring(start, end);
            if (!dict.contains(word)) continue;
            for (String tail : dfs(s, end)) ans.add(tail.isEmpty() ? word : word + " " + tail);
        }
        memo.put(start, ans);
        return ans;
    }
}
```


#### 基础语法与算法思想

- 返回空串作为拼接终点，可统一处理最后一个单词。

---

## 141. 环形链表 (Easy)

给你一个链表的头节点  `head`  ，判断链表中是否有环。
如果链表中有某个节点，可以通过连续跟踪  `next`  指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数  `pos`  来表示链表尾连接到链表中的位置（索引从 0 开始）。 **注意： `pos`  不作为参数进行传递** 。仅仅是为了标识链表的实际情况。
如果链表中存在环 ，则返回  `true`  。 否则，返回  `false`  。
 
 **示例 1：** 

```text
输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。
```

 **示例 2：** 

```text
输入：head = [1,2], pos = 0
输出：true
解释：链表中有一个环，其尾部连接到第一个节点。
```

 **示例 3：** 

```text
输入：head = [1], pos = -1
输出：false
解释：链表中没有环。
```

 
 **提示：** 

链表中节点的数目范围是  `[0, 104]` 
 `-105 <= Node.val <= 105` 
 `pos`  为  `-1`  或者链表中的一个  **有效索引**  。

 
 **进阶：** 你能用  `O(1)` （即，常量）内存解决此问题吗？

### Java 解法补充

#### 基础解法：集合记录访问过的节点

算法思想：集合记录访问过的节点。

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        java.util.Set<ListNode> seen = new java.util.HashSet<>();
        while (head != null) {
            if (seen.contains(head)) return true;
            seen.add(head);
            head = head.next;
        }
        return false;
    }
}
```

#### 资深解法：快慢指针

算法思想：快慢指针，相遇则有环。


```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        ListNode slow = head, fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (slow == fast) return true;
        }
        return false;
    }
}
```


#### 基础语法与算法思想

- 快指针每次走两步，若有环必追上慢指针。

---

## 142. 环形链表 II (Medium)

给定一个链表的头节点   `head`  ，返回链表开始入环的第一个节点。 如果链表无环，则返回  `null` 。
如果链表中有某个节点，可以通过连续跟踪  `next`  指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数  `pos`  来表示链表尾连接到链表中的位置（ **索引从 0 开始** ）。如果  `pos`  是  `-1` ，则在该链表中没有环。 **注意： `pos`  不作为参数进行传递** ，仅仅是为了标识链表的实际情况。
 **不允许修改** 链表。

 
 **示例 1：** 

```text
输入：head = [3,2,0,-4], pos = 1
输出：返回索引为 1 的链表节点
解释：链表中有一个环，其尾部连接到第二个节点。
```

 **示例 2：** 

```text
输入：head = [1,2], pos = 0
输出：返回索引为 0 的链表节点
解释：链表中有一个环，其尾部连接到第一个节点。
```

 **示例 3：** 

```text
输入：head = [1], pos = -1
输出：返回 null
解释：链表中没有环。
```

 
 **提示：** 

链表中节点的数目范围在范围  `[0, 104]`  内
 `-105 <= Node.val <= 105` 
 `pos`  的值为  `-1`  或者链表中的一个有效索引

 
 **进阶：** 你是否可以使用  `O(1)`  空间解决此题？

### Java 解法补充

#### 基础解法：集合保存访问节点

算法思想：集合保存访问节点，第一次重复就是入环点。

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        java.util.Set<ListNode> seen = new java.util.HashSet<>();
        while (head != null) {
            if (seen.contains(head)) return head;
            seen.add(head);
            head = head.next;
        }
        return null;
    }
}
```

#### 资深解法：快慢指针相遇后

算法思想：快慢指针相遇后，一个指针回到头节点，两者同步走，相遇处为入环点。


```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode slow = head, fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (slow == fast) {
                ListNode p = head;
                while (p != slow) { p = p.next; slow = slow.next; }
                return p;
            }
        }
        return null;
    }
}
```


#### 基础语法与算法思想

- Floyd 判圈还能通过距离关系定位入口。

---

## 143. 重排链表 (Medium)

给定一个单链表  `L`  的头节点  `head`  ，单链表  `L`  表示为：

```text
L0 → L1 → … → Ln - 1 → Ln
```

请将其重新排列后变为：

```text
L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …
```

不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。
 
 **示例 1：** 

```text
输入：head = [1,2,3,4]
输出：[1,4,2,3]
```

 **示例 2：** 

```text
输入：head = [1,2,3,4,5]
输出：[1,5,2,4,3]
```

 
 **提示：** 

链表的长度范围为  `[1, 5 * 104]` 
 `1 <= node.val <= 1000`

### Java 解法补充

#### 基础解法：把节点放入数组

算法思想：把节点放入数组，双指针按首尾顺序重连。

```java
class Solution {
    public void reorderList(ListNode head) {
        java.util.List<ListNode> list = new java.util.ArrayList<>();
        ListNode cur = head;
        while (cur != null) {
            list.add(cur);
            cur = cur.next;
        }
        int left = 0, right = list.size() - 1;
        while (left < right) {
            list.get(left).next = list.get(right);
            left++;
            if (left == right) break;
            list.get(right).next = list.get(left);
            right--;
        }
        if (!list.isEmpty()) list.get(left).next = null;
    }
}
```

#### 资深解法：找中点、反转后半段、交替合并

算法思想：找中点、反转后半段、交替合并。


```java
class Solution {
    public void reorderList(ListNode head) {
        if (head == null || head.next == null) return;
        ListNode slow = head, fast = head;
        while (fast.next != null && fast.next.next != null) { slow = slow.next; fast = fast.next.next; }
        ListNode second = reverse(slow.next);
        slow.next = null;
        ListNode first = head;
        while (second != null) {
            ListNode a = first.next, b = second.next;
            first.next = second; second.next = a;
            first = a; second = b;
        }
    }
    private ListNode reverse(ListNode head) {
        ListNode prev = null;
        while (head != null) { ListNode next = head.next; head.next = prev; prev = head; head = next; }
        return prev;
    }
}
```


#### 基础语法与算法思想

- 链表重排通常拆成“找中点 + 反转 + 合并”。

---

## 144. 二叉树的前序遍历 (Easy)

给你二叉树的根节点  `root`  ，返回它节点值的  **前序**  遍历。
 
 **示例 1：** 

 **输入：** root = [1,null,2,3]
 **输出：** [1,2,3]
 **解释：** 

 **示例 2：** 

 **输入：** root = [1,2,3,4,5,null,8,null,null,6,7,9]
 **输出：** [1,2,4,5,6,7,3,8,9]
 **解释：** 

 **示例 3：** 

 **输入：** root = []
 **输出：** []

 **示例 4：** 

 **输入：** root = [1]
 **输出：** [1]

 
 **提示：** 

树中节点数目在范围  `[0, 100]`  内
 `-100 <= Node.val <= 100` 

 
 **进阶：** 递归算法很简单，你可以通过迭代算法完成吗？

### Java 解法补充

#### 基础解法：递归根、左、右

算法思想：递归根、左、右。

```java
class Solution {
    public java.util.List<Integer> preorderTraversal(TreeNode root) {
        java.util.List<Integer> ans = new java.util.ArrayList<>();
        dfs(root, ans);
        return ans;
    }

    private void dfs(TreeNode node, java.util.List<Integer> ans) {
        if (node == null) return;
        ans.add(node.val);
        dfs(node.left, ans);
        dfs(node.right, ans);
    }
}
```

#### 资深解法：栈迭代

算法思想：栈迭代，先压右再压左。


```java
class Solution {
    public java.util.List<Integer> preorderTraversal(TreeNode root) {
        java.util.List<Integer> ans = new java.util.ArrayList<>();
        if (root == null) return ans;
        java.util.Deque<TreeNode> stack = new java.util.ArrayDeque<>();
        stack.push(root);
        while (!stack.isEmpty()) {
            TreeNode node = stack.pop();
            ans.add(node.val);
            if (node.right != null) stack.push(node.right);
            if (node.left != null) stack.push(node.left);
        }
        return ans;
    }
}
```


#### 基础语法与算法思想

- 栈后进先出，先压右才能先处理左。

---

## 145. 二叉树的后序遍历 (Easy)

给你一棵二叉树的根节点  `root`  ，返回其节点值的  **后序遍历** 。
 
 **示例 1：** 

 **输入：** root = [1,null,2,3]
 **输出：** [3,2,1]
 **解释：** 

 **示例 2：** 

 **输入：** root = [1,2,3,4,5,null,8,null,null,6,7,9]
 **输出：** [4,6,7,5,2,9,8,3,1]
 **解释：** 

 **示例 3：** 

 **输入：** root = []
 **输出：** []

 **示例 4：** 

 **输入：** root = [1]
 **输出：** [1]

 
 **提示：** 

树中节点的数目在范围  `[0, 100]`  内
 `-100 <= Node.val <= 100` 

 
 **进阶：** 递归算法很简单，你可以通过迭代算法完成吗？

### Java 解法补充

#### 基础解法：递归左、右、根

算法思想：递归左、右、根。

```java
class Solution {
    public java.util.List<Integer> postorderTraversal(TreeNode root) {
        java.util.List<Integer> ans = new java.util.ArrayList<>();
        dfs(root, ans);
        return ans;
    }

    private void dfs(TreeNode node, java.util.List<Integer> ans) {
        if (node == null) return;
        dfs(node.left, ans);
        dfs(node.right, ans);
        ans.add(node.val);
    }
}
```

#### 资深解法：栈按根、右、左收集

算法思想：栈按根、右、左收集，再反转得到左、右、根。


```java
class Solution {
    public java.util.List<Integer> postorderTraversal(TreeNode root) {
        java.util.LinkedList<Integer> ans = new java.util.LinkedList<>();
        if (root == null) return ans;
        java.util.Deque<TreeNode> stack = new java.util.ArrayDeque<>();
        stack.push(root);
        while (!stack.isEmpty()) {
            TreeNode node = stack.pop();
            ans.addFirst(node.val);
            if (node.left != null) stack.push(node.left);
            if (node.right != null) stack.push(node.right);
        }
        return ans;
    }
}
```


#### 基础语法与算法思想

- `addFirst` 可把根右左反向变成左右根。

---

## 146. LRU 缓存 (Medium)

请你设计并实现一个满足  LRU (最近最少使用) 缓存 约束的数据结构。
实现  `LRUCache`  类：

 `LRUCache(int capacity)`  以  **正整数**  作为容量  `capacity`  初始化 LRU 缓存
 `int get(int key)`  如果关键字  `key`  存在于缓存中，则返回关键字的值，否则返回  `-1`  。
 `void put(int key, int value)`  如果关键字  `key`  已经存在，则变更其数据值  `value`  ；如果不存在，则向缓存中插入该组  `key-value`  。如果插入操作导致关键字数量超过  `capacity`  ，则应该  **逐出**  最久未使用的关键字。

函数  `get`  和  `put`  必须以  `O(1)`  的平均时间复杂度运行。

 
 **示例：** 

```text
输入
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
输出
[null, null, null, 1, null, -1, null, -1, 3, 4]

解释
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // 缓存是 {1=1}
lRUCache.put(2, 2); // 缓存是 {1=1, 2=2}
lRUCache.get(1);    // 返回 1
lRUCache.put(3, 3); // 该操作会使得关键字 2 作废，缓存是 {1=1, 3=3}
lRUCache.get(2);    // 返回 -1 (未找到)
lRUCache.put(4, 4); // 该操作会使得关键字 1 作废，缓存是 {4=4, 3=3}
lRUCache.get(1);    // 返回 -1 (未找到)
lRUCache.get(3);    // 返回 3
lRUCache.get(4);    // 返回 4
```

 
 **提示：** 

 `1 <= capacity <= 3000` 
 `0 <= key <= 10000` 
 `0 <= value <= 105` 
最多调用  `2 * 105`  次  `get`  和  `put`

### Java 解法补充

#### 基础解法：用列表维护最近使用顺序

算法思想：用列表维护最近使用顺序，查找和移动为 `O(n)`。

```java
class LRUCache {
    private int capacity;
    private java.util.Map<Integer, Integer> map = new java.util.HashMap<>();
    private java.util.List<Integer> order = new java.util.ArrayList<>();

    public LRUCache(int capacity) {
        this.capacity = capacity;
    }

    public int get(int key) {
        if (!map.containsKey(key)) return -1;
        order.remove(Integer.valueOf(key));
        order.add(key);
        return map.get(key);
    }

    public void put(int key, int value) {
        if (map.containsKey(key)) {
            map.put(key, value);
            order.remove(Integer.valueOf(key));
            order.add(key);
            return;
        }
        if (order.size() == capacity) {
            int old = order.remove(0);
            map.remove(old);
        }
        map.put(key, value);
        order.add(key);
    }
}
```

#### 资深解法：`LinkedHashMap` 开启访问顺序并重写删除 eldest

算法思想：`LinkedHashMap` 开启访问顺序并重写删除 eldest。


```java
class LRUCache extends java.util.LinkedHashMap<Integer, Integer> {
    private final int capacity;
    public LRUCache(int capacity) {
        super(capacity, 0.75f, true);
        this.capacity = capacity;
    }
    public int get(int key) {
        return super.getOrDefault(key, -1);
    }
    public void put(int key, int value) {
        super.put(key, value);
    }
    protected boolean removeEldestEntry(java.util.Map.Entry<Integer, Integer> eldest) {
        return size() > capacity;
    }
}
```


#### 基础语法与算法思想

- `LinkedHashMap` 的 access-order 正好表达最近使用顺序。

---

## 147. 对链表进行插入排序 (Medium)

给定单个链表的头  `head`  ，使用  **插入排序**  对链表进行排序，并返回 排序后链表的头 。
 **插入排序**  算法的步骤:

插入排序是迭代的，每次只移动一个元素，直到所有元素可以形成一个有序的输出列表。
每次迭代中，插入排序只从输入数据中移除一个待排序的元素，找到它在序列中适当的位置，并将其插入。
重复直到所有输入数据插入完为止。

下面是插入排序算法的一个图形示例。部分排序的列表(黑色)最初只包含列表中的第一个元素。每次迭代时，从输入数据中删除一个元素(红色)，并就地插入已排序的列表中。
对链表进行插入排序。

 
 **示例 1：** 

```text
输入: head = [4,2,1,3]
输出: [1,2,3,4]
```

 **示例 2：** 

```text
输入: head = [-1,5,3,4,0]
输出: [-1,0,3,4,5]
```

 
 **提示：** 

列表中的节点数在  `[1, 5000]` 范围内
 `-5000 <= Node.val <= 5000`

### Java 解法补充

#### 基础解法：把值放入数组排序后写回

算法思想：把值放入数组排序后写回。

```java
class Solution {
    public ListNode insertionSortList(ListNode head) {
        java.util.List<Integer> values = new java.util.ArrayList<>();
        ListNode cur = head;
        while (cur != null) {
            values.add(cur.val);
            cur = cur.next;
        }
        java.util.Collections.sort(values);
        cur = head;
        int i = 0;
        while (cur != null) {
            cur.val = values.get(i++);
            cur = cur.next;
        }
        return head;
    }
}
```

#### 资深解法：维护已排序链表

算法思想：维护已排序链表，逐个把原节点插入正确位置。


```java
class Solution {
    public ListNode insertionSortList(ListNode head) {
        ListNode dummy = new ListNode(Integer.MIN_VALUE);
        while (head != null) {
            ListNode next = head.next;
            ListNode p = dummy;
            while (p.next != null && p.next.val < head.val) p = p.next;
            head.next = p.next;
            p.next = head;
            head = next;
        }
        return dummy.next;
    }
}
```


#### 基础语法与算法思想

- 插入排序链表时要先保存 `next`，避免丢失未处理部分。

---

## 148. 排序链表 (Medium)

给你链表的头结点  `head`  ，请将其按  **升序**  排列并返回  **排序后的链表**  。

 
 **示例 1：** 

```text
输入：head = [4,2,1,3]
输出：[1,2,3,4]
```

 **示例 2：** 

```text
输入：head = [-1,5,3,4,0]
输出：[-1,0,3,4,5]
```

 **示例 3：** 

```text
输入：head = []
输出：[]
```

 
 **提示：** 

链表中节点的数目在范围  `[0, 5 * 104]`  内
 `-105 <= Node.val <= 105` 

 
 **进阶：** 你可以在  `O(n log n)`  时间复杂度和常数级空间复杂度下，对链表进行排序吗？

### Java 解法补充

#### 基础解法：把值放入数组排序后写回链表

算法思想：把值放入数组排序后写回链表。

```java
class Solution {
    public ListNode sortList(ListNode head) {
        java.util.List<Integer> values = new java.util.ArrayList<>();
        ListNode cur = head;
        while (cur != null) {
            values.add(cur.val);
            cur = cur.next;
        }
        java.util.Collections.sort(values);
        cur = head;
        int i = 0;
        while (cur != null) {
            cur.val = values.get(i++);
            cur = cur.next;
        }
        return head;
    }
}
```

#### 资深解法：归并排序链表

算法思想：归并排序链表，快慢指针切分，中间断开。


```java
class Solution {
    public ListNode sortList(ListNode head) {
        if (head == null || head.next == null) return head;
        ListNode slow = head, fast = head.next;
        while (fast != null && fast.next != null) { slow = slow.next; fast = fast.next.next; }
        ListNode mid = slow.next;
        slow.next = null;
        return merge(sortList(head), sortList(mid));
    }
    private ListNode merge(ListNode a, ListNode b) {
        ListNode dummy = new ListNode(0), tail = dummy;
        while (a != null && b != null) {
            if (a.val <= b.val) { tail.next = a; a = a.next; } else { tail.next = b; b = b.next; }
            tail = tail.next;
        }
        tail.next = a == null ? b : a;
        return dummy.next;
    }
}
```


#### 基础语法与算法思想

- 链表排序适合归并，因为合并链表是 `O(1)` 额外空间连接。

---

## 149. 直线上最多的点数 (Hard)

给你一个数组  `points`  ，其中  `points[i] = [xi, yi]`  表示  **X-Y**  平面上的一个点。求最多有多少个点在同一条直线上。
 
 **示例 1：** 

```text
输入：points = [[1,1],[2,2],[3,3]]
输出：3
```

 **示例 2：** 

```text
输入：points = [[1,1],[3,2],[5,3],[4,1],[2,3],[1,4]]
输出：4
```

 
 **提示：** 

 `1 <= points.length <= 300` 
 `points[i].length == 2` 
 `-104 <= xi, yi <= 104` 
 `points`  中的所有点  **互不相同**

### Java 解法补充

#### 基础解法：枚举两点确定直线

算法思想：枚举两点确定直线，再统计所有点是否在线上。

```java
class Solution {
    public int maxPoints(int[][] points) {
        int n = points.length;
        if (n <= 2) return n;
        int ans = 2;
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                int count = 2;
                for (int k = 0; k < n; k++) {
                    if (k != i && k != j && sameLine(points[i], points[j], points[k])) {
                        count++;
                    }
                }
                ans = Math.max(ans, count);
            }
        }
        return ans;
    }

    private boolean sameLine(int[] a, int[] b, int[] c) {
        long x1 = b[0] - a[0], y1 = b[1] - a[1];
        long x2 = c[0] - a[0], y2 = c[1] - a[1];
        return x1 * y2 == x2 * y1;
    }
}
```

#### 资深解法：固定一个点

算法思想：固定一个点，用归一化斜率哈希统计相同方向的点数。


```java
class Solution {
    public int maxPoints(int[][] points) {
        int n = points.length, ans = 1;
        for (int i = 0; i < n; i++) {
            java.util.Map<String, Integer> map = new java.util.HashMap<>();
            for (int j = i + 1; j < n; j++) {
                int dx = points[j][0] - points[i][0], dy = points[j][1] - points[i][1];
                int g = gcd(Math.abs(dx), Math.abs(dy));
                dx /= g; dy /= g;
                if (dx == 0) dy = 1;
                else if (dy == 0) dx = 1;
                else if (dx < 0) { dx = -dx; dy = -dy; }
                String key = dx + "/" + dy;
                map.put(key, map.getOrDefault(key, 1) + 1);
                ans = Math.max(ans, map.get(key));
            }
        }
        return ans;
    }
    private int gcd(int a, int b) { return b == 0 ? a : gcd(b, a % b); }
}
```


#### 基础语法与算法思想

- 斜率用约分后的 `(dx,dy)` 表示，避免浮点误差。

---

## 150. 逆波兰表达式求值 (Medium)

给你一个字符串数组  `tokens`  ，表示一个根据 逆波兰表示法 表示的算术表达式。
请你计算该表达式。返回一个表示表达式值的整数。
 **注意：** 

有效的算符为  `'+'` 、 `'-'` 、 `'*'`  和  `'/'`  。
每个操作数（运算对象）都可以是一个整数或者另一个表达式。
两个整数之间的除法总是  **向零截断**  。
表达式中不含除零运算。
输入是一个根据逆波兰表示法表示的算术表达式。
答案及所有中间计算结果可以用  **32 位**  整数表示。

 
 **示例 1：** 

```text
输入：tokens = ["2","1","+","3","*"]
输出：9
解释：该算式转化为常见的中缀算术表达式为：((2 + 1) * 3) = 9
```

 **示例 2：** 

```text
输入：tokens = ["4","13","5","/","+"]
输出：6
解释：该算式转化为常见的中缀算术表达式为：(4 + (13 / 5)) = 6
```

 **示例 3：** 

```text
输入：tokens = ["10","6","9","3","+","-11","*","/","*","17","+","5","+"]
输出：22
解释：该算式转化为常见的中缀算术表达式为：
  ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22
```

 
 **提示：** 

 `1 <= tokens.length <= 104` 
 `tokens[i]`  是一个算符（ `"+"` 、 `"-"` 、 `"*"`  或  `"/"` ），或是在范围  `[-200, 200]`  内的一个整数

 
 **逆波兰表达式：** 
逆波兰表达式是一种后缀表达式，所谓后缀就是指算符写在后面。

平常使用的算式则是一种中缀表达式，如  `( 1 + 2 ) * ( 3 + 4 )`  。
该算式的逆波兰表达式写法为  `( ( 1 2 + ) ( 3 4 + ) * )`  。

逆波兰表达式主要有以下两个优点：

去掉括号后表达式无歧义，上式即便写成  `1 2 + 3 4 + *` 也可以依据次序计算出正确结果。
适合用栈操作运算：遇到数字则入栈；遇到算符则取出栈顶两个数字进行计算，并将结果压入栈中

### Java 解法补充

#### 基础解法：递归解析后缀表达式

算法思想：递归解析后缀表达式。

```java
class Solution {
    public int evalRPN(String[] tokens) {
        int[] index = {tokens.length - 1};
        return eval(tokens, index);
    }

    private int eval(String[] tokens, int[] index) {
        String t = tokens[index[0]--];
        if (!isOp(t)) return Integer.parseInt(t);
        int right = eval(tokens, index);
        int left = eval(tokens, index);
        if (t.equals("+")) return left + right;
        if (t.equals("-")) return left - right;
        if (t.equals("*")) return left * right;
        return left / right;
    }

    private boolean isOp(String s) {
        return s.equals("+") || s.equals("-") || s.equals("*") || s.equals("/");
    }
}
```

#### 资深解法：栈

算法思想：栈，遇到数字入栈，遇到运算符弹出两个数计算。


```java
class Solution {
    public int evalRPN(String[] tokens) {
        java.util.Deque<Integer> stack = new java.util.ArrayDeque<>();
        for (String t : tokens) {
            if ("+-*/".contains(t) && t.length() == 1) {
                int b = stack.pop(), a = stack.pop();
                if (t.equals("+")) stack.push(a + b);
                else if (t.equals("-")) stack.push(a - b);
                else if (t.equals("*")) stack.push(a * b);
                else stack.push(a / b);
            } else stack.push(Integer.parseInt(t));
        }
        return stack.pop();
    }
}
```


#### 基础语法与算法思想

- 注意弹栈顺序，先弹出的是右操作数。

---
