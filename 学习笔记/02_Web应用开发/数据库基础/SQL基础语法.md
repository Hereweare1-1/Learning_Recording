# SQL基础语法

这篇笔记只保留SQL的通用书写规则和分类入口。各类命令的详细说明放在对应的专题笔记中。

## 1. SQL语句的基本书写规则

### 1.1 使用分号结束语句

SQL语句可以写成一行，也可以为了便于阅读拆成多行，通常使用分号`;`表示一条语句结束。

```sql
SELECT id, name
FROM employee
WHERE dept_id = 1;
```

### 1.2 使用空格和缩进

SQL通常不依赖普通空格和换行判断语句结构，但合理的换行和缩进可以让查询更容易阅读。

```sql
SELECT id, name FROM employee WHERE dept_id = 1;
```

上面的一行写法与前面的多行写法作用相同。

### 1.3 大小写

MySQL中的SQL关键字不区分大小写，下面两种写法都可以执行：

```sql
select * from employee;

SELECT * FROM employee;
```

为了区分关键字和名称，通常将SQL关键字写成大写，将数据库名、表名和字段名写成小写。

> [!note]
> SQL关键字不区分大小写，不代表所有名称都一定不区分大小写。MySQL中的数据库名和表名是否区分大小写，可能受操作系统和配置影响，因此我应保持命名和引用时的大小写一致。

## 2. SQL注释

### 2.1 单行注释

```sql
-- 查询所有员工，双横线后需要有空格
SELECT * FROM employee;

# MySQL支持的单行注释
SELECT * FROM department;
```

- `-- `：常见的SQL单行注释。MySQL要求双横线后跟空格或控制字符。
- `#`：MySQL支持的单行注释写法，不是所有数据库都支持。

### 2.2 多行注释

```sql
/*
查询研发部中的员工
这段注释可以跨越多行
*/
SELECT * FROM employee WHERE dept_id = 1;
```

## 3. SQL语句的四种分类

| 分类 | 全称 | 主要作用 | 常见关键字 | 专题笔记 |
| --- | --- | --- | --- | --- |
| DDL | Data Definition Language | 定义数据库、表和字段等对象 | `CREATE`、`ALTER`、`DROP`、`TRUNCATE` | [[DDL数据库与表结构操作]] |
| DML | Data Manipulation Language | 新增、修改和删除表中的数据 | `INSERT`、`UPDATE`、`DELETE` | [[DML数据操作]] |
| DQL | Data Query Language | 查询表中的数据 | `SELECT` | [[DQL基本查询]] |
| DCL | Data Control Language | 管理用户和数据库访问权限 | `GRANT`、`REVOKE` | [[DCL权限控制]] |

返回：[[数据库学习导航]]。
