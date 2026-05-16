# LeetCode 题目合集 Part 16

## 451. 根据字符出现频率排序 (Medium)

给定一个字符串  `s`  ，根据字符出现的  **频率**  对其进行  **降序排序**  。一个字符出现的  **频率**  是它出现在字符串中的次数。
返回 已排序的字符串 。如果有多个答案，返回其中任何一个。
 
 **示例 1:** 

```text
输入: s = "tree"
输出: "eert"
解释: 'e'出现两次，'r'和't'都只出现一次。
因此'e'必须出现在'r'和't'之前。此外，"eetr"也是一个有效的答案。
```

 **示例 2:** 

```text
输入: s = "cccaaa"
输出: "cccaaa"
解释: 'c'和'a'都出现三次。此外，"aaaccc"也是有效的答案。
注意"cacaca"是不正确的，因为相同的字母必须放在一起。
```

 **示例 3:** 

```text
输入: s = "Aabb"
输出: "bbAa"
解释: 此外，"bbaA"也是一个有效的答案，但"Aabb"是不正确的。
注意'A'和'a'被认为是两种不同的字符。
```

 
 **提示:** 

 `1 <= s.length <= 5 * 105` 
 `s`  由大小写英文字母和数字组成

### Java 解法补充

#### 基础解法：统计后排序字符

算法思想：先统计每个字符出现次数，再把不同字符按频次降序排序，最后按频次重复追加。

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

class Solution {
    public String frequencySort(String s) {
        Map<Character, Integer> count = new HashMap<>();
        for (char c : s.toCharArray()) {
            count.put(c, count.getOrDefault(c, 0) + 1);
        }
        List<Character> chars = new ArrayList<>(count.keySet());
        chars.sort((a, b) -> count.get(b) - count.get(a));
        StringBuilder ans = new StringBuilder();
        for (char c : chars) {
            for (int i = 0; i < count.get(c); i++) ans.append(c);
        }
        return ans.toString();
    }
}
```

复杂度：时间 `O(n + m log m)`，空间 `O(m)`，`m` 为不同字符数。

#### 资深解法：桶排序

算法思想：频次最大不超过 `s.length()`，用桶数组把相同频次的字符放在一起，再从高频桶向低频桶输出。

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

class Solution {
    public String frequencySort(String s) {
        Map<Character, Integer> count = new HashMap<>();
        for (char c : s.toCharArray()) count.put(c, count.getOrDefault(c, 0) + 1);
        List<Character>[] buckets = new ArrayList[s.length() + 1];
        for (Map.Entry<Character, Integer> entry : count.entrySet()) {
            int freq = entry.getValue();
            if (buckets[freq] == null) buckets[freq] = new ArrayList<>();
            buckets[freq].add(entry.getKey());
        }

        StringBuilder ans = new StringBuilder();
        for (int freq = buckets.length - 1; freq > 0; freq--) {
            if (buckets[freq] == null) continue;
            for (char c : buckets[freq]) {
                for (int i = 0; i < freq; i++) ans.append(c);
            }
        }
        return ans.toString();
    }
}
```

复杂度：时间 `O(n + m)`，空间 `O(n)`。

#### 基础语法与算法思想

- `Map<Character, Integer>` 适合统计任意字符频次。
- 频次范围有限时，桶排序可以避免比较排序。
- 核心思想：按频次排序先统计，再决定用排序或桶。

---

## 452. 用最少数量的箭引爆气球 (Medium)

有一些球形气球贴在一堵用 XY 平面表示的墙面上。墙面上的气球记录在整数数组  `points`  ，其中 `points[i] = [xstart, xend]`  表示水平直径在  `xstart`  和  `xend` 之间的气球。你不知道气球的确切 y 坐标。
一支弓箭可以沿着 x 轴从不同点  **完全垂直**  地射出。在坐标  `x`  处射出一支箭，若有一个气球的直径的开始和结束坐标为  `xstart` ， `xend` ， 且满足   `xstart ≤ x ≤ xend` ，则该气球会被  **引爆**  。可以射出的弓箭的数量  **没有限制**  。 弓箭一旦被射出之后，可以无限地前进。
给你一个数组  `points`  ，返回引爆所有气球所必须射出的  **最小**  弓箭数 。
 

 **示例 1：** 

```text
输入：points = [[10,16],[2,8],[1,6],[7,12]]
输出：2
解释：气球可以用2支箭来爆破:
-在x = 6处射出箭，击破气球[2,8]和[1,6]。
-在x = 11处发射箭，击破气球[10,16]和[7,12]。
```

 **示例 2：** 

```text
输入：points = [[1,2],[3,4],[5,6],[7,8]]
输出：4
解释：每个气球需要射出一支箭，总共需要4支箭。
```

 **示例 3：** 

```text
输入：points = [[1,2],[2,3],[3,4],[4,5]]
输出：2
解释：气球可以用2支箭来爆破:
- 在x = 2处发射箭，击破气球[1,2]和[2,3]。
- 在x = 4处射出箭，击破气球[3,4]和[4,5]。
```

 

 **提示:** 

 `1 <= points.length <= 105` 
 `points[i].length == 2` 
 `-231 <= xstart < xend <= 231 - 1`

### Java 解法补充

#### 基础解法：按起点排序并维护重叠区间

算法思想：按起点排序，维护当前一支箭可以覆盖的重叠区间 `[left, right]`。若新气球与它不重叠，就需要新箭。

```java
import java.util.Arrays;

class Solution {
    public int findMinArrowShots(int[][] points) {
        Arrays.sort(points, (a, b) -> Integer.compare(a[0], b[0]));
        int arrows = 1;
        int right = points[0][1];
        for (int i = 1; i < points.length; i++) {
            if (points[i][0] <= right) {
                right = Math.min(right, points[i][1]);
            } else {
                arrows++;
                right = points[i][1];
            }
        }
        return arrows;
    }
}
```

复杂度：时间 `O(n log n)`，空间 `O(1)`。

#### 资深解法：按右端点贪心

算法思想：每次把箭射在当前最早结束气球的右端点，可以覆盖所有起点不超过该点的气球。遇到无法覆盖的气球再开新箭。

```java
import java.util.Arrays;

class Solution {
    public int findMinArrowShots(int[][] points) {
        Arrays.sort(points, (a, b) -> Integer.compare(a[1], b[1]));
        int arrows = 1;
        int shoot = points[0][1];
        for (int i = 1; i < points.length; i++) {
            if (points[i][0] > shoot) {
                arrows++;
                shoot = points[i][1];
            }
        }
        return arrows;
    }
}
```

复杂度：时间 `O(n log n)`，空间 `O(1)`。

#### 基础语法与算法思想

- `Integer.compare` 避免端点相减溢出。
- 区间问题中“最少点覆盖区间”常按右端点贪心。
- 核心思想：尽量把箭放在当前气球最靠右的位置，给后续留下最大机会。

---

## 453. 最小操作次数使数组元素相等 (Medium)

给你一个长度为  `n`  的整数数组，每次操作将会使  `n - 1`  个元素增加  `1`  。返回让数组所有元素相等的最小操作次数。
 
 **示例 1：** 

```text
输入：nums = [1,2,3]
输出：3
解释：
只需要3次操作（注意每次操作会增加两个元素的值）：
[1,2,3]  =>  [2,3,3]  =>  [3,4,3]  =>  [4,4,4]
```

 **示例 2：** 

```text
输入：nums = [1,1,1]
输出：0
```

 
 **提示：** 

 `n == nums.length` 
 `1 <= nums.length <= 105` 
 `-109 <= nums[i] <= 109` 
答案保证符合  **32-bit**  整数

### Java 解法补充

#### 基础解法：模拟提高较小元素

算法思想：每次操作等价于选择一个元素不加，其余元素加一。直接模拟时可以每轮让除最大值外的元素加一，直到全部相等，直观但会超时。

```java
class Solution {
    public int minMoves(int[] nums) {
        int moves = 0;
        while (true) {
            int maxIndex = 0;
            boolean same = true;
            for (int i = 1; i < nums.length; i++) {
                if (nums[i] != nums[0]) same = false;
                if (nums[i] > nums[maxIndex]) maxIndex = i;
            }
            if (same) return moves;
            for (int i = 0; i < nums.length; i++) {
                if (i != maxIndex) nums[i]++;
            }
            moves++;
        }
    }
}
```

复杂度：时间与答案成正比，空间 `O(1)`。

#### 资深解法：转化为减少到最小值

算法思想：给 `n-1` 个元素加一，等价于让剩下那个元素减一。最终所有元素都降到最小值，操作次数为 `sum(nums[i] - min)`。

```java
class Solution {
    public int minMoves(int[] nums) {
        int min = nums[0];
        for (int num : nums) min = Math.min(min, num);
        int ans = 0;
        for (int num : nums) ans += num - min;
        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 反向思考操作，经常能把复杂模拟变成公式。
- 所有数相等时，目标一定可以看成原数组最小值。
- 核心思想：整体加一的相对效果等价于单个元素减一。

---

## 454. 四数相加 II (Medium)

给你四个整数数组  `nums1` 、 `nums2` 、 `nums3`  和  `nums4`  ，数组长度都是  `n`  ，请你计算有多少个元组  `(i, j, k, l)`  能满足：

 `0 <= i, j, k, l < n` 
 `nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0` 

 
 **示例 1：** 

```text
输入：nums1 = [1,2], nums2 = [-2,-1], nums3 = [-1,2], nums4 = [0,2]
输出：2
解释：
两个元组如下：
1. (0, 0, 0, 1) -> nums1[0] + nums2[0] + nums3[0] + nums4[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> nums1[1] + nums2[1] + nums3[0] + nums4[0] = 2 + (-1) + (-1) + 0 = 0
```

 **示例 2：** 

```text
输入：nums1 = [0], nums2 = [0], nums3 = [0], nums4 = [0]
输出：1
```

 
   **提示：** 

 `n == nums1.length` 
 `n == nums2.length` 
 `n == nums3.length` 
 `n == nums4.length` 
 `1 <= n <= 200` 
 `-228 <= nums1[i], nums2[i], nums3[i], nums4[i] <= 228`

### Java 解法补充

#### 基础解法：四重循环

算法思想：直接枚举四个数组中的元素，和为 0 就计数。

```java
class Solution {
    public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
        int ans = 0;
        for (int a : nums1) {
            for (int b : nums2) {
                for (int c : nums3) {
                    for (int d : nums4) {
                        if (a + b + c + d == 0) ans++;
                    }
                }
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(n^4)`，空间 `O(1)`。

#### 资深解法：两两分组哈希

算法思想：先统计 `nums1 + nums2` 的所有和出现次数，再枚举 `nums3 + nums4`，查找相反数出现次数。

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
        Map<Integer, Integer> count = new HashMap<>();
        for (int a : nums1) {
            for (int b : nums2) {
                count.put(a + b, count.getOrDefault(a + b, 0) + 1);
            }
        }
        int ans = 0;
        for (int c : nums3) {
            for (int d : nums4) {
                ans += count.getOrDefault(-c - d, 0);
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(n^2)`。

#### 基础语法与算法思想

- `getOrDefault` 是频次统计常用方法。
- 四数和可拆成两个两数和，降低枚举维度。
- 核心思想：多数组求和计数常用“分组 + 哈希表”。

---

## 455. 分发饼干 (Easy)

假设你是一位很棒的家长，想要给你的孩子们一些小饼干。但是，每个孩子最多只能给一块饼干。
对每个孩子  `i` ，都有一个胃口值  `g[i]` ，这是能让孩子们满足胃口的饼干的最小尺寸；并且每块饼干  `j` ，都有一个尺寸  `s[j]`  。如果  `s[j] >= g[i]` ，我们可以将这个饼干  `j`  分配给孩子  `i`  ，这个孩子会得到满足。你的目标是满足尽可能多的孩子，并输出这个最大数值。
 

 **示例 1:** 

```text
输入: g = [1,2,3], s = [1,1]
输出: 1
解释: 
你有三个孩子和两块小饼干，3 个孩子的胃口值分别是：1,2,3。
虽然你有两块小饼干，由于他们的尺寸都是 1，你只能让胃口值是 1 的孩子满足。
所以你应该输出 1。
```

 **示例 2:** 

```text
输入: g = [1,2], s = [1,2,3]
输出: 2
解释: 
你有两个孩子和三块小饼干，2 个孩子的胃口值分别是 1,2。
你拥有的饼干数量和尺寸都足以让所有孩子满足。
所以你应该输出 2。
```

 
 **提示：** 

 `1 <= g.length <= 3 * 104` 
 `0 <= s.length <= 3 * 104` 
 `1 <= g[i], s[j] <= 231 - 1` 

 
 **注意：** 本题与 2410. 运动员和训练师的最大匹配数 题相同。

### Java 解法补充

#### 基础解法：每个孩子找一块可用饼干

算法思想：按孩子顺序遍历，对每个孩子扫描饼干数组，找一块未使用且尺寸足够的最小饼干。

```java
class Solution {
    public int findContentChildren(int[] g, int[] s) {
        boolean[] used = new boolean[s.length];
        int ans = 0;
        for (int greed : g) {
            int best = -1;
            for (int i = 0; i < s.length; i++) {
                if (!used[i] && s[i] >= greed && (best == -1 || s[i] < s[best])) {
                    best = i;
                }
            }
            if (best != -1) {
                used[best] = true;
                ans++;
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(mn)`，空间 `O(n)`。

#### 资深解法：排序双指针

算法思想：胃口和饼干都从小到大排序。用当前最小可用饼干尽量满足当前最小胃口孩子，满足则两个指针都前进，否则换更大的饼干。

```java
import java.util.Arrays;

class Solution {
    public int findContentChildren(int[] g, int[] s) {
        Arrays.sort(g);
        Arrays.sort(s);
        int child = 0;
        int cookie = 0;
        while (child < g.length && cookie < s.length) {
            if (s[cookie] >= g[child]) {
                child++;
            }
            cookie++;
        }
        return child;
    }
}
```

复杂度：时间 `O(m log m + n log n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 贪心分配中，小资源优先满足小需求通常更稳。
- 排序后双指针可以避免反复查找。
- 核心思想：不要用大饼干浪费在小胃口前，能满足就立刻匹配。

---

## 456. 132 模式 (Medium)

给你一个整数数组  `nums`  ，数组中共有  `n`  个整数。 **132 模式的子序列**  由三个整数  `nums[i]` 、 `nums[j]`  和  `nums[k]`  组成，并同时满足： `i < j < k`  和  `nums[i] < nums[k] < nums[j]`  。
如果  `nums`  中存在  **132 模式的子序列**  ，返回  `true`  ；否则，返回  `false`  。
 
 **示例 1：** 

```text
输入：nums = [1,2,3,4]
输出：false
解释：序列中不存在 132 模式的子序列。
```

 **示例 2：** 

```text
输入：nums = [3,1,4,2]
输出：true
解释：序列中有 1 个 132 模式的子序列： [1, 4, 2] 。
```

 **示例 3：** 

```text
输入：nums = [-1,3,2,0]
输出：true
解释：序列中有 3 个 132 模式的的子序列：[-1, 3, 2]、[-1, 3, 0] 和 [-1, 2, 0] 。
```

 
 **提示：** 

 `n == nums.length` 
 `1 <= n <= 2 * 105` 
 `-109 <= nums[i] <= 109`

### Java 解法补充

#### 基础解法：枚举中间和右端

算法思想：枚举 `j` 和 `k`，维护 `j` 左侧最小值 `leftMin[j]`。只要存在 `leftMin[j] < nums[k] < nums[j]`，就找到了 132 模式。

```java
class Solution {
    public boolean find132pattern(int[] nums) {
        int n = nums.length;
        int[] leftMin = new int[n];
        leftMin[0] = nums[0];
        for (int i = 1; i < n; i++) {
            leftMin[i] = Math.min(leftMin[i - 1], nums[i]);
        }
        for (int j = 1; j < n - 1; j++) {
            for (int k = j + 1; k < n; k++) {
                if (leftMin[j] < nums[k] && nums[k] < nums[j]) {
                    return true;
                }
            }
        }
        return false;
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(n)`。

#### 资深解法：从右向左单调栈

算法思想：从右往左扫描，把可能作为 `nums[k]` 的值维护在栈中，变量 `second` 保存当前能作为 132 中 “2” 的最大值。如果当前 `nums[i] < second`，则找到模式。

```java
import java.util.ArrayDeque;
import java.util.Deque;

class Solution {
    public boolean find132pattern(int[] nums) {
        Deque<Integer> stack = new ArrayDeque<>();
        int second = Integer.MIN_VALUE;
        for (int i = nums.length - 1; i >= 0; i--) {
            if (nums[i] < second) return true;
            while (!stack.isEmpty() && nums[i] > stack.peek()) {
                second = stack.pop();
            }
            stack.push(nums[i]);
        }
        return false;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 基础语法与算法思想

- 132 模式中，中间值最大，右侧值居中，左侧值最小。
- 单调栈从右侧维护候选 “3/2” 关系。
- 核心思想：从右往左更容易在看到左侧小值时确认模式。

---

## 457. 环形数组是否存在循环 (Medium)

存在一个不含  `0`  的 **环形** 数组  `nums`  ，每个  `nums[i]`  都表示位于下标  `i`  的角色应该向前或向后移动的下标个数：

如果  `nums[i]`  是正数， **向前** （下标递增方向）移动  `nums[i]`  步
如果  `nums[i]`  是负数， **向后** （下标递减方向）移动  `abs(nums[i])`  步

因为数组是  **环形**  的，所以可以假设从最后一个元素向前移动一步会到达第一个元素，而第一个元素向后移动一步会到达最后一个元素。
数组中的  **循环**  由长度为  `k`  的下标序列  `seq`  标识：

遵循上述移动规则将导致一组重复下标序列  `seq[0] -> seq[1] -> ... -> seq[k - 1] -> seq[0] -> ...` 
所有  `nums[seq[j]]`  应当不是  **全正**  就是  **全负** 
 `k > 1` 

如果  `nums`  中存在循环，返回  `true`  ；否则，返回  `false`  。
 
 **示例 1：** 

```text
输入：nums = [2,-1,1,2,2]
输出：true
解释：图片展示了节点间如何连接。白色节点向前跳跃，而红色节点向后跳跃。
我们可以看到存在循环，按下标 0 -> 2 -> 3 -> 0 --> ...，并且其中的所有节点都是白色（以相同方向跳跃）。
```

 **示例 2：** 

```text
输入：nums = [-1,-2,-3,-4,-5,6]
输出：false
解释：图片展示了节点间如何连接。白色节点向前跳跃，而红色节点向后跳跃。
唯一的循环长度为 1，所以返回 false。
```

 **示例 3：** 

```text
输入：nums = [1,-1,5,1,4]
输出：true
解释：图片展示了节点间如何连接。白色节点向前跳跃，而红色节点向后跳跃。
我们可以看到存在循环，按下标 0 --> 1 --> 0 --> ...，当它的大小大于 1 时，它有一个向前跳的节点和一个向后跳的节点，所以 它不是一个循环。
我们可以看到存在循环，按下标 3 --> 4 --> 3 --> ...，并且其中的所有节点都是白色（以相同方向跳跃）。
```

 
 **提示：** 

 `1 <= nums.length <= 5000` 
 `-1000 <= nums[i] <= 1000` 
 `nums[i] != 0` 

 
 **进阶：** 你能设计一个时间复杂度为  `O(n)`  且额外空间复杂度为  `O(1)`  的算法吗？

### Java 解法补充

#### 基础解法：从每个起点用集合判环

算法思想：从每个下标出发，按跳转规则前进，用集合记录当前路径访问过的下标。方向变化、长度为 1 的自环都不是合法循环。

```java
import java.util.HashSet;
import java.util.Set;

class Solution {
    public boolean circularArrayLoop(int[] nums) {
        for (int start = 0; start < nums.length; start++) {
            Set<Integer> seen = new HashSet<>();
            int cur = start;
            boolean positive = nums[start] > 0;
            while ((nums[cur] > 0) == positive) {
                if (!seen.add(cur)) return true;
                int next = next(nums, cur);
                if (next == cur) break;
                cur = next;
            }
        }
        return false;
    }

    private int next(int[] nums, int index) {
        int n = nums.length;
        return ((index + nums[index]) % n + n) % n;
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(n)`。

#### 资深解法：快慢指针加原地清理

算法思想：每个下标看作链表节点，用快慢指针判环；必须保证方向一致且环长大于 1。处理完一个起点后，把同方向路径标记为 0，避免重复。

```java
class Solution {
    public boolean circularArrayLoop(int[] nums) {
        int n = nums.length;
        for (int i = 0; i < n; i++) {
            if (nums[i] == 0) continue;
            int slow = i, fast = next(nums, i);
            while (nums[fast] * nums[i] > 0 && nums[next(nums, fast)] * nums[i] > 0) {
                if (slow == fast) {
                    if (slow == next(nums, slow)) break;
                    return true;
                }
                slow = next(nums, slow);
                fast = next(nums, next(nums, fast));
            }
            int cur = i;
            while (nums[cur] * nums[i] > 0) {
                int next = next(nums, cur);
                nums[cur] = 0;
                cur = next;
            }
        }
        return false;
    }

    private int next(int[] nums, int index) {
        int n = nums.length;
        return ((index + nums[index]) % n + n) % n;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- Java 负数取模可能为负，所以用 `((x % n) + n) % n` 修正。
- 合法循环要求方向一致且长度大于 1。
- 核心思想：环形跳转数组可以当作函数图，用快慢指针判环。

---

## 458. 可怜的小猪 (Hard)

有 `buckets`  桶液体，其中  **正好有一桶**  含有毒药，其余装的都是水。它们从外观看起来都一样。为了弄清楚哪只水桶含有毒药，你可以喂一些猪喝，通过观察猪是否会死进行判断。不幸的是，你只有  `minutesToTest`  分钟时间来确定哪桶液体是有毒的。
喂猪的规则如下：

选择若干活猪进行喂养
可以允许小猪同时饮用任意数量的桶中的水，并且该过程不需要时间。
小猪喝完水后，必须有  `minutesToDie`  分钟的冷却时间。在这段时间里，你只能观察，而不允许继续喂猪。
过了  `minutesToDie`  分钟后，所有喝到毒药的猪都会死去，其他所有猪都会活下来。
重复这一过程，直到时间用完。

给你桶的数目  `buckets`  ， `minutesToDie`  和  `minutesToTest`  ，返回 在规定时间内判断哪个桶有毒所需的  **最小**  猪数 。
 
 **示例 1：** 

```text
输入：buckets = 1000, minutesToDie = 15, minutesToTest = 60
输出：5
```

 **示例 2：** 

```text
输入：buckets = 4, minutesToDie = 15, minutesToTest = 15
输出：2
```

 **示例 3：** 

```text
输入：buckets = 4, minutesToDie = 15, minutesToTest = 30
输出：2
```

 
 **提示：** 

 `1 <= buckets <= 1000` 
 `1 <= minutesToDie <= minutesToTest <= 100`

### Java 解法补充

#### 基础解法：逐个增加小猪数

算法思想：一只猪在测试时间内有 `rounds + 1` 种状态：第几轮死亡，或始终不死。`p` 只猪能区分 `(rounds + 1)^p` 个桶，逐个增加 `p` 直到覆盖所有桶。

```java
class Solution {
    public int poorPigs(int buckets, int minutesToDie, int minutesToTest) {
        int states = minutesToTest / minutesToDie + 1;
        int pigs = 0;
        int capacity = 1;
        while (capacity < buckets) {
            pigs++;
            capacity *= states;
        }
        return pigs;
    }
}
```

复杂度：时间 `O(answer)`，空间 `O(1)`。

#### 资深解法：对数计算最小猪数

算法思想：直接求最小 `p` 使 `states^p >= buckets`，可用对数近似后再微调，适合更大数值范围。

```java
class Solution {
    public int poorPigs(int buckets, int minutesToDie, int minutesToTest) {
        int states = minutesToTest / minutesToDie + 1;
        int pigs = (int) Math.ceil(Math.log(buckets) / Math.log(states));
        while (Math.pow(states, pigs) < buckets) pigs++;
        return pigs;
    }
}
```

复杂度：时间 `O(1)`，空间 `O(1)`。

#### 基础语法与算法思想

- 每只猪不是二元状态，而是有多个死亡轮次状态。
- `minutesToTest / minutesToDie` 是可观察的完整轮数。
- 核心思想：多只猪的状态组合数必须覆盖桶数。

---

## 459. 重复的子字符串 (Easy)

给定一个非空的字符串  `s`  ，检查是否可以通过由它的一个子串重复多次构成。
 
 **示例 1:** 

```text
输入: s = "abab"
输出: true
解释: 可由子串 "ab" 重复两次构成。
```

 **示例 2:** 

```text
输入: s = "aba"
输出: false
```

 **示例 3:** 

```text
输入: s = "abcabcabcabc"
输出: true
解释: 可由子串 "abc" 重复四次构成。 (或子串 "abcabc" 重复两次构成。)
```

 
 **提示：** 

 `1 <= s.length <= 104` 
 `s`  由小写英文字母组成

### Java 解法补充

#### 基础解法：枚举重复单元长度

算法思想：枚举可能的子串长度，只有能整除总长度的长度才可能成立。取前缀作为模式，重复拼接后与原串比较。

```java
class Solution {
    public boolean repeatedSubstringPattern(String s) {
        int n = s.length();
        for (int len = 1; len <= n / 2; len++) {
            if (n % len != 0) continue;
            String pattern = s.substring(0, len);
            StringBuilder builder = new StringBuilder();
            for (int i = 0; i < n / len; i++) builder.append(pattern);
            if (builder.toString().equals(s)) return true;
        }
        return false;
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(n)`。

#### 资深解法：拼接字符串判断

算法思想：如果 `s` 由某个子串重复构成，那么在 `(s + s)` 去掉首尾字符后仍能找到 `s`。

```java
class Solution {
    public boolean repeatedSubstringPattern(String s) {
        String doubled = s + s;
        return doubled.substring(1, doubled.length() - 1).contains(s);
    }
}
```

复杂度：时间 `O(n)` 到 `O(n^2)` 取决于 `contains` 实现，空间 `O(n)`。

#### 基础语法与算法思想

- 重复子串长度必须整除原字符串长度。
- `(s+s)` 技巧可以检测周期性字符串。
- 核心思想：周期串在双倍字符串的中间区域会再次出现自身。

---

## 460. LFU 缓存 (Hard)

请你为 最不经常使用（LFU）缓存算法设计并实现数据结构。
实现  `LFUCache`  类：

 `LFUCache(int capacity)`  - 用数据结构的容量  `capacity`  初始化对象
 `int get(int key)`  - 如果键  `key`  存在于缓存中，则获取键的值，否则返回  `-1`  。
 `void put(int key, int value)`  - 如果键  `key`  已存在，则变更其值；如果键不存在，请插入键值对。当缓存达到其容量  `capacity`  时，则应该在插入新项之前，移除最不经常使用的项。在此问题中，当存在平局（即两个或更多个键具有相同使用频率）时，应该去除  **最久未使用**  的键。

为了确定最不常使用的键，可以为缓存中的每个键维护一个  **使用计数器**  。使用计数最小的键是最久未使用的键。
当一个键首次插入到缓存中时，它的使用计数器被设置为  `1`  (由于 put 操作)。对缓存中的键执行  `get`  或  `put`  操作，使用计数器的值将会递增。
函数  `get`  和  `put`  必须以  `O(1)`  的平均时间复杂度运行。
 
 **示例：** 

```text
输入：
["LFUCache", "put", "put", "get", "put", "get", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [3], [4, 4], [1], [3], [4]]
输出：
[null, null, null, 1, null, -1, 3, null, -1, 3, 4]

解释：
// cnt(x) = 键 x 的使用计数
// cache=[] 将显示最后一次使用的顺序（最左边的元素是最近的）
LFUCache lfu = new LFUCache(2);
lfu.put(1, 1);   // cache=[1,_], cnt(1)=1
lfu.put(2, 2);   // cache=[2,1], cnt(2)=1, cnt(1)=1
lfu.get(1);      // 返回 1
                 // cache=[1,2], cnt(2)=1, cnt(1)=2
lfu.put(3, 3);   // 去除键 2 ，因为 cnt(2)=1 ，使用计数最小
                 // cache=[3,1], cnt(3)=1, cnt(1)=2
lfu.get(2);      // 返回 -1（未找到）
lfu.get(3);      // 返回 3
                 // cache=[3,1], cnt(3)=2, cnt(1)=2
lfu.put(4, 4);   // 去除键 1 ，1 和 3 的 cnt 相同，但 1 最久未使用
                 // cache=[4,3], cnt(4)=1, cnt(3)=2
lfu.get(1);      // 返回 -1（未找到）
lfu.get(3);      // 返回 3
                 // cache=[3,4], cnt(4)=1, cnt(3)=3
lfu.get(4);      // 返回 4
                 // cache=[3,4], cnt(4)=2, cnt(3)=3
```

 
 **提示：** 

 `1 <= capacity <= 104` 
 `0 <= key <= 105` 
 `0 <= value <= 109` 
最多调用  `2 * 105`  次  `get`  和  `put`  方法

### Java 解法补充

#### 基础解法：哈希表加时间戳扫描淘汰

算法思想：用哈希表保存值、频次和最后访问时间。容量满时扫描所有 key，淘汰频次最小且最久未使用的项。逻辑直观但淘汰是线性的。

```java
import java.util.HashMap;
import java.util.Map;

class LFUCache {
    private static class Entry {
        int value, freq, time;
        Entry(int value, int time) {
            this.value = value;
            this.freq = 1;
            this.time = time;
        }
    }

    private final int capacity;
    private int time = 0;
    private final Map<Integer, Entry> map = new HashMap<>();

    public LFUCache(int capacity) {
        this.capacity = capacity;
    }

    public int get(int key) {
        Entry entry = map.get(key);
        if (entry == null) return -1;
        entry.freq++;
        entry.time = ++time;
        return entry.value;
    }

    public void put(int key, int value) {
        if (capacity == 0) return;
        if (map.containsKey(key)) {
            Entry entry = map.get(key);
            entry.value = value;
            entry.freq++;
            entry.time = ++time;
            return;
        }
        if (map.size() == capacity) {
            int evict = -1;
            for (int k : map.keySet()) {
                Entry e = map.get(k);
                if (evict == -1 || e.freq < map.get(evict).freq
                        || (e.freq == map.get(evict).freq && e.time < map.get(evict).time)) {
                    evict = k;
                }
            }
            map.remove(evict);
        }
        map.put(key, new Entry(value, ++time));
    }
}
```

复杂度：`get` 时间 `O(1)`，`put` 最坏 `O(capacity)`，空间 `O(capacity)`。

#### 资深解法：频次到有序 key 集合

算法思想：用三个结构：`key -> value`、`key -> freq`、`freq -> LinkedHashSet<key>`。`LinkedHashSet` 保留同频次内的 LRU 顺序，`minFreq` 指向当前最小频次。

```java
import java.util.HashMap;
import java.util.LinkedHashSet;
import java.util.Map;

class LFUCache {
    private final int capacity;
    private int minFreq = 0;
    private final Map<Integer, Integer> values = new HashMap<>();
    private final Map<Integer, Integer> freqs = new HashMap<>();
    private final Map<Integer, LinkedHashSet<Integer>> groups = new HashMap<>();

    public LFUCache(int capacity) {
        this.capacity = capacity;
    }

    public int get(int key) {
        if (!values.containsKey(key)) return -1;
        touch(key);
        return values.get(key);
    }

    public void put(int key, int value) {
        if (capacity == 0) return;
        if (values.containsKey(key)) {
            values.put(key, value);
            touch(key);
            return;
        }
        if (values.size() == capacity) {
            LinkedHashSet<Integer> keys = groups.get(minFreq);
            int evict = keys.iterator().next();
            keys.remove(evict);
            values.remove(evict);
            freqs.remove(evict);
        }
        values.put(key, value);
        freqs.put(key, 1);
        groups.computeIfAbsent(1, x -> new LinkedHashSet<>()).add(key);
        minFreq = 1;
    }

    private void touch(int key) {
        int freq = freqs.get(key);
        LinkedHashSet<Integer> oldGroup = groups.get(freq);
        oldGroup.remove(key);
        if (oldGroup.isEmpty() && minFreq == freq) {
            minFreq++;
        }
        freqs.put(key, freq + 1);
        groups.computeIfAbsent(freq + 1, x -> new LinkedHashSet<>()).add(key);
    }
}
```

复杂度：`get/put` 平均时间 `O(1)`，空间 `O(capacity)`。

#### 基础语法与算法思想

- LFU 先比访问频次，频次相同再按 LRU 淘汰。
- `LinkedHashSet` 可以在同频次组内保留插入顺序，并支持 `O(1)` 删除。
- 核心思想：要做到 `O(1)`，必须直接定位 key 所在频次组，并维护当前最小频次。

---

## 461. 汉明距离 (Easy)

两个整数之间的 汉明距离 指的是这两个数字对应二进制位不同的位置的数目。
给你两个整数  `x`  和  `y` ，计算并返回它们之间的汉明距离。
 
 **示例 1：** 

```text
输入：x = 1, y = 4
输出：2
解释：
1   (0 0 0 1)
4   (0 1 0 0)
       ↑   ↑
上面的箭头指出了对应二进制位不同的位置。
```

 **示例 2：** 

```text
输入：x = 3, y = 1
输出：1
```

 
 **提示：** 

 `0 <= x, y <= 231 - 1` 

 
 **注意：** 本题与 2220. 转换数字的最少位翻转次数 相同。

### Java 解法补充

#### 基础解法：逐位比较

算法思想：逐位比较 `x` 和 `y` 的最低位是否相同，然后同时右移，统计不同的次数。

```java
class Solution {
    public int hammingDistance(int x, int y) {
        int ans = 0;
        while (x != 0 || y != 0) {
            if ((x & 1) != (y & 1)) ans++;
            x >>>= 1;
            y >>>= 1;
        }
        return ans;
    }
}
```

复杂度：时间 `O(32)`，空间 `O(1)`。

#### 资深解法：异或后计数

算法思想：`x ^ y` 的二进制中，1 的位置正是二者不同的位置，直接统计 1 的个数。

```java
class Solution {
    public int hammingDistance(int x, int y) {
        return Integer.bitCount(x ^ y);
    }
}
```

复杂度：时间 `O(1)`，空间 `O(1)`。

#### 基础语法与算法思想

- `& 1` 可以取最低位。
- 异或能标记两个数不同的二进制位。
- 核心思想：汉明距离就是异或结果中 1 的数量。

---

## 462. 最小操作次数使数组元素相等 II (Medium)

给你一个长度为  `n`  的整数数组  `nums`  ，返回使所有数组元素相等需要的最小操作数。
在一次操作中，你可以使数组中的一个元素加  `1`  或者减  `1`  。
测试用例经过设计以使答案在  **32 位**  整数范围内。
 
 **示例 1：** 

```text
输入：nums = [1,2,3]
输出：2
解释：
只需要两次操作（每次操作指南使一个元素加 1 或减 1）：
[1,2,3]  =>  [2,2,3]  =>  [2,2,2]
```

 **示例 2：** 

```text
输入：nums = [1,10,2,9]
输出：16
```

 
 **提示：** 

 `n == nums.length` 
 `1 <= nums.length <= 105` 
 `-109 <= nums[i] <= 109`

### Java 解法补充

#### 基础解法：枚举目标值

算法思想：所有数最终都变成同一个目标值。枚举最小值到最大值之间的目标，计算所有元素变过去的代价，取最小值。

```java
class Solution {
    public int minMoves2(int[] nums) {
        int min = nums[0], max = nums[0];
        for (int num : nums) {
            min = Math.min(min, num);
            max = Math.max(max, num);
        }
        int ans = Integer.MAX_VALUE;
        for (int target = min; target <= max; target++) {
            int cost = 0;
            for (int num : nums) cost += Math.abs(num - target);
            ans = Math.min(ans, cost);
        }
        return ans;
    }
}
```

复杂度：时间 `O(n * range)`，空间 `O(1)`。

#### 资深解法：排序取中位数

算法思想：绝对值距离和在中位数处最小。排序后用双指针从两端向中间收缩，累加差值。

```java
import java.util.Arrays;

class Solution {
    public int minMoves2(int[] nums) {
        Arrays.sort(nums);
        int left = 0, right = nums.length - 1;
        int ans = 0;
        while (left < right) {
            ans += nums[right] - nums[left];
            left++;
            right--;
        }
        return ans;
    }
}
```

复杂度：时间 `O(n log n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 变成同一个数的总代价是绝对差之和。
- 中位数最小化绝对距离和。
- 核心思想：成对看最小和最大，它们最终向中间靠拢代价最小。

---

## 463. 岛屿的周长 (Easy)

给定一个  `row x col`  的二维网格地图  `grid`  ，其中： `grid[i][j] = 1`  表示陆地，  `grid[i][j] = 0`  表示水域。
网格中的格子  **水平和垂直**  方向相连（对角线方向不相连）。整个网格被水完全包围，但其中恰好有一个岛屿（或者说，一个或多个表示陆地的格子相连组成的岛屿）。
岛屿中没有“湖”（“湖” 指水域在岛屿内部且不和岛屿周围的水相连）。格子是边长为 1 的正方形。网格为长方形，且宽度和高度均不超过 100 。计算这个岛屿的周长。
 
 **示例 1：** 

```text
输入：grid = [[0,1,0,0],[1,1,1,0],[0,1,0,0],[1,1,0,0]]
输出：16
解释：它的周长是上面图片中的 16 个黄色的边
```

 **示例 2：** 

```text
输入：grid = [[1]]
输出：4
```

 **示例 3：** 

```text
输入：grid = [[1,0]]
输出：4
```

 
 **提示：** 

 `row == grid.length` 
 `col == grid[i].length` 
 `1 <= row, col <= 100` 
 `grid[i][j]`  为  `0`  或  `1`

### Java 解法补充

#### 基础解法：每个陆地检查四边

算法思想：每个陆地格子最多贡献 4 条边。四个方向中如果越界或相邻是水，就贡献 1 条周长。

```java
class Solution {
    public int islandPerimeter(int[][] grid) {
        int ans = 0;
        int[][] dirs = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (grid[i][j] == 0) continue;
                for (int[] dir : dirs) {
                    int x = i + dir[0], y = j + dir[1];
                    if (x < 0 || x >= grid.length || y < 0 || y >= grid[0].length || grid[x][y] == 0) {
                        ans++;
                    }
                }
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(mn)`，空间 `O(1)`。

#### 资深解法：陆地数和相邻边数

算法思想：每个陆地先贡献 4，若它和上方或左方陆地相邻，则这条公共边会让总周长减少 2。

```java
class Solution {
    public int islandPerimeter(int[][] grid) {
        int perimeter = 0;
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (grid[i][j] == 1) {
                    perimeter += 4;
                    if (i > 0 && grid[i - 1][j] == 1) perimeter -= 2;
                    if (j > 0 && grid[i][j - 1] == 1) perimeter -= 2;
                }
            }
        }
        return perimeter;
    }
}
```

复杂度：时间 `O(mn)`，空间 `O(1)`。

#### 基础语法与算法思想

- 公共边会被两个陆地格子各少贡献一条边，所以减 2。
- 只看上方和左方可以避免重复统计相邻关系。
- 核心思想：网格周长等于陆地贡献减去内部公共边抵消。

---

## 464. 我能赢吗 (Medium)

在 "100 game" 这个游戏中，两名玩家轮流选择从  `1`  到  `10`  的任意整数，累计整数和，先使得累计整数和  **达到或超过**   100 的玩家，即为胜者。
如果我们将游戏规则改为 “玩家  **不能**  重复使用整数” 呢？
例如，两个玩家可以轮流从公共整数池中抽取从 1 到 15 的整数（不放回），直到累计整数和 >= 100。
给定两个整数  `maxChoosableInteger`  （整数池中可选择的最大数）和  `desiredTotal` （累计和），若先出手的玩家能稳赢则返回  `true`  ，否则返回  `false`  。假设两位玩家游戏时都表现  **最佳**  。
 
 **示例 1：** 

```text
输入：maxChoosableInteger = 10, desiredTotal = 11
输出：false
解释：
无论第一个玩家选择哪个整数，他都会失败。
第一个玩家可以选择从 1 到 10 的整数。
如果第一个玩家选择 1，那么第二个玩家只能选择从 2 到 10 的整数。
第二个玩家可以通过选择整数 10（那么累积和为 11 >= desiredTotal），从而取得胜利.
同样地，第一个玩家选择任意其他整数，第二个玩家都会赢。
```

 **示例 2:** 

```text
输入：maxChoosableInteger = 10, desiredTotal = 0
输出：true
```

 **示例 3:** 

```text
输入：maxChoosableInteger = 10, desiredTotal = 1
输出：true
```

 
 **提示:** 

 `1 <= maxChoosableInteger <= 20` 
 `0 <= desiredTotal <= 300`

### Java 解法补充

#### 基础解法：回溯尝试所有选择

算法思想：当前玩家枚举每个还没用过的数字。如果选它能直接达到目标，或让对手进入必败状态，则当前玩家能赢。

```java
class Solution {
    public boolean canIWin(int maxChoosableInteger, int desiredTotal) {
        boolean[] used = new boolean[maxChoosableInteger + 1];
        return dfs(maxChoosableInteger, desiredTotal, used);
    }

    private boolean dfs(int max, int remain, boolean[] used) {
        if (remain <= 0) return false;
        for (int i = 1; i <= max; i++) {
            if (!used[i]) {
                used[i] = true;
                boolean win = !dfs(max, remain - i, used);
                used[i] = false;
                if (win) return true;
            }
        }
        return false;
    }
}
```

复杂度：时间指数级，空间 `O(maxChoosableInteger)`。

#### 资深解法：状态压缩记忆化

算法思想：用 bitmask 表示哪些数字已经被使用，同一个使用状态下剩余目标唯一。记忆化避免重复搜索。

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    private final Map<Integer, Boolean> memo = new HashMap<>();

    public boolean canIWin(int maxChoosableInteger, int desiredTotal) {
        int sum = (1 + maxChoosableInteger) * maxChoosableInteger / 2;
        if (desiredTotal <= 0) return true;
        if (sum < desiredTotal) return false;
        return dfs(maxChoosableInteger, desiredTotal, 0);
    }

    private boolean dfs(int max, int remain, int usedMask) {
        if (memo.containsKey(usedMask)) return memo.get(usedMask);
        for (int i = 1; i <= max; i++) {
            int bit = 1 << (i - 1);
            if ((usedMask & bit) == 0) {
                if (i >= remain || !dfs(max, remain - i, usedMask | bit)) {
                    memo.put(usedMask, true);
                    return true;
                }
            }
        }
        memo.put(usedMask, false);
        return false;
    }
}
```

复杂度：时间 `O(2^m * m)`，空间 `O(2^m)`。

#### 基础语法与算法思想

- 博弈搜索中，“我能让对手输”就是我赢。
- bitmask 可以压缩布尔使用数组。
- 核心思想：状态由已选数字集合决定，适合记忆化。

---

## 465. 最优账单平衡 (Hard)

暂无内容描述。

### Java 解法补充

#### 基础解法：整理净余额后回溯

算法思想：先把每个人的净收支算出来，只保留非零余额。每次选择第一个未结清的人，尝试和后面异号的人结算。

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

class Solution {
    public int minTransfers(int[][] transactions) {
        Map<Integer, Integer> balance = new HashMap<>();
        for (int[] t : transactions) {
            balance.put(t[0], balance.getOrDefault(t[0], 0) - t[2]);
            balance.put(t[1], balance.getOrDefault(t[1], 0) + t[2]);
        }
        List<Integer> debts = new ArrayList<>();
        for (int value : balance.values()) {
            if (value != 0) debts.add(value);
        }
        return dfs(debts, 0);
    }

    private int dfs(List<Integer> debts, int index) {
        while (index < debts.size() && debts.get(index) == 0) index++;
        if (index == debts.size()) return 0;
        int ans = Integer.MAX_VALUE;
        for (int i = index + 1; i < debts.size(); i++) {
            if (debts.get(index) * debts.get(i) < 0) {
                debts.set(i, debts.get(i) + debts.get(index));
                ans = Math.min(ans, 1 + dfs(debts, index + 1));
                debts.set(i, debts.get(i) - debts.get(index));
            }
        }
        return ans;
    }
}
```

复杂度：最坏指数级，空间 `O(n)`。

#### 资深解法：回溯剪枝

算法思想：同样回溯结算净余额，但当某次结算刚好清零时可以提前停止，减少等价分支。

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

class Solution {
    public int minTransfers(int[][] transactions) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int[] t : transactions) {
            map.put(t[0], map.getOrDefault(t[0], 0) - t[2]);
            map.put(t[1], map.getOrDefault(t[1], 0) + t[2]);
        }
        List<Integer> debt = new ArrayList<>();
        for (int v : map.values()) if (v != 0) debt.add(v);
        return settle(debt, 0);
    }

    private int settle(List<Integer> debt, int start) {
        while (start < debt.size() && debt.get(start) == 0) start++;
        if (start == debt.size()) return 0;
        int ans = Integer.MAX_VALUE;
        for (int i = start + 1; i < debt.size(); i++) {
            if (debt.get(start) * debt.get(i) >= 0) continue;
            int original = debt.get(i);
            debt.set(i, original + debt.get(start));
            ans = Math.min(ans, 1 + settle(debt, start + 1));
            debt.set(i, original);
            if (original + debt.get(start) == 0) break;
        }
        return ans;
    }
}
```

复杂度：最坏指数级，剪枝后实际规模通常较小。

#### 基础语法与算法思想

- 多笔交易先归并成每个人的净余额。
- 只需要处理非零余额的人。
- 核心思想：最少转账数取决于如何把正负净额配对抵消。

---

## 466. 统计重复个数 (Hard)

定义  `str = [s, n]`  表示  `str`  由  `n`  个字符串  `s`  连接构成。

例如， `str == ["abc", 3] =="abcabcabc"`  。

如果可以从  `s2`  中删除某些字符使其变为  `s1` ，则称字符串  `s1`  可以从字符串  `s2`  获得。

例如，根据定义， `s1 = "abc"`  可以从  `s2 = "abdbec"`  获得，仅需要删除加粗且用斜体标识的字符。

现在给你两个字符串  `s1`  和  `s2`  和两个整数  `n1`  和  `n2`  。由此构造得到两个字符串，其中  `str1 = [s1, n1]` 、 `str2 = [s2, n2]`  。
请你找出一个最大整数  `m`  ，以满足  `str = [str2, m]`  可以从  `str1`  获得。
 
 **示例 1：** 

```text
输入：s1 = "acb", n1 = 4, s2 = "ab", n2 = 2
输出：2
```

 **示例 2：** 

```text
输入：s1 = "acb", n1 = 1, s2 = "acb", n2 = 1
输出：1
```

 
 **提示：** 

 `1 <= s1.length, s2.length <= 100` 
 `s1`  和  `s2`  由小写英文字母组成
 `1 <= n1, n2 <= 106`

### Java 解法补充

#### 基础解法：直接模拟匹配

算法思想：把 `s1` 重复扫描 `n1` 次，用指针 `j` 匹配 `s2`。每当 `j` 走到 `s2.length()`，说明从当前扫描过的字符里获得了一个 `s2`，计数加一并把 `j` 清零。最后完整获得的 `s2` 数量除以 `n2`。

```java
class Solution {
    public int getMaxRepetitions(String s1, int n1, String s2, int n2) {
        int index = 0;
        int countS2 = 0;

        for (int round = 0; round < n1; round++) {
            for (int i = 0; i < s1.length(); i++) {
                if (s1.charAt(i) == s2.charAt(index)) {
                    index++;
                    if (index == s2.length()) {
                        index = 0;
                        countS2++;
                    }
                }
            }
        }

        return countS2 / n2;
    }
}
```

复杂度：时间 `O(n1 * s1.length())`，空间 `O(1)`。

#### 资深解法：循环节加速

算法思想：每扫描完一轮 `s1` 后，状态只由 `s2` 当前匹配下标决定。如果同一个下标再次出现，说明之后会按固定循环增加 `s1` 轮数和 `s2` 个数，可以一次跳过多个循环。

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int getMaxRepetitions(String s1, int n1, String s2, int n2) {
        Map<Integer, int[]> seen = new HashMap<>();
        int index = 0;
        int countS2 = 0;
        int round = 0;

        while (round < n1) {
            round++;
            for (int i = 0; i < s1.length(); i++) {
                if (s1.charAt(i) == s2.charAt(index)) {
                    index++;
                    if (index == s2.length()) {
                        index = 0;
                        countS2++;
                    }
                }
            }

            if (seen.containsKey(index)) {
                int[] prev = seen.get(index);
                int cycleRounds = round - prev[0];
                int cycleCount = countS2 - prev[1];
                int remain = n1 - round;
                int times = remain / cycleRounds;
                round += times * cycleRounds;
                countS2 += times * cycleCount;
            } else {
                seen.put(index, new int[]{round, countS2});
            }
        }

        return countS2 / n2;
    }
}
```

复杂度：时间接近 `O((前缀轮数 + 循环剩余轮数) * s1.length())`，空间 `O(s2.length())`。

#### 基础语法与算法思想

- 子序列匹配只需要一个指针，不需要真正构造重复后的长字符串。
- `Map<Integer, int[]>` 可以记录某个状态第一次出现时的轮数和累计数量。
- 核心思想：重复字符串题要关注“扫描完一轮后的状态”，相同状态通常意味着循环节。

---

## 467. 环绕字符串中唯一的子字符串 (Medium)

定义字符串  `base`  为一个  `"abcdefghijklmnopqrstuvwxyz"`  无限环绕的字符串，所以  `base`  看起来是这样的：

 `"...zabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcd...."` .

给你一个字符串  `s`  ，请你统计并返回  `s`  中有多少  **不同**  **非空子串**  也在  `base`  中出现。
 
 **示例 1：** 

```text
输入：s = "a"
输出：1
解释：字符串 s 的子字符串 "a" 在 base 中出现。
```

 **示例 2：** 

```text
输入：s = "cac"
输出：2
解释：字符串 s 有两个子字符串 ("a", "c") 在 base 中出现。
```

 **示例 3：** 

```text
输入：s = "zab"
输出：6
解释：字符串 s 有六个子字符串 ("z", "a", "b", "za", "ab", and "zab") 在 base 中出现。
```

 
 **提示：** 

 `1 <= s.length <= 105` 
s 由小写英文字母组成

### Java 解法补充

#### 基础解法：枚举合法环绕子串

算法思想：枚举每个起点，向右扩展时检查相邻字符是否满足环绕连续关系。合法子串放入 `HashSet` 去重，最后集合大小就是答案。

```java
import java.util.HashSet;
import java.util.Set;

class Solution {
    public int findSubstringInWraproundString(String s) {
        Set<String> seen = new HashSet<>();

        for (int left = 0; left < s.length(); left++) {
            StringBuilder cur = new StringBuilder();
            for (int right = left; right < s.length(); right++) {
                if (right > left && !isNext(s.charAt(right - 1), s.charAt(right))) {
                    break;
                }
                cur.append(s.charAt(right));
                seen.add(cur.toString());
            }
        }

        return seen.size();
    }

    private boolean isNext(char a, char b) {
        return (a == 'z' && b == 'a') || b - a == 1;
    }
}
```

复杂度：时间最坏 `O(n^2)`，空间 `O(n^2)`。

#### 资深解法：记录每个结尾字符的最长长度

算法思想：同一个结尾字符下，长度较短的合法子串都会被最长合法子串包含。例如以 `c` 结尾的最长长度是 4，就贡献 4 个不同子串。维护 26 个字符作为结尾时的最大连续长度并求和。

```java
class Solution {
    public int findSubstringInWraproundString(String s) {
        int[] best = new int[26];
        int len = 0;

        for (int i = 0; i < s.length(); i++) {
            if (i > 0 && isNext(s.charAt(i - 1), s.charAt(i))) {
                len++;
            } else {
                len = 1;
            }
            int index = s.charAt(i) - 'a';
            best[index] = Math.max(best[index], len);
        }

        int ans = 0;
        for (int value : best) {
            ans += value;
        }
        return ans;
    }

    private boolean isNext(char a, char b) {
        return (a == 'z' && b == 'a') || b - a == 1;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- `HashSet` 用于去重，但保存大量字符串会比较重。
- `s.charAt(i) - 'a'` 可以把小写字母映射到 `0..25`。
- 核心思想：统计不同子串时，如果能用“最长覆盖较短”的性质，就能避免显式存所有子串。

---

## 468. 验证IP地址 (Medium)

给定一个字符串  `queryIP` 。如果是有效的 IPv4 地址，返回  `"IPv4"`  ；如果是有效的 IPv6 地址，返回  `"IPv6"`  ；如果不是上述类型的 IP 地址，返回  `"Neither"`  。
 **有效的IPv4地址**  是  `“x1.x2.x3.x4”`  形式的IP地址。 其中  `0 <= xi <= 255`  且  `xi`   **不能包含**  前导零。例如:  `“192.168.1.1”`  、  `“192.168.1.0”`  为有效IPv4地址，  `“192.168.01.1”`  为无效IPv4地址;  `“192.168.1.00”`  、  `“192.168@1.1”`  为无效IPv4地址。
 **一个有效的IPv6地址** 是一个格式为 `“x1:x2:x3:x4:x5:x6:x7:x8”`  的IP地址，其中:

 `1 <= xi.length <= 4` 
 `xi`  是一个  **十六进制字符串**  ，可以包含数字、小写英文字母(  `'a'`  到  `'f'`  )和大写英文字母(  `'A'`  到  `'F'`  )。
在  `xi`  中允许前导零。

例如  `"2001:0db8:85a3:0000:0000:8a2e:0370:7334"`  和  `"2001:db8:85a3:0:0:8A2E:0370:7334"`  是有效的 IPv6 地址，而  `"2001:0db8:85a3::8A2E:037j:7334"`  和  `"02001:0db8:85a3:0000:0000:8a2e:0370:7334"`  是无效的 IPv6 地址。
 
 **示例 1：** 

```text
输入：queryIP = "172.16.254.1"
输出："IPv4"
解释：有效的 IPv4 地址，返回 "IPv4"
```

 **示例 2：** 

```text
输入：queryIP = "2001:0db8:85a3:0:0:8A2E:0370:7334"
输出："IPv6"
解释：有效的 IPv6 地址，返回 "IPv6"
```

 **示例 3：** 

```text
输入：queryIP = "256.256.256.256"
输出："Neither"
解释：既不是 IPv4 地址，又不是 IPv6 地址
```

 
 **提示：** 

 `queryIP`  仅由英文字母，数字，字符  `'.'`  和  `':'`  组成。

### Java 解法补充

#### 基础解法：分隔后逐段校验

算法思想：如果字符串包含 `.`，按 IPv4 规则校验 4 段；如果包含 `:`，按 IPv6 规则校验 8 段。使用 `split(..., -1)` 保留空段，方便识别连续分隔符或首尾分隔符。

```java
class Solution {
    public String validIPAddress(String queryIP) {
        if (queryIP.indexOf('.') >= 0) {
            String[] parts = queryIP.split("\\.", -1);
            if (parts.length != 4) return "Neither";
            for (String part : parts) {
                if (!validIPv4Part(part)) return "Neither";
            }
            return "IPv4";
        }

        if (queryIP.indexOf(':') >= 0) {
            String[] parts = queryIP.split(":", -1);
            if (parts.length != 8) return "Neither";
            for (String part : parts) {
                if (!validIPv6Part(part)) return "Neither";
            }
            return "IPv6";
        }

        return "Neither";
    }

    private boolean validIPv4Part(String s) {
        if (s.length() == 0 || s.length() > 3) return false;
        if (s.length() > 1 && s.charAt(0) == '0') return false;
        int value = 0;
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (c < '0' || c > '9') return false;
            value = value * 10 + c - '0';
        }
        return value <= 255;
    }

    private boolean validIPv6Part(String s) {
        if (s.length() == 0 || s.length() > 4) return false;
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            boolean digit = c >= '0' && c <= '9';
            boolean lower = c >= 'a' && c <= 'f';
            boolean upper = c >= 'A' && c <= 'F';
            if (!digit && !lower && !upper) return false;
        }
        return true;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 资深解法：先排除混合分隔符并复用校验器

算法思想：真实解析场景里先判断协议形态，IPv4 不能含 `:`，IPv6 不能含 `.`。之后把段校验拆成清晰的工具方法，避免 `Integer.parseInt` 的异常分支参与常规流程。

```java
class Solution {
    public String validIPAddress(String queryIP) {
        if (queryIP.indexOf('.') >= 0 && queryIP.indexOf(':') < 0) {
            return isIPv4(queryIP) ? "IPv4" : "Neither";
        }
        if (queryIP.indexOf(':') >= 0 && queryIP.indexOf('.') < 0) {
            return isIPv6(queryIP) ? "IPv6" : "Neither";
        }
        return "Neither";
    }

    private boolean isIPv4(String ip) {
        String[] parts = ip.split("\\.", -1);
        if (parts.length != 4) return false;
        for (String part : parts) {
            if (part.length() == 0 || part.length() > 3) return false;
            if (part.length() > 1 && part.charAt(0) == '0') return false;
            int value = 0;
            for (char c : part.toCharArray()) {
                if (!Character.isDigit(c)) return false;
                value = value * 10 + c - '0';
            }
            if (value > 255) return false;
        }
        return true;
    }

    private boolean isIPv6(String ip) {
        String[] parts = ip.split(":", -1);
        if (parts.length != 8) return false;
        for (String part : parts) {
            if (part.length() == 0 || part.length() > 4) return false;
            for (char c : part.toCharArray()) {
                if (!isHex(c)) return false;
            }
        }
        return true;
    }

    private boolean isHex(char c) {
        return Character.isDigit(c) ||
                (c >= 'a' && c <= 'f') ||
                (c >= 'A' && c <= 'F');
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 基础语法与算法思想

- `split("\\.", -1)` 中 `.` 是正则特殊字符，要写成 `\\.`。
- `split(..., -1)` 会保留空字符串，适合校验格式。
- 核心思想：格式校验题先判整体形态，再校验每一段的局部规则。

---

## 469. 凸多边形 (Medium)

暂无内容描述。

### Java 解法补充

#### 基础解法：逐组三点判断转向

算法思想：按多边形顶点顺序，每次取连续三个点，计算叉积判断转向方向。凸多边形的所有非零转向方向必须一致。

```java
import java.util.List;

class Solution {
    public boolean isConvex(List<List<Integer>> points) {
        int n = points.size();
        int sign = 0;

        for (int i = 0; i < n; i++) {
            List<Integer> a = points.get(i);
            List<Integer> b = points.get((i + 1) % n);
            List<Integer> c = points.get((i + 2) % n);
            int cross = cross(a, b, c);
            if (cross != 0) {
                if (sign == 0) {
                    sign = cross > 0 ? 1 : -1;
                } else if ((cross > 0 ? 1 : -1) != sign) {
                    return false;
                }
            }
        }

        return true;
    }

    private int cross(List<Integer> a, List<Integer> b, List<Integer> c) {
        int x1 = b.get(0) - a.get(0);
        int y1 = b.get(1) - a.get(1);
        int x2 = c.get(0) - b.get(0);
        int y2 = c.get(1) - b.get(1);
        return x1 * y2 - y1 * x2;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 资深解法：使用 long 规避坐标乘法溢出

算法思想：业务实现里几何计算容易被坐标范围放大，叉积统一用 `long` 更稳。流程仍然是维护第一个非零叉积方向，后续方向必须一致。

```java
import java.util.List;

class Solution {
    public boolean isConvex(List<List<Integer>> points) {
        int n = points.size();
        int direction = 0;

        for (int i = 0; i < n; i++) {
            long cross = cross(points.get(i), points.get((i + 1) % n), points.get((i + 2) % n));
            if (cross == 0) {
                continue;
            }
            int current = cross > 0 ? 1 : -1;
            if (direction == 0) {
                direction = current;
            } else if (direction != current) {
                return false;
            }
        }

        return true;
    }

    private long cross(List<Integer> a, List<Integer> b, List<Integer> c) {
        long abx = b.get(0) - a.get(0);
        long aby = b.get(1) - a.get(1);
        long bcx = c.get(0) - b.get(0);
        long bcy = c.get(1) - b.get(1);
        return abx * bcy - aby * bcx;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- `(i + 1) % n` 用来让最后几个点继续回到开头，形成环。
- 叉积正负表示两个向量的转向方向。
- 核心思想：凸多边形沿边走时不能一会左转、一会右转。

---

## 470. 用 Rand7() 实现 Rand10() (Medium)

给定方法  `rand7`  可生成  `[1,7]`  范围内的均匀随机整数，试写一个方法  `rand10`  生成  `[1,10]`  范围内的均匀随机整数。
你只能调用  `rand7()`  且不能调用其他方法。请不要使用系统的  `Math.random()`  方法。

每个测试用例将有一个内部参数  `n` ，即你实现的函数  `rand10()`  在测试时将被调用的次数。请注意，这不是传递给  `rand10()`  的参数。
 
 **示例 1:** 

```text
输入: 1
输出: [2]
```

 **示例 2:** 

```text
输入: 2
输出: [2,8]
```

 **示例 3:** 

```text
输入: 3
输出: [3,8,10]
```

 
 **提示:** 

 `1 <= n <= 105` 

 
 **进阶:** 

 `rand7()` 调用次数的 期望值 是多少 ?
你能否尽量少调用  `rand7()`  ?

### Java 解法补充

#### 基础解法：49 个状态拒绝采样

算法思想：两次调用 `rand7()` 可以生成 `1..49` 的均匀整数。只接受前 40 个状态，因为 40 可以被 10 整除；落在 41 到 49 就重试。

```java
class Solution extends SolBase {
    public int rand10() {
        while (true) {
            int row = rand7();
            int col = rand7();
            int num = (row - 1) * 7 + col;
            if (num <= 40) {
                return (num - 1) % 10 + 1;
            }
        }
    }
}
```

复杂度：期望时间 `O(1)`，空间 `O(1)`。

#### 资深解法：复用被拒绝的剩余随机状态

算法思想：第一次得到 `1..49`，超过 40 的 9 个状态仍然均匀，可以和一次 `rand7()` 组合成 `1..63`；超过 60 的 3 个状态还能再组合成 `1..21`。这样减少期望调用次数。

```java
class Solution extends SolBase {
    public int rand10() {
        while (true) {
            int num = (rand7() - 1) * 7 + rand7();
            if (num <= 40) {
                return (num - 1) % 10 + 1;
            }

            num = (num - 40 - 1) * 7 + rand7();
            if (num <= 60) {
                return (num - 1) % 10 + 1;
            }

            num = (num - 60 - 1) * 7 + rand7();
            if (num <= 20) {
                return (num - 1) % 10 + 1;
            }
        }
    }
}
```

复杂度：期望时间 `O(1)`，空间 `O(1)`。

#### 基础语法与算法思想

- 拒绝采样的关键是只接受能均分到目标结果的状态数。
- `(num - 1) % 10 + 1` 把连续整数均匀映射到 `1..10`。
- 核心思想：随机题要保证每个结果的概率完全相等，不能简单取模一个非 10 倍数的区间。

---

## 471. 编码最短长度的字符串 (Hard)

暂无内容描述。

### Java 解法补充

#### 基础解法：区间动态规划

算法思想：`dp[i][j]` 表示子串 `s[i..j]` 的最短编码。一个区间可以由左右两段拼接得到，也可以在自身由某个更短模式重复构成时写成 `k[pattern]`。

```java
class Solution {
    public String encode(String s) {
        int n = s.length();
        String[][] dp = new String[n][n];

        for (int len = 1; len <= n; len++) {
            for (int i = 0; i + len <= n; i++) {
                int j = i + len - 1;
                dp[i][j] = s.substring(i, j + 1);

                for (int k = i; k < j; k++) {
                    String candidate = dp[i][k] + dp[k + 1][j];
                    if (candidate.length() < dp[i][j].length()) {
                        dp[i][j] = candidate;
                    }
                }

                for (int unit = 1; unit <= len / 2; unit++) {
                    if (len % unit == 0 && repeated(s, i, j, unit)) {
                        String candidate = len / unit + "[" + dp[i][i + unit - 1] + "]";
                        if (candidate.length() < dp[i][j].length()) {
                            dp[i][j] = candidate;
                        }
                    }
                }
            }
        }

        return dp[0][n - 1];
    }

    private boolean repeated(String s, int left, int right, int unit) {
        for (int i = left + unit; i <= right; i++) {
            if (s.charAt(i) != s.charAt(left + (i - left) % unit)) {
                return false;
            }
        }
        return true;
    }
}
```

复杂度：时间 `O(n^4)`，空间 `O(n^2)`。

#### 资深解法：用重复子串定位优化判断

算法思想：仍然使用区间 DP，但判断一个字符串是否由更短模式重复时，利用 `(sub + sub).indexOf(sub, 1)` 找到最小循环节位置，减少手写重复判断的复杂度。

```java
class Solution {
    public String encode(String s) {
        int n = s.length();
        String[][] dp = new String[n][n];

        for (int len = 1; len <= n; len++) {
            for (int left = 0; left + len <= n; left++) {
                int right = left + len - 1;
                String sub = s.substring(left, right + 1);
                dp[left][right] = sub;

                for (int mid = left; mid < right; mid++) {
                    String joined = dp[left][mid] + dp[mid + 1][right];
                    if (joined.length() < dp[left][right].length()) {
                        dp[left][right] = joined;
                    }
                }

                int pos = (sub + sub).indexOf(sub, 1);
                if (pos < sub.length() && len % pos == 0) {
                    String encoded = len / pos + "[" + dp[left][left + pos - 1] + "]";
                    if (encoded.length() < dp[left][right].length()) {
                        dp[left][right] = encoded;
                    }
                }
            }
        }

        return dp[0][n - 1];
    }
}
```

复杂度：时间 `O(n^3)` 级别，空间 `O(n^2)`。

#### 基础语法与算法思想

- `dp[i][j]` 是字符串区间题的常见定义。
- `k[pattern]` 只有在编码后更短时才应该替换原字符串。
- 核心思想：最短编码既可能来自整体重复，也可能来自左右两个最优编码拼接。

---

## 472. 连接词 (Hard)

给你一个  **不含重复** 单词的字符串数组  `words`  ，请你找出并返回  `words`  中的所有  **连接词**  。
 **连接词**  定义为：一个完全由给定数组中的至少两个较短单词（不一定是不同的两个单词）组成的字符串。
 
 **示例 1：** 

```text
输入：words = ["cat","cats","catsdogcats","dog","dogcatsdog","hippopotamuses","rat","ratcatdogcat"]
输出：["catsdogcats","dogcatsdog","ratcatdogcat"]
解释："catsdogcats" 由 "cats", "dog" 和 "cats" 组成; 
     "dogcatsdog" 由 "dog", "cats" 和 "dog" 组成; 
     "ratcatdogcat" 由 "rat", "cat", "dog" 和 "cat" 组成。
```

 **示例 2：** 

```text
输入：words = ["cat","dog","catdog"]
输出：["catdog"]
```

 
 **提示：** 

 `1 <= words.length <= 104` 
 `1 <= words[i].length <= 30` 
 `words[i]`  仅由小写英文字母组成。 
 `words`  中的所有字符串都是  **唯一**  的。
 `1 <= sum(words[i].length) <= 105`

### Java 解法补充

#### 基础解法：逐词 DFS 拆分

算法思想：把所有单词放入集合。判断某个单词时，从每个切分点尝试取前缀，如果前缀在集合中，就递归判断后缀能否继续拆分。通过 `count` 保证至少由两个单词组成。

```java
import java.util.ArrayList;
import java.util.HashSet;
import java.util.List;
import java.util.Set;

class Solution {
    private Set<String> dict;

    public List<String> findAllConcatenatedWordsInADict(String[] words) {
        dict = new HashSet<>();
        for (String word : words) {
            dict.add(word);
        }

        List<String> ans = new ArrayList<>();
        for (String word : words) {
            if (dfs(word, 0, 0)) {
                ans.add(word);
            }
        }
        return ans;
    }

    private boolean dfs(String word, int start, int count) {
        if (start == word.length()) {
            return count >= 2;
        }
        for (int end = start + 1; end <= word.length(); end++) {
            String part = word.substring(start, end);
            if (dict.contains(part) && dfs(word, end, count + 1)) {
                return true;
            }
        }
        return false;
    }
}
```

复杂度：时间最坏指数级，空间 `O(totalLength)`。

#### 资深解法：按长度排序加单词拆分 DP

算法思想：按长度从短到长处理单词。判断当前单词时，集合中只包含更短单词，天然避免把自己当作组成部分。用一维 DP 判断能否被已有词拼出。

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashSet;
import java.util.List;
import java.util.Set;

class Solution {
    public List<String> findAllConcatenatedWordsInADict(String[] words) {
        Arrays.sort(words, (a, b) -> a.length() - b.length());
        Set<String> dict = new HashSet<>();
        List<String> ans = new ArrayList<>();

        for (String word : words) {
            if (word.length() == 0) {
                continue;
            }
            if (canBuild(word, dict)) {
                ans.add(word);
            }
            dict.add(word);
        }

        return ans;
    }

    private boolean canBuild(String word, Set<String> dict) {
        if (dict.isEmpty()) return false;
        boolean[] dp = new boolean[word.length() + 1];
        dp[0] = true;

        for (int i = 1; i <= word.length(); i++) {
            for (int j = 0; j < i; j++) {
                if (dp[j] && dict.contains(word.substring(j, i))) {
                    dp[i] = true;
                    break;
                }
            }
        }

        return dp[word.length()];
    }
}
```

复杂度：时间 `O(sumLen * maxLen)` 级别，空间 `O(totalLength)`。

#### 基础语法与算法思想

- `Set<String>` 适合做字典查询。
- 排序后只用更短单词构造当前单词，可以避免自引用。
- 核心思想：连接词本质是 Word Break，只是需要对每个候选词运行一次。

---

## 473. 火柴拼正方形 (Medium)

你将得到一个整数数组  `matchsticks`  ，其中  `matchsticks[i]`  是第  `i`  个火柴棒的长度。你要用  **所有的火柴棍**  拼成一个正方形。你  **不能折断**  任何一根火柴棒，但你可以把它们连在一起，而且每根火柴棒必须  **使用一次**  。
如果你能使这个正方形，则返回  `true`  ，否则返回  `false`  。
 
 **示例 1:** 

```text
输入: matchsticks = [1,1,2,2,2]
输出: true
解释: 能拼成一个边长为2的正方形，每边两根火柴。
```

 **示例 2:** 

```text
输入: matchsticks = [3,3,3,3,4]
输出: false
解释: 不能用所有火柴拼成一个正方形。
```

 
 **提示:** 

 `1 <= matchsticks.length <= 15` 
 `1 <= matchsticks[i] <= 108`

### Java 解法补充

#### 基础解法：回溯分配四条边

算法思想：先求总长度，若不能被 4 整除直接返回 `false`。之后逐根火柴尝试放入四条边，只要某条边不超过目标边长就继续递归。

```java
class Solution {
    public boolean makesquare(int[] matchsticks) {
        int sum = 0;
        for (int stick : matchsticks) {
            sum += stick;
        }
        if (sum % 4 != 0) {
            return false;
        }

        int[] sides = new int[4];
        return dfs(matchsticks, 0, sides, sum / 4);
    }

    private boolean dfs(int[] matchsticks, int index, int[] sides, int target) {
        if (index == matchsticks.length) {
            return sides[0] == target && sides[1] == target &&
                    sides[2] == target && sides[3] == target;
        }

        for (int i = 0; i < 4; i++) {
            if (sides[i] + matchsticks[index] <= target) {
                sides[i] += matchsticks[index];
                if (dfs(matchsticks, index + 1, sides, target)) {
                    return true;
                }
                sides[i] -= matchsticks[index];
            }
        }
        return false;
    }
}
```

复杂度：时间最坏 `O(4^n)`，空间 `O(n)`。

#### 资深解法：倒序放置加对称剪枝

算法思想：先排序，让长火柴优先放置，失败更早暴露。回溯时如果两条边当前长度相同，尝试其中一条失败后，另一条等价状态也不用再试。

```java
import java.util.Arrays;

class Solution {
    public boolean makesquare(int[] matchsticks) {
        int sum = 0;
        for (int stick : matchsticks) {
            sum += stick;
        }
        if (sum % 4 != 0) {
            return false;
        }

        Arrays.sort(matchsticks);
        int target = sum / 4;
        if (matchsticks[matchsticks.length - 1] > target) {
            return false;
        }

        return backtrack(matchsticks, matchsticks.length - 1, new int[4], target);
    }

    private boolean backtrack(int[] sticks, int index, int[] sides, int target) {
        if (index < 0) {
            return true;
        }

        int stick = sticks[index];
        for (int i = 0; i < 4; i++) {
            if (sides[i] + stick > target) {
                continue;
            }
            boolean sameState = false;
            for (int j = 0; j < i; j++) {
                if (sides[j] == sides[i]) {
                    sameState = true;
                    break;
                }
            }
            if (sameState) {
                continue;
            }

            sides[i] += stick;
            if (backtrack(sticks, index - 1, sides, target)) {
                return true;
            }
            sides[i] -= stick;
        }
        return false;
    }
}
```

复杂度：时间最坏仍为指数级，但剪枝后实际更快；空间 `O(n)`。

#### 基础语法与算法思想

- 回溯中数组 `sides` 表示当前四条边的累计长度。
- 排序后从大到小尝试，常用于装箱、分组等搜索题。
- 核心思想：正方形要求四个桶容量相等，本题可以看作把火柴分配到四个容量相同的桶。

---

## 474. 一和零 (Medium)

给你一个二进制字符串数组  `strs`  和两个整数  `m`  和  `n`  。

请你找出并返回  `strs`  的最大子集的长度，该子集中  **最多**  有  `m`  个  `0`  和  `n`  个  `1`  。
如果  `x`  的所有元素也是  `y`  的元素，集合  `x`  是集合  `y`  的  **子集**  。

 
 **示例 1：** 

```text
输入：strs = ["10", "0001", "111001", "1", "0"], m = 5, n = 3
输出：4
解释：最多有 5 个 0 和 3 个 1 的最大子集是 {"10","0001","1","0"} ，因此答案是 4 。
其他满足题意但较小的子集包括 {"0001","1"} 和 {"10","1","0"} 。{"111001"} 不满足题意，因为它含 4 个 1 ，大于 n 的值 3 。
```

 **示例 2：** 

```text
输入：strs = ["10", "0", "1"], m = 1, n = 1
输出：2
解释：最大的子集是 {"0", "1"} ，所以答案是 2 。
```

 
 **提示：** 

 `1 <= strs.length <= 600` 
 `1 <= strs[i].length <= 100` 
 `strs[i]`  仅由  `'0'`  和  `'1'`  组成
 `1 <= m, n <= 100`

### Java 解法补充

#### 基础解法：递归选择或跳过

算法思想：对每个字符串只有两种选择：放入子集或跳过。放入时消耗对应数量的 `0` 和 `1`，递归返回能得到的最大数量。

```java
class Solution {
    public int findMaxForm(String[] strs, int m, int n) {
        return dfs(strs, 0, m, n);
    }

    private int dfs(String[] strs, int index, int zerosLeft, int onesLeft) {
        if (index == strs.length) {
            return 0;
        }

        int skip = dfs(strs, index + 1, zerosLeft, onesLeft);
        int[] count = count(strs[index]);
        int take = 0;
        if (count[0] <= zerosLeft && count[1] <= onesLeft) {
            take = 1 + dfs(strs, index + 1, zerosLeft - count[0], onesLeft - count[1]);
        }
        return Math.max(skip, take);
    }

    private int[] count(String s) {
        int zeros = 0;
        int ones = 0;
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '0') zeros++;
            else ones++;
        }
        return new int[]{zeros, ones};
    }
}
```

复杂度：时间最坏 `O(2^len)`，空间 `O(len)`。

#### 资深解法：二维 0/1 背包

算法思想：每个字符串是一个物品，成本是 `zeros` 和 `ones` 两个维度，价值是 1。倒序更新二维 DP，保证每个字符串最多使用一次。

```java
class Solution {
    public int findMaxForm(String[] strs, int m, int n) {
        int[][] dp = new int[m + 1][n + 1];

        for (String str : strs) {
            int[] count = count(str);
            int zeros = count[0];
            int ones = count[1];

            for (int i = m; i >= zeros; i--) {
                for (int j = n; j >= ones; j--) {
                    dp[i][j] = Math.max(dp[i][j], dp[i - zeros][j - ones] + 1);
                }
            }
        }

        return dp[m][n];
    }

    private int[] count(String s) {
        int[] ans = new int[2];
        for (int i = 0; i < s.length(); i++) {
            ans[s.charAt(i) - '0']++;
        }
        return ans;
    }
}
```

复杂度：时间 `O(strs.length * m * n)`，空间 `O(mn)`。

#### 基础语法与算法思想

- `ans[s.charAt(i) - '0']++` 可以统计二进制字符数量。
- 0/1 背包压缩空间时要倒序遍历容量。
- 核心思想：有两个资源限制时，把背包容量扩展成二维即可。

---

## 475. 供暖器 (Medium)

冬季已经来临。 你的任务是设计一个有固定加热半径的供暖器向所有房屋供暖。
在加热器的加热半径范围内的每个房屋都可以获得供暖。
现在，给出位于一条水平线上的房屋  `houses`  和供暖器  `heaters`  的位置，请你找出并返回可以覆盖所有房屋的最小加热半径。
 **注意** ：所有供暖器  `heaters`  都遵循你的半径标准，加热的半径也一样。
 
 **示例 1:** 

```text
输入: houses = [1,2,3], heaters = [2]
输出: 1
解释: 仅在位置 2 上有一个供暖器。如果我们将加热半径设为 1，那么所有房屋就都能得到供暖。
```

 **示例 2:** 

```text
输入: houses = [1,2,3,4], heaters = [1,4]
输出: 1
解释: 在位置 1, 4 上有两个供暖器。我们需要将加热半径设为 1，这样所有房屋就都能得到供暖。
```

 **示例 3：** 

```text
输入：houses = [1,5], heaters = [2]
输出：3
```

 
 **提示：** 

 `1 <= houses.length, heaters.length <= 3 * 104` 
 `1 <= houses[i], heaters[i] <= 109`

### Java 解法补充

#### 基础解法：每个房屋扫描所有供暖器

算法思想：对每个房屋，计算它到所有供暖器的最小距离。所有房屋都要被覆盖，所以答案是这些最小距离中的最大值。

```java
class Solution {
    public int findRadius(int[] houses, int[] heaters) {
        int ans = 0;

        for (int house : houses) {
            int nearest = Integer.MAX_VALUE;
            for (int heater : heaters) {
                nearest = Math.min(nearest, Math.abs(house - heater));
            }
            ans = Math.max(ans, nearest);
        }

        return ans;
    }
}
```

复杂度：时间 `O(houses.length * heaters.length)`，空间 `O(1)`。

#### 资深解法：排序后二分最近供暖器

算法思想：先排序供暖器。对每个房屋，用二分找到第一个不小于它的供暖器，再比较左右两个候选距离，更新全局最大值。

```java
import java.util.Arrays;

class Solution {
    public int findRadius(int[] houses, int[] heaters) {
        Arrays.sort(heaters);
        int ans = 0;

        for (int house : houses) {
            int pos = Arrays.binarySearch(heaters, house);
            if (pos < 0) {
                pos = -pos - 1;
            }

            int right = pos < heaters.length ? Math.abs(heaters[pos] - house) : Integer.MAX_VALUE;
            int left = pos > 0 ? Math.abs(house - heaters[pos - 1]) : Integer.MAX_VALUE;
            ans = Math.max(ans, Math.min(left, right));
        }

        return ans;
    }
}
```

复杂度：时间 `O(m log m + n log m)`，空间 `O(1)`。

#### 基础语法与算法思想

- `Arrays.binarySearch` 找不到时返回负插入点，可用 `-pos - 1` 转回插入位置。
- 覆盖全部房屋需要取每个房屋最近距离的最大值。
- 核心思想：一维最近点问题排序后二分左右邻居即可。

---

## 476. 数字的补数 (Easy)

对整数的二进制表示取反（ `0`  变  `1`  ， `1`  变  `0` ）后，再转换为十进制表示，可以得到这个整数的补数。

例如，整数  `5`  的二进制表示是  `"101"`  ，取反后得到  `"010"`  ，再转回十进制表示得到补数  `2`  。

给你一个整数  `num`  ，输出它的补数。
 

 **示例 1：** 

```text
输入：num = 5
输出：2
解释：5 的二进制表示为 101（没有前导零位），其补数为 010。所以你需要输出 2 。
```

 **示例 2：** 

```text
输入：num = 1
输出：0
解释：1 的二进制表示为 1（没有前导零位），其补数为 0。所以你需要输出 0 。
```

 
 **提示：** 

 `1 <= num < 231` 

 
 **注意：** 本题与 1009 https://leetcode.cn/problems/complement-of-base-10-integer/ 相同

### Java 解法补充

#### 基础解法：字符串翻转二进制位

算法思想：先把数字转成二进制字符串，逐位把 `0` 变成 `1`、`1` 变成 `0`，最后再按二进制解析成整数。

```java
class Solution {
    public int findComplement(int num) {
        String binary = Integer.toBinaryString(num);
        StringBuilder builder = new StringBuilder();

        for (int i = 0; i < binary.length(); i++) {
            builder.append(binary.charAt(i) == '0' ? '1' : '0');
        }

        return Integer.parseInt(builder.toString(), 2);
    }
}
```

复杂度：时间 `O(log num)`，空间 `O(log num)`。

#### 资深解法：构造同位数全 1 掩码

算法思想：补数只反转有效二进制位。构造一个和 `num` 位数相同、每一位都是 1 的 `mask`，答案就是 `mask ^ num`。

```java
class Solution {
    public int findComplement(int num) {
        int mask = 0;
        int value = num;

        while (value > 0) {
            mask = (mask << 1) | 1;
            value >>= 1;
        }

        return mask ^ num;
    }
}
```

复杂度：时间 `O(log num)`，空间 `O(1)`。

#### 基础语法与算法思想

- `Integer.toBinaryString(num)` 返回不含前导零的二进制字符串。
- `^` 是按位异或，不同为 1，相同为 0。
- 核心思想：只补有效位，不能把 Java `int` 的所有 32 位都取反。

---

## 477. 汉明距离总和 (Medium)

两个整数的 汉明距离 指的是这两个数字的二进制数对应位不同的数量。
给你一个整数数组  `nums` ，请你计算并返回  `nums`  中任意两个数之间  **汉明距离的总和**  。
 
 **示例 1：** 

```text
输入：nums = [4,14,2]
输出：6
解释：在二进制表示中，4 表示为 0100 ，14 表示为 1110 ，2表示为 0010 。（这样表示是为了体现后四位之间关系）
所以答案为：
HammingDistance(4, 14) + HammingDistance(4, 2) + HammingDistance(14, 2) = 2 + 2 + 2 = 6
```

 **示例 2：** 

```text
输入：nums = [4,14,4]
输出：4
```

 
 **提示：** 

 `1 <= nums.length <= 104` 
 `0 <= nums[i] <= 109` 
给定输入的对应答案符合  **32-bit**  整数范围

### Java 解法补充

#### 基础解法：枚举每一对数字

算法思想：直接枚举所有数字对，两个数异或后，二进制中 1 的个数就是这一对的汉明距离。

```java
class Solution {
    public int totalHammingDistance(int[] nums) {
        int ans = 0;

        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                ans += Integer.bitCount(nums[i] ^ nums[j]);
            }
        }

        return ans;
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(1)`。

#### 资深解法：按位统计贡献

算法思想：每一位单独计算。如果某一位有 `ones` 个数字为 1，有 `zeros` 个数字为 0，那么这一位对答案贡献 `ones * zeros`。

```java
class Solution {
    public int totalHammingDistance(int[] nums) {
        int ans = 0;

        for (int bit = 0; bit < 31; bit++) {
            int ones = 0;
            for (int num : nums) {
                ones += (num >> bit) & 1;
            }
            ans += ones * (nums.length - ones);
        }

        return ans;
    }
}
```

复杂度：时间 `O(31n)`，空间 `O(1)`。

#### 基础语法与算法思想

- `Integer.bitCount(x)` 返回整数二进制中 1 的个数。
- `(num >> bit) & 1` 用于读取某一位。
- 核心思想：成对差异可以拆到每个二进制位独立统计。

---

## 478. 在圆内随机生成点 (Medium)

给定圆的半径和圆心的位置，实现函数  `randPoint`  ，在圆中产生均匀随机点。
实现  `Solution`  类:

 `Solution(double radius, double x_center, double y_center)`  用圆的半径  `radius`  和圆心的位置 `(x_center, y_center)`  初始化对象
 `randPoint()`  返回圆内的一个随机点。圆周上的一点被认为在圆内。答案作为数组返回  `[x, y]`  。

 
 **示例 1：** 

```text
输入: 
["Solution","randPoint","randPoint","randPoint"]
[[1.0, 0.0, 0.0], [], [], []]
输出: [null, [-0.02493, -0.38077], [0.82314, 0.38945], [0.36572, 0.17248]]
解释:
Solution solution = new Solution(1.0, 0.0, 0.0);
solution.randPoint ();//返回[-0.02493，-0.38077]
solution.randPoint ();//返回[0.82314,0.38945]
solution.randPoint ();//返回[0.36572,0.17248]
```

 
 **提示：** 

 `0 < radius <= 108` 
 `-107 <= x_center, y_center <= 107` 
 `randPoint`  最多被调用  `3 * 104`  次

### Java 解法补充

#### 基础解法：正方形拒绝采样

算法思想：先在包住圆的正方形内随机生成点，如果点到圆心的距离不超过半径，就返回；否则继续生成。

```java
import java.util.Random;

class Solution {
    private final double radius;
    private final double xCenter;
    private final double yCenter;
    private final Random random = new Random();

    public Solution(double radius, double x_center, double y_center) {
        this.radius = radius;
        this.xCenter = x_center;
        this.yCenter = y_center;
    }

    public double[] randPoint() {
        while (true) {
            double x = xCenter - radius + random.nextDouble() * 2 * radius;
            double y = yCenter - radius + random.nextDouble() * 2 * radius;
            double dx = x - xCenter;
            double dy = y - yCenter;
            if (dx * dx + dy * dy <= radius * radius) {
                return new double[]{x, y};
            }
        }
    }
}
```

复杂度：期望时间 `O(1)`，空间 `O(1)`。

#### 资深解法：极坐标均匀采样

算法思想：角度在 `[0, 2π)` 中均匀选取。半径不能直接均匀取，否则点会偏向圆心；应使用 `sqrt(random)` 修正面积分布。

```java
import java.util.Random;

class Solution {
    private final double radius;
    private final double xCenter;
    private final double yCenter;
    private final Random random = new Random();

    public Solution(double radius, double x_center, double y_center) {
        this.radius = radius;
        this.xCenter = x_center;
        this.yCenter = y_center;
    }

    public double[] randPoint() {
        double r = Math.sqrt(random.nextDouble()) * radius;
        double angle = random.nextDouble() * 2.0 * Math.PI;
        double x = xCenter + r * Math.cos(angle);
        double y = yCenter + r * Math.sin(angle);
        return new double[]{x, y};
    }
}
```

复杂度：时间 `O(1)`，空间 `O(1)`。

#### 基础语法与算法思想

- `Random.nextDouble()` 返回 `[0.0, 1.0)` 的随机小数。
- `Math.sqrt` 用于修正半径分布，保证面积均匀。
- 核心思想：二维均匀不是半径均匀，而是面积均匀。

---

## 479. 最大回文数乘积 (Hard)

给定一个整数 n ，返回 可表示为两个  `n`  位整数乘积的  **最大回文整数**  。因为答案可能非常大，所以返回它对  `1337`   **取余**  。
 
 **示例 1：** 

```text
输入：n = 2
输出：987
解释：99 x 91 = 9009, 9009 % 1337 = 987
```

 **示例 2：** 

```text
输入：n = 1
输出：9
```

 
 **提示:** 

 `1 <= n <= 8`

### Java 解法补充

#### 基础解法：枚举乘积并判断回文

算法思想：枚举两个 `n` 位数的乘积，维护最大的回文乘积。这个写法直观，但当 `n` 较大时会很慢，只适合理解题意。

```java
class Solution {
    public int largestPalindrome(int n) {
        if (n == 1) return 9;

        long upper = (long) Math.pow(10, n) - 1;
        long lower = (long) Math.pow(10, n - 1);
        long best = 0;

        for (long a = upper; a >= lower; a--) {
            for (long b = a; b >= lower && a * b > best; b--) {
                long product = a * b;
                if (isPalindrome(product)) {
                    best = product;
                    break;
                }
            }
        }

        return (int) (best % 1337);
    }

    private boolean isPalindrome(long value) {
        String s = Long.toString(value);
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

复杂度：时间最坏 `O(10^(2n) * n)`，空间 `O(n)`。

#### 资深解法：枚举回文前半部分

算法思想：最大乘积一定接近上界。按前半部分从大到小生成偶数长度回文，再检查它是否能分解成两个 `n` 位数。第一次命中就是最大回文。

```java
class Solution {
    public int largestPalindrome(int n) {
        if (n == 1) return 9;

        long upper = (long) Math.pow(10, n) - 1;
        long lower = (long) Math.pow(10, n - 1);

        for (long left = upper; left >= lower; left--) {
            long palindrome = buildPalindrome(left);
            for (long factor = upper; factor * factor >= palindrome; factor--) {
                if (palindrome % factor == 0) {
                    long other = palindrome / factor;
                    if (other >= lower && other <= upper) {
                        return (int) (palindrome % 1337);
                    }
                }
            }
        }

        return 0;
    }

    private long buildPalindrome(long left) {
        long ans = left;
        long value = left;
        while (value > 0) {
            ans = ans * 10 + value % 10;
            value /= 10;
        }
        return ans;
    }
}
```

复杂度：时间显著小于暴力枚举，空间 `O(1)`。

#### 基础语法与算法思想

- `Math.pow(10, n)` 可用于计算 `n` 位数上下界。
- 从大到小生成候选，第一次可分解时即可返回。
- 核心思想：与其枚举乘积再判断回文，不如直接枚举回文再判断是否可分解。

---

## 480. 滑动窗口中位数 (Hard)

中位数是有序序列最中间的那个数。如果序列的长度是偶数，则没有最中间的数；此时中位数是最中间的两个数的平均数。
例如：

 `[2,3,4]` ，中位数是  `3` 
 `[2,3]` ，中位数是  `(2 + 3) / 2 = 2.5` 

给你一个数组 nums，有一个长度为 k 的窗口从最左端滑动到最右端。窗口中有 k 个数，每次窗口向右移动 1 位。你的任务是找出每次窗口移动后得到的新窗口中元素的中位数，并输出由它们组成的数组。
 
 **示例：** 
给出 nums =  `[1,3,-1,-3,5,3,6,7]` ，以及 k = 3。

```text
窗口位置                      中位数
---------------               -----
[1  3  -1] -3  5  3  6  7       1
 1 [3  -1  -3] 5  3  6  7      -1
 1  3 [-1  -3  5] 3  6  7      -1
 1  3  -1 [-3  5  3] 6  7       3
 1  3  -1  -3 [5  3  6] 7       5
 1  3  -1  -3  5 [3  6  7]      6
```

 因此，返回该滑动窗口的中位数数组  `[1,-1,-1,3,5,6]` 。
 
 **提示：** 

你可以假设  `k`  始终有效，即： `k`  始终小于等于输入的非空数组的元素个数。
与真实值误差在  `10 ^ -5`  以内的答案将被视作正确答案。

### Java 解法补充

#### 基础解法：每个窗口排序

算法思想：每次窗口右移后，把窗口内的 `k` 个数复制出来排序，再取中位数。代码最直观，但重复排序很多。

```java
import java.util.Arrays;

class Solution {
    public double[] medianSlidingWindow(int[] nums, int k) {
        double[] ans = new double[nums.length - k + 1];

        for (int left = 0; left + k <= nums.length; left++) {
            int[] window = new int[k];
            for (int i = 0; i < k; i++) {
                window[i] = nums[left + i];
            }
            Arrays.sort(window);
            if (k % 2 == 1) {
                ans[left] = window[k / 2];
            } else {
                ans[left] = ((long) window[k / 2 - 1] + window[k / 2]) / 2.0;
            }
        }

        return ans;
    }
}
```

复杂度：时间 `O((n - k + 1) * k log k)`，空间 `O(k)`。

#### 资深解法：TreeMap 双有序集合

算法思想：维护两个可重复有序集合：`small` 保存较小的一半，`large` 保存较大的一半。让 `small` 的元素个数等于或比 `large` 多 1，中位数就能从边界元素直接得到。

```java
import java.util.TreeMap;

class Solution {
    public double[] medianSlidingWindow(int[] nums, int k) {
        DualSet window = new DualSet(k);
        double[] ans = new double[nums.length - k + 1];

        for (int i = 0; i < nums.length; i++) {
            window.add(nums[i]);
            if (i >= k) {
                window.remove(nums[i - k]);
            }
            if (i >= k - 1) {
                ans[i - k + 1] = window.median();
            }
        }

        return ans;
    }

    private static class DualSet {
        private final TreeMap<Integer, Integer> small = new TreeMap<>();
        private final TreeMap<Integer, Integer> large = new TreeMap<>();
        private final int k;
        private int smallSize;
        private int largeSize;

        DualSet(int k) {
            this.k = k;
        }

        void add(int num) {
            if (smallSize == 0 || num <= small.lastKey()) {
                addTo(small, num);
                smallSize++;
            } else {
                addTo(large, num);
                largeSize++;
            }
            balance();
        }

        void remove(int num) {
            if (removeFrom(small, num)) {
                smallSize--;
            } else {
                removeFrom(large, num);
                largeSize--;
            }
            balance();
        }

        double median() {
            if (k % 2 == 1) {
                return small.lastKey();
            }
            return ((long) small.lastKey() + large.firstKey()) / 2.0;
        }

        private void balance() {
            while (smallSize > largeSize + 1) {
                int value = small.lastKey();
                removeFrom(small, value);
                smallSize--;
                addTo(large, value);
                largeSize++;
            }
            while (smallSize < largeSize) {
                int value = large.firstKey();
                removeFrom(large, value);
                largeSize--;
                addTo(small, value);
                smallSize++;
            }
        }

        private void addTo(TreeMap<Integer, Integer> map, int num) {
            map.put(num, map.getOrDefault(num, 0) + 1);
        }

        private boolean removeFrom(TreeMap<Integer, Integer> map, int num) {
            Integer count = map.get(num);
            if (count == null) {
                return false;
            }
            if (count == 1) {
                map.remove(num);
            } else {
                map.put(num, count - 1);
            }
            return true;
        }
    }
}
```

复杂度：时间 `O(n log k)`，空间 `O(k)`。

#### 基础语法与算法思想

- `TreeMap` 可以当作有序多重集合使用，值保存出现次数。
- 偶数长度中位数要先转 `long` 再相加，避免整数溢出。
- 核心思想：动态中位数维护“较小一半”和“较大一半”，平衡后中位数就在边界。

---

