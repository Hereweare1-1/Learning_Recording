# DQL基本查询

DQL（Data Query Language，数据查询语言）用于查询数据表中的记录，核心关键字是`SELECT`。

## 1. 语句汇总

| 查询类型 | 关键字或函数 | 作用 |
| --- | --- | --- |
| 基本查询 | `SELECT`、`FROM` | 指定要查询的字段和数据表 |
| 条件查询 | `WHERE` | 筛选符合条件的记录 |
| 聚合与分组查询 | `COUNT`、`GROUP BY`、`HAVING`等 | 统计数据并筛选分组结果 |
| 排序查询 | `ORDER BY` | 按指定字段排序 |
| 分页查询 | `LIMIT` | 限制返回记录的位置和数量 |
| 多表查询 | 连接查询、联合查询、子查询 | 从多张表或多次查询中组合数据 |

详细内容见：

- [[DQL条件查询]]
- [[DQL分组查询]]
- [[DQL排序查询]]
- [[DQL分页查询]]
- [[多表查询概述]]

## 2. DQL查询结构与执行顺序

### 2.1 书写顺序

```text
SELECT 字段列表
FROM 表名列表
[WHERE 条件列表]
[GROUP BY 分组字段列表]
[HAVING 分组后条件列表]
[ORDER BY 排序字段列表]
[LIMIT 分页参数];
```

方括号`[]`表示其中的子句可以省略，实际书写SQL时不需要输入方括号。各子句在SQL中应按照上面的顺序书写：

1. `SELECT`指定返回哪些字段。
2. `FROM`指定从哪些表中查询数据。
3. `WHERE`在分组前筛选记录。
4. `GROUP BY`按照指定字段分组。
5. `HAVING`筛选分组后的结果。
6. `ORDER BY`对查询结果排序。
7. `LIMIT`限制最终返回的记录。

### 2.2 逻辑执行顺序

SQL的书写顺序和逻辑执行顺序并不相同。查询可以按照下面的顺序理解：

```text
FROM：确定数据来自哪些表
  ↓
WHERE：筛选参与查询的原始记录
  ↓
GROUP BY：对记录进行分组并计算聚合结果
  ↓
HAVING：筛选分组后的结果
  ↓
SELECT：确定最终返回的字段
  ↓
DISTINCT：去除重复结果
  ↓
ORDER BY：对结果排序
  ↓
LIMIT：限制最终返回的记录
```

例如，`WHERE`在`SELECT`之前处理，因此通常不能在`WHERE`中直接使用本次查询在`SELECT`里设置的字段别名。

> [!note]
> 这里描述的是便于理解查询结果的逻辑执行顺序。数据库优化器可能调整内部的实际执行方式，但不会改变SQL应得到的结果。

## 3. SELECT：基本查询

### 3.1 查询指定字段或全部字段

查询一个或多个指定字段：

```text
SELECT 字段1, 字段2, 字段3, ...
FROM 表名;
```

例如：

```sql
SELECT id, name
FROM employee;
```

这条语句会查询`employee`表中的`id`和`name`字段。

使用`*`可以查询表中的全部字段：

```text
SELECT *
FROM 表名;
```

例如：

```sql
SELECT *
FROM employee;
```

`*`书写方便，但查询结果会依赖表中现有的全部字段。只需要部分数据时，明确写出字段名更容易看出查询目的。

### 3.2 设置字段别名

```text
SELECT 字段1 [AS 别名1], 字段2 [AS 别名2], ...
FROM 表名;
```

方括号表示设置别名是可选的，实际SQL中不写方括号。别名只改变查询结果中显示的列名，不会修改数据表原来的字段名。

例如：

```sql
SELECT name AS employee_name, job AS employee_job
FROM employee;
```

在MySQL中，`AS`关键字也可以省略，但保留`AS`通常更容易看出原字段名和别名的关系。

### 3.3 去除重复记录

```text
SELECT DISTINCT 字段列表
FROM 表名;
```

例如，查询员工表中不重复的岗位：

```sql
SELECT DISTINCT job
FROM employee;
```

当`DISTINCT`后面有多个字段时，只有这些字段的组合完全相同，才会被视为重复记录。

## 4. 相关笔记

- [[DQL条件查询]]
- [[DQL分组查询]]
- [[DQL排序查询]]
- [[DQL分页查询]]
- [[多表查询概述]]
- [[MySQL字符串函数]]
- [[MySQL数值函数]]
- [[MySQL日期函数]]
- [[MySQL流程函数]]
- [[SQL基础语法]]
- [[数据库学习导航]]
