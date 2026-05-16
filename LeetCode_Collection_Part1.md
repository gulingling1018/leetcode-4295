## 1. 两数之和 (Easy)

给定一个整数数组  `nums`  和一个整数目标值  `target` ，请你在该数组中找出 和为目标值  `target`   的那 两个 整数，并返回它们的数组下标。
你可以假设每种输入只会对应一个答案，并且你不能使用两次相同的元素。
你可以按任意顺序返回答案。
 
示例 1：

```text
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

示例 2：

```text
输入：nums = [3,2,4], target = 6
输出：[1,2]
```

示例 3：

```text
输入：nums = [3,3], target = 6
输出：[0,1]
```

 
提示：

 `2 <= nums.length <= 104` 
 `-109 <= nums[i] <= 109` 
 `-109 <= target <= 109` 
只会存在一个有效答案

 
进阶：你可以想出一个时间复杂度小于  `O(n2)`  的算法吗？

### Java 解法补充

#### 基础解法：双重循环

算法思想：枚举第一个数 `i`，再枚举它后面的数 `j`，只要 `nums[i] + nums[j] == target` 就返回两个下标。这个写法最直观，适合理解题意和数组下标。

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[i] + nums[j] == target) {
                    return new int[]{i, j};
                }
            }
        }
        return new int[]{-1, -1};
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(1)`。

#### 资深解法：哈希表一次扫描

算法思想：扫描到 `nums[i]` 时，目标是找到之前是否出现过 `target - nums[i]`。用哈希表保存“值 -> 下标”，把查找补数降到均摊 `O(1)`。

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> index = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int need = target - nums[i];
            if (index.containsKey(need)) {
                return new int[]{index.get(need), i};
            }
            index.put(nums[i], i);
        }
        return new int[]{-1, -1};
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 基础语法与算法思想

- `nums.length`：数组长度。
- `new int[]{i, j}`：创建并返回一个匿名整型数组。
- `Map<Integer, Integer>`：泛型哈希表，键和值都是整数包装类型。
- 核心思想：用空间换时间，把“找另一个数”从遍历变成哈希查询。

---

## 2. 两数相加 (Medium)

给你两个 非空 的链表，表示两个非负的整数。它们每位数字都是按照 逆序 的方式存储的，并且每个节点只能存储 一位 数字。
请你将两个数相加，并以相同形式返回一个表示和的链表。
你可以假设除了数字 0 之外，这两个数都不会以 0 开头。
 
示例 1：

```text
输入：l1 = [2,4,3], l2 = [5,6,4]
输出：[7,0,8]
解释：342 + 465 = 807.
```

示例 2：

```text
输入：l1 = [0], l2 = [0]
输出：[0]
```

示例 3：

```text
输入：l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
输出：[8,9,9,9,0,0,0,1]
```

 
提示：

每个链表中的节点数在范围  `[1, 100]`  内
 `0 <= Node.val <= 9` 
题目数据保证列表表示的数字不含前导零

### Java 解法补充

#### 基础解法：逐位相加

算法思想：链表已经按逆序存储，正好从个位开始相加。每次读取两个节点的值和上一位进位 `carry`，新节点保存 `sum % 10`，新的进位是 `sum / 10`。

```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0);
        ListNode cur = dummy;
        int carry = 0;

        while (l1 != null || l2 != null) {
            int x = l1 == null ? 0 : l1.val;
            int y = l2 == null ? 0 : l2.val;
            int sum = x + y + carry;

            carry = sum / 10;
            cur.next = new ListNode(sum % 10);
            cur = cur.next;

            if (l1 != null) l1 = l1.next;
            if (l2 != null) l2 = l2.next;
        }

        if (carry > 0) {
            cur.next = new ListNode(carry);
        }
        return dummy.next;
    }
}
```

复杂度：时间 `O(max(m, n))`，空间 `O(max(m, n))`，其中空间为返回链表。

#### 资深解法：把进位并入循环条件

算法思想：把 `carry != 0` 也放进循环条件，最后一位进位不用单独写分支。用小工具函数读取可空节点的值，主流程更集中。

```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0);
        ListNode tail = dummy;
        int carry = 0;

        while (l1 != null || l2 != null || carry != 0) {
            int sum = valueOf(l1) + valueOf(l2) + carry;
            carry = sum / 10;

            tail.next = new ListNode(sum % 10);
            tail = tail.next;

            l1 = l1 == null ? null : l1.next;
            l2 = l2 == null ? null : l2.next;
        }

        return dummy.next;
    }

    private int valueOf(ListNode node) {
        return node == null ? 0 : node.val;
    }
}
```

复杂度：时间 `O(max(m, n))`，空间 `O(max(m, n))`。

#### 基础语法与算法思想

- `ListNode dummy = new ListNode(0)`：虚拟头结点，避免单独处理结果链表的第一个节点。
- `cur.next = new ListNode(...)`：创建新节点并接到链表尾部。
- 三元表达式 `条件 ? 值1 : 值2`：常用于空节点给默认值。
- 核心思想：模拟小学竖式加法，链表节点移动就是数字位数向高位推进。

---

## 3. 无重复字符的最长子串 (Medium)

给定一个字符串  `s`  ，请你找出其中不含有重复字符的 最长 子串 的长度。
 
示例 1:

```text
输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。注意 "bca" 和 "cab" 也是正确答案。
```

示例 2:

```text
输入: s = "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

示例 3:

```text
输入: s = "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

 
提示：

 `0 <= s.length <= 5 * 104` 
 `s`  由英文字母、数字、符号和空格组成

### Java 解法补充

#### 基础解法：枚举起点并向右扩展

算法思想：以每个位置作为子串起点，向右扩展并用布尔数组记录字符是否出现过。遇到重复字符就停止当前起点的扩展。

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int ans = 0;
        for (int left = 0; left < s.length(); left++) {
            boolean[] seen = new boolean[128];
            for (int right = left; right < s.length(); right++) {
                char c = s.charAt(right);
                if (seen[c]) {
                    break;
                }
                seen[c] = true;
                ans = Math.max(ans, right - left + 1);
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(1)`，字符集大小视为常数。

#### 资深解法：滑动窗口

算法思想：维护一个无重复窗口 `[left, right]`。如果 `s[right]` 上次出现位置在窗口内，就把 `left` 跳到上次位置之后。

```java
import java.util.Arrays;

class Solution {
    public int lengthOfLongestSubstring(String s) {
        int[] last = new int[128];
        Arrays.fill(last, -1);

        int ans = 0;
        int left = 0;
        for (int right = 0; right < s.length(); right++) {
            char c = s.charAt(right);
            if (last[c] >= left) {
                left = last[c] + 1;
            }
            last[c] = right;
            ans = Math.max(ans, right - left + 1);
        }
        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- `s.charAt(i)`：读取字符串第 `i` 个字符。
- `Arrays.fill(last, -1)`：把数组全部填为指定值。
- `Math.max(a, b)`：取较大值。
- 核心思想：滑动窗口适合“连续子串/子数组 + 约束条件”的题；右边界扩张，左边界只向右移动。

---

## 4. 寻找两个正序数组的中位数 (Hard)

给定两个大小分别为  `m`  和  `n`  的正序（从小到大）数组  `nums1`  和  `nums2` 。请你找出并返回这两个正序数组的 中位数 。
算法的时间复杂度应该为  `O(log (m+n))`  。
 
示例 1：

```text
输入：nums1 = [1,3], nums2 = [2]
输出：2.00000
解释：合并数组 = [1,2,3] ，中位数 2
```

示例 2：

```text
输入：nums1 = [1,2], nums2 = [3,4]
输出：2.50000
解释：合并数组 = [1,2,3,4] ，中位数 (2 + 3) / 2 = 2.5
```

 
 
提示：

 `nums1.length == m` 
 `nums2.length == n` 
 `0 <= m <= 1000` 
 `0 <= n <= 1000` 
 `1 <= m + n <= 2000` 
 `-106 <= nums1[i], nums2[i] <= 106`

### Java 解法补充

#### 基础解法：归并后取中位数

算法思想：两个数组已经有序，按归并排序的合并过程生成一个有序数组，再根据总长度奇偶性取中位数。

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m = nums1.length;
        int n = nums2.length;
        int[] merged = new int[m + n];
        int i = 0;
        int j = 0;
        int k = 0;

        while (i < m || j < n) {
            if (j == n || (i < m && nums1[i] <= nums2[j])) {
                merged[k++] = nums1[i++];
            } else {
                merged[k++] = nums2[j++];
            }
        }

        int total = m + n;
        if (total % 2 == 1) {
            return merged[total / 2];
        }
        return (merged[total / 2 - 1] + merged[total / 2]) / 2.0;
    }
}
```

复杂度：时间 `O(m + n)`，空间 `O(m + n)`。

#### 资深解法：在较短数组上二分切分

算法思想：中位数本质是把两个数组切成左右两部分，左半部分元素数量等于或比右半部分多 1，并且左半部分最大值不大于右半部分最小值。只需要在较短数组上二分切分点。

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        if (nums1.length > nums2.length) {
            return findMedianSortedArrays(nums2, nums1);
        }

        int m = nums1.length;
        int n = nums2.length;
        int leftSize = (m + n + 1) / 2;
        int low = 0;
        int high = m;

        while (low <= high) {
            int i = low + (high - low) / 2;
            int j = leftSize - i;

            int aLeft = i == 0 ? Integer.MIN_VALUE : nums1[i - 1];
            int aRight = i == m ? Integer.MAX_VALUE : nums1[i];
            int bLeft = j == 0 ? Integer.MIN_VALUE : nums2[j - 1];
            int bRight = j == n ? Integer.MAX_VALUE : nums2[j];

            if (aLeft <= bRight && bLeft <= aRight) {
                int leftMax = Math.max(aLeft, bLeft);
                if ((m + n) % 2 == 1) {
                    return leftMax;
                }
                int rightMin = Math.min(aRight, bRight);
                return (leftMax + rightMin) / 2.0;
            } else if (aLeft > bRight) {
                high = i - 1;
            } else {
                low = i + 1;
            }
        }

        return 0.0;
    }
}
```

复杂度：时间 `O(log min(m, n))`，空间 `O(1)`。

#### 基础语法与算法思想

- `Integer.MIN_VALUE` / `Integer.MAX_VALUE`：用作越界切分时的哨兵值。
- `low + (high - low) / 2`：避免二分中点相加溢出。
- `return (a + b) / 2.0`：除以 `2.0` 得到浮点结果。
- 核心思想：有序数组题先考虑二分；本题二分的对象不是答案值，而是数组切分位置。

---

## 5. 最长回文子串 (Medium)

给你一个字符串  `s` ，找到  `s`  中最长的 回文 子串。
 
示例 1：

```text
输入：s = "babad"
输出："bab"
解释："aba" 同样是符合题意的答案。
```

示例 2：

```text
输入：s = "cbbd"
输出："bb"
```

 
提示：

 `1 <= s.length <= 1000` 
 `s`  仅由数字和英文字母组成

### Java 解法补充

#### 基础解法：枚举所有子串

算法思想：枚举子串左右端点，判断这个子串是否为回文，并记录最长答案。写法直接，但重复判断很多。

```java
class Solution {
    public String longestPalindrome(String s) {
        String ans = "";
        for (int left = 0; left < s.length(); left++) {
            for (int right = left; right < s.length(); right++) {
                if (right - left + 1 > ans.length() && isPalindrome(s, left, right)) {
                    ans = s.substring(left, right + 1);
                }
            }
        }
        return ans;
    }

    private boolean isPalindrome(String s, int left, int right) {
        while (left < right) {
            if (s.charAt(left) != s.charAt(right)) {
                return false;
            }
            left++;
            right--;
        }
        return true;
    }
}
```

复杂度：时间 `O(n^3)`，空间 `O(1)`。

#### 资深解法：中心扩展

算法思想：回文串从中心向两侧对称扩展。中心可能是一个字符，也可能是两个字符之间的位置，因此每个下标都尝试奇数中心和偶数中心。

```java
class Solution {
    public String longestPalindrome(String s) {
        int bestLeft = 0;
        int bestRight = 0;

        for (int i = 0; i < s.length(); i++) {
            int len1 = expand(s, i, i);
            int len2 = expand(s, i, i + 1);
            int len = Math.max(len1, len2);

            if (len > bestRight - bestLeft + 1) {
                bestLeft = i - (len - 1) / 2;
                bestRight = i + len / 2;
            }
        }

        return s.substring(bestLeft, bestRight + 1);
    }

    private int expand(String s, int left, int right) {
        while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
            left--;
            right++;
        }
        return right - left - 1;
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(1)`。

#### 基础语法与算法思想

- `s.substring(left, right + 1)`：截取闭区间 `[left, right]` 时，右端要写成 `right + 1`。
- `private` 方法：把判断回文或扩展逻辑拆成辅助函数。
- 核心思想：回文问题优先想“对称中心”，再考虑 DP 或 Manacher。

---

## 6. Z 字形变换 (Medium)

将一个给定字符串  `s`  根据给定的行数  `numRows`  ，以从上往下、从左到右进行 Z 字形排列。
比如输入字符串为  `"PAYPALISHIRING"`  行数为  `3`  时，排列如下：

```text
P   A   H   N
A P L S I I G
Y   I   R
```

之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如： `"PAHNAPLSIIGYIR"` 。
请你实现这个将字符串进行指定行数变换的函数：

```text
string convert(string s, int numRows);
```

 
示例 1：

```text
输入：s = "PAYPALISHIRING", numRows = 3
输出："PAHNAPLSIIGYIR"
```

示例 2：

```text
输入：s = "PAYPALISHIRING", numRows = 4
输出："PINALSIGYAHRPI"
解释：
P     I    N
A   L S  I G
Y A   H R
P     I
```

示例 3：

```text
输入：s = "A", numRows = 1
输出："A"
```

 
提示：

 `1 <= s.length <= 1000` 
 `s`  由英文字母（小写和大写）、 `','`  和  `'.'`  组成
 `1 <= numRows <= 1000`

### Java 解法补充

#### 基础解法：逐字符模拟行走

算法思想：准备 `numRows` 个 `StringBuilder` 表示每一行，指针从上到下再从下到上移动，把字符放到对应行，最后拼接所有行。

```java
class Solution {
    public String convert(String s, int numRows) {
        if (numRows == 1 || numRows >= s.length()) {
            return s;
        }

        StringBuilder[] rows = new StringBuilder[numRows];
        for (int i = 0; i < numRows; i++) {
            rows[i] = new StringBuilder();
        }

        int row = 0;
        int direction = 1;
        for (int i = 0; i < s.length(); i++) {
            rows[row].append(s.charAt(i));
            if (row == 0) {
                direction = 1;
            } else if (row == numRows - 1) {
                direction = -1;
            }
            row += direction;
        }

        StringBuilder ans = new StringBuilder();
        for (StringBuilder builder : rows) {
            ans.append(builder);
        }
        return ans.toString();
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 资深解法：按周期直接取字符

算法思想：Z 字形每个周期长度为 `2 * numRows - 2`。逐行收集字符时，首尾行每个周期取一个字符，中间行每个周期取两个字符。

```java
class Solution {
    public String convert(String s, int numRows) {
        if (numRows == 1 || numRows >= s.length()) {
            return s;
        }

        int cycle = 2 * numRows - 2;
        StringBuilder ans = new StringBuilder();

        for (int row = 0; row < numRows; row++) {
            for (int start = row; start < s.length(); start += cycle) {
                ans.append(s.charAt(start));
                int diagonal = start + cycle - 2 * row;
                if (row != 0 && row != numRows - 1 && diagonal < s.length()) {
                    ans.append(s.charAt(diagonal));
                }
            }
        }

        return ans.toString();
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 基础语法与算法思想

- `StringBuilder[] rows = new StringBuilder[numRows]`：创建对象数组后，每个格子还要单独 `new`。
- 增强 `for`：`for (StringBuilder builder : rows)` 遍历数组元素。
- 核心思想：模拟题可以先按过程写；熟练后寻找周期或数学规律，减少状态变量。

---

## 7. 整数反转 (Medium)

给你一个 32 位的有符号整数  `x`  ，返回将  `x`  中的数字部分反转后的结果。
如果反转后整数超过 32 位的有符号整数的范围  `[−231,  231 − 1]`  ，就返回 0。
假设环境不允许存储 64 位整数（有符号或无符号）。
 
示例 1：

```text
输入：x = 123
输出：321
```

示例 2：

```text
输入：x = -123
输出：-321
```

示例 3：

```text
输入：x = 120
输出：21
```

示例 4：

```text
输入：x = 0
输出：0
```

 
提示：

 `-231 <= x <= 231 - 1`

### Java 解法补充

#### 基础解法：字符串反转并捕获溢出

算法思想：把整数转成字符串，去掉负号后反转，再尝试解析回 `int`。如果解析溢出，`Integer.parseInt` 会抛异常，返回 0。

```java
class Solution {
    public int reverse(int x) {
        String s = Integer.toString(x);
        boolean negative = s.charAt(0) == '-';
        String body = negative ? s.substring(1) : s;
        String reversed = new StringBuilder(body).reverse().toString();
        if (negative) {
            reversed = "-" + reversed;
        }

        try {
            return Integer.parseInt(reversed);
        } catch (NumberFormatException e) {
            return 0;
        }
    }
}
```

复杂度：时间 `O(d)`，空间 `O(d)`，`d` 为数字位数。

#### 资深解法：整数逐位反转并提前判断溢出

算法思想：每次取出末位 `digit = x % 10`，准备执行 `ans = ans * 10 + digit`。在乘 10 和加 digit 之前先判断是否越界。

```java
class Solution {
    public int reverse(int x) {
        int ans = 0;

        while (x != 0) {
            int digit = x % 10;
            x /= 10;

            if (ans > Integer.MAX_VALUE / 10 ||
                    (ans == Integer.MAX_VALUE / 10 && digit > 7)) {
                return 0;
            }
            if (ans < Integer.MIN_VALUE / 10 ||
                    (ans == Integer.MIN_VALUE / 10 && digit < -8)) {
                return 0;
            }

            ans = ans * 10 + digit;
        }

        return ans;
    }
}
```

复杂度：时间 `O(d)`，空间 `O(1)`。

#### 基础语法与算法思想

- `x % 10`：取个位；Java 中负数取模仍保留负号，例如 `-123 % 10 == -3`。
- `x /= 10`：去掉个位。
- `try/catch`：捕获异常，适合基础版理解溢出。
- 核心思想：整数边界题要在危险操作之前判断，而不是操作之后再补救。

---

## 8. 字符串转换整数 (atoi) (Medium)

请你来实现一个  `myAtoi(string s)`  函数，使其能将字符串转换成一个 32 位有符号整数。
函数  `myAtoi(string s)`  的算法如下：

空格：读入字符串并丢弃无用的前导空格（ `" "` ）
符号：检查下一个字符（假设还未到字符末尾）为  `'-'`  还是  `'+'` 。如果两者都不存在，则假定结果为正。
转换：通过跳过前置零来读取该整数，直到遇到非数字字符或到达字符串的结尾。如果没有读取数字，则结果为0。
舍入：如果整数数超过 32 位有符号整数范围  `[−231,  231 − 1]`  ，需要截断这个整数，使其保持在这个范围内。具体来说，小于  `−231`  的整数应该被舍入为  `−231`  ，大于  `231 − 1`  的整数应该被舍入为  `231 − 1`  。

返回整数作为最终结果。
 
示例 1：

输入：s = "42"
输出：42
解释：加粗的字符串为已经读入的字符，插入符号是当前读取的字符。

```text
带下划线线的字符是所读的内容，插入符号是当前读入位置。
第 1 步："42"（当前没有读入字符，因为没有前导空格）
         ^
第 2 步："42"（当前没有读入字符，因为这里不存在 '-' 或者 '+'）
         ^
第 3 步："42"（读入 "42"）
           ^
```

示例 2：

输入：s = " -042"
输出：-42
解释：

```text
第 1 步："   -042"（读入前导空格，但忽视掉）
            ^
第 2 步："   -042"（读入 '-' 字符，所以结果应该是负数）
             ^
第 3 步："   -042"（读入 "042"，在结果中忽略前导零）
               ^
```

示例 3：

输入：s = "1337c0d3"
输出：1337
解释：

```text
第 1 步："1337c0d3"（当前没有读入字符，因为没有前导空格）
         ^
第 2 步："1337c0d3"（当前没有读入字符，因为这里不存在 '-' 或者 '+'）
         ^
第 3 步："1337c0d3"（读入 "1337"；由于下一个字符不是一个数字，所以读入停止）
             ^
```

示例 4：

输入：s = "0-1"
输出：0
解释：

```text
第 1 步："0-1" (当前没有读入字符，因为没有前导空格)
         ^
第 2 步："0-1" (当前没有读入字符，因为这里不存在 '-' 或者 '+')
         ^
第 3 步："0-1" (读入 "0"；由于下一个字符不是一个数字，所以读入停止)
          ^
```

示例 5：

输入：s = "words and 987"
输出：0
解释：
读取在第一个非数字字符“w”处停止。

 
提示：

 `0 <= s.length <= 200` 
 `s`  由英文字母（大写和小写）、数字（ `0-9` ）、 `' '` 、 `'+'` 、 `'-'`  和  `'.'`  组成

### Java 解法补充

#### 基础解法：按规则扫描

算法思想：按题目规则分四步处理：跳过前导空格、读取符号、读取连续数字、超过范围时截断。

```java
class Solution {
    public int myAtoi(String s) {
        int i = 0;
        int n = s.length();

        while (i < n && s.charAt(i) == ' ') {
            i++;
        }

        int sign = 1;
        if (i < n && (s.charAt(i) == '+' || s.charAt(i) == '-')) {
            sign = s.charAt(i) == '-' ? -1 : 1;
            i++;
        }

        long value = 0;
        while (i < n && Character.isDigit(s.charAt(i))) {
            value = value * 10 + (s.charAt(i) - '0');
            if (sign == 1 && value > Integer.MAX_VALUE) {
                return Integer.MAX_VALUE;
            }
            if (sign == -1 && -value < Integer.MIN_VALUE) {
                return Integer.MIN_VALUE;
            }
            i++;
        }

        return (int) (value * sign);
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 资深解法：用 int 边界提前截断

算法思想：不用 `long` 存大数，而是在 `ans = ans * 10 + digit` 之前判断是否会超过正数边界。负数边界比正数多 1，最终按符号返回对应截断值。

```java
class Solution {
    public int myAtoi(String s) {
        int i = 0;
        int n = s.length();

        while (i < n && s.charAt(i) == ' ') {
            i++;
        }

        int sign = 1;
        if (i < n && (s.charAt(i) == '+' || s.charAt(i) == '-')) {
            sign = s.charAt(i) == '-' ? -1 : 1;
            i++;
        }

        int ans = 0;
        while (i < n && Character.isDigit(s.charAt(i))) {
            int digit = s.charAt(i) - '0';
            if (ans > (Integer.MAX_VALUE - digit) / 10) {
                return sign == 1 ? Integer.MAX_VALUE : Integer.MIN_VALUE;
            }
            ans = ans * 10 + digit;
            i++;
        }

        return sign * ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- `Character.isDigit(c)`：判断字符是否为数字。
- `s.charAt(i) - '0'`：把数字字符转成整数。
- `(int) value`：强制类型转换。
- 核心思想：字符串解析题适合拆成有限步骤，每一步只消费自己负责的字符。

---

## 9. 回文数 (Easy)

给你一个整数  `x`  ，如果  `x`  是一个回文整数，返回  `true`  ；否则，返回  `false`  。
回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

例如， `121`  是回文，而  `123`  不是。

 
示例 1：

```text
输入：x = 121
输出：true
```

示例 2：

```text
输入：x = -121
输出：false
解释：从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。
```

示例 3：

```text
输入：x = 10
输出：false
解释：从右向左读, 为 01 。因此它不是一个回文数。
```

 
提示：

 `-231 <= x <= 231 - 1` 

 
进阶：你能不将整数转为字符串来解决这个问题吗？

### Java 解法补充

#### 基础解法：转字符串双指针

算法思想：把整数转成字符串，用左右指针从两端向中间比较。只要有一对字符不同，就不是回文。

```java
class Solution {
    public boolean isPalindrome(int x) {
        String s = Integer.toString(x);
        int left = 0;
        int right = s.length() - 1;

        while (left < right) {
            if (s.charAt(left) != s.charAt(right)) {
                return false;
            }
            left++;
            right--;
        }

        return true;
    }
}
```

复杂度：时间 `O(d)`，空间 `O(d)`，`d` 为数字位数。

#### 资深解法：反转后一半数字

算法思想：负数一定不是回文；非零且末尾是 0 的数也不是回文。之后只反转数字后一半，当 `reversed >= x` 时停止，再比较两半是否相等。

```java
class Solution {
    public boolean isPalindrome(int x) {
        if (x < 0 || (x != 0 && x % 10 == 0)) {
            return false;
        }

        int reversed = 0;
        while (x > reversed) {
            reversed = reversed * 10 + x % 10;
            x /= 10;
        }

        return x == reversed || x == reversed / 10;
    }
}
```

复杂度：时间 `O(d)`，空间 `O(1)`。

#### 基础语法与算法思想

- `||`：逻辑或，用来组合两个可接受条件。
- `x % 10 == 0`：判断末位是否为 0。
- `reversed / 10`：奇数位数字时去掉中间位。
- 核心思想：只反转一半可以避免完整反转的溢出风险。

---

## 10. 正则表达式匹配 (Hard)

给你一个字符串  `s`  和一个字符规律  `p` ，请你来实现一个支持  `'.'`  和  `'*'`  的正则表达式匹配。

 `'.'`  匹配任意单个字符
 `'*'`  匹配零个或多个前面的那一个元素

返回一个布尔值，表示匹配是否覆盖整个输入字符串（而非部分）。
 

示例 1：

```text
输入：s = "aa", p = "a"
输出：false
解释："a" 无法匹配 "aa" 整个字符串。
```

示例 2:

```text
输入：s = "aa", p = "a*"
输出：true
解释：因为 '*' 代表可以匹配零个或多个前面的那一个元素, 在这里前面的元素就是 'a'。因此，字符串 "aa" 可被视为 'a' 重复了一次。
```

示例 3：

```text
输入：s = "ab", p = ".*"
输出：true
解释：".*" 表示可匹配零个或多个（'*'）任意字符（'.'）。
```

 
提示：

 `1 <= s.length <= 20` 
 `1 <= p.length <= 20` 
 `s`  只包含从  `a-z`  的小写字母。
 `p`  只包含从  `a-z`  的小写字母，以及字符  `.`  和  `*` 。
保证每次出现字符  `*`  时，前面都匹配到有效的字符

### Java 解法补充

#### 基础解法：记忆化递归

算法思想：定义 `match(i, j)` 表示 `s` 从 `i` 开始的后缀能否匹配 `p` 从 `j` 开始的后缀。若 `p[j + 1]` 是 `*`，可以让前一个元素出现 0 次，也可以在当前字符匹配时消耗 `s[i]` 并继续留在同一个模式位置。

```java
class Solution {
    private Boolean[][] memo;

    public boolean isMatch(String s, String p) {
        memo = new Boolean[s.length() + 1][p.length() + 1];
        return dfs(s, p, 0, 0);
    }

    private boolean dfs(String s, String p, int i, int j) {
        if (memo[i][j] != null) {
            return memo[i][j];
        }

        boolean ans;
        if (j == p.length()) {
            ans = i == s.length();
        } else {
            boolean firstMatch = i < s.length() &&
                    (p.charAt(j) == s.charAt(i) || p.charAt(j) == '.');

            if (j + 1 < p.length() && p.charAt(j + 1) == '*') {
                ans = dfs(s, p, i, j + 2) || (firstMatch && dfs(s, p, i + 1, j));
            } else {
                ans = firstMatch && dfs(s, p, i + 1, j + 1);
            }
        }

        memo[i][j] = ans;
        return ans;
    }
}
```

复杂度：时间 `O(mn)`，空间 `O(mn)`，`m = s.length()`，`n = p.length()`。

#### 资深解法：自底向上动态规划

算法思想：`dp[i][j]` 表示 `s` 的前 `i` 个字符能否匹配 `p` 的前 `j` 个字符。遇到普通字符或 `.` 时，看前一格；遇到 `*` 时，先考虑它让前一个字符出现 0 次，再考虑出现至少 1 次。

```java
class Solution {
    public boolean isMatch(String s, String p) {
        int m = s.length();
        int n = p.length();
        boolean[][] dp = new boolean[m + 1][n + 1];
        dp[0][0] = true;

        for (int j = 2; j <= n; j++) {
            if (p.charAt(j - 1) == '*') {
                dp[0][j] = dp[0][j - 2];
            }
        }

        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                char pc = p.charAt(j - 1);
                if (pc == '*') {
                    dp[i][j] = dp[i][j - 2];
                    char prev = p.charAt(j - 2);
                    if (prev == '.' || prev == s.charAt(i - 1)) {
                        dp[i][j] = dp[i][j] || dp[i - 1][j];
                    }
                } else if (pc == '.' || pc == s.charAt(i - 1)) {
                    dp[i][j] = dp[i - 1][j - 1];
                }
            }
        }

        return dp[m][n];
    }
}
```

复杂度：时间 `O(mn)`，空间 `O(mn)`。

#### 基础语法与算法思想

- `Boolean[][] memo`：包装类型可以用 `null` 表示“还没算过”。
- `boolean[][] dp`：二维布尔表，默认值为 `false`。
- `&&` 和 `||`：递归和 DP 中常用来组合状态。
- 核心思想：含 `*` 的匹配题必须拆成两种情况：前一个元素出现 0 次，或当前字符匹配后继续消耗。

---

## 11. 盛最多水的容器 (Medium)

给定一个长度为  `n`  的整数数组  `height`  。有  `n`  条垂线，第  `i`  条线的两个端点是  `(i, 0)`  和  `(i, height[i])`  。
找出其中的两条线，使得它们与  `x`  轴共同构成的容器可以容纳最多的水。
返回容器可以储存的最大水量。
说明：你不能倾斜容器。
 
示例 1：

```text
输入：[1,8,6,2,5,4,8,3,7]
输出：49 
解释：图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。
```

示例 2：

```text
输入：height = [1,1]
输出：1
```

 
提示：

 `n == height.length` 
 `2 <= n <= 105` 
 `0 <= height[i] <= 104`

### Java 解法补充

#### 基础解法：枚举两条线

算法思想：枚举所有左右边界 `(i, j)`，容器高度由较短线决定，宽度是 `j - i`，面积为 `min(height[i], height[j]) * (j - i)`。

```java
class Solution {
    public int maxArea(int[] height) {
        int ans = 0;
        for (int i = 0; i < height.length; i++) {
            for (int j = i + 1; j < height.length; j++) {
                int area = Math.min(height[i], height[j]) * (j - i);
                ans = Math.max(ans, area);
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(1)`。

#### 资深解法：双指针

算法思想：左右指针从两端开始。当前面积受较短线限制，移动较长线不会让高度变大，只会让宽度变小，所以每次移动较短线。

```java
class Solution {
    public int maxArea(int[] height) {
        int left = 0;
        int right = height.length - 1;
        int ans = 0;

        while (left < right) {
            int area = Math.min(height[left], height[right]) * (right - left);
            ans = Math.max(ans, area);
            if (height[left] < height[right]) {
                left++;
            } else {
                right--;
            }
        }

        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- `Math.min(a, b)`：取较小值。
- 双指针常用于数组两端向中间收缩。
- 核心思想：本题的贪心依据是“面积短板由短线决定”，所以只移动短线才可能获得更高高度。

---

## 12. 整数转罗马数字 (Medium)

七个不同的符号代表罗马数字，其值如下：

符号
值

I
1

V
5

X
10

L
50

C
100

D
500

M
1000

罗马数字是通过添加从最高到最低的小数位值的转换而形成的。将小数位值转换为罗马数字有以下规则：

如果该值不是以 4 或 9 开头，请选择可以从输入中减去的最大值的符号，将该符号附加到结果，减去其值，然后将其余部分转换为罗马数字。
如果该值以 4 或 9 开头，使用 减法形式，表示从以下符号中减去一个符号，例如 4 是 5 ( `V` ) 减 1 ( `I` ):  `IV`  ，9 是 10 ( `X` ) 减 1 ( `I` )： `IX` 。仅使用以下减法形式：4 ( `IV` )，9 ( `IX` )，40 ( `XL` )，90 ( `XC` )，400 ( `CD` ) 和 900 ( `CM` )。
只有 10 的次方（ `I` ,  `X` ,  `C` ,  `M` ）最多可以连续附加 3 次以代表 10 的倍数。你不能多次附加 5 ( `V` )，50 ( `L` ) 或 500 ( `D` )。如果需要将符号附加4次，请使用 减法形式。

给定一个整数，将其转换为罗马数字。
 
示例 1：

输入：num = 3749
输出： "MMMDCCXLIX"
解释：

```text
3000 = MMM 由于 1000 (M) + 1000 (M) + 1000 (M)
 700 = DCC 由于 500 (D) + 100 (C) + 100 (C)
  40 = XL 由于 50 (L) 减 10 (X)
   9 = IX 由于 10 (X) 减 1 (I)
注意：49 不是 50 (L) 减 1 (I) 因为转换是基于小数位
```

示例 2：

输入：num = 58
输出："LVIII"
解释：

```text
50 = L
 8 = VIII
```

示例 3：

输入：num = 1994
输出："MCMXCIV"
解释：

```text
1000 = M
 900 = CM
  90 = XC
   4 = IV
```

 
提示：

 `1 <= num <= 3999`

### Java 解法补充

#### 基础解法：贪心减法

算法思想：从大到小列出所有罗马数值和符号，每次尽量使用当前最大符号，直到数字减为 0。

```java
class Solution {
    public String intToRoman(int num) {
        int[] values = {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
        String[] symbols = {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};
        StringBuilder ans = new StringBuilder();

        for (int i = 0; i < values.length; i++) {
            while (num >= values[i]) {
                num -= values[i];
                ans.append(symbols[i]);
            }
        }

        return ans.toString();
    }
}
```

复杂度：时间 `O(1)`，空间 `O(1)`，因为输入范围固定。

#### 资深解法：按位查表

算法思想：罗马数字的千位、百位、十位、个位各自只有 10 种以内的固定写法，直接按数位查表拼接，代码更短且不需要循环减法。

```java
class Solution {
    public String intToRoman(int num) {
        String[] thousands = {"", "M", "MM", "MMM"};
        String[] hundreds = {"", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM"};
        String[] tens = {"", "X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC"};
        String[] ones = {"", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX"};

        return thousands[num / 1000]
                + hundreds[num % 1000 / 100]
                + tens[num % 100 / 10]
                + ones[num % 10];
    }
}
```

复杂度：时间 `O(1)`，空间 `O(1)`。

#### 基础语法与算法思想

- `StringBuilder`：频繁拼接字符串时更高效。
- 字符串数组可作为查表结构，直接用下标取固定答案。
- 核心思想：映射规则固定且范围很小时，查表通常比复杂判断更清晰。

---

## 13. 罗马数字转整数 (Easy)

罗马数字包含以下七种字符:  `I` ，  `V` ，  `X` ，  `L` ， `C` ， `D`  和  `M` 。

```text
字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

例如， 罗马数字  `2`  写做  `II`  ，即为两个并列的 1 。 `12`  写做  `XII`  ，即为  `X`  +  `II`  。  `27`  写做   `XXVII` , 即为  `XX`  +  `V`  +  `II`  。
通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做  `IIII` ，而是  `IV` 。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为  `IX` 。这个特殊的规则只适用于以下六种情况：

 `I`  可以放在  `V`  (5) 和  `X`  (10) 的左边，来表示 4 和 9。
 `X`  可以放在  `L`  (50) 和  `C`  (100) 的左边，来表示 40 和 90。 
 `C`  可以放在  `D`  (500) 和  `M`  (1000) 的左边，来表示 400 和 900。

给定一个罗马数字，将其转换成整数。
 
示例 1:

```text
输入: s = "III"
输出: 3
```

示例 2:

```text
输入: s = "IV"
输出: 4
```

示例 3:

```text
输入: s = "IX"
输出: 9
```

示例 4:

```text
输入: s = "LVIII"
输出: 58
解释: L = 50, V= 5, III = 3.
```

示例 5:

```text
输入: s = "MCMXCIV"
输出: 1994
解释: M = 1000, CM = 900, XC = 90, IV = 4.
```

 
提示：

 `1 <= s.length <= 15` 
 `s`  仅含字符  `('I', 'V', 'X', 'L', 'C', 'D', 'M')` 
题目数据保证  `s`  是一个有效的罗马数字，且表示整数在范围  `[1, 3999]`  内
题目所给测试用例皆符合罗马数字书写规则，不会出现跨位等情况。
IL 和 IM 这样的例子并不符合题目要求，49 应该写作 XLIX，999 应该写作 CMXCIX 。
关于罗马数字的详尽书写规则，可以参考 罗马数字 - 百度百科。

### Java 解法补充

#### 基础解法：从左到右识别减法对

算法思想：如果当前符号小于右侧符号，说明它和右侧组成减法形式，例如 `IV`、`CM`，本轮减去当前值；否则加上当前值。

```java
class Solution {
    public int romanToInt(String s) {
        int ans = 0;
        for (int i = 0; i < s.length(); i++) {
            int cur = valueOf(s.charAt(i));
            if (i + 1 < s.length() && cur < valueOf(s.charAt(i + 1))) {
                ans -= cur;
            } else {
                ans += cur;
            }
        }
        return ans;
    }

    private int valueOf(char c) {
        if (c == 'I') return 1;
        if (c == 'V') return 5;
        if (c == 'X') return 10;
        if (c == 'L') return 50;
        if (c == 'C') return 100;
        if (c == 'D') return 500;
        return 1000;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 资深解法：从右到左维护右侧最大值

算法思想：从右向左扫描，若当前值小于右侧已经见过的最大值，就减去它；否则加上它并更新最大值。

```java
class Solution {
    public int romanToInt(String s) {
        int ans = 0;
        int maxRight = 0;

        for (int i = s.length() - 1; i >= 0; i--) {
            int cur = valueOf(s.charAt(i));
            if (cur < maxRight) {
                ans -= cur;
            } else {
                ans += cur;
                maxRight = cur;
            }
        }

        return ans;
    }

    private int valueOf(char c) {
        switch (c) {
            case 'I': return 1;
            case 'V': return 5;
            case 'X': return 10;
            case 'L': return 50;
            case 'C': return 100;
            case 'D': return 500;
            default: return 1000;
        }
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- `char`：保存单个字符。
- `switch`：适合把少量固定字符映射成固定值。
- 核心思想：罗马数字的减法只发生在小值位于大值左侧时。

---

## 14. 最长公共前缀 (Easy)

编写一个函数来查找字符串数组中的最长公共前缀。
如果不存在公共前缀，返回空字符串  `""` 。
 
示例 1：

```text
输入：strs = ["flower","flow","flight"]
输出："fl"
```

示例 2：

```text
输入：strs = ["dog","racecar","car"]
输出：""
解释：输入不存在公共前缀。
```

 
提示：

 `1 <= strs.length <= 200` 
 `0 <= strs[i].length <= 200` 
 `strs[i]`  如果非空，则仅由小写英文字母组成

### Java 解法补充

#### 基础解法：逐步缩短前缀

算法思想：先把第一个字符串当作公共前缀，再逐个检查后面的字符串。如果某个字符串不是以当前前缀开头，就不断删除前缀最后一个字符。

```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        String prefix = strs[0];

        for (int i = 1; i < strs.length; i++) {
            while (!strs[i].startsWith(prefix)) {
                prefix = prefix.substring(0, prefix.length() - 1);
                if (prefix.isEmpty()) {
                    return "";
                }
            }
        }

        return prefix;
    }
}
```

复杂度：时间 `O(S)`，空间 `O(1)`，`S` 为所有字符串总字符数级别。

#### 资深解法：纵向扫描

算法思想：按列比较所有字符串的同一位置。只要某个字符串到头或字符不一致，就返回第一列到当前列之前的部分。

```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        for (int col = 0; col < strs[0].length(); col++) {
            char c = strs[0].charAt(col);
            for (int row = 1; row < strs.length; row++) {
                if (col == strs[row].length() || strs[row].charAt(col) != c) {
                    return strs[0].substring(0, col);
                }
            }
        }
        return strs[0];
    }
}
```

复杂度：时间 `O(S)`，空间 `O(1)`。

#### 基础语法与算法思想

- `startsWith(prefix)`：判断字符串是否以指定前缀开头。
- `isEmpty()`：判断字符串长度是否为 0。
- 核心思想：公共前缀一旦某一列不一致，后面不可能再成为前缀。

---

## 15. 三数之和 (Medium)

给你一个整数数组  `nums`  ，判断是否存在三元组  `[nums[i], nums[j], nums[k]]`  满足  `i != j` 、 `i != k`  且  `j != k`  ，同时还满足  `nums[i] + nums[j] + nums[k] == 0`  。请你返回所有和为  `0`  且不重复的三元组。
注意：答案中不可以包含重复的三元组。
 
 
示例 1：

```text
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
解释：
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0 。
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0 。
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0 。
不同的三元组是 [-1,0,1] 和 [-1,-1,2] 。
注意，输出的顺序和三元组的顺序并不重要。
```

示例 2：

```text
输入：nums = [0,1,1]
输出：[]
解释：唯一可能的三元组和不为 0 。
```

示例 3：

```text
输入：nums = [0,0,0]
输出：[[0,0,0]]
解释：唯一可能的三元组和为 0 。
```

 
提示：

 `3 <= nums.length <= 3000` 
 `-105 <= nums[i] <= 105`

### Java 解法补充

#### 基础解法：三重循环加去重集合

算法思想：枚举所有三元组，命中和为 0 后放入集合去重。为了让同一组数字有相同表示，先对数组排序。

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashSet;
import java.util.List;
import java.util.Set;

class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        Set<List<Integer>> seen = new HashSet<>();

        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                for (int k = j + 1; k < nums.length; k++) {
                    if (nums[i] + nums[j] + nums[k] == 0) {
                        seen.add(Arrays.asList(nums[i], nums[j], nums[k]));
                    }
                }
            }
        }

        return new ArrayList<>(seen);
    }
}
```

复杂度：时间 `O(n^3)`，空间 `O(ans)`。

#### 资深解法：排序加双指针

算法思想：排序后固定第一个数 `i`，问题变成在右侧有序区间里找两个数之和为 `-nums[i]`。用左右指针收缩，并跳过重复值。

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> ans = new ArrayList<>();

        for (int i = 0; i < nums.length - 2; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            if (nums[i] > 0) {
                break;
            }

            int left = i + 1;
            int right = nums.length - 1;
            while (left < right) {
                int sum = nums[i] + nums[left] + nums[right];
                if (sum == 0) {
                    ans.add(Arrays.asList(nums[i], nums[left], nums[right]));
                    left++;
                    right--;
                    while (left < right && nums[left] == nums[left - 1]) left++;
                    while (left < right && nums[right] == nums[right + 1]) right--;
                } else if (sum < 0) {
                    left++;
                } else {
                    right--;
                }
            }
        }

        return ans;
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(1)`，不计答案空间。

#### 基础语法与算法思想

- `Arrays.sort(nums)`：原地升序排序数组。
- `List<List<Integer>>`：二维列表，保存多个三元组。
- `Arrays.asList(a, b, c)`：快速创建固定元素列表。
- 核心思想：排序后可用双指针把两数搜索从 `O(n^2)` 降到 `O(n)`。

---

## 16. 最接近的三数之和 (Medium)

给你一个长度为  `n`  的整数数组  `nums`  和 一个目标值  `target` 。请你从  `nums`  中选出三个在 不同下标位置 的整数，使它们的和与  `target`  最接近。
返回这三个数的和。
假定每组输入只存在恰好一个解。
 
示例 1：

```text
输入：nums = [-1,2,1,-4], target = 1
输出：2
解释：与 target 最接近的和是 2 (-1 + 2 + 1 = 2)。
```

示例 2：

```text
输入：nums = [0,0,0], target = 1
输出：0
解释：与 target 最接近的和是 0（0 + 0 + 0 = 0）。
```

 
提示：

 `3 <= nums.length <= 1000` 
 `-1000 <= nums[i] <= 1000` 
 `-104 <= target <= 104`

### Java 解法补充

#### 基础解法：枚举所有三元组

算法思想：直接枚举所有三元组，维护与 `target` 差距最小的和。

```java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        int best = nums[0] + nums[1] + nums[2];

        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                for (int k = j + 1; k < nums.length; k++) {
                    int sum = nums[i] + nums[j] + nums[k];
                    if (Math.abs(sum - target) < Math.abs(best - target)) {
                        best = sum;
                    }
                }
            }
        }

        return best;
    }
}
```

复杂度：时间 `O(n^3)`，空间 `O(1)`。

#### 资深解法：排序加双指针

算法思想：固定一个数后，用双指针搜索另外两个数。当前和小于目标时左指针右移，当前和大于目标时右指针左移。

```java
import java.util.Arrays;

class Solution {
    public int threeSumClosest(int[] nums, int target) {
        Arrays.sort(nums);
        int best = nums[0] + nums[1] + nums[2];

        for (int i = 0; i < nums.length - 2; i++) {
            int left = i + 1;
            int right = nums.length - 1;

            while (left < right) {
                int sum = nums[i] + nums[left] + nums[right];
                if (Math.abs(sum - target) < Math.abs(best - target)) {
                    best = sum;
                }
                if (sum == target) {
                    return target;
                } else if (sum < target) {
                    left++;
                } else {
                    right--;
                }
            }
        }

        return best;
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(1)`。

#### 基础语法与算法思想

- `Math.abs(x)`：绝对值，常用于比较差距。
- 初始化答案时要用一个真实三元组，避免默认值干扰。
- 核心思想：最接近问题不要求列出所有答案，只要在搜索过程中维护当前最优。

---

## 17. 电话号码的字母组合 (Medium)

给定一个仅包含数字  `2-9`  的字符串，返回所有它能表示的字母组合。答案可以按 任意顺序 返回。
给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

 
示例 1：

```text
输入：digits = "23"
输出：["ad","ae","af","bd","be","bf","cd","ce","cf"]
```

示例 2：

```text
输入：digits = "2"
输出：["a","b","c"]
```

 
提示：

 `1 <= digits.length <= 4` 
 `digits[i]`  是范围  `['2', '9']`  的一个数字。

### Java 解法补充

#### 基础解法：逐层扩展列表

算法思想：从空字符串开始，每读取一个数字，就把当前所有前缀与该数字对应的每个字母拼接，形成下一层结果。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<String> letterCombinations(String digits) {
        List<String> ans = new ArrayList<>();
        if (digits.length() == 0) {
            return ans;
        }

        String[] map = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
        ans.add("");

        for (int i = 0; i < digits.length(); i++) {
            String letters = map[digits.charAt(i) - '0'];
            List<String> next = new ArrayList<>();
            for (String prefix : ans) {
                for (int j = 0; j < letters.length(); j++) {
                    next.add(prefix + letters.charAt(j));
                }
            }
            ans = next;
        }

        return ans;
    }
}
```

复杂度：时间 `O(4^n * n)`，空间 `O(4^n * n)`。

#### 资深解法：回溯

算法思想：每一层选择当前数字对应的一个字母，加入路径；递归到长度等于数字串长度时收集答案；返回时撤销选择。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    private final String[] map = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
    private final List<String> ans = new ArrayList<>();

    public List<String> letterCombinations(String digits) {
        if (digits.length() == 0) {
            return ans;
        }
        backtrack(digits, 0, new StringBuilder());
        return ans;
    }

    private void backtrack(String digits, int index, StringBuilder path) {
        if (index == digits.length()) {
            ans.add(path.toString());
            return;
        }

        String letters = map[digits.charAt(index) - '0'];
        for (int i = 0; i < letters.length(); i++) {
            path.append(letters.charAt(i));
            backtrack(digits, index + 1, path);
            path.deleteCharAt(path.length() - 1);
        }
    }
}
```

复杂度：时间 `O(4^n * n)`，空间 `O(n)`，不计答案空间。

#### 基础语法与算法思想

- `List<String>`：字符串列表。
- `path.append(c)` / `path.deleteCharAt(...)`：回溯中做选择和撤销选择。
- 核心思想：组合生成题天然适合回溯，“层数”对应输入位置，“选择”对应当前数字的字母。

---

## 18. 四数之和 (Medium)

给你一个由  `n`  个整数组成的数组  `nums`  ，和一个目标值  `target`  。请你找出并返回满足下述全部条件且不重复的四元组  `[nums[a], nums[b], nums[c], nums[d]]`  （若两个四元组元素一一对应，则认为两个四元组重复）：

 `0 <= a, b, c, d < n` 
 `a` 、 `b` 、 `c`  和  `d`  互不相同
 `nums[a] + nums[b] + nums[c] + nums[d] == target` 

你可以按 任意顺序 返回答案 。
 
示例 1：

```text
输入：nums = [1,0,-1,0,-2,2], target = 0
输出：[[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]
```

示例 2：

```text
输入：nums = [2,2,2,2,2], target = 8
输出：[[2,2,2,2]]
```

 
提示：

 `1 <= nums.length <= 200` 
 `-109 <= nums[i] <= 109` 
 `-109 <= target <= 109`

### Java 解法补充

#### 基础解法：四重循环加集合去重

算法思想：枚举所有四元组，排序数组后同一组数字天然有固定顺序，放进集合去重。用 `long` 计算和，避免整数溢出。

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashSet;
import java.util.List;
import java.util.Set;

class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        Arrays.sort(nums);
        Set<List<Integer>> seen = new HashSet<>();

        for (int a = 0; a < nums.length; a++) {
            for (int b = a + 1; b < nums.length; b++) {
                for (int c = b + 1; c < nums.length; c++) {
                    for (int d = c + 1; d < nums.length; d++) {
                        long sum = (long) nums[a] + nums[b] + nums[c] + nums[d];
                        if (sum == target) {
                            seen.add(Arrays.asList(nums[a], nums[b], nums[c], nums[d]));
                        }
                    }
                }
            }
        }

        return new ArrayList<>(seen);
    }
}
```

复杂度：时间 `O(n^4)`，空间 `O(ans)`。

#### 资深解法：排序加两层固定和双指针

算法思想：固定前两个数，把问题转成有序区间内的两数之和。每一层都跳过重复值，并用 `long` 保存四数之和。

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        Arrays.sort(nums);
        List<List<Integer>> ans = new ArrayList<>();
        int n = nums.length;

        for (int a = 0; a < n - 3; a++) {
            if (a > 0 && nums[a] == nums[a - 1]) continue;
            for (int b = a + 1; b < n - 2; b++) {
                if (b > a + 1 && nums[b] == nums[b - 1]) continue;

                int left = b + 1;
                int right = n - 1;
                while (left < right) {
                    long sum = (long) nums[a] + nums[b] + nums[left] + nums[right];
                    if (sum == target) {
                        ans.add(Arrays.asList(nums[a], nums[b], nums[left], nums[right]));
                        left++;
                        right--;
                        while (left < right && nums[left] == nums[left - 1]) left++;
                        while (left < right && nums[right] == nums[right + 1]) right--;
                    } else if (sum < target) {
                        left++;
                    } else {
                        right--;
                    }
                }
            }
        }

        return ans;
    }
}
```

复杂度：时间 `O(n^3)`，空间 `O(1)`，不计答案空间。

#### 基础语法与算法思想

- `(long) nums[a]`：先把第一个数转为 `long`，后续加法会按 `long` 计算。
- `continue`：跳过当前循环剩余部分，常用于去重。
- 核心思想：`kSum` 常见套路是排序、固定若干个数、最后用双指针处理两数之和。

---

## 19. 删除链表的倒数第 N 个结点 (Medium)

给你一个链表，删除链表的倒数第  `n`  个结点，并且返回链表的头结点。
 
示例 1：

```text
输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]
```

示例 2：

```text
输入：head = [1], n = 1
输出：[]
```

示例 3：

```text
输入：head = [1,2], n = 1
输出：[1]
```

 
提示：

链表中结点的数目为  `sz` 
 `1 <= sz <= 30` 
 `0 <= Node.val <= 100` 
 `1 <= n <= sz` 

 
进阶：你能尝试使用一趟扫描实现吗？

### Java 解法补充

#### 基础解法：先统计长度

算法思想：第一次遍历得到链表长度 `len`，要删除倒数第 `n` 个节点，也就是正数第 `len - n + 1` 个节点。用虚拟头结点处理删除头节点的情况。

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        int len = 0;
        ListNode cur = head;
        while (cur != null) {
            len++;
            cur = cur.next;
        }

        ListNode dummy = new ListNode(0);
        dummy.next = head;
        cur = dummy;
        for (int i = 0; i < len - n; i++) {
            cur = cur.next;
        }

        cur.next = cur.next.next;
        return dummy.next;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 资深解法：快慢指针一次遍历

算法思想：让快指针先走 `n + 1` 步，使快慢指针之间隔着 `n` 个节点。之后同时移动，快指针到空时，慢指针正好在待删除节点前一个位置。

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode fast = dummy;
        ListNode slow = dummy;

        for (int i = 0; i <= n; i++) {
            fast = fast.next;
        }

        while (fast != null) {
            fast = fast.next;
            slow = slow.next;
        }

        slow.next = slow.next.next;
        return dummy.next;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- `dummy.next = head`：把原链表接到虚拟头后面。
- 删除节点本质是 `prev.next = prev.next.next`。
- 核心思想：链表倒数问题常用快慢指针制造固定距离。

---

## 20. 有效的括号 (Easy)

给定一个只包括  `'('` ， `')'` ， `'{'` ， `'}'` ， `'['` ， `']'`  的字符串  `s`  ，判断字符串是否有效。
有效字符串需满足：

左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
每个右括号都有一个对应的相同类型的左括号。

 
示例 1：

输入：s = "()"
输出：true

示例 2：

输入：s = "()[]{}"
输出：true

示例 3：

输入：s = "(]"
输出：false

示例 4：

输入：s = "([])"
输出：true

示例 5：

输入：s = "([)]"
输出：false

 
提示：

 `1 <= s.length <= 104` 
 `s`  仅由括号  `'()[]{}'`  组成

### Java 解法补充

#### 基础解法：反复消除成对括号

算法思想：合法括号串中一定存在相邻的 `()`、`[]` 或 `{}`。反复删除这些成对括号，如果最后变成空串就是合法的。

```java
class Solution {
    public boolean isValid(String s) {
        int prevLength;
        do {
            prevLength = s.length();
            s = s.replace("()", "")
                    .replace("[]", "")
                    .replace("{}", "");
        } while (s.length() != prevLength);

        return s.isEmpty();
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(n)`。

#### 资深解法：栈

算法思想：遇到左括号就把对应右括号压栈；遇到右括号时，必须和栈顶相同，否则无效。最后栈为空才表示全部匹配。

```java
import java.util.ArrayDeque;
import java.util.Deque;

class Solution {
    public boolean isValid(String s) {
        Deque<Character> stack = new ArrayDeque<>();

        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (c == '(') {
                stack.push(')');
            } else if (c == '[') {
                stack.push(']');
            } else if (c == '{') {
                stack.push('}');
            } else {
                if (stack.isEmpty() || stack.pop() != c) {
                    return false;
                }
            }
        }

        return stack.isEmpty();
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 基础语法与算法思想

- `String.replace(old, new)`：替换字符串中的指定片段。
- `Deque<Character>`：双端队列，常用作栈。
- `push` / `pop`：入栈和出栈。
- 核心思想：括号匹配遵循“后打开的先关闭”，这正是栈的后进先出。

---

## 21. 合并两个有序链表 (Easy)

将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 
 
示例 1：

```text
输入：l1 = [1,2,4], l2 = [1,3,4]
输出：[1,1,2,3,4,4]
```

示例 2：

```text
输入：l1 = [], l2 = []
输出：[]
```

示例 3：

```text
输入：l1 = [], l2 = [0]
输出：[0]
```

 
提示：

两个链表的节点数目范围是  `[0, 50]` 
 `-100 <= Node.val <= 100` 
 `l1`  和  `l2`  均按 非递减顺序 排列

### Java 解法补充

#### 基础解法：递归合并

算法思想：两个链表都已升序，每次选择较小头结点作为当前节点，剩余部分继续递归合并。

```java
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        if (list1 == null) return list2;
        if (list2 == null) return list1;
        if (list1.val <= list2.val) {
            list1.next = mergeTwoLists(list1.next, list2);
            return list1;
        }
        list2.next = mergeTwoLists(list1, list2.next);
        return list2;
    }
}
```

复杂度：时间 `O(m + n)`，空间 `O(m + n)`，递归栈消耗。

#### 资深解法：迭代加虚拟头结点

算法思想：用 `tail` 指针维护结果链表尾部，每次接上两个链表中较小的当前节点，最后把剩余链表整体接上。

```java
class Solution {
    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        ListNode dummy = new ListNode(0);
        ListNode tail = dummy;

        while (list1 != null && list2 != null) {
            if (list1.val <= list2.val) {
                tail.next = list1;
                list1 = list1.next;
            } else {
                tail.next = list2;
                list2 = list2.next;
            }
            tail = tail.next;
        }

        tail.next = list1 == null ? list2 : list1;
        return dummy.next;
    }
}
```

复杂度：时间 `O(m + n)`，空间 `O(1)`。

#### 基础语法与算法思想

- `return list2`：递归边界，空链表与另一个链表合并就是另一个链表。
- `tail.next = ...`：把节点接到结果链表尾部。
- 核心思想：有序链表合并像归并排序的合并步骤。

---

## 22. 括号生成 (Medium)

数字  `n`  代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 有效的 括号组合。
 
示例 1：

```text
输入：n = 3
输出：["((()))","(()())","(())()","()(())","()()()"]
```

示例 2：

```text
输入：n = 1
输出：["()"]
```

 
提示：

 `1 <= n <= 8`

### Java 解法补充

#### 基础解法：枚举所有括号串并校验

算法思想：长度为 `2n` 的每个位置都可以放左括号或右括号，生成后用计数器判断是否合法。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> ans = new ArrayList<>();
        build(new StringBuilder(), 2 * n, ans);
        return ans;
    }

    private void build(StringBuilder path, int len, List<String> ans) {
        if (path.length() == len) {
            if (valid(path)) ans.add(path.toString());
            return;
        }
        path.append('(');
        build(path, len, ans);
        path.deleteCharAt(path.length() - 1);
        path.append(')');
        build(path, len, ans);
        path.deleteCharAt(path.length() - 1);
    }

    private boolean valid(StringBuilder s) {
        int balance = 0;
        for (int i = 0; i < s.length(); i++) {
            balance += s.charAt(i) == '(' ? 1 : -1;
            if (balance < 0) return false;
        }
        return balance == 0;
    }
}
```

复杂度：时间 `O(2^(2n) * n)`，空间 `O(n)`，不计答案空间。

#### 资深解法：回溯剪枝

算法思想：只生成合法前缀。左括号数量小于 `n` 时可以继续放左括号；右括号数量小于左括号数量时才可以放右括号。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> ans = new ArrayList<>();
        backtrack(n, 0, 0, new StringBuilder(), ans);
        return ans;
    }

    private void backtrack(int n, int open, int close, StringBuilder path, List<String> ans) {
        if (path.length() == 2 * n) {
            ans.add(path.toString());
            return;
        }
        if (open < n) {
            path.append('(');
            backtrack(n, open + 1, close, path, ans);
            path.deleteCharAt(path.length() - 1);
        }
        if (close < open) {
            path.append(')');
            backtrack(n, open, close + 1, path, ans);
            path.deleteCharAt(path.length() - 1);
        }
    }
}
```

复杂度：时间与合法括号数量成正比，空间 `O(n)`。

#### 基础语法与算法思想

- `StringBuilder` 用作可变路径，递归后要撤销选择。
- `balance` 表示当前未闭合的左括号数量。
- 核心思想：回溯的剪枝条件来自合法括号前缀的不变量。

---

## 23. 合并 K 个升序链表 (Hard)

给你一个链表数组，每个链表都已经按升序排列。
请你将所有链表合并到一个升序链表中，返回合并后的链表。
 
示例 1：

```text
输入：lists = [[1,4,5],[1,3,4],[2,6]]
输出：[1,1,2,3,4,4,5,6]
解释：链表数组如下：
[
  1->4->5,
  1->3->4,
  2->6
]
将它们合并到一个有序链表中得到。
1->1->2->3->4->4->5->6
```

示例 2：

```text
输入：lists = []
输出：[]
```

示例 3：

```text
输入：lists = [[]]
输出：[]
```

 
提示：

 `k == lists.length` 
 `0 <= k <= 10^4` 
 `0 <= lists[i].length <= 500` 
 `-10^4 <= lists[i][j] <= 10^4` 
 `lists[i]`  按 升序 排列
 `lists[i].length`  的总和不超过  `10^4`

### Java 解法补充

#### 基础解法：顺序两两合并

算法思想：先拿一个空链表作为结果，再依次把每个链表合并进结果。复用两个有序链表合并函数。

```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        ListNode ans = null;
        for (ListNode list : lists) {
            ans = merge(ans, list);
        }
        return ans;
    }

    private ListNode merge(ListNode a, ListNode b) {
        ListNode dummy = new ListNode(0);
        ListNode tail = dummy;
        while (a != null && b != null) {
            if (a.val <= b.val) {
                tail.next = a;
                a = a.next;
            } else {
                tail.next = b;
                b = b.next;
            }
            tail = tail.next;
        }
        tail.next = a == null ? b : a;
        return dummy.next;
    }
}
```

复杂度：时间最坏 `O(kN)`，空间 `O(1)`，`N` 为总节点数。

#### 资深解法：优先队列

算法思想：把每条链表的头节点放入小根堆，每次取出当前最小节点接到结果尾部，并把它的下一个节点放入堆。

```java
import java.util.PriorityQueue;

class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        PriorityQueue<ListNode> heap = new PriorityQueue<>((a, b) -> a.val - b.val);
        for (ListNode node : lists) {
            if (node != null) heap.offer(node);
        }

        ListNode dummy = new ListNode(0);
        ListNode tail = dummy;
        while (!heap.isEmpty()) {
            ListNode node = heap.poll();
            tail.next = node;
            tail = tail.next;
            if (node.next != null) heap.offer(node.next);
        }
        return dummy.next;
    }
}
```

复杂度：时间 `O(N log k)`，空间 `O(k)`。

#### 基础语法与算法思想

- `PriorityQueue`：小根堆，适合动态获取最小元素。
- Lambda `(a, b) -> a.val - b.val`：自定义节点比较规则。
- 核心思想：多个有序流合并，用堆维护每个流的当前最小候选。

---

## 24. 两两交换链表中的节点 (Medium)

给你一个链表，两两交换其中相邻的节点，并返回交换后链表的头节点。你必须在不修改节点内部的值的情况下完成本题（即，只能进行节点交换）。
 
示例 1：

```text
输入：head = [1,2,3,4]
输出：[2,1,4,3]
```

示例 2：

```text
输入：head = []
输出：[]
```

示例 3：

```text
输入：head = [1]
输出：[1]
```

 
提示：

链表中节点的数目在范围  `[0, 100]`  内
 `0 <= Node.val <= 100`

### Java 解法补充

#### 基础解法：递归交换

算法思想：每次处理前两个节点，交换后把原第一个节点接上后续递归交换结果。

```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode second = head.next;
        head.next = swapPairs(second.next);
        second.next = head;
        return second;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`，递归栈消耗。

#### 资深解法：迭代原地交换

算法思想：用虚拟头结点和 `prev` 指向待交换两节点之前的位置，反复重连 `a`、`b` 两个节点。

```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode prev = dummy;

        while (prev.next != null && prev.next.next != null) {
            ListNode a = prev.next;
            ListNode b = a.next;
            a.next = b.next;
            b.next = a;
            prev.next = b;
            prev = a;
        }

        return dummy.next;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 改链表指针前先保存 `a`、`b`，避免断链。
- `prev` 始终指向下一组待交换节点的前一个节点。
- 核心思想：链表节点交换不能改值，只能重连 `next`。

---

## 25. K 个一组翻转链表 (Hard)

给你链表的头节点  `head`  ，每  `k`  个节点一组进行翻转，请你返回修改后的链表。
 `k`  是一个正整数，它的值小于或等于链表的长度。如果节点总数不是  `k`  的整数倍，那么请将最后剩余的节点保持原有顺序。
你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。
 
示例 1：

```text
输入：head = [1,2,3,4,5], k = 2
输出：[2,1,4,3,5]
```

示例 2：

```text
输入：head = [1,2,3,4,5], k = 3
输出：[3,2,1,4,5]
```

 
提示：

链表中的节点数目为  `n` 
 `1 <= k <= n <= 5000` 
 `0 <= Node.val <= 1000` 

 
进阶：你可以设计一个只用  `O(1)`  额外内存空间的算法解决此问题吗？

### Java 解法补充

#### 基础解法：用数组暂存节点值

算法思想：先把节点值读入数组，每 `k` 个一组反转数组中的值，再写回链表。这个写法容易理解，但题目进阶要求实际交换节点，面试中应继续掌握资深解法。

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        List<Integer> values = new ArrayList<>();
        for (ListNode cur = head; cur != null; cur = cur.next) {
            values.add(cur.val);
        }
        for (int i = 0; i + k <= values.size(); i += k) {
            Collections.reverse(values.subList(i, i + k));
        }
        ListNode cur = head;
        for (int value : values) {
            cur.val = value;
            cur = cur.next;
        }
        return head;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 资深解法：原地分组反转节点

算法思想：每次检查后面是否有 `k` 个节点；若有，就反转 `[groupStart, groupEnd]` 这一段，再把前后链表接回去。

```java
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode groupPrev = dummy;

        while (true) {
            ListNode kth = getKth(groupPrev, k);
            if (kth == null) break;
            ListNode groupNext = kth.next;

            ListNode prev = groupNext;
            ListNode cur = groupPrev.next;
            while (cur != groupNext) {
                ListNode next = cur.next;
                cur.next = prev;
                prev = cur;
                cur = next;
            }

            ListNode newTail = groupPrev.next;
            groupPrev.next = kth;
            groupPrev = newTail;
        }

        return dummy.next;
    }

    private ListNode getKth(ListNode cur, int k) {
        while (cur != null && k > 0) {
            cur = cur.next;
            k--;
        }
        return cur;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- `Collections.reverse(list.subList(l, r))`：反转列表的左闭右开子视图。
- 原地反转链表使用 `prev / cur / next` 三指针。
- 核心思想：复杂链表题先划清“组前、组头、组尾、组后”四个位置。

---

## 26. 删除有序数组中的重复项 (Easy)

给你一个 非严格递增排列 的数组  `nums`  ，请你 原地 删除重复出现的元素，使每个元素 只出现一次 ，返回删除后数组的新长度。元素的 相对顺序 应该保持 一致 。然后返回  `nums`  中唯一元素的个数。
考虑  `nums`  的唯一元素的数量为  `k` 。去重后，返回唯一元素的数量  `k` 。
 `nums`  的前  `k`  个元素应包含 排序后 的唯一数字。下标  `k - 1`  之后的剩余元素可以忽略。
判题标准:
系统会用下面的代码来测试你的题解:

```text
int[] nums = [...]; // 输入数组
int[] expectedNums = [...]; // 长度正确的期望答案

int k = removeDuplicates(nums); // 调用

assert k == expectedNums.length;
for (int i = 0; i < k; i++) {
    assert nums[i] == expectedNums[i];
}
```

如果所有断言都通过，那么您的题解将被 通过。
 
示例 1：

```text
输入：nums = [1,1,2]
输出：2, nums = [1,2,_]
解释：函数应该返回新的长度 2 ，并且原数组 nums 的前两个元素被修改为 1, 2 。不需要考虑数组中超出新长度后面的元素。
```

示例 2：

```text
输入：nums = [0,0,1,1,1,2,2,3,3,4]
输出：5, nums = [0,1,2,3,4,_,_,_,_,_]
解释：函数应该返回新的长度 5 ， 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4 。不需要考虑数组中超出新长度后面的元素。
```

 
提示：

 `1 <= nums.length <= 3 * 104` 
 `-100 <= nums[i] <= 100` 
 `nums`  已按 非递减 顺序排列。

### Java 解法补充

#### 基础解法：借助列表收集唯一值

算法思想：遍历有序数组，遇到第一个元素或与前一个元素不同的元素就加入列表，最后写回数组前部。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public int removeDuplicates(int[] nums) {
        List<Integer> unique = new ArrayList<>();
        for (int i = 0; i < nums.length; i++) {
            if (i == 0 || nums[i] != nums[i - 1]) {
                unique.add(nums[i]);
            }
        }
        for (int i = 0; i < unique.size(); i++) {
            nums[i] = unique.get(i);
        }
        return unique.size();
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 资深解法：快慢指针原地去重

算法思想：`slow` 指向下一个唯一元素应写入的位置，`fast` 扫描数组。只要 `nums[fast]` 与前一个不同，就写到 `slow`。

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int slow = 1;
        for (int fast = 1; fast < nums.length; fast++) {
            if (nums[fast] != nums[fast - 1]) {
                nums[slow] = nums[fast];
                slow++;
            }
        }
        return slow;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 有序数组中重复元素一定相邻。
- 原地数组题常用 `slow` 表示有效区间长度。
- 核心思想：快指针负责读，慢指针负责写。

---

## 27. 移除元素 (Easy)

给你一个数组  `nums`  和一个值  `val` ，你需要 原地 移除所有数值等于  `val`  的元素。元素的顺序可能发生改变。然后返回  `nums`  中与  `val`  不同的元素的数量。
假设  `nums`  中不等于  `val`  的元素数量为  `k` ，要通过此题，您需要执行以下操作：

更改  `nums`  数组，使  `nums`  的前  `k`  个元素包含不等于  `val`  的元素。 `nums`  的其余元素和  `nums`  的大小并不重要。
返回  `k` 。

用户评测：
评测机将使用以下代码测试您的解决方案：

```text
int[] nums = [...]; // 输入数组
int val = ...; // 要移除的值
int[] expectedNums = [...]; // 长度正确的预期答案。
                            // 它以不等于 val 的值排序。

int k = removeElement(nums, val); // 调用你的实现

assert k == expectedNums.length;
sort(nums, 0, k); // 排序 nums 的前 k 个元素
for (int i = 0; i < actualLength; i++) {
    assert nums[i] == expectedNums[i];
}
```

如果所有的断言都通过，你的解决方案将会 通过。
 
示例 1：

```text
输入：nums = [3,2,2,3], val = 3
输出：2, nums = [2,2,_,_]
解释：你的函数应该返回 k = 2, 并且 nums 中的前两个元素均为 2。
你在返回的 k 个元素之外留下了什么并不重要（因此它们并不计入评测）。
```

示例 2：

```text
输入：nums = [0,1,2,2,3,0,4,2], val = 2
输出：5, nums = [0,1,4,0,3,_,_,_]
解释：你的函数应该返回 k = 5，并且 nums 中的前五个元素为 0,0,1,3,4。
注意这五个元素可以任意顺序返回。
你在返回的 k 个元素之外留下了什么并不重要（因此它们并不计入评测）。
```

 
提示：

 `0 <= nums.length <= 100` 
 `0 <= nums[i] <= 50` 
 `0 <= val <= 100`

### Java 解法补充

#### 基础解法：借助新数组过滤

算法思想：把不等于 `val` 的元素复制到临时数组，再写回 `nums` 前部。

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int[] temp = new int[nums.length];
        int k = 0;
        for (int num : nums) {
            if (num != val) {
                temp[k++] = num;
            }
        }
        for (int i = 0; i < k; i++) {
            nums[i] = temp[i];
        }
        return k;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 资深解法：原地快慢指针

算法思想：`fast` 扫描所有元素，遇到不等于 `val` 的元素就写到 `slow` 位置，最后 `slow` 就是保留元素数量。

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int slow = 0;
        for (int fast = 0; fast < nums.length; fast++) {
            if (nums[fast] != val) {
                nums[slow] = nums[fast];
                slow++;
            }
        }
        return slow;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 增强 `for (int num : nums)` 适合只读遍历数组。
- 原地覆盖时，后面未使用的元素无需清理。
- 核心思想：保留类数组题的答案通常是“新长度 + 前缀有效”。

---

## 28. 找出字符串中第一个匹配项的下标 (Easy)

给你两个字符串  `haystack`  和  `needle`  ，请你在  `haystack`  字符串中找出  `needle`  字符串的第一个匹配项的下标（下标从 0 开始）。如果  `needle`  不是  `haystack`  的一部分，则返回   `-1`  。
 
示例 1：

```text
输入：haystack = "sadbutsad", needle = "sad"
输出：0
解释："sad" 在下标 0 和 6 处匹配。
第一个匹配项的下标是 0 ，所以返回 0 。
```

示例 2：

```text
输入：haystack = "leetcode", needle = "leeto"
输出：-1
解释："leeto" 没有在 "leetcode" 中出现，所以返回 -1 。
```

 
提示：

 `1 <= haystack.length, needle.length <= 104` 
 `haystack`  和  `needle`  仅由小写英文字符组成

### Java 解法补充

#### 基础解法：枚举起点逐字符匹配

算法思想：枚举 `haystack` 中每个可能起点，逐个字符与 `needle` 比较，全部相等就返回起点。

```java
class Solution {
    public int strStr(String haystack, String needle) {
        int n = haystack.length();
        int m = needle.length();
        for (int i = 0; i + m <= n; i++) {
            int j = 0;
            while (j < m && haystack.charAt(i + j) == needle.charAt(j)) {
                j++;
            }
            if (j == m) return i;
        }
        return -1;
    }
}
```

复杂度：时间 `O(nm)`，空间 `O(1)`。

#### 资深解法：KMP

算法思想：预处理 `needle` 的最长相等前后缀数组 `lps`。匹配失败时不用回退主串下标，只把模式串下标跳到可复用前缀位置。

```java
class Solution {
    public int strStr(String haystack, String needle) {
        int[] lps = buildLps(needle);
        int j = 0;
        for (int i = 0; i < haystack.length(); i++) {
            while (j > 0 && haystack.charAt(i) != needle.charAt(j)) {
                j = lps[j - 1];
            }
            if (haystack.charAt(i) == needle.charAt(j)) {
                j++;
            }
            if (j == needle.length()) {
                return i - needle.length() + 1;
            }
        }
        return -1;
    }

    private int[] buildLps(String p) {
        int[] lps = new int[p.length()];
        int j = 0;
        for (int i = 1; i < p.length(); i++) {
            while (j > 0 && p.charAt(i) != p.charAt(j)) {
                j = lps[j - 1];
            }
            if (p.charAt(i) == p.charAt(j)) {
                lps[i] = ++j;
            }
        }
        return lps;
    }
}
```

复杂度：时间 `O(n + m)`，空间 `O(m)`。

#### 基础语法与算法思想

- `while` 中先判断边界，再访问字符。
- `lps[i]` 表示模式串 `0..i` 的最长相等真前后缀长度。
- 核心思想：KMP 用模式串自身结构减少重复比较。

---

## 29. 两数相除 (Medium)

给你两个整数，被除数  `dividend`  和除数  `divisor` 。将两数相除，要求 不使用 乘法、除法和取余运算。
整数除法应该向零截断，也就是截去（ `truncate` ）其小数部分。例如， `8.345`  将被截断为  `8`  ， `-2.7335`  将被截断至  `-2`  。
返回被除数  `dividend`  除以除数  `divisor`  得到的 商 。
注意：假设我们的环境只能存储 32 位 有符号整数，其数值范围是  `[−231,  231 − 1]`  。本题中，如果商 严格大于  `231 − 1`  ，则返回  `231 − 1`  ；如果商 严格小于  `-231`  ，则返回  `-231`  。
 
示例 1:

```text
输入: dividend = 10, divisor = 3
输出: 3
解释: 10/3 = 3.33333.. ，向零截断后得到 3 。
```

示例 2:

```text
输入: dividend = 7, divisor = -3
输出: -2
解释: 7/-3 = -2.33333.. ，向零截断后得到 -2 。
```

 
提示：

 `-231 <= dividend, divisor <= 231 - 1` 
 `divisor != 0`

### Java 解法补充

#### 基础解法：循环减法

算法思想：把被除数和除数转为正的 `long`，不断减去除数并计数。这个方法直观但大数据会超时，只适合理解除法含义。

```java
class Solution {
    public int divide(int dividend, int divisor) {
        if (dividend == Integer.MIN_VALUE && divisor == -1) {
            return Integer.MAX_VALUE;
        }
        boolean negative = (dividend < 0) ^ (divisor < 0);
        long a = Math.abs((long) dividend);
        long b = Math.abs((long) divisor);
        long ans = 0;
        while (a >= b) {
            a -= b;
            ans++;
        }
        return negative ? (int) -ans : (int) ans;
    }
}
```

复杂度：时间 `O(|quotient|)`，空间 `O(1)`。

#### 资深解法：倍增减法

算法思想：每次把除数不断翻倍，找到不超过当前被除数的最大倍数，一次减掉大块，并把对应倍数累加到答案。

```java
class Solution {
    public int divide(int dividend, int divisor) {
        if (dividend == Integer.MIN_VALUE && divisor == -1) {
            return Integer.MAX_VALUE;
        }
        boolean negative = (dividend < 0) ^ (divisor < 0);
        long a = Math.abs((long) dividend);
        long b = Math.abs((long) divisor);
        long ans = 0;

        while (a >= b) {
            long value = b;
            long count = 1;
            while ((value << 1) <= a) {
                value <<= 1;
                count <<= 1;
            }
            a -= value;
            ans += count;
        }

        return negative ? (int) -ans : (int) ans;
    }
}
```

复杂度：时间 `O(log^2 |dividend|)`，空间 `O(1)`。

#### 基础语法与算法思想

- `^`：布尔异或，判断结果符号是否为负。
- `(long) dividend`：先转 `long` 再取绝对值，避免 `Integer.MIN_VALUE` 溢出。
- `<<= 1`：左移一位，相当于乘 2。
- 核心思想：不能用乘除时，用位移倍增模拟快速除法。

---

## 30. 串联所有单词的子串 (Hard)

给定一个字符串  `s`  和一个字符串数组  `words` 。  `words`  中所有字符串 长度相同。
  `s`  中的 串联子串 是指一个包含   `words`  中所有字符串以任意顺序排列连接起来的子串。

例如，如果  `words = ["ab","cd","ef"]` ， 那么  `"abcdef"` ，  `"abefcd"` ， `"cdabef"` ，  `"cdefab"` ， `"efabcd"` ， 和  `"efcdab"`  都是串联子串。  `"acdbef"`  不是串联子串，因为他不是任何  `words`  排列的连接。

返回所有串联子串在  `s`  中的开始索引。你可以以 任意顺序 返回答案。
 
示例 1：

```text
输入：s = "barfoothefoobarman", words = ["foo","bar"]
输出：[0,9]
解释：因为 words.length == 2 同时 words[i].length == 3，连接的子字符串的长度必须为 6。
子串 "barfoo" 开始位置是 0。它是 words 中以 ["bar","foo"] 顺序排列的连接。
子串 "foobar" 开始位置是 9。它是 words 中以 ["foo","bar"] 顺序排列的连接。
输出顺序无关紧要。返回 [9,0] 也是可以的。
```

示例 2：

```text
输入：s = "wordgoodgoodgoodbestword", words = ["word","good","best","word"]
输出：[]
解释：因为 words.length == 4 并且 words[i].length == 4，所以串联子串的长度必须为 16。
s 中没有子串长度为 16 并且等于 words 的任何顺序排列的连接。
所以我们返回一个空数组。
```

示例 3：

```text
输入：s = "barfoofoobarthefoobarman", words = ["bar","foo","the"]
输出：[6,9,12]
解释：因为 words.length == 3 并且 words[i].length == 3，所以串联子串的长度必须为 9。
子串 "foobarthe" 开始位置是 6。它是 words 中以 ["foo","bar","the"] 顺序排列的连接。
子串 "barthefoo" 开始位置是 9。它是 words 中以 ["bar","the","foo"] 顺序排列的连接。
子串 "thefoobar" 开始位置是 12。它是 words 中以 ["the","foo","bar"] 顺序排列的连接。
```

 
提示：

 `1 <= s.length <= 104` 
 `1 <= words.length <= 5000` 
 `1 <= words[i].length <= 30` 
 `words[i]`  和  `s`  由小写英文字母组成

### Java 解法补充

#### 基础解法：枚举起点并统计单词

算法思想：每个答案子串长度固定为 `words.length * wordLen`。枚举起点，把子串按单词长度切块，统计是否与 `words` 的频次一致。

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

class Solution {
    public List<Integer> findSubstring(String s, String[] words) {
        List<Integer> ans = new ArrayList<>();
        Map<String, Integer> need = new HashMap<>();
        for (String word : words) {
            need.put(word, need.getOrDefault(word, 0) + 1);
        }

        int wordLen = words[0].length();
        int totalLen = wordLen * words.length;
        for (int i = 0; i + totalLen <= s.length(); i++) {
            Map<String, Integer> seen = new HashMap<>();
            int count = 0;
            while (count < words.length) {
                String word = s.substring(i + count * wordLen, i + (count + 1) * wordLen);
                if (!need.containsKey(word)) break;
                seen.put(word, seen.getOrDefault(word, 0) + 1);
                if (seen.get(word) > need.get(word)) break;
                count++;
            }
            if (count == words.length) ans.add(i);
        }
        return ans;
    }
}
```

复杂度：时间 `O(n * words.length * wordLen)`，空间 `O(words.length)`。

#### 资深解法：按单词长度分组滑动窗口

算法思想：按起点模 `wordLen` 分组扫描，窗口每次移动一个单词长度。窗口内某个单词频次超标时，从左侧弹出单词直到合法。

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

class Solution {
    public List<Integer> findSubstring(String s, String[] words) {
        List<Integer> ans = new ArrayList<>();
        Map<String, Integer> need = new HashMap<>();
        for (String word : words) {
            need.put(word, need.getOrDefault(word, 0) + 1);
        }

        int wordLen = words[0].length();
        int wordCount = words.length;
        for (int offset = 0; offset < wordLen; offset++) {
            Map<String, Integer> window = new HashMap<>();
            int left = offset;
            int count = 0;

            for (int right = offset; right + wordLen <= s.length(); right += wordLen) {
                String word = s.substring(right, right + wordLen);
                if (!need.containsKey(word)) {
                    window.clear();
                    count = 0;
                    left = right + wordLen;
                    continue;
                }

                window.put(word, window.getOrDefault(word, 0) + 1);
                count++;
                while (window.get(word) > need.get(word)) {
                    String removed = s.substring(left, left + wordLen);
                    window.put(removed, window.get(removed) - 1);
                    left += wordLen;
                    count--;
                }
                if (count == wordCount) {
                    ans.add(left);
                }
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(n * wordLen)` 级别，空间 `O(words.length)`。

#### 基础语法与算法思想

- `getOrDefault`：频次统计的常用 API。
- `substring(l, r)`：切出固定长度单词。
- 核心思想：固定词长后，字符串窗口可以按“单词”为单位滑动。

---
