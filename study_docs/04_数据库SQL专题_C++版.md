# 数据库 SQL 学习手册（C++ 版）

说明：LeetCode 数据库题依旧以 SQL 为核心，C++ 版本主要补充“SQL 思维 + C++ 工程接入”。

## 1. SQL 主体（和语言无关）

核心语句：

```sql
SELECT ...
FROM ...
JOIN ...
WHERE ...
GROUP BY ...
HAVING ...
ORDER BY ...
LIMIT ...;
```

重点题型：

- 连接：`175 181 183 607`
- 分组聚合：`184 185 586 1193 1484`
- 排名窗口：`178 185 571 1407`
- 日期分析：`197 1141 1164 1327`

## 2. 三层解法（数据库题固定模板）

### 基础

- 先写“能跑通”的 SQL，不追求最短。
- 先保证口径正确：分组维度、筛选条件、排序方向。

### 熟练

- 用 CTE（`WITH`）拆步骤，让查询可读。
- 对“找不存在”的问题优先考虑 `LEFT JOIN ... IS NULL`。

### 资深

- 明确执行代价：
  - 哪个条件可走索引
  - 哪一步会放大数据行数
  - 是否能先聚合再连接

## 3. C++ 侧常见接入方式（工程）

LeetCode 平台不要求你写 C++ 连接数据库，但真实开发常见：

- ODBC / MySQL Connector C++
- 预编译语句（prepared statement）

伪代码示例（强调接口概念）：

```cpp
auto stmt = conn.prepare("SELECT id, name FROM users WHERE score > ?");
stmt.bind(1, 80);
auto rs = stmt.executeQuery();
while (rs.next()) {
    int id = rs.getInt("id");
    std::string name = rs.getString("name");
}
```

函数语义：

- `prepare(sql)`：预编译 SQL，减少注入风险。
- `bind(pos, value)`：参数绑定。
- `executeQuery()`：执行查询语句。
- `next()`：迭代结果集。

## 4. SQL 关键语法详解

### `GROUP BY` 与 `HAVING`

- `WHERE`：聚合前过滤
- `HAVING`：聚合后过滤

### `RANK / DENSE_RANK / ROW_NUMBER`

- `RANK`：并列后跳号
- `DENSE_RANK`：并列后不跳号
- `ROW_NUMBER`：强制唯一序号

### 空值处理

- `COALESCE(a, b)`：取第一个非空值
- `IFNULL(a, b)`：MySQL 常用空值替换

## 5. 高频错误清单

- 把 `LEFT JOIN` 条件错误写到 `WHERE`，导致退化为内连接
- 忘记 `GROUP BY` 粒度，导致重复统计
- 排名题没处理并列，结果错误
- 日期比较把字符串当日期直接比较

## 6. 必练清单（SQL）

- 基础：`175 176 177 181 182 183`
- 聚合：`184 185 586 1193 1321`
- 业务分析：`1141 1164 1174 1327 1484`
- 综合：`1393 1407 1517 1527`

## 7. C++ 刷题协同建议

并行推进两条线：

1. C++ 算法题（数组/树图/DP）
2. SQL 数据库题（每天 2~3 题）

目标：避免“算法强但 SQL 弱”的短板。

