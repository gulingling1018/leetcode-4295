# LeetCode 题目合集 Part 14

## 391. 完美矩形 (Hard)

给你一个数组  `rectangles`  ，其中  `rectangles[i] = [xi, yi, ai, bi]`  表示一个坐标轴平行的矩形。这个矩形的左下顶点是  `(xi, yi)`  ，右上顶点是  `(ai, bi)`  。
如果所有矩形一起精确覆盖了某个矩形区域，则返回  `true`  ；否则，返回  `false`  。
 

 **示例 1：** 

```text
输入：rectangles = [[1,1,3,3],[3,1,4,2],[3,2,4,4],[1,3,2,4],[2,3,3,4]]
输出：true
解释：5 个矩形一起可以精确地覆盖一个矩形区域。
```

 **示例 2：** 

```text
输入：rectangles = [[1,1,2,3],[1,3,2,4],[3,1,4,2],[3,2,4,4]]
输出：false
解释：两个矩形之间有间隔，无法覆盖成一个矩形。
```

 **示例 3：** 

```text
输入：rectangles = [[1,1,3,3],[3,1,4,2],[1,3,2,4],[2,2,4,4]]
输出：false
解释：因为中间有相交区域，虽然形成了矩形，但不是精确覆盖。
```

 
 **提示：** 

 `1 <= rectangles.length <= 2 * 104` 
 `rectangles[i].length == 4` 
 `-105 <= xi < ai <= 105` 
 `-105 <= yi < bi <= 105`

### Java 解法补充

#### 基础解法：面积加角点抵消

算法思想：所有小矩形如果刚好拼成一个大矩形，那么小矩形面积之和等于外接大矩形面积，并且内部角点会成对或四个抵消，最后只剩外接矩形四个角点。

```java
import java.util.HashSet;
import java.util.Set;

class Solution {
    public boolean isRectangleCover(int[][] rectangles) {
        int minX = Integer.MAX_VALUE, minY = Integer.MAX_VALUE;
        int maxX = Integer.MIN_VALUE, maxY = Integer.MIN_VALUE;
        long area = 0;
        Set<String> corners = new HashSet<>();

        for (int[] r : rectangles) {
            minX = Math.min(minX, r[0]);
            minY = Math.min(minY, r[1]);
            maxX = Math.max(maxX, r[2]);
            maxY = Math.max(maxY, r[3]);
            area += (long) (r[2] - r[0]) * (r[3] - r[1]);
            toggle(corners, r[0], r[1]);
            toggle(corners, r[0], r[3]);
            toggle(corners, r[2], r[1]);
            toggle(corners, r[2], r[3]);
        }

        long outer = (long) (maxX - minX) * (maxY - minY);
        if (area != outer || corners.size() != 4) return false;
        return corners.contains(key(minX, minY)) && corners.contains(key(minX, maxY))
                && corners.contains(key(maxX, minY)) && corners.contains(key(maxX, maxY));
    }

    private void toggle(Set<String> set, int x, int y) {
        String key = key(x, y);
        if (!set.add(key)) set.remove(key);
    }

    private String key(int x, int y) {
        return x + "," + y;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 资深解法：角点编码减少对象创建

算法思想：仍使用面积和角点奇偶性，但把角点编码成 `long`，减少字符串拼接开销，更适合高频数据处理。

```java
import java.util.HashSet;
import java.util.Set;

class Solution {
    public boolean isRectangleCover(int[][] rectangles) {
        long area = 0;
        int minX = Integer.MAX_VALUE, minY = Integer.MAX_VALUE;
        int maxX = Integer.MIN_VALUE, maxY = Integer.MIN_VALUE;
        Set<Long> points = new HashSet<>();

        for (int[] r : rectangles) {
            minX = Math.min(minX, r[0]);
            minY = Math.min(minY, r[1]);
            maxX = Math.max(maxX, r[2]);
            maxY = Math.max(maxY, r[3]);
            area += (long) (r[2] - r[0]) * (r[3] - r[1]);
            flip(points, r[0], r[1]);
            flip(points, r[0], r[3]);
            flip(points, r[2], r[1]);
            flip(points, r[2], r[3]);
        }

        return area == (long) (maxX - minX) * (maxY - minY)
                && points.size() == 4
                && points.contains(code(minX, minY))
                && points.contains(code(minX, maxY))
                && points.contains(code(maxX, minY))
                && points.contains(code(maxX, maxY));
    }

    private void flip(Set<Long> points, int x, int y) {
        long p = code(x, y);
        if (!points.add(p)) points.remove(p);
    }

    private long code(int x, int y) {
        return (((long) x) << 32) ^ (y & 0xffffffffL);
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 基础语法与算法思想

- `long` 面积可避免坐标差相乘时溢出。
- 内部角点出现偶数次，使用集合“加入/删除”可以完成抵消。
- 核心思想：无缝无重叠覆盖同时满足“面积相等”和“角点正确”。

---

## 392. 判断子序列 (Easy)

给定字符串  **s**  和  **t**  ，判断  **s**  是否为  **t**  的子序列。
字符串的一个子序列是原始字符串删除一些（也可以不删除）字符而不改变剩余字符相对位置形成的新字符串。（例如， `"ace"` 是 `"abcde"` 的一个子序列，而 `"aec"` 不是）。
 **进阶：** 
如果有大量输入的 S，称作 S1, S2, ... , Sk 其中 k >= 10亿，你需要依次检查它们是否为 T 的子序列。在这种情况下，你会怎样改变代码？
 **致谢：** 
特别感谢 **** @pbrother 添加此问题并且创建所有测试用例。
 
 **示例 1：** 

```text
输入：s = "abc", t = "ahbgdc"
输出：true
```

 **示例 2：** 

```text
输入：s = "axc", t = "ahbgdc"
输出：false
```

 
 **提示：** 

 `0 <= s.length <= 100` 
 `0 <= t.length <= 10^4` 
两个字符串都只由小写字符组成。

### Java 解法补充

#### 基础解法：双指针

算法思想：一个指针扫 `s`，一个指针扫 `t`。当字符相等时推进 `s`，无论是否相等都推进 `t`。最后如果 `s` 被匹配完，就是子序列。

```java
class Solution {
    public boolean isSubsequence(String s, String t) {
        int i = 0;
        int j = 0;
        while (i < s.length() && j < t.length()) {
            if (s.charAt(i) == t.charAt(j)) {
                i++;
            }
            j++;
        }
        return i == s.length();
    }
}
```

复杂度：时间 `O(|t|)`，空间 `O(1)`。

#### 资深解法：预处理 next 表

算法思想：如果有大量 `s` 要查询同一个 `t`，可以预处理 `next[i][c]` 表示从 `t` 的位置 `i` 开始字符 `c` 下一次出现的位置。每次查询只需按 `s` 跳转。

```java
import java.util.Arrays;

class Solution {
    public boolean isSubsequence(String s, String t) {
        int n = t.length();
        int[][] next = new int[n + 1][26];
        Arrays.fill(next[n], -1);
        for (int i = n - 1; i >= 0; i--) {
            for (int c = 0; c < 26; c++) {
                next[i][c] = next[i + 1][c];
            }
            next[i][t.charAt(i) - 'a'] = i;
        }

        int pos = 0;
        for (int i = 0; i < s.length(); i++) {
            if (pos > n) return false;
            int found = next[pos][s.charAt(i) - 'a'];
            if (found == -1) return false;
            pos = found + 1;
        }
        return true;
    }
}
```

复杂度：预处理时间 `O(26|t|)`，单次查询 `O(|s|)`，空间 `O(26|t|)`。

#### 基础语法与算法思想

- 子序列只要求相对顺序，不要求连续。
- `Arrays.fill` 可初始化一整行默认值。
- 核心思想：单次查询用双指针；海量查询时把 `t` 建成可跳转索引。

---

## 393. UTF-8 编码验证 (Medium)

给定一个表示数据的整数数组  `data`  ，返回它是否为有效的  **UTF-8**  编码。
 **UTF-8**  中的一个字符可能的长度为  **1 到 4 字节** ，遵循以下的规则：

对于  **1 字节**  的字符，字节的第一位设为 0 ，后面 7 位为这个符号的 unicode 码。
对于  **n 字节**  的字符 (n > 1)，第一个字节的前 n 位都设为1，第 n+1 位设为 0 ，后面字节的前两位一律设为 10 。剩下的没有提及的二进制位，全部为这个符号的 unicode 码。

这是 UTF-8 编码的工作方式：

```text
Number of Bytes  |        UTF-8 octet sequence
                       |              (binary)
   --------------------+---------------------------------------------
            1          | 0xxxxxxx
            2          | 110xxxxx 10xxxxxx
            3          | 1110xxxx 10xxxxxx 10xxxxxx
            4          | 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx
```

 `x`  表示二进制形式的一位，可以是  `0`  或  `1` 。
 **注意：** 输入是整数数组。只有每个整数的  **最低 8 个有效位**  用来存储数据。这意味着每个整数只表示 1 字节的数据。
 
 **示例 1：** 

```text
输入：data = [197,130,1]
输出：true
解释：数据表示字节序列:11000101 10000010 00000001。
这是有效的 utf-8 编码，为一个 2 字节字符，跟着一个 1 字节字符。
```

 **示例 2：** 

```text
输入：data = [235,140,4]
输出：false
解释：数据表示 8 位的序列: 11101011 10001100 00000100.
前 3 位都是 1 ，第 4 位为 0 表示它是一个 3 字节字符。
下一个字节是开头为 10 的延续字节，这是正确的。
但第二个延续字节不以 10 开头，所以是不符合规则的。
```

 
 **提示:** 

 `1 <= data.length <= 2 * 104` 
 `0 <= data[i] <= 255`

### Java 解法补充

#### 基础解法：按首字节判断长度

算法思想：遍历数组，先根据首字节前缀判断当前字符需要几个字节，再检查后续字节是否都以二进制 `10` 开头。

```java
class Solution {
    public boolean validUtf8(int[] data) {
        int i = 0;
        while (i < data.length) {
            int bytes = bytesOf(data[i]);
            if (bytes == 0 || i + bytes > data.length) return false;
            for (int j = 1; j < bytes; j++) {
                if ((data[i + j] & 0b11000000) != 0b10000000) {
                    return false;
                }
            }
            i += bytes;
        }
        return true;
    }

    private int bytesOf(int value) {
        if ((value & 0b10000000) == 0) return 1;
        if ((value & 0b11100000) == 0b11000000) return 2;
        if ((value & 0b11110000) == 0b11100000) return 3;
        if ((value & 0b11111000) == 0b11110000) return 4;
        return 0;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 资深解法：维护剩余延续字节数

算法思想：流式扫描每个字节。若当前还需要延续字节，则必须以 `10` 开头；否则当前字节必须是合法首字节，并设置后续需要的字节数量。

```java
class Solution {
    public boolean validUtf8(int[] data) {
        int remaining = 0;
        for (int value : data) {
            if (remaining > 0) {
                if ((value & 0b11000000) != 0b10000000) return false;
                remaining--;
            } else if ((value & 0b10000000) == 0) {
                remaining = 0;
            } else if ((value & 0b11100000) == 0b11000000) {
                remaining = 1;
            } else if ((value & 0b11110000) == 0b11100000) {
                remaining = 2;
            } else if ((value & 0b11111000) == 0b11110000) {
                remaining = 3;
            } else {
                return false;
            }
        }
        return remaining == 0;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 二进制字面量 `0b11000000` 便于写位掩码。
- `&` 可检查某几位是否符合固定模式。
- 核心思想：编码验证题按状态机处理最稳。

---

## 394. 字符串解码 (Medium)

给定一个经过编码的字符串，返回它解码后的字符串。
编码规则为:  `k[encoded_string]` ，表示其中方括号内部的  `encoded_string`  正好重复  `k`  次。注意  `k`  保证为正整数。
你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。
此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数  `k`  ，例如不会出现像  `3a`  或  `2[4]`  的输入。
测试用例保证输出的长度不会超过  `105` 。
 
 **示例 1：** 

```text
输入：s = "3[a]2[bc]"
输出："aaabcbc"
```

 **示例 2：** 

```text
输入：s = "3[a2[c]]"
输出："accaccacc"
```

 **示例 3：** 

```text
输入：s = "2[abc]3[cd]ef"
输出："abcabccdcdcdef"
```

 **示例 4：** 

```text
输入：s = "abc3[cd]xyz"
输出："abccdcdcdxyz"
```

 
 **提示：** 

 `1 <= s.length <= 30` 
 `s`  由小写英文字母、数字和方括号  `'[]'`  组成
 `s`  保证是一个  **有效**  的输入。
 `s`  中所有整数的取值范围为  `[1, 300]`

### Java 解法补充

#### 基础解法：递归解析括号

算法思想：遇到普通字符直接追加；遇到数字就解析重复次数，跳过 `'['` 后递归解码内部字符串，直到遇到对应 `']'`。

```java
class Solution {
    public String decodeString(String s) {
        int[] index = {0};
        return decode(s, index);
    }

    private String decode(String s, int[] index) {
        StringBuilder ans = new StringBuilder();
        while (index[0] < s.length() && s.charAt(index[0]) != ']') {
            char c = s.charAt(index[0]);
            if (Character.isLetter(c)) {
                ans.append(c);
                index[0]++;
            } else {
                int count = 0;
                while (Character.isDigit(s.charAt(index[0]))) {
                    count = count * 10 + s.charAt(index[0]) - '0';
                    index[0]++;
                }
                index[0]++;
                String inner = decode(s, index);
                index[0]++;
                for (int i = 0; i < count; i++) {
                    ans.append(inner);
                }
            }
        }
        return ans.toString();
    }
}
```

复杂度：时间 `O(output)`，空间 `O(depth)`。

#### 资深解法：双栈迭代

算法思想：数字入次数栈，遇到 `'['` 时保存当前字符串并开始新片段；遇到 `']'` 时弹出上一层字符串和次数，拼接后回到上一层。

```java
import java.util.ArrayDeque;
import java.util.Deque;

class Solution {
    public String decodeString(String s) {
        Deque<Integer> counts = new ArrayDeque<>();
        Deque<StringBuilder> builders = new ArrayDeque<>();
        StringBuilder cur = new StringBuilder();
        int count = 0;

        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (Character.isDigit(c)) {
                count = count * 10 + c - '0';
            } else if (c == '[') {
                counts.push(count);
                builders.push(cur);
                cur = new StringBuilder();
                count = 0;
            } else if (c == ']') {
                StringBuilder parent = builders.pop();
                int repeat = counts.pop();
                for (int r = 0; r < repeat; r++) {
                    parent.append(cur);
                }
                cur = parent;
            } else {
                cur.append(c);
            }
        }
        return cur.toString();
    }
}
```

复杂度：时间 `O(output)`，空间 `O(depth + output)`。

#### 基础语法与算法思想

- `StringBuilder` 适合频繁拼接字符串。
- 栈可以保存进入下一层括号前的上下文。
- 核心思想：嵌套表达式要么递归解析，要么用栈保存层级状态。

---

## 395. 至少有 K 个重复字符的最长子串 (Medium)

给你一个字符串  `s`  和一个整数  `k`  ，请你找出  `s`  中的最长子串， 要求该子串中的每一字符出现次数都不少于  `k`  。返回这一子串的长度。
如果不存在这样的子字符串，则返回 0。
 
 **示例 1：** 

```text
输入：s = "aaabb", k = 3
输出：3
解释：最长子串为 "aaa" ，其中 'a' 重复了 3 次。
```

 **示例 2：** 

```text
输入：s = "ababbc", k = 2
输出：5
解释：最长子串为 "ababb" ，其中 'a' 重复了 2 次， 'b' 重复了 3 次。
```

 
 **提示：** 

 `1 <= s.length <= 104` 
 `s`  仅由小写英文字母组成
 `1 <= k <= 105`

### Java 解法补充

#### 基础解法：枚举所有子串并统计

算法思想：枚举子串起点和终点，用计数数组维护当前子串字符频次，每次检查所有出现过的字符是否都不少于 `k`。

```java
class Solution {
    public int longestSubstring(String s, int k) {
        int ans = 0;
        for (int left = 0; left < s.length(); left++) {
            int[] count = new int[26];
            for (int right = left; right < s.length(); right++) {
                count[s.charAt(right) - 'a']++;
                if (valid(count, k)) {
                    ans = Math.max(ans, right - left + 1);
                }
            }
        }
        return ans;
    }

    private boolean valid(int[] count, int k) {
        for (int c : count) {
            if (c > 0 && c < k) return false;
        }
        return true;
    }
}
```

复杂度：时间 `O(26n^2)`，空间 `O(1)`。

#### 资深解法：分治切分无效字符

算法思想：如果某个字符在当前区间总次数小于 `k`，那么任何合法子串都不能包含它。用这些字符作为切分点，递归求左右子区间答案。

```java
class Solution {
    public int longestSubstring(String s, int k) {
        return solve(s, 0, s.length(), k);
    }

    private int solve(String s, int left, int right, int k) {
        if (right - left < k) return 0;
        int[] count = new int[26];
        for (int i = left; i < right; i++) {
            count[s.charAt(i) - 'a']++;
        }
        for (int i = left; i < right; i++) {
            if (count[s.charAt(i) - 'a'] < k) {
                int next = i + 1;
                while (next < right && count[s.charAt(next) - 'a'] < k) {
                    next++;
                }
                return Math.max(solve(s, left, i, k), solve(s, next, right, k));
            }
        }
        return right - left;
    }
}
```

复杂度：平均时间接近 `O(26n)`，最坏 `O(n^2)`，空间 `O(depth)`。

#### 基础语法与算法思想

- 子串要求连续，切分字符一旦不可用，左右区间可独立求解。
- `right` 使用开区间便于递归边界处理。
- 核心思想：合法子串不能包含总频次不足 `k` 的字符。

---

## 396. 旋转函数 (Medium)

给定一个长度为  `n`  的整数数组  `nums`  。
假设  `arrk`  是数组  `nums`  顺时针旋转  `k`  个位置后的数组，我们定义  `nums`  的  **旋转函数**    `F`  为：

 `F(k) = 0 * arrk[0] + 1 * arrk[1] + ... + (n - 1) * arrk[n - 1]` 

返回  `F(0), F(1), ..., F(n-1)` 中的最大值 。
生成的测试用例让答案符合  **32 位**  整数。
 
 **示例 1:** 

```text
输入: nums = [4,3,2,6]
输出: 26
解释:
F(0) = (0 * 4) + (1 * 3) + (2 * 2) + (3 * 6) = 0 + 3 + 4 + 18 = 25
F(1) = (0 * 6) + (1 * 4) + (2 * 3) + (3 * 2) = 0 + 4 + 6 + 6 = 16
F(2) = (0 * 2) + (1 * 6) + (2 * 4) + (3 * 3) = 0 + 6 + 8 + 9 = 23
F(3) = (0 * 3) + (1 * 2) + (2 * 6) + (3 * 4) = 0 + 2 + 12 + 12 = 26
所以 F(0), F(1), F(2), F(3) 中的最大值是 F(3) = 26 。
```

 **示例 2:** 

```text
输入: nums = [100]
输出: 0
```

 
 **提示:** 

 `n == nums.length` 
 `1 <= n <= 105` 
 `-100 <= nums[i] <= 100`

### Java 解法补充

#### 基础解法：逐次旋转后计算

算法思想：对每个 `k` 都按定义计算旋转后的 `F(k)`，维护最大值。

```java
class Solution {
    public int maxRotateFunction(int[] nums) {
        int n = nums.length;
        int ans = Integer.MIN_VALUE;
        for (int k = 0; k < n; k++) {
            int value = 0;
            for (int i = 0; i < n; i++) {
                int originalIndex = (i - k + n) % n;
                value += i * nums[originalIndex];
            }
            ans = Math.max(ans, value);
        }
        return ans;
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(1)`。

#### 资深解法：使用递推公式

算法思想：设数组总和为 `sum`，长度为 `n`。从 `F(k - 1)` 到 `F(k)` 时，最后一个被旋到开头的元素是 `nums[n - k]`，有 `F(k) = F(k - 1) + sum - n * nums[n - k]`。

```java
class Solution {
    public int maxRotateFunction(int[] nums) {
        int n = nums.length;
        int sum = 0;
        int current = 0;
        for (int i = 0; i < n; i++) {
            sum += nums[i];
            current += i * nums[i];
        }

        int ans = current;
        for (int k = 1; k < n; k++) {
            current = current + sum - n * nums[n - k];
            ans = Math.max(ans, current);
        }
        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- `% n` 可把旋转下标映射回原数组范围。
- 推导相邻状态公式能把重复计算降到一次扫描。
- 核心思想：旋转后的函数值之间有强关联，不必每次从头算。

---

## 397. 整数替换 (Medium)

给定一个正整数  `n`  ，你可以做如下操作：

如果  `n`  是偶数，则用  `n / 2` 替换  `n`  。
如果  `n`  是奇数，则可以用  `n + 1` 或 `n - 1` 替换  `n`  。

返回  `n`  变为  `1`  所需的 最小替换次数 。
 
 **示例 1：** 

```text
输入：n = 8
输出：3
解释：8 -> 4 -> 2 -> 1
```

 **示例 2：** 

```text
输入：n = 7
输出：4
解释：7 -> 8 -> 4 -> 2 -> 1
或 7 -> 6 -> 3 -> 2 -> 1
```

 **示例 3：** 

```text
输入：n = 4
输出：2
```

 
 **提示：** 

 `1 <= n <= 231 - 1`

### Java 解法补充

#### 基础解法：递归搜索并记忆化

算法思想：偶数只能除以 2；奇数可以加一或减一。用 `long` 避免 `Integer.MAX_VALUE + 1` 溢出，并用哈希表缓存子问题。

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    private final Map<Long, Integer> memo = new HashMap<>();

    public int integerReplacement(int n) {
        return dfs(n);
    }

    private int dfs(long n) {
        if (n == 1) return 0;
        if (memo.containsKey(n)) return memo.get(n);
        int ans;
        if (n % 2 == 0) {
            ans = 1 + dfs(n / 2);
        } else {
            ans = 1 + Math.min(dfs(n - 1), dfs(n + 1));
        }
        memo.put(n, ans);
        return ans;
    }
}
```

复杂度：时间 `O(log n)` 级别，空间 `O(log n)`。

#### 资深解法：位运算贪心

算法思想：偶数右移。奇数时，除了 `3` 应选择减一，其余如果低两位是 `11`，加一能制造更多连续 0，后续可连续除以 2；否则减一。

```java
class Solution {
    public int integerReplacement(int n) {
        long x = n;
        int steps = 0;
        while (x != 1) {
            if ((x & 1) == 0) {
                x >>= 1;
            } else if (x == 3 || (x & 3) == 1) {
                x--;
            } else {
                x++;
            }
            steps++;
        }
        return steps;
    }
}
```

复杂度：时间 `O(log n)`，空间 `O(1)`。

#### 基础语法与算法思想

- `long x = n` 防止 `2147483647 + 1` 溢出。
- `(x & 3)` 可以观察最低两位。
- 核心思想：奇数加减的选择目标是尽快制造更多可除以 2 的机会。

---

## 398. 随机数索引 (Medium)

给你一个可能含有  **重复元素**  的整数数组  `nums`  ，请你随机输出给定的目标数字  `target`  的索引。你可以假设给定的数字一定存在于数组中。
实现  `Solution`  类：

 `Solution(int[] nums)`  用数组  `nums`  初始化对象。
 `int pick(int target)`  从  `nums`  中选出一个满足  `nums[i] == target`  的随机索引  `i`  。如果存在多个有效的索引，则每个索引的返回概率应当相等。

 
 **示例：** 

```text
输入
["Solution", "pick", "pick", "pick"]
[[[1, 2, 3, 3, 3]], [3], [1], [3]]
输出
[null, 4, 0, 2]

解释
Solution solution = new Solution([1, 2, 3, 3, 3]);
solution.pick(3); // 随机返回索引 2, 3 或者 4 之一。每个索引的返回概率应该相等。
solution.pick(1); // 返回 0 。因为只有 nums[0] 等于 1 。
solution.pick(3); // 随机返回索引 2, 3 或者 4 之一。每个索引的返回概率应该相等。
```

 

 **提示：** 

 `1 <= nums.length <= 2 * 104` 
 `-231 <= nums[i] <= 231 - 1` 
 `target`  是  `nums`  中的一个整数
最多调用  `pick`  函数  `104`  次

### Java 解法补充

#### 基础解法：值到下标列表

算法思想：构造时把每个值出现的下标都保存到哈希表中。`pick` 时从目标值对应的下标列表里随机选一个。

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Random;

class Solution {
    private final Map<Integer, List<Integer>> index = new HashMap<>();
    private final Random random = new Random();

    public Solution(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
            index.computeIfAbsent(nums[i], key -> new ArrayList<>()).add(i);
        }
    }

    public int pick(int target) {
        List<Integer> list = index.get(target);
        return list.get(random.nextInt(list.size()));
    }
}
```

复杂度：初始化时间和空间 `O(n)`，查询时间 `O(1)`。

#### 资深解法：蓄水池抽样

算法思想：如果不想为所有值保存下标列表，`pick` 时扫描数组。第 `count` 次遇到目标值时，以 `1 / count` 的概率替换答案，最终每个目标下标等概率。

```java
import java.util.Random;

class Solution {
    private final int[] nums;
    private final Random random = new Random();

    public Solution(int[] nums) {
        this.nums = nums;
    }

    public int pick(int target) {
        int ans = -1;
        int count = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == target) {
                count++;
                if (random.nextInt(count) == 0) {
                    ans = i;
                }
            }
        }
        return ans;
    }
}
```

复杂度：查询时间 `O(n)`，额外空间 `O(1)`。

#### 基础语法与算法思想

- `computeIfAbsent` 可以简化哈希表中列表的创建。
- 多个等概率候选可用随机下标，也可用蓄水池抽样。
- 核心思想：空间换时间版适合查询多，蓄水池版适合节省内存。

---

## 399. 除法求值 (Medium)

给你一个变量对数组  `equations`  和一个实数值数组  `values`  作为已知条件，其中  `equations[i] = [Ai, Bi]`  和  `values[i]`  共同表示等式  `Ai / Bi = values[i]`  。每个  `Ai`  或  `Bi`  是一个表示单个变量的字符串。
另有一些以数组  `queries`  表示的问题，其中  `queries[j] = [Cj, Dj]`  表示第  `j`  个问题，请你根据已知条件找出  `Cj / Dj = ?`  的结果作为答案。
返回  **所有问题的答案**  。如果存在某个无法确定的答案，则用  `-1.0`  替代这个答案。如果问题中出现了给定的已知条件中没有出现的字符串，也需要用  `-1.0`  替代这个答案。
 **注意：** 输入总是有效的。你可以假设除法运算中不会出现除数为 0 的情况，且不存在任何矛盾的结果。
 **注意：** 未在等式列表中出现的变量是未定义的，因此无法确定它们的答案。
 
 **示例 1：** 

```text
输入：equations = [["a","b"],["b","c"]], values = [2.0,3.0], queries = [["a","c"],["b","a"],["a","e"],["a","a"],["x","x"]]
输出：[6.00000,0.50000,-1.00000,1.00000,-1.00000]
解释：
条件：a / b = 2.0, b / c = 3.0
问题：a / c = ?, b / a = ?, a / e = ?, a / a = ?, x / x = ?
结果：[6.0, 0.5, -1.0, 1.0, -1.0 ]
注意：x 是未定义的 => -1.0
```

 **示例 2：** 

```text
输入：equations = [["a","b"],["b","c"],["bc","cd"]], values = [1.5,2.5,5.0], queries = [["a","c"],["c","b"],["bc","cd"],["cd","bc"]]
输出：[3.75000,0.40000,5.00000,0.20000]
```

 **示例 3：** 

```text
输入：equations = [["a","b"]], values = [0.5], queries = [["a","b"],["b","a"],["a","c"],["x","y"]]
输出：[0.50000,2.00000,-1.00000,-1.00000]
```

 
 **提示：** 

 `1 <= equations.length <= 20` 
 `equations[i].length == 2` 
 `1 <= Ai.length, Bi.length <= 5` 
 `values.length == equations.length` 
 `0.0 < values[i] <= 20.0` 
 `1 <= queries.length <= 20` 
 `queries[i].length == 2` 
 `1 <= Cj.length, Dj.length <= 5` 
 `Ai, Bi, Cj, Dj`  由小写英文字母与数字组成

### Java 解法补充

#### 基础解法：建图后 DFS

算法思想：把 `a / b = v` 看成有向边 `a -> b` 权重 `v`，反向边 `b -> a` 权重 `1/v`。查询时从起点 DFS 到终点，沿途乘边权。

```java
import java.util.HashMap;
import java.util.HashSet;
import java.util.List;
import java.util.Map;
import java.util.Set;

class Solution {
    public double[] calcEquation(List<List<String>> equations, double[] values,
                                 List<List<String>> queries) {
        Map<String, Map<String, Double>> graph = new HashMap<>();
        for (int i = 0; i < equations.size(); i++) {
            String a = equations.get(i).get(0);
            String b = equations.get(i).get(1);
            graph.computeIfAbsent(a, key -> new HashMap<>()).put(b, values[i]);
            graph.computeIfAbsent(b, key -> new HashMap<>()).put(a, 1.0 / values[i]);
        }

        double[] ans = new double[queries.size()];
        for (int i = 0; i < queries.size(); i++) {
            String from = queries.get(i).get(0);
            String to = queries.get(i).get(1);
            ans[i] = dfs(graph, from, to, new HashSet<>(), 1.0);
        }
        return ans;
    }

    private double dfs(Map<String, Map<String, Double>> graph, String from, String to,
                       Set<String> seen, double product) {
        if (!graph.containsKey(from) || !seen.add(from)) return -1.0;
        if (from.equals(to)) return product;
        for (Map.Entry<String, Double> edge : graph.get(from).entrySet()) {
            double result = dfs(graph, edge.getKey(), to, seen, product * edge.getValue());
            if (result != -1.0) return result;
        }
        return -1.0;
    }
}
```

复杂度：单次查询时间 `O(V+E)`，空间 `O(V+E)`。

#### 资深解法：带权并查集

算法思想：用并查集维护变量连通性，`weight[x]` 表示 `x / parent[x]`。合并时根据 `a / b = value` 调整根节点权重，查询时若同根则返回 `weight[a] / weight[b]`。

```java
import java.util.HashMap;
import java.util.List;
import java.util.Map;

class Solution {
    private final Map<String, String> parent = new HashMap<>();
    private final Map<String, Double> weight = new HashMap<>();

    public double[] calcEquation(List<List<String>> equations, double[] values,
                                 List<List<String>> queries) {
        for (int i = 0; i < equations.size(); i++) {
            union(equations.get(i).get(0), equations.get(i).get(1), values[i]);
        }
        double[] ans = new double[queries.size()];
        for (int i = 0; i < queries.size(); i++) {
            String a = queries.get(i).get(0);
            String b = queries.get(i).get(1);
            if (!parent.containsKey(a) || !parent.containsKey(b) || !find(a).equals(find(b))) {
                ans[i] = -1.0;
            } else {
                ans[i] = weight.get(a) / weight.get(b);
            }
        }
        return ans;
    }

    private void union(String a, String b, double value) {
        add(a);
        add(b);
        String rootA = find(a);
        String rootB = find(b);
        if (rootA.equals(rootB)) return;
        parent.put(rootA, rootB);
        weight.put(rootA, value * weight.get(b) / weight.get(a));
    }

    private void add(String x) {
        parent.putIfAbsent(x, x);
        weight.putIfAbsent(x, 1.0);
    }

    private String find(String x) {
        if (!parent.get(x).equals(x)) {
            String oldParent = parent.get(x);
            String root = find(oldParent);
            weight.put(x, weight.get(x) * weight.get(oldParent));
            parent.put(x, root);
        }
        return parent.get(x);
    }
}
```

复杂度：构建和查询近似 `O((E+Q) α(V))`，空间 `O(V)`。

#### 基础语法与算法思想

- 带权图适合直接搜索路径乘积。
- 并查集适合大量连通性和比例查询。
- 核心思想：除法关系可以视为图边权，也可以压缩成集合内相对根节点的比例。

---

## 400. 第 N 位数字 (Medium)

给你一个整数  `n`  ，请你在无限的整数序列  `[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ...]`  中找出并返回第  `n`  位上的数字。
 
 **示例 1：** 

```text
输入：n = 3
输出：3
```

 **示例 2：** 

```text
输入：n = 11
输出：0
解释：第 11 位数字在序列 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ... 里是 0 ，它是 10 的一部分。
```

 
 **提示：** 

 `1 <= n <= 231 - 1`

### Java 解法补充

#### 基础解法：拼接字符串后取位

算法思想：从 1 开始不断拼接数字字符串，直到长度至少为 `n`，再返回第 `n - 1` 个字符。这个写法只适合理解序列。

```java
class Solution {
    public int findNthDigit(int n) {
        StringBuilder builder = new StringBuilder();
        int num = 1;
        while (builder.length() < n) {
            builder.append(num);
            num++;
        }
        return builder.charAt(n - 1) - '0';
    }
}
```

复杂度：时间和空间 `O(n)`。

#### 资深解法：按数字位数定位

算法思想：一位数有 9 个、两位数有 90 个、三位数有 900 个。先确定第 `n` 位落在哪个位数段，再定位具体数字和数字内部的下标。

```java
class Solution {
    public int findNthDigit(int n) {
        long count = 9;
        int digits = 1;
        long start = 1;

        while (n > count * digits) {
            n -= count * digits;
            digits++;
            count *= 10;
            start *= 10;
        }

        long number = start + (n - 1) / digits;
        int index = (n - 1) % digits;
        return Long.toString(number).charAt(index) - '0';
    }
}
```

复杂度：时间 `O(log n)`，空间 `O(1)`。

#### 基础语法与算法思想

- `(n - 1) / digits` 用来定位第几个数字，`(n - 1) % digits` 定位数字内第几位。
- 位数段统计时要用 `long` 防止乘法溢出。
- 核心思想：无限序列题通常按长度分组定位，而不是实际生成。

---

## 401. 二进制手表 (Easy)

二进制手表顶部有 4 个 LED 代表 **小时（0-11）** ，底部的 6 个 LED 代表 **分钟（0-59）** 。每个 LED 代表一个 0 或 1，最低位在右侧。

例如，下面的二进制手表读取  `"4:51"`  。

给你一个整数  `turnedOn`  ，表示当前亮着的 LED 的数量，返回二进制手表可以表示的所有可能时间。你可以  **按任意顺序**  返回答案。
小时不会以零开头：

例如， `"01:00"`  是无效的时间，正确的写法应该是  `"1:00"`  。

分钟必须由两位数组成，可能会以零开头：

例如， `"10:2"`  是无效的时间，正确的写法应该是  `"10:02"`  。

 
 **示例 1：** 

```text
输入：turnedOn = 1
输出：["0:01","0:02","0:04","0:08","0:16","0:32","1:00","2:00","4:00","8:00"]
```

 **示例 2：** 

```text
输入：turnedOn = 9
输出：[]
```

 
 **提示：** 

 `0 <= turnedOn <= 10`

### Java 解法补充

#### 基础解法：枚举小时和分钟

算法思想：小时范围 `0..11`，分钟范围 `0..59`，枚举所有合法时间，统计二进制中 1 的数量是否等于 `turnedOn`。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<String> readBinaryWatch(int turnedOn) {
        List<String> ans = new ArrayList<>();
        for (int hour = 0; hour < 12; hour++) {
            for (int minute = 0; minute < 60; minute++) {
                if (Integer.bitCount(hour) + Integer.bitCount(minute) == turnedOn) {
                    ans.add(hour + ":" + (minute < 10 ? "0" : "") + minute);
                }
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(1)`，空间 `O(1)`，枚举范围固定。

#### 资深解法：按 LED 位置回溯

算法思想：10 个 LED 中前 4 个表示小时，后 6 个表示分钟。回溯选择亮灯位置，生成后过滤非法时间。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    private final int[] values = {1, 2, 4, 8, 1, 2, 4, 8, 16, 32};

    public List<String> readBinaryWatch(int turnedOn) {
        List<String> ans = new ArrayList<>();
        backtrack(0, turnedOn, 0, 0, ans);
        return ans;
    }

    private void backtrack(int index, int left, int hour, int minute, List<String> ans) {
        if (hour > 11 || minute > 59) return;
        if (left == 0) {
            ans.add(hour + ":" + (minute < 10 ? "0" : "") + minute);
            return;
        }
        for (int i = index; i < 10; i++) {
            if (i < 4) {
                backtrack(i + 1, left - 1, hour + values[i], minute, ans);
            } else {
                backtrack(i + 1, left - 1, hour, minute + values[i], ans);
            }
        }
    }
}
```

复杂度：时间 `O(C(10, turnedOn))`，空间 `O(turnedOn)`。

#### 基础语法与算法思想

- `Integer.bitCount(x)` 返回整数二进制中 1 的个数。
- 分钟不足两位时需要手动补 0。
- 核心思想：固定小范围枚举往往比复杂公式更可靠。

---

## 402. 移掉 K 位数字 (Medium)

给你一个以字符串表示的非负整数  `num`  和一个整数  `k`  ，移除这个数中的  `k`  位数字，使得剩下的数字最小。请你以字符串形式返回这个最小的数字。
 

 **示例 1 ：** 

```text
输入：num = "1432219", k = 3
输出："1219"
解释：移除掉三个数字 4, 3, 和 2 形成一个新的最小的数字 1219 。
```

 **示例 2 ：** 

```text
输入：num = "10200", k = 1
输出："200"
解释：移掉首位的 1 剩下的数字为 200. 注意输出不能有任何前导零。
```

 **示例 3 ：** 

```text
输入：num = "10", k = 2
输出："0"
解释：从原数字移除所有的数字，剩余为空就是 0 。
```

 
 **提示：** 

 `1 <= k <= num.length <= 105` 
 `num`  仅由若干位数字（0 - 9）组成
除了  **0**  本身之外， `num`  不含任何前导零

### Java 解法补充

#### 基础解法：每次删除第一个下降前的数字

算法思想：要让数字最小，每次从左往右找到第一个比右边大的数字并删除；如果整个字符串递增，就删除最后一位。重复 `k` 次。

```java
class Solution {
    public String removeKdigits(String num, int k) {
        StringBuilder builder = new StringBuilder(num);
        for (int removed = 0; removed < k; removed++) {
            int i = 0;
            while (i + 1 < builder.length() && builder.charAt(i) <= builder.charAt(i + 1)) {
                i++;
            }
            builder.deleteCharAt(i);
        }
        while (builder.length() > 1 && builder.charAt(0) == '0') {
            builder.deleteCharAt(0);
        }
        return builder.length() == 0 ? "0" : builder.toString();
    }
}
```

复杂度：时间 `O(kn)`，空间 `O(n)`。

#### 资深解法：单调栈

算法思想：维护一个递增栈。当前数字小于栈顶时，删除栈顶可以让高位更小。处理完后若还没删够，就从尾部继续删，最后去掉前导零。

```java
class Solution {
    public String removeKdigits(String num, int k) {
        StringBuilder stack = new StringBuilder();
        for (int i = 0; i < num.length(); i++) {
            char c = num.charAt(i);
            while (k > 0 && stack.length() > 0 && stack.charAt(stack.length() - 1) > c) {
                stack.deleteCharAt(stack.length() - 1);
                k--;
            }
            stack.append(c);
        }
        while (k > 0 && stack.length() > 0) {
            stack.deleteCharAt(stack.length() - 1);
            k--;
        }

        int start = 0;
        while (start < stack.length() && stack.charAt(start) == '0') {
            start++;
        }
        String ans = stack.substring(start);
        return ans.isEmpty() ? "0" : ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 基础语法与算法思想

- `deleteCharAt` 可以删除指定位置字符。
- 高位数字越小，整个数字越小。
- 核心思想：移除数字形成最小数，是典型单调栈问题。

---

## 403. 青蛙过河 (Hard)

一只青蛙想要过河。 假定河流被等分为若干个单元格，并且在每一个单元格内都有可能放有一块石子（也有可能没有）。 青蛙可以跳上石子，但是不可以跳入水中。
给你石子的位置列表  `stones` （用单元格序号  **升序**  表示）， 请判定青蛙能否成功过河（即能否在最后一步跳至最后一块石子上）。开始时， 青蛙默认已站在第一块石子上，并可以假定它第一步只能跳跃  `1`  个单位（即只能从单元格 1 跳至单元格 2 ）。
如果青蛙上一步跳跃了  `k`  个单位，那么它接下来的跳跃距离只能选择为  `k - 1` 、 `k`  或  `k + 1`  个单位。 另请注意，青蛙只能向前方（终点的方向）跳跃。
 
 **示例 1：** 

```text
输入：stones = [0,1,3,5,6,8,12,17]
输出：true
解释：青蛙可以成功过河，按照如下方案跳跃：跳 1 个单位到第 2 块石子, 然后跳 2 个单位到第 3 块石子, 接着 跳 2 个单位到第 4 块石子, 然后跳 3 个单位到第 6 块石子, 跳 4 个单位到第 7 块石子, 最后，跳 5 个单位到第 8 个石子（即最后一块石子）。
```

 **示例 2：** 

```text
输入：stones = [0,1,2,3,4,8,9,11]
输出：false
解释：这是因为第 5 和第 6 个石子之间的间距太大，没有可选的方案供青蛙跳跃过去。
```

 
 **提示：** 

 `2 <= stones.length <= 2000` 
 `0 <= stones[i] <= 231 - 1` 
 `stones[0] == 0` 
 `stones`  按严格升序排列

### Java 解法补充

#### 基础解法：DFS 加记忆化

算法思想：状态是“当前石头位置”和“上一次跳跃距离”。从当前状态尝试 `k - 1`、`k`、`k + 1` 三种下一跳，能到最后一块石头就成功。

```java
import java.util.HashMap;
import java.util.HashSet;
import java.util.Map;
import java.util.Set;

class Solution {
    private Set<Integer> stonesSet;
    private Map<String, Boolean> memo;
    private int target;

    public boolean canCross(int[] stones) {
        stonesSet = new HashSet<>();
        memo = new HashMap<>();
        for (int stone : stones) stonesSet.add(stone);
        target = stones[stones.length - 1];
        return dfs(0, 0);
    }

    private boolean dfs(int position, int jump) {
        if (position == target) return true;
        String key = position + "," + jump;
        if (memo.containsKey(key)) return memo.get(key);
        for (int nextJump = jump - 1; nextJump <= jump + 1; nextJump++) {
            if (nextJump > 0 && stonesSet.contains(position + nextJump)
                    && dfs(position + nextJump, nextJump)) {
                memo.put(key, true);
                return true;
            }
        }
        memo.put(key, false);
        return false;
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(n^2)`。

#### 资深解法：每块石头维护可达步长

算法思想：用哈希表记录每个石头位置能以哪些跳跃距离到达。遍历石头和可达步长，把下一步能落到的石头更新进去。

```java
import java.util.HashMap;
import java.util.HashSet;
import java.util.Map;
import java.util.Set;

class Solution {
    public boolean canCross(int[] stones) {
        Map<Integer, Set<Integer>> jumps = new HashMap<>();
        for (int stone : stones) {
            jumps.put(stone, new HashSet<>());
        }
        jumps.get(0).add(0);

        for (int stone : stones) {
            for (int jump : jumps.get(stone)) {
                for (int next = jump - 1; next <= jump + 1; next++) {
                    if (next > 0 && jumps.containsKey(stone + next)) {
                        jumps.get(stone + next).add(next);
                    }
                }
            }
        }
        return !jumps.get(stones[stones.length - 1]).isEmpty();
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(n^2)`。

#### 基础语法与算法思想

- `Map<Integer, Set<Integer>>` 可以表示每个石头上的可达状态集合。
- 跳跃距离不能为 0 或负数。
- 核心思想：路径存在性题要把“位置 + 上一步距离”一起作为状态。

---

## 404. 左叶子之和 (Easy)

给定二叉树的根节点  `root`  ，返回所有左叶子之和。
 
 **示例 1：** 

```text
输入: root = [3,9,20,null,null,15,7] 
输出: 24 
解释: 在这个二叉树中，有两个左叶子，分别是 9 和 15，所以返回 24
```

 **示例 2:** 

```text
输入: root = [1]
输出: 0
```

 
 **提示:** 

节点数在  `[1, 1000]`  范围内
 `-1000 <= Node.val <= 1000`

### Java 解法补充

#### 基础解法：递归传入是否左孩子

算法思想：DFS 遍历树，递归参数记录当前节点是否是父节点的左孩子。如果当前节点是叶子且是左孩子，就累加它的值。

```java
class Solution {
    public int sumOfLeftLeaves(TreeNode root) {
        return dfs(root, false);
    }

    private int dfs(TreeNode node, boolean isLeft) {
        if (node == null) return 0;
        if (node.left == null && node.right == null) {
            return isLeft ? node.val : 0;
        }
        return dfs(node.left, true) + dfs(node.right, false);
    }
}
```

复杂度：时间 `O(n)`，空间 `O(h)`。

#### 资深解法：在父节点处判断左叶子

算法思想：访问每个父节点时，如果它的左孩子是叶子，直接加左孩子值；否则继续递归左子树。右子树照常递归。

```java
class Solution {
    public int sumOfLeftLeaves(TreeNode root) {
        if (root == null) return 0;
        int sum = 0;
        if (root.left != null) {
            if (root.left.left == null && root.left.right == null) {
                sum += root.left.val;
            } else {
                sum += sumOfLeftLeaves(root.left);
            }
        }
        return sum + sumOfLeftLeaves(root.right);
    }
}
```

复杂度：时间 `O(n)`，空间 `O(h)`。

#### 基础语法与算法思想

- 叶子节点是左右孩子都为空的节点。
- “左叶子”需要同时知道节点位置和节点是否为叶子。
- 核心思想：树题常可以在当前节点处理孩子节点的特征。

---

## 405. 数字转换为十六进制数 (Easy)

给定一个整数，编写一个算法将这个数转换为十六进制数。对于负整数，我们通常使用 补码运算 方法。
答案字符串中的所有字母都应该是小写字符，并且除了 0 本身之外，答案中不应该有任何前置零。
 **注意:** 不允许使用任何由库提供的将数字直接转换或格式化为十六进制的方法来解决这个问题。
 
 **示例 1：** 

```text
输入：num = 26
输出："1a"
```

 **示例 2：** 

```text
输入：num = -1
输出："ffffffff"
```

 
 **提示：** 

 `-231 <= num <= 231 - 1`

### Java 解法补充

#### 基础解法：每 4 位取一位十六进制

算法思想：十六进制一位对应二进制 4 位。每次取 `num & 15` 得到最低 4 位，再无符号右移 4 位，最后反转结果。

```java
class Solution {
    public String toHex(int num) {
        if (num == 0) return "0";
        char[] digits = "0123456789abcdef".toCharArray();
        StringBuilder ans = new StringBuilder();
        while (num != 0) {
            ans.append(digits[num & 15]);
            num >>>= 4;
        }
        return ans.reverse().toString();
    }
}
```

复杂度：时间 `O(1)`，空间 `O(1)`。

#### 资深解法：固定最多 8 位并跳过前导零

算法思想：`int` 固定 32 位，最多 8 个十六进制字符。从高位到低位读取每 4 位，遇到第一个非零后开始追加。

```java
class Solution {
    public String toHex(int num) {
        if (num == 0) return "0";
        char[] digits = "0123456789abcdef".toCharArray();
        StringBuilder ans = new StringBuilder();
        boolean started = false;
        for (int shift = 28; shift >= 0; shift -= 4) {
            int value = (num >>> shift) & 15;
            if (value != 0 || started) {
                ans.append(digits[value]);
                started = true;
            }
        }
        return ans.toString();
    }
}
```

复杂度：时间 `O(1)`，空间 `O(1)`。

#### 基础语法与算法思想

- `>>>` 是无符号右移，负数高位会补 0。
- `num & 15` 取得最低 4 个二进制位。
- 核心思想：十六进制转换本质是按 4 位一组读取二进制。

---

## 406. 根据身高重建队列 (Medium)

假设有打乱顺序的一群人站成一个队列，数组  `people`  表示队列中一些人的属性（不一定按顺序）。每个  `people[i] = [hi, ki]`  表示第  `i`  个人的身高为  `hi`  ，前面  **正好**  有  `ki`  个身高大于或等于  `hi`  的人。
请你重新构造并返回输入数组  `people`  所表示的队列。返回的队列应该格式化为数组  `queue`  ，其中  `queue[j] = [hj, kj]`  是队列中第  `j`  个人的属性（ `queue[0]`  是排在队列前面的人）。
 

 **示例 1：** 

```text
输入：people = [[7,0],[4,4],[7,1],[5,0],[6,1],[5,2]]
输出：[[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]]
解释：
编号为 0 的人身高为 5 ，没有身高更高或者相同的人排在他前面。
编号为 1 的人身高为 7 ，没有身高更高或者相同的人排在他前面。
编号为 2 的人身高为 5 ，有 2 个身高更高或者相同的人排在他前面，即编号为 0 和 1 的人。
编号为 3 的人身高为 6 ，有 1 个身高更高或者相同的人排在他前面，即编号为 1 的人。
编号为 4 的人身高为 4 ，有 4 个身高更高或者相同的人排在他前面，即编号为 0、1、2、3 的人。
编号为 5 的人身高为 7 ，有 1 个身高更高或者相同的人排在他前面，即编号为 1 的人。
因此 [[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]] 是重新构造后的队列。
```

 **示例 2：** 

```text
输入：people = [[6,0],[5,0],[4,0],[3,2],[2,2],[1,4]]
输出：[[4,0],[5,0],[2,2],[3,2],[1,4],[6,0]]
```

 
 **提示：** 

 `1 <= people.length <= 2000` 
 `0 <= hi <= 106` 
 `0 <= ki < people.length` 
题目数据确保队列可以被重建

### Java 解法补充

#### 基础解法：逐个找位置

算法思想：先按身高从低到高处理。对当前人，在结果数组中寻找第 `k + 1` 个空位或已放入矮个的位置，因为这些位置不会影响它前面“更高或相等”的数量。

```java
import java.util.Arrays;

class Solution {
    public int[][] reconstructQueue(int[][] people) {
        Arrays.sort(people, (a, b) -> a[0] == b[0] ? b[1] - a[1] : a[0] - b[0]);
        int n = people.length;
        int[][] ans = new int[n][];

        for (int[] person : people) {
            int spaces = person[1] + 1;
            for (int i = 0; i < n; i++) {
                if (ans[i] == null) {
                    spaces--;
                    if (spaces == 0) {
                        ans[i] = person;
                        break;
                    }
                }
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(n)`。

#### 资深解法：高个优先插入

算法思想：先按身高降序、`k` 升序排序。插入某个人时，队列中已有的人都比他高或等高，所以直接把他插入下标 `k`，即可保证前面有 `k` 个高个。

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

class Solution {
    public int[][] reconstructQueue(int[][] people) {
        Arrays.sort(people, (a, b) -> a[0] == b[0] ? a[1] - b[1] : b[0] - a[0]);
        List<int[]> queue = new ArrayList<>();
        for (int[] person : people) {
            queue.add(person[1], person);
        }
        return queue.toArray(new int[people.length][]);
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(n)`。

#### 基础语法与算法思想

- `List.add(index, value)` 会把元素插入指定位置。
- 排序比较器可用三元表达式处理主键和次键。
- 核心思想：先放高个后，矮个不会影响高个的 `k` 条件。

---

## 407. 接雨水 II (Hard)

给你一个  `m x n`  的矩阵，其中的值均为非负整数，代表二维高度图每个单元的高度，请计算图中形状最多能接多少体积的雨水。
 
 **示例 1:** 

```text
输入: heightMap = [[1,4,3,1,3,2],[3,2,1,3,2,4],[2,3,3,2,3,1]]
输出: 4
解释: 下雨后，雨水将会被上图蓝色的方块中。总的接雨水量为1+2+1=4。
```

 **示例 2:** 

```text
输入: heightMap = [[3,3,3,3,3],[3,2,2,2,3],[3,2,1,2,3],[3,2,2,2,3],[3,3,3,3,3]]
输出: 10
```

 
 **提示:** 

 `m == heightMap.length` 
 `n == heightMap[i].length` 
 `1 <= m, n <= 200` 
 `0 <= heightMap[i][j] <= 2 * 104`

### Java 解法补充

#### 基础解法：逐格向四周找最低围墙

算法思想：对每个非边界格子，分别向上下左右找路径上的最高约束，当前格能接的水不超过四个方向最大高度中的最小值。写法直观但复杂度高。

```java
class Solution {
    public int trapRainWater(int[][] heightMap) {
        int m = heightMap.length;
        int n = heightMap[0].length;
        int ans = 0;
        for (int i = 1; i < m - 1; i++) {
            for (int j = 1; j < n - 1; j++) {
                int up = 0, down = 0, left = 0, right = 0;
                for (int r = i; r >= 0; r--) up = Math.max(up, heightMap[r][j]);
                for (int r = i; r < m; r++) down = Math.max(down, heightMap[r][j]);
                for (int c = j; c >= 0; c--) left = Math.max(left, heightMap[i][c]);
                for (int c = j; c < n; c++) right = Math.max(right, heightMap[i][c]);
                int waterLevel = Math.min(Math.min(up, down), Math.min(left, right));
                ans += Math.max(0, waterLevel - heightMap[i][j]);
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(mn(m+n))`，空间 `O(1)`。

#### 资深解法：边界小根堆扩展

算法思想：从所有边界格子开始建小根堆，每次弹出当前最低边界，用它尝试包围相邻格子。若相邻格更低，就能接到当前边界高度的水；入堆高度取两者较大值。

```java
import java.util.PriorityQueue;

class Solution {
    public int trapRainWater(int[][] heightMap) {
        int m = heightMap.length;
        int n = heightMap[0].length;
        if (m <= 2 || n <= 2) return 0;

        boolean[][] seen = new boolean[m][n];
        PriorityQueue<int[]> heap = new PriorityQueue<>((a, b) -> a[2] - b[2]);
        for (int i = 0; i < m; i++) {
            heap.offer(new int[]{i, 0, heightMap[i][0]});
            heap.offer(new int[]{i, n - 1, heightMap[i][n - 1]});
            seen[i][0] = seen[i][n - 1] = true;
        }
        for (int j = 1; j < n - 1; j++) {
            heap.offer(new int[]{0, j, heightMap[0][j]});
            heap.offer(new int[]{m - 1, j, heightMap[m - 1][j]});
            seen[0][j] = seen[m - 1][j] = true;
        }

        int ans = 0;
        int[][] dirs = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
        while (!heap.isEmpty()) {
            int[] cell = heap.poll();
            for (int[] dir : dirs) {
                int x = cell[0] + dir[0];
                int y = cell[1] + dir[1];
                if (x < 0 || x >= m || y < 0 || y >= n || seen[x][y]) continue;
                seen[x][y] = true;
                ans += Math.max(0, cell[2] - heightMap[x][y]);
                heap.offer(new int[]{x, y, Math.max(cell[2], heightMap[x][y])});
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(mn log(mn))`，空间 `O(mn)`。

#### 基础语法与算法思想

- 二维接雨水不能只看单行单列，需要从外边界逐步向内收缩。
- 小根堆每次提供当前最低的有效围墙。
- 核心思想：水位由最短边界决定，优先处理最低边界最安全。

---

## 408. 有效单词缩写 (Easy)

暂无内容描述。

### Java 解法补充

#### 基础解法：双指针解析数字

算法思想：指针 `i` 扫原单词，指针 `j` 扫缩写。遇到字母必须相等；遇到数字就解析完整数字并让 `i` 跳过对应长度。数字不能有前导零。

```java
class Solution {
    public boolean validWordAbbreviation(String word, String abbr) {
        int i = 0;
        int j = 0;
        while (i < word.length() && j < abbr.length()) {
            char c = abbr.charAt(j);
            if (Character.isLetter(c)) {
                if (word.charAt(i) != c) return false;
                i++;
                j++;
            } else {
                if (c == '0') return false;
                int count = 0;
                while (j < abbr.length() && Character.isDigit(abbr.charAt(j))) {
                    count = count * 10 + abbr.charAt(j) - '0';
                    j++;
                }
                i += count;
            }
        }
        return i == word.length() && j == abbr.length();
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 资深解法：边界优先的同思路实现

算法思想：数字段解析时直接检查跳过后是否超过单词长度，遇到不匹配或越界立即返回，便于实装中快速失败。

```java
class Solution {
    public boolean validWordAbbreviation(String word, String abbr) {
        int wordIndex = 0;
        int abbrIndex = 0;
        while (abbrIndex < abbr.length()) {
            char c = abbr.charAt(abbrIndex);
            if (Character.isDigit(c)) {
                if (c == '0') return false;
                int skip = 0;
                while (abbrIndex < abbr.length() && Character.isDigit(abbr.charAt(abbrIndex))) {
                    skip = skip * 10 + abbr.charAt(abbrIndex) - '0';
                    abbrIndex++;
                }
                wordIndex += skip;
                if (wordIndex > word.length()) return false;
            } else {
                if (wordIndex >= word.length() || word.charAt(wordIndex) != c) return false;
                wordIndex++;
                abbrIndex++;
            }
        }
        return wordIndex == word.length();
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- `Character.isDigit` 判断字符是否为数字。
- 缩写数字表示跳过原单词中的若干字符。
- 核心思想：字符串缩写校验要同步维护原串位置和缩写位置。

---

## 409. 最长回文串 (Easy)

给定一个包含大写字母和小写字母的字符串  `s`  ，返回 通过这些字母构造成的  **最长的 回文串**  的长度。
在构造过程中，请注意  **区分大小写**  。比如  `"Aa"`  不能当做一个回文字符串。
 
 **示例 1:** 

```text
输入:s = "abccccdd"
输出:7
解释:
我们可以构造的最长的回文串是"dccaccd", 它的长度是 7。
```

 **示例 2:** 

```text
输入:s = "a"
输出:1
解释：可以构造的最长回文串是"a"，它的长度是 1。
```

 
 **提示:** 

 `1 <= s.length <= 2000` 
 `s`  只由小写  **和/或**  大写英文字母组成

### Java 解法补充

#### 基础解法：哈希表统计频次

算法思想：统计每个字符出现次数。每个字符能贡献 `count / 2 * 2` 个字符；如果存在奇数频次字符，中心还能额外放 1 个。

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int longestPalindrome(String s) {
        Map<Character, Integer> count = new HashMap<>();
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            count.put(c, count.getOrDefault(c, 0) + 1);
        }
        int ans = 0;
        boolean hasOdd = false;
        for (int value : count.values()) {
            ans += value / 2 * 2;
            if (value % 2 == 1) hasOdd = true;
        }
        return hasOdd ? ans + 1 : ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`，字母字符集固定。

#### 资深解法：用集合配对

算法思想：遍历字符，第一次见到放入集合，第二次见到就组成一对并移除。每组成一对答案加 2，最后如果集合非空，说明可放一个中心字符。

```java
import java.util.HashSet;
import java.util.Set;

class Solution {
    public int longestPalindrome(String s) {
        Set<Character> odd = new HashSet<>();
        int ans = 0;
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (odd.remove(c)) {
                ans += 2;
            } else {
                odd.add(c);
            }
        }
        return odd.isEmpty() ? ans : ans + 1;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 回文串两侧字符成对，中间最多放一个单字符。
- `Set.remove` 返回是否成功移除，可用于检测是否已有一个未配对字符。
- 核心思想：最长可构造回文长度等于所有字符偶数贡献加一个可选中心。

---

## 410. 分割数组的最大值 (Hard)

给定一个非负整数数组  `nums`  和一个整数  `k`  ，你需要将这个数组分成  `k`  个非空的连续子数组，使得这  `k`  个子数组各自和的最大值  **最小** 。
返回分割后最小的和的最大值。
 **子数组**  是数组中连续的部分。
 
 **示例 1：** 

```text
输入：nums = [7,2,5,10,8], k = 2
输出：18
解释：
一共有四种方法将 nums 分割为 2 个子数组。 
其中最好的方式是将其分为 [7,2,5] 和 [10,8] 。
因为此时这两个子数组各自的和的最大值为18，在所有情况中最小。
```

 **示例 2：** 

```text
输入：nums = [1,2,3,4,5], k = 2
输出：9
```

 **示例 3：** 

```text
输入：nums = [1,4,4], k = 3
输出：4
```

 
 **提示：** 

 `1 <= nums.length <= 1000` 
 `0 <= nums[i] <= 106` 
 `1 <= k <= min(50, nums.length)`

### Java 解法补充

#### 基础解法：动态规划枚举分割点

算法思想：`dp[i][p]` 表示前 `i` 个数分成 `p` 段时，最大段和的最小可能值。枚举最后一段的起点 `j`，转移为 `max(dp[j][p - 1], sum(j, i))`。

```java
import java.util.Arrays;

class Solution {
    public int splitArray(int[] nums, int k) {
        int n = nums.length;
        long[] prefix = new long[n + 1];
        for (int i = 0; i < n; i++) {
            prefix[i + 1] = prefix[i] + nums[i];
        }

        long[][] dp = new long[n + 1][k + 1];
        for (long[] row : dp) Arrays.fill(row, Long.MAX_VALUE / 2);
        dp[0][0] = 0;
        for (int i = 1; i <= n; i++) {
            for (int p = 1; p <= k; p++) {
                for (int j = 0; j < i; j++) {
                    long last = prefix[i] - prefix[j];
                    dp[i][p] = Math.min(dp[i][p], Math.max(dp[j][p - 1], last));
                }
            }
        }
        return (int) dp[n][k];
    }
}
```

复杂度：时间 `O(n^2 k)`，空间 `O(nk)`。

#### 资深解法：二分答案加贪心校验

算法思想：最大段和越大，越容易完成分割，具有单调性。二分最大段和上限，用贪心统计在该上限下至少需要几段。

```java
class Solution {
    public int splitArray(int[] nums, int k) {
        int left = 0;
        int right = 0;
        for (int num : nums) {
            left = Math.max(left, num);
            right += num;
        }

        while (left < right) {
            int mid = left + (right - left) / 2;
            if (canSplit(nums, k, mid)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }

    private boolean canSplit(int[] nums, int k, int limit) {
        int groups = 1;
        int sum = 0;
        for (int num : nums) {
            if (sum + num > limit) {
                groups++;
                sum = 0;
            }
            sum += num;
        }
        return groups <= k;
    }
}
```

复杂度：时间 `O(n log sum)`，空间 `O(1)`。

#### 基础语法与算法思想

- `left` 至少是数组最大值，`right` 至多是数组总和。
- 连续分割题常能用“二分最大允许代价 + 贪心验证”。
- 核心思想：答案越宽松越可行，这种单调性可以转成二分。

---

## 411. 最短独占单词缩写 (Hard)

暂无内容描述。

### Java 解法补充

#### 基础解法：生成所有缩写再校验

算法思想：回溯生成 `target` 的全部缩写，按长度从短到长排序。逐个检查该缩写是否不会匹配字典中任何同长度单词，第一个合法缩写就是答案。

```java
import java.util.ArrayList;
import java.util.Comparator;
import java.util.List;

class Solution {
    public String minAbbreviation(String target, String[] dictionary) {
        List<String> abbreviations = new ArrayList<>();
        generate(target, 0, 0, new StringBuilder(), abbreviations);
        abbreviations.sort(Comparator.comparingInt(String::length));

        for (String abbr : abbreviations) {
            boolean unique = true;
            for (String word : dictionary) {
                if (word.length() == target.length() && matches(word, abbr)) {
                    unique = false;
                    break;
                }
            }
            if (unique) return abbr;
        }
        return target;
    }

    private void generate(String s, int index, int count, StringBuilder path, List<String> ans) {
        int len = path.length();
        if (index == s.length()) {
            if (count > 0) path.append(count);
            ans.add(path.toString());
            path.setLength(len);
            return;
        }
        generate(s, index + 1, count + 1, path, ans);
        if (count > 0) path.append(count);
        path.append(s.charAt(index));
        generate(s, index + 1, 0, path, ans);
        path.setLength(len);
    }

    private boolean matches(String word, String abbr) {
        int i = 0, j = 0;
        while (i < word.length() && j < abbr.length()) {
            if (Character.isDigit(abbr.charAt(j))) {
                int skip = 0;
                while (j < abbr.length() && Character.isDigit(abbr.charAt(j))) {
                    skip = skip * 10 + abbr.charAt(j++) - '0';
                }
                i += skip;
            } else if (word.charAt(i++) != abbr.charAt(j++)) {
                return false;
            }
        }
        return i == word.length() && j == abbr.length();
    }
}
```

复杂度：时间 `O(2^n * dictionary.length * n)`，空间 `O(2^n)`。

#### 资深解法：位掩码枚举保留字符

算法思想：用二进制掩码表示缩写中哪些位置保留字符。一个缩写能区分某个字典单词，当且仅当它保留了至少一个与该单词不同的位置。枚举所有掩码，找长度最短且能区分所有单词的方案。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public String minAbbreviation(String target, String[] dictionary) {
        int n = target.length();
        List<Integer> diffs = new ArrayList<>();
        for (String word : dictionary) {
            if (word.length() != n) continue;
            int diff = 0;
            for (int i = 0; i < n; i++) {
                if (target.charAt(i) != word.charAt(i)) {
                    diff |= 1 << i;
                }
            }
            diffs.add(diff);
        }
        if (diffs.isEmpty()) return String.valueOf(n);

        int bestMask = 0;
        int bestLen = Integer.MAX_VALUE;
        for (int mask = 0; mask < (1 << n); mask++) {
            int len = abbreviationLength(mask, n);
            if (len >= bestLen) continue;
            boolean valid = true;
            for (int diff : diffs) {
                if ((mask & diff) == 0) {
                    valid = false;
                    break;
                }
            }
            if (valid) {
                bestLen = len;
                bestMask = mask;
            }
        }
        return build(target, bestMask);
    }

    private int abbreviationLength(int mask, int n) {
        int len = 0, skipped = 0;
        for (int i = 0; i < n; i++) {
            if ((mask & (1 << i)) == 0) {
                skipped++;
            } else {
                if (skipped > 0) len++;
                skipped = 0;
                len++;
            }
        }
        return skipped > 0 ? len + 1 : len;
    }

    private String build(String target, int mask) {
        StringBuilder ans = new StringBuilder();
        int skipped = 0;
        for (int i = 0; i < target.length(); i++) {
            if ((mask & (1 << i)) == 0) {
                skipped++;
            } else {
                if (skipped > 0) ans.append(skipped);
                skipped = 0;
                ans.append(target.charAt(i));
            }
        }
        if (skipped > 0) ans.append(skipped);
        return ans.toString();
    }
}
```

复杂度：时间 `O(2^n * dictionary.length)`，空间 `O(dictionary.length)`。

#### 基础语法与算法思想

- 缩写中数字代表连续省略的字符数。
- 位掩码可压缩“保留/省略”这种二选一状态。
- 核心思想：独占缩写必须在每个同长度字典词上至少保留一个不同字符。

---

## 412. Fizz Buzz (Easy)

给你一个整数  `n`  ，返回一个字符串数组  `answer` （ **下标从 1 开始** ），其中：

 `answer[i] == "FizzBuzz"`  如果  `i`  同时是  `3`  和  `5`  的倍数。
 `answer[i] == "Fizz"`  如果  `i`  是  `3`  的倍数。
 `answer[i] == "Buzz"`  如果  `i`  是  `5`  的倍数。
 `answer[i] == i`  （以字符串形式）如果上述条件全不满足。

 
 **示例 1：** 

```text
输入：n = 3
输出：["1","2","Fizz"]
```

 **示例 2：** 

```text
输入：n = 5
输出：["1","2","Fizz","4","Buzz"]
```

 **示例 3：** 

```text
输入：n = 15
输出：["1","2","Fizz","4","Buzz","Fizz","7","8","Fizz","Buzz","11","Fizz","13","14","FizzBuzz"]
```

 
 **提示：** 

 `1 <= n <= 104`

### Java 解法补充

#### 基础解法：按条件分支

算法思想：从 1 到 `n` 枚举数字，优先判断是否同时被 3 和 5 整除，再判断单独被 3 或 5 整除。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<String> fizzBuzz(int n) {
        List<String> ans = new ArrayList<>();
        for (int i = 1; i <= n; i++) {
            if (i % 15 == 0) ans.add("FizzBuzz");
            else if (i % 3 == 0) ans.add("Fizz");
            else if (i % 5 == 0) ans.add("Buzz");
            else ans.add(String.valueOf(i));
        }
        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`，不计返回结果。

#### 资深解法：字符串组合规则

算法思想：把能被 3 整除和能被 5 整除看成两条规则，分别追加 `"Fizz"` 和 `"Buzz"`。如果没有追加任何内容，再放入数字本身。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<String> fizzBuzz(int n) {
        List<String> ans = new ArrayList<>();
        for (int i = 1; i <= n; i++) {
            StringBuilder item = new StringBuilder();
            if (i % 3 == 0) item.append("Fizz");
            if (i % 5 == 0) item.append("Buzz");
            ans.add(item.length() == 0 ? String.valueOf(i) : item.toString());
        }
        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`，不计返回结果。

#### 基础语法与算法思想

- `%` 用于判断整除。
- `String.valueOf(i)` 把整数转成字符串。
- 核心思想：条件有优先级时先写清楚基础分支；规则变多时用组合式写法更容易扩展。

---

## 413. 等差数列划分 (Medium)

如果一个数列  **至少有三个元素**  ，并且任意两个相邻元素之差相同，则称该数列为等差数列。

例如， `[1,3,5,7,9]` 、 `[7,7,7,7]`  和  `[3,-1,-5,-9]`  都是等差数列。

给你一个整数数组  `nums`  ，返回数组  `nums`  中所有为等差数组的  **子数组**  个数。
 **子数组**  是数组中的一个连续序列。
 
 **示例 1：** 

```text
输入：nums = [1,2,3,4]
输出：3
解释：nums 中有三个子等差数组：[1, 2, 3]、[2, 3, 4] 和 [1,2,3,4] 自身。
```

 **示例 2：** 

```text
输入：nums = [1]
输出：0
```

 
 **提示：** 

 `1 <= nums.length <= 5000` 
 `-1000 <= nums[i] <= 1000`

### Java 解法补充

#### 基础解法：枚举子数组并检查

算法思想：枚举所有长度至少为 3 的子数组，检查相邻差是否全部相等，是等差数组就计数。

```java
class Solution {
    public int numberOfArithmeticSlices(int[] nums) {
        int ans = 0;
        for (int left = 0; left < nums.length; left++) {
            for (int right = left + 2; right < nums.length; right++) {
                int diff = nums[left + 1] - nums[left];
                boolean ok = true;
                for (int i = left + 2; i <= right; i++) {
                    if (nums[i] - nums[i - 1] != diff) {
                        ok = false;
                        break;
                    }
                }
                if (ok) ans++;
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(n^3)`，空间 `O(1)`。

#### 资深解法：连续等差段累加

算法思想：如果以 `i` 结尾的三个数成等差，那么以 `i - 1` 结尾的等差子数组都可以延长，并新增一个长度为 3 的子数组。

```java
class Solution {
    public int numberOfArithmeticSlices(int[] nums) {
        int current = 0;
        int ans = 0;
        for (int i = 2; i < nums.length; i++) {
            if (nums[i] - nums[i - 1] == nums[i - 1] - nums[i - 2]) {
                current++;
                ans += current;
            } else {
                current = 0;
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 子数组必须连续，和子序列不同。
- `current` 表示当前连续等差段能新增多少个以当前位置结尾的子数组。
- 核心思想：连续段越长，新增等差子数组数量递增。

---

## 414. 第三大的数 (Easy)

给你一个非空数组，返回此数组中  **第三大的数**  。如果不存在，则返回数组中最大的数。
 
 **示例 1：** 

```text
输入：[3, 2, 1]
输出：1
解释：第三大的数是 1 。
```

 **示例 2：** 

```text
输入：[1, 2]
输出：2
解释：第三大的数不存在, 所以返回最大的数 2 。
```

 **示例 3：** 

```text
输入：[2, 2, 3, 1]
输出：1
解释：注意，要求返回第三大的数，是指在所有不同数字中排第三大的数。
此例中存在两个值为 2 的数，它们都排第二。在所有不同数字中排第三大的数为 1 。
```

 
 **提示：** 

 `1 <= nums.length <= 104` 
 `-231 <= nums[i] <= 231 - 1` 

 
 **进阶：** 你能设计一个时间复杂度  `O(n)`  的解决方案吗？

### Java 解法补充

#### 基础解法：去重后排序

算法思想：把数组放入集合去重，再排序。若不同数字少于 3 个，返回最大值，否则返回第三大值。

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.HashSet;
import java.util.List;
import java.util.Set;

class Solution {
    public int thirdMax(int[] nums) {
        Set<Integer> set = new HashSet<>();
        for (int num : nums) set.add(num);
        List<Integer> list = new ArrayList<>(set);
        Collections.sort(list);
        if (list.size() < 3) {
            return list.get(list.size() - 1);
        }
        return list.get(list.size() - 3);
    }
}
```

复杂度：时间 `O(n log n)`，空间 `O(n)`。

#### 资深解法：三个变量一次扫描

算法思想：用 `Long` 保存第一、第二、第三大的不同数字，遇到重复值跳过，按大小关系更新三个变量。

```java
class Solution {
    public int thirdMax(int[] nums) {
        Long first = null, second = null, third = null;
        for (int num : nums) {
            long value = num;
            if ((first != null && value == first)
                    || (second != null && value == second)
                    || (third != null && value == third)) {
                continue;
            }
            if (first == null || value > first) {
                third = second;
                second = first;
                first = value;
            } else if (second == null || value > second) {
                third = second;
                second = value;
            } else if (third == null || value > third) {
                third = value;
            }
        }
        return third == null ? first.intValue() : third.intValue();
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 用包装类型 `Long` 可以用 `null` 表示还没有值。
- 第三大要求不同数字，重复元素要跳过。
- 核心思想：固定排名前几的最大值，可以用少量变量在线维护。

---

## 415. 字符串相加 (Easy)

给定两个字符串形式的非负整数  `num1`  和 `num2`  ，计算它们的和并同样以字符串形式返回。
你不能使用任何內建的用于处理大整数的库（比如  `BigInteger` ）， 也不能直接将输入的字符串转换为整数形式。
 
 **示例 1：** 

```text
输入：num1 = "11", num2 = "123"
输出："134"
```

 **示例 2：** 

```text
输入：num1 = "456", num2 = "77"
输出："533"
```

 **示例 3：** 

```text
输入：num1 = "0", num2 = "0"
输出："0"
```

 
 
 **提示：** 

 `1 <= num1.length, num2.length <= 104` 
 `num1`  和 `num2`  都只包含数字  `0-9` 
 `num1`  和 `num2`  都不包含任何前导零

### Java 解法补充

#### 基础解法：从末尾逐位相加

算法思想：模拟小学加法，从两个字符串末尾开始取数字，加上进位，当前位放 `sum % 10`，进位为 `sum / 10`。

```java
class Solution {
    public String addStrings(String num1, String num2) {
        int i = num1.length() - 1;
        int j = num2.length() - 1;
        int carry = 0;
        StringBuilder ans = new StringBuilder();

        while (i >= 0 || j >= 0 || carry != 0) {
            int x = i >= 0 ? num1.charAt(i--) - '0' : 0;
            int y = j >= 0 ? num2.charAt(j--) - '0' : 0;
            int sum = x + y + carry;
            ans.append(sum % 10);
            carry = sum / 10;
        }
        return ans.reverse().toString();
    }
}
```

复杂度：时间 `O(max(m,n))`，空间 `O(max(m,n))`。

#### 资深解法：封装下标读取

算法思想：主循环只负责移动指针和进位，读取数字封装成小函数，降低边界判断噪音。

```java
class Solution {
    public String addStrings(String num1, String num2) {
        int i = num1.length() - 1;
        int j = num2.length() - 1;
        int carry = 0;
        StringBuilder ans = new StringBuilder();
        while (i >= 0 || j >= 0 || carry > 0) {
            int sum = digitAt(num1, i) + digitAt(num2, j) + carry;
            ans.append((char) ('0' + sum % 10));
            carry = sum / 10;
            i--;
            j--;
        }
        return ans.reverse().toString();
    }

    private int digitAt(String s, int index) {
        return index >= 0 ? s.charAt(index) - '0' : 0;
    }
}
```

复杂度：时间 `O(max(m,n))`，空间 `O(max(m,n))`。

#### 基础语法与算法思想

- 字符数字转整数用 `c - '0'`。
- 从低位到高位处理时，最后需要反转结果。
- 核心思想：大数字字符串运算不能转整数，要按位模拟。

---

## 416. 分割等和子集 (Medium)

给你一个  **只包含正整数** 的  **非空** 数组  `nums`  。请你判断是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。
 
 **示例 1：** 

```text
输入：nums = [1,5,11,5]
输出：true
解释：数组可以分割成 [1, 5, 5] 和 [11] 。
```

 **示例 2：** 

```text
输入：nums = [1,2,3,5]
输出：false
解释：数组不能分割成两个元素和相等的子集。
```

 
 **提示：** 

 `1 <= nums.length <= 200` 
 `1 <= nums[i] <= 100`

### Java 解法补充

#### 基础解法：回溯搜索目标和

算法思想：数组总和若为奇数一定不能平分。否则目标是找一个子集和为 `sum / 2`，递归尝试选或不选每个数字。

```java
class Solution {
    public boolean canPartition(int[] nums) {
        int sum = 0;
        for (int num : nums) sum += num;
        if (sum % 2 == 1) return false;
        return dfs(nums, 0, sum / 2);
    }

    private boolean dfs(int[] nums, int index, int remain) {
        if (remain == 0) return true;
        if (index == nums.length || remain < 0) return false;
        return dfs(nums, index + 1, remain - nums[index])
                || dfs(nums, index + 1, remain);
    }
}
```

复杂度：时间 `O(2^n)`，空间 `O(n)`。

#### 资深解法：0/1 背包 DP

算法思想：`dp[j]` 表示是否能用已处理数字凑出和 `j`。每个数字只能用一次，所以目标和从大到小更新。

```java
class Solution {
    public boolean canPartition(int[] nums) {
        int sum = 0;
        for (int num : nums) sum += num;
        if (sum % 2 == 1) return false;
        int target = sum / 2;
        boolean[] dp = new boolean[target + 1];
        dp[0] = true;
        for (int num : nums) {
            for (int j = target; j >= num; j--) {
                dp[j] = dp[j] || dp[j - num];
            }
        }
        return dp[target];
    }
}
```

复杂度：时间 `O(n * target)`，空间 `O(target)`。

#### 基础语法与算法思想

- 总和为奇数无法分成两个相等整数和。
- 0/1 背包一维优化时，容量要倒序枚举。
- 核心思想：平分数组等价于找一个子集凑出总和的一半。

---

## 417. 太平洋大西洋水流问题 (Medium)

有一个  `m × n`  的矩形岛屿，与  **太平洋**  和  **大西洋**  相邻。  **“太平洋”** 处于大陆的左边界和上边界，而  **“大西洋”**  处于大陆的右边界和下边界。
这个岛被分割成一个由若干方形单元格组成的网格。给定一个  `m x n`  的整数矩阵  `heights`  ，  `heights[r][c]`  表示坐标  `(r, c)`  上单元格  **高于海平面的高度**  。
岛上雨水较多，如果相邻单元格的高度  **小于或等于**  当前单元格的高度，雨水可以直接向北、南、东、西流向相邻单元格。水可以从海洋附近的任何单元格流入海洋。
返回网格坐标  `result`  的  **2D 列表**  ，其中  `result[i] = [ri, ci]`  表示雨水从单元格  `(ri, ci)`  流动  **既可流向太平洋也可流向大西洋**  。
 
 **示例 1：** 

```text
输入: heights = [[1,2,2,3,5],[3,2,3,4,4],[2,4,5,3,1],[6,7,1,4,5],[5,1,1,2,4]]
输出: [[0,4],[1,3],[1,4],[2,2],[3,0],[3,1],[4,0]]
```

 **示例 2：** 

```text
输入: heights = [[2,1],[1,2]]
输出: [[0,0],[0,1],[1,0],[1,1]]
```

 
 **提示：** 

 `m == heights.length` 
 `n == heights[r].length` 
 `1 <= m, n <= 200` 
 `0 <= heights[r][c] <= 105`

### Java 解法补充

#### 基础解法：每个格子 DFS 判断两海

算法思想：对每个格子分别 DFS，看水是否能沿着不升高的方向到达太平洋和大西洋。这个写法直观，但会重复搜索。

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

class Solution {
    private int[][] heights;
    private int m, n;
    private final int[][] dirs = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};

    public List<List<Integer>> pacificAtlantic(int[][] heights) {
        this.heights = heights;
        m = heights.length;
        n = heights[0].length;
        List<List<Integer>> ans = new ArrayList<>();
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (canReach(i, j, true, new boolean[m][n])
                        && canReach(i, j, false, new boolean[m][n])) {
                    ans.add(Arrays.asList(i, j));
                }
            }
        }
        return ans;
    }

    private boolean canReach(int x, int y, boolean pacific, boolean[][] seen) {
        if (pacific && (x == 0 || y == 0)) return true;
        if (!pacific && (x == m - 1 || y == n - 1)) return true;
        seen[x][y] = true;
        for (int[] dir : dirs) {
            int nx = x + dir[0], ny = y + dir[1];
            if (nx < 0 || nx >= m || ny < 0 || ny >= n || seen[nx][ny]) continue;
            if (heights[nx][ny] <= heights[x][y] && canReach(nx, ny, pacific, seen)) {
                return true;
            }
        }
        return false;
    }
}
```

复杂度：最坏时间 `O(m^2n^2)`，空间 `O(mn)`。

#### 资深解法：从海洋反向 DFS

算法思想：水从高处流向低处。反过来，从海洋边界出发，只能走到高度大于等于当前格子的邻居。分别标记能到太平洋和大西洋的格子，最后取交集。

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

class Solution {
    private int[][] heights;
    private int m, n;
    private final int[][] dirs = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};

    public List<List<Integer>> pacificAtlantic(int[][] heights) {
        this.heights = heights;
        m = heights.length;
        n = heights[0].length;
        boolean[][] pacific = new boolean[m][n];
        boolean[][] atlantic = new boolean[m][n];

        for (int i = 0; i < m; i++) {
            dfs(i, 0, pacific);
            dfs(i, n - 1, atlantic);
        }
        for (int j = 0; j < n; j++) {
            dfs(0, j, pacific);
            dfs(m - 1, j, atlantic);
        }

        List<List<Integer>> ans = new ArrayList<>();
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (pacific[i][j] && atlantic[i][j]) {
                    ans.add(Arrays.asList(i, j));
                }
            }
        }
        return ans;
    }

    private void dfs(int x, int y, boolean[][] ocean) {
        if (ocean[x][y]) return;
        ocean[x][y] = true;
        for (int[] dir : dirs) {
            int nx = x + dir[0], ny = y + dir[1];
            if (nx < 0 || nx >= m || ny < 0 || ny >= n || ocean[nx][ny]) continue;
            if (heights[nx][ny] >= heights[x][y]) {
                dfs(nx, ny, ocean);
            }
        }
    }
}
```

复杂度：时间 `O(mn)`，空间 `O(mn)`。

#### 基础语法与算法思想

- 从每个格子正向搜索会重复；从边界反向搜索能一次标记一片区域。
- `Arrays.asList(i, j)` 可创建坐标列表。
- 核心思想：流动方向可反转，条件从“往低处流”变成“往高处爬”。

---

## 418. 屏幕可显示句子的数量 (Medium)

暂无内容描述。

### Java 解法补充

#### 基础解法：逐行放单词

算法思想：按行模拟屏幕。每行从当前单词开始，能放下就放并消耗列宽；一轮句子放完时计数加一。

```java
class Solution {
    public int wordsTyping(String[] sentence, int rows, int cols) {
        int index = 0;
        int count = 0;
        for (int r = 0; r < rows; r++) {
            int used = 0;
            while (used + sentence[index].length() <= cols) {
                used += sentence[index].length() + 1;
                index++;
                if (index == sentence.length) {
                    index = 0;
                    count++;
                }
            }
        }
        return count;
    }
}
```

复杂度：时间与实际放入单词数相关，空间 `O(1)`。

#### 资深解法：拼接句子后用指针回退

算法思想：把句子拼成 `"word1 word2 ... "`。每行先假设放 `cols` 个字符，如果落在空格上就前进一步；如果落在单词中间，就回退到上一个空格后。总走过字符数除以句子串长度就是显示次数。

```java
class Solution {
    public int wordsTyping(String[] sentence, int rows, int cols) {
        String text = String.join(" ", sentence) + " ";
        int pos = 0;
        for (int r = 0; r < rows; r++) {
            pos += cols;
            if (text.charAt(pos % text.length()) == ' ') {
                pos++;
            } else {
                while (pos > 0 && text.charAt((pos - 1) % text.length()) != ' ') {
                    pos--;
                }
            }
        }
        return pos / text.length();
    }
}
```

复杂度：时间 `O(rows * maxWordLength)`，空间 `O(sentenceLength)`。

#### 基础语法与算法思想

- 每个单词之间至少需要一个空格。
- `String.join` 可以用分隔符拼接字符串数组。
- 核心思想：整句循环显示时，可以把屏幕位置映射到拼接后的循环字符串。

---

## 419. 棋盘上的战舰 (Medium)

给你一个大小为  `m x n`  的矩阵  `board`  表示棋盘，其中，每个单元格可以是一艘战舰  `'X'`  或者是一个空位  `'.'`  ，返回在棋盘  `board`  上放置的  **舰队**  的数量。
 **舰队**  只能水平或者垂直放置在  `board`  上。换句话说，舰队只能按  `1 x k` （ `1`  行， `k`  列）或  `k x 1` （ `k`  行， `1`  列）的形状放置，其中  `k`  可以是任意大小。两个舰队之间至少有一个水平或垂直的空格分隔 （即没有相邻的舰队）。
 
 **示例 1：** 

```text
输入：board = [["X",".",".","X"],[".",".",".","X"],[".",".",".","X"]]
输出：2
```

 **示例 2：** 

```text
输入：board = [["."]]
输出：0
```

 
 **提示：** 

 `m == board.length` 
 `n == board[i].length` 
 `1 <= m, n <= 200` 
 `board[i][j]`  是  `'.'`  或  `'X'` 

 
 **进阶：** 你可以实现一次扫描算法，并只使用 ****  `O(1)`  **** 额外空间，并且不修改  `board`  的值来解决这个问题吗？

### Java 解法补充

#### 基础解法：DFS 标记整艘战舰

算法思想：遇到未访问的 `'X'`，说明发现一艘新战舰，答案加一，并 DFS 把这艘战舰的所有连续 `'X'` 标记为已访问。

```java
class Solution {
    public int countBattleships(char[][] board) {
        int m = board.length;
        int n = board[0].length;
        boolean[][] seen = new boolean[m][n];
        int ans = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] == 'X' && !seen[i][j]) {
                    ans++;
                    dfs(board, seen, i, j);
                }
            }
        }
        return ans;
    }

    private void dfs(char[][] board, boolean[][] seen, int x, int y) {
        if (x < 0 || x >= board.length || y < 0 || y >= board[0].length) return;
        if (seen[x][y] || board[x][y] != 'X') return;
        seen[x][y] = true;
        dfs(board, seen, x + 1, y);
        dfs(board, seen, x - 1, y);
        dfs(board, seen, x, y + 1);
        dfs(board, seen, x, y - 1);
    }
}
```

复杂度：时间 `O(mn)`，空间 `O(mn)`。

#### 资深解法：只数战舰头部

算法思想：一艘战舰的头部是一个 `'X'`，且上方和左方都不是 `'X'`。只统计头部，就能在一次扫描中得到战舰数，不需要额外访问数组。

```java
class Solution {
    public int countBattleships(char[][] board) {
        int ans = 0;
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                if (board[i][j] != 'X') continue;
                if (i > 0 && board[i - 1][j] == 'X') continue;
                if (j > 0 && board[i][j - 1] == 'X') continue;
                ans++;
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(mn)`，空间 `O(1)`。

#### 基础语法与算法思想

- 题目保证战舰之间不会相邻，避免复杂连通块形状。
- 只数每艘战舰最左上方的起点，可以避免重复计数。
- 核心思想：线段形状的连通块，通常可以通过判断前驱格来计数。

---

## 420. 强密码检验器 (Hard)

满足以下条件的密码被认为是强密码：

由至少  `6`  个，至多  `20`  个字符组成。
包含至少  **一个小写** 字母，至少  **一个大写**  字母，和至少  **一个数字**  。
不包含连续三个重复字符 (比如  `"Baaabb0"`  是弱密码, 但是  `"Baaba0"`  是强密码)。

给你一个字符串  `password`  ，返回 将  `password`  修改到满足强密码条件需要的最少修改步数。如果  `password`  已经是强密码，则返回  `0`  。
在一步修改操作中，你可以：

插入一个字符到  `password`  ，
从  `password`  中删除一个字符，或
用另一个字符来替换  `password`  中的某个字符。

 
 **示例 1：** 

```text
输入：password = "a"
输出：5
```

 **示例 2：** 

```text
输入：password = "aA1"
输出：3
```

 **示例 3：** 

```text
输入：password = "1337C0d3"
输出：0
```

 
 **提示：** 

 `1 <= password.length <= 50` 
 `password`  由字母、数字、点  `'.'`  或者感叹号  `'!'`  组成

### Java 解法补充

#### 基础解法：按长度和重复段分情况

算法思想：先统计缺少的小写、大写、数字类型数，再统计所有连续重复段需要的替换次数。长度不足时用插入补长度；长度合规时取类型缺失和重复替换的较大值；长度超出时优先删除重复段中的字符来减少替换次数。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public int strongPasswordChecker(String password) {
        int n = password.length();
        int missing = missingTypes(password);
        List<Integer> groups = repeatGroups(password);

        if (n < 6) {
            return Math.max(missing, 6 - n);
        }

        int replace = 0;
        for (int len : groups) {
            replace += len / 3;
        }
        if (n <= 20) {
            return Math.max(missing, replace);
        }

        int delete = n - 20;
        int remainDelete = delete;
        for (int i = 0; i < groups.size() && remainDelete > 0; i++) {
            if (groups.get(i) >= 3 && groups.get(i) % 3 == 0) {
                groups.set(i, groups.get(i) - 1);
                remainDelete--;
            }
        }
        for (int i = 0; i < groups.size() && remainDelete > 1; i++) {
            if (groups.get(i) >= 3 && groups.get(i) % 3 == 1) {
                groups.set(i, groups.get(i) - 2);
                remainDelete -= 2;
            }
        }
        for (int i = 0; i < groups.size() && remainDelete > 2; i++) {
            while (groups.get(i) >= 3 && remainDelete > 2) {
                groups.set(i, groups.get(i) - 3);
                remainDelete -= 3;
            }
        }

        replace = 0;
        for (int len : groups) {
            replace += len / 3;
        }
        return delete + Math.max(missing, replace);
    }

    private int missingTypes(String s) {
        boolean lower = false, upper = false, digit = false;
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            lower |= Character.isLowerCase(c);
            upper |= Character.isUpperCase(c);
            digit |= Character.isDigit(c);
        }
        int missing = 0;
        if (!lower) missing++;
        if (!upper) missing++;
        if (!digit) missing++;
        return missing;
    }

    private List<Integer> repeatGroups(String s) {
        List<Integer> groups = new ArrayList<>();
        for (int i = 0; i < s.length();) {
            int j = i;
            while (j < s.length() && s.charAt(j) == s.charAt(i)) j++;
            if (j - i >= 3) groups.add(j - i);
            i = j;
        }
        return groups;
    }
}
```

复杂度：时间 `O(n + g * delete)`，空间 `O(g)`，`g` 为重复段数量。

#### 资深解法：贪心删除优化替换次数

算法思想：长度超过 20 时，删除是必需操作。对长度为 `len` 的重复段，替换次数是 `len / 3`；当 `len % 3` 分别为 0、1、2 时，减少一次替换所需删除数分别为 1、2、3，所以按这个优先级消耗删除次数。

```java
class Solution {
    public int strongPasswordChecker(String password) {
        int n = password.length();
        boolean lower = false, upper = false, digit = false;
        for (int i = 0; i < n; i++) {
            char c = password.charAt(i);
            lower |= Character.isLowerCase(c);
            upper |= Character.isUpperCase(c);
            digit |= Character.isDigit(c);
        }
        int missing = (lower ? 0 : 1) + (upper ? 0 : 1) + (digit ? 0 : 1);

        int replace = 0;
        int oneDelete = 0;
        int twoDelete = 0;
        for (int i = 0; i < n;) {
            int j = i;
            while (j < n && password.charAt(j) == password.charAt(i)) j++;
            int len = j - i;
            if (len >= 3) {
                replace += len / 3;
                if (len % 3 == 0) oneDelete++;
                else if (len % 3 == 1) twoDelete++;
            }
            i = j;
        }

        if (n < 6) {
            return Math.max(missing, 6 - n);
        }
        if (n <= 20) {
            return Math.max(missing, replace);
        }

        int delete = n - 20;
        int use = Math.min(delete, oneDelete);
        replace -= use;

        use = Math.min(Math.max(delete - oneDelete, 0), twoDelete * 2);
        replace -= use / 2;

        use = Math.max(delete - oneDelete - twoDelete * 2, 0);
        replace -= use / 3;

        return delete + Math.max(missing, replace);
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- `Character.isLowerCase`、`isUpperCase`、`isDigit` 可检查字符类型。
- 三个连续相同字符至少需要一次替换或通过插入、删除打断。
- 核心思想：短密码优先插入，合规长度优先替换，超长密码必须先删除且删除要优先作用于重复段。

---

