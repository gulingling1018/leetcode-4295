# LeetCode 题目合集 Part 15

## 421. 数组中两个数的最大异或值 (Medium)

给你一个整数数组  `nums`  ，返回  `nums[i] XOR nums[j]`  的最大运算结果，其中  `0 ≤ i ≤ j < n`  。
 

 **示例 1：** 

```text
输入：nums = [3,10,5,25,2,8]
输出：28
解释：最大运算结果是 5 XOR 25 = 28.
```

 **示例 2：** 

```text
输入：nums = [14,70,53,83,49,91,36,80,92,51,66,70]
输出：127
```

 
 **提示：** 

 `1 <= nums.length <= 2 * 105` 
 `0 <= nums[i] <= 231 - 1`

### Java 解法补充

#### 基础解法：枚举所有数对

算法思想：直接枚举两个下标，计算异或值并维护最大值。写法直观，但无法通过大数据。

```java
class Solution {
    public int findMaximumXOR(int[] nums) {
        int ans = 0;
        for (int i = 0; i < nums.length; i++) {
            for (int j = i; j < nums.length; j++) {
                ans = Math.max(ans, nums[i] ^ nums[j]);
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(1)`。

#### 资深解法：按位贪心加前缀集合

算法思想：从高位到低位尝试把答案当前位设为 1。若存在两个数的当前前缀异或能得到候选答案，说明这一位可以保留为 1。

```java
import java.util.HashSet;
import java.util.Set;

class Solution {
    public int findMaximumXOR(int[] nums) {
        int ans = 0;
        int mask = 0;
        for (int bit = 30; bit >= 0; bit--) {
            mask |= 1 << bit;
            Set<Integer> prefixes = new HashSet<>();
            for (int num : nums) {
                prefixes.add(num & mask);
            }
            int candidate = ans | (1 << bit);
            for (int prefix : prefixes) {
                if (prefixes.contains(prefix ^ candidate)) {
                    ans = candidate;
                    break;
                }
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(31n)`，空间 `O(n)`。

#### 基础语法与算法思想

- `^` 表示按位异或，相同为 0，不同为 1。
- 从高位贪心是因为高位对数值大小影响最大。
- 核心思想：最大异或值可以逐位构造，并用前缀集合验证候选值是否可达。

---

## 422. 有效的单词方块 (Easy)

暂无内容描述。

### Java 解法补充

#### 基础解法：逐格比较

算法思想：单词方块要求第 `i` 行第 `j` 列等于第 `j` 行第 `i` 列。枚举所有位置，任何越界或字符不等都返回 `false`。

```java
import java.util.List;

class Solution {
    public boolean validWordSquare(List<String> words) {
        for (int i = 0; i < words.size(); i++) {
            for (int j = 0; j < words.get(i).length(); j++) {
                if (j >= words.size() || i >= words.get(j).length()) {
                    return false;
                }
                if (words.get(i).charAt(j) != words.get(j).charAt(i)) {
                    return false;
                }
            }
        }
        return true;
    }
}
```

复杂度：时间 `O(totalChars)`，空间 `O(1)`。

#### 资深解法：构造列字符串对比

算法思想：逐列构造列字符串，再与对应行比较。代码更贴近“行列相等”的定义，便于调试数据不规则的情况。

```java
import java.util.List;

class Solution {
    public boolean validWordSquare(List<String> words) {
        for (int col = 0; col < words.size(); col++) {
            StringBuilder builder = new StringBuilder();
            for (String word : words) {
                if (col < word.length()) {
                    builder.append(word.charAt(col));
                }
            }
            if (!builder.toString().equals(words.get(col))) {
                return false;
            }
        }
        return true;
    }
}
```

复杂度：时间 `O(totalChars)`，空间 `O(maxLen)`。

#### 基础语法与算法思想

- 不规则字符串数组要先判断下标是否越界。
- 方块类题常检查 `matrix[i][j] == matrix[j][i]`。
- 核心思想：行列互为转置，长度也必须一致。

---

## 423. 从英文中重建数字 (Medium)

给你一个字符串  `s`  ，其中包含字母顺序打乱的用英文单词表示的若干数字（ `0-9` ）。按  **升序**  返回原始的数字。
 
 **示例 1：** 

```text
输入：s = "owoztneoer"
输出："012"
```

 **示例 2：** 

```text
输入：s = "fviefuro"
输出："45"
```

 
 **提示：** 

 `1 <= s.length <= 105` 
 `s[i]`  为  `["e","g","f","i","h","o","n","s","r","u","t","w","v","x","z"]`  这些字符之一
 `s`  保证是一个符合题目要求的字符串

### Java 解法补充

#### 基础解法：按独特字母计数

算法思想：某些英文数字有独特字母，例如 `z` 只在 zero 中出现，`w` 只在 two 中出现。先找独特数字，再从剩余字母中推出其他数字数量。

```java
class Solution {
    public String originalDigits(String s) {
        int[] letters = new int[26];
        for (int i = 0; i < s.length(); i++) {
            letters[s.charAt(i) - 'a']++;
        }

        int[] count = new int[10];
        count[0] = letters['z' - 'a'];
        count[2] = letters['w' - 'a'];
        count[4] = letters['u' - 'a'];
        count[6] = letters['x' - 'a'];
        count[8] = letters['g' - 'a'];
        count[3] = letters['h' - 'a'] - count[8];
        count[5] = letters['f' - 'a'] - count[4];
        count[7] = letters['s' - 'a'] - count[6];
        count[1] = letters['o' - 'a'] - count[0] - count[2] - count[4];
        count[9] = letters['i' - 'a'] - count[5] - count[6] - count[8];

        StringBuilder ans = new StringBuilder();
        for (int digit = 0; digit <= 9; digit++) {
            for (int i = 0; i < count[digit]; i++) {
                ans.append(digit);
            }
        }
        return ans.toString();
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 资深解法：规则表驱动扣减

算法思想：把“识别字母、数字、单词”做成固定顺序表。按顺序识别一个数字后，从字母频次中扣掉对应英文单词，最后升序输出。

```java
class Solution {
    public String originalDigits(String s) {
        int[] letters = new int[26];
        for (char c : s.toCharArray()) letters[c - 'a']++;

        char[] keys = {'z', 'w', 'u', 'x', 'g', 'h', 'f', 's', 'o', 'i'};
        int[] digits = {0, 2, 4, 6, 8, 3, 5, 7, 1, 9};
        String[] words = {"zero", "two", "four", "six", "eight",
                "three", "five", "seven", "one", "nine"};
        int[] count = new int[10];

        for (int i = 0; i < keys.length; i++) {
            int times = letters[keys[i] - 'a'];
            count[digits[i]] = times;
            for (char c : words[i].toCharArray()) {
                letters[c - 'a'] -= times;
            }
        }

        StringBuilder ans = new StringBuilder();
        for (int digit = 0; digit <= 9; digit++) {
            ans.append(String.valueOf(digit).repeat(count[digit]));
        }
        return ans.toString();
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 固定小写字母统计用 `int[26]`。
- 英文数字可以按独特字母建立推导顺序。
- 核心思想：先处理有唯一标识的数字，再处理被它们影响后的剩余数字。

---

## 424. 替换后的最长重复字符 (Medium)

给你一个字符串  `s`  和一个整数  `k`  。你可以选择字符串中的任一字符，并将其更改为任何其他大写英文字符。该操作最多可执行  `k`  次。
在执行上述操作后，返回 包含相同字母的最长子字符串的长度。
 
 **示例 1：** 

```text
输入：s = "ABAB", k = 2
输出：4
解释：用两个'A'替换为两个'B',反之亦然。
```

 **示例 2：** 

```text
输入：s = "AABABBA", k = 1
输出：4
解释：
将中间的一个'A'替换为'B',字符串变为 "AABBBBA"。
子串 "BBBB" 有最长重复字母, 答案为 4。
可能存在其他的方法来得到同样的结果。
```

 
 **提示：** 

 `1 <= s.length <= 105` 
 `s`  仅由大写英文字母组成
 `0 <= k <= s.length`

### Java 解法补充

#### 基础解法：枚举目标字符滑动窗口

算法思想：枚举最终要变成的字母 `A..Z`。对每个目标字母，用窗口维护非目标字母数量不超过 `k`，更新最大长度。

```java
class Solution {
    public int characterReplacement(String s, int k) {
        int ans = 0;
        for (char target = 'A'; target <= 'Z'; target++) {
            int left = 0;
            int changes = 0;
            for (int right = 0; right < s.length(); right++) {
                if (s.charAt(right) != target) changes++;
                while (changes > k) {
                    if (s.charAt(left) != target) changes--;
                    left++;
                }
                ans = Math.max(ans, right - left + 1);
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(26n)`，空间 `O(1)`。

#### 资深解法：维护窗口最大频次

算法思想：窗口长度减去窗口内最高频字符数量，就是需要替换的字符数。若超过 `k`，左边界右移。

```java
class Solution {
    public int characterReplacement(String s, int k) {
        int[] count = new int[26];
        int left = 0;
        int maxFreq = 0;
        int ans = 0;
        for (int right = 0; right < s.length(); right++) {
            int index = s.charAt(right) - 'A';
            count[index]++;
            maxFreq = Math.max(maxFreq, count[index]);
            while (right - left + 1 - maxFreq > k) {
                count[s.charAt(left) - 'A']--;
                left++;
            }
            ans = Math.max(ans, right - left + 1);
        }
        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 统一成某个字符所需替换次数等于窗口长度减去该字符频次。
- `maxFreq` 即使不回退也不影响答案正确性，因为窗口只会在可形成更优时扩张。
- 核心思想：滑动窗口适合“最长连续区间 + 可容忍 k 次修改”的问题。

---

## 425. 单词方块 (Hard)

暂无内容描述。

### Java 解法补充

#### 基础解法：回溯时线性查找前缀

算法思想：逐行构造单词方块。第 `row` 行需要匹配由前面各行第 `row` 列组成的前缀，直接遍历所有单词找前缀匹配者继续递归。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<List<String>> wordSquares(String[] words) {
        List<List<String>> ans = new ArrayList<>();
        for (String word : words) {
            List<String> path = new ArrayList<>();
            path.add(word);
            backtrack(words, path, ans, word.length());
        }
        return ans;
    }

    private void backtrack(String[] words, List<String> path, List<List<String>> ans, int size) {
        if (path.size() == size) {
            ans.add(new ArrayList<>(path));
            return;
        }
        StringBuilder prefix = new StringBuilder();
        int row = path.size();
        for (String word : path) prefix.append(word.charAt(row));
        for (String word : words) {
            if (word.startsWith(prefix.toString())) {
                path.add(word);
                backtrack(words, path, ans, size);
                path.remove(path.size() - 1);
            }
        }
    }
}
```

复杂度：指数级，前缀查找每次 `O(words.length * wordLength)`。

#### 资深解法：前缀表加速候选查询

算法思想：预处理“前缀 -> 拥有该前缀的单词列表”。回溯时直接从前缀表拿候选，避免每层扫描所有单词。

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

class Solution {
    private final Map<String, List<String>> prefixMap = new HashMap<>();

    public List<List<String>> wordSquares(String[] words) {
        int size = words[0].length();
        for (String word : words) {
            for (int i = 0; i <= size; i++) {
                String prefix = word.substring(0, i);
                prefixMap.computeIfAbsent(prefix, key -> new ArrayList<>()).add(word);
            }
        }

        List<List<String>> ans = new ArrayList<>();
        backtrack(new ArrayList<>(), size, ans);
        return ans;
    }

    private void backtrack(List<String> path, int size, List<List<String>> ans) {
        if (path.size() == size) {
            ans.add(new ArrayList<>(path));
            return;
        }
        int row = path.size();
        StringBuilder prefix = new StringBuilder();
        for (String word : path) prefix.append(word.charAt(row));
        for (String candidate : prefixMap.getOrDefault(prefix.toString(), new ArrayList<>())) {
            path.add(candidate);
            backtrack(path, size, ans);
            path.remove(path.size() - 1);
        }
    }
}
```

复杂度：预处理 `O(words.length * L^2)`，搜索复杂度取决于候选数量，空间 `O(words.length * L^2)`。

#### 基础语法与算法思想

- 回溯路径中的第几行，决定下一行必须满足的列前缀。
- `startsWith` 可做基础前缀判断。
- 核心思想：高频前缀查询要预处理索引表。

---

## 426. 将二叉搜索树转化为排序的双向链表 (Medium)

暂无内容描述。

### Java 解法补充

#### 基础解法：中序遍历后统一连接

算法思想：BST 的中序遍历是升序。先把节点按中序放入列表，再遍历列表连接前后指针，并把首尾相连形成循环链表。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public Node treeToDoublyList(Node root) {
        if (root == null) return null;
        List<Node> nodes = new ArrayList<>();
        inorder(root, nodes);
        int n = nodes.size();
        for (int i = 0; i < n; i++) {
            nodes.get(i).left = nodes.get((i - 1 + n) % n);
            nodes.get(i).right = nodes.get((i + 1) % n);
        }
        return nodes.get(0);
    }

    private void inorder(Node node, List<Node> nodes) {
        if (node == null) return;
        inorder(node.left, nodes);
        nodes.add(node);
        inorder(node.right, nodes);
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 资深解法：中序遍历原地串联

算法思想：中序遍历过程中维护 `prev` 和 `head`。访问当前节点时，把 `prev` 与当前节点互相连接；遍历完成后再连接头尾。

```java
class Solution {
    private Node head;
    private Node prev;

    public Node treeToDoublyList(Node root) {
        if (root == null) return null;
        inorder(root);
        head.left = prev;
        prev.right = head;
        return head;
    }

    private void inorder(Node node) {
        if (node == null) return;
        inorder(node.left);
        if (prev == null) {
            head = node;
        } else {
            prev.right = node;
            node.left = prev;
        }
        prev = node;
        inorder(node.right);
    }
}
```

复杂度：时间 `O(n)`，空间 `O(h)`。

#### 基础语法与算法思想

- BST 中序遍历天然有序。
- 双向链表需要同时维护前驱和后继指针。
- 核心思想：树转有序链表时，把遍历顺序直接作为链表顺序。

---

## 427. 建立四叉树 (Medium)

给你一个  `n * n`  矩阵  `grid`  ，矩阵由若干  `0`  和  `1`  组成。请你用四叉树表示该矩阵  `grid`  。
你需要返回能表示矩阵  `grid`  的 四叉树 的根结点。
四叉树数据结构中，每个内部节点只有四个子节点。此外，每个节点都有两个属性：

 `val` ：储存叶子结点所代表的区域的值。1 对应  **True** ，0 对应  **False** 。注意，当  `isLeaf`  为  **False** 时，你可以把  **True**  或者  **False**  赋值给节点，两种值都会被判题机制  **接受**  。
 `isLeaf` : 当这个节点是一个叶子结点时为  **True** ，如果它有 4 个子节点则为  **False**  。

```text
class Node {
    public boolean val;
    public boolean isLeaf;
    public Node topLeft;
    public Node topRight;
    public Node bottomLeft;
    public Node bottomRight;
}
```

我们可以按以下步骤为二维区域构建四叉树：

如果当前网格的值相同（即，全为  `0`  或者全为  `1` ），将  `isLeaf`  设为 True ，将  `val`  设为网格相应的值，并将四个子节点都设为 Null 然后停止。
如果当前网格的值不同，将  `isLeaf`  设为 False， 将  `val`  设为任意值，然后如下图所示，将当前网格划分为四个子网格。
使用适当的子网格递归每个子节点。

如果你想了解更多关于四叉树的内容，可以参考 百科 。
 **四叉树格式：** 
你不需要阅读本节来解决这个问题。只有当你想了解输出格式时才会这样做。输出为使用层序遍历后四叉树的序列化形式，其中  `null`  表示路径终止符，其下面不存在节点。
它与二叉树的序列化非常相似。唯一的区别是节点以列表形式表示  `[isLeaf, val]`  。
如果  `isLeaf`  或者  `val`  的值为 True ，则表示它在列表  `[isLeaf, val]`  中的值为  **1**  ；如果  `isLeaf`  或者  `val`  的值为 False ，则表示值为  **0** 。
 
 **示例 1：** 

```text
输入：grid = [[0,1],[1,0]]
输出：[[0,1],[1,0],[1,1],[1,1],[1,0]]
解释：此示例的解释如下：
请注意，在下面四叉树的图示中，0 表示 false，1 表示 True 。
```

 **示例 2：** 

```text
输入：grid = [[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0],[1,1,1,1,1,1,1,1],[1,1,1,1,1,1,1,1],[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0],[1,1,1,1,0,0,0,0]]
输出：[[0,1],[1,1],[0,1],[1,1],[1,0],null,null,null,null,[1,0],[1,0],[1,1],[1,1]]
解释：网格中的所有值都不相同。我们将网格划分为四个子网格。
topLeft，bottomLeft 和 bottomRight 均具有相同的值。
topRight 具有不同的值，因此我们将其再分为 4 个子网格，这样每个子网格都具有相同的值。
解释如下图所示：
```

 
 **提示：** 

 `n == grid.length == grid[i].length` 
 `n == 2x`  其中  `0 <= x <= 6`

### Java 解法补充

#### 基础解法：递归检查区域是否同值

算法思想：对当前正方形区域扫描，若所有值相同就创建叶子节点；否则把区域四等分，递归构造四个子节点。

```java
class Solution {
    public Node construct(int[][] grid) {
        return build(grid, 0, 0, grid.length);
    }

    private Node build(int[][] grid, int row, int col, int size) {
        if (same(grid, row, col, size)) {
            return new Node(grid[row][col] == 1, true);
        }
        int half = size / 2;
        Node node = new Node(true, false);
        node.topLeft = build(grid, row, col, half);
        node.topRight = build(grid, row, col + half, half);
        node.bottomLeft = build(grid, row + half, col, half);
        node.bottomRight = build(grid, row + half, col + half, half);
        return node;
    }

    private boolean same(int[][] grid, int row, int col, int size) {
        int value = grid[row][col];
        for (int i = row; i < row + size; i++) {
            for (int j = col; j < col + size; j++) {
                if (grid[i][j] != value) return false;
            }
        }
        return true;
    }
}
```

复杂度：最坏时间 `O(n^2 log n)`，空间 `O(log n)`。

#### 资深解法：二维前缀和判断同值

算法思想：用二维前缀和快速计算区域内 1 的数量。如果数量为 0 或区域面积，说明整块同值；否则继续递归拆分。

```java
class Solution {
    private int[][] prefix;

    public Node construct(int[][] grid) {
        int n = grid.length;
        prefix = new int[n + 1][n + 1];
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= n; j++) {
                prefix[i][j] = grid[i - 1][j - 1] + prefix[i - 1][j]
                        + prefix[i][j - 1] - prefix[i - 1][j - 1];
            }
        }
        return build(0, 0, n);
    }

    private Node build(int row, int col, int size) {
        int ones = sum(row, col, size);
        if (ones == 0 || ones == size * size) {
            return new Node(ones > 0, true);
        }
        int half = size / 2;
        Node node = new Node(true, false);
        node.topLeft = build(row, col, half);
        node.topRight = build(row, col + half, half);
        node.bottomLeft = build(row + half, col, half);
        node.bottomRight = build(row + half, col + half, half);
        return node;
    }

    private int sum(int row, int col, int size) {
        int r2 = row + size;
        int c2 = col + size;
        return prefix[r2][c2] - prefix[row][c2] - prefix[r2][col] + prefix[row][col];
    }
}
```

复杂度：构建前缀和 `O(n^2)`，递归节点数 `O(nodes)`，空间 `O(n^2)`。

#### 基础语法与算法思想

- 四叉树每个非叶节点固定拆成四个象限。
- 二维前缀和可快速判断区域内 1 的个数。
- 核心思想：递归建树时，停止条件是当前区域可以被一个叶子表示。

---

## 428. 序列化和反序列化 N 叉树 (Hard)

暂无内容描述。

### Java 解法补充

#### 基础解法：前序遍历带孩子数量

算法思想：序列化时对每个节点写入“节点值,孩子数量”，然后递归写入所有孩子。反序列化时按同样顺序读取，先建节点，再递归读取指定数量的孩子。

```java
import java.util.ArrayList;
import java.util.Arrays;

class Codec {
    public String serialize(Node root) {
        StringBuilder builder = new StringBuilder();
        write(root, builder);
        return builder.toString();
    }

    private void write(Node node, StringBuilder builder) {
        if (node == null) {
            builder.append("#,");
            return;
        }
        builder.append(node.val).append(',').append(node.children.size()).append(',');
        for (Node child : node.children) {
            write(child, builder);
        }
    }

    public Node deserialize(String data) {
        String[] parts = data.split(",");
        int[] index = {0};
        return read(parts, index);
    }

    private Node read(String[] parts, int[] index) {
        if (parts[index[0]].equals("#")) {
            index[0]++;
            return null;
        }
        int val = Integer.parseInt(parts[index[0]++]);
        int size = Integer.parseInt(parts[index[0]++]);
        Node node = new Node(val, new ArrayList<>());
        for (int i = 0; i < size; i++) {
            node.children.add(read(parts, index));
        }
        return node;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 资深解法：层序序列化加分隔符

算法思想：按层序写节点值，每个节点的孩子列表后追加一个 `#` 分隔。反序列化时队列保存等待填充孩子的父节点。

```java
import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.Queue;

class Codec {
    public String serialize(Node root) {
        if (root == null) return "";
        StringBuilder builder = new StringBuilder();
        Queue<Node> queue = new ArrayDeque<>();
        queue.offer(root);
        builder.append(root.val).append(',');
        while (!queue.isEmpty()) {
            Node node = queue.poll();
            for (Node child : node.children) {
                builder.append(child.val).append(',');
                queue.offer(child);
            }
            builder.append("#,");
        }
        return builder.toString();
    }

    public Node deserialize(String data) {
        if (data.isEmpty()) return null;
        String[] parts = data.split(",");
        Node root = new Node(Integer.parseInt(parts[0]), new ArrayList<>());
        Queue<Node> queue = new ArrayDeque<>();
        queue.offer(root);
        int index = 1;
        while (!queue.isEmpty()) {
            Node parent = queue.poll();
            while (index < parts.length && !parts[index].equals("#")) {
                Node child = new Node(Integer.parseInt(parts[index++]), new ArrayList<>());
                parent.children.add(child);
                queue.offer(child);
            }
            index++;
        }
        return root;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 基础语法与算法思想

- N 叉树序列化必须保存每个节点有多少孩子，或用分隔符标记孩子列表结束。
- `split(",")` 适合解析简单分隔格式。
- 核心思想：序列化和反序列化必须使用完全一致的遍历协议。

---

## 429. N 叉树的层序遍历 (Medium)

给定一个 N 叉树，返回其节点值的层序遍历。（即从左到右，逐层遍历）。
树的序列化输入是用层序遍历，每组子节点都由 null 值分隔（参见示例）。
 
 **示例 1：** 

```text
输入：root = [1,null,3,2,4,null,5,6]
输出：[[1],[3,2,4],[5,6]]
```

 **示例 2：** 

```text
输入：root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
输出：[[1],[2,3,4,5],[6,7,8,9,10],[11,12,13],[14]]
```

 
 **提示：** 

树的高度不会超过  `1000` 
树的节点总数在  `[0, 104]`  之间

### Java 解法补充

#### 基础解法：DFS 按深度收集

算法思想：递归遍历节点，参数记录当前深度。第一次到达某个深度时创建列表，再把当前节点值加入对应层。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<List<Integer>> levelOrder(Node root) {
        List<List<Integer>> ans = new ArrayList<>();
        dfs(root, 0, ans);
        return ans;
    }

    private void dfs(Node node, int depth, List<List<Integer>> ans) {
        if (node == null) return;
        if (depth == ans.size()) {
            ans.add(new ArrayList<>());
        }
        ans.get(depth).add(node.val);
        for (Node child : node.children) {
            dfs(child, depth + 1, ans);
        }
    }
}
```

复杂度：时间 `O(n)`，空间 `O(h)`，不计答案。

#### 资深解法：队列层序遍历

算法思想：层序遍历天然按层处理。每轮先记录当前队列大小，只弹出这一层的节点，并把它们的孩子加入队列。

```java
import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.List;
import java.util.Queue;

class Solution {
    public List<List<Integer>> levelOrder(Node root) {
        List<List<Integer>> ans = new ArrayList<>();
        if (root == null) return ans;

        Queue<Node> queue = new ArrayDeque<>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            List<Integer> level = new ArrayList<>();
            for (int i = 0; i < size; i++) {
                Node node = queue.poll();
                level.add(node.val);
                for (Node child : node.children) {
                    queue.offer(child);
                }
            }
            ans.add(level);
        }
        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(w)`，`w` 为最大层宽。

#### 基础语法与算法思想

- 队列适合按层遍历树。
- `queue.size()` 在每轮开始时固定当前层节点数。
- 核心思想：层序结果最好按层批量弹出队列。

---

## 430. 扁平化多级双向链表 (Medium)

你会得到一个双链表，其中包含的节点有一个下一个指针、一个前一个指针和一个额外的  **子指针**  。这个子指针可能指向一个单独的双向链表，也包含这些特殊的节点。这些子列表可以有一个或多个自己的子列表，以此类推，以生成如下面的示例所示的  **多层数据结构**  。
给定链表的头节点 head ，将链表  **扁平化**  ，以便所有节点都出现在单层双链表中。让  `curr`  是一个带有子列表的节点。子列表中的节点应该出现在 **扁平化列表** 中的  `curr`   **之后**  和  `curr.next`   **之前**  。
返回 扁平列表的  `head`  。列表中的节点必须将其  **所有**  子指针设置为  `null`  。
 
 **示例 1：** 

```text
输入：head = [1,2,3,4,5,6,null,null,null,7,8,9,10,null,null,11,12]
输出：[1,2,3,7,8,11,12,9,10,4,5,6]
解释：输入的多级列表如上图所示。
扁平化后的链表如下图：
```

 **示例 2：** 

```text
输入：head = [1,2,null,3]
输出：[1,3,2]
解释：输入的多级列表如上图所示。
扁平化后的链表如下图：
```

 **示例 3：** 

```text
输入：head = []
输出：[]
说明：输入中可能存在空列表。
```

 
 **提示：** 

节点数目不超过  `1000` 
 `1 <= Node.val <= 105` 

 
 **如何表示测试用例中的多级链表？** 
以  **示例 1**  为例：

```text
1---2---3---4---5---6--NULL
         |
         7---8---9---10--NULL
             |
             11--12--NULL
```

序列化其中的每一级之后：

```text
[1,2,3,4,5,6,null]
[7,8,9,10,null]
[11,12,null]
```

为了将每一级都序列化到一起，我们需要每一级中添加值为 null 的元素，以表示没有节点连接到上一级的上级节点。

```text
[1,2,3,4,5,6,null]
[null,null,7,8,9,10,null]
[null,11,12,null]
```

合并所有序列化结果，并去除末尾的 null 。

```text
[1,2,3,4,5,6,null,null,null,7,8,9,10,null,null,11,12]
```

### Java 解法补充

#### 基础解法：DFS 收集后重新连接

算法思想：按“节点、child、next”的顺序 DFS 收集所有节点，再顺序重连 `prev/next`，并清空 `child`。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public Node flatten(Node head) {
        if (head == null) return null;
        List<Node> nodes = new ArrayList<>();
        collect(head, nodes);
        for (int i = 0; i < nodes.size(); i++) {
            Node node = nodes.get(i);
            node.prev = i == 0 ? null : nodes.get(i - 1);
            node.next = i + 1 == nodes.size() ? null : nodes.get(i + 1);
            node.child = null;
        }
        return nodes.get(0);
    }

    private void collect(Node node, List<Node> nodes) {
        while (node != null) {
            nodes.add(node);
            Node next = node.next;
            if (node.child != null) {
                collect(node.child, nodes);
            }
            node = next;
        }
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 资深解法：原地 DFS 返回尾节点

算法思想：遍历当前链表，遇到 child 时先扁平化 child，得到子链尾部，再把子链插入当前节点和原 next 之间，并继续向后处理。

```java
class Solution {
    public Node flatten(Node head) {
        flattenTail(head);
        return head;
    }

    private Node flattenTail(Node head) {
        Node cur = head;
        Node tail = head;
        while (cur != null) {
            Node next = cur.next;
            if (cur.child != null) {
                Node childHead = cur.child;
                Node childTail = flattenTail(childHead);
                cur.child = null;
                cur.next = childHead;
                childHead.prev = cur;
                if (next != null) {
                    childTail.next = next;
                    next.prev = childTail;
                }
                tail = childTail;
            } else {
                tail = cur;
            }
            cur = next;
        }
        return tail;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(depth)`。

#### 基础语法与算法思想

- 多级链表的扁平化顺序是深度优先。
- 插入 child 链表前要保存原来的 `next`，避免断链。
- 核心思想：原地链表改造时，先保存后继，再重连前后边界。

---

## 431. 将 N 叉树编码为二叉树 (Hard)

暂无内容描述。

### Java 解法补充

#### 基础解法：左孩子右兄弟表示法

算法思想：二叉树的 `left` 指向 N 叉节点的第一个孩子，`right` 指向这个孩子的下一个兄弟。递归编码每个孩子链，解码时沿着 `right` 链还原孩子列表。

```java
import java.util.ArrayList;

class Codec {
    public TreeNode encode(Node root) {
        if (root == null) return null;
        TreeNode node = new TreeNode(root.val);
        if (!root.children.isEmpty()) {
            node.left = encode(root.children.get(0));
        }
        TreeNode cur = node.left;
        for (int i = 1; i < root.children.size(); i++) {
            cur.right = encode(root.children.get(i));
            cur = cur.right;
        }
        return node;
    }

    public Node decode(TreeNode root) {
        if (root == null) return null;
        Node node = new Node(root.val, new ArrayList<>());
        TreeNode child = root.left;
        while (child != null) {
            node.children.add(decode(child));
            child = child.right;
        }
        return node;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(h)`。

#### 资深解法：封装兄弟链转换

算法思想：把“孩子列表转兄弟链”和“兄弟链转孩子列表”拆成辅助函数，主流程只关心当前节点值映射，结构边界更清晰。

```java
import java.util.ArrayList;
import java.util.List;

class Codec {
    public TreeNode encode(Node root) {
        if (root == null) return null;
        TreeNode node = new TreeNode(root.val);
        node.left = encodeChildren(root.children);
        return node;
    }

    private TreeNode encodeChildren(List<Node> children) {
        TreeNode dummy = new TreeNode(0);
        TreeNode tail = dummy;
        for (Node child : children) {
            tail.right = encode(child);
            tail = tail.right;
        }
        return dummy.right;
    }

    public Node decode(TreeNode root) {
        if (root == null) return null;
        return new Node(root.val, decodeChildren(root.left));
    }

    private List<Node> decodeChildren(TreeNode child) {
        List<Node> children = new ArrayList<>();
        while (child != null) {
            children.add(decode(child));
            child = child.right;
        }
        return children;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(h)`。

#### 基础语法与算法思想

- N 叉树孩子列表可以用二叉树的左孩子、右兄弟模型表达。
- 虚拟头节点常用于简化链式拼接。
- 核心思想：编码结构时要保证解码规则和编码规则完全互逆。

---

## 432. 全 O(1) 的数据结构 (Hard)

请你设计一个用于存储字符串计数的数据结构，并能够返回计数最小和最大的字符串。
实现  `AllOne`  类：

 `AllOne()`  初始化数据结构的对象。
 `inc(String key)`  字符串  `key`  的计数增加  `1`  。如果数据结构中尚不存在  `key`  ，那么插入计数为  `1`  的  `key`  。
 `dec(String key)`  字符串  `key`  的计数减少  `1`  。如果  `key`  的计数在减少后为  `0`  ，那么需要将这个  `key`  从数据结构中删除。测试用例保证：在减少计数前， `key`  存在于数据结构中。
 `getMaxKey()`  返回任意一个计数最大的字符串。如果没有元素存在，返回一个空字符串  `""`  。
 `getMinKey()`  返回任意一个计数最小的字符串。如果没有元素存在，返回一个空字符串  `""`  。

 **注意：** 每个函数都应当满足  `O(1)`  平均时间复杂度。
 
 **示例：** 

```text
输入
["AllOne", "inc", "inc", "getMaxKey", "getMinKey", "inc", "getMaxKey", "getMinKey"]
[[], ["hello"], ["hello"], [], [], ["leet"], [], []]
输出
[null, null, null, "hello", "hello", null, "hello", "leet"]

解释
AllOne allOne = new AllOne();
allOne.inc("hello");
allOne.inc("hello");
allOne.getMaxKey(); // 返回 "hello"
allOne.getMinKey(); // 返回 "hello"
allOne.inc("leet");
allOne.getMaxKey(); // 返回 "hello"
allOne.getMinKey(); // 返回 "leet"
```

 
 **提示：** 

 `1 <= key.length <= 10` 
 `key`  由小写英文字母组成
测试用例保证：在每次调用  `dec`  时，数据结构中总存在  `key` 
最多调用  `inc` 、 `dec` 、 `getMaxKey`  和  `getMinKey`  方法  `5 * 104`  次

### Java 解法补充

#### 基础解法：哈希表加有序表

算法思想：用哈希表记录每个 key 的计数，用 `TreeMap` 把计数映射到这一计数下的 key 集合。增减时在两个结构中同步移动 key。

```java
import java.util.HashMap;
import java.util.HashSet;
import java.util.Map;
import java.util.Set;
import java.util.TreeMap;

class AllOne {
    private final Map<String, Integer> count = new HashMap<>();
    private final TreeMap<Integer, Set<String>> buckets = new TreeMap<>();

    public void inc(String key) {
        move(key, count.getOrDefault(key, 0), count.getOrDefault(key, 0) + 1);
    }

    public void dec(String key) {
        move(key, count.get(key), count.get(key) - 1);
    }

    public String getMaxKey() {
        return buckets.isEmpty() ? "" : buckets.lastEntry().getValue().iterator().next();
    }

    public String getMinKey() {
        return buckets.isEmpty() ? "" : buckets.firstEntry().getValue().iterator().next();
    }

    private void move(String key, int oldCount, int newCount) {
        if (oldCount > 0) {
            Set<String> set = buckets.get(oldCount);
            set.remove(key);
            if (set.isEmpty()) buckets.remove(oldCount);
        }
        if (newCount == 0) {
            count.remove(key);
        } else {
            count.put(key, newCount);
            buckets.computeIfAbsent(newCount, x -> new HashSet<>()).add(key);
        }
    }
}
```

复杂度：`inc/dec` 时间 `O(log n)`，查询 `O(1)`，空间 `O(n)`。

#### 资深解法：双向桶链表

算法思想：每个桶保存同一计数的 key，桶按计数递增组成双向链表。key 增减时只会移动到相邻计数桶，因此能保持平均 `O(1)`。

```java
import java.util.HashMap;
import java.util.HashSet;
import java.util.Map;
import java.util.Set;

class AllOne {
    private static class Bucket {
        int count;
        Set<String> keys = new HashSet<>();
        Bucket prev, next;
        Bucket(int count) { this.count = count; }
    }

    private final Bucket head = new Bucket(0);
    private final Bucket tail = new Bucket(0);
    private final Map<String, Bucket> location = new HashMap<>();

    public AllOne() {
        head.next = tail;
        tail.prev = head;
    }

    public void inc(String key) {
        if (!location.containsKey(key)) {
            Bucket bucket = head.next.count == 1 ? head.next : insertAfter(head, new Bucket(1));
            bucket.keys.add(key);
            location.put(key, bucket);
        } else {
            Bucket cur = location.get(key);
            Bucket next = cur.next.count == cur.count + 1 ? cur.next : insertAfter(cur, new Bucket(cur.count + 1));
            moveKey(key, cur, next);
        }
    }

    public void dec(String key) {
        Bucket cur = location.get(key);
        if (cur.count == 1) {
            cur.keys.remove(key);
            location.remove(key);
            if (cur.keys.isEmpty()) remove(cur);
        } else {
            Bucket prev = cur.prev.count == cur.count - 1 ? cur.prev : insertAfter(cur.prev, new Bucket(cur.count - 1));
            moveKey(key, cur, prev);
        }
    }

    public String getMaxKey() {
        return tail.prev == head ? "" : tail.prev.keys.iterator().next();
    }

    public String getMinKey() {
        return head.next == tail ? "" : head.next.keys.iterator().next();
    }

    private void moveKey(String key, Bucket from, Bucket to) {
        from.keys.remove(key);
        to.keys.add(key);
        location.put(key, to);
        if (from.keys.isEmpty()) remove(from);
    }

    private Bucket insertAfter(Bucket prev, Bucket node) {
        node.next = prev.next;
        node.prev = prev;
        prev.next.prev = node;
        prev.next = node;
        return node;
    }

    private void remove(Bucket node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }
}
```

复杂度：所有操作平均时间 `O(1)`，空间 `O(n)`。

#### 基础语法与算法思想

- `TreeMap` 适合快速拿最小/最大 key，但更新是 `O(log n)`。
- 真正 `O(1)` 需要“计数桶 + 双向链表 + key 到桶的定位表”。
- 核心思想：计数每次只变化 1，因此 key 只会移动到相邻桶。

---

## 433. 最小基因变化 (Medium)

基因序列可以表示为一条由 8 个字符组成的字符串，其中每个字符都是  `'A'` 、 `'C'` 、 `'G'`  和  `'T'`  之一。
假设我们需要调查从基因序列  `start`  变为  `end`  所发生的基因变化。一次基因变化就意味着这个基因序列中的一个字符发生了变化。

例如， `"AACCGGTT" --> "AACCGGTA"`  就是一次基因变化。

另有一个基因库  `bank`  记录了所有有效的基因变化，只有基因库中的基因才是有效的基因序列。（变化后的基因必须位于基因库  `bank`  中）
给你两个基因序列  `start`  和  `end`  ，以及一个基因库  `bank`  ，请你找出并返回能够使  `start`  变化为  `end`  所需的最少变化次数。如果无法完成此基因变化，返回  `-1`  。
注意：起始基因序列  `start`  默认是有效的，但是它并不一定会出现在基因库中。
 
 **示例 1：** 

```text
输入：start = "AACCGGTT", end = "AACCGGTA", bank = ["AACCGGTA"]
输出：1
```

 **示例 2：** 

```text
输入：start = "AACCGGTT", end = "AAACGGTA", bank = ["AACCGGTA","AACCGCTA","AAACGGTA"]
输出：2
```

 **示例 3：** 

```text
输入：start = "AAAAACCC", end = "AACCCCCC", bank = ["AAAACCCC","AAACCCCC","AACCCCCC"]
输出：3
```

 
 **提示：** 

 `start.length == 8` 
 `end.length == 8` 
 `0 <= bank.length <= 10` 
 `bank[i].length == 8` 
 `start` 、 `end`  和  `bank[i]`  仅由字符  `['A', 'C', 'G', 'T']`  组成

### Java 解法补充

#### 基础解法：BFS 枚举基因库

算法思想：把每个基因串看成图节点，两个串只差一个字符就有边。从 `start` BFS，第一次到达 `end` 的层数就是最少变化次数。

```java
import java.util.ArrayDeque;
import java.util.HashSet;
import java.util.Queue;
import java.util.Set;

class Solution {
    public int minMutation(String start, String end, String[] bank) {
        Set<String> bankSet = new HashSet<>();
        for (String gene : bank) bankSet.add(gene);
        if (!bankSet.contains(end)) return -1;

        Queue<String> queue = new ArrayDeque<>();
        Set<String> seen = new HashSet<>();
        queue.offer(start);
        seen.add(start);
        int steps = 0;

        while (!queue.isEmpty()) {
            for (int size = queue.size(); size > 0; size--) {
                String cur = queue.poll();
                if (cur.equals(end)) return steps;
                for (String next : bank) {
                    if (!seen.contains(next) && diffOne(cur, next)) {
                        seen.add(next);
                        queue.offer(next);
                    }
                }
            }
            steps++;
        }
        return -1;
    }

    private boolean diffOne(String a, String b) {
        int diff = 0;
        for (int i = 0; i < a.length(); i++) {
            if (a.charAt(i) != b.charAt(i)) diff++;
        }
        return diff == 1;
    }
}
```

复杂度：时间 `O(bank.length^2 * 8)`，空间 `O(bank.length)`。

#### 资深解法：枚举单点变异

算法思想：每次从当前基因串的 8 个位置尝试改成 `A/C/G/T`，只有在基因库中的结果才入队。避免每层扫描整个 bank。

```java
import java.util.ArrayDeque;
import java.util.HashSet;
import java.util.Queue;
import java.util.Set;

class Solution {
    public int minMutation(String start, String end, String[] bank) {
        Set<String> bankSet = new HashSet<>();
        for (String gene : bank) bankSet.add(gene);
        if (!bankSet.contains(end)) return -1;

        char[] genes = {'A', 'C', 'G', 'T'};
        Queue<String> queue = new ArrayDeque<>();
        queue.offer(start);
        bankSet.remove(start);
        int steps = 0;

        while (!queue.isEmpty()) {
            for (int size = queue.size(); size > 0; size--) {
                String cur = queue.poll();
                if (cur.equals(end)) return steps;
                char[] chars = cur.toCharArray();
                for (int i = 0; i < chars.length; i++) {
                    char old = chars[i];
                    for (char g : genes) {
                        chars[i] = g;
                        String next = new String(chars);
                        if (bankSet.remove(next)) {
                            queue.offer(next);
                        }
                    }
                    chars[i] = old;
                }
            }
            steps++;
        }
        return -1;
    }
}
```

复杂度：时间 `O(steps * 8 * 4)` 级别，空间 `O(bank.length)`。

#### 基础语法与算法思想

- 最少变化次数是无权图最短路，优先用 BFS。
- `Set.remove` 可同时完成存在性判断和去重。
- 核心思想：状态空间较小时，直接枚举下一步合法变异比建全图更轻。

---

## 434. 字符串中的单词数 (Easy)

统计字符串中的单词个数，这里的单词指的是连续的不是空格的字符。
请注意，你可以假定字符串里不包括任何不可打印的字符。
**示例:** 

```text
输入: "Hello, my name is John"
输出: 5
解释: 这里的单词是指连续的不是空格的字符，所以 "Hello," 算作 1 个单词。
```

### Java 解法补充

#### 基础解法：按空格切分

算法思想：先去掉首尾空格，再按一个或多个空格切分字符串，切出的片段数量就是单词数。

```java
class Solution {
    public int countSegments(String s) {
        String trimmed = s.trim();
        if (trimmed.isEmpty()) return 0;
        return trimmed.split("\\s+").length;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 资深解法：扫描单词开头

算法思想：不创建额外数组。只统计当前位置不是空格，且它前面是字符串开头或空格的情况。

```java
class Solution {
    public int countSegments(String s) {
        int ans = 0;
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) != ' ' && (i == 0 || s.charAt(i - 1) == ' ')) {
                ans++;
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- `trim` 去除首尾空格，`split("\\s+")` 按连续空白切分。
- 扫描法只数单词开头，避免创建临时字符串数组。
- 核心思想：连续非空格字符组成一个单词。

---

## 435. 无重叠区间 (Medium)

给定一个区间的集合  `intervals`  ，其中  `intervals[i] = [starti, endi]`  。返回 需要移除区间的最小数量，使剩余区间互不重叠 。
 **注意**  只在一点上接触的区间是  **不重叠的** 。例如  `[1, 2]`  和  `[2, 3]`  是不重叠的。
 
 **示例 1:** 

```text
输入: intervals = [[1,2],[2,3],[3,4],[1,3]]
输出: 1
解释: 移除 [1,3] 后，剩下的区间没有重叠。
```

 **示例 2:** 

```text
输入: intervals = [ [1,2], [1,2], [1,2] ]
输出: 2
解释: 你需要移除两个 [1,2] 来使剩下的区间没有重叠。
```

 **示例 3:** 

```text
输入: intervals = [ [1,2], [2,3] ]
输出: 0
解释: 你不需要移除任何区间，因为它们已经是无重叠的了。
```

 
 **提示:** 

 `1 <= intervals.length <= 105` 
 `intervals[i].length == 2` 
 `-5 * 104 <= starti < endi <= 5 * 104`

### Java 解法补充

#### 基础解法：排序后动态规划保留最多区间

算法思想：按起点排序，`dp[i]` 表示以第 `i` 个区间结尾的最多不重叠区间数量。最后用总区间数减去最多保留数量。

```java
import java.util.Arrays;

class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
        int n = intervals.length;
        int[] dp = new int[n];
        Arrays.fill(dp, 1);
        int keep = 1;
        for (int i = 1; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (intervals[j][1] <= intervals[i][0]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
            keep = Math.max(keep, dp[i]);
        }
        return n - keep;
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(n)`。

#### 资深解法：按结束时间贪心

算法思想：想保留最多不重叠区间，每次选择结束最早的区间，为后续留下最大空间。遇到起点早于当前结束点的区间就移除。

```java
import java.util.Arrays;

class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        Arrays.sort(intervals, (a, b) -> Integer.compare(a[1], b[1]));
        int removed = 0;
        int end = intervals[0][1];
        for (int i = 1; i < intervals.length; i++) {
            if (intervals[i][0] < end) {
                removed++;
            } else {
                end = intervals[i][1];
            }
        }
        return removed;
    }
}
```

复杂度：时间 `O(n log n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 不重叠允许 `end == nextStart`。
- `Integer.compare` 比直接相减更稳，避免极端值溢出。
- 核心思想：区间调度问题通常按结束时间贪心。

---

## 436. 寻找右区间 (Medium)

给你一个区间数组  `intervals`  ，其中  `intervals[i] = [starti, endi]`  ，且每个  `starti`  都  **不同**  。
区间  `i`  的  **右侧区间**  是满足  `startj >= endi` ，且  `startj`   **最小** 的区间  `j` 。注意  `i`  可能等于  `j`  。
返回一个由每个区间  `i`  对应的  **右侧区间**  下标组成的数组。如果某个区间  `i`  不存在对应的  **右侧区间**  ，则下标  `i`  处的值设为  `-1`  。
 

 **示例 1：** 

```text
输入：intervals = [[1,2]]
输出：[-1]
解释：集合中只有一个区间，所以输出-1。
```

 **示例 2：** 

```text
输入：intervals = [[3,4],[2,3],[1,2]]
输出：[-1,0,1]
解释：对于 [3,4] ，没有满足条件的“右侧”区间。
对于 [2,3] ，区间[3,4]具有最小的“右”起点;
对于 [1,2] ，区间[2,3]具有最小的“右”起点。
```

 **示例 3：** 

```text
输入：intervals = [[1,4],[2,3],[3,4]]
输出：[-1,2,-1]
解释：对于区间 [1,4] 和 [3,4] ，没有满足条件的“右侧”区间。
对于 [2,3] ，区间 [3,4] 有最小的“右”起点。
```

 
 **提示：** 

 `1 <= intervals.length <= 2 * 104` 
 `intervals[i].length == 2` 
 `-106 <= starti <= endi <= 106` 
每个间隔的起点都  **不相同**

### Java 解法补充

#### 基础解法：逐个区间线性查找

算法思想：对每个区间，遍历所有区间的起点，找出大于等于当前终点且最小的起点对应下标。

```java
class Solution {
    public int[] findRightInterval(int[][] intervals) {
        int n = intervals.length;
        int[] ans = new int[n];
        for (int i = 0; i < n; i++) {
            int best = -1;
            for (int j = 0; j < n; j++) {
                if (intervals[j][0] >= intervals[i][1]
                        && (best == -1 || intervals[j][0] < intervals[best][0])) {
                    best = j;
                }
            }
            ans[i] = best;
        }
        return ans;
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(1)`。

#### 资深解法：TreeMap 天花板查询

算法思想：每个起点唯一，把“起点 -> 下标”放入 `TreeMap`。对每个区间终点调用 `ceilingEntry` 找到不小于它的最小起点。

```java
import java.util.Map;
import java.util.TreeMap;

class Solution {
    public int[] findRightInterval(int[][] intervals) {
        TreeMap<Integer, Integer> starts = new TreeMap<>();
        for (int i = 0; i < intervals.length; i++) {
            starts.put(intervals[i][0], i);
        }

        int[] ans = new int[intervals.length];
        for (int i = 0; i < intervals.length; i++) {
            Map.Entry<Integer, Integer> entry = starts.ceilingEntry(intervals[i][1]);
            ans[i] = entry == null ? -1 : entry.getValue();
        }
        return ans;
    }
}
```

复杂度：时间 `O(n log n)`，空间 `O(n)`。

#### 基础语法与算法思想

- `TreeMap.ceilingEntry(x)` 返回 key 大于等于 `x` 的最小条目。
- 起点唯一时可以直接映射到原下标。
- 核心思想：找“不小于某值的最小值”是有序表或二分的典型场景。

---

## 437. 路径总和 III (Medium)

给定一个二叉树的根节点  `root`  ，和一个整数  `targetSum`  ，求该二叉树里节点值之和等于  `targetSum`  的  **路径**  的数目。
 **路径**  不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。
 
 **示例 1：** 

```text
输入：root = [10,5,-3,3,2,null,11,3,-2,null,1], targetSum = 8
输出：3
解释：和等于 8 的路径有 3 条，如图所示。
```

 **示例 2：** 

```text
输入：root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
输出：3
```

 
 **提示:** 

二叉树的节点个数的范围是  `[0,1000]` 
 `-109 <= Node.val <= 109`  
 `-1000 <= targetSum <= 1000`

### Java 解法补充

#### 基础解法：以每个节点为起点 DFS

算法思想：路径可以从任意节点开始，所以先遍历每个节点，把它当作起点，再向下搜索所有路径和。

```java
class Solution {
    public int pathSum(TreeNode root, int targetSum) {
        if (root == null) return 0;
        return countFrom(root, targetSum)
                + pathSum(root.left, targetSum)
                + pathSum(root.right, targetSum);
    }

    private int countFrom(TreeNode node, long remain) {
        if (node == null) return 0;
        int ans = node.val == remain ? 1 : 0;
        ans += countFrom(node.left, remain - node.val);
        ans += countFrom(node.right, remain - node.val);
        return ans;
    }
}
```

复杂度：最坏时间 `O(n^2)`，空间 `O(h)`。

#### 资深解法：前缀和哈希表

算法思想：从根到当前节点的路径前缀和为 `sum`。若之前存在前缀和 `sum - target`，则这两段之间的路径和为目标值。

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int pathSum(TreeNode root, int targetSum) {
        Map<Long, Integer> prefix = new HashMap<>();
        prefix.put(0L, 1);
        return dfs(root, 0, targetSum, prefix);
    }

    private int dfs(TreeNode node, long sum, int target, Map<Long, Integer> prefix) {
        if (node == null) return 0;
        sum += node.val;
        int ans = prefix.getOrDefault(sum - target, 0);
        prefix.put(sum, prefix.getOrDefault(sum, 0) + 1);
        ans += dfs(node.left, sum, target, prefix);
        ans += dfs(node.right, sum, target, prefix);
        prefix.put(sum, prefix.get(sum) - 1);
        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(h)` 到 `O(n)`。

#### 基础语法与算法思想

- 路径方向只能向下，但起点和终点可以任意。
- 回溯时要撤销当前前缀和计数，避免影响兄弟子树。
- 核心思想：树上路径和可以借鉴数组前缀和。

---

## 438. 找到字符串中所有字母异位词 (Medium)

给定两个字符串  `s`  和  `p` ，找到  `s`  **** 中所有  `p`  **** 的  **异位词** 的子串，返回这些子串的起始索引。不考虑答案输出的顺序。
 
 **示例 1:** 

```text
输入: s = "cbaebabacd", p = "abc"
输出: [0,6]
解释:
起始索引等于 0 的子串是 "cba", 它是 "abc" 的异位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的异位词。
```

 **示例 2:** 

```text
输入: s = "abab", p = "ab"
输出: [0,1,2]
解释:
起始索引等于 0 的子串是 "ab", 它是 "ab" 的异位词。
起始索引等于 1 的子串是 "ba", 它是 "ab" 的异位词。
起始索引等于 2 的子串是 "ab", 它是 "ab" 的异位词。
```

 
 **提示:** 

 `1 <= s.length, p.length <= 3 * 104` 
 `s`  和  `p`  仅包含小写字母

### Java 解法补充

#### 基础解法：固定窗口逐次比较计数

算法思想：先统计 `p` 的字符频次。枚举 `s` 中所有长度为 `p.length()` 的窗口，统计窗口频次并比较。

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> ans = new ArrayList<>();
        int m = p.length();
        int[] target = new int[26];
        for (int i = 0; i < p.length(); i++) target[p.charAt(i) - 'a']++;

        for (int left = 0; left + m <= s.length(); left++) {
            int[] count = new int[26];
            for (int i = left; i < left + m; i++) {
                count[s.charAt(i) - 'a']++;
            }
            if (Arrays.equals(count, target)) ans.add(left);
        }
        return ans;
    }
}
```

复杂度：时间 `O(26n + n * |p|)`，空间 `O(1)`。

#### 资深解法：滑动窗口差分计数

算法思想：维护固定长度窗口的字符频次。右侧加入新字符，左侧移出旧字符，每次比较 26 个计数即可。

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> ans = new ArrayList<>();
        if (s.length() < p.length()) return ans;
        int[] need = new int[26];
        int[] window = new int[26];
        for (int i = 0; i < p.length(); i++) {
            need[p.charAt(i) - 'a']++;
            window[s.charAt(i) - 'a']++;
        }
        if (Arrays.equals(need, window)) ans.add(0);

        for (int right = p.length(); right < s.length(); right++) {
            window[s.charAt(right) - 'a']++;
            window[s.charAt(right - p.length()) - 'a']--;
            if (Arrays.equals(need, window)) {
                ans.add(right - p.length() + 1);
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(26n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 异位词字符频次完全相同。
- 固定窗口长度等于模式串长度。
- 核心思想：窗口每次只变化两个字符，频次数组可以增量维护。

---

## 439. 三元表达式解析器 (Medium)

暂无内容描述。

### Java 解法补充

#### 基础解法：递归解析表达式

算法思想：如果表达式长度为 1，直接返回。否则第一个字符是条件，后面形如 `? trueExpr : falseExpr`，需要找到与第一个 `?` 对应的冒号，再递归解析选中的分支。

```java
class Solution {
    public String parseTernary(String expression) {
        return parse(expression, 0, expression.length() - 1);
    }

    private String parse(String s, int left, int right) {
        if (left == right) return String.valueOf(s.charAt(left));
        int colon = findColon(s, left + 2, right);
        if (s.charAt(left) == 'T') {
            return parse(s, left + 2, colon - 1);
        }
        return parse(s, colon + 1, right);
    }

    private int findColon(String s, int left, int right) {
        int depth = 0;
        for (int i = left; i <= right; i++) {
            if (s.charAt(i) == '?') depth++;
            else if (s.charAt(i) == ':') {
                if (depth == 0) return i;
                depth--;
            }
        }
        return -1;
    }
}
```

复杂度：最坏时间 `O(n^2)`，空间 `O(n)`。

#### 资深解法：从右向左用栈

算法思想：三元表达式右结合。从右往左扫描，遇到条件字符且右边是 `?` 时，栈顶两个值就是 true/false 分支，根据条件选择一个压回。

```java
import java.util.ArrayDeque;
import java.util.Deque;

class Solution {
    public String parseTernary(String expression) {
        Deque<Character> stack = new ArrayDeque<>();
        for (int i = expression.length() - 1; i >= 0; i--) {
            char c = expression.charAt(i);
            if (!stack.isEmpty() && stack.peek() == '?') {
                stack.pop();
                char trueValue = stack.pop();
                stack.pop();
                char falseValue = stack.pop();
                stack.push(c == 'T' ? trueValue : falseValue);
            } else if (c != ':') {
                stack.push(c);
            }
        }
        return String.valueOf(stack.peek());
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 基础语法与算法思想

- 三元表达式是右结合，适合从右往左归约。
- 栈中同时保存操作符和值，可逐步把子表达式折叠成一个值。
- 核心思想：嵌套表达式要么找匹配分隔符递归，要么反向扫描用栈消解。

---

## 440. 字典序的第K小数字 (Hard)

给定整数  `n`  和  `k` ，返回   `[1, n]`  中字典序第  `k`  小的数字。
 
 **示例 1:** 

```text
输入: n = 13, k = 2
输出: 10
解释: 字典序的排列是 [1, 10, 11, 12, 13, 2, 3, 4, 5, 6, 7, 8, 9]，所以第二小的数字是 10。
```

 **示例 2:** 

```text
输入: n = 1, k = 1
输出: 1
```

 
 **提示:** 

 `1 <= k <= n <= 109`

### Java 解法补充

#### 基础解法：生成后按字符串排序

算法思想：生成 `1..n`，按字符串字典序排序，返回第 `k - 1` 个。这个写法只适合理解题意，`n` 很大时不可行。

```java
import java.util.ArrayList;
import java.util.Comparator;
import java.util.List;

class Solution {
    public int findKthNumber(int n, int k) {
        List<Integer> nums = new ArrayList<>();
        for (int i = 1; i <= n; i++) nums.add(i);
        nums.sort(Comparator.comparing(String::valueOf));
        return nums.get(k - 1);
    }
}
```

复杂度：时间 `O(n log n)`，空间 `O(n)`。

#### 资深解法：按前缀统计子树大小

算法思想：字典序可以看作十叉前缀树。当前前缀为 `prefix`，先计算它下面有多少个不超过 `n` 的数字。如果数量小于等于剩余 `k`，跳到下一个兄弟前缀；否则进入当前前缀的下一层。

```java
class Solution {
    public int findKthNumber(int n, int k) {
        int prefix = 1;
        k--;
        while (k > 0) {
            long count = countPrefix(prefix, n);
            if (count <= k) {
                prefix++;
                k -= count;
            } else {
                prefix *= 10;
                k--;
            }
        }
        return prefix;
    }

    private long countPrefix(long prefix, int n) {
        long count = 0;
        long cur = prefix;
        long next = prefix + 1;
        while (cur <= n) {
            count += Math.min(n + 1L, next) - cur;
            cur *= 10;
            next *= 10;
        }
        return count;
    }
}
```

复杂度：时间 `O(log^2 n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 字典序数字可以看作按前缀组织的十叉树。
- `countPrefix` 统计某个前缀子树中有多少合法数字。
- 核心思想：不逐个走字典序，而是按前缀子树整块跳过。

---

## 441. 排列硬币 (Easy)

你总共有  `n`  枚硬币，并计划将它们按阶梯状排列。对于一个由  `k`  行组成的阶梯，其第  `i`  行必须正好有  `i`  枚硬币。阶梯的最后一行  **可能**  是不完整的。
给你一个数字  `n`  ，计算并返回可形成  **完整阶梯行**  的总行数。
 
 **示例 1：** 

```text
输入：n = 5
输出：2
解释：因为第三行不完整，所以返回 2 。
```

 **示例 2：** 

```text
输入：n = 8
输出：3
解释：因为第四行不完整，所以返回 3 。
```

 
 **提示：** 

 `1 <= n <= 231 - 1`

### Java 解法补充

#### 基础解法：逐行扣硬币

算法思想：第 1 行用 1 枚，第 2 行用 2 枚，依次扣减，直到剩余硬币不足以填满下一行。

```java
class Solution {
    public int arrangeCoins(int n) {
        int row = 0;
        long coins = n;
        while (coins >= row + 1) {
            row++;
            coins -= row;
        }
        return row;
    }
}
```

复杂度：时间 `O(sqrt(n))`，空间 `O(1)`。

#### 资深解法：二分完整行数

算法思想：若能摆满 `x` 行，需要 `x * (x + 1) / 2` 枚硬币。这个需求随 `x` 单调增加，用二分找最大可行行数。

```java
class Solution {
    public int arrangeCoins(int n) {
        long left = 0;
        long right = n;
        while (left <= right) {
            long mid = left + (right - left) / 2;
            long need = mid * (mid + 1) / 2;
            if (need <= n) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return (int) right;
    }
}
```

复杂度：时间 `O(log n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 等差和公式为 `x * (x + 1) / 2`。
- 乘法用 `long` 避免溢出。
- 核心思想：可行行数具有单调性，适合二分。

---

## 442. 数组中重复的数据 (Medium)

给你一个长度为  `n`  的整数数组  `nums`  ，其中  `nums`  的所有整数都在范围  `[1, n]`  内，且每个整数出现  **最多**  **两次**  。请你找出所有出现  **两次**  的整数，并以数组形式返回。
你必须设计并实现一个时间复杂度为  `O(n)`  且仅使用常量额外空间（不包括存储输出所需的空间）的算法解决此问题。
 
 **示例 1：** 

```text
输入：nums = [4,3,2,7,8,2,3,1]
输出：[2,3]
```

 **示例 2：** 

```text
输入：nums = [1,1,2]
输出：[1]
```

 **示例 3：** 

```text
输入：nums = [1]
输出：[]
```

 
 **提示：** 

 `n == nums.length` 
 `1 <= n <= 105` 
 `1 <= nums[i] <= n` 
 `nums`  中的每个元素出现  **一次**  或  **两次**

### Java 解法补充

#### 基础解法：哈希集合判重

算法思想：遍历数组，用集合记录已经见过的数字。再次遇到的数字就是重复数字。

```java
import java.util.ArrayList;
import java.util.HashSet;
import java.util.List;
import java.util.Set;

class Solution {
    public List<Integer> findDuplicates(int[] nums) {
        Set<Integer> seen = new HashSet<>();
        List<Integer> ans = new ArrayList<>();
        for (int num : nums) {
            if (!seen.add(num)) {
                ans.add(num);
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 资深解法：原地取负标记

算法思想：数字范围是 `1..n`，可把数字 `x` 映射到下标 `x - 1`。第一次见到 `x` 时把该位置变负；第二次发现已经为负，说明 `x` 重复。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<Integer> findDuplicates(int[] nums) {
        List<Integer> ans = new ArrayList<>();
        for (int num : nums) {
            int index = Math.abs(num) - 1;
            if (nums[index] < 0) {
                ans.add(index + 1);
            } else {
                nums[index] = -nums[index];
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(n)`，额外空间 `O(1)`，不计答案。

#### 基础语法与算法思想

- `Math.abs` 用于读取被标记后的原始数字。
- 原地标记利用了数值范围和数组下标一一对应。
- 核心思想：当数组值限定在 `1..n` 时，经常可以把值映射成下标做标记。

---

## 443. 压缩字符串 (Medium)

给你一个字符数组  `chars`  ，请使用下述算法压缩：
从一个空字符串  `s`  开始。对于  `chars`  中的每组  **连续重复字符**  ：

如果这一组长度为  `1`  ，则将字符追加到  `s`  中。
否则，需要向  `s`  追加字符，后跟这一组的长度。

压缩后得到的字符串  `s`   **不应该直接返回**  ，需要转储到字符数组  `chars`  中。需要注意的是，如果组长度为  `10`  或  `10`  以上，则在  `chars`  数组中会被拆分为多个字符。
请在  **修改完输入数组后**  ，返回该数组的新长度。
你必须设计并实现一个只使用常量额外空间的算法来解决此问题。
 **注意：** 数组中超出返回长度的字符无关紧要，应予忽略。
 
 **示例 1：** 

```text
输入：chars = ["a","a","b","b","c","c","c"]
输出：6
解释："aa" 被 "a2" 替代。"bb" 被 "b2" 替代。"ccc" 被 "c3" 替代。
```

 **示例 2：** 

```text
输入：chars = ["a"]
输出：1
解释：唯一的组是“a”，它保持未压缩，因为它是一个字符。
```

 **示例 3：** 

```text
输入：chars = ["a","b","b","b","b","b","b","b","b","b","b","b","b"]
输出：4
解释：由于字符 "a" 不重复，所以不会被压缩。"bbbbbbbbbbbb" 被 “b12” 替代。
```

 
 **提示：** 

 `1 <= chars.length <= 2000` 
 `chars[i]`  可以是小写英文字母、大写英文字母、数字或符号

### Java 解法补充

#### 基础解法：构造压缩字符串再写回

算法思想：先用 `StringBuilder` 按规则生成压缩结果，再把结果逐字符写回原数组前部。

```java
class Solution {
    public int compress(char[] chars) {
        StringBuilder builder = new StringBuilder();
        int i = 0;
        while (i < chars.length) {
            int j = i;
            while (j < chars.length && chars[j] == chars[i]) j++;
            builder.append(chars[i]);
            if (j - i > 1) builder.append(j - i);
            i = j;
        }
        for (int k = 0; k < builder.length(); k++) {
            chars[k] = builder.charAt(k);
        }
        return builder.length();
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 资深解法：原地读写双指针

算法思想：`read` 扫描每组连续字符，`write` 写压缩后的字符和计数字符。计数大于 1 时，把数字字符串逐位写入。

```java
class Solution {
    public int compress(char[] chars) {
        int read = 0;
        int write = 0;
        while (read < chars.length) {
            char c = chars[read];
            int start = read;
            while (read < chars.length && chars[read] == c) read++;
            chars[write++] = c;
            int count = read - start;
            if (count > 1) {
                for (char digit : String.valueOf(count).toCharArray()) {
                    chars[write++] = digit;
                }
            }
        }
        return write;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 压缩计数如 `12` 要拆成字符 `'1'` 和 `'2'`。
- 原地数组处理常用读指针和写指针。
- 核心思想：连续分组题先找到一段 `[start, end)`，再处理这一段的输出。

---

## 444. 序列重建 (Medium)

暂无内容描述。

### Java 解法补充

#### 基础解法：检查相邻关系是否都出现

算法思想：唯一重建为 `nums` 时，`nums` 中每一对相邻数字都必须在某个子序列中以相邻顺序出现；同时所有出现的数字都必须合法。

```java
import java.util.HashSet;
import java.util.List;
import java.util.Set;

class Solution {
    public boolean sequenceReconstruction(int[] nums, List<List<Integer>> sequences) {
        int n = nums.length;
        int[] pos = new int[n + 1];
        for (int i = 0; i < n; i++) pos[nums[i]] = i;
        boolean[] matched = new boolean[n - 1];
        int count = 0;
        Set<Integer> seen = new HashSet<>();

        for (List<Integer> seq : sequences) {
            for (int num : seq) {
                if (num < 1 || num > n) return false;
                seen.add(num);
            }
            for (int i = 1; i < seq.size(); i++) {
                int a = seq.get(i - 1);
                int b = seq.get(i);
                if (pos[a] >= pos[b]) return false;
                if (pos[a] + 1 == pos[b] && !matched[pos[a]]) {
                    matched[pos[a]] = true;
                    count++;
                }
            }
        }
        return seen.size() == n && count == n - 1;
    }
}
```

复杂度：时间 `O(totalLength)`，空间 `O(n)`。

#### 资深解法：拓扑排序检查唯一队头

算法思想：根据所有相邻约束建图并计算入度。拓扑排序每一步队列中必须只有一个可选节点，并且弹出的顺序必须等于 `nums`。

```java
import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.HashSet;
import java.util.List;
import java.util.Queue;
import java.util.Set;

class Solution {
    public boolean sequenceReconstruction(int[] nums, List<List<Integer>> sequences) {
        int n = nums.length;
        List<Set<Integer>> graph = new ArrayList<>();
        for (int i = 0; i <= n; i++) graph.add(new HashSet<>());
        int[] indegree = new int[n + 1];
        boolean[] seen = new boolean[n + 1];

        for (List<Integer> seq : sequences) {
            for (int num : seq) {
                if (num < 1 || num > n) return false;
                seen[num] = true;
            }
            for (int i = 1; i < seq.size(); i++) {
                int a = seq.get(i - 1), b = seq.get(i);
                if (graph.get(a).add(b)) indegree[b]++;
            }
        }

        Queue<Integer> queue = new ArrayDeque<>();
        for (int i = 1; i <= n; i++) {
            if (!seen[i]) return false;
            if (indegree[i] == 0) queue.offer(i);
        }
        int index = 0;
        while (!queue.isEmpty()) {
            if (queue.size() > 1) return false;
            int cur = queue.poll();
            if (index == n || nums[index++] != cur) return false;
            for (int next : graph.get(cur)) {
                if (--indegree[next] == 0) queue.offer(next);
            }
        }
        return index == n;
    }
}
```

复杂度：时间 `O(totalLength)`，空间 `O(n + edges)`。

#### 基础语法与算法思想

- 唯一拓扑序要求每一步只有一个入度为 0 的节点。
- 邻接表用 `Set` 可避免重复边导致入度重复增加。
- 核心思想：序列重建本质是由局部顺序约束推出唯一全序。

---

## 445. 两数相加 II (Medium)

给你两个  **非空** 链表来代表两个非负整数。数字最高位位于链表开始位置。它们的每个节点只存储一位数字。将这两数相加会返回一个新的链表。
你可以假设除了数字 0 之外，这两个数字都不会以零开头。
 
 **示例1：** 

```text
输入：l1 = [7,2,4,3], l2 = [5,6,4]
输出：[7,8,0,7]
```

 **示例2：** 

```text
输入：l1 = [2,4,3], l2 = [5,6,4]
输出：[8,0,7]
```

 **示例3：** 

```text
输入：l1 = [0], l2 = [0]
输出：[0]
```

 
 **提示：** 

链表的长度范围为 `[1, 100]` 
 `0 <= node.val <= 9` 
输入数据保证链表代表的数字无前导 0

 
 **进阶：** 如果输入链表不能翻转该如何解决？

### Java 解法补充

#### 基础解法：反转链表后相加

算法思想：链表高位在前，先反转两个链表，让低位在前，再按普通链表加法处理，最后反转结果。

```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        l1 = reverse(l1);
        l2 = reverse(l2);
        ListNode head = null;
        int carry = 0;
        while (l1 != null || l2 != null || carry != 0) {
            int sum = carry;
            if (l1 != null) {
                sum += l1.val;
                l1 = l1.next;
            }
            if (l2 != null) {
                sum += l2.val;
                l2 = l2.next;
            }
            ListNode node = new ListNode(sum % 10);
            node.next = head;
            head = node;
            carry = sum / 10;
        }
        return head;
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

复杂度：时间 `O(m+n)`，空间 `O(1)`，不计返回链表。

#### 资深解法：栈保存数字

算法思想：不翻转输入链表。把两个链表的数字分别压栈，弹栈时就是从低位到高位相加，每次把新节点插到结果头部。

```java
import java.util.ArrayDeque;
import java.util.Deque;

class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        Deque<Integer> s1 = new ArrayDeque<>();
        Deque<Integer> s2 = new ArrayDeque<>();
        while (l1 != null) {
            s1.push(l1.val);
            l1 = l1.next;
        }
        while (l2 != null) {
            s2.push(l2.val);
            l2 = l2.next;
        }

        ListNode head = null;
        int carry = 0;
        while (!s1.isEmpty() || !s2.isEmpty() || carry != 0) {
            int sum = carry;
            if (!s1.isEmpty()) sum += s1.pop();
            if (!s2.isEmpty()) sum += s2.pop();
            ListNode node = new ListNode(sum % 10);
            node.next = head;
            head = node;
            carry = sum / 10;
        }
        return head;
    }
}
```

复杂度：时间 `O(m+n)`，空间 `O(m+n)`。

#### 基础语法与算法思想

- 栈可以把高位在前的链表转换成低位先处理。
- 头插法可以直接生成高位在前的结果链表。
- 核心思想：不能反转输入时，用栈模拟从尾到头访问。

---

## 446. 等差数列划分 II - 子序列 (Hard)

给你一个整数数组  `nums`  ，返回  `nums`  中所有  **等差子序列**  的数目。
如果一个序列中  **至少有三个元素**  ，并且任意两个相邻元素之差相同，则称该序列为等差序列。

例如， `[1, 3, 5, 7, 9]` 、 `[7, 7, 7, 7]`  和  `[3, -1, -5, -9]`  都是等差序列。
再例如， `[1, 1, 2, 5, 7]`  不是等差序列。

数组中的子序列是从数组中删除一些元素（也可能不删除）得到的一个序列。

例如， `[2,5,10]`  是  `[1,2,1,2,4,1,5,10]`  的一个子序列。

题目数据保证答案是一个  **32-bit**  整数。
 
 **示例 1：** 

```text
输入：nums = [2,4,6,8,10]
输出：7
解释：所有的等差子序列为：
[2,4,6]
[4,6,8]
[6,8,10]
[2,4,6,8]
[4,6,8,10]
[2,4,6,8,10]
[2,6,10]
```

 **示例 2：** 

```text
输入：nums = [7,7,7,7,7]
输出：16
解释：数组中的任意子序列都是等差子序列。
```

 
 **提示：** 

 `1  <= nums.length <= 1000` 
 `-231 <= nums[i] <= 231 - 1`

### Java 解法补充

#### 基础解法：枚举前两项递归扩展

算法思想：枚举子序列的前两项和公差，再向后递归选择能接上的数字。这个方法直观但会重复计算很多状态。

```java
class Solution {
    public int numberOfArithmeticSlices(int[] nums) {
        int ans = 0;
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                ans += extend(nums, j + 1, nums[j], (long) nums[j] - nums[i], 2);
            }
        }
        return ans;
    }

    private int extend(int[] nums, int index, int last, long diff, int length) {
        int ans = 0;
        for (int i = index; i < nums.length; i++) {
            if ((long) nums[i] - last == diff) {
                if (length + 1 >= 3) ans++;
                ans += extend(nums, i + 1, nums[i], diff, length + 1);
            }
        }
        return ans;
    }
}
```

复杂度：指数级，空间 `O(n)`。

#### 资深解法：按结尾和公差 DP

算法思想：`dp[i][diff]` 表示以 `nums[i]` 结尾、公差为 `diff`、长度至少为 2 的子序列数量。枚举 `(j, i)`，`dp[j][diff]` 中的序列接上 `i` 后都成为合法答案。

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int numberOfArithmeticSlices(int[] nums) {
        int n = nums.length;
        Map<Long, Integer>[] dp = new HashMap[n];
        for (int i = 0; i < n; i++) dp[i] = new HashMap<>();

        int ans = 0;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < i; j++) {
                long diff = (long) nums[i] - nums[j];
                int count = dp[j].getOrDefault(diff, 0);
                ans += count;
                dp[i].put(diff, dp[i].getOrDefault(diff, 0) + count + 1);
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(n^2)`。

#### 基础语法与算法思想

- 公差可能超出 `int` 范围，需要用 `long`。
- DP 中 `+1` 表示只包含 `nums[j], nums[i]` 的长度为 2 的序列。
- 核心思想：长度至少 3 的答案来自已有长度至少 2 的等差子序列继续扩展。

---

## 447. 回旋镖的数量 (Medium)

给定平面上  `n`  对  **互不相同**  的点  `points`  ，其中  `points[i] = [xi, yi]`  。 **回旋镖**  是由点  `(i, j, k)`  表示的元组 ，其中  `i`  和  `j`  之间的欧式距离和  `i`  和  `k`  之间的欧式距离相等（ **需要考虑元组的顺序** ）。
返回平面上所有回旋镖的数量。
 

 **示例 1：** 

```text
输入：points = [[0,0],[1,0],[2,0]]
输出：2
解释：两个回旋镖为 [[1,0],[0,0],[2,0]] 和 [[1,0],[2,0],[0,0]]
```

 **示例 2：** 

```text
输入：points = [[1,1],[2,2],[3,3]]
输出：2
```

 **示例 3：** 

```text
输入：points = [[1,1]]
输出：0
```

 
 **提示：** 

 `n == points.length` 
 `1 <= n <= 500` 
 `points[i].length == 2` 
 `-104 <= xi, yi <= 104` 
所有点都  **互不相同**

### Java 解法补充

#### 基础解法：枚举三元组

算法思想：枚举中心点 `i` 和两个端点 `j、k`，如果 `i` 到它们的距离平方相等，就形成一个有序回旋镖。

```java
class Solution {
    public int numberOfBoomerangs(int[][] points) {
        int ans = 0;
        for (int i = 0; i < points.length; i++) {
            for (int j = 0; j < points.length; j++) {
                for (int k = 0; k < points.length; k++) {
                    if (i != j && i != k && j != k
                            && dist(points[i], points[j]) == dist(points[i], points[k])) {
                        ans++;
                    }
                }
            }
        }
        return ans;
    }

    private int dist(int[] a, int[] b) {
        int dx = a[0] - b[0];
        int dy = a[1] - b[1];
        return dx * dx + dy * dy;
    }
}
```

复杂度：时间 `O(n^3)`，空间 `O(1)`。

#### 资深解法：按中心点统计距离频次

算法思想：固定中心点，统计其他点到它的距离平方频次。若某个距离有 `c` 个点，可以组成 `c * (c - 1)` 个有序端点对。

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int numberOfBoomerangs(int[][] points) {
        int ans = 0;
        for (int[] center : points) {
            Map<Integer, Integer> count = new HashMap<>();
            for (int[] point : points) {
                int d = dist(center, point);
                int c = count.getOrDefault(d, 0);
                ans += 2 * c;
                count.put(d, c + 1);
            }
        }
        return ans;
    }

    private int dist(int[] a, int[] b) {
        int dx = a[0] - b[0];
        int dy = a[1] - b[1];
        return dx * dx + dy * dy;
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(n)`。

#### 基础语法与算法思想

- 比较距离时使用平方距离，避免开方。
- 回旋镖考虑顺序，所以同距离点数为 `c` 时贡献 `c * (c - 1)`。
- 核心思想：固定中心点后，问题变成按距离分组计数。

---

## 448. 找到所有数组中消失的数字 (Easy)

给你一个含  `n`  个整数的数组  `nums`  ，其中  `nums[i]`  在区间  `[1, n]`  内。请你找出所有在  `[1, n]`  范围内但没有出现在  `nums`  中的数字，并以数组的形式返回结果。
 
 **示例 1：** 

```text
输入：nums = [4,3,2,7,8,2,3,1]
输出：[5,6]
```

 **示例 2：** 

```text
输入：nums = [1,1]
输出：[2]
```

 
 **提示：** 

 `n == nums.length` 
 `1 <= n <= 105` 
 `1 <= nums[i] <= n` 

 **进阶：** 你能在不使用额外空间且时间复杂度为  `O(n)`  的情况下解决这个问题吗? 你可以假定返回的数组不算在额外空间内。

### Java 解法补充

#### 基础解法：哈希集合记录出现数字

算法思想：先把出现过的数字放入集合，再遍历 `1..n`，不在集合中的数字就是消失数字。

```java
import java.util.ArrayList;
import java.util.HashSet;
import java.util.List;
import java.util.Set;

class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        Set<Integer> seen = new HashSet<>();
        for (int num : nums) seen.add(num);
        List<Integer> ans = new ArrayList<>();
        for (int i = 1; i <= nums.length; i++) {
            if (!seen.contains(i)) ans.add(i);
        }
        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 资深解法：原地负号标记

算法思想：数字 `x` 对应下标 `x - 1`。遍历数组时把出现数字对应位置标成负数，最后仍为正数的位置说明对应数字没有出现。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        for (int num : nums) {
            int index = Math.abs(num) - 1;
            nums[index] = -Math.abs(nums[index]);
        }
        List<Integer> ans = new ArrayList<>();
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] > 0) {
                ans.add(i + 1);
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(n)`，额外空间 `O(1)`，不计答案。

#### 基础语法与算法思想

- 数值范围为 `1..n` 时，可以用下标 `num - 1` 表示该数字是否出现。
- 标记时用 `-Math.abs`，避免重复数字把负号翻回正数。
- 核心思想：原地标记用数组本身承载访问状态。

---

## 449. 序列化和反序列化二叉搜索树 (Medium)

序列化是将数据结构或对象转换为一系列位的过程，以便它可以存储在文件或内存缓冲区中，或通过网络连接链路传输，以便稍后在同一个或另一个计算机环境中重建。
设计一个算法来序列化和反序列化 **二叉搜索树**  。 对序列化/反序列化算法的工作方式没有限制。 您只需确保二叉搜索树可以序列化为字符串，并且可以将该字符串反序列化为最初的二叉搜索树。
 **编码的字符串应尽可能紧凑。** 
 
 **示例 1：** 

```text
输入：root = [2,1,3]
输出：[2,1,3]
```

 **示例 2：** 

```text
输入：root = []
输出：[]
```

 
 **提示：** 

树中节点数范围是  `[0, 104]` 
 `0 <= Node.val <= 104` 
题目数据  **保证**  输入的树是一棵二叉搜索树。

### Java 解法补充

#### 基础解法：带空标记的先序序列化

算法思想：普通二叉树通用做法：先序遍历时记录空节点标记，反序列化时按同样顺序重建。

```java
import java.util.ArrayDeque;
import java.util.Arrays;
import java.util.Queue;

public class Codec {
    public String serialize(TreeNode root) {
        StringBuilder builder = new StringBuilder();
        write(root, builder);
        return builder.toString();
    }

    private void write(TreeNode node, StringBuilder builder) {
        if (node == null) {
            builder.append("#,");
            return;
        }
        builder.append(node.val).append(',');
        write(node.left, builder);
        write(node.right, builder);
    }

    public TreeNode deserialize(String data) {
        Queue<String> queue = new ArrayDeque<>(Arrays.asList(data.split(",")));
        return read(queue);
    }

    private TreeNode read(Queue<String> queue) {
        String value = queue.poll();
        if (value.equals("#")) return null;
        TreeNode node = new TreeNode(Integer.parseInt(value));
        node.left = read(queue);
        node.right = read(queue);
        return node;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 资深解法：利用 BST 先序和边界

算法思想：BST 的先序序列不需要空标记。反序列化时按 `(lower, upper)` 边界消费序列，当前值不在边界内就属于上层或右侧子树。

```java
public class Codec {
    public String serialize(TreeNode root) {
        StringBuilder builder = new StringBuilder();
        preorder(root, builder);
        return builder.toString();
    }

    private void preorder(TreeNode node, StringBuilder builder) {
        if (node == null) return;
        builder.append(node.val).append(',');
        preorder(node.left, builder);
        preorder(node.right, builder);
    }

    public TreeNode deserialize(String data) {
        if (data.isEmpty()) return null;
        String[] parts = data.split(",");
        int[] index = {0};
        return build(parts, index, Integer.MIN_VALUE, Integer.MAX_VALUE);
    }

    private TreeNode build(String[] parts, int[] index, int low, int high) {
        if (index[0] == parts.length || parts[index[0]].isEmpty()) return null;
        int value = Integer.parseInt(parts[index[0]]);
        if (value < low || value > high) return null;
        index[0]++;
        TreeNode node = new TreeNode(value);
        node.left = build(parts, index, low, value);
        node.right = build(parts, index, value, high);
        return node;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(h)`。

#### 基础语法与算法思想

- 通用序列化需要空标记，BST 可以利用值域约束省掉空标记。
- `int[] index` 可在递归中共享当前消费位置。
- 核心思想：BST 左子树值小于根，右子树值大于根，反序列化可用边界控制。

---

## 450. 删除二叉搜索树中的节点 (Medium)

给定一个二叉搜索树的根节点  **root** 和一个值  **key** ，删除二叉搜索树中的  **key** 对应的节点，并保证二叉搜索树的性质不变。返回二叉搜索树（有可能被更新）的根节点的引用。
一般来说，删除节点可分为两个步骤：

首先找到需要删除的节点；
如果找到了，删除它。

 
 **示例 1:** 

```text
输入：root = [5,3,6,2,4,null,7], key = 3
输出：[5,4,6,2,null,null,7]
解释：给定需要删除的节点值是 3，所以我们首先找到 3 这个节点，然后删除它。
一个正确的答案是 [5,4,6,2,null,null,7], 如下图所示。
另一个正确答案是 [5,2,6,null,4,null,7]。
```

 **示例 2:** 

```text
输入: root = [5,3,6,2,4,null,7], key = 0
输出: [5,3,6,2,4,null,7]
解释: 二叉树不包含值为 0 的节点
```

 **示例 3:** 

```text
输入: root = [], key = 0
输出: []
```

 
 **提示:** 

节点数的范围  `[0, 104]` .
 `-105 <= Node.val <= 105` 
节点值唯一
 `root`  是合法的二叉搜索树
 `-105 <= key <= 105` 

 
 **进阶：**  要求算法时间复杂度为 O(h)，h 为树的高度。

### Java 解法补充

#### 基础解法：递归查找并删除

算法思想：利用 BST 性质查找节点。找到后，如果只有一个孩子，直接返回另一个孩子；如果有两个孩子，用右子树最小节点替换当前值，再删除那个最小节点。

```java
class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        if (root == null) return null;
        if (key < root.val) {
            root.left = deleteNode(root.left, key);
        } else if (key > root.val) {
            root.right = deleteNode(root.right, key);
        } else {
            if (root.left == null) return root.right;
            if (root.right == null) return root.left;
            TreeNode successor = root.right;
            while (successor.left != null) successor = successor.left;
            root.val = successor.val;
            root.right = deleteNode(root.right, successor.val);
        }
        return root;
    }
}
```

复杂度：时间 `O(h)`，空间 `O(h)`。

#### 资深解法：拼接左右子树

算法思想：删除有两个孩子的节点时，把左子树接到右子树最小节点的左侧，然后返回右子树作为新的根，避免复制节点值。

```java
class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        if (root == null) return null;
        if (key < root.val) {
            root.left = deleteNode(root.left, key);
            return root;
        }
        if (key > root.val) {
            root.right = deleteNode(root.right, key);
            return root;
        }
        if (root.left == null) return root.right;
        if (root.right == null) return root.left;

        TreeNode min = root.right;
        while (min.left != null) {
            min = min.left;
        }
        min.left = root.left;
        return root.right;
    }
}
```

复杂度：时间 `O(h)`，空间 `O(h)`。

#### 基础语法与算法思想

- BST 查找时，小于当前值走左子树，大于当前值走右子树。
- 删除双子节点时，可用中序后继维护有序性。
- 核心思想：删除节点后返回新的子树根，父节点用返回值重连。

---

