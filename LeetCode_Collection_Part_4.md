# LeetCode 题目合集 Part 4

## 101. 对称二叉树 (Easy)

给你一个二叉树的根节点  `root`  ， 检查它是否轴对称。
 
 **示例 1：** 

```text
输入：root = [1,2,2,3,4,4,3]
输出：true
```

 **示例 2：** 

```text
输入：root = [1,2,2,null,3,null,3]
输出：false
```

 
 **提示：** 

树中节点数目在范围  `[1, 1000]`  内
 `-100 <= Node.val <= 100` 

 
 **进阶：** 你可以运用递归和迭代两种方法解决这个问题吗？

---

## 102. 二叉树的层序遍历 (Medium)

给你二叉树的根节点  `root`  ，返回其节点值的  **层序遍历**  。 （即逐层地，从左到右访问所有节点）。
 
 **示例 1：** 

```text
输入：root = [3,9,20,null,null,15,7]
输出：[[3],[9,20],[15,7]]
```

 **示例 2：** 

```text
输入：root = [1]
输出：[[1]]
```

 **示例 3：** 

```text
输入：root = []
输出：[]
```

 
 **提示：** 

树中节点数目在范围  `[0, 2000]`  内
 `-1000 <= Node.val <= 1000`

---

## 103. 二叉树的锯齿形层序遍历 (Medium)

给你二叉树的根节点  `root`  ，返回其节点值的  **锯齿形层序遍历**  。（即先从左往右，再从右往左进行下一层遍历，以此类推，层与层之间交替进行）。
 
 **示例 1：** 

```text
输入：root = [3,9,20,null,null,15,7]
输出：[[3],[20,9],[15,7]]
```

 **示例 2：** 

```text
输入：root = [1]
输出：[[1]]
```

 **示例 3：** 

```text
输入：root = []
输出：[]
```

 
 **提示：** 

树中节点数目在范围  `[0, 2000]`  内
 `-100 <= Node.val <= 100`

---

## 104. 二叉树的最大深度 (Easy)

给定一个二叉树  `root`  ，返回其最大深度。
二叉树的  **最大深度**  是指从根节点到最远叶子节点的最长路径上的节点数。
 
 **示例 1：** 

 

```text
输入：root = [3,9,20,null,null,15,7]
输出：3
```

 **示例 2：** 

```text
输入：root = [1,null,2]
输出：2
```

 
 **提示：** 

树中节点的数量在  `[0, 104]`  区间内。
 `-100 <= Node.val <= 100`

---

## 105. 从前序与中序遍历序列构造二叉树 (Medium)

给定两个整数数组  `preorder`  和  `inorder`  ，其中  `preorder`  是二叉树的 **先序遍历** ，  `inorder`  是同一棵树的 **中序遍历** ，请构造二叉树并返回其根节点。
 
 **示例 1:** 

```text
输入: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
输出: [3,9,20,null,null,15,7]
```

 **示例 2:** 

```text
输入: preorder = [-1], inorder = [-1]
输出: [-1]
```

 
 **提示:** 

 `1 <= preorder.length <= 3000` 
 `inorder.length == preorder.length` 
 `-3000 <= preorder[i], inorder[i] <= 3000` 
 `preorder`  和  `inorder`  均  **无重复**  元素
 `inorder`  均出现在  `preorder` 
 `preorder`   **保证**  为二叉树的前序遍历序列
 `inorder`   **保证**  为二叉树的中序遍历序列

---

## 106. 从中序与后序遍历序列构造二叉树 (Medium)

给定两个整数数组  `inorder`  和  `postorder`  ，其中  `inorder`  是二叉树的中序遍历，  `postorder`  是同一棵树的后序遍历，请你构造并返回这颗 二叉树 。
 
 **示例 1:** 

```text
输入：inorder = [9,3,15,20,7], postorder = [9,15,7,20,3]
输出：[3,9,20,null,null,15,7]
```

 **示例 2:** 

```text
输入：inorder = [-1], postorder = [-1]
输出：[-1]
```

 
 **提示:** 

 `1 <= inorder.length <= 3000` 
 `postorder.length == inorder.length` 
 `-3000 <= inorder[i], postorder[i] <= 3000` 
 `inorder`  和  `postorder`  都由  **不同**  的值组成
 `postorder`  中每一个值都在  `inorder`  中
 `inorder`   **保证** 是树的中序遍历
 `postorder`   **保证** 是树的后序遍历

---

## 107. 二叉树的层序遍历 II (Medium)

给你二叉树的根节点  `root`  ，返回其节点值  **自底向上的层序遍历**  。 （即按从叶子节点所在层到根节点所在的层，逐层从左向右遍历）
 
 **示例 1：** 

```text
输入：root = [3,9,20,null,null,15,7]
输出：[[15,7],[9,20],[3]]
```

 **示例 2：** 

```text
输入：root = [1]
输出：[[1]]
```

 **示例 3：** 

```text
输入：root = []
输出：[]
```

 
 **提示：** 

树中节点数目在范围  `[0, 2000]`  内
 `-1000 <= Node.val <= 1000`

---

## 108. 将有序数组转换为二叉搜索树 (Easy)

给你一个整数数组  `nums`  ，其中元素已经按  **升序**  排列，请你将其转换为一棵 平衡 二叉搜索树。
 
 **示例 1：** 

```text
输入：nums = [-10,-3,0,5,9]
输出：[0,-3,9,-10,null,5]
解释：[0,-10,5,null,-3,null,9] 也将被视为正确答案：
```

 **示例 2：** 

```text
输入：nums = [1,3]
输出：[3,1]
解释：[1,null,3] 和 [3,1] 都是高度平衡二叉搜索树。
```

 
 **提示：** 

 `1 <= nums.length <= 104` 
 `-104 <= nums[i] <= 104` 
 `nums`  按  **严格递增**  顺序排列

---

## 109. 有序链表转换二叉搜索树 (Medium)

给定一个单链表的头节点   `head`  ，其中的元素  **按升序排序**  ，将其转换为 平衡 二叉搜索树。
 
 **示例 1:** 

```text
输入: head = [-10,-3,0,5,9]
输出: [0,-3,9,-10,null,5]
解释: 一个可能的答案是[0，-3,9，-10,null,5]，它表示所示的高度平衡的二叉搜索树。
```

 **示例 2:** 

```text
输入: head = []
输出: []
```

 
 **提示:** 

 `head`  中的节点数在 `[0, 2 * 104]`  范围内
 `-105 <= Node.val <= 105`

---

## 110. 平衡二叉树 (Easy)

给定一个二叉树，判断它是否是 平衡二叉树  
 
 **示例 1：** 

```text
输入：root = [3,9,20,null,null,15,7]
输出：true
```

 **示例 2：** 

```text
输入：root = [1,2,2,3,3,null,null,4,4]
输出：false
```

 **示例 3：** 

```text
输入：root = []
输出：true
```

 
 **提示：** 

树中的节点数在范围  `[0, 5000]`  内
 `-104 <= Node.val <= 104`

---

## 111. 二叉树的最小深度 (Easy)

给定一个二叉树，找出其最小深度。
最小深度是从根节点到最近叶子节点的最短路径上的节点数量。
 **说明：** 叶子节点是指没有子节点的节点。
 
 **示例 1：** 

```text
输入：root = [3,9,20,null,null,15,7]
输出：2
```

 **示例 2：** 

```text
输入：root = [2,null,3,null,4,null,5,null,6]
输出：5
```

 
 **提示：** 

树中节点数的范围在  `[0, 105]`  内
 `-1000 <= Node.val <= 1000`

---

## 112. 路径总和 (Easy)

给你二叉树的根节点  `root`  和一个表示目标和的整数  `targetSum`  。判断该树中是否存在  **根节点到叶子节点**  的路径，这条路径上所有节点值相加等于目标和  `targetSum`  。如果存在，返回  `true`  ；否则，返回  `false`  。
 **叶子节点**  是指没有子节点的节点。
 
 **示例 1：** 

```text
输入：root = [5,4,8,11,null,13,4,7,2,null,null,null,1], targetSum = 22
输出：true
解释：等于目标和的根节点到叶节点路径如上图所示。
```

 **示例 2：** 

```text
输入：root = [1,2,3], targetSum = 5
输出：false
解释：树中存在两条根节点到叶子节点的路径：
(1 --> 2): 和为 3
(1 --> 3): 和为 4
不存在 sum = 5 的根节点到叶子节点的路径。
```

 **示例 3：** 

```text
输入：root = [], targetSum = 0
输出：false
解释：由于树是空的，所以不存在根节点到叶子节点的路径。
```

 
 **提示：** 

树中节点的数目在范围  `[0, 5000]`  内
 `-1000 <= Node.val <= 1000` 
 `-1000 <= targetSum <= 1000`

---

## 113. 路径总和 II (Medium)

给你二叉树的根节点  `root`  和一个整数目标和  `targetSum`  ，找出所有  **从根节点到叶子节点**  路径总和等于给定目标和的路径。
 **叶子节点**  是指没有子节点的节点。

 
 **示例 1：** 

```text
输入：root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
输出：[[5,4,11,2],[5,8,4,5]]
```

 **示例 2：** 

```text
输入：root = [1,2,3], targetSum = 5
输出：[]
```

 **示例 3：** 

```text
输入：root = [1,2], targetSum = 0
输出：[]
```

 
 **提示：** 

树中节点总数在范围  `[0, 5000]`  内
 `-1000 <= Node.val <= 1000` 
 `-1000 <= targetSum <= 1000`

---

## 114. 二叉树展开为链表 (Medium)

给你二叉树的根结点  `root`  ，请你将它展开为一个单链表：

展开后的单链表应该同样使用  `TreeNode`  ，其中  `right`  子指针指向链表中下一个结点，而左子指针始终为  `null`  。
展开后的单链表应该与二叉树  **先序遍历**  顺序相同。

 
 **示例 1：** 

```text
输入：root = [1,2,5,3,4,null,6]
输出：[1,null,2,null,3,null,4,null,5,null,6]
```

 **示例 2：** 

```text
输入：root = []
输出：[]
```

 **示例 3：** 

```text
输入：root = [0]
输出：[0]
```

 
 **提示：** 

树中结点数在范围  `[0, 2000]`  内
 `-100 <= Node.val <= 100` 

 
 **进阶：** 你可以使用原地算法（ `O(1)`  额外空间）展开这棵树吗？

---

## 115. 不同的子序列 (Hard)

给你两个字符串  `s`  **** 和  `t`  ，统计并返回在  `s`  的  **子序列**  中  `t`  出现的个数。
测试用例保证结果在 32 位有符号整数范围内。
 
 **示例 1：** 

```text
输入：s = "rabbbit", t = "rabbit"
输出：3
解释：
如下所示, 有 3 种可以从 s 中得到 "rabbit" 的方案。
rabbbit
rabbbit
rabbbit
```

 **示例 2：** 

```text
输入：s = "babgbag", t = "bag"
输出：5
解释：
如下所示, 有 5 种可以从 s 中得到 "bag" 的方案。 
babgbag
babgbag
babgbag
babgbag
babgbag
```

 
 **提示：** 

 `1 <= s.length, t.length <= 1000` 
 `s`  和  `t`  由英文字母组成

---

## 116. 填充每个节点的下一个右侧节点指针 (Medium)

给定一个  **完美二叉树** ，其所有叶子节点都在同一层，每个父节点都有两个子节点。二叉树定义如下：

```text
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```

填充它的每个 next 指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将 next 指针设置为  `NULL` 。
初始状态下，所有 next 指针都被设置为  `NULL` 。
 
 **示例 1：** 

```text
输入：root = [1,2,3,4,5,6,7]
输出：[1,#,2,3,#,4,5,6,7,#]
解释：给定二叉树如图 A 所示，你的函数应该填充它的每个 next 指针，以指向其下一个右侧节点，如图 B 所示。序列化的输出按层序遍历排列，同一层节点由 next 指针连接，'#' 标志着每一层的结束。
```

 **示例 2:** 

```text
输入：root = []
输出：[]
```

 
 **提示：** 

树中节点的数量在  `[0, 212 - 1]`  范围内
 `-1000 <= node.val <= 1000` 

 
 **进阶：** 

你只能使用常量级额外空间。
使用递归解题也符合要求，本题中递归程序占用的栈空间不算做额外的空间复杂度。

---

## 117. 填充每个节点的下一个右侧节点指针 II (Medium)

给定一个二叉树：

```text
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```

填充它的每个 next 指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将 next 指针设置为  `NULL`  。
初始状态下，所有 next 指针都被设置为  `NULL`  。
 
 **示例 1：** 

```text
输入：root = [1,2,3,4,5,null,7]
输出：[1,#,2,3,#,4,5,7,#]
解释：给定二叉树如图 A 所示，你的函数应该填充它的每个 next 指针，以指向其下一个右侧节点，如图 B 所示。序列化输出按层序遍历顺序（由 next 指针连接），'#' 表示每层的末尾。
```

 **示例 2：** 

```text
输入：root = []
输出：[]
```

 
 **提示：** 

树中的节点数在范围  `[0, 6000]`  内
 `-100 <= Node.val <= 100` 

 **进阶：** 

你只能使用常量级额外空间。
使用递归解题也符合要求，本题中递归程序的隐式栈空间不计入额外空间复杂度。

---

## 118. 杨辉三角 (Easy)

给定一个非负整数  `numRows` ，生成「杨辉三角」的前  `numRows`  行。
在 **「杨辉三角」** 中，每个数是它左上方和右上方的数的和。

 
 **示例 1:** 

```text
输入: numRows = 5
输出: [[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]]
```

 **示例 2:** 

```text
输入: numRows = 1
输出: [[1]]
```

 
 **提示:** 

 `1 <= numRows <= 30`

---

## 119. 杨辉三角 II (Easy)

给定一个非负索引  `rowIndex` ，返回「杨辉三角」的第  `rowIndex`  行。
在「杨辉三角」中，每个数是它左上方和右上方的数的和。

 
 **示例 1:** 

```text
输入: rowIndex = 3
输出: [1,3,3,1]
```

 **示例 2:** 

```text
输入: rowIndex = 0
输出: [1]
```

 **示例 3:** 

```text
输入: rowIndex = 1
输出: [1,1]
```

 
 **提示:** 

 `0 <= rowIndex <= 33` 

 
 **进阶：** 
你可以优化你的算法到  `O(rowIndex)`  空间复杂度吗？

---

## 120. 三角形最小路径和 (Medium)

给定一个三角形  `triangle`  ，找出自顶向下的最小路径和。
每一步只能移动到下一行中相邻的结点上。 **相邻的结点** 在这里指的是  **下标**  与  **上一层结点下标**  相同或者等于  **上一层结点下标 + 1**  的两个结点。也就是说，如果正位于当前行的下标  `i`  ，那么下一步可以移动到下一行的下标  `i`  或  `i + 1`  。
 
 **示例 1：** 

```text
输入：triangle = [[2],[3,4],[6,5,7],[4,1,8,3]]
输出：11
解释：如下面简图所示：
   2
  3 4
 6 5 7
4 1 8 3
自顶向下的最小路径和为 11（即，2 + 3 + 5 + 1 = 11）。
```

 **示例 2：** 

```text
输入：triangle = [[-10]]
输出：-10
```

 
 **提示：** 

 `1 <= triangle.length <= 200` 
 `triangle[0].length == 1` 
 `triangle[i].length == triangle[i - 1].length + 1` 
 `-104 <= triangle[i][j] <= 104` 

 
 **进阶：** 

你可以只使用  `O(n)`  的额外空间（ `n`  为三角形的总行数）来解决这个问题吗？

---

# Java 解法补充附录（101-120）

### 101. 对称二叉树

基础解法：层序遍历每层并比较镜像位置。
资深解法：递归比较左右子树是否互为镜像。

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        return root == null || mirror(root.left, root.right);
    }
    private boolean mirror(TreeNode a, TreeNode b) {
        if (a == null || b == null) return a == b;
        return a.val == b.val && mirror(a.left, b.right) && mirror(a.right, b.left);
    }
}
```

基础语法与思想：树镜像比较不是同向比较，而是左的左对右的右、左的右对右的左。

### 102. 二叉树的层序遍历

基础解法：递归 DFS 按深度放入列表。
资深解法：队列 BFS，每次处理一整层。

```java
class Solution {
    public java.util.List<java.util.List<Integer>> levelOrder(TreeNode root) {
        java.util.List<java.util.List<Integer>> ans = new java.util.ArrayList<>();
        if (root == null) return ans;
        java.util.Queue<TreeNode> q = new java.util.ArrayDeque<>();
        q.offer(root);
        while (!q.isEmpty()) {
            int size = q.size();
            java.util.List<Integer> level = new java.util.ArrayList<>();
            for (int i = 0; i < size; i++) {
                TreeNode node = q.poll();
                level.add(node.val);
                if (node.left != null) q.offer(node.left);
                if (node.right != null) q.offer(node.right);
            }
            ans.add(level);
        }
        return ans;
    }
}
```

基础语法与思想：`Queue.offer/poll` 实现 BFS；`size` 固定当前层节点数。

### 103. 二叉树的锯齿形层序遍历

基础解法：普通层序后，奇数层反转列表。
资深解法：按层使用双端队列决定头插或尾插。

```java
class Solution {
    public java.util.List<java.util.List<Integer>> zigzagLevelOrder(TreeNode root) {
        java.util.List<java.util.List<Integer>> ans = new java.util.ArrayList<>();
        if (root == null) return ans;
        java.util.Queue<TreeNode> q = new java.util.ArrayDeque<>();
        q.offer(root);
        boolean leftToRight = true;
        while (!q.isEmpty()) {
            java.util.LinkedList<Integer> level = new java.util.LinkedList<>();
            for (int i = q.size(); i > 0; i--) {
                TreeNode node = q.poll();
                if (leftToRight) level.addLast(node.val); else level.addFirst(node.val);
                if (node.left != null) q.offer(node.left);
                if (node.right != null) q.offer(node.right);
            }
            ans.add(level);
            leftToRight = !leftToRight;
        }
        return ans;
    }
}
```

基础语法与思想：`LinkedList.addFirst` 可在层内反向收集。

### 104. 二叉树的最大深度

基础解法：层序遍历统计层数。
资深解法：递归深度等于左右子树最大深度加一。

```java
class Solution {
    public int maxDepth(TreeNode root) {
        if (root == null) return 0;
        return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
    }
}
```

基础语法与思想：树高度题天然适合后序递归。

### 105. 从前序与中序遍历序列构造二叉树

基础解法：每次在中序数组线性查找根节点位置。
资深解法：用哈希表记录中序值到下标，递归切分左右子树。

```java
class Solution {
    private int preIndex = 0;
    private java.util.Map<Integer, Integer> inIndex = new java.util.HashMap<>();
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        for (int i = 0; i < inorder.length; i++) inIndex.put(inorder[i], i);
        return build(preorder, 0, inorder.length - 1);
    }
    private TreeNode build(int[] preorder, int left, int right) {
        if (left > right) return null;
        int val = preorder[preIndex++];
        TreeNode root = new TreeNode(val);
        int mid = inIndex.get(val);
        root.left = build(preorder, left, mid - 1);
        root.right = build(preorder, mid + 1, right);
        return root;
    }
}
```

基础语法与思想：前序第一个是根；中序根左侧是左子树、右侧是右子树。

### 106. 从中序与后序遍历序列构造二叉树

基础解法：在中序中线性找根并递归。
资深解法：后序从末尾取根，递归顺序先右后左。

```java
class Solution {
    private int postIndex;
    private java.util.Map<Integer, Integer> inIndex = new java.util.HashMap<>();
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        postIndex = postorder.length - 1;
        for (int i = 0; i < inorder.length; i++) inIndex.put(inorder[i], i);
        return build(postorder, 0, inorder.length - 1);
    }
    private TreeNode build(int[] postorder, int left, int right) {
        if (left > right) return null;
        int val = postorder[postIndex--];
        TreeNode root = new TreeNode(val);
        int mid = inIndex.get(val);
        root.right = build(postorder, mid + 1, right);
        root.left = build(postorder, left, mid - 1);
        return root;
    }
}
```

基础语法与思想：后序末尾是根；倒序消费后序时要先构造右子树。

### 107. 二叉树的层序遍历 II

基础解法：普通层序后 `Collections.reverse(ans)`。
资深解法：每层结果头插到链表。

```java
class Solution {
    public java.util.List<java.util.List<Integer>> levelOrderBottom(TreeNode root) {
        java.util.LinkedList<java.util.List<Integer>> ans = new java.util.LinkedList<>();
        if (root == null) return ans;
        java.util.Queue<TreeNode> q = new java.util.ArrayDeque<>();
        q.offer(root);
        while (!q.isEmpty()) {
            java.util.List<Integer> level = new java.util.ArrayList<>();
            for (int i = q.size(); i > 0; i--) {
                TreeNode node = q.poll();
                level.add(node.val);
                if (node.left != null) q.offer(node.left);
                if (node.right != null) q.offer(node.right);
            }
            ans.addFirst(level);
        }
        return ans;
    }
}
```

基础语法与思想：链表头插适合倒序收集层。

### 108. 将有序数组转换为二叉搜索树

基础解法：每次取中点作为根。
资深解法：递归区间 `[left,right]` 构造高度平衡 BST。

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

基础语法与思想：有序数组中点作为根可保证左右规模接近。

### 109. 有序链表转换二叉搜索树

基础解法：链表转数组后复用 108。
资深解法：快慢指针找中点作为根，递归左右链表区间。

```java
class Solution {
    public TreeNode sortedListToBST(ListNode head) {
        java.util.List<Integer> list = new java.util.ArrayList<>();
        while (head != null) { list.add(head.val); head = head.next; }
        return build(list, 0, list.size() - 1);
    }
    private TreeNode build(java.util.List<Integer> a, int l, int r) {
        if (l > r) return null;
        int m = l + (r - l) / 2;
        TreeNode root = new TreeNode(a.get(m));
        root.left = build(a, l, m - 1);
        root.right = build(a, m + 1, r);
        return root;
    }
}
```

基础语法与思想：链表不支持随机访问，转数组可简化实现。

### 110. 平衡二叉树

基础解法：每个节点重复计算左右子树高度。
资深解法：后序递归返回高度，发现不平衡返回 `-1` 作为哨兵。

```java
class Solution {
    public boolean isBalanced(TreeNode root) {
        return height(root) != -1;
    }
    private int height(TreeNode node) {
        if (node == null) return 0;
        int left = height(node.left), right = height(node.right);
        if (left == -1 || right == -1 || Math.abs(left - right) > 1) return -1;
        return 1 + Math.max(left, right);
    }
}
```

基础语法与思想：后序能同时获得子树信息并向上剪枝。

### 111. 二叉树的最小深度

基础解法：DFS 处理空子树边界。
资深解法：BFS 第一次遇到叶子就是最小深度。

```java
class Solution {
    public int minDepth(TreeNode root) {
        if (root == null) return 0;
        java.util.Queue<TreeNode> q = new java.util.ArrayDeque<>();
        q.offer(root);
        int depth = 1;
        while (!q.isEmpty()) {
            for (int i = q.size(); i > 0; i--) {
                TreeNode node = q.poll();
                if (node.left == null && node.right == null) return depth;
                if (node.left != null) q.offer(node.left);
                if (node.right != null) q.offer(node.right);
            }
            depth++;
        }
        return depth;
    }
}
```

基础语法与思想：最短层数用 BFS 更早停止。

### 112. 路径总和

基础解法：DFS 枚举所有根到叶路径和。
资深解法：递归传递剩余目标值，到叶子时判断是否等于节点值。

```java
class Solution {
    public boolean hasPathSum(TreeNode root, int targetSum) {
        if (root == null) return false;
        if (root.left == null && root.right == null) return targetSum == root.val;
        return hasPathSum(root.left, targetSum - root.val) || hasPathSum(root.right, targetSum - root.val);
    }
}
```

基础语法与思想：根到叶路径必须在叶子节点结束。

### 113. 路径总和 II

基础解法：DFS 路径列表，命中后复制路径。
资深解法：回溯维护路径和剩余目标，返回时移除当前节点。

```java
class Solution {
    public java.util.List<java.util.List<Integer>> pathSum(TreeNode root, int targetSum) {
        java.util.List<java.util.List<Integer>> ans = new java.util.ArrayList<>();
        dfs(root, targetSum, new java.util.ArrayList<>(), ans);
        return ans;
    }
    private void dfs(TreeNode node, int remain, java.util.List<Integer> path, java.util.List<java.util.List<Integer>> ans) {
        if (node == null) return;
        path.add(node.val);
        if (node.left == null && node.right == null && remain == node.val) ans.add(new java.util.ArrayList<>(path));
        dfs(node.left, remain - node.val, path, ans);
        dfs(node.right, remain - node.val, path, ans);
        path.remove(path.size() - 1);
    }
}
```

基础语法与思想：收集路径时必须复制 `path`，否则会被后续回溯修改。

### 114. 二叉树展开为链表

基础解法：前序遍历保存节点列表，再重连成右链。
资深解法：后序递归原地展开，或用 Morris 风格把左子树插到右侧。

```java
class Solution {
    public void flatten(TreeNode root) {
        TreeNode cur = root;
        while (cur != null) {
            if (cur.left != null) {
                TreeNode pre = cur.left;
                while (pre.right != null) pre = pre.right;
                pre.right = cur.right;
                cur.right = cur.left;
                cur.left = null;
            }
            cur = cur.right;
        }
    }
}
```

基础语法与思想：展开顺序是前序；左子树最右节点接原右子树。

### 115. 不同的子序列

基础解法：递归比较选或不选当前字符。
资深解法：`dp[i][j]` 表示 `s` 前 `i` 个中形成 `t` 前 `j` 个的方案数。

```java
class Solution {
    public int numDistinct(String s, String t) {
        int m = s.length(), n = t.length();
        long[][] dp = new long[m + 1][n + 1];
        for (int i = 0; i <= m; i++) dp[i][0] = 1;
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                dp[i][j] = dp[i - 1][j];
                if (s.charAt(i - 1) == t.charAt(j - 1)) dp[i][j] += dp[i - 1][j - 1];
            }
        }
        return (int) dp[m][n];
    }
}
```

基础语法与思想：子序列可以跳过字符；空 `t` 有 1 种形成方式。

### 116. 填充每个节点的下一个右侧节点指针

基础解法：层序遍历逐层连接 `next`。
资深解法：完美二叉树可用已有 `next` 串联下一层。

```java
class Solution {
    public Node connect(Node root) {
        if (root == null) return null;
        Node leftmost = root;
        while (leftmost.left != null) {
            Node cur = leftmost;
            while (cur != null) {
                cur.left.next = cur.right;
                if (cur.next != null) cur.right.next = cur.next.left;
                cur = cur.next;
            }
            leftmost = leftmost.left;
        }
        return root;
    }
}
```

基础语法与思想：完美二叉树每个非叶节点都有左右孩子，可以常数空间连接。

### 117. 填充每个节点的下一个右侧节点指针 II

基础解法：BFS 层序连接。
资深解法：用虚拟头结点收集下一层链表，适用于非完美二叉树。

```java
class Solution {
    public Node connect(Node root) {
        Node cur = root;
        while (cur != null) {
            Node dummy = new Node(0), tail = dummy;
            while (cur != null) {
                if (cur.left != null) { tail.next = cur.left; tail = tail.next; }
                if (cur.right != null) { tail.next = cur.right; tail = tail.next; }
                cur = cur.next;
            }
            cur = dummy.next;
        }
        return root;
    }
}
```

基础语法与思想：`next` 链让我们能横向扫描当前层，同时构造下一层。

### 118. 杨辉三角

基础解法：按行生成，每个中间值等于上一行相邻两数之和。
资深解法：滚动构造当前行后加入答案。

```java
class Solution {
    public java.util.List<java.util.List<Integer>> generate(int numRows) {
        java.util.List<java.util.List<Integer>> ans = new java.util.ArrayList<>();
        for (int r = 0; r < numRows; r++) {
            java.util.List<Integer> row = new java.util.ArrayList<>();
            for (int c = 0; c <= r; c++) {
                if (c == 0 || c == r) row.add(1);
                else row.add(ans.get(r - 1).get(c - 1) + ans.get(r - 1).get(c));
            }
            ans.add(row);
        }
        return ans;
    }
}
```

基础语法与思想：二维列表用 `get(row).get(col)` 访问。

### 119. 杨辉三角 II

基础解法：生成前 `rowIndex + 1` 行后取最后一行。
资深解法：一维数组从右向左更新，避免覆盖左上角旧值。

```java
class Solution {
    public java.util.List<Integer> getRow(int rowIndex) {
        java.util.List<Integer> row = new java.util.ArrayList<>();
        for (int i = 0; i <= rowIndex; i++) {
            row.add(1);
            for (int j = i - 1; j > 0; j--) row.set(j, row.get(j) + row.get(j - 1));
        }
        return row;
    }
}
```

基础语法与思想：`List.set(index,value)` 修改已有元素；从右往左防止状态污染。

### 120. 三角形最小路径和

基础解法：二维 DP 自顶向下。
资深解法：自底向上一维 DP，`dp[j] = min(dp[j], dp[j+1]) + value`。

```java
class Solution {
    public int minimumTotal(java.util.List<java.util.List<Integer>> triangle) {
        int n = triangle.size();
        int[] dp = new int[n + 1];
        for (int i = n - 1; i >= 0; i--) {
            for (int j = 0; j <= i; j++) {
                dp[j] = Math.min(dp[j], dp[j + 1]) + triangle.get(i).get(j);
            }
        }
        return dp[0];
    }
}
```

基础语法与思想：自底向上可天然处理底边界；三角形每步只能走下一行相邻位置。
