# LeetCode 题目合集 Part 17

## 481. 神奇字符串 (Medium)

神奇字符串  `s`  仅由  `'1'`  和  `'2'`  组成，并需要遵守下面的规则：

将连续相同字符组  `'1'`  和  `'2'`  长度的序列连接起来会生成字符串  `s`  本身。

 `s`  的前几个元素是  `s = "1221121221221121122……"`  。如果将  `s`  中连续的若干  `1`  和  `2`  进行分组，可以得到  `"1 22 11 2 1 22 1 22 11 2 11 22 ......"`  。每组中  `1`  或者  `2`  的出现次数分别是  `"1 2 2 1 1 2 1 2 2 1 2 2 ......"`  。上面的出现次数正是  `s`  自身。
给你一个整数  `n`  ，返回在神奇字符串  `s`  的前  `n`  个数字中  `1`  的数目。
 
 **示例 1：** 

```text
输入：n = 6
输出：3
解释：神奇字符串 s 的前 6 个元素是 “122112”，它包含三个 1，因此返回 3 。
```

 **示例 2：** 

```text
输入：n = 1
输出：1
```

 
 **提示：** 

 `1 <= n <= 105`

### Java 解法补充

#### 基础解法：按定义生成字符串

算法思想：神奇字符串前缀为 `"122"`。用 `head` 指向当前要读取的组长度，用 `next` 表示下一组要填的字符，按定义不断追加，直到长度达到 `n` 后统计前 `n` 个字符中的 `1`。

```java
class Solution {
    public int magicalString(int n) {
        if (n <= 3) {
            return 1;
        }

        StringBuilder s = new StringBuilder("122");
        int head = 2;
        char next = '1';

        while (s.length() < n) {
            int count = s.charAt(head) - '0';
            for (int i = 0; i < count; i++) {
                s.append(next);
            }
            next = next == '1' ? '2' : '1';
            head++;
        }

        int ans = 0;
        for (int i = 0; i < n; i++) {
            if (s.charAt(i) == '1') {
                ans++;
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 资深解法：数组生成时同步计数

算法思想：用整型数组代替字符串，`head` 读组长度，`tail` 写新字符。写入时如果字符是 1，就同步更新答案，避免最后再扫一遍。

```java
class Solution {
    public int magicalString(int n) {
        if (n <= 0) return 0;
        if (n <= 3) return 1;

        int[] s = new int[n + 2];
        s[0] = 1;
        s[1] = 2;
        s[2] = 2;

        int head = 2;
        int tail = 3;
        int num = 1;
        int ans = 1;

        while (tail < n) {
            int count = s[head++];
            for (int i = 0; i < count && tail < n; i++) {
                s[tail++] = num;
                if (num == 1) {
                    ans++;
                }
            }
            num = 3 - num;
        }

        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 基础语法与算法思想

- `StringBuilder` 适合逐步追加字符。
- `num = 3 - num` 可以在 1 和 2 之间切换。
- 核心思想：本题是按“已有内容描述后续生成规则”的自生成序列。

---

## 482. 密钥格式化 (Easy)

给定一个许可密钥字符串  `s` ，仅由字母、数字字符和破折号组成。字符串由  `n`  个破折号分成  `n + 1`  组。你也会得到一个整数  `k`  。
我们想要重新格式化字符串  `s` ，使每一组包含  `k`  个字符，除了第一组，它可以比  `k`  短，但仍然必须包含至少一个字符。此外，两组之间必须插入破折号，并且应该将所有小写字母转换为大写字母。
返回 重新格式化的许可密钥 。
 
 **示例 1：** 

```text
输入：S = "5F3Z-2e-9-w", k = 4
输出："5F3Z-2E9W"
解释：字符串 S 被分成了两个部分，每部分 4 个字符；
     注意，两个额外的破折号需要删掉。
```

 **示例 2：** 

```text
输入：S = "2-5g-3-J", k = 2
输出："2-5G-3J"
解释：字符串 S 被分成了 3 个部分，按照前面的规则描述，第一部分的字符可以少于给定的数量，其余部分皆为 2 个字符。
```

 
 **提示:** 

 `1 <= s.length <= 105` 
 `s`  只包含字母、数字和破折号  `'-'` .
 `1 <= k <= 104`

### Java 解法补充

#### 基础解法：先清理再分组

算法思想：先删除所有破折号并转大写，再计算第一组长度。第一组可以较短，其余每组固定 `k` 个字符。

```java
class Solution {
    public String licenseKeyFormatting(String s, int k) {
        StringBuilder clean = new StringBuilder();
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (c != '-') {
                clean.append(Character.toUpperCase(c));
            }
        }

        if (clean.length() == 0) {
            return "";
        }

        int first = clean.length() % k;
        if (first == 0) {
            first = k;
        }

        StringBuilder ans = new StringBuilder();
        ans.append(clean.substring(0, first));
        for (int i = first; i < clean.length(); i += k) {
            ans.append('-');
            ans.append(clean.substring(i, i + k));
        }
        return ans.toString();
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 资深解法：从右向左构建后反转

算法思想：从后向前遍历原字符串，每收集 `k` 个有效字符就插入一个破折号。这样天然保证除第一组外每组都是 `k` 个字符，最后反转即可。

```java
class Solution {
    public String licenseKeyFormatting(String s, int k) {
        StringBuilder ans = new StringBuilder();
        int count = 0;

        for (int i = s.length() - 1; i >= 0; i--) {
            char c = s.charAt(i);
            if (c == '-') {
                continue;
            }
            if (count == k) {
                ans.append('-');
                count = 0;
            }
            ans.append(Character.toUpperCase(c));
            count++;
        }

        return ans.reverse().toString();
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 基础语法与算法思想

- `Character.toUpperCase(c)` 用于大小写转换。
- 从右往左处理分组，能避免单独计算第一组长度。
- 核心思想：固定尾部分组的问题，反向构造通常更简单。

---

## 483. 最小好进制 (Hard)

以字符串的形式给出  `n`  , 以字符串的形式返回  `n`  的最小  **好进制**   。
如果  `n`  的   `k(k>=2)`  进制数的所有数位全为1，则称  `k(k>=2)`  是  `n`  的一个  **好进制** 。
 
 **示例 1：** 

```text
输入：n = "13"
输出："3"
解释：13 的 3 进制是 111。
```

 **示例 2：** 

```text
输入：n = "4681"
输出："8"
解释：4681 的 8 进制是 11111。
```

 **示例 3：** 

```text
输入：n = "1000000000000000000"
输出："999999999999999999"
解释：1000000000000000000 的 999999999999999999 进制是 11。
```

 
 **提示：** 

 `n`  的取值范围是  `[3, 1018]` 
 `n`  没有前导 0

### Java 解法补充

#### 基础解法：BigInteger 枚举位数和进制

算法思想：如果 `n` 在 `k` 进制下是若干个 1，那么 `n = 1 + k + k^2 + ... + k^(len-1)`。从较长位数开始尝试，找到的第一个进制最小。用 `BigInteger` 避免溢出。

```java
import java.math.BigInteger;

class Solution {
    public String smallestGoodBase(String n) {
        BigInteger target = new BigInteger(n);
        int maxLen = target.bitLength();

        for (int len = maxLen; len >= 2; len--) {
            BigInteger low = BigInteger.valueOf(2);
            BigInteger high = target.subtract(BigInteger.ONE);

            while (low.compareTo(high) <= 0) {
                BigInteger mid = low.add(high).shiftRight(1);
                BigInteger sum = sumOfOnes(mid, len);
                int cmp = sum.compareTo(target);
                if (cmp == 0) {
                    return mid.toString();
                } else if (cmp < 0) {
                    low = mid.add(BigInteger.ONE);
                } else {
                    high = mid.subtract(BigInteger.ONE);
                }
            }
        }

        return target.subtract(BigInteger.ONE).toString();
    }

    private BigInteger sumOfOnes(BigInteger base, int len) {
        BigInteger sum = BigInteger.ZERO;
        BigInteger cur = BigInteger.ONE;
        for (int i = 0; i < len; i++) {
            sum = sum.add(cur);
            cur = cur.multiply(base);
        }
        return sum;
    }
}
```

复杂度：时间 `O(log^3 n)` 级别，空间 `O(log n)`。

#### 资深解法：long 二分和溢出安全求和

算法思想：`n <= 10^18` 可以放入 `long`。对每个位数二分进制，并在计算等比和时一旦会超过目标就提前返回，避免乘法溢出。

```java
class Solution {
    public String smallestGoodBase(String n) {
        long target = Long.parseLong(n);
        int maxLen = 64 - Long.numberOfLeadingZeros(target);

        for (int len = maxLen; len >= 2; len--) {
            long low = 2;
            long high = (long) Math.pow(target, 1.0 / (len - 1)) + 1;

            while (low <= high) {
                long mid = low + (high - low) / 2;
                int cmp = compareSum(mid, len, target);
                if (cmp == 0) {
                    return Long.toString(mid);
                } else if (cmp < 0) {
                    low = mid + 1;
                } else {
                    high = mid - 1;
                }
            }
        }

        return Long.toString(target - 1);
    }

    private int compareSum(long base, int len, long target) {
        long sum = 1;
        long cur = 1;

        for (int i = 1; i < len; i++) {
            if (cur > (target - sum) / base) {
                return 1;
            }
            cur *= base;
            sum += cur;
        }

        return Long.compare(sum, target);
    }
}
```

复杂度：时间 `O(log^2 n)` 级别，空间 `O(1)`。

#### 基础语法与算法思想

- 全 1 进制表示等价于等比数列求和。
- 位数越多，满足条件的进制越小，因此从长位数开始找。
- 核心思想：最小好进制不是转换字符串逐位试，而是反推等比数列的公比。

---

## 484. 寻找排列 (Medium)

暂无内容描述。

### Java 解法补充

#### 基础解法：遇到连续 D 就反转区间

算法思想：先构造升序排列 `1..n+1`。模式串中的 `D` 表示当前位置要下降，连续的一段 `D` 可以通过反转对应区间一次满足。

```java
class Solution {
    public int[] findPermutation(String s) {
        int n = s.length();
        int[] ans = new int[n + 1];
        for (int i = 0; i <= n; i++) {
            ans[i] = i + 1;
        }

        int i = 0;
        while (i < n) {
            if (s.charAt(i) == 'I') {
                i++;
                continue;
            }
            int start = i;
            while (i < n && s.charAt(i) == 'D') {
                i++;
            }
            reverse(ans, start, i);
        }

        return ans;
    }

    private void reverse(int[] nums, int left, int right) {
        while (left < right) {
            int temp = nums[left];
            nums[left++] = nums[right];
            nums[right--] = temp;
        }
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`，不计返回数组。

#### 资深解法：栈延迟输出

算法思想：从 `1` 到 `n + 1` 依次入栈。遇到 `I` 或到达末尾时，把栈中元素全部弹出，连续 `D` 段会自然逆序输出。

```java
import java.util.ArrayDeque;
import java.util.Deque;

class Solution {
    public int[] findPermutation(String s) {
        int[] ans = new int[s.length() + 1];
        Deque<Integer> stack = new ArrayDeque<>();
        int index = 0;

        for (int i = 0; i <= s.length(); i++) {
            stack.push(i + 1);
            if (i == s.length() || s.charAt(i) == 'I') {
                while (!stack.isEmpty()) {
                    ans[index++] = stack.pop();
                }
            }
        }

        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 基础语法与算法思想

- `D` 的连续段需要下降，反转升序区间即可得到最小字典序排列。
- 栈的后进先出可以自然制造局部逆序。
- 核心思想：排列题先从最小升序数组出发，再按约束做最小改动。

---

## 485. 最大连续 1 的个数 (Easy)

给定一个二进制数组  `nums`  ， 计算其中最大连续  `1`  的个数。
 
 **示例 1：** 

```text
输入：nums = [1,1,0,1,1,1]
输出：3
解释：开头的两位和最后的三位都是连续 1 ，所以最大连续 1 的个数是 3.
```

 **示例 2:** 

```text
输入：nums = [1,0,1,1,0,1]
输出：2
```

 
 **提示：** 

 `1 <= nums.length <= 105` 
 `nums[i]`  不是  `0`  就是  `1` .

### Java 解法补充

#### 基础解法：枚举每段连续 1

算法思想：从左到右扫描，遇到 `1` 就继续向右数这一段有多长，遇到 `0` 则跳过。每段结束后更新最大长度。

```java
class Solution {
    public int findMaxConsecutiveOnes(int[] nums) {
        int ans = 0;
        int i = 0;

        while (i < nums.length) {
            if (nums[i] == 0) {
                i++;
                continue;
            }
            int count = 0;
            while (i < nums.length && nums[i] == 1) {
                count++;
                i++;
            }
            ans = Math.max(ans, count);
        }

        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 资深解法：滚动计数

算法思想：维护当前连续 1 的长度 `cur`。读到 1 就加一，读到 0 就清零，同时更新最大值。

```java
class Solution {
    public int findMaxConsecutiveOnes(int[] nums) {
        int ans = 0;
        int cur = 0;

        for (int num : nums) {
            if (num == 1) {
                cur++;
                ans = Math.max(ans, cur);
            } else {
                cur = 0;
            }
        }

        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 增强 `for` 适合只读遍历数组。
- 连续段问题常用一个当前计数器和一个最大值。
- 核心思想：遇到不满足连续条件的元素，就重置当前状态。

---

## 486. 预测赢家 (Medium)

给你一个整数数组  `nums`  。玩家 1 和玩家 2 基于这个数组设计了一个游戏。
玩家 1 和玩家 2 轮流进行自己的回合，玩家 1 先手。开始时，两个玩家的初始分值都是  `0`  。每一回合，玩家从数组的任意一端取一个数字（即， `nums[0]`  或  `nums[nums.length - 1]` ），取到的数字将会从数组中移除（数组长度减  `1`  ）。玩家选中的数字将会加到他的得分上。当数组中没有剩余数字可取时，游戏结束。
如果玩家 1 能成为赢家，返回  `true`  。如果两个玩家得分相等，同样认为玩家 1 是游戏的赢家，也返回  `true`  。你可以假设每个玩家的玩法都会使他的分数最大化。
 
 **示例 1：** 

```text
输入：nums = [1,5,2]
输出：false
解释：一开始，玩家 1 可以从 1 和 2 中进行选择。
如果他选择 2（或者 1 ），那么玩家 2 可以从 1（或者 2 ）和 5 中进行选择。如果玩家 2 选择了 5 ，那么玩家 1 则只剩下 1（或者 2 ）可选。 
所以，玩家 1 的最终分数为 1 + 2 = 3，而玩家 2 为 5 。
因此，玩家 1 永远不会成为赢家，返回 false 。
```

 **示例 2：** 

```text
输入：nums = [1,5,233,7]
输出：true
解释：玩家 1 一开始选择 1 。然后玩家 2 必须从 5 和 7 中进行选择。无论玩家 2 选择了哪个，玩家 1 都可以选择 233 。
最终，玩家 1（234 分）比玩家 2（12 分）获得更多的分数，所以返回 true，表示玩家 1 可以成为赢家。
```

 
 **提示：** 

 `1 <= nums.length <= 20` 
 `0 <= nums[i] <= 107`

### Java 解法补充

#### 基础解法：递归模拟双方最优

算法思想：定义递归返回当前玩家在区间 `[left, right]` 内最多能比对手多拿多少分。当前玩家可以拿左端或右端，拿完之后角色交换，所以要减去对手在剩余区间能获得的优势。

```java
class Solution {
    public boolean predictTheWinner(int[] nums) {
        return scoreDiff(nums, 0, nums.length - 1) >= 0;
    }

    private int scoreDiff(int[] nums, int left, int right) {
        if (left == right) {
            return nums[left];
        }

        int takeLeft = nums[left] - scoreDiff(nums, left + 1, right);
        int takeRight = nums[right] - scoreDiff(nums, left, right - 1);
        return Math.max(takeLeft, takeRight);
    }
}
```

复杂度：时间 `O(2^n)`，空间 `O(n)`。

#### 资深解法：区间 DP

算法思想：`dp[left][right]` 表示当前玩家在这个区间能取得的最大分差。状态转移和递归一致，但自底向上计算，避免重复子问题。

```java
class Solution {
    public boolean predictTheWinner(int[] nums) {
        int n = nums.length;
        int[][] dp = new int[n][n];

        for (int i = 0; i < n; i++) {
            dp[i][i] = nums[i];
        }

        for (int len = 2; len <= n; len++) {
            for (int left = 0; left + len <= n; left++) {
                int right = left + len - 1;
                int takeLeft = nums[left] - dp[left + 1][right];
                int takeRight = nums[right] - dp[left][right - 1];
                dp[left][right] = Math.max(takeLeft, takeRight);
            }
        }

        return dp[0][n - 1] >= 0;
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(n^2)`。

#### 基础语法与算法思想

- “当前玩家分数 - 对手分数”可以把双人博弈简化成一个分差状态。
- 区间 DP 通常按长度从小到大填表。
- 核心思想：双方都最优时，当前选择要考虑下一回合对手也会最大化自己的优势。

---

## 487. 最大连续1的个数 II (Medium)

暂无内容描述。

### Java 解法补充

#### 基础解法：尝试翻转每个 0

算法思想：枚举每个位置作为被翻转的 0，然后向左、向右统计连续 1 的长度。如果当前位置本来就是 1，也可以把它作为不翻转时的连续段起点处理。

```java
class Solution {
    public int findMaxConsecutiveOnes(int[] nums) {
        int ans = 0;

        for (int i = 0; i < nums.length; i++) {
            int count = 0;
            int zeros = 0;
            for (int j = i; j < nums.length; j++) {
                if (nums[j] == 0) {
                    zeros++;
                }
                if (zeros > 1) {
                    break;
                }
                count++;
            }
            ans = Math.max(ans, count);
        }

        return ans;
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(1)`。

#### 资深解法：最多包含一个 0 的滑动窗口

算法思想：维护一个窗口，窗口内最多有一个 0。右边界持续扩张；当 0 的数量超过 1 时，移动左边界直到窗口重新合法。

```java
class Solution {
    public int findMaxConsecutiveOnes(int[] nums) {
        int left = 0;
        int zeros = 0;
        int ans = 0;

        for (int right = 0; right < nums.length; right++) {
            if (nums[right] == 0) {
                zeros++;
            }
            while (zeros > 1) {
                if (nums[left] == 0) {
                    zeros--;
                }
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

- “最多翻转一个 0” 等价于窗口内最多允许一个 0。
- 滑动窗口适合处理连续区间长度最大化问题。
- 核心思想：右边界负责扩张答案，左边界负责恢复约束。

---

## 488. 祖玛游戏 (Hard)

你正在参与祖玛游戏的一个变种。
在这个祖玛游戏变体中，桌面上有  **一排**  彩球，每个球的颜色可能是：红色  `'R'` 、黄色  `'Y'` 、蓝色  `'B'` 、绿色  `'G'`  或白色  `'W'`  。你的手中也有一些彩球。
你的目标是  **清空**  桌面上所有的球。每一回合：

从你手上的彩球中选出  **任意一颗**  ，然后将其插入桌面上那一排球中：两球之间或这一排球的任一端。
接着，如果有出现  **三个或者三个以上**  且  **颜色相同**  的球相连的话，就把它们移除掉。
	
如果这种移除操作同样导致出现三个或者三个以上且颜色相同的球相连，则可以继续移除这些球，直到不再满足移除条件。

如果桌面上所有球都被移除，则认为你赢得本场游戏。
重复这个过程，直到你赢了游戏或者手中没有更多的球。

给你一个字符串  `board`  ，表示桌面上最开始的那排球。另给你一个字符串  `hand`  ，表示手里的彩球。请你按上述操作步骤移除掉桌上所有球，计算并返回所需的  **最少**  球数。如果不能移除桌上所有的球，返回  `-1`  。
 
 **示例 1：** 

```text
输入：board = "WRRBBW", hand = "RB"
输出：-1
解释：无法移除桌面上的所有球。可以得到的最好局面是：
- 插入一个 'R' ，使桌面变为 WRRRBBW 。WRRRBBW -> WBBW
- 插入一个 'B' ，使桌面变为 WBBBW 。WBBBW -> WW
桌面上还剩着球，没有其他球可以插入。
```

 **示例 2：** 

```text
输入：board = "WWRRBBWW", hand = "WRBRW"
输出：2
解释：要想清空桌面上的球，可以按下述步骤：
- 插入一个 'R' ，使桌面变为 WWRRRBBWW 。WWRRRBBWW -> WWBBWW
- 插入一个 'B' ，使桌面变为 WWBBBWW 。WWBBBWW -> WWWW -> empty
只需从手中出 2 个球就可以清空桌面。
```

 **示例 3：** 

```text
输入：board = "G", hand = "GGGGG"
输出：2
解释：要想清空桌面上的球，可以按下述步骤：
- 插入一个 'G' ，使桌面变为 GG 。
- 插入一个 'G' ，使桌面变为 GGG 。GGG -> empty
只需从手中出 2 个球就可以清空桌面。
```

 **示例 4：** 

```text
输入：board = "RBYYBBRRB", hand = "YRBGB"
输出：3
解释：要想清空桌面上的球，可以按下述步骤：
- 插入一个 'Y' ，使桌面变为 RBYYYBBRRB 。RBYYYBBRRB -> RBBBRRB -> RRRB -> B
- 插入一个 'B' ，使桌面变为 BB 。
- 插入一个 'B' ，使桌面变为 BBB 。BBB -> empty
只需从手中出 3 个球就可以清空桌面。
```

 
 **提示：** 

 `1 <= board.length <= 16` 
 `1 <= hand.length <= 5` 
 `board`  和  `hand`  由字符  `'R'` 、 `'Y'` 、 `'B'` 、 `'G'`  和  `'W'`  组成
桌面上一开始的球中，不会有三个及三个以上颜色相同且连着的球

### Java 解法补充

#### 基础解法：枚举插入位置回溯

算法思想：每一步从手牌中选一颗球，插入桌面任意位置，然后执行连续消除。递归尝试所有可能，取清空桌面需要的最小步数。

```java
import java.util.Arrays;

class Solution {
    private static final int INF = 1_000_000;

    public int findMinStep(String board, String hand) {
        char[] balls = hand.toCharArray();
        Arrays.sort(balls);
        boolean[] used = new boolean[balls.length];
        int ans = dfs(board, balls, used);
        return ans >= INF ? -1 : ans;
    }

    private int dfs(String board, char[] balls, boolean[] used) {
        if (board.length() == 0) {
            return 0;
        }

        int ans = INF;
        for (int i = 0; i < balls.length; i++) {
            if (used[i] || (i > 0 && balls[i] == balls[i - 1] && !used[i - 1])) {
                continue;
            }
            used[i] = true;
            for (int pos = 0; pos <= board.length(); pos++) {
                String next = shrink(board.substring(0, pos) + balls[i] + board.substring(pos));
                int rest = dfs(next, balls, used);
                if (rest != INF) {
                    ans = Math.min(ans, rest + 1);
                }
            }
            used[i] = false;
        }

        return ans;
    }

    private String shrink(String s) {
        for (int i = 0; i < s.length();) {
            int j = i;
            while (j < s.length() && s.charAt(j) == s.charAt(i)) {
                j++;
            }
            if (j - i >= 3) {
                return shrink(s.substring(0, i) + s.substring(j));
            }
            i = j;
        }
        return s;
    }
}
```

复杂度：时间指数级，空间 `O(hand.length + board.length)`。

#### 资深解法：按连续段消除并记忆化

算法思想：与其枚举所有插入位置，不如枚举桌面上的连续同色段。若某段还差 `need` 个球就能消除，且手牌足够，就直接使用这些球消除该段，并对消除后的桌面递归。状态用桌面字符串和手牌计数记忆化。

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    private static final int INF = 1_000_000;
    private final Map<String, Integer> memo = new HashMap<>();

    public int findMinStep(String board, String hand) {
        int[] count = new int[128];
        for (int i = 0; i < hand.length(); i++) {
            count[hand.charAt(i)]++;
        }
        int ans = dfs(board, count);
        return ans >= INF ? -1 : ans;
    }

    private int dfs(String board, int[] count) {
        board = shrink(board);
        if (board.length() == 0) {
            return 0;
        }

        String key = board + "#" + count['R'] + "," + count['Y'] + "," +
                count['B'] + "," + count['G'] + "," + count['W'];
        if (memo.containsKey(key)) {
            return memo.get(key);
        }

        int ans = INF;
        for (int i = 0; i < board.length();) {
            int j = i;
            while (j < board.length() && board.charAt(j) == board.charAt(i)) {
                j++;
            }

            char color = board.charAt(i);
            int need = 3 - (j - i);
            if (count[color] >= need) {
                count[color] -= need;
                int rest = dfs(board.substring(0, i) + board.substring(j), count);
                if (rest != INF) {
                    ans = Math.min(ans, need + rest);
                }
                count[color] += need;
            }
            i = j;
        }

        memo.put(key, ans);
        return ans;
    }

    private String shrink(String s) {
        for (int i = 0; i < s.length();) {
            int j = i;
            while (j < s.length() && s.charAt(j) == s.charAt(i)) {
                j++;
            }
            if (j - i >= 3) {
                return shrink(s.substring(0, i) + s.substring(j));
            }
            i = j;
        }
        return s;
    }
}
```

复杂度：状态数有限但仍为指数级，记忆化后实际性能更稳定；空间取决于状态数量。

#### 基础语法与算法思想

- `substring(0, i) + substring(j)` 表示删除 `[i, j)` 这一段。
- 递归消除函数要反复执行，直到没有长度大于等于 3 的连续段。
- 核心思想：搜索题要尽量把动作抽象成“有意义的状态变化”，并用记忆化缓存重复状态。

---

## 489. 扫地机器人 (Hard)

暂无内容描述。

### Java 解法补充

#### 基础解法：DFS 回溯清扫可达区域

算法思想：机器人不知道地图，只能通过 `move()` 探测。把起点当作 `(0,0)`，用方向数组记录相对坐标；每进入一个新格子就清扫，并尝试四个方向。递归返回时要让机器人回到进入前的位置和朝向。

```java
import java.util.HashSet;
import java.util.Set;

interface Robot {
    boolean move();
    void turnLeft();
    void turnRight();
    void clean();
}

class Solution {
    private final int[][] dirs = {{-1, 0}, {0, 1}, {1, 0}, {0, -1}};
    private final Set<String> visited = new HashSet<>();

    public void cleanRoom(Robot robot) {
        dfs(robot, 0, 0, 0);
    }

    private void dfs(Robot robot, int row, int col, int dir) {
        visited.add(row + "," + col);
        robot.clean();

        for (int i = 0; i < 4; i++) {
            int nextDir = (dir + i) % 4;
            int nextRow = row + dirs[nextDir][0];
            int nextCol = col + dirs[nextDir][1];
            String key = nextRow + "," + nextCol;

            if (!visited.contains(key) && robot.move()) {
                dfs(robot, nextRow, nextCol, nextDir);
                goBack(robot);
            }
            robot.turnRight();
        }
    }

    private void goBack(Robot robot) {
        robot.turnRight();
        robot.turnRight();
        robot.move();
        robot.turnRight();
        robot.turnRight();
    }
}
```

复杂度：时间 `O(cells)`，空间 `O(cells)`，`cells` 为可达空格数量。

#### 资深解法：封装坐标编码和方向回溯

算法思想：生产实现中把坐标编码、回退动作和方向遍历拆清楚，减少机器人朝向错乱的风险。每次循环结束都右转一次，保证四次循环后恢复原朝向。

```java
import java.util.HashSet;
import java.util.Set;

class Solution {
    private static final int[][] DIRECTIONS = {{-1, 0}, {0, 1}, {1, 0}, {0, -1}};
    private final Set<Long> visited = new HashSet<>();

    public void cleanRoom(Robot robot) {
        backtrack(robot, 0, 0, 0);
    }

    private void backtrack(Robot robot, int row, int col, int direction) {
        visited.add(encode(row, col));
        robot.clean();

        for (int turn = 0; turn < 4; turn++) {
            int nextDirection = (direction + turn) % 4;
            int nextRow = row + DIRECTIONS[nextDirection][0];
            int nextCol = col + DIRECTIONS[nextDirection][1];
            long key = encode(nextRow, nextCol);

            if (!visited.contains(key) && robot.move()) {
                backtrack(robot, nextRow, nextCol, nextDirection);
                moveBack(robot);
            }
            robot.turnRight();
        }
    }

    private long encode(int row, int col) {
        return (((long) row) << 32) ^ (col & 0xffffffffL);
    }

    private void moveBack(Robot robot) {
        robot.turnRight();
        robot.turnRight();
        robot.move();
        robot.turnRight();
        robot.turnRight();
    }
}
```

复杂度：时间 `O(cells)`，空间 `O(cells)`。

#### 基础语法与算法思想

- 机器人 API 是相对移动，没有全局地图，需要自己维护相对坐标。
- 回溯不只要返回递归，还要把实体机器人移动回父节点。
- 核心思想：未知地图探索可以看作 DFS，`move()` 的返回值就是边是否可走。

---

## 490. 迷宫 (Medium)

暂无内容描述。

### Java 解法补充

#### 基础解法：DFS 滚动到停点

算法思想：球不是走一格停一格，而是沿一个方向一直滚到撞墙才停。DFS 每次从停点出发，尝试四个方向滚到新的停点。

```java
class Solution {
    private final int[][] dirs = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};

    public boolean hasPath(int[][] maze, int[] start, int[] destination) {
        boolean[][] visited = new boolean[maze.length][maze[0].length];
        return dfs(maze, start[0], start[1], destination, visited);
    }

    private boolean dfs(int[][] maze, int row, int col, int[] dest, boolean[][] visited) {
        if (visited[row][col]) {
            return false;
        }
        if (row == dest[0] && col == dest[1]) {
            return true;
        }

        visited[row][col] = true;
        for (int[] dir : dirs) {
            int nextRow = row;
            int nextCol = col;
            while (canMove(maze, nextRow + dir[0], nextCol + dir[1])) {
                nextRow += dir[0];
                nextCol += dir[1];
            }
            if (dfs(maze, nextRow, nextCol, dest, visited)) {
                return true;
            }
        }
        return false;
    }

    private boolean canMove(int[][] maze, int row, int col) {
        return row >= 0 && row < maze.length &&
                col >= 0 && col < maze[0].length &&
                maze[row][col] == 0;
    }
}
```

复杂度：时间 `O(mn * max(m, n))`，空间 `O(mn)`。

#### 资深解法：BFS 遍历停点图

算法思想：把每个能停下来的位置当作图节点，四个滚动方向产生边。用队列 BFS，避免递归深度风险，也更贴近服务端常规图搜索实现。

```java
import java.util.ArrayDeque;
import java.util.Queue;

class Solution {
    private static final int[][] DIRS = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};

    public boolean hasPath(int[][] maze, int[] start, int[] destination) {
        int m = maze.length;
        int n = maze[0].length;
        boolean[][] visited = new boolean[m][n];
        Queue<int[]> queue = new ArrayDeque<>();

        queue.offer(start);
        visited[start[0]][start[1]] = true;

        while (!queue.isEmpty()) {
            int[] cur = queue.poll();
            if (cur[0] == destination[0] && cur[1] == destination[1]) {
                return true;
            }

            for (int[] dir : DIRS) {
                int row = cur[0];
                int col = cur[1];
                while (row + dir[0] >= 0 && row + dir[0] < m &&
                        col + dir[1] >= 0 && col + dir[1] < n &&
                        maze[row + dir[0]][col + dir[1]] == 0) {
                    row += dir[0];
                    col += dir[1];
                }
                if (!visited[row][col]) {
                    visited[row][col] = true;
                    queue.offer(new int[]{row, col});
                }
            }
        }

        return false;
    }
}
```

复杂度：时间 `O(mn * max(m, n))`，空间 `O(mn)`。

#### 基础语法与算法思想

- 本题的节点是“停下的位置”，不是滚动经过的每个格子。
- `Queue<int[]>` 是 BFS 的常见写法。
- 核心思想：移动规则特殊时，先定义清楚图的节点和边，再套 DFS/BFS。

---

## 491. 非递减子序列 (Medium)

给你一个整数数组  `nums`  ，找出并返回所有该数组中不同的递增子序列，递增子序列中  **至少有两个元素**  。你可以按  **任意顺序**  返回答案。
数组中可能含有重复元素，如出现两个整数相等，也可以视作递增序列的一种特殊情况。
 
 **示例 1：** 

```text
输入：nums = [4,6,7,7]
输出：[[4,6],[4,6,7],[4,6,7,7],[4,7],[4,7,7],[6,7],[6,7,7],[7,7]]
```

 **示例 2：** 

```text
输入：nums = [4,4,3,2,1]
输出：[[4,4]]
```

 
 **提示：** 

 `1 <= nums.length <= 15` 
 `-100 <= nums[i] <= 100`

### Java 解法补充

#### 基础解法：枚举所有子序列并去重

算法思想：数组长度最多 15，可以用二进制掩码枚举所有非空子序列。对长度至少为 2 且非递减的序列放入集合去重。

```java
import java.util.ArrayList;
import java.util.HashSet;
import java.util.List;
import java.util.Set;

class Solution {
    public List<List<Integer>> findSubsequences(int[] nums) {
        Set<List<Integer>> seen = new HashSet<>();
        int total = 1 << nums.length;

        for (int mask = 0; mask < total; mask++) {
            List<Integer> path = new ArrayList<>();
            for (int i = 0; i < nums.length; i++) {
                if (((mask >> i) & 1) == 1) {
                    path.add(nums[i]);
                }
            }
            if (path.size() >= 2 && nonDecreasing(path)) {
                seen.add(path);
            }
        }

        return new ArrayList<>(seen);
    }

    private boolean nonDecreasing(List<Integer> path) {
        for (int i = 1; i < path.size(); i++) {
            if (path.get(i) < path.get(i - 1)) {
                return false;
            }
        }
        return true;
    }
}
```

复杂度：时间 `O(2^n * n)`，空间 `O(ans * n)`。

#### 资深解法：回溯加层级去重

算法思想：从左到右选择元素，保证新元素不小于路径最后一个元素。每一层用一个集合记录已经尝试过的数，避免同层选择重复值导致重复序列。

```java
import java.util.ArrayList;
import java.util.HashSet;
import java.util.List;
import java.util.Set;

class Solution {
    private final List<List<Integer>> ans = new ArrayList<>();

    public List<List<Integer>> findSubsequences(int[] nums) {
        backtrack(nums, 0, new ArrayList<>());
        return ans;
    }

    private void backtrack(int[] nums, int start, List<Integer> path) {
        if (path.size() >= 2) {
            ans.add(new ArrayList<>(path));
        }

        Set<Integer> used = new HashSet<>();
        for (int i = start; i < nums.length; i++) {
            if (used.contains(nums[i])) {
                continue;
            }
            if (!path.isEmpty() && nums[i] < path.get(path.size() - 1)) {
                continue;
            }

            used.add(nums[i]);
            path.add(nums[i]);
            backtrack(nums, i + 1, path);
            path.remove(path.size() - 1);
        }
    }
}
```

复杂度：时间 `O(2^n * n)`，空间 `O(n)`，不计答案空间。

#### 基础语法与算法思想

- 子序列保持原数组相对顺序，不能排序后再选。
- 回溯中的同层去重可以避免重复答案，同时不影响不同位置形成的合法扩展。
- 核心思想：非递减约束只需要和路径最后一个元素比较。

---

## 492. 构造矩形 (Easy)

作为一位web开发者， 懂得怎样去规划一个页面的尺寸是很重要的。 所以，现给定一个具体的矩形页面面积，你的任务是设计一个长度为 L 和宽度为 W 且满足以下要求的矩形的页面。要求：

你设计的矩形页面必须等于给定的目标面积。
宽度  `W`  不应大于长度  `L`  ，换言之，要求  `L >= W` 。
长度  `L`  和宽度  `W`  之间的差距应当尽可能小。

返回一个 数组  `[L, W]` ，其中  `L`  和  `W`  是你按照顺序设计的网页的长度和宽度。
 
 **示例1：** 

```text
输入: 4
输出: [2, 2]
解释: 目标面积是 4， 所有可能的构造方案有 [1,4], [2,2], [4,1]。
但是根据要求2，[1,4] 不符合要求; 根据要求3，[2,2] 比 [4,1] 更能符合要求. 所以输出长度 L 为 2， 宽度 W 为 2。
```

 **示例 2:** 

```text
输入: area = 37
输出: [37,1]
```

 **示例 3:** 

```text
输入: area = 122122
输出: [427,286]
```

 
 **提示:** 

 `1 <= area <= 107`

### Java 解法补充

#### 基础解法：枚举所有宽度

算法思想：枚举所有可能的宽度 `w`，如果能整除面积，就得到一个矩形 `(area / w, w)`。在满足 `L >= W` 的方案里选择差值最小的。

```java
class Solution {
    public int[] constructRectangle(int area) {
        int bestL = area;
        int bestW = 1;

        for (int w = 1; w <= area; w++) {
            if (area % w == 0) {
                int l = area / w;
                if (l >= w && l - w < bestL - bestW) {
                    bestL = l;
                    bestW = w;
                }
            }
        }

        return new int[]{bestL, bestW};
    }
}
```

复杂度：时间 `O(area)`，空间 `O(1)`。

#### 资深解法：从平方根向下找因子

算法思想：长宽差最小的矩形最接近正方形，因此从 `sqrt(area)` 开始向下找第一个能整除的宽度即可。

```java
class Solution {
    public int[] constructRectangle(int area) {
        int width = (int) Math.sqrt(area);
        while (area % width != 0) {
            width--;
        }
        return new int[]{area / width, width};
    }
}
```

复杂度：时间 `O(sqrt(area))`，空间 `O(1)`。

#### 基础语法与算法思想

- `area % width == 0` 表示 `width` 是面积的因子。
- `Math.sqrt(area)` 返回平方根。
- 核心思想：乘积固定时，两个因子越接近，差值越小。

---

## 493. 翻转对 (Hard)

给定一个数组  `nums`  ，如果  `i < j`  且  `nums[i] > 2*nums[j]`  我们就将  `(i, j)`  称作一个 **重要翻转对** 。
你需要返回给定数组中的重要翻转对的数量。
 **示例 1:** 

```text
输入: [1,3,2,3,1]
输出: 2
```

 **示例 2:** 

```text
输入: [2,4,3,5,1]
输出: 3
```

 **注意:** 

给定数组的长度不会超过 `50000` 。
输入数组中的所有数字都在32位整数的表示范围内。

### Java 解法补充

#### 基础解法：双重循环

算法思想：直接枚举所有 `i < j` 的数对，判断 `nums[i] > 2 * nums[j]` 是否成立。乘 2 时转成 `long`，避免整数溢出。

```java
class Solution {
    public int reversePairs(int[] nums) {
        int ans = 0;

        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                if ((long) nums[i] > 2L * nums[j]) {
                    ans++;
                }
            }
        }

        return ans;
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(1)`。

#### 资深解法：归并排序计数

算法思想：在归并排序过程中，左右两半都已经有序。对左半每个数，用指针在右半找满足 `left > 2 * right` 的最大范围，累计数量后再正常归并。

```java
class Solution {
    public int reversePairs(int[] nums) {
        int[] temp = new int[nums.length];
        return mergeSort(nums, 0, nums.length - 1, temp);
    }

    private int mergeSort(int[] nums, int left, int right, int[] temp) {
        if (left >= right) {
            return 0;
        }

        int mid = left + (right - left) / 2;
        int ans = mergeSort(nums, left, mid, temp) +
                mergeSort(nums, mid + 1, right, temp);

        int j = mid + 1;
        for (int i = left; i <= mid; i++) {
            while (j <= right && (long) nums[i] > 2L * nums[j]) {
                j++;
            }
            ans += j - (mid + 1);
        }

        int i = left;
        j = mid + 1;
        int k = left;
        while (i <= mid || j <= right) {
            if (j > right || (i <= mid && nums[i] <= nums[j])) {
                temp[k++] = nums[i++];
            } else {
                temp[k++] = nums[j++];
            }
        }
        for (i = left; i <= right; i++) {
            nums[i] = temp[i];
        }

        return ans;
    }
}
```

复杂度：时间 `O(n log n)`，空间 `O(n)`。

#### 基础语法与算法思想

- `2L * nums[j]` 让乘法按 `long` 计算。
- 归并排序能在保持有序的同时统计跨左右区间的数对。
- 核心思想：当右半有序时，满足条件的位置具有单调性，指针只向右移动。

---

## 494. 目标和 (Medium)

给你一个非负整数数组  `nums`  和一个整数  `target`  。
向数组中的每个整数前添加  `'+'`  或  `'-'`  ，然后串联起所有整数，可以构造一个  **表达式**  ：

例如， `nums = [2, 1]`  ，可以在  `2`  之前添加  `'+'`  ，在  `1`  之前添加  `'-'`  ，然后串联起来得到表达式  `"+2-1"`  。

返回可以通过上述方法构造的、运算结果等于  `target`  的不同  **表达式**  的数目。
 
 **示例 1：** 

```text
输入：nums = [1,1,1,1,1], target = 3
输出：5
解释：一共有 5 种方法让最终目标和为 3 。
-1 + 1 + 1 + 1 + 1 = 3
+1 - 1 + 1 + 1 + 1 = 3
+1 + 1 - 1 + 1 + 1 = 3
+1 + 1 + 1 - 1 + 1 = 3
+1 + 1 + 1 + 1 - 1 = 3
```

 **示例 2：** 

```text
输入：nums = [1], target = 1
输出：1
```

 
 **提示：** 

 `1 <= nums.length <= 20` 
 `0 <= nums[i] <= 1000` 
 `0 <= sum(nums[i]) <= 1000` 
 `-1000 <= target <= 1000`

### Java 解法补充

#### 基础解法：DFS 枚举正负号

算法思想：每个数字前可以放 `+` 或 `-`，递归枚举所有符号组合。到末尾时判断当前和是否等于目标。

```java
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        return dfs(nums, 0, 0, target);
    }

    private int dfs(int[] nums, int index, int sum, int target) {
        if (index == nums.length) {
            return sum == target ? 1 : 0;
        }
        return dfs(nums, index + 1, sum + nums[index], target) +
                dfs(nums, index + 1, sum - nums[index], target);
    }
}
```

复杂度：时间 `O(2^n)`，空间 `O(n)`。

#### 资深解法：转换为子集和 DP

算法思想：设加号集合和为 `P`，减号集合和为 `N`，有 `P - N = target` 且 `P + N = sum`，得到 `P = (sum + target) / 2`。问题转成从数组中选若干数凑出 `P` 的方案数。

```java
class Solution {
    public int findTargetSumWays(int[] nums, int target) {
        int sum = 0;
        for (int num : nums) {
            sum += num;
        }
        if (Math.abs(target) > sum || (sum + target) % 2 != 0) {
            return 0;
        }

        int bag = (sum + target) / 2;
        int[] dp = new int[bag + 1];
        dp[0] = 1;

        for (int num : nums) {
            for (int j = bag; j >= num; j--) {
                dp[j] += dp[j - num];
            }
        }

        return dp[bag];
    }
}
```

复杂度：时间 `O(n * sum)`，空间 `O(sum)`。

#### 基础语法与算法思想

- 每个数字只有正负两种选择，天然可以用 DFS。
- 目标和可以转成“选出一部分数作为正数集合”的背包计数。
- 核心思想：符号问题常能通过方程变形成子集和问题。

---

## 495. 提莫攻击 (Easy)

在《英雄联盟》的世界中，有一个叫 “提莫” 的英雄。他的攻击可以让敌方英雄艾希（编者注：寒冰射手）进入中毒状态。
当提莫攻击艾希，艾希的中毒状态正好持续  `duration`  秒。
正式地讲，提莫在  `t`  发起攻击意味着艾希在时间区间  `[t, t + duration - 1]` （含  `t`  和  `t + duration - 1` ）处于中毒状态。如果提莫在中毒影响结束  **前**  再次攻击，中毒状态计时器将会  **重置**  ，在新的攻击之后，中毒影响将会在  `duration`  秒后结束。
给你一个  **非递减**  的整数数组  `timeSeries`  ，其中  `timeSeries[i]`  表示提莫在  `timeSeries[i]`  秒时对艾希发起攻击，以及一个表示中毒持续时间的整数  `duration`  。
返回艾希处于中毒状态的  **总**  秒数。
 

 **示例 1：** 

```text
输入：timeSeries = [1,4], duration = 2
输出：4
解释：提莫攻击对艾希的影响如下：
- 第 1 秒，提莫攻击艾希并使其立即中毒。中毒状态会维持 2 秒，即第 1 秒和第 2 秒。
- 第 4 秒，提莫再次攻击艾希，艾希中毒状态又持续 2 秒，即第 4 秒和第 5 秒。
艾希在第 1、2、4、5 秒处于中毒状态，所以总中毒秒数是 4 。
```

 **示例 2：** 

```text
输入：timeSeries = [1,2], duration = 2
输出：3
解释：提莫攻击对艾希的影响如下：
- 第 1 秒，提莫攻击艾希并使其立即中毒。中毒状态会维持 2 秒，即第 1 秒和第 2 秒。
- 第 2 秒，提莫再次攻击艾希，并重置中毒计时器，艾希中毒状态需要持续 2 秒，即第 2 秒和第 3 秒。
艾希在第 1、2、3 秒处于中毒状态，所以总中毒秒数是 3 。
```

 
 **提示：** 

 `1 <= timeSeries.length <= 104` 
 `0 <= timeSeries[i], duration <= 107` 
 `timeSeries`  按  **非递减**  顺序排列

### Java 解法补充

#### 基础解法：合并中毒区间

算法思想：每次攻击产生一个区间 `[time, time + duration)`。如果新区间和当前区间重叠，就延长当前结束时间；否则把当前区间长度加入答案并开启新区间。

```java
class Solution {
    public int findPoisonedDuration(int[] timeSeries, int duration) {
        int ans = 0;
        int start = timeSeries[0];
        int end = start + duration;

        for (int i = 1; i < timeSeries.length; i++) {
            int nextStart = timeSeries[i];
            int nextEnd = nextStart + duration;
            if (nextStart <= end) {
                end = Math.max(end, nextEnd);
            } else {
                ans += end - start;
                start = nextStart;
                end = nextEnd;
            }
        }

        return ans + end - start;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 资深解法：累加相邻攻击贡献

算法思想：相邻两次攻击间隔为 `gap`。如果 `gap < duration`，前一次只贡献 `gap` 秒；否则贡献完整 `duration` 秒。最后一次攻击一定贡献完整时长。

```java
class Solution {
    public int findPoisonedDuration(int[] timeSeries, int duration) {
        int ans = 0;

        for (int i = 1; i < timeSeries.length; i++) {
            ans += Math.min(duration, timeSeries[i] - timeSeries[i - 1]);
        }

        return ans + duration;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 用左闭右开区间 `[start, end)` 表示持续时间，长度就是 `end - start`。
- 非递减时间序列允许只比较相邻攻击。
- 核心思想：重叠时间不要重复计算，取相邻间隔和持续时间的较小值即可。

---

## 496. 下一个更大元素 I (Easy)

`nums1`  中数字  `x`  的  **下一个更大元素**  是指  `x`  在  `nums2`  中对应位置  **右侧**  的  **第一个**  比  `x`  **** 大的元素。
给你两个 **没有重复元素**  的数组  `nums1`  和  `nums2`  ，下标从  **0**  开始计数，其中 `nums1`  是  `nums2`  的子集。
对于每个  `0 <= i < nums1.length`  ，找出满足  `nums1[i] == nums2[j]`  的下标  `j`  ，并且在  `nums2`  确定  `nums2[j]`  的  **下一个更大元素**  。如果不存在下一个更大元素，那么本次查询的答案是  `-1`  。
返回一个长度为  `nums1.length`  的数组  `ans`  作为答案，满足  `ans[i]`  是如上所述的  **下一个更大元素**  。
 
 **示例 1：** 

```text
输入：nums1 = [4,1,2], nums2 = [1,3,4,2].
输出：[-1,3,-1]
解释：nums1 中每个值的下一个更大元素如下所述：
- 4 ，用加粗斜体标识，nums2 = [1,3,4,2]。不存在下一个更大元素，所以答案是 -1 。
- 1 ，用加粗斜体标识，nums2 = [1,3,4,2]。下一个更大元素是 3 。
- 2 ，用加粗斜体标识，nums2 = [1,3,4,2]。不存在下一个更大元素，所以答案是 -1 。
```

 **示例 2：** 

```text
输入：nums1 = [2,4], nums2 = [1,2,3,4].
输出：[3,-1]
解释：nums1 中每个值的下一个更大元素如下所述：
- 2 ，用加粗斜体标识，nums2 = [1,2,3,4]。下一个更大元素是 3 。
- 4 ，用加粗斜体标识，nums2 = [1,2,3,4]。不存在下一个更大元素，所以答案是 -1 。
```

 
 **提示：** 

 `1 <= nums1.length <= nums2.length <= 1000` 
 `0 <= nums1[i], nums2[i] <= 104` 
 `nums1` 和 `nums2` 中所有整数  **互不相同** 
 `nums1`  中的所有整数同样出现在  `nums2`  中

 
 **进阶：** 你可以设计一个时间复杂度为  `O(nums1.length + nums2.length)`  的解决方案吗？

### Java 解法补充

#### 基础解法：按题意逐个查找

算法思想：对 `nums1` 中每个数，先在 `nums2` 中找到它的位置，再继续向右找第一个更大的数。找不到就返回 `-1`。

```java
class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        int[] ans = new int[nums1.length];

        for (int i = 0; i < nums1.length; i++) {
            int pos = 0;
            while (nums2[pos] != nums1[i]) {
                pos++;
            }

            ans[i] = -1;
            for (int j = pos + 1; j < nums2.length; j++) {
                if (nums2[j] > nums1[i]) {
                    ans[i] = nums2[j];
                    break;
                }
            }
        }

        return ans;
    }
}
```

复杂度：时间 `O(nums1.length * nums2.length)`，空间 `O(1)`，不计返回数组。

#### 资深解法：单调栈预处理

算法思想：遍历 `nums2`，用单调递减栈维护还没找到下一个更大元素的数。当前数比栈顶大时，当前数就是栈顶的下一个更大元素，记录到哈希表中。

```java
import java.util.ArrayDeque;
import java.util.Deque;
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int[] nextGreaterElement(int[] nums1, int[] nums2) {
        Map<Integer, Integer> nextGreater = new HashMap<>();
        Deque<Integer> stack = new ArrayDeque<>();

        for (int num : nums2) {
            while (!stack.isEmpty() && stack.peek() < num) {
                nextGreater.put(stack.pop(), num);
            }
            stack.push(num);
        }

        int[] ans = new int[nums1.length];
        for (int i = 0; i < nums1.length; i++) {
            ans[i] = nextGreater.getOrDefault(nums1[i], -1);
        }
        return ans;
    }
}
```

复杂度：时间 `O(nums1.length + nums2.length)`，空间 `O(nums2.length)`。

#### 基础语法与算法思想

- `Map` 可以把 `nums2` 中每个值映射到它的下一个更大元素。
- 单调栈适合“右侧第一个更大/更小”这类问题。
- 核心思想：一个元素一旦遇到右侧第一个更大值，就可以出栈并确定答案。

---

## 497. 非重叠矩形中的随机点 (Medium)

给定一个由非重叠的轴对齐矩形的数组  `rects`  ，其中  `rects[i] = [ai, bi, xi, yi]`  表示  `(ai, bi)`  是第  `i`  个矩形的左下角点， `(xi, yi)`  是第  `i`  个矩形的右上角点。设计一个算法来随机挑选一个被某一矩形覆盖的整数点。矩形周长上的点也算做是被矩形覆盖。所有满足要求的点必须等概率被返回。
在给定的矩形覆盖的空间内的任何整数点都有可能被返回。
 **请注意** ，整数点是具有整数坐标的点。
实现  `Solution`  类:

 `Solution(int[][] rects)`  用给定的矩形数组  `rects`  初始化对象。
 `int[] pick()`  返回一个随机的整数点  `[u, v]`  在给定的矩形所覆盖的空间内。

 
 **示例 1：** 

```text
输入: 
["Solution", "pick", "pick", "pick", "pick", "pick"]
[[[[-2, -2, 1, 1], [2, 2, 4, 6]]], [], [], [], [], []]
输出: 
[null, [1, -2], [1, -1], [-1, -2], [-2, -2], [0, 0]]

解释：
Solution solution = new Solution([[-2, -2, 1, 1], [2, 2, 4, 6]]);
solution.pick(); // 返回 [1, -2]
solution.pick(); // 返回 [1, -1]
solution.pick(); // 返回 [-1, -2]
solution.pick(); // 返回 [-2, -2]
solution.pick(); // 返回 [0, 0]
```

 
 **提示：** 

 `1 <= rects.length <= 100` 
 `rects[i].length == 4` 
 `-109 <= ai < xi <= 109` 
 `-109 <= bi < yi <= 109` 
 `xi - ai <= 2000` 
 `yi - bi <= 2000` 
所有的矩形不重叠。
 `pick`  最多被调用  `104`  次。

### Java 解法补充

#### 基础解法：预生成所有整数点

算法思想：把所有矩形覆盖的整数点都加入列表。因为列表里每个点只出现一次，所以随机取列表下标即可保证每个点等概率。

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Random;

class Solution {
    private final List<int[]> points = new ArrayList<>();
    private final Random random = new Random();

    public Solution(int[][] rects) {
        for (int[] rect : rects) {
            for (int x = rect[0]; x <= rect[2]; x++) {
                for (int y = rect[1]; y <= rect[3]; y++) {
                    points.add(new int[]{x, y});
                }
            }
        }
    }

    public int[] pick() {
        return points.get(random.nextInt(points.size()));
    }
}
```

复杂度：初始化时间和空间 `O(totalPoints)`，单次 `pick` 时间 `O(1)`。

#### 资深解法：前缀面积加二分

算法思想：每个矩形包含的整数点数为 `(x2 - x1 + 1) * (y2 - y1 + 1)`。先做点数前缀和，随机选中第几个点，再二分定位矩形，最后在矩形内随机坐标。

```java
import java.util.Arrays;
import java.util.Random;

class Solution {
    private final int[][] rects;
    private final int[] prefix;
    private final Random random = new Random();

    public Solution(int[][] rects) {
        this.rects = rects;
        this.prefix = new int[rects.length];

        int sum = 0;
        for (int i = 0; i < rects.length; i++) {
            int[] r = rects[i];
            sum += (r[2] - r[0] + 1) * (r[3] - r[1] + 1);
            prefix[i] = sum;
        }
    }

    public int[] pick() {
        int target = random.nextInt(prefix[prefix.length - 1]) + 1;
        int index = Arrays.binarySearch(prefix, target);
        if (index < 0) {
            index = -index - 1;
        }

        int[] r = rects[index];
        int x = r[0] + random.nextInt(r[2] - r[0] + 1);
        int y = r[1] + random.nextInt(r[3] - r[1] + 1);
        return new int[]{x, y};
    }
}
```

复杂度：初始化时间 `O(rects.length)`，空间 `O(rects.length)`；单次 `pick` 时间 `O(log rects.length)`。

#### 基础语法与算法思想

- `Random.nextInt(bound)` 返回 `[0, bound)` 的整数。
- 前缀和可以把按权重随机选择转成一次二分查找。
- 核心思想：想让每个点等概率，就要按矩形包含的点数作为矩形权重。

---

## 498. 对角线遍历 (Medium)

给你一个大小为  `m x n`  的矩阵  `mat`  ，请以对角线遍历的顺序，用一个数组返回这个矩阵中的所有元素。
 
 **示例 1：** 

```text
输入：mat = [[1,2,3],[4,5,6],[7,8,9]]
输出：[1,2,4,7,5,3,6,8,9]
```

 **示例 2：** 

```text
输入：mat = [[1,2],[3,4]]
输出：[1,2,3,4]
```

 
 **提示：** 

 `m == mat.length` 
 `n == mat[i].length` 
 `1 <= m, n <= 104` 
 `1 <= m * n <= 104` 
 `-105 <= mat[i][j] <= 105`

### Java 解法补充

#### 基础解法：按对角线编号收集

算法思想：同一条对角线上的元素满足 `row + col` 相同。按对角线编号收集元素，偶数编号需要反向输出，奇数编号按收集顺序输出。

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

class Solution {
    public int[] findDiagonalOrder(int[][] mat) {
        int m = mat.length;
        int n = mat[0].length;
        int[] ans = new int[m * n];
        int index = 0;

        for (int sum = 0; sum <= m + n - 2; sum++) {
            List<Integer> diagonal = new ArrayList<>();
            int startRow = Math.max(0, sum - n + 1);
            int endRow = Math.min(m - 1, sum);

            for (int row = startRow; row <= endRow; row++) {
                int col = sum - row;
                diagonal.add(mat[row][col]);
            }

            if (sum % 2 == 0) {
                Collections.reverse(diagonal);
            }
            for (int value : diagonal) {
                ans[index++] = value;
            }
        }

        return ans;
    }
}
```

复杂度：时间 `O(mn)`，空间 `O(min(m, n))`。

#### 资深解法：方向模拟

算法思想：维护当前位置和方向。向右上走时，如果越过上边界或右边界，就切换方向并修正位置；向左下走时同理。每个元素只访问一次。

```java
class Solution {
    public int[] findDiagonalOrder(int[][] mat) {
        int m = mat.length;
        int n = mat[0].length;
        int[] ans = new int[m * n];
        int row = 0;
        int col = 0;
        int direction = 1;

        for (int i = 0; i < ans.length; i++) {
            ans[i] = mat[row][col];

            if (direction == 1) {
                if (col == n - 1) {
                    row++;
                    direction = -1;
                } else if (row == 0) {
                    col++;
                    direction = -1;
                } else {
                    row--;
                    col++;
                }
            } else {
                if (row == m - 1) {
                    col++;
                    direction = 1;
                } else if (col == 0) {
                    row++;
                    direction = 1;
                } else {
                    row++;
                    col--;
                }
            }
        }

        return ans;
    }
}
```

复杂度：时间 `O(mn)`，空间 `O(1)`，不计返回数组。

#### 基础语法与算法思想

- 对角线题常用 `row + col` 分组。
- 模拟方向时要优先处理边界，否则容易访问越界。
- 核心思想：先确定遍历顺序的数学规律，再选择分组或直接模拟。

---

## 499. 迷宫 III (Hard)

暂无内容描述。

### Java 解法补充

#### 基础解法：优先队列搜索

算法思想：球会一直滚到墙边或洞口。用优先队列按“距离短优先，路径字典序小优先”取状态；第一次取出洞口时就是答案。

```java
import java.util.PriorityQueue;

class Solution {
    private static final int[][] DIRS = {{1, 0}, {0, -1}, {0, 1}, {-1, 0}};
    private static final String[] NAMES = {"d", "l", "r", "u"};

    public String findShortestWay(int[][] maze, int[] ball, int[] hole) {
        int m = maze.length;
        int n = maze[0].length;
        boolean[][] visited = new boolean[m][n];
        PriorityQueue<Node> pq = new PriorityQueue<>((a, b) -> {
            if (a.distance != b.distance) return a.distance - b.distance;
            return a.path.compareTo(b.path);
        });

        pq.offer(new Node(ball[0], ball[1], 0, ""));
        while (!pq.isEmpty()) {
            Node cur = pq.poll();
            if (visited[cur.row][cur.col]) {
                continue;
            }
            visited[cur.row][cur.col] = true;
            if (cur.row == hole[0] && cur.col == hole[1]) {
                return cur.path;
            }

            for (int i = 0; i < 4; i++) {
                int row = cur.row;
                int col = cur.col;
                int dist = cur.distance;
                while (row + DIRS[i][0] >= 0 && row + DIRS[i][0] < m &&
                        col + DIRS[i][1] >= 0 && col + DIRS[i][1] < n &&
                        maze[row + DIRS[i][0]][col + DIRS[i][1]] == 0) {
                    row += DIRS[i][0];
                    col += DIRS[i][1];
                    dist++;
                    if (row == hole[0] && col == hole[1]) {
                        break;
                    }
                }
                pq.offer(new Node(row, col, dist, cur.path + NAMES[i]));
            }
        }

        return "impossible";
    }

    private static class Node {
        int row;
        int col;
        int distance;
        String path;

        Node(int row, int col, int distance, String path) {
            this.row = row;
            this.col = col;
            this.distance = distance;
            this.path = path;
        }
    }
}
```

复杂度：时间 `O(mn log(mn) * max(m, n))`，空间 `O(mn)`。

#### 资深解法：Dijkstra 维护距离和路径

算法思想：显式维护每个停点的最短距离与当前最优字典序路径。只有当新状态更短，或距离相同但路径更小，才更新并入队，避免无意义状态扩张。

```java
import java.util.Arrays;
import java.util.PriorityQueue;

class Solution {
    private static final int[][] DIRS = {{1, 0}, {0, -1}, {0, 1}, {-1, 0}};
    private static final String[] NAMES = {"d", "l", "r", "u"};

    public String findShortestWay(int[][] maze, int[] ball, int[] hole) {
        int m = maze.length;
        int n = maze[0].length;
        int[][] dist = new int[m][n];
        String[][] path = new String[m][n];
        for (int[] row : dist) {
            Arrays.fill(row, Integer.MAX_VALUE);
        }

        PriorityQueue<Node> pq = new PriorityQueue<>((a, b) -> {
            if (a.distance != b.distance) return a.distance - b.distance;
            return a.path.compareTo(b.path);
        });

        dist[ball[0]][ball[1]] = 0;
        path[ball[0]][ball[1]] = "";
        pq.offer(new Node(ball[0], ball[1], 0, ""));

        while (!pq.isEmpty()) {
            Node cur = pq.poll();
            if (cur.distance > dist[cur.row][cur.col]) {
                continue;
            }
            if (cur.row == hole[0] && cur.col == hole[1]) {
                return cur.path;
            }

            for (int i = 0; i < 4; i++) {
                int row = cur.row;
                int col = cur.col;
                int distance = cur.distance;
                while (row + DIRS[i][0] >= 0 && row + DIRS[i][0] < m &&
                        col + DIRS[i][1] >= 0 && col + DIRS[i][1] < n &&
                        maze[row + DIRS[i][0]][col + DIRS[i][1]] == 0) {
                    row += DIRS[i][0];
                    col += DIRS[i][1];
                    distance++;
                    if (row == hole[0] && col == hole[1]) {
                        break;
                    }
                }

                String nextPath = cur.path + NAMES[i];
                if (distance < dist[row][col] ||
                        (distance == dist[row][col] &&
                                (path[row][col] == null || nextPath.compareTo(path[row][col]) < 0))) {
                    dist[row][col] = distance;
                    path[row][col] = nextPath;
                    pq.offer(new Node(row, col, distance, nextPath));
                }
            }
        }

        return "impossible";
    }

    private static class Node {
        int row;
        int col;
        int distance;
        String path;

        Node(int row, int col, int distance, String path) {
            this.row = row;
            this.col = col;
            this.distance = distance;
            this.path = path;
        }
    }
}
```

复杂度：时间 `O(mn log(mn) * max(m, n))`，空间 `O(mn)`。

#### 基础语法与算法思想

- 优先队列比较器可以同时按距离和字符串字典序排序。
- 球遇到洞要立即停下，不能继续滚到墙。
- 核心思想：本题是带字典序 tie-break 的最短路问题，用 Dijkstra 更稳。

---

## 500. 键盘行 (Easy)

给你一个字符串数组  `words`  ，只返回可以使用在  **美式键盘**  同一行的字母打印出来的单词。键盘如下图所示。
 **请注意** ，字符串  **不区分大小写** ，相同字母的大小写形式都被视为在同一行 **。** 
 **美式键盘**  中：

第一行由字符  `"qwertyuiop"`  组成。
第二行由字符  `"asdfghjkl"`  组成。
第三行由字符  `"zxcvbnm"`  组成。

 
 **示例 1：** 

 **输入：** words = ["Hello","Alaska","Dad","Peace"]
 **输出：** ["Alaska","Dad"]
 **解释：** 
由于不区分大小写， `"a"`  和  `"A"`  都在美式键盘的第二行。

 **示例 2：** 

 **输入：** words = ["omk"]
 **输出：** []

 **示例 3：** 

 **输入：** words = ["adsdf","sfd"]
 **输出：** ["adsdf","sfd"]

 
 **提示：** 

 `1 <= words.length <= 20` 
 `1 <= words[i].length <= 100` 
 `words[i]`  由英文字母（小写和大写字母）组成

### Java 解法补充

#### 基础解法：逐行字符串检查

算法思想：把三行键盘分别保存成字符串。对每个单词，转成小写后检查它是否能完全落在某一行中。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public String[] findWords(String[] words) {
        String[] rows = {"qwertyuiop", "asdfghjkl", "zxcvbnm"};
        List<String> ans = new ArrayList<>();

        for (String word : words) {
            String lower = word.toLowerCase();
            for (String row : rows) {
                boolean ok = true;
                for (int i = 0; i < lower.length(); i++) {
                    if (row.indexOf(lower.charAt(i)) < 0) {
                        ok = false;
                        break;
                    }
                }
                if (ok) {
                    ans.add(word);
                    break;
                }
            }
        }

        return ans.toArray(new String[0]);
    }
}
```

复杂度：时间 `O(totalChars)`，空间 `O(words.length)`。

#### 资深解法：字符到行号映射

算法思想：预处理每个字母所在的键盘行。检查单词时，只要所有字符映射到同一个行号，就加入答案。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public String[] findWords(String[] words) {
        int[] rowOf = new int[26];
        fill(rowOf, "qwertyuiop", 1);
        fill(rowOf, "asdfghjkl", 2);
        fill(rowOf, "zxcvbnm", 3);

        List<String> ans = new ArrayList<>();
        for (String word : words) {
            String lower = word.toLowerCase();
            int row = rowOf[lower.charAt(0) - 'a'];
            boolean ok = true;
            for (int i = 1; i < lower.length(); i++) {
                if (rowOf[lower.charAt(i) - 'a'] != row) {
                    ok = false;
                    break;
                }
            }
            if (ok) {
                ans.add(word);
            }
        }

        return ans.toArray(new String[0]);
    }

    private void fill(int[] rowOf, String letters, int row) {
        for (int i = 0; i < letters.length(); i++) {
            rowOf[letters.charAt(i) - 'a'] = row;
        }
    }
}
```

复杂度：时间 `O(totalChars)`，空间 `O(1)`，不计返回数组。

#### 基础语法与算法思想

- `toLowerCase()` 可以统一处理大小写。
- `ans.toArray(new String[0])` 把列表转成字符串数组。
- 核心思想：固定字符分类问题适合预处理映射表，查询时直接取下标。

---

## 501. 二叉搜索树中的众数 (Easy)

给你一个含重复值的二叉搜索树（BST）的根节点  `root`  ，找出并返回 BST 中的所有 众数（即，出现频率最高的元素）。
如果树中有不止一个众数，可以按  **任意顺序**  返回。
假定 BST 满足如下定义：

结点左子树中所含节点的值  **小于等于**  当前节点的值
结点右子树中所含节点的值  **大于等于**  当前节点的值
左子树和右子树都是二叉搜索树

 
 **示例 1：** 

```text
输入：root = [1,null,2,2]
输出：[2]
```

 **示例 2：** 

```text
输入：root = [0]
输出：[0]
```

 
 **提示：** 

树中节点的数目在范围  `[1, 104]`  内
 `-105 <= Node.val <= 105` 

 
 **进阶：** 你可以不使用额外的空间吗？（假设由递归产生的隐式调用栈的开销不被计算在内）

---

## 502. IPO (Hard)

假设 力扣（LeetCode）即将开始  **IPO**  。为了以更高的价格将股票卖给风险投资公司，力扣 希望在 IPO 之前开展一些项目以增加其资本。 由于资源有限，它只能在 IPO 之前完成最多  `k`  个不同的项目。帮助 力扣 设计完成最多  `k`  个不同项目后得到最大总资本的方式。
给你  `n`  个项目。对于每个项目  `i`  **** ，它都有一个纯利润  `profits[i]`  ，和启动该项目需要的最小资本  `capital[i]`  。
最初，你的资本为  `w`  。当你完成一个项目时，你将获得纯利润，且利润将被添加到你的总资本中。
总而言之，从给定项目中选择  **最多**   `k`  个不同项目的列表，以  **最大化最终资本**  ，并输出最终可获得的最多资本。
答案保证在 32 位有符号整数范围内。
 
 **示例 1：** 

```text
输入：k = 2, w = 0, profits = [1,2,3], capital = [0,1,1]
输出：4
解释：
由于你的初始资本为 0，你仅可以从 0 号项目开始。
在完成后，你将获得 1 的利润，你的总资本将变为 1。
此时你可以选择开始 1 号或 2 号项目。
由于你最多可以选择两个项目，所以你需要完成 2 号项目以获得最大的资本。
因此，输出最后最大化的资本，为 0 + 1 + 3 = 4。
```

 **示例 2：** 

```text
输入：k = 3, w = 0, profits = [1,2,3], capital = [0,1,2]
输出：6
```

 
 **提示：** 

 `1 <= k <= 105` 
 `0 <= w <= 109` 
 `n == profits.length` 
 `n == capital.length` 
 `1 <= n <= 105` 
 `0 <= profits[i] <= 104` 
 `0 <= capital[i] <= 109`

---

## 503. 下一个更大元素 II (Medium)

给定一个循环数组  `nums`  （  `nums[nums.length - 1]`  的下一个元素是  `nums[0]`  ），返回  `nums`  中每个元素的  **下一个更大元素**  。
数字  `x`  的  **下一个更大的元素**  是按数组遍历顺序，这个数字之后的第一个比它更大的数，这意味着你应该循环地搜索它的下一个更大的数。如果不存在，则输出  `-1`  。
 
 **示例 1:** 

```text
输入: nums = [1,2,1]
输出: [2,-1,2]
解释: 第一个 1 的下一个更大的数是 2；
数字 2 找不到下一个更大的数； 
第二个 1 的下一个最大的数需要循环搜索，结果也是 2。
```

 **示例 2:** 

```text
输入: nums = [1,2,3,4,3]
输出: [2,3,4,-1,4]
```

 
 **提示:** 

 `1 <= nums.length <= 104` 
 `-109 <= nums[i] <= 109`

---

## 504. 七进制数 (Easy)

给定一个整数  `num` ，将其转化为  **7 进制** ，并以字符串形式输出。
 
 **示例 1:** 

```text
输入: num = 100
输出: "202"
```

 **示例 2:** 

```text
输入: num = -7
输出: "-10"
```

 
 **提示：** 

 `-107 <= num <= 107`

---

## 505. 迷宫 II (Medium)

暂无内容描述。

---

## 506. 相对名次 (Easy)

给你一个长度为  `n`  的整数数组  `score`  ，其中  `score[i]`  是第  `i`  位运动员在比赛中的得分。所有得分都  **互不相同**  。
运动员将根据得分  **决定名次**  ，其中名次第  `1`  的运动员得分最高，名次第  `2`  的运动员得分第  `2`  高，依此类推。运动员的名次决定了他们的获奖情况：

名次第  `1`  的运动员获金牌  `"Gold Medal"`  。
名次第  `2`  的运动员获银牌  `"Silver Medal"`  。
名次第  `3`  的运动员获铜牌  `"Bronze Medal"`  。
从名次第  `4`  到第  `n`  的运动员，只能获得他们的名次编号（即，名次第  `x`  的运动员获得编号  `"x"` ）。

使用长度为  `n`  的数组  `answer`  返回获奖，其中  `answer[i]`  是第  `i`  位运动员的获奖情况。
 
 **示例 1：** 

```text
输入：score = [5,4,3,2,1]
输出：["Gold Medal","Silver Medal","Bronze Medal","4","5"]
解释：名次为 [1st, 2nd, 3rd, 4th, 5th] 。
```

 **示例 2：** 

```text
输入：score = [10,3,8,9,4]
输出：["Gold Medal","5","Bronze Medal","Silver Medal","4"]
解释：名次为 [1st, 5th, 3rd, 2nd, 4th] 。
```

 
 **提示：** 

 `n == score.length` 
 `1 <= n <= 104` 
 `0 <= score[i] <= 106` 
 `score`  中的所有值  **互不相同**

---

## 507. 完美数 (Easy)

对于一个  **正整数** ，如果它和除了它自身以外的所有  **正因子**  之和相等，我们称它为  **「完美数」** 。
给定一个  **整数**  `n` ， 如果是完美数，返回  `true` ；否则返回  `false` 。
 
 **示例 1：** 

```text
输入：num = 28
输出：true
解释：28 = 1 + 2 + 4 + 7 + 14
1, 2, 4, 7, 和 14 是 28 的所有正因子。
```

 **示例 2：** 

```text
输入：num = 7
输出：false
```

 
 **提示：** 

 `1 <= num <= 108`

---

## 508. 出现次数最多的子树元素和 (Medium)

给你一个二叉树的根结点  `root`  ，请返回出现次数最多的子树元素和。如果有多个元素出现的次数相同，返回所有出现次数最多的子树元素和（不限顺序）。
一个结点的  **「子树元素和」**  定义为以该结点为根的二叉树上所有结点的元素之和（包括结点本身）。
 
 **示例 1：** 

```text
输入: root = [5,2,-3]
输出: [2,-3,4]
```

 **示例 2：** 

```text
输入: root = [5,2,-5]
输出: [2]
```

 
 **提示:** 

节点数在  `[1, 104]`  范围内
 `-105 <= Node.val <= 105`

---

## 509. 斐波那契数 (Easy)

**斐波那契数**  （通常用  `F(n)`  表示）形成的序列称为  **斐波那契数列**  。该数列由  `0`  和  `1`  开始，后面的每一项数字都是前面两项数字的和。也就是：

```text
F(0) = 0，F(1) = 1
F(n) = F(n - 1) + F(n - 2)，其中 n > 1
```

给定  `n`  ，请计算  `F(n)`  。
 
 **示例 1：** 

```text
输入：n = 2
输出：1
解释：F(2) = F(1) + F(0) = 1 + 0 = 1
```

 **示例 2：** 

```text
输入：n = 3
输出：2
解释：F(3) = F(2) + F(1) = 1 + 1 = 2
```

 **示例 3：** 

```text
输入：n = 4
输出：3
解释：F(4) = F(3) + F(2) = 2 + 1 = 3
```

 
 **提示：** 

 `0 <= n <= 30`

---

## 510. 二叉搜索树中的中序后继 II (Medium)

暂无内容描述。

---

