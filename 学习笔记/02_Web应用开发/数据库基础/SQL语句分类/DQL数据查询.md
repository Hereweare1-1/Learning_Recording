# DQL数据查询

DQL（Data Query Language，数据查询语言）用于查询数据表中的记录，核心关键字是`SELECT`。

## 1. 语句汇总

| 作用 | 基本语句 |
| --- | --- |
| 查询指定字段 | `SELECT 字段名 FROM 表名;` |

## 2. SELECT：查询数据

```sql
SELECT id, name
FROM employee;
```

这条语句会查询`employee`表中的`id`和`name`字段。

`SELECT`可以与查询条件、排序、分组和表连接等语法组合，完成不同的数据查询需求。

## 3. 相关笔记

- [[SQL基础语法]]
- [[数据库学习导航]]
