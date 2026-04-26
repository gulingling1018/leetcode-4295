# 数据库 SQL 学习手册（Java 版）

面向题库中的数据库高相关题（约 55 题），目标是让 0 基础同学独立完成绝大多数 SQL 题。

## 1. SQL 基础语法（先背会）

```sql
SELECT col1, col2
FROM table_a
LEFT JOIN table_b ON a.id = b.a_id
WHERE condition
GROUP BY col1
HAVING agg_condition
ORDER BY col1 DESC
LIMIT 10;
```

关键子句说明：

- `SELECT`：选择列
- `FROM`：数据来源
- `JOIN ... ON ...`：多表连接
- `WHERE`：分组前过滤
- `GROUP BY`：分组
- `HAVING`：分组后过滤
- `ORDER BY`：排序
- `LIMIT`：限制行数

## 2. 题型一：单表查询与过滤

代表题：`595 619 1517 1527`

### 基础

- `SELECT + WHERE + ORDER BY`
- 明确比较运算：`=`, `<>`, `>`, `<`, `BETWEEN`, `IN`

### 熟练

- 使用 `CASE WHEN` 做条件映射。
- 使用 `IFNULL/COALESCE` 处理空值。

### 资深

- 先写最小可读查询，再考虑索引列过滤顺序。
- 过滤条件写成可复用片段（便于联调）。

## 3. 题型二：连接查询（JOIN）

代表题：`175 181 183 607 1045 1398`

### 基础

- `INNER JOIN`：两边都匹配才保留
- `LEFT JOIN`：保留左表全部

### 熟练

- 反连接（找“没有”）：
  - `LEFT JOIN ... WHERE right.id IS NULL`

### 资深

- 区分“连接条件”与“过滤条件”的放置位置（`ON` vs `WHERE`）。
- 对 1:N 连接导致重复行保持敏感。

## 4. 题型三：分组与聚合

代表题：`184 185 586 1193 1321 1484`

### 基础

- `COUNT/SUM/AVG/MIN/MAX`
- `GROUP BY` 单列或多列

### 熟练

- `HAVING` 过滤聚合结果。
- 多指标一起输出（例如销售额 + 订单数）。

### 资深

- 先聚合再连接，减少中间数据量。
- 分组口径固定，避免“粒度漂移”。

## 5. 题型四：排名与窗口函数

代表题：`178 185 571 1407`

### 基础

- 子查询做排名（传统写法）。

### 熟练

- 使用窗口函数：
  - `ROW_NUMBER() OVER(...)`
  - `RANK() OVER(...)`
  - `DENSE_RANK() OVER(...)`

### 资深

- 明确并列处理规则（跳号/不跳号）。
- 将窗口函数和 CTE 组合提高可读性。

## 6. 题型五：日期与行为分析

代表题：`197 1141 1164 1174 1327`

### 基础

- 日期比较：`DATE(col)`, `DATEDIFF(...)`
- 固定时间窗：最近 N 天

### 熟练

- 按日/周/月聚合行为数据。
- 处理缺失日期（补零）与重复记录去重。

### 资深

- 多时间窗口并行对比（本周 vs 上周）。
- 注意时区与边界时间（`00:00:00`）。

## 7. Java 侧配套（JDBC）

LeetCode 数据库题核心是 SQL，但工程中要会 Java 调用：

```java
String sql = "SELECT id, name FROM users WHERE score > ?";
PreparedStatement ps = conn.prepareStatement(sql);
ps.setInt(1, 80);
ResultSet rs = ps.executeQuery();
while (rs.next()) {
    int id = rs.getInt("id");
    String name = rs.getString("name");
}
```

语法与函数详解：

- `prepareStatement`：预编译 SQL，防注入。
- `setInt(index, value)`：绑定参数，`index` 从 1 开始。
- `executeQuery()`：执行查询，返回 `ResultSet`。
- `rs.next()`：游标后移，返回是否还有数据。

## 8. 数据库题三层作答模板

1. 基础：先写正确 SQL（即使较长）
2. 熟练：改为更清晰版本（CTE/窗口函数）
3. 资深：解释可维护性与性能（索引、连接顺序、扫描行数）

## 9. 必练清单（建议顺序）

- 入门：`175 181 182 183 595 620`
- 进阶：`184 185 586 607 1141 1164 1193`
- 强化：`1321 1341 1393 1484 1517 1527`

