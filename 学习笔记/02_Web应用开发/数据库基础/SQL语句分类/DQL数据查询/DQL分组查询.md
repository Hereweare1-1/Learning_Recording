# DQL分组查询

聚合函数用于统计一组数据，`GROUP BY`用于把记录分组后分别统计。

## 1. 聚合函数

### 1.1 常见聚合函数

| 函数 | 作用 |
| --- | --- |
| `COUNT()` | 统计数量 |
| `MAX()` | 计算最大值 |
| `MIN()` | 计算最小值 |
| `AVG()` | 计算平均值 |
| `SUM()` | 计算总和 |

### 1.2 基本语法

```text
SELECT 聚合函数(字段)
FROM 表名
[WHERE 条件];
```

方括号表示`WHERE`条件可以省略，实际SQL中不写方括号。

例如，统计员工总数：

```sql
SELECT COUNT(*) AS employee_count
FROM employee;
```

计算员工的最高年龄、最低年龄、平均年龄和年龄总和：

```sql
SELECT
    MAX(age) AS max_age,
    MIN(age) AS min_age,
    AVG(age) AS avg_age,
    SUM(age) AS total_age
FROM employee;
```

### 1.3 COUNT的两种常见写法

- `COUNT(*)`：统计查询结果中的记录总数。
- `COUNT(字段)`：统计该字段不为`NULL`的记录数量。

`MAX()`、`MIN()`、`AVG()`和`SUM()`在计算时也会忽略值为`NULL`的记录。

聚合函数可以和`WHERE`组合，先筛选记录，再进行统计：

```sql
SELECT COUNT(*) AS developer_count
FROM employee
WHERE job = '开发';
```

## 2. GROUP BY：分组查询

`GROUP BY`按照一个或多个字段把记录分成若干组，通常与聚合函数一起使用，用于分别统计每一组的数据。

### 2.1 基本语法

```text
SELECT 分组字段, 聚合函数(字段)
FROM 表名
[WHERE 分组前筛选条件]
GROUP BY 分组字段
[HAVING 分组后筛选条件];
```

方括号表示`WHERE`和`HAVING`可以省略，实际SQL中不写方括号。

例如，按照部门统计员工数量和平均年龄：

```sql
SELECT
    dept_id,
    COUNT(*) AS employee_count,
    AVG(age) AS avg_age
FROM employee
GROUP BY dept_id;
```

查询结果中，每个`dept_id`对应一组统计结果。

### 2.2 WHERE和HAVING的区别

| 对比项 | `WHERE` | `HAVING` |
| --- | --- | --- |
| 执行位置 | 分组和聚合计算之前 | 分组和聚合计算之后 |
| 筛选对象 | 数据表中的原始记录 | 分组后的统计结果 |
| 聚合函数 | 不能直接用聚合函数作为判断条件 | 可以使用聚合函数作为判断条件 |

相关处理顺序可以简单理解为：

```text
WHERE筛选原始记录
        ↓
GROUP BY进行分组
        ↓
聚合函数计算每组结果
        ↓
HAVING筛选分组结果
```

例如，先筛选成年员工，再按部门分组，只保留员工数量不少于`2`人的部门：

```sql
SELECT
    dept_id,
    COUNT(*) AS employee_count
FROM employee
WHERE age >= 18
GROUP BY dept_id
HAVING COUNT(*) >= 2;
```

### 2.3 分组查询的字段规则

使用`GROUP BY`后，`SELECT`中的字段通常应当是：

- `GROUP BY`中使用的分组字段。
- 经过`COUNT()`、`MAX()`、`MIN()`、`AVG()`、`SUM()`等聚合函数计算的字段。

不要直接查询既不参与分组、也没有经过聚合计算的普通字段，否则该字段无法明确对应分组中的哪一条记录，并且在MySQL的严格分组模式下可能直接报错。

## 3. 相关笔记

- [[DQL基本查询]]
- [[DQL条件查询]]
- [[DQL排序查询]]
- [[DQL分页查询]]
- [[SQL基础语法]]
- [[数据库学习导航]]
