# DQL分页查询

`LIMIT`用于限制查询结果返回的记录数量，也可以指定从哪一条记录开始返回。分页语法在不同数据库中可能不同，MySQL使用`LIMIT`。

## 1. 查询前几条记录

```text
SELECT 字段列表
FROM 表名
LIMIT 记录数;
```

例如，查询前`5`名员工：

```sql
SELECT id, name
FROM employee
ORDER BY id ASC
LIMIT 5;
```

## 2. 指定起始位置和记录数

MySQL支持以下两种等价写法：

```text
LIMIT 起始位置, 记录数;
```

```text
LIMIT 记录数 OFFSET 起始位置;
```

起始位置从`0`开始：`0`表示从第一条记录开始，`5`表示跳过前五条记录后再开始返回。

查询第一页时，起始位置是`0`，因此可以省略起始位置，直接写成`LIMIT 记录数`。例如，每页显示`10`条记录时，第一页可以写成`LIMIT 10`。

例如，跳过前`5`条记录，再返回`5`条记录：

```sql
SELECT id, name
FROM employee
ORDER BY id ASC
LIMIT 5, 5;
```

也可以写成：

```sql
SELECT id, name
FROM employee
ORDER BY id ASC
LIMIT 5 OFFSET 5;
```

## 3. 根据页码计算起始位置

```text
起始位置 = (页码 - 1) × 每页数量
```

假设每页显示`10`条记录：

| 页码 | 起始位置 | LIMIT写法 |
| ---: | ---: | --- |
| 第1页 | `0` | `LIMIT 0, 10` |
| 第2页 | `10` | `LIMIT 10, 10` |
| 第3页 | `20` | `LIMIT 20, 10` |

查询第`3`页的示例：

```sql
SELECT id, name
FROM employee
ORDER BY id ASC
LIMIT 20, 10;
```

## 4. 排序与分页

分页查询通常应配合`ORDER BY`使用。没有明确排序时，数据库不保证每次都按照相同顺序返回记录，可能造成不同页面之间出现重复或遗漏。

排序字段的值可能重复时，可以再加入唯一字段作为第二排序条件，使分页顺序更加稳定：

```sql
SELECT id, name, age
FROM employee
ORDER BY age DESC, id ASC
LIMIT 0, 10;
```

## 5. 相关笔记

- [[DQL基本查询]]
- [[DQL条件查询]]
- [[DQL分组查询]]
- [[DQL排序查询]]
- [[多表查询概述]]
- [[SQL基础语法]]
- [[数据库学习导航]]
