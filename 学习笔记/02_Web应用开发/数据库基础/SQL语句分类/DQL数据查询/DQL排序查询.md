# DQL排序查询

`ORDER BY`按照一个或多个字段对查询结果进行排序。

## 1. 基本语法

```text
SELECT 字段列表
FROM 表名
ORDER BY 字段1 排序方式1, 字段2 排序方式2, ...;
```

排序方式包括：

- `ASC`：升序，也是省略排序方式时的默认值。
- `DESC`：降序。

## 2. 单字段排序

按照年龄从小到大查询员工：

```sql
SELECT id, name, age
FROM employee
ORDER BY age ASC;
```

按照年龄从大到小查询员工：

```sql
SELECT id, name, age
FROM employee
ORDER BY age DESC;
```

## 3. 多字段排序

```sql
SELECT id, name, age
FROM employee
ORDER BY age DESC, name ASC;
```

多字段排序会按照字段的书写顺序依次判断：

1. 先按照`age`降序排列。
2. 只有当两条记录的`age`相同时，才按照`name`升序排列。

每个排序字段都可以单独指定`ASC`或`DESC`。

## 4. 相关笔记

- [[DQL基本查询]]
- [[DQL条件查询]]
- [[DQL分组查询]]
- [[DQL分页查询]]
- [[DQL多表查询]]
- [[SQL基础语法]]
- [[数据库学习导航]]
