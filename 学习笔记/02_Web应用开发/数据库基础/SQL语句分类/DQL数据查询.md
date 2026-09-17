# DQL数据查询

DQL（Data Query Language，数据查询语言）用于查询数据表中的记录，核心关键字是`SELECT`。

## 1. 语句汇总

| 查询类型 | 关键字或函数 | 作用 |
| --- | --- | --- |
| 基本查询 | `SELECT`、`FROM` | 指定要查询的字段和数据表 |
| 条件查询 | `WHERE` | 筛选符合条件的记录 |
| 聚合查询 | `COUNT`、`MAX`、`MIN`、`AVG`、`SUM` | 统计数量、最大值、最小值、平均值或总和 |
| 分组查询 | `GROUP BY`、`HAVING` | 对记录分组，并筛选分组后的结果 |
| 排序查询 | `ORDER BY` | 按指定字段排序 |
| 分页查询 | `LIMIT` | 限制返回记录的位置和数量 |

## 2. DQL查询语句结构

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

`WHERE`和`HAVING`都用于筛选，但作用对象不同：`WHERE`筛选原始记录，`HAVING`筛选分组后的结果。

## 3. SELECT：基本查询

```sql
SELECT id, name
FROM employee;
```

这条语句会查询`employee`表中的`id`和`name`字段。

`SELECT`可以与查询条件、排序、分组和表连接等语法组合，完成不同的数据查询需求。

## 4. 相关笔记

- [[SQL基础语法]]
- [[数据库学习导航]]
