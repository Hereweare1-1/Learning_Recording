# SQL基础语法

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

## 3. SQL语句的常见分类

不同资料对SQL分类的范围可能略有差异，现阶段可以按照下面的常见分类理解：

| 分类 | 全称 | 主要作用 | 常见关键字 | 学习要求 |
| --- | --- | --- | --- | --- |
| DDL | Data Definition Language | 定义数据库、表和字段等对象 | `CREATE`、`ALTER`、`DROP` | **重点掌握** |
| DML | Data Manipulation Language | 新增、修改和删除表中的数据 | `INSERT`、`UPDATE`、`DELETE` | **重点掌握** |
| DQL | Data Query Language | 查询表中的数据 | `SELECT` | **重点掌握** |
| DCL | Data Control Language | 管理用户和访问权限 | `GRANT`、`REVOKE` | 了解即可 |
| TCL | Transaction Control Language | 控制事务的提交和回滚 | `COMMIT`、`ROLLBACK` | 先了解，学习事务时再掌握 |

### 3.1 DDL：定义数据结构

DDL操作的是数据库和数据表的结构，而不是表中的某一条具体数据。

**如何阅读语法格式**

```text
CREATE DATABASE [IF NOT EXISTS] 数据库名
    [DEFAULT CHARACTER SET 字符集]
    [COLLATE 排序规则];
```

- 方括号`[]`中的内容表示**可以省略的可选部分**。
- 实际输入SQL时，不要把方括号本身写进去。
- `数据库名`、`字符集`和`排序规则`是占位说明，需要替换成实际值。

最简写法：

```sql
CREATE DATABASE demo;
```

包含可选部分的写法：

```sql
CREATE DATABASE IF NOT EXISTS demo
DEFAULT CHARACTER SET utf8mb4
COLLATE utf8mb4_unicode_ci;
```

**查询数据库**

查询当前账户能够看到的数据库：

```sql
SHOW DATABASES;
```

查询当前正在使用的数据库：

```sql
SELECT DATABASE();
```

如果还没有使用`USE`选择数据库，`SELECT DATABASE()`通常返回`NULL`。

**创建数据库**

```text
CREATE DATABASE [IF NOT EXISTS] 数据库名
    [DEFAULT CHARACTER SET 字符集]
    [COLLATE 排序规则];
```

- `IF NOT EXISTS`：数据库不存在时才创建，避免数据库已经存在时报错。
- `DEFAULT CHARACTER SET`：指定数据库的默认字符集。
- `COLLATE`：指定数据库的默认排序和比较规则。

学习示例：

```sql
CREATE DATABASE IF NOT EXISTS fastapi_test
DEFAULT CHARACTER SET utf8mb4
COLLATE utf8mb4_unicode_ci;
```

**删除数据库**

```text
DROP DATABASE [IF EXISTS] 数据库名;
```

`IF EXISTS`表示数据库存在时才删除，也可以省略。

```sql
DROP DATABASE IF EXISTS demo;
```

> [!warning]
> `DROP DATABASE`会删除整个数据库以及其中的数据表和数据。执行前必须确认数据库名称，并确保重要数据已经备份。

**选择数据库**

```text
USE 数据库名;
```

例如：

```sql
USE fastapi_test;
```

选择数据库后，后续没有明确写出数据库名的建表和查询操作，会默认在当前数据库中执行。重新连接MySQL后，通常需要再次使用`USE`选择数据库。

这组命令通常放在“DDL数据库操作”中一起学习。其中`CREATE DATABASE`和`DROP DATABASE`属于DDL；`SHOW DATABASES`、`SELECT DATABASE()`和`USE`是配套使用的查询或切换命令。

### 3.2 DML：修改数据

```sql
INSERT INTO employee (name, job, dept_id)
VALUES ('小明', '开发', 1);
```

DML主要负责向表中新增、修改和删除数据。

### 3.3 DQL：查询数据

```sql
SELECT id, name
FROM employee;
```

`SELECT`是实际开发中使用非常频繁的SQL语句，后续需要重点学习查询条件、排序、分组和多表查询。

## 4. 当前需要掌握什么

我需要掌握SQL语句的分号、缩进、大小写习惯和三种注释写法，能够区分DDL、DML和DQL，并会使用DDL相关命令查询、创建、选择和谨慎删除数据库。DCL和TCL现阶段知道用途即可，等学习用户权限和事务时再深入。

返回：[[数据库学习导航]]。
