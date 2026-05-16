# LeetCode 热题 100 Java 题解补充

整理日期：`2026-05-16`

说明：
- 这份文档只补仓库里 `Hot 100` 中尚未内置 `Java 解法补充` 的题目。
- 题面仍以原始分片文档为准，这里只给 Java 题解与思路。
- 格式保持和前 100 题一致：`基础解法 / 资深解法 / 基础语法与算法思想`。

---

## 128. 最长连续序列 (Medium)

原题位置：`LeetCode_Collection_Part_5.md:273`

### Java 解法补充

#### 基础解法：排序后线性扫描

算法思想：先排序，再从左到右统计连续段长度。重复数字跳过，遇到断点就重新计数。

```java
import java.util.Arrays;

class Solution {
    public int longestConsecutive(int[] nums) {
        if (nums.length == 0) return 0;
        Arrays.sort(nums);
        int ans = 1;
        int cur = 1;
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] == nums[i - 1]) continue;
            if (nums[i] == nums[i - 1] + 1) {
                cur++;
            } else {
                cur = 1;
            }
            ans = Math.max(ans, cur);
        }
        return ans;
    }
}
```

复杂度：时间 `O(n log n)`，空间 `O(1)` 或排序栈空间。

#### 资深解法：哈希集合找连续段起点

算法思想：把所有数字放入集合。只有当 `x - 1` 不在集合里时，`x` 才可能是某段连续序列的起点，然后向右扩展统计长度。

```java
import java.util.HashSet;
import java.util.Set;

class Solution {
    public int longestConsecutive(int[] nums) {
        Set<Integer> set = new HashSet<>();
        for (int num : nums) set.add(num);

        int ans = 0;
        for (int num : set) {
            if (set.contains(num - 1)) continue;
            int cur = num;
            int len = 1;
            while (set.contains(cur + 1)) {
                cur++;
                len++;
            }
            ans = Math.max(ans, len);
        }
        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 基础语法与算法思想

- `Arrays.sort(nums)`：对数组原地排序。
- `HashSet`：把“是否存在”查询降到均摊 `O(1)`。
- 核心思想：连续段只从起点开始扩展，避免重复统计。

---

## 283. 移动零 (Easy)

原题位置：`LeetCode_Collection_Part_10.md:246`

### Java 解法补充

#### 基础解法：额外数组收集非零元素

算法思想：先把所有非零数按顺序写入临时数组，再把剩余位置补零，最后拷回原数组。

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int[] temp = new int[nums.length];
        int idx = 0;
        for (int num : nums) {
            if (num != 0) temp[idx++] = num;
        }
        for (int i = 0; i < nums.length; i++) {
            nums[i] = temp[i];
        }
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 资深解法：双指针原地稳定移动

算法思想：`slow` 指向下一个应放非零元素的位置，`fast` 负责扫描。遇到非零数就和 `slow` 交换，保证非零元素相对顺序不变。

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int slow = 0;
        for (int fast = 0; fast < nums.length; fast++) {
            if (nums[fast] != 0) {
                int temp = nums[slow];
                nums[slow] = nums[fast];
                nums[fast] = temp;
                slow++;
            }
        }
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- `slow` 维护有效区间 `[0, slow)`。
- 原地交换后，前缀始终是已经整理好的非零部分。
- 核心思想：把“保留顺序的过滤”写成双指针覆盖。

---

## 438. 找到字符串中所有字母异位词 (Medium)

原题位置：`LeetCode_Collection_Part_15.md:493`

### Java 解法补充

#### 基础解法：固定窗口逐段比较计数

算法思想：维护长度为 `p.length()` 的窗口，每次统计窗口字符频次，再和模式串频次数组比较。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> ans = new ArrayList<>();
        int m = p.length();
        if (s.length() < m) return ans;

        int[] target = new int[26];
        for (char c : p.toCharArray()) target[c - 'a']++;

        for (int i = 0; i + m <= s.length(); i++) {
            int[] count = new int[26];
            for (int j = i; j < i + m; j++) count[s.charAt(j) - 'a']++;
            if (same(count, target)) ans.add(i);
        }
        return ans;
    }

    private boolean same(int[] a, int[] b) {
        for (int i = 0; i < 26; i++) {
            if (a[i] != b[i]) return false;
        }
        return true;
    }
}
```

复杂度：时间 `O(26 * n + n * m)`，空间 `O(1)`。

#### 资深解法：滑动窗口增量维护频次

算法思想：先统计模式串频次，再让窗口右扩、左缩。窗口长度固定为 `p.length()`，每移动一步只改两个字符的计数。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> ans = new ArrayList<>();
        int n = s.length(), m = p.length();
        if (n < m) return ans;

        int[] need = new int[26];
        int[] window = new int[26];
        for (char c : p.toCharArray()) need[c - 'a']++;

        for (int i = 0; i < n; i++) {
            window[s.charAt(i) - 'a']++;
            if (i >= m) {
                window[s.charAt(i - m) - 'a']--;
            }
            if (i >= m - 1 && same(window, need)) {
                ans.add(i - m + 1);
            }
        }
        return ans;
    }

    private boolean same(int[] a, int[] b) {
        for (int i = 0; i < 26; i++) {
            if (a[i] != b[i]) return false;
        }
        return true;
    }
}
```

复杂度：时间 `O(26 * n)`，空间 `O(1)`。

#### 基础语法与算法思想

- `int[26]` 很适合小写字母频次统计。
- 固定长度窗口常见写法：右端加入，长度超限时左端移出。
- 核心思想：异位词比较的是字符多重集，不是顺序。

---

## 560. 和为 K 的子数组 (Medium)

原题位置：`LeetCode_Collection_Part_19.md:556`

### Java 解法补充

#### 基础解法：枚举右端点并向左累加

算法思想：固定每个右端点 `i`，向左不断累加区间和，只要等于 `k` 就计数。

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int ans = 0;
        for (int right = 0; right < nums.length; right++) {
            int sum = 0;
            for (int left = right; left >= 0; left--) {
                sum += nums[left];
                if (sum == k) ans++;
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(1)`。

#### 资深解法：前缀和 + 哈希表计数

算法思想：若 `prefix[i] - prefix[j] = k`，则 `[j + 1, i]` 的和为 `k`。扫描到当前前缀和 `sum` 时，查之前出现过多少个 `sum - k`。

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int subarraySum(int[] nums, int k) {
        Map<Integer, Integer> count = new HashMap<>();
        count.put(0, 1);
        int sum = 0;
        int ans = 0;
        for (int num : nums) {
            sum += num;
            ans += count.getOrDefault(sum - k, 0);
            count.put(sum, count.getOrDefault(sum, 0) + 1);
        }
        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 基础语法与算法思想

- `count.put(0, 1)`：表示空前缀先出现 1 次。
- `getOrDefault`：读取哈希表时给默认值。
- 核心思想：把“区间和”转成“两个前缀和的差”。

---

## 239. 滑动窗口最大值 (Hard)

原题位置：`LeetCode_Collection_Part_8.md:1024`

### Java 解法补充

#### 基础解法：每个窗口单独求最大值

算法思想：窗口每向右移动一次，就重新扫描这 `k` 个元素求最大值。

```java
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int[] ans = new int[nums.length - k + 1];
        for (int i = 0; i + k <= nums.length; i++) {
            int max = nums[i];
            for (int j = i + 1; j < i + k; j++) {
                max = Math.max(max, nums[j]);
            }
            ans[i] = max;
        }
        return ans;
    }
}
```

复杂度：时间 `O(nk)`，空间 `O(1)`。

#### 资深解法：单调队列维护候选最大值

算法思想：队列中存下标，并保持对应值单调递减。队首永远是当前窗口最大值的下标。

```java
import java.util.ArrayDeque;
import java.util.Deque;

class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int n = nums.length;
        int[] ans = new int[n - k + 1];
        Deque<Integer> deque = new ArrayDeque<>();

        for (int i = 0; i < n; i++) {
            while (!deque.isEmpty() && deque.peekFirst() <= i - k) {
                deque.pollFirst();
            }
            while (!deque.isEmpty() && nums[deque.peekLast()] <= nums[i]) {
                deque.pollLast();
            }
            deque.offerLast(i);
            if (i >= k - 1) {
                ans[i - k + 1] = nums[deque.peekFirst()];
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(k)`。

#### 基础语法与算法思想

- `Deque<Integer>` 同时支持队首和队尾操作。
- 队列里存下标，比直接存值更容易判断元素是否过期。
- 核心思想：让无用的小值在进入窗口时就被淘汰。

---

## 76. 最小覆盖子串 (Hard)

原题位置：`LeetCode_Collection_Part3.md:589`

### Java 解法补充

#### 基础解法：枚举左端点并向右找最短合法区间

算法思想：从每个左端点出发向右扩展，直到包含 `t` 的全部字符，再更新最短答案。

```java
class Solution {
    public String minWindow(String s, String t) {
        int[] need = new int[128];
        for (char c : t.toCharArray()) need[c]++;

        int bestLen = Integer.MAX_VALUE;
        int bestStart = 0;
        for (int left = 0; left < s.length(); left++) {
            int[] count = new int[128];
            for (int right = left; right < s.length(); right++) {
                count[s.charAt(right)]++;
                if (covers(count, need)) {
                    if (right - left + 1 < bestLen) {
                        bestLen = right - left + 1;
                        bestStart = left;
                    }
                    break;
                }
            }
        }
        return bestLen == Integer.MAX_VALUE ? "" : s.substring(bestStart, bestStart + bestLen);
    }

    private boolean covers(int[] count, int[] need) {
        for (int i = 0; i < 128; i++) {
            if (count[i] < need[i]) return false;
        }
        return true;
    }
}
```

复杂度：时间 `O(n^2 * |Sigma|)`，空间 `O(|Sigma|)`。

#### 资深解法：滑动窗口 + 有效字符计数

算法思想：右端不断扩张直到窗口满足要求，然后尽量收缩左端，始终维护当前最短合法区间。

```java
class Solution {
    public String minWindow(String s, String t) {
        int[] need = new int[128];
        for (char c : t.toCharArray()) need[c]++;

        int required = t.length();
        int left = 0;
        int bestLen = Integer.MAX_VALUE;
        int bestStart = 0;

        for (int right = 0; right < s.length(); right++) {
            char c = s.charAt(right);
            if (need[c] > 0) required--;
            need[c]--;

            while (required == 0) {
                if (right - left + 1 < bestLen) {
                    bestLen = right - left + 1;
                    bestStart = left;
                }
                char out = s.charAt(left++);
                need[out]++;
                if (need[out] > 0) required++;
            }
        }
        return bestLen == Integer.MAX_VALUE ? "" : s.substring(bestStart, bestStart + bestLen);
    }
}
```

复杂度：时间 `O(n)`，空间 `O(|Sigma|)`。

#### 基础语法与算法思想

- `need[c]` 在窗口推进过程中既表示需求，也能顺带表示“多出来多少”。
- `required` 记录还差多少个字符，避免每次全表扫描。
- 核心思想：这是典型的“先扩到合法，再缩到极致”的窗口题。

---

## 189. 轮转数组 (Medium)

原题位置：`LeetCode_Collection_Part_7.md:401`

### Java 解法补充

#### 基础解法：额外数组按目标位置写入

算法思想：元素 `nums[i]` 轮转后会去 `(i + k) % n`，直接写进新数组，最后拷回原数组。

```java
class Solution {
    public void rotate(int[] nums, int k) {
        int n = nums.length;
        int[] temp = new int[n];
        k %= n;
        for (int i = 0; i < n; i++) {
            temp[(i + k) % n] = nums[i];
        }
        for (int i = 0; i < n; i++) {
            nums[i] = temp[i];
        }
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 资深解法：三次反转原地轮转

算法思想：先整体反转，再分别反转前 `k` 个和后 `n-k` 个元素，顺序就会变成轮转后的结果。

```java
class Solution {
    public void rotate(int[] nums, int k) {
        int n = nums.length;
        k %= n;
        reverse(nums, 0, n - 1);
        reverse(nums, 0, k - 1);
        reverse(nums, k, n - 1);
    }

    private void reverse(int[] nums, int left, int right) {
        while (left < right) {
            int temp = nums[left];
            nums[left] = nums[right];
            nums[right] = temp;
            left++;
            right--;
        }
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- `k %= n`：避免轮转次数超过数组长度。
- 原地反转是数组变换题里的常见技巧。
- 核心思想：把“后半段搬到前面”转成区间反转组合。

---

## 238. 除了自身以外数组的乘积 (Medium)

原题位置：`LeetCode_Collection_Part_8.md:992`

### Java 解法补充

#### 基础解法：左右乘积数组

算法思想：`left[i]` 保存左边所有数的乘积，`right[i]` 保存右边所有数的乘积，答案就是两者相乘。

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int[] left = new int[n];
        int[] right = new int[n];
        int[] ans = new int[n];

        left[0] = 1;
        for (int i = 1; i < n; i++) {
            left[i] = left[i - 1] * nums[i - 1];
        }

        right[n - 1] = 1;
        for (int i = n - 2; i >= 0; i--) {
            right[i] = right[i + 1] * nums[i + 1];
        }

        for (int i = 0; i < n; i++) {
            ans[i] = left[i] * right[i];
        }
        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 资深解法：答案数组复用前缀积，再乘后缀积

算法思想：先让 `ans[i]` 保存左侧乘积，再从右向左滚动维护后缀乘积 `suffix`，直接乘到 `ans[i]` 上。

```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int[] ans = new int[n];
        ans[0] = 1;
        for (int i = 1; i < n; i++) {
            ans[i] = ans[i - 1] * nums[i - 1];
        }

        int suffix = 1;
        for (int i = n - 1; i >= 0; i--) {
            ans[i] *= suffix;
            suffix *= nums[i];
        }
        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`，不计返回数组。

#### 基础语法与算法思想

- `ans[i]` 可以先存一部分信息，再二次遍历补齐另一部分。
- `suffix` 是从右往左滚动更新的后缀乘积。
- 核心思想：当前位置的答案等于“左积 * 右积”。

---

## 73. 矩阵置零 (Medium)

原题位置：`LeetCode_Collection_Part3.md:485`

### Java 解法补充

#### 基础解法：记录要清零的行和列

算法思想：先扫描矩阵，记录哪些行、哪些列需要清零；再第二次遍历按标记改值。

```java
class Solution {
    public void setZeroes(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        boolean[] rows = new boolean[m];
        boolean[] cols = new boolean[n];

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == 0) {
                    rows[i] = true;
                    cols[j] = true;
                }
            }
        }

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (rows[i] || cols[j]) matrix[i][j] = 0;
            }
        }
    }
}
```

复杂度：时间 `O(mn)`，空间 `O(m+n)`。

#### 资深解法：用首行首列做原地标记

算法思想：把第 0 行和第 0 列当作标记数组，额外用两个布尔值记录首行首列是否本来就有零，最后统一处理。

```java
class Solution {
    public void setZeroes(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        boolean firstRowZero = false;
        boolean firstColZero = false;

        for (int j = 0; j < n; j++) {
            if (matrix[0][j] == 0) firstRowZero = true;
        }
        for (int i = 0; i < m; i++) {
            if (matrix[i][0] == 0) firstColZero = true;
        }

        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][j] == 0) {
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }

        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][0] == 0 || matrix[0][j] == 0) {
                    matrix[i][j] = 0;
                }
            }
        }

        if (firstRowZero) {
            for (int j = 0; j < n; j++) matrix[0][j] = 0;
        }
        if (firstColZero) {
            for (int i = 0; i < m; i++) matrix[i][0] = 0;
        }
    }
}
```

复杂度：时间 `O(mn)`，空间 `O(1)`。

#### 基础语法与算法思想

- 先处理首行首列原始状态，否则标记会污染原值。
- `matrix[i][0]`、`matrix[0][j]` 可以充当布尔标记位。
- 核心思想：借现有矩阵存元信息，省掉额外数组。

---

## 240. 搜索二维矩阵 II (Medium)

原题位置：`LeetCode_Collection_Part_8.md:1061`

### Java 解法补充

#### 基础解法：逐行二分查找

算法思想：每一行都是升序数组，判断目标值是否可能落在当前行后，再做二分查找。

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        for (int[] row : matrix) {
            if (target < row[0] || target > row[row.length - 1]) continue;
            int left = 0, right = row.length - 1;
            while (left <= right) {
                int mid = left + (right - left) / 2;
                if (row[mid] == target) return true;
                if (row[mid] < target) left = mid + 1;
                else right = mid - 1;
            }
        }
        return false;
    }
}
```

复杂度：时间 `O(m log n)`，空间 `O(1)`。

#### 资深解法：从右上角开始线性排除

算法思想：右上角左边都更小、下边都更大。若当前值大于目标就左移，小于目标就下移，每一步都能排除一整行或一整列。

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int row = 0;
        int col = matrix[0].length - 1;
        while (row < matrix.length && col >= 0) {
            if (matrix[row][col] == target) return true;
            if (matrix[row][col] > target) {
                col--;
            } else {
                row++;
            }
        }
        return false;
    }
}
```

复杂度：时间 `O(m+n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 这题的关键不是普通二分，而是利用二维单调性。
- 右上角和左下角都适合作为搜索起点。
- 核心思想：每一步都必须能安全排除一大片区域。

---

## 160. 相交链表 (Easy)

原题位置：`LeetCode_Collection_Part_6.md:340`

### Java 解法补充

#### 基础解法：先求长度再对齐

算法思想：分别求两条链表长度，让长链表先走若干步，之后两指针同步向前，第一次相遇点就是交点。

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        int lenA = length(headA);
        int lenB = length(headB);
        while (lenA > lenB) {
            headA = headA.next;
            lenA--;
        }
        while (lenB > lenA) {
            headB = headB.next;
            lenB--;
        }
        while (headA != headB) {
            headA = headA.next;
            headB = headB.next;
        }
        return headA;
    }

    private int length(ListNode node) {
        int len = 0;
        while (node != null) {
            len++;
            node = node.next;
        }
        return len;
    }
}
```

复杂度：时间 `O(m+n)`，空间 `O(1)`。

#### 资深解法：双指针走完两条链表

算法思想：指针 `a` 走完 A 就切到 B，`b` 走完 B 就切到 A。两人总路程一样，所以会在交点或 `null` 相遇。

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode a = headA;
        ListNode b = headB;
        while (a != b) {
            a = (a == null) ? headB : a.next;
            b = (b == null) ? headA : b.next;
        }
        return a;
    }
}
```

复杂度：时间 `O(m+n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 链表题常用“长度对齐”或“双指针补差”。
- 判断是否相交比较的是节点引用是否相同，不是 `val` 是否相等。
- 核心思想：让两个指针在同一剩余长度上开始同步移动。

---

## 206. 反转链表 (Easy)

原题位置：`LeetCode_Collection_Part_7.md:1010`

### Java 解法补充

#### 基础解法：迭代原地反转

算法思想：用 `prev` 保存已经反转好的前缀，`cur` 逐个摘下节点并插到前面。

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode cur = head;
        while (cur != null) {
            ListNode next = cur.next;
            cur.next = prev;
            prev = cur;
            cur = next;
        }
        return prev;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 资深解法：递归反转

算法思想：先反转后面的链表，回溯时把当前节点接到尾部，实现整条链表翻转。

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) return head;
        ListNode newHead = reverseList(head.next);
        head.next.next = head;
        head.next = null;
        return newHead;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`，递归栈。

#### 基础语法与算法思想

- 反转链表前必须先保存 `next`，否则后续链断掉。
- 递归版更短，但会占用调用栈。
- 核心思想：链表反转本质上是不断改 `next` 指向。

---

## 234. 回文链表 (Easy)

原题位置：`LeetCode_Collection_Part_8.md:848`

### Java 解法补充

#### 基础解法：转成数组后双指针判断

算法思想：遍历链表把值放入数组，再用左右指针比较是否回文。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public boolean isPalindrome(ListNode head) {
        List<Integer> values = new ArrayList<>();
        while (head != null) {
            values.add(head.val);
            head = head.next;
        }
        int left = 0, right = values.size() - 1;
        while (left < right) {
            if (!values.get(left).equals(values.get(right))) return false;
            left++;
            right--;
        }
        return true;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 资深解法：快慢指针 + 反转后半段

算法思想：先找中点，再反转后半段，最后从两头向中间比较。

```java
class Solution {
    public boolean isPalindrome(ListNode head) {
        if (head == null || head.next == null) return true;
        ListNode slow = head, fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        if (fast != null) slow = slow.next;

        ListNode right = reverse(slow);
        ListNode left = head;
        while (right != null) {
            if (left.val != right.val) return false;
            left = left.next;
            right = right.next;
        }
        return true;
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

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 快慢指针能在线性时间找到链表中点。
- 奇数长度链表要跳过正中间那个节点。
- 核心思想：把链表回文判断转成两段顺序一致的线性比较。

---

## 141. 环形链表 (Easy)

原题位置：`LeetCode_Collection_Part_5.md:766`

### Java 解法补充

#### 基础解法：哈希集合判重

算法思想：遍历链表，把每个访问过的节点放入集合；如果某节点再次出现，说明有环。

```java
import java.util.HashSet;
import java.util.Set;

public class Solution {
    public boolean hasCycle(ListNode head) {
        Set<ListNode> seen = new HashSet<>();
        while (head != null) {
            if (!seen.add(head)) return true;
            head = head.next;
        }
        return false;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 资深解法：快慢指针判环

算法思想：慢指针一次走一步，快指针一次走两步；若有环，两者一定在环内相遇。

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (slow == fast) return true;
        }
        return false;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 判环比较的是节点对象是否相同。
- `fast != null && fast.next != null` 是快指针题的标准保护条件。
- 核心思想：速度差会让双指针在环内追上彼此。

---

## 142. 环形链表 II (Medium)

原题位置：`LeetCode_Collection_Part_5.md:808`

### Java 解法补充

#### 基础解法：哈希集合返回首次重复节点

算法思想：遍历节点并放入集合，第一个重复访问到的节点就是入环点。

```java
import java.util.HashSet;
import java.util.Set;

public class Solution {
    public ListNode detectCycle(ListNode head) {
        Set<ListNode> seen = new HashSet<>();
        while (head != null) {
            if (!seen.add(head)) return head;
            head = head.next;
        }
        return null;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 资深解法：Floyd 判环后找入环点

算法思想：快慢指针先在环内相遇；再让一个指针回到头节点，二者同步前进，再次相遇的位置就是入环点。

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode slow = head, fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (slow == fast) {
                ListNode p = head;
                while (p != slow) {
                    p = p.next;
                    slow = slow.next;
                }
                return p;
            }
        }
        return null;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`。

#### 基础语法与算法思想

- 这是 Floyd 判环算法的第二阶段应用。
- 相遇后回到头节点再同步前进，是数学关系推导出的结论。
- 核心思想：相遇点不是答案，但能帮助定位入环点。

---

## 138. 随机链表的复制 (Medium)

原题位置：`LeetCode_Collection_Part_5.md:644`

### Java 解法补充

#### 基础解法：哈希表建立新旧节点映射

算法思想：第一次遍历复制所有节点并建映射，第二次遍历补 `next` 和 `random` 指针。

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public Node copyRandomList(Node head) {
        if (head == null) return null;
        Map<Node, Node> map = new HashMap<>();
        Node cur = head;
        while (cur != null) {
            map.put(cur, new Node(cur.val));
            cur = cur.next;
        }
        cur = head;
        while (cur != null) {
            map.get(cur).next = map.get(cur.next);
            map.get(cur).random = map.get(cur.random);
            cur = cur.next;
        }
        return map.get(head);
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 资深解法：穿插复制再拆分

算法思想：先把复制节点插到原节点后面，再借原链表直接定位 `random`，最后把两条链拆开。

```java
class Solution {
    public Node copyRandomList(Node head) {
        if (head == null) return null;

        Node cur = head;
        while (cur != null) {
            Node copy = new Node(cur.val);
            copy.next = cur.next;
            cur.next = copy;
            cur = copy.next;
        }

        cur = head;
        while (cur != null) {
            if (cur.random != null) {
                cur.next.random = cur.random.next;
            }
            cur = cur.next.next;
        }

        cur = head;
        Node newHead = head.next;
        while (cur != null) {
            Node copy = cur.next;
            cur.next = copy.next;
            copy.next = (copy.next == null) ? null : copy.next.next;
            cur = cur.next;
        }
        return newHead;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(1)`，不计返回链表。

#### 基础语法与算法思想

- `random` 指针可能为 `null`，补边时要判空。
- 穿插法能把“旧节点到新节点”的映射隐含在 `old.next` 里。
- 核心思想：先制造局部邻接关系，再用它完成复杂指针复制。

---

## 148. 排序链表 (Medium)

原题位置：`LeetCode_Collection_Part_5.md:1042`

### Java 解法补充

#### 基础解法：转数组排序后重建链表

算法思想：先把链表值收集到数组排序，再重新生成有序链表。实现简单，但没有利用链表结构。

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

class Solution {
    public ListNode sortList(ListNode head) {
        List<Integer> values = new ArrayList<>();
        while (head != null) {
            values.add(head.val);
            head = head.next;
        }
        Collections.sort(values);
        ListNode dummy = new ListNode(0);
        ListNode tail = dummy;
        for (int val : values) {
            tail.next = new ListNode(val);
            tail = tail.next;
        }
        return dummy.next;
    }
}
```

复杂度：时间 `O(n log n)`，空间 `O(n)`。

#### 资深解法：链表归并排序

算法思想：用快慢指针找中点把链表一分为二，分别排序后再归并，符合链表排序的主流最优解。

```java
class Solution {
    public ListNode sortList(ListNode head) {
        if (head == null || head.next == null) return head;
        ListNode slow = head, fast = head.next;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        ListNode mid = slow.next;
        slow.next = null;
        ListNode left = sortList(head);
        ListNode right = sortList(mid);
        return merge(left, right);
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
        tail.next = (a != null) ? a : b;
        return dummy.next;
    }
}
```

复杂度：时间 `O(n log n)`，空间 `O(log n)`，递归栈。

#### 基础语法与算法思想

- 链表适合归并，不适合像数组那样随机访问。
- `slow.next = null` 是切断链表的关键一步。
- 核心思想：分治后再合并，能把局部有序扩展成整体有序。

---

## 146. LRU 缓存 (Medium)

原题位置：`LeetCode_Collection_Part_5.md:963`

### Java 解法补充

#### 基础解法：LinkedHashMap 直接实现访问顺序

算法思想：Java 自带 `LinkedHashMap` 可以维护访问顺序，重写 `removeEldestEntry` 就能自动淘汰最久未使用元素。

```java
import java.util.LinkedHashMap;
import java.util.Map;

class LRUCache extends LinkedHashMap<Integer, Integer> {
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

    @Override
    protected boolean removeEldestEntry(Map.Entry<Integer, Integer> eldest) {
        return size() > capacity;
    }
}
```

复杂度：时间均摊 `O(1)`，空间 `O(capacity)`。

#### 资深解法：哈希表 + 双向链表

算法思想：哈希表负责按 key `O(1)` 找节点，双向链表维护最近使用顺序。访问或写入时把节点移到头部，超容量就删尾部。

```java
import java.util.HashMap;
import java.util.Map;

class LRUCache {
    private static class Node {
        int key;
        int value;
        Node prev;
        Node next;
        Node(int key, int value) {
            this.key = key;
            this.value = value;
        }
    }

    private final int capacity;
    private final Map<Integer, Node> map = new HashMap<>();
    private final Node head = new Node(0, 0);
    private final Node tail = new Node(0, 0);

    public LRUCache(int capacity) {
        this.capacity = capacity;
        head.next = tail;
        tail.prev = head;
    }

    public int get(int key) {
        Node node = map.get(key);
        if (node == null) return -1;
        moveToHead(node);
        return node.value;
    }

    public void put(int key, int value) {
        Node node = map.get(key);
        if (node != null) {
            node.value = value;
            moveToHead(node);
            return;
        }
        Node newNode = new Node(key, value);
        map.put(key, newNode);
        addFirst(newNode);
        if (map.size() > capacity) {
            Node removed = removeLast();
            map.remove(removed.key);
        }
    }

    private void moveToHead(Node node) {
        remove(node);
        addFirst(node);
    }

    private void addFirst(Node node) {
        node.next = head.next;
        node.prev = head;
        head.next.prev = node;
        head.next = node;
    }

    private void remove(Node node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }

    private Node removeLast() {
        Node node = tail.prev;
        remove(node);
        return node;
    }
}
```

复杂度：时间均摊 `O(1)`，空间 `O(capacity)`。

#### 基础语法与算法思想

- `LinkedHashMap` 是标准库级解法，适合快速实现。
- 面试里更常考哈希表 + 双向链表的底层实现。
- 核心思想：查询靠哈希，顺序维护靠链表。

---

## 94. 二叉树的中序遍历 (Easy)

原题位置：`LeetCode_Collection_Part4.md:120`

### Java 解法补充

#### 基础解法：递归中序遍历

算法思想：按照“左子树 -> 根节点 -> 右子树”的顺序递归访问。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> ans = new ArrayList<>();
        dfs(root, ans);
        return ans;
    }

    private void dfs(TreeNode node, List<Integer> ans) {
        if (node == null) return;
        dfs(node.left, ans);
        ans.add(node.val);
        dfs(node.right, ans);
    }
}
```

复杂度：时间 `O(n)`，空间 `O(h)`。

#### 资深解法：显式栈模拟递归

算法思想：不断向左入栈，弹出栈顶后访问节点，再转向其右子树。

```java
import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.Deque;
import java.util.List;

class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> ans = new ArrayList<>();
        Deque<TreeNode> stack = new ArrayDeque<>();
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

复杂度：时间 `O(n)`，空间 `O(h)`。

#### 基础语法与算法思想

- 树遍历的三种顺序只差访问根节点的位置。
- 迭代写法的核心是“沿左链入栈”。
- 核心思想：栈就是手动维护递归调用过程。

---

## 104. 二叉树的最大深度 (Easy)

原题位置：`LeetCode_Collection_Part_4.md:98`

### Java 解法补充

#### 基础解法：递归求左右子树最大值

算法思想：当前节点深度等于 `1 + max(leftDepth, rightDepth)`。

```java
class Solution {
    public int maxDepth(TreeNode root) {
        if (root == null) return 0;
        return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
    }
}
```

复杂度：时间 `O(n)`，空间 `O(h)`。

#### 资深解法：层序遍历按层计数

算法思想：BFS 每处理完一层，深度加一，最后得到树高。

```java
import java.util.ArrayDeque;
import java.util.Queue;

class Solution {
    public int maxDepth(TreeNode root) {
        if (root == null) return 0;
        Queue<TreeNode> queue = new ArrayDeque<>();
        queue.offer(root);
        int depth = 0;
        while (!queue.isEmpty()) {
            int size = queue.size();
            depth++;
            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                if (node.left != null) queue.offer(node.left);
                if (node.right != null) queue.offer(node.right);
            }
        }
        return depth;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 基础语法与算法思想

- 递归版更短，层序版更直观看到“层高”。
- `queue.size()` 先存下来，避免本层循环中队列大小变化。
- 核心思想：树高既可以自顶向下按层算，也可以自底向上递归算。

---

## 226. 翻转二叉树 (Easy)

原题位置：`LeetCode_Collection_Part_8.md:554`

### Java 解法补充

#### 基础解法：递归交换左右子树

算法思想：对每个节点交换 `left` 和 `right`，并递归处理两个孩子。

```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if (root == null) return null;
        TreeNode temp = root.left;
        root.left = invertTree(root.right);
        root.right = invertTree(temp);
        return root;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(h)`。

#### 资深解法：队列迭代层序翻转

算法思想：用队列逐层遍历，每弹出一个节点就交换左右孩子。

```java
import java.util.ArrayDeque;
import java.util.Queue;

class Solution {
    public TreeNode invertTree(TreeNode root) {
        if (root == null) return null;
        Queue<TreeNode> queue = new ArrayDeque<>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            TreeNode node = queue.poll();
            TreeNode temp = node.left;
            node.left = node.right;
            node.right = temp;
            if (node.left != null) queue.offer(node.left);
            if (node.right != null) queue.offer(node.right);
        }
        return root;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 基础语法与算法思想

- 交换树结构时先用临时变量保存一边。
- 递归和 BFS 都是遍历整棵树，差别在访问顺序。
- 核心思想：每个节点的局部操作都相同，天然适合递归。

---

## 101. 对称二叉树 (Easy)

原题位置：`LeetCode_Collection_Part_4.md:3`

### Java 解法补充

#### 基础解法：递归判断镜像

算法思想：两棵子树镜像对称，当且仅当根值相同，且“左的左 == 右的右、左的右 == 右的左”。

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        return root == null || same(root.left, root.right);
    }

    private boolean same(TreeNode a, TreeNode b) {
        if (a == null || b == null) return a == b;
        if (a.val != b.val) return false;
        return same(a.left, b.right) && same(a.right, b.left);
    }
}
```

复杂度：时间 `O(n)`，空间 `O(h)`。

#### 资深解法：队列成对比较

算法思想：把应该互为镜像的两个节点成对入队，出队时检查值和位置关系。

```java
import java.util.ArrayDeque;
import java.util.Queue;

class Solution {
    public boolean isSymmetric(TreeNode root) {
        if (root == null) return true;
        Queue<TreeNode> queue = new ArrayDeque<>();
        queue.offer(root.left);
        queue.offer(root.right);
        while (!queue.isEmpty()) {
            TreeNode a = queue.poll();
            TreeNode b = queue.poll();
            if (a == null && b == null) continue;
            if (a == null || b == null || a.val != b.val) return false;
            queue.offer(a.left);
            queue.offer(b.right);
            queue.offer(a.right);
            queue.offer(b.left);
        }
        return true;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 基础语法与算法思想

- 对称判断不是普通相等，而是镜像相等。
- 递归里参数通常是两个节点，而不是一个节点。
- 核心思想：始终成对维护“应该互相对应”的位置。

---

## 543. 二叉树的直径 (Easy)

原题位置：`LeetCode_Collection_Part_19.md:65`

### Java 解法补充

#### 基础解法：枚举每个节点为最高点

算法思想：对每个节点计算左高和右高，直径候选值是 `left + right`，再递归地在左右子树中找最大值。

```java
class Solution {
    public int diameterOfBinaryTree(TreeNode root) {
        if (root == null) return 0;
        int throughRoot = depth(root.left) + depth(root.right);
        int left = diameterOfBinaryTree(root.left);
        int right = diameterOfBinaryTree(root.right);
        return Math.max(throughRoot, Math.max(left, right));
    }

    private int depth(TreeNode node) {
        if (node == null) return 0;
        return 1 + Math.max(depth(node.left), depth(node.right));
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(h)`。

#### 资深解法：一次后序遍历同时求深度和直径

算法思想：后序遍历时，递归返回当前节点深度；同时用全局变量维护 `leftDepth + rightDepth` 的最大值。

```java
class Solution {
    private int ans = 0;

    public int diameterOfBinaryTree(TreeNode root) {
        depth(root);
        return ans;
    }

    private int depth(TreeNode node) {
        if (node == null) return 0;
        int left = depth(node.left);
        int right = depth(node.right);
        ans = Math.max(ans, left + right);
        return 1 + Math.max(left, right);
    }
}
```

复杂度：时间 `O(n)`，空间 `O(h)`。

#### 基础语法与算法思想

- 直径按题意统计的是边数，所以候选值是 `leftDepth + rightDepth`。
- 后序遍历很适合处理“先知道孩子信息，再算父节点”的树题。
- 核心思想：把多个子问题合并在同一次 DFS 里完成。

---

## 102. 二叉树的层序遍历 (Medium)

原题位置：`LeetCode_Collection_Part_4.md:32`

### Java 解法补充

#### 基础解法：队列按层 BFS

算法思想：每次先记录当前层节点数，再循环弹出这一层的节点并把下一层孩子入队。

```java
import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.List;
import java.util.Queue;

class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> ans = new ArrayList<>();
        if (root == null) return ans;
        Queue<TreeNode> queue = new ArrayDeque<>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            List<Integer> level = new ArrayList<>();
            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                level.add(node.val);
                if (node.left != null) queue.offer(node.left);
                if (node.right != null) queue.offer(node.right);
            }
            ans.add(level);
        }
        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 资深解法：DFS 带层号收集

算法思想：深度优先遍历时把当前深度 `depth` 传下去，如果这是第一次到达该层，就先创建新列表。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> ans = new ArrayList<>();
        dfs(root, 0, ans);
        return ans;
    }

    private void dfs(TreeNode node, int depth, List<List<Integer>> ans) {
        if (node == null) return;
        if (depth == ans.size()) ans.add(new ArrayList<>());
        ans.get(depth).add(node.val);
        dfs(node.left, depth + 1, ans);
        dfs(node.right, depth + 1, ans);
    }
}
```

复杂度：时间 `O(n)`，空间 `O(h)`。

#### 基础语法与算法思想

- 层序遍历的标准解法是 BFS。
- DFS 也能做，只要把层号传递下去。
- 核心思想：每个节点都知道自己属于第几层。

---

## 108. 将有序数组转换为二叉搜索树 (Easy)

原题位置：`LeetCode_Collection_Part_4.md:222`

### Java 解法补充

#### 基础解法：递归选中点建树

算法思想：中点作为根节点，左半段递归构造左子树，右半段递归构造右子树，就能保证 BST 和高度平衡。

```java
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        return build(nums, 0, nums.length - 1);
    }

    private TreeNode build(int[] nums, int left, int right) {
        if (left > right) return null;
        int mid = left + (right - left) / 2;
        TreeNode root = new TreeNode(nums[mid]);
        root.left = build(nums, left, mid - 1);
        root.right = build(nums, mid + 1, right);
        return root;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(log n)`。

#### 资深解法：右中点写法避免固定偏左

算法思想：仍然递归取中点，但长度为偶数时取右中点，让树形更均衡一些，写法上只是中点选择略有不同。

```java
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        return build(nums, 0, nums.length - 1);
    }

    private TreeNode build(int[] nums, int left, int right) {
        if (left > right) return null;
        int mid = left + (right - left + 1) / 2;
        TreeNode root = new TreeNode(nums[mid]);
        root.left = build(nums, left, mid - 1);
        root.right = build(nums, mid + 1, right);
        return root;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(log n)`。

#### 基础语法与算法思想

- 有序数组转 BST 的关键是“选中点做根”。
- 这题是典型分治，左右区间天然对应左右子树。
- 核心思想：中点切分能同时满足 BST 和平衡性。

---

## 98. 验证二叉搜索树 (Medium)

原题位置：`LeetCode_Collection_Part4.md:252`

### Java 解法补充

#### 基础解法：中序遍历后检查是否严格递增

算法思想：BST 的中序遍历序列必须严格递增。先收集到列表，再逐项比较。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public boolean isValidBST(TreeNode root) {
        List<Integer> order = new ArrayList<>();
        inorder(root, order);
        for (int i = 1; i < order.size(); i++) {
            if (order.get(i) <= order.get(i - 1)) return false;
        }
        return true;
    }

    private void inorder(TreeNode node, List<Integer> order) {
        if (node == null) return;
        inorder(node.left, order);
        order.add(node.val);
        inorder(node.right, order);
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 资深解法：递归维护合法取值范围

算法思想：每个节点都必须落在 `(lower, upper)` 范围内，左子树上界变成当前值，右子树下界变成当前值。

```java
class Solution {
    public boolean isValidBST(TreeNode root) {
        return valid(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }

    private boolean valid(TreeNode node, long lower, long upper) {
        if (node == null) return true;
        if (node.val <= lower || node.val >= upper) return false;
        return valid(node.left, lower, node.val) && valid(node.right, node.val, upper);
    }
}
```

复杂度：时间 `O(n)`，空间 `O(h)`。

#### 基础语法与算法思想

- 这里用 `long` 是为了避免节点值在边界时溢出。
- “左小右大”不只看父子节点，还要满足整棵子树范围约束。
- 核心思想：BST 的限制是从祖先一路传下来的。

---

## 230. 二叉搜索树中第 K 小的元素 (Medium)

原题位置：`LeetCode_Collection_Part_8.md:706`

### Java 解法补充

#### 基础解法：中序遍历后取第 k 个

算法思想：BST 中序遍历天然有序，遍历完后直接取第 `k-1` 个位置。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public int kthSmallest(TreeNode root, int k) {
        List<Integer> order = new ArrayList<>();
        inorder(root, order);
        return order.get(k - 1);
    }

    private void inorder(TreeNode node, List<Integer> order) {
        if (node == null) return;
        inorder(node.left, order);
        order.add(node.val);
        inorder(node.right, order);
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 资深解法：迭代中序遍历到第 k 个就停止

算法思想：不必遍历完整棵树，中序过程中每弹出一个节点就让 `k--`，当 `k == 0` 时直接返回。

```java
import java.util.ArrayDeque;
import java.util.Deque;

class Solution {
    public int kthSmallest(TreeNode root, int k) {
        Deque<TreeNode> stack = new ArrayDeque<>();
        TreeNode cur = root;
        while (cur != null || !stack.isEmpty()) {
            while (cur != null) {
                stack.push(cur);
                cur = cur.left;
            }
            cur = stack.pop();
            if (--k == 0) return cur.val;
            cur = cur.right;
        }
        return -1;
    }
}
```

复杂度：时间 `O(h+k)`，空间 `O(h)`。

#### 基础语法与算法思想

- `k` 递减是很常见的“按访问顺序找第 k 个”写法。
- BST 的第 k 小和中序顺序直接对应。
- 核心思想：有序性质能让搜索提前停止。

---

## 199. 二叉树的右视图 (Medium)

原题位置：`LeetCode_Collection_Part_7.md:763`

### Java 解法补充

#### 基础解法：层序遍历取每层最后一个节点

算法思想：BFS 处理每一层时，最后一个出队的节点就是这一层从右边看到的节点。

```java
import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.List;
import java.util.Queue;

class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> ans = new ArrayList<>();
        if (root == null) return ans;
        Queue<TreeNode> queue = new ArrayDeque<>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                if (node.left != null) queue.offer(node.left);
                if (node.right != null) queue.offer(node.right);
                if (i == size - 1) ans.add(node.val);
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 资深解法：DFS 先右后左

算法思想：深度优先遍历时总是先走右子树。某一层第一次访问到的节点，就是该层右视图节点。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> ans = new ArrayList<>();
        dfs(root, 0, ans);
        return ans;
    }

    private void dfs(TreeNode node, int depth, List<Integer> ans) {
        if (node == null) return;
        if (depth == ans.size()) ans.add(node.val);
        dfs(node.right, depth + 1, ans);
        dfs(node.left, depth + 1, ans);
    }
}
```

复杂度：时间 `O(n)`，空间 `O(h)`。

#### 基础语法与算法思想

- BFS 和 DFS 都能做，关键是如何定义“这一层最右边”。
- `depth == ans.size()` 表示第一次到达该层。
- 核心思想：优先遍历右侧时，第一次见到的就是右视图。

---

## 114. 二叉树展开为链表 (Medium)

原题位置：`LeetCode_Collection_Part_4.md:416`

### Java 解法补充

#### 基础解法：先前序遍历收集节点再重连

算法思想：前序遍历得到节点顺序，然后把每个节点的 `left` 置空、`right` 指向下一个节点。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public void flatten(TreeNode root) {
        List<TreeNode> nodes = new ArrayList<>();
        preorder(root, nodes);
        for (int i = 1; i < nodes.size(); i++) {
            TreeNode prev = nodes.get(i - 1);
            TreeNode cur = nodes.get(i);
            prev.left = null;
            prev.right = cur;
        }
    }

    private void preorder(TreeNode node, List<TreeNode> nodes) {
        if (node == null) return;
        nodes.add(node);
        preorder(node.left, nodes);
        preorder(node.right, nodes);
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 资深解法：后序展开并接回右链

算法思想：先递归展开左右子树，再把左子树整体接到右边，原来的右子树接到新右链的末尾。

```java
class Solution {
    public void flatten(TreeNode root) {
        if (root == null) return;
        flatten(root.left);
        flatten(root.right);

        TreeNode left = root.left;
        TreeNode right = root.right;
        root.left = null;
        root.right = left;

        TreeNode cur = root;
        while (cur.right != null) cur = cur.right;
        cur.right = right;
    }
}
```

复杂度：时间最坏 `O(n^2)`，空间 `O(h)`。

#### 基础语法与算法思想

- 题目要求原地修改，所以最后所有 `left` 都必须为 `null`。
- 展开后的顺序必须是前序遍历顺序。
- 核心思想：把树结构改写成单链表结构。

---

## 105. 从前序与中序遍历序列构造二叉树 (Medium)

原题位置：`LeetCode_Collection_Part_4.md:127`

### Java 解法补充

#### 基础解法：递归切分子数组

算法思想：前序第一个值一定是根，在中序中找到根的位置后，就能知道左右子树的范围。

```java
class Solution {
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        return build(preorder, 0, preorder.length - 1, inorder, 0, inorder.length - 1);
    }

    private TreeNode build(int[] preorder, int pl, int pr, int[] inorder, int il, int ir) {
        if (pl > pr) return null;
        int rootVal = preorder[pl];
        int idx = il;
        while (inorder[idx] != rootVal) idx++;
        int leftSize = idx - il;
        TreeNode root = new TreeNode(rootVal);
        root.left = build(preorder, pl + 1, pl + leftSize, inorder, il, idx - 1);
        root.right = build(preorder, pl + leftSize + 1, pr, inorder, idx + 1, ir);
        return root;
    }
}
```

复杂度：时间 `O(n^2)`，空间 `O(h)`。

#### 资深解法：哈希表加速中序定位

算法思想：用哈希表保存中序数组中每个值的位置，把每次找根节点位置降到 `O(1)`。

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    private Map<Integer, Integer> index = new HashMap<>();

    public TreeNode buildTree(int[] preorder, int[] inorder) {
        for (int i = 0; i < inorder.length; i++) index.put(inorder[i], i);
        return build(preorder, 0, preorder.length - 1, 0, inorder.length - 1);
    }

    private TreeNode build(int[] preorder, int pl, int pr, int il, int ir) {
        if (pl > pr) return null;
        int rootVal = preorder[pl];
        int idx = index.get(rootVal);
        int leftSize = idx - il;
        TreeNode root = new TreeNode(rootVal);
        root.left = build(preorder, pl + 1, pl + leftSize, il, idx - 1);
        root.right = build(preorder, pl + leftSize + 1, pr, idx + 1, ir);
        return root;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 基础语法与算法思想

- 前序确定根，中序确定左右边界。
- `leftSize` 是分治切分的核心中间量。
- 核心思想：利用遍历顺序反推出树的结构。

---

## 437. 路径总和 III (Medium)

原题位置：`LeetCode_Collection_Part_15.md:464`

### Java 解法补充

#### 基础解法：以每个节点为起点向下 DFS

算法思想：枚举每个节点作为路径起点，再向下统计和为 `targetSum` 的路径数。

```java
class Solution {
    public int pathSum(TreeNode root, int targetSum) {
        if (root == null) return 0;
        return count(root, targetSum) + pathSum(root.left, targetSum) + pathSum(root.right, targetSum);
    }

    private int count(TreeNode node, long target) {
        if (node == null) return 0;
        int ans = (node.val == target) ? 1 : 0;
        ans += count(node.left, target - node.val);
        ans += count(node.right, target - node.val);
        return ans;
    }
}
```

复杂度：时间最坏 `O(n^2)`，空间 `O(h)`。

#### 资深解法：前缀和 + 哈希表

算法思想：根到当前节点前缀和为 `sum` 时，只要之前出现过 `sum - target`，就说明存在若干条以当前节点结尾的合法路径。

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int pathSum(TreeNode root, int targetSum) {
        Map<Long, Integer> prefix = new HashMap<>();
        prefix.put(0L, 1);
        return dfs(root, 0L, targetSum, prefix);
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

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 基础语法与算法思想

- 回溯返回父节点前要撤销当前前缀和出现次数。
- 树上的前缀和和数组前缀和思路一致，只是遍历方式改成 DFS。
- 核心思想：把“任意起点到当前点”的路径计数转成前缀和差值。

---

## 236. 二叉树的最近公共祖先 (Medium)

原题位置：`LeetCode_Collection_Part_8.md:908`

### Java 解法补充

#### 基础解法：记录根到目标节点路径

算法思想：分别找到根到 `p`、`q` 的路径，最后比较两条路径的最长公共前缀，末尾节点就是 LCA。

```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        List<TreeNode> pathP = new ArrayList<>();
        List<TreeNode> pathQ = new ArrayList<>();
        find(root, p, pathP);
        find(root, q, pathQ);
        TreeNode ans = null;
        for (int i = 0; i < Math.min(pathP.size(), pathQ.size()); i++) {
            if (pathP.get(i) == pathQ.get(i)) ans = pathP.get(i);
            else break;
        }
        return ans;
    }

    private boolean find(TreeNode node, TreeNode target, List<TreeNode> path) {
        if (node == null) return false;
        path.add(node);
        if (node == target) return true;
        if (find(node.left, target, path) || find(node.right, target, path)) return true;
        path.remove(path.size() - 1);
        return false;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(n)`。

#### 资深解法：后序递归返回命中情况

算法思想：若当前节点就是 `p` 或 `q`，直接返回；否则递归左右子树。若左右都命中，当前节点就是最近公共祖先。

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (root == null || root == p || root == q) return root;
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if (left != null && right != null) return root;
        return left != null ? left : right;
    }
}
```

复杂度：时间 `O(n)`，空间 `O(h)`。

#### 基础语法与算法思想

- 递归返回值表示“这个子树里找到的结果”。
- 当左右都非空时，说明 `p` 和 `q` 分布在当前节点两侧。
- 核心思想：LCA 是后序信息汇总的典型题。

---

## 124. 二叉树中的最大路径和 (Hard)

原题位置：`LeetCode_Collection_Part_5.md:123`

### Java 解法补充

#### 基础解法：递归计算单边最大贡献

算法思想：对每个节点，向父节点返回“从当前节点出发向下延伸的最大单边路径和”；同时尝试用“左贡献 + 当前值 + 右贡献”更新全局答案。

```java
class Solution {
    private int ans = Integer.MIN_VALUE;

    public int maxPathSum(TreeNode root) {
        gain(root);
        return ans;
    }

    private int gain(TreeNode node) {
        if (node == null) return 0;
        int left = Math.max(gain(node.left), 0);
        int right = Math.max(gain(node.right), 0);
        ans = Math.max(ans, node.val + left + right);
        return node.val + Math.max(left, right);
    }
}
```

复杂度：时间 `O(n)`，空间 `O(h)`。

#### 资深解法：写成显式“贡献值”语义

算法思想：和上面同一思路，但把“负贡献直接舍弃”单独抽出来，代码语义更接近题意。

```java
class Solution {
    private int best = Integer.MIN_VALUE;

    public int maxPathSum(TreeNode root) {
        contribution(root);
        return best;
    }

    private int contribution(TreeNode node) {
        if (node == null) return 0;
        int left = positive(contribution(node.left));
        int right = positive(contribution(node.right));
        best = Math.max(best, node.val + left + right);
        return node.val + Math.max(left, right);
    }

    private int positive(int x) {
        return Math.max(x, 0);
    }
}
```

复杂度：时间 `O(n)`，空间 `O(h)`。

#### 基础语法与算法思想

- 向父节点返回的路径不能同时选左右两边，只能选一边。
- 全局最优路径可能完全不经过根节点。
- 核心思想：区分“全局答案候选”和“递归返回值”的含义。

---

## 200. 岛屿数量 (Medium)

原题位置：`LeetCode_Collection_Part_7.md:797`

### Java 解法补充

#### 基础解法：DFS 淹没整座岛

算法思想：扫到一个陆地 `'1'` 就把答案加一，再用 DFS 把与它连通的整块陆地都标记成已访问。

```java
class Solution {
    public int numIslands(char[][] grid) {
        int ans = 0;
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (grid[i][j] == '1') {
                    ans++;
                    dfs(grid, i, j);
                }
            }
        }
        return ans;
    }

    private void dfs(char[][] grid, int i, int j) {
        if (i < 0 || i >= grid.length || j < 0 || j >= grid[0].length || grid[i][j] != '1') {
            return;
        }
        grid[i][j] = '0';
        dfs(grid, i + 1, j);
        dfs(grid, i - 1, j);
        dfs(grid, i, j + 1);
        dfs(grid, i, j - 1);
    }
}
```

复杂度：时间 `O(mn)`，空间 `O(mn)`，递归最坏栈。

#### 资深解法：BFS 按块扩展

算法思想：思路和 DFS 一样，只是把递归改成队列扩展，避免深递归风险。

```java
import java.util.ArrayDeque;
import java.util.Queue;

class Solution {
    public int numIslands(char[][] grid) {
        int ans = 0;
        int m = grid.length, n = grid[0].length;
        int[] dx = {1, -1, 0, 0};
        int[] dy = {0, 0, 1, -1};

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] != '1') continue;
                ans++;
                Queue<int[]> queue = new ArrayDeque<>();
                queue.offer(new int[]{i, j});
                grid[i][j] = '0';
                while (!queue.isEmpty()) {
                    int[] cur = queue.poll();
                    for (int k = 0; k < 4; k++) {
                        int x = cur[0] + dx[k];
                        int y = cur[1] + dy[k];
                        if (x >= 0 && x < m && y >= 0 && y < n && grid[x][y] == '1') {
                            grid[x][y] = '0';
                            queue.offer(new int[]{x, y});
                        }
                    }
                }
            }
        }
        return ans;
    }
}
```

复杂度：时间 `O(mn)`，空间 `O(mn)`。

#### 基础语法与算法思想

- 扫描网格时，发现新陆地就意味着发现一座新岛。
- 把访问过的陆地改成 `'0'`，可以省掉 `visited` 数组。
- 核心思想：连通块计数 = 发现起点次数。

---

## 994. 腐烂的橘子 (Medium)

原题位置：`LeetCode_Collection_Part_34.md:113`

### Java 解法补充

#### 基础解法：每分钟全图扫描一遍

算法思想：每轮扫描所有腐烂橘子，把相邻新鲜橘子标记为本轮将要腐烂。重复直到没有变化。

```java
class Solution {
    public int orangesRotting(int[][] grid) {
        int minutes = 0;
        while (true) {
            boolean changed = false;
            int m = grid.length, n = grid[0].length;
            int[][] next = new int[m][n];
            for (int i = 0; i < m; i++) {
                System.arraycopy(grid[i], 0, next[i], 0, n);
            }
            for (int i = 0; i < m; i++) {
                for (int j = 0; j < n; j++) {
                    if (grid[i][j] == 2) {
                        if (i > 0 && grid[i - 1][j] == 1) { next[i - 1][j] = 2; changed = true; }
                        if (i + 1 < m && grid[i + 1][j] == 1) { next[i + 1][j] = 2; changed = true; }
                        if (j > 0 && grid[i][j - 1] == 1) { next[i][j - 1] = 2; changed = true; }
                        if (j + 1 < n && grid[i][j + 1] == 1) { next[i][j + 1] = 2; changed = true; }
                    }
                }
            }
            if (!changed) break;
            grid = next;
            minutes++;
        }
        for (int[] row : grid) {
            for (int cell : row) {
                if (cell == 1) return -1;
            }
        }
        return minutes;
    }
}
```

复杂度：时间 `O((mn)^2)`，空间 `O(mn)`。

#### 资深解法：多源 BFS 按层扩散

算法思想：所有初始腐烂橘子同时作为 BFS 起点，按层向外扩散。层数就是分钟数。

```java
import java.util.ArrayDeque;
import java.util.Queue;

class Solution {
    public int orangesRotting(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        Queue<int[]> queue = new ArrayDeque<>();
        int fresh = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 2) queue.offer(new int[]{i, j});
                if (grid[i][j] == 1) fresh++;
            }
        }
        int minutes = 0;
        int[] dx = {1, -1, 0, 0};
        int[] dy = {0, 0, 1, -1};
        while (!queue.isEmpty() && fresh > 0) {
            int size = queue.size();
            minutes++;
            for (int s = 0; s < size; s++) {
                int[] cur = queue.poll();
                for (int k = 0; k < 4; k++) {
                    int x = cur[0] + dx[k];
                    int y = cur[1] + dy[k];
                    if (x >= 0 && x < m && y >= 0 && y < n && grid[x][y] == 1) {
                        grid[x][y] = 2;
                        fresh--;
                        queue.offer(new int[]{x, y});
                    }
                }
            }
        }
        return fresh == 0 ? minutes : -1;
    }
}
```

复杂度：时间 `O(mn)`，空间 `O(mn)`。

#### 基础语法与算法思想

- 这是典型的多源最短扩散问题。
- `fresh` 用于判断是否还有没被感染的新鲜橘子。
- 核心思想：所有腐烂源点同时出发，层数就是时间。

---

## 207. 课程表 (Medium)
原题位置：`LeetCode_Collection_Part_7.md:1047`
### Java 解法补充
#### 基础解法：DFS 判环
算法思想：把课程图看成有向图，`0/1/2` 标记未访问、访问中、已完成；DFS 过程中再次遇到“访问中”节点就有环。
```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        List<Integer>[] graph = new ArrayList[numCourses];
        for (int i = 0; i < numCourses; i++) graph[i] = new ArrayList<>();
        for (int[] e : prerequisites) graph[e[1]].add(e[0]);
        int[] state = new int[numCourses];
        for (int i = 0; i < numCourses; i++) {
            if (!dfs(graph, state, i)) return false;
        }
        return true;
    }
    private boolean dfs(List<Integer>[] graph, int[] state, int u) {
        if (state[u] == 1) return false;
        if (state[u] == 2) return true;
        state[u] = 1;
        for (int v : graph[u]) if (!dfs(graph, state, v)) return false;
        state[u] = 2;
        return true;
    }
}
```
复杂度：时间 `O(V+E)`，空间 `O(V+E)`。
#### 资深解法：拓扑排序
算法思想：统计入度，把入度为 0 的课入队，不断删边；最终若处理课程数等于总数，则无环。
```java
import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.List;
import java.util.Queue;

class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        List<Integer>[] graph = new ArrayList[numCourses];
        for (int i = 0; i < numCourses; i++) graph[i] = new ArrayList<>();
        int[] indegree = new int[numCourses];
        for (int[] e : prerequisites) {
            graph[e[1]].add(e[0]);
            indegree[e[0]]++;
        }
        Queue<Integer> queue = new ArrayDeque<>();
        for (int i = 0; i < numCourses; i++) if (indegree[i] == 0) queue.offer(i);
        int count = 0;
        while (!queue.isEmpty()) {
            int u = queue.poll();
            count++;
            for (int v : graph[u]) if (--indegree[v] == 0) queue.offer(v);
        }
        return count == numCourses;
    }
}
```
复杂度：时间 `O(V+E)`，空间 `O(V+E)`。
#### 基础语法与算法思想
- 判是否能学完，本质是判有向图是否有环。
- 拓扑排序处理完所有点，等价于图无环。
- 核心思想：课程依赖就是典型 DAG / 有向环问题。
---

## 208. 实现 Trie (前缀树) (Medium)
原题位置：`LeetCode_Collection_Part_7.md:1083`
### Java 解法补充
#### 基础解法：节点数组存 26 个孩子
算法思想：每个节点维护 26 个子节点指针，插入和查询都按字符逐层向下走。
```java
class Trie {
    private static class Node {
        Node[] next = new Node[26];
        boolean end;
    }
    private final Node root = new Node();

    public void insert(String word) {
        Node cur = root;
        for (char c : word.toCharArray()) {
            int idx = c - 'a';
            if (cur.next[idx] == null) cur.next[idx] = new Node();
            cur = cur.next[idx];
        }
        cur.end = true;
    }

    public boolean search(String word) {
        Node node = find(word);
        return node != null && node.end;
    }

    public boolean startsWith(String prefix) {
        return find(prefix) != null;
    }

    private Node find(String s) {
        Node cur = root;
        for (char c : s.toCharArray()) {
            cur = cur.next[c - 'a'];
            if (cur == null) return null;
        }
        return cur;
    }
}
```
复杂度：单次操作时间 `O(L)`，空间与总字符数成正比。
#### 资深解法：HashMap 孩子表
算法思想：如果字符集更大，可以把固定数组换成哈希表，节省稀疏节点空间。
```java
import java.util.HashMap;
import java.util.Map;

class Trie {
    private static class Node {
        Map<Character, Node> next = new HashMap<>();
        boolean end;
    }
    private final Node root = new Node();

    public void insert(String word) {
        Node cur = root;
        for (char c : word.toCharArray()) {
            cur.next.putIfAbsent(c, new Node());
            cur = cur.next.get(c);
        }
        cur.end = true;
    }

    public boolean search(String word) {
        Node node = find(word);
        return node != null && node.end;
    }

    public boolean startsWith(String prefix) {
        return find(prefix) != null;
    }

    private Node find(String s) {
        Node cur = root;
        for (char c : s.toCharArray()) {
            cur = cur.next.get(c);
            if (cur == null) return null;
        }
        return cur;
    }
}
```
复杂度：单次操作时间均摊 `O(L)`。
#### 基础语法与算法思想
- Trie 适合处理前缀匹配。
- `search` 和 `startsWith` 的区别只在是否要求单词结尾。
- 核心思想：把公共前缀压成共享路径。
---

## 78. 子集 (Medium)
原题位置：`LeetCode_Collection_Part3.md:667`
### Java 解法补充
#### 基础解法：位运算枚举
算法思想：长度为 `n` 的数组共有 `2^n` 个子集，用二进制位决定每个元素取不取。
```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> ans = new ArrayList<>();
        int total = 1 << nums.length;
        for (int mask = 0; mask < total; mask++) {
            List<Integer> cur = new ArrayList<>();
            for (int i = 0; i < nums.length; i++) {
                if (((mask >> i) & 1) == 1) cur.add(nums[i]);
            }
            ans.add(cur);
        }
        return ans;
    }
}
```
复杂度：时间 `O(n2^n)`，空间 `O(n)`。
#### 资深解法：回溯
算法思想：每个位置只有“选”或“不选”两种决策，DFS 遍历整棵决策树。
```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> ans = new ArrayList<>();
        dfs(nums, 0, new ArrayList<>(), ans);
        return ans;
    }
    private void dfs(int[] nums, int index, List<Integer> path, List<List<Integer>> ans) {
        ans.add(new ArrayList<>(path));
        for (int i = index; i < nums.length; i++) {
            path.add(nums[i]);
            dfs(nums, i + 1, path, ans);
            path.remove(path.size() - 1);
        }
    }
}
```
复杂度：时间 `O(n2^n)`，空间 `O(n)`。
#### 基础语法与算法思想
- 子集问题不要求顺序，只关心选择集合。
- 回溯里加入答案时要拷贝 `path`。
- 核心思想：枚举所有选择状态。
---

## 79. 单词搜索 (Medium)
原题位置：`LeetCode_Collection_Part3.md:695`
### Java 解法补充
#### 基础解法：DFS + visited
算法思想：从每个起点出发，按上下左右 DFS 匹配下一个字符，用 `visited` 防止重复使用格子。
```java
class Solution {
    public boolean exist(char[][] board, String word) {
        int m = board.length, n = board[0].length;
        boolean[][] visited = new boolean[m][n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (dfs(board, word, i, j, 0, visited)) return true;
            }
        }
        return false;
    }
    private boolean dfs(char[][] board, String word, int i, int j, int k, boolean[][] visited) {
        if (k == word.length()) return true;
        if (i < 0 || i >= board.length || j < 0 || j >= board[0].length) return false;
        if (visited[i][j] || board[i][j] != word.charAt(k)) return false;
        visited[i][j] = true;
        boolean ok = dfs(board, word, i + 1, j, k + 1, visited)
                || dfs(board, word, i - 1, j, k + 1, visited)
                || dfs(board, word, i, j + 1, k + 1, visited)
                || dfs(board, word, i, j - 1, k + 1, visited);
        visited[i][j] = false;
        return ok;
    }
}
```
复杂度：时间最坏 `O(mn*4^L)`，空间 `O(mn)`。
#### 资深解法：原地标记
算法思想：不单开 `visited`，进入格子时暂时改成特殊字符，回溯时恢复。
```java
class Solution {
    public boolean exist(char[][] board, String word) {
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                if (dfs(board, word, i, j, 0)) return true;
            }
        }
        return false;
    }
    private boolean dfs(char[][] board, String word, int i, int j, int k) {
        if (k == word.length()) return true;
        if (i < 0 || i >= board.length || j < 0 || j >= board[0].length || board[i][j] != word.charAt(k)) return false;
        char saved = board[i][j];
        board[i][j] = '#';
        boolean ok = dfs(board, word, i + 1, j, k + 1)
                || dfs(board, word, i - 1, j, k + 1)
                || dfs(board, word, i, j + 1, k + 1)
                || dfs(board, word, i, j - 1, k + 1);
        board[i][j] = saved;
        return ok;
    }
}
```
复杂度：时间最坏 `O(mn*4^L)`，空间 `O(L)`。
#### 基础语法与算法思想
- 匹配到第 `k` 个字符时，下一步要找 `k+1`。
- 回溯题一定要恢复现场。
- 核心思想：路径搜索 + 状态撤销。
---

## 131. 分割回文串 (Medium)
原题位置：`LeetCode_Collection_Part_5.md:385`
### Java 解法补充
#### 基础解法：回溯 + 在线判断回文
算法思想：枚举每个切分点，只在子串是回文时继续向下递归。
```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<List<String>> partition(String s) {
        List<List<String>> ans = new ArrayList<>();
        dfs(s, 0, new ArrayList<>(), ans);
        return ans;
    }
    private void dfs(String s, int start, List<String> path, List<List<String>> ans) {
        if (start == s.length()) {
            ans.add(new ArrayList<>(path));
            return;
        }
        for (int end = start; end < s.length(); end++) {
            if (!isPalindrome(s, start, end)) continue;
            path.add(s.substring(start, end + 1));
            dfs(s, end + 1, path, ans);
            path.remove(path.size() - 1);
        }
    }
    private boolean isPalindrome(String s, int l, int r) {
        while (l < r) if (s.charAt(l++) != s.charAt(r--)) return false;
        return true;
    }
}
```
复杂度：时间较高，最坏近 `O(n*2^n)`。
#### 资深解法：DP 预处理回文表 + 回溯
算法思想：先用 DP 预处理任意区间是否为回文，回溯时就能 `O(1)` 查询。
```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<List<String>> partition(String s) {
        int n = s.length();
        boolean[][] pal = new boolean[n][n];
        for (int i = n - 1; i >= 0; i--) {
            for (int j = i; j < n; j++) {
                pal[i][j] = s.charAt(i) == s.charAt(j) && (j - i < 2 || pal[i + 1][j - 1]);
            }
        }
        List<List<String>> ans = new ArrayList<>();
        dfs(s, 0, pal, new ArrayList<>(), ans);
        return ans;
    }
    private void dfs(String s, int start, boolean[][] pal, List<String> path, List<List<String>> ans) {
        if (start == s.length()) { ans.add(new ArrayList<>(path)); return; }
        for (int end = start; end < s.length(); end++) {
            if (!pal[start][end]) continue;
            path.add(s.substring(start, end + 1));
            dfs(s, end + 1, pal, path, ans);
            path.remove(path.size() - 1);
        }
    }
}
```
复杂度：预处理 `O(n^2)`，搜索取决于答案规模。
#### 基础语法与算法思想
- 切分题常见状态是“当前从哪个下标开始切”。
- 回文判断可以预处理，减少重复工作。
- 核心思想：合法前缀 + 递归处理后缀。
---

## 74. 搜索二维矩阵 (Medium)
原题位置：`LeetCode_Collection_Part3.md:521`
### Java 解法补充
#### 基础解法：逐行二分
算法思想：先判断目标值是否可能落在某一行，再在该行内部二分。
```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        for (int[] row : matrix) {
            if (target < row[0] || target > row[row.length - 1]) continue;
            int l = 0, r = row.length - 1;
            while (l <= r) {
                int m = l + (r - l) / 2;
                if (row[m] == target) return true;
                if (row[m] < target) l = m + 1; else r = m - 1;
            }
        }
        return false;
    }
}
```
复杂度：时间 `O(m log n)`。
#### 资深解法：整体当一维数组二分
算法思想：矩阵按行展开后整体有序，可以把下标 `mid` 映射回 `(mid / n, mid % n)`。
```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int m = matrix.length, n = matrix[0].length;
        int l = 0, r = m * n - 1;
        while (l <= r) {
            int mid = l + (r - l) / 2;
            int val = matrix[mid / n][mid % n];
            if (val == target) return true;
            if (val < target) l = mid + 1; else r = mid - 1;
        }
        return false;
    }
}
```
复杂度：时间 `O(log(mn))`，空间 `O(1)`。
#### 基础语法与算法思想
- 这题的二维矩阵有“行尾小于下一行行首”的更强单调性。
- 一维映射是矩阵二分常见技巧。
- 核心思想：把二维有序结构压平成一维有序数组。
---

## 153. 寻找旋转排序数组中的最小值 (Medium)
原题位置：`LeetCode_Collection_Part_6.md:76`
### Java 解法补充
#### 基础解法：线性扫描找最小值
算法思想：数组整体升序但发生了一次旋转，最小值一定存在，直接遍历一遍取最小即可。
```java
class Solution {
    public int findMin(int[] nums) {
        int ans = nums[0];
        for (int num : nums) {
            ans = Math.min(ans, num);
        }
        return ans;
    }
}
```
复杂度：时间 `O(n)`，空间 `O(1)`。
#### 资深解法：二分查找旋转点
算法思想：比较 `mid` 和 `right`。若 `nums[mid] < nums[right]`，说明最小值在左半段含 `mid`；否则在右半段不含 `mid`。
```java
class Solution {
    public int findMin(int[] nums) {
        int left = 0, right = nums.length - 1;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] < nums[right]) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return nums[left];
    }
}
```
复杂度：时间 `O(log n)`，空间 `O(1)`。
#### 基础语法与算法思想
- `left < right` 的二分写法适合找极值位置。
- 旋转有序数组的二分关键是判断哪一半保持有序。
- 核心思想：最小值就是有序断点。
---

## 155. 最小栈 (Medium)
原题位置：`LeetCode_Collection_Part_6.md:160`
### Java 解法补充
#### 基础解法：普通栈查询最小值时遍历
算法思想：入栈和出栈都正常做，查询最小值时把栈元素遍历一遍找最小。
```java
import java.util.ArrayDeque;
import java.util.Deque;

class MinStack {
    private final Deque<Integer> stack = new ArrayDeque<>();

    public void push(int val) {
        stack.push(val);
    }

    public void pop() {
        stack.pop();
    }

    public int top() {
        return stack.peek();
    }

    public int getMin() {
        int ans = Integer.MAX_VALUE;
        for (int x : stack) ans = Math.min(ans, x);
        return ans;
    }
}
```
复杂度：`push/pop/top` 为 `O(1)`，`getMin` 为 `O(n)`。
#### 资深解法：辅助栈同步维护最小值
算法思想：数据栈正常存值，最小栈栈顶始终保存当前最小值。每次入栈时同步压入当前阶段最小值，出栈时同步弹出。
```java
import java.util.ArrayDeque;
import java.util.Deque;

class MinStack {
    private final Deque<Integer> stack = new ArrayDeque<>();
    private final Deque<Integer> minStack = new ArrayDeque<>();

    public void push(int val) {
        stack.push(val);
        if (minStack.isEmpty()) {
            minStack.push(val);
        } else {
            minStack.push(Math.min(val, minStack.peek()));
        }
    }

    public void pop() {
        stack.pop();
        minStack.pop();
    }

    public int top() {
        return stack.peek();
    }

    public int getMin() {
        return minStack.peek();
    }
}
```
复杂度：所有操作都是 `O(1)`，空间 `O(n)`。
#### 基础语法与算法思想
- `Deque` 比 `Stack` 更推荐。
- 辅助栈常用于把“区间最值”变成常数时间查询。
- 核心思想：把历史最小值随操作同步保存。
---

## 394. 字符串解码 (Medium)
原题位置：`LeetCode_Collection_Part_14.md:126`
### Java 解法补充
#### 基础解法：递归解析括号结构
算法思想：遇到数字时先解析重复次数，再递归解析对应的中括号内容，最后把子串重复拼接。
```java
class Solution {
    private int index = 0;

    public String decodeString(String s) {
        return parse(s);
    }

    private String parse(String s) {
        StringBuilder sb = new StringBuilder();
        while (index < s.length() && s.charAt(index) != ']') {
            char c = s.charAt(index);
            if (Character.isLetter(c)) {
                sb.append(c);
                index++;
            } else {
                int num = 0;
                while (Character.isDigit(s.charAt(index))) {
                    num = num * 10 + (s.charAt(index) - '0');
                    index++;
                }
                index++;
                String part = parse(s);
                index++;
                for (int i = 0; i < num; i++) sb.append(part);
            }
        }
        return sb.toString();
    }
}
```
复杂度：时间 `O(n + ansLen)`，空间 `O(n)`。
#### 资深解法：双栈迭代解析
算法思想：一个栈保存重复次数，一个栈保存进入括号前的字符串。遇到 `]` 时把当前子串重复后接回上一层。
```java
import java.util.ArrayDeque;
import java.util.Deque;

class Solution {
    public String decodeString(String s) {
        Deque<Integer> countStack = new ArrayDeque<>();
        Deque<StringBuilder> strStack = new ArrayDeque<>();
        StringBuilder cur = new StringBuilder();
        int num = 0;

        for (char c : s.toCharArray()) {
            if (Character.isDigit(c)) {
                num = num * 10 + (c - '0');
            } else if (c == '[') {
                countStack.push(num);
                strStack.push(cur);
                num = 0;
                cur = new StringBuilder();
            } else if (c == ']') {
                int repeat = countStack.pop();
                StringBuilder prev = strStack.pop();
                for (int i = 0; i < repeat; i++) prev.append(cur);
                cur = prev;
            } else {
                cur.append(c);
            }
        }
        return cur.toString();
    }
}
```
复杂度：时间 `O(n + ansLen)`，空间 `O(n)`。
#### 基础语法与算法思想
- `StringBuilder` 适合高频拼接字符串。
- 栈特别适合处理括号嵌套结构。
- 核心思想：每层括号都对应一个局部子问题。
---

## 739. 每日温度 (Medium)
原题位置：`LeetCode_Collection_Part_25.md:613`
### Java 解法补充
#### 基础解法：枚举每一天向后找
算法思想：对每一天，向右找到第一个更高温度的位置，距离就是答案。
```java
class Solution {
    public int[] dailyTemperatures(int[] temperatures) {
        int n = temperatures.length;
        int[] ans = new int[n];
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                if (temperatures[j] > temperatures[i]) {
                    ans[i] = j - i;
                    break;
                }
            }
        }
        return ans;
    }
}
```
复杂度：时间 `O(n^2)`，空间 `O(1)`。
#### 资深解法：单调栈存下标
算法思想：栈里保存还没找到更高温度的下标，并保持温度单调递减。当前温度更高时，就可以给栈顶那些天结算答案。
```java
import java.util.ArrayDeque;
import java.util.Deque;

class Solution {
    public int[] dailyTemperatures(int[] temperatures) {
        int n = temperatures.length;
        int[] ans = new int[n];
        Deque<Integer> stack = new ArrayDeque<>();
        for (int i = 0; i < n; i++) {
            while (!stack.isEmpty() && temperatures[i] > temperatures[stack.peek()]) {
                int idx = stack.pop();
                ans[idx] = i - idx;
            }
            stack.push(i);
        }
        return ans;
    }
}
```
复杂度：时间 `O(n)`，空间 `O(n)`。
#### 基础语法与算法思想
- 栈里存下标比存值更方便算距离。
- “下一个更大元素”是单调栈经典模型。
- 核心思想：未解决的位置先挂起，等更优信息出现再统一处理。
---

## 84. 柱状图中最大的矩形 (Hard)
原题位置：`LeetCode_Collection_Part3.md:872`
### Java 解法补充
#### 基础解法：枚举每个区间求最小高度
算法思想：固定左右边界，区间内最小高度决定该区间矩形面积。
```java
class Solution {
    public int largestRectangleArea(int[] heights) {
        int ans = 0;
        for (int i = 0; i < heights.length; i++) {
            int minHeight = Integer.MAX_VALUE;
            for (int j = i; j < heights.length; j++) {
                minHeight = Math.min(minHeight, heights[j]);
                ans = Math.max(ans, minHeight * (j - i + 1));
            }
        }
        return ans;
    }
}
```
复杂度：时间 `O(n^2)`，空间 `O(1)`。
#### 资深解法：单调栈找左右边界
算法思想：对每根柱子，找到左边第一个更矮柱子和右边第一个更矮柱子，它作为最低柱时的最大宽度就确定了。
```java
import java.util.ArrayDeque;
import java.util.Deque;

class Solution {
    public int largestRectangleArea(int[] heights) {
        int n = heights.length;
        int ans = 0;
        Deque<Integer> stack = new ArrayDeque<>();
        for (int i = 0; i <= n; i++) {
            int cur = (i == n) ? 0 : heights[i];
            while (!stack.isEmpty() && cur < heights[stack.peek()]) {
                int h = heights[stack.pop()];
                int left = stack.isEmpty() ? -1 : stack.peek();
                int width = i - left - 1;
                ans = Math.max(ans, h * width);
            }
            stack.push(i);
        }
        return ans;
    }
}
```
复杂度：时间 `O(n)`，空间 `O(n)`。
#### 基础语法与算法思想
- 末尾补一个高度 `0` 是常见清栈技巧。
- 单调栈能快速找到“最近更小元素”。
- 核心思想：把“枚举区间”转成“枚举最低柱”。
---

## 215. 数组中的第K个最大元素 (Medium)
原题位置：`LeetCode_Collection_Part_8.md:139`
### Java 解法补充
#### 基础解法：排序后取下标
算法思想：把数组升序排序，倒数第 `k` 个就是答案。
```java
import java.util.Arrays;

class Solution {
    public int findKthLargest(int[] nums, int k) {
        Arrays.sort(nums);
        return nums[nums.length - k];
    }
}
```
复杂度：时间 `O(n log n)`，空间取决于排序实现。
#### 资深解法：快速选择
算法思想：和快速排序分区类似，每次把一个基准放到最终位置。如果该位置正好是目标下标，直接返回。
```java
import java.util.Random;

class Solution {
    private final Random random = new Random();

    public int findKthLargest(int[] nums, int k) {
        int target = nums.length - k;
        int left = 0, right = nums.length - 1;
        while (true) {
            int pivot = partition(nums, left, right);
            if (pivot == target) return nums[pivot];
            if (pivot < target) left = pivot + 1;
            else right = pivot - 1;
        }
    }

    private int partition(int[] nums, int left, int right) {
        int idx = left + random.nextInt(right - left + 1);
        swap(nums, idx, right);
        int pivot = nums[right];
        int p = left;
        for (int i = left; i < right; i++) {
            if (nums[i] <= pivot) swap(nums, p++, i);
        }
        swap(nums, p, right);
        return p;
    }

    private void swap(int[] nums, int i, int j) {
        int tmp = nums[i];
        nums[i] = nums[j];
        nums[j] = tmp;
    }
}
```
复杂度：平均时间 `O(n)`，最坏 `O(n^2)`，空间 `O(1)`。
#### 基础语法与算法思想
- 第 `k` 大对应升序后的下标 `n-k`。
- 快速选择是分治思想在线性选择问题中的应用。
- 核心思想：不需要把整个数组完全排好序。
---

## 347. 前 K 个高频元素 (Medium)
原题位置：`LeetCode_Collection_Part_12.md:459`
### Java 解法补充
#### 基础解法：统计频次后排序
算法思想：先统计每个数出现次数，再把键按频次从高到低排序，取前 `k` 个。
```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> count = new HashMap<>();
        for (int num : nums) count.put(num, count.getOrDefault(num, 0) + 1);
        List<Integer> keys = new ArrayList<>(count.keySet());
        keys.sort((a, b) -> count.get(b) - count.get(a));
        int[] ans = new int[k];
        for (int i = 0; i < k; i++) ans[i] = keys.get(i);
        return ans;
    }
}
```
复杂度：时间 `O(n log n)`，空间 `O(n)`。
#### 资深解法：最小堆维护前 k 个
算法思想：哈希表统计频次后，用一个按频次升序的最小堆维护前 `k` 个元素，堆满后只保留更高频的元素。
```java
import java.util.HashMap;
import java.util.Map;
import java.util.PriorityQueue;

class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> count = new HashMap<>();
        for (int num : nums) count.put(num, count.getOrDefault(num, 0) + 1);

        PriorityQueue<int[]> heap = new PriorityQueue<>((a, b) -> a[1] - b[1]);
        for (Map.Entry<Integer, Integer> e : count.entrySet()) {
            heap.offer(new int[]{e.getKey(), e.getValue()});
            if (heap.size() > k) heap.poll();
        }

        int[] ans = new int[k];
        for (int i = k - 1; i >= 0; i--) ans[i] = heap.poll()[0];
        return ans;
    }
}
```
复杂度：时间 `O(n log k)`，空间 `O(n)`。
#### 基础语法与算法思想
- `Map<Integer, Integer>` 常用于频次统计。
- `PriorityQueue` 默认是最小堆。
- 核心思想：前 k 问题优先考虑堆。
---

## 295. 数据流的中位数 (Hard)
原题位置：`LeetCode_Collection_Part_10.md:542`
### Java 解法补充
#### 基础解法：插入后排序
算法思想：每次加入一个数就放进列表并重新排序，中位数按列表长度奇偶直接读取。
```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

class MedianFinder {
    private final List<Integer> list = new ArrayList<>();

    public void addNum(int num) {
        list.add(num);
        Collections.sort(list);
    }

    public double findMedian() {
        int n = list.size();
        if ((n & 1) == 1) return list.get(n / 2);
        return (list.get(n / 2 - 1) + list.get(n / 2)) / 2.0;
    }
}
```
复杂度：`addNum` 为 `O(n log n)`。
#### 资深解法：大根堆 + 小根堆
算法思想：左边大根堆保存较小一半，右边小根堆保存较大一半，并保持两边数量平衡。中位数就看堆顶。
```java
import java.util.Collections;
import java.util.PriorityQueue;

class MedianFinder {
    private final PriorityQueue<Integer> left = new PriorityQueue<>(Collections.reverseOrder());
    private final PriorityQueue<Integer> right = new PriorityQueue<>();

    public void addNum(int num) {
        if (left.isEmpty() || num <= left.peek()) left.offer(num);
        else right.offer(num);

        if (left.size() > right.size() + 1) right.offer(left.poll());
        if (right.size() > left.size()) left.offer(right.poll());
    }

    public double findMedian() {
        if (left.size() > right.size()) return left.peek();
        return (left.peek() + right.peek()) / 2.0;
    }
}
```
复杂度：`addNum` 为 `O(log n)`，`findMedian` 为 `O(1)`。
#### 基础语法与算法思想
- `Collections.reverseOrder()` 可构造大根堆。
- 数据流问题强调动态维护，不是一次性处理。
- 核心思想：用两个堆维护“左半边”和“右半边”。
---

## 121. 买卖股票的最佳时机 (Easy)
原题位置：`LeetCode_Collection_Part_5.md:3`
### Java 解法补充
#### 基础解法：枚举买卖日
算法思想：枚举买入日和之后的卖出日，计算所有利润取最大。
```java
class Solution {
    public int maxProfit(int[] prices) {
        int ans = 0;
        for (int i = 0; i < prices.length; i++) {
            for (int j = i + 1; j < prices.length; j++) {
                ans = Math.max(ans, prices[j] - prices[i]);
            }
        }
        return ans;
    }
}
```
复杂度：时间 `O(n^2)`，空间 `O(1)`。
#### 资深解法：维护历史最低价
算法思想：扫描到当天价格时，先尝试作为卖出价更新利润，再更新历史最低买入价。
```java
class Solution {
    public int maxProfit(int[] prices) {
        int minPrice = Integer.MAX_VALUE;
        int ans = 0;
        for (int price : prices) {
            minPrice = Math.min(minPrice, price);
            ans = Math.max(ans, price - minPrice);
        }
        return ans;
    }
}
```
复杂度：时间 `O(n)`，空间 `O(1)`。
#### 基础语法与算法思想
- 遍历中维护最优历史状态是典型 DP/贪心写法。
- 这题只允许一次交易。
- 核心思想：卖出时只需要知道此前最低买价。
---

## 763. 划分字母区间 (Medium)
原题位置：`LeetCode_Collection_Part_26.md:330`
### Java 解法补充
#### 基础解法：枚举区间并校验
算法思想：从左到右尝试切分，若某段中所有字符最后一次出现都不超过当前段右端，就可以切开。
```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<Integer> partitionLabels(String s) {
        List<Integer> ans = new ArrayList<>();
        int start = 0;
        while (start < s.length()) {
            int end = start;
            boolean changed = true;
            while (changed) {
                changed = false;
                for (int i = start; i <= end; i++) {
                    int last = s.lastIndexOf(s.charAt(i));
                    if (last > end) {
                        end = last;
                        changed = true;
                    }
                }
                if (end == start) end = s.lastIndexOf(s.charAt(start));
            }
            ans.add(end - start + 1);
            start = end + 1;
        }
        return ans;
    }
}
```
复杂度：时间较高，最坏 `O(n^2)`。
#### 资深解法：预处理最后出现位置后贪心划分
算法思想：先记录每个字符最后一次出现的位置。扫描时持续更新当前段必须覆盖到的最远位置，走到该位置时就可以切一段。
```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<Integer> partitionLabels(String s) {
        int[] last = new int[26];
        for (int i = 0; i < s.length(); i++) last[s.charAt(i) - 'a'] = i;

        List<Integer> ans = new ArrayList<>();
        int start = 0, end = 0;
        for (int i = 0; i < s.length(); i++) {
            end = Math.max(end, last[s.charAt(i) - 'a']);
            if (i == end) {
                ans.add(end - start + 1);
                start = i + 1;
            }
        }
        return ans;
    }
}
```
复杂度：时间 `O(n)`，空间 `O(1)`。
#### 基础语法与算法思想
- 贪心切分的关键是知道每个字符最晚会出现到哪。
- `last[c]` 这种预处理很常见。
- 核心思想：一段一旦开始，必须覆盖段内所有字符的最后位置。
---

## 70. 爬楼梯 (Easy)
原题位置：`LeetCode_Collection_Part3.md:347`
### Java 解法补充
#### 基础解法：递归枚举最后一步
算法思想：到第 `n` 阶最后一步要么从 `n-1` 走 1 步上来，要么从 `n-2` 走 2 步上来。
```java
class Solution {
    public int climbStairs(int n) {
        if (n <= 2) return n;
        return climbStairs(n - 1) + climbStairs(n - 2);
    }
}
```
复杂度：时间 `O(2^n)`，空间 `O(n)`。
#### 资深解法：滚动数组 DP
算法思想：状态转移是 `dp[i] = dp[i - 1] + dp[i - 2]`，只保留前两项即可。
```java
class Solution {
    public int climbStairs(int n) {
        if (n <= 2) return n;
        int a = 1, b = 2;
        for (int i = 3; i <= n; i++) {
            int c = a + b;
            a = b;
            b = c;
        }
        return b;
    }
}
```
复杂度：时间 `O(n)`，空间 `O(1)`。
#### 基础语法与算法思想
- 这是最典型的一维 DP 入门题。
- 滚动变量可以替代完整数组。
- 核心思想：当前状态只依赖前两个状态。
---

## 118. 杨辉三角 (Easy)
原题位置：`LeetCode_Collection_Part_4.md:584`
### Java 解法补充
#### 基础解法：按定义逐行构造
算法思想：每行两端都是 `1`，中间元素等于上一行相邻两个数之和。
```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<List<Integer>> generate(int numRows) {
        List<List<Integer>> ans = new ArrayList<>();
        for (int i = 0; i < numRows; i++) {
            List<Integer> row = new ArrayList<>();
            for (int j = 0; j <= i; j++) {
                if (j == 0 || j == i) row.add(1);
                else row.add(ans.get(i - 1).get(j - 1) + ans.get(i - 1).get(j));
            }
            ans.add(row);
        }
        return ans;
    }
}
```
复杂度：时间 `O(numRows^2)`，空间 `O(numRows^2)`。
#### 资深解法：原地更新单行
算法思想：只维护当前行，从右往左更新，避免覆盖还没用到的旧值。
```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<List<Integer>> generate(int numRows) {
        List<List<Integer>> ans = new ArrayList<>();
        List<Integer> row = new ArrayList<>();
        for (int i = 0; i < numRows; i++) {
            row.add(1);
            for (int j = i - 1; j > 0; j--) {
                row.set(j, row.get(j) + row.get(j - 1));
            }
            ans.add(new ArrayList<>(row));
        }
        return ans;
    }
}
```
复杂度：时间 `O(numRows^2)`，空间 `O(numRows)`，不计答案。
#### 基础语法与算法思想
- `row.set(j, ...)` 用于更新 `List` 现有位置。
- 从右往左更新是原地 DP 常见技巧。
- 核心思想：下一行由上一行局部关系推出。
---

## 198. 打家劫舍 (Medium)
原题位置：`LeetCode_Collection_Part_7.md:732`
### Java 解法补充
#### 基础解法：递归枚举偷或不偷
算法思想：到第 `i` 家时，要么偷它然后跳到 `i-2`，要么不偷它转到 `i-1`，取两者最大值。
```java
class Solution {
    public int rob(int[] nums) {
        return dfs(nums, nums.length - 1);
    }

    private int dfs(int[] nums, int i) {
        if (i < 0) return 0;
        return Math.max(dfs(nums, i - 1), dfs(nums, i - 2) + nums[i]);
    }
}
```
复杂度：时间 `O(2^n)`，空间 `O(n)`。
#### 资深解法：线性 DP
算法思想：`dp[i]` 表示前 `i` 家的最大收益，转移为 `max(dp[i-1], dp[i-2] + nums[i])`。
```java
class Solution {
    public int rob(int[] nums) {
        int prev2 = 0, prev1 = 0;
        for (int num : nums) {
            int cur = Math.max(prev1, prev2 + num);
            prev2 = prev1;
            prev1 = cur;
        }
        return prev1;
    }
}
```
复杂度：时间 `O(n)`，空间 `O(1)`。
#### 基础语法与算法思想
- “选当前”通常会限制前一个状态能否选。
- 这是经典的线性打家劫舍模型。
- 核心思想：相邻冲突约束常转成 DP。
---

## 279. 完全平方数 (Medium)
原题位置：`LeetCode_Collection_Part_10.md:162`
### Java 解法补充
#### 基础解法：完全背包 DP
算法思想：把每个平方数当作可重复使用的物品，`dp[i]` 表示凑出 `i` 的最少数量。
```java
import java.util.Arrays;

class Solution {
    public int numSquares(int n) {
        int[] dp = new int[n + 1];
        Arrays.fill(dp, Integer.MAX_VALUE / 2);
        dp[0] = 0;
        for (int i = 1; i * i <= n; i++) {
            int sq = i * i;
            for (int j = sq; j <= n; j++) {
                dp[j] = Math.min(dp[j], dp[j - sq] + 1);
            }
        }
        return dp[n];
    }
}
```
复杂度：时间 `O(n * sqrt(n))`，空间 `O(n)`。
#### 资深解法：按层 BFS
算法思想：每层表示使用若干个平方数能到达的和。第一次到达 `0` 或 `n` 的目标层数，就是最少个数。
```java
import java.util.ArrayDeque;
import java.util.HashSet;
import java.util.Queue;
import java.util.Set;

class Solution {
    public int numSquares(int n) {
        Queue<Integer> queue = new ArrayDeque<>();
        Set<Integer> seen = new HashSet<>();
        queue.offer(n);
        seen.add(n);
        int level = 0;
        while (!queue.isEmpty()) {
            level++;
            for (int size = queue.size(); size > 0; size--) {
                int cur = queue.poll();
                for (int i = 1; i * i <= cur; i++) {
                    int next = cur - i * i;
                    if (next == 0) return level;
                    if (seen.add(next)) queue.offer(next);
                }
            }
        }
        return level;
    }
}
```
复杂度：时间和状态数相关，常见写法可过，空间 `O(n)`。
#### 基础语法与算法思想
- 这题既能看成 DP，也能看成最短路分层搜索。
- `Integer.MAX_VALUE / 2` 用来避免加一时溢出。
- 核心思想：最少个数问题常对应最短步数。
---

## 322. 零钱兑换 (Medium)
原题位置：`LeetCode_Collection_Part_11.md:554`
### Java 解法补充
#### 基础解法：递归尝试每种硬币
算法思想：对当前金额，尝试减去每种硬币后递归求解，取最少张数。
```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int ans = dfs(coins, amount);
        return ans >= 1_000_000 ? -1 : ans;
    }

    private int dfs(int[] coins, int amount) {
        if (amount == 0) return 0;
        if (amount < 0) return 1_000_000;
        int ans = 1_000_000;
        for (int coin : coins) {
            ans = Math.min(ans, dfs(coins, amount - coin) + 1);
        }
        return ans;
    }
}
```
复杂度：时间指数级。
#### 资深解法：完全背包 DP
算法思想：`dp[i]` 表示凑出金额 `i` 的最少硬币数，转移是 `dp[i] = min(dp[i], dp[i-coin] + 1)`。
```java
import java.util.Arrays;

class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount + 1];
        Arrays.fill(dp, amount + 1);
        dp[0] = 0;
        for (int i = 1; i <= amount; i++) {
            for (int coin : coins) {
                if (i >= coin) {
                    dp[i] = Math.min(dp[i], dp[i - coin] + 1);
                }
            }
        }
        return dp[amount] > amount ? -1 : dp[amount];
    }
}
```
复杂度：时间 `O(amount * coins.length)`，空间 `O(amount)`。
#### 基础语法与算法思想
- 完全背包允许同一物品重复使用。
- `amount + 1` 常作为“不可能状态”的初始大值。
- 核心思想：最优子结构明确时优先写 DP。
---

## 139. 单词拆分 (Medium)
原题位置：`LeetCode_Collection_Part_5.md:688`
### Java 解法补充
#### 基础解法：递归尝试每个切分点
算法思想：从位置 `start` 出发，尝试所有后缀前缀，如果前缀在词典里，就继续递归处理剩余部分。
```java
import java.util.HashSet;
import java.util.List;
import java.util.Set;

class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        return dfs(s, 0, new HashSet<>(wordDict));
    }

    private boolean dfs(String s, int start, Set<String> dict) {
        if (start == s.length()) return true;
        for (int end = start + 1; end <= s.length(); end++) {
            if (dict.contains(s.substring(start, end)) && dfs(s, end, dict)) {
                return true;
            }
        }
        return false;
    }
}
```
复杂度：时间最坏指数级。
#### 资深解法：DP 判断前缀可达性
算法思想：`dp[i]` 表示前 `i` 个字符能否拆分。若某个 `j < i` 满足 `dp[j]` 为真且 `s[j,i)` 在词典中，则 `dp[i] = true`。
```java
import java.util.HashSet;
import java.util.List;
import java.util.Set;

class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        Set<String> dict = new HashSet<>(wordDict);
        boolean[] dp = new boolean[s.length() + 1];
        dp[0] = true;
        for (int i = 1; i <= s.length(); i++) {
            for (int j = 0; j < i; j++) {
                if (dp[j] && dict.contains(s.substring(j, i))) {
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp[s.length()];
    }
}
```
复杂度：时间 `O(n^2)`，空间 `O(n)`。
#### 基础语法与算法思想
- `HashSet` 能加速单词存在性判断。
- 字符串切分问题常用“前缀能否到达”的 DP。
- 核心思想：原问题是否成立，取决于某个合法前缀后的子问题。
---

## 300. 最长递增子序列 (Medium)
原题位置：`LeetCode_Collection_Part_10.md:685`
### Java 解法补充
#### 基础解法：朴素 DP
算法思想：`dp[i]` 表示以 `nums[i]` 结尾的 LIS 长度，枚举前面所有更小元素转移。
```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int n = nums.length;
        int[] dp = new int[n];
        int ans = 1;
        for (int i = 0; i < n; i++) {
            dp[i] = 1;
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
复杂度：时间 `O(n^2)`，空间 `O(n)`。
#### 资深解法：贪心 + 二分维护结尾最小值
算法思想：`tails[len]` 表示长度为 `len+1` 的递增子序列最小结尾。新数用二分找到替换位置。
```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int[] tails = new int[nums.length];
        int size = 0;
        for (int num : nums) {
            int left = 0, right = size;
            while (left < right) {
                int mid = left + (right - left) / 2;
                if (tails[mid] < num) left = mid + 1;
                else right = mid;
            }
            tails[left] = num;
            if (left == size) size++;
        }
        return size;
    }
}
```
复杂度：时间 `O(n log n)`，空间 `O(n)`。
#### 基础语法与算法思想
- LIS 不要求连续，所以和最长连续递增子数组不同。
- `tails` 不是实际子序列，只是用于维护最优结尾。
- 核心思想：更小的结尾更有利于后续延长。
---

## 152. 乘积最大子数组 (Medium)
原题位置：`LeetCode_Collection_Part_6.md:45`
### Java 解法补充
#### 基础解法：枚举所有子数组乘积
算法思想：固定左端点，向右不断累乘并更新最大值。
```java
class Solution {
    public int maxProduct(int[] nums) {
        int ans = Integer.MIN_VALUE;
        for (int i = 0; i < nums.length; i++) {
            int product = 1;
            for (int j = i; j < nums.length; j++) {
                product *= nums[j];
                ans = Math.max(ans, product);
            }
        }
        return ans;
    }
}
```
复杂度：时间 `O(n^2)`，空间 `O(1)`。
#### 资深解法：同时维护最大乘积和最小乘积
算法思想：负数会让最大最小互换，所以每一步都要同时维护“以当前位置结尾的最大积”和“最小积”。
```java
class Solution {
    public int maxProduct(int[] nums) {
        int maxProd = nums[0], minProd = nums[0], ans = nums[0];
        for (int i = 1; i < nums.length; i++) {
            int x = nums[i];
            if (x < 0) {
                int temp = maxProd;
                maxProd = minProd;
                minProd = temp;
            }
            maxProd = Math.max(x, maxProd * x);
            minProd = Math.min(x, minProd * x);
            ans = Math.max(ans, maxProd);
        }
        return ans;
    }
}
```
复杂度：时间 `O(n)`，空间 `O(1)`。
#### 基础语法与算法思想
- 乘积题和和题不同，负数会改变大小关系。
- 需要同时保留最坏状态，因它可能在下一步转成最好状态。
- 核心思想：状态转移要考虑符号翻转。
---

## 416. 分割等和子集 (Medium)
原题位置：`LeetCode_Collection_Part_14.md:848`
### Java 解法补充
#### 基础解法：回溯尝试选取子集
算法思想：先求总和，若为奇数直接返回 false；否则尝试挑选部分数使和为 `sum/2`。
```java
class Solution {
    public boolean canPartition(int[] nums) {
        int sum = 0;
        for (int num : nums) sum += num;
        if ((sum & 1) == 1) return false;
        return dfs(nums, 0, sum / 2);
    }

    private boolean dfs(int[] nums, int index, int target) {
        if (target == 0) return true;
        if (index == nums.length || target < 0) return false;
        return dfs(nums, index + 1, target - nums[index]) || dfs(nums, index + 1, target);
    }
}
```
复杂度：时间最坏指数级。
#### 资深解法：0/1 背包
算法思想：问题转化为能否选出若干数恰好装满容量 `sum/2` 的背包。
```java
class Solution {
    public boolean canPartition(int[] nums) {
        int sum = 0;
        for (int num : nums) sum += num;
        if ((sum & 1) == 1) return false;
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
- 0/1 背包滚动数组要从大到小遍历容量。
- 先判总和奇偶，能快速剪枝。
- 核心思想：等和划分本质是子集和问题。
---

## 62. 不同路径 (Medium)
原题位置：`LeetCode_Collection_Part3.md:28`
### Java 解法补充
#### 基础解法：二维 DP
算法思想：到达某格的方法数等于上方和左方方法数之和，第一行第一列都只有一种走法。
```java
class Solution {
    public int uniquePaths(int m, int n) {
        int[][] dp = new int[m][n];
        for (int i = 0; i < m; i++) dp[i][0] = 1;
        for (int j = 0; j < n; j++) dp[0][j] = 1;
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
            }
        }
        return dp[m - 1][n - 1];
    }
}
```
复杂度：时间 `O(mn)`，空间 `O(mn)`。
#### 资深解法：一维滚动 DP
算法思想：当前行只依赖上一行和当前行左边，因此可压缩到一维数组。
```java
class Solution {
    public int uniquePaths(int m, int n) {
        int[] dp = new int[n];
        for (int j = 0; j < n; j++) dp[j] = 1;
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                dp[j] += dp[j - 1];
            }
        }
        return dp[n - 1];
    }
}
```
复杂度：时间 `O(mn)`，空间 `O(n)`。
#### 基础语法与算法思想
- 网格路径题通常从小格推大格。
- 第一行和第一列常是边界初始化。
- 核心思想：当前位置只依赖能走到它的前驱位置。
---

## 64. 最小路径和 (Medium)
原题位置：`LeetCode_Collection_Part3.md:110`
### Java 解法补充
#### 基础解法：二维 DP
算法思想：到达某格的最小路径和等于该格值加上“上方最小路径和”和“左方最小路径和”的较小者。
```java
class Solution {
    public int minPathSum(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int[][] dp = new int[m][n];
        dp[0][0] = grid[0][0];
        for (int i = 1; i < m; i++) dp[i][0] = dp[i - 1][0] + grid[i][0];
        for (int j = 1; j < n; j++) dp[0][j] = dp[0][j - 1] + grid[0][j];
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                dp[i][j] = Math.min(dp[i - 1][j], dp[i][j - 1]) + grid[i][j];
            }
        }
        return dp[m - 1][n - 1];
    }
}
```
复杂度：时间 `O(mn)`，空间 `O(mn)`。
#### 资深解法：原地修改网格
算法思想：直接把 `grid[i][j]` 改成到这里的最小路径和，省掉额外 DP 数组。
```java
class Solution {
    public int minPathSum(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        for (int i = 1; i < m; i++) grid[i][0] += grid[i - 1][0];
        for (int j = 1; j < n; j++) grid[0][j] += grid[0][j - 1];
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                grid[i][j] += Math.min(grid[i - 1][j], grid[i][j - 1]);
            }
        }
        return grid[m - 1][n - 1];
    }
}
```
复杂度：时间 `O(mn)`，空间 `O(1)`。
#### 基础语法与算法思想
- 最短路径和属于权值累加型网格 DP。
- 原地 DP 适合允许修改输入的场景。
- 核心思想：局部最优可由前驱最优推得。
---

## 1143. 最长公共子序列 (Medium)
原题位置：`LeetCode_Collection_Part_39.md:66`
### Java 解法补充
#### 基础解法：递归比较最后一个字符
算法思想：若两个串最后字符相等，答案等于去掉最后字符后的答案加一；否则在“删掉第一个串最后字符”和“删掉第二个串最后字符”中取较大。
```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        return dfs(text1, text2, text1.length(), text2.length());
    }

    private int dfs(String a, String b, int i, int j) {
        if (i == 0 || j == 0) return 0;
        if (a.charAt(i - 1) == b.charAt(j - 1)) {
            return dfs(a, b, i - 1, j - 1) + 1;
        }
        return Math.max(dfs(a, b, i - 1, j), dfs(a, b, i, j - 1));
    }
}
```
复杂度：时间指数级。
#### 资深解法：二维 DP
算法思想：`dp[i][j]` 表示 `text1` 前 `i` 个字符和 `text2` 前 `j` 个字符的 LCS 长度。
```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int m = text1.length(), n = text2.length();
        int[][] dp = new int[m + 1][n + 1];
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (text1.charAt(i - 1) == text2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[m][n];
    }
}
```
复杂度：时间 `O(mn)`，空间 `O(mn)`。
#### 基础语法与算法思想
- 子序列不要求连续，子串要求连续。
- 二维字符串 DP 的常见定义是“前 i 个”和“前 j 个”。
- 核心思想：匹配与不匹配对应不同转移。
---

## 72. 编辑距离 (Medium)
原题位置：`LeetCode_Collection_Part3.md:443`
### Java 解法补充
#### 基础解法：递归模拟插入删除替换
算法思想：若最后字符相同，转成更短前缀问题；否则尝试插入、删除、替换三种操作，取最小步数。
```java
class Solution {
    public int minDistance(String word1, String word2) {
        return dfs(word1, word2, word1.length(), word2.length());
    }

    private int dfs(String a, String b, int i, int j) {
        if (i == 0) return j;
        if (j == 0) return i;
        if (a.charAt(i - 1) == b.charAt(j - 1)) return dfs(a, b, i - 1, j - 1);
        int insert = dfs(a, b, i, j - 1);
        int delete = dfs(a, b, i - 1, j);
        int replace = dfs(a, b, i - 1, j - 1);
        return Math.min(insert, Math.min(delete, replace)) + 1;
    }
}
```
复杂度：时间指数级。
#### 资深解法：二维 DP
算法思想：`dp[i][j]` 表示 `word1` 前 `i` 个字符转成 `word2` 前 `j` 个字符的最小操作数。
```java
class Solution {
    public int minDistance(String word1, String word2) {
        int m = word1.length(), n = word2.length();
        int[][] dp = new int[m + 1][n + 1];
        for (int i = 0; i <= m; i++) dp[i][0] = i;
        for (int j = 0; j <= n; j++) dp[0][j] = j;
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1];
                } else {
                    dp[i][j] = Math.min(dp[i - 1][j - 1],
                            Math.min(dp[i - 1][j], dp[i][j - 1])) + 1;
                }
            }
        }
        return dp[m][n];
    }
}
```
复杂度：时间 `O(mn)`，空间 `O(mn)`。
#### 基础语法与算法思想
- 编辑操作通常有插入、删除、替换三种。
- 边界初始化表示和空串互转的代价。
- 核心思想：大问题拆成更短前缀的最优子问题。
---

## 136. 只出现一次的数字 (Easy)
原题位置：`LeetCode_Collection_Part_5.md:586`
### Java 解法补充
#### 基础解法：哈希表计数
算法思想：统计每个数字出现次数，最后找出出现一次的那个。
```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int singleNumber(int[] nums) {
        Map<Integer, Integer> count = new HashMap<>();
        for (int num : nums) count.put(num, count.getOrDefault(num, 0) + 1);
        for (int num : nums) {
            if (count.get(num) == 1) return num;
        }
        return -1;
    }
}
```
复杂度：时间 `O(n)`，空间 `O(n)`。
#### 资深解法：异或
算法思想：相同数字异或两次会抵消为 `0`，而 `0 ^ x = x`，所以所有数异或后只剩答案。
```java
class Solution {
    public int singleNumber(int[] nums) {
        int ans = 0;
        for (int num : nums) ans ^= num;
        return ans;
    }
}
```
复杂度：时间 `O(n)`，空间 `O(1)`。
#### 基础语法与算法思想
- `^` 是按位异或运算符。
- 位运算常用于“成对抵消”类题目。
- 核心思想：利用二进制运算性质代替计数。
---

## 169. 多数元素 (Easy)
原题位置：`LeetCode_Collection_Part_6.md:711`
### Java 解法补充
#### 基础解法：哈希表计数
算法思想：统计频次，只要某个元素次数大于 `n/2` 就返回它。
```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int majorityElement(int[] nums) {
        Map<Integer, Integer> count = new HashMap<>();
        for (int num : nums) {
            int c = count.getOrDefault(num, 0) + 1;
            if (c > nums.length / 2) return num;
            count.put(num, c);
        }
        return -1;
    }
}
```
复杂度：时间 `O(n)`，空间 `O(n)`。
#### 资深解法：摩尔投票
算法思想：不同元素两两抵消，最后剩下的候选者一定是多数元素。
```java
class Solution {
    public int majorityElement(int[] nums) {
        int candidate = 0, count = 0;
        for (int num : nums) {
            if (count == 0) candidate = num;
            count += (num == candidate) ? 1 : -1;
        }
        return candidate;
    }
}
```
复杂度：时间 `O(n)`，空间 `O(1)`。
#### 基础语法与算法思想
- 摩尔投票依赖题目保证多数元素一定存在。
- 候选值变化时相当于重新开始统计。
- 核心思想：多数元素无法被少数元素完全抵消。
---

## 75. 颜色分类 (Medium)
原题位置：`LeetCode_Collection_Part3.md:554`
### Java 解法补充
#### 基础解法：计数后回写
算法思想：先统计 `0/1/2` 各有多少个，再按数量回填到原数组。
```java
class Solution {
    public void sortColors(int[] nums) {
        int[] count = new int[3];
        for (int num : nums) count[num]++;
        int idx = 0;
        for (int color = 0; color < 3; color++) {
            for (int c = 0; c < count[color]; c++) nums[idx++] = color;
        }
    }
}
```
复杂度：时间 `O(n)`，空间 `O(1)`。
#### 资深解法：荷兰国旗三指针
算法思想：`left` 左边全是 `0`，`right` 右边全是 `2`，`i` 扫描中间区域并把元素放到正确分区。
```java
class Solution {
    public void sortColors(int[] nums) {
        int left = 0, i = 0, right = nums.length - 1;
        while (i <= right) {
            if (nums[i] == 0) {
                swap(nums, left++, i++);
            } else if (nums[i] == 2) {
                swap(nums, i, right--);
            } else {
                i++;
            }
        }
    }

    private void swap(int[] nums, int i, int j) {
        int tmp = nums[i];
        nums[i] = nums[j];
        nums[j] = tmp;
    }
}
```
复杂度：时间 `O(n)`，空间 `O(1)`。
#### 基础语法与算法思想
- 三指针分区是原地分类题的经典解。
- 遇到 `2` 交换后 `i` 不能立刻前进，因为换来的值还没检查。
- 核心思想：维护三个连续区间的不变量。
---

## 287. 寻找重复数 (Medium)
原题位置：`LeetCode_Collection_Part_10.md:335`
### Java 解法补充
#### 基础解法：排序后找相邻相等
算法思想：排序后重复数字一定出现在相邻位置。
```java
import java.util.Arrays;

class Solution {
    public int findDuplicate(int[] nums) {
        Arrays.sort(nums);
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] == nums[i - 1]) return nums[i];
        }
        return -1;
    }
}
```
复杂度：时间 `O(n log n)`，空间取决于排序实现。
#### 资深解法：Floyd 判环
算法思想：把数组看成“下标指向值”的链表结构，因为有重复数，所以一定形成环，环入口就是重复数。
```java
class Solution {
    public int findDuplicate(int[] nums) {
        int slow = nums[0];
        int fast = nums[0];
        do {
            slow = nums[slow];
            fast = nums[nums[fast]];
        } while (slow != fast);

        slow = nums[0];
        while (slow != fast) {
            slow = nums[slow];
            fast = nums[fast];
        }
        return slow;
    }
}
```
复杂度：时间 `O(n)`，空间 `O(1)`。
#### 基础语法与算法思想
- 题目约束“数值范围 1..n、长度 n+1”是能建环的关键。
- 判环思路不只适用于链表，也能用于函数映射。
- 核心思想：重复值意味着多个下标汇入同一节点。
---
