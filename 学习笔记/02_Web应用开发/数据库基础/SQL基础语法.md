# SQL基础语法

## 1. 常用命令速查

为了避免在Obsidian表格中显示失效的内部链接，这里直接列出命令。需要详细说明时，可以通过右侧大纲点击后面的同名标题。

### 1.1 对数据库进行操作

| 作用 | 命令 |
| --- | --- |
| 查看所有数据库 | `SHOW DATABASES;` |
| 查看当前数据库 | `SELECT DATABASE();` |
| 创建数据库 | `CREATE DATABASE 数据库名;` |
| 选择数据库 | `USE 数据库名;` |
| 删除数据库 | `DROP DATABASE 数据库名;` |

### 1.2 对数据表进行操作

| 作用 | 命令 |
| --- | --- |
| 创建数据表 | `CREATE TABLE 表名 (...);` |
| 查看所有表 | `SHOW TABLES;` |
| 查看表结构 | `DESC 表名;` |
| 查看建表语句 | `SHOW CREATE TABLE 表名;` |
| 添加字段 | `ALTER TABLE 表名 ADD 字段名 数据类型;` |
| 修改字段类型 | `ALTER TABLE 表名 MODIFY 字段名 新数据类型;` |
| 修改字段名和类型 | `ALTER TABLE 表名 CHANGE 旧字段名 新字段名 数据类型;` |
| 删除字段 | `ALTER TABLE 表名 DROP COLUMN 字段名;` |
| 修改表名 | `ALTER TABLE 表名 RENAME TO 新表名;` |
| 删除整张表 | `DROP TABLE 表名;` |
| 清空表中全部数据 | `TRUNCATE TABLE 表名;` |

## 2. SQL语句的基本书写规则

### 2.1 使用分号结束语句

SQL语句可以写成一行，也可以为了便于阅读拆成多行，通常使用分号`;`表示一条语句结束。

```sql
SELECT id, name
FROM employee
WHERE dept_id = 1;
```

### 2.2 使用空格和缩进

SQL通常不依赖普通空格和换行判断语句结构，但合理的换行和缩进可以让查询更容易阅读。

```sql
SELECT id, name FROM employee WHERE dept_id = 1;
```

上面的一行写法与前面的多行写法作用相同。

### 2.3 大小写

MySQL中的SQL关键字不区分大小写，下面两种写法都可以执行：

```sql
select * from employee;

SELECT * FROM employee;
```

为了区分关键字和名称，通常将SQL关键字写成大写，将数据库名、表名和字段名写成小写。

> [!note]
> SQL关键字不区分大小写，不代表所有名称都一定不区分大小写。MySQL中的数据库名和表名是否区分大小写，可能受操作系统和配置影响，因此我应保持命名和引用时的大小写一致。

## 3. SQL注释

### 3.1 单行注释

```sql
-- 查询所有员工，双横线后需要有空格
SELECT * FROM employee;

# MySQL支持的单行注释
SELECT * FROM department;
```

- `-- `：常见的SQL单行注释。MySQL要求双横线后跟空格或控制字符。
- `#`：MySQL支持的单行注释写法，不是所有数据库都支持。

### 3.2 多行注释

```sql
/*
查询研发部中的员工
这段注释可以跨越多行
*/
SELECT * FROM employee WHERE dept_id = 1;
```

## 4. SQL语句的常见分类

不同资料对SQL分类的范围可能略有差异，现阶段可以按照下面的常见分类理解：

| 分类 | 全称 | 主要作用 | 常见关键字 | 学习要求 |
| --- | --- | --- | --- | --- |
| DDL | Data Definition Language | 定义数据库、表和字段等对象 | `CREATE`、`ALTER`、`DROP` | **重点掌握** |
| DML | Data Manipulation Language | 新增、修改和删除表中的数据 | `INSERT`、`UPDATE`、`DELETE` | **重点掌握** |
| DQL | Data Query Language | 查询表中的数据 | `SELECT` | **重点掌握** |
| DCL | Data Control Language | 管理用户和访问权限 | `GRANT`、`REVOKE` | 了解即可 |
| TCL | Transaction Control Language | 控制事务的提交和回滚 | `COMMIT`、`ROLLBACK` | 先了解，学习事务时再掌握 |

## 5. DDL数据库操作

DDL操作的是数据库和数据表的结构，而不是表中的某一条具体数据。

语法格式中的方括号`[]`表示可以省略的可选部分，实际输入SQL时不写方括号。`数据库名`等中文说明是占位内容，需要替换成实际值。

### 5.1 SHOW DATABASES：查看所有数据库

```sql
SHOW DATABASES;
```

该命令显示当前账户有权查看的数据库。

### 5.2 SELECT DATABASE：查看当前数据库

```sql
SELECT DATABASE();
```

如果还没有使用`USE`选择数据库，查询结果通常是`NULL`。

### 5.3 CREATE DATABASE：创建数据库

```text
CREATE DATABASE [IF NOT EXISTS] 数据库名
    [DEFAULT CHARACTER SET 字符集]
    [COLLATE 排序规则];
```

- `IF NOT EXISTS`：数据库不存在时才创建，避免数据库已经存在时报错。
- `DEFAULT CHARACTER SET`：指定数据库的默认字符集。
- `COLLATE`：指定数据库的默认排序和比较规则。

最简示例：

```sql
CREATE DATABASE demo;
```

完整示例：

```sql
CREATE DATABASE IF NOT EXISTS fastapi_test
DEFAULT CHARACTER SET utf8mb4
COLLATE utf8mb4_unicode_ci;
```

### 5.4 USE：选择数据库

```text
USE 数据库名;
```

例如：

```sql
USE fastapi_test;
```

选择数据库后，后续没有明确写出数据库名的建表和查询操作，会默认在当前数据库中执行。重新连接MySQL后，通常需要再次使用`USE`选择数据库。

### 5.5 DROP DATABASE：删除数据库

```text
DROP DATABASE [IF EXISTS] 数据库名;
```

`IF EXISTS`表示数据库存在时才删除，也可以省略。

```sql
DROP DATABASE IF EXISTS demo;
```

> [!warning]
> `DROP DATABASE`会删除整个数据库以及其中的数据表和数据。执行前必须确认数据库名称，并确保重要数据已经备份。

这组命令通常放在“DDL数据库操作”中一起学习。其中`CREATE DATABASE`和`DROP DATABASE`属于DDL；`SHOW DATABASES`、`SELECT DATABASE()`和`USE`是配套使用的查询或切换命令。

## 6. DDL表操作

### 6.1 CREATE TABLE：创建数据表

创建表之前，需要先使用`USE`选择数据库。

```text
CREATE TABLE 表名 (
    字段1 字段类型 [COMMENT '字段1注释'],
    字段2 字段类型 [COMMENT '字段2注释'],
    ...
    字段n 字段类型 [COMMENT '字段n注释']
) [COMMENT '表注释'];
```

- 方括号中的`COMMENT`属于可选内容，可以省略，实际输入时不写方括号。
- 每个字段之间使用逗号分隔。
- **最后一个字段后面不能写逗号**，否则会出现SQL语法错误。
- 字段注释和表注释需要使用引号包住具体内容。

例如，创建员工表：

```sql
CREATE TABLE employee (
    id INT COMMENT '员工编号',
    name VARCHAR(50) COMMENT '员工姓名',
    job VARCHAR(50) COMMENT '职位',
    dept_id INT COMMENT '部门编号'
) COMMENT '员工表';
```

这个例子暂时只演示表名、字段名、字段类型和注释。主键、非空和默认值等约束后续再学习。

常用的MySQL字段类型见：[[MySQL数据类型]]。

### 6.2 SHOW TABLES：查看所有表

查询当前数据库中的所有表：

```sql
SHOW TABLES;
```

### 6.3 DESC：查看表结构

```text
DESC 表名;
```

例如：

```sql
DESC employee;
```

`DESC`是`DESCRIBE`的简写，可以查看字段名、字段类型、是否允许为空和键信息等表结构。

### 6.4 SHOW CREATE TABLE：查看建表语句

```text
SHOW CREATE TABLE 表名;
```

例如：

```sql
SHOW CREATE TABLE employee;
```

这个命令可以查看MySQL实际保存的建表语句，包括自动补充的默认配置。

### 6.5 ALTER TABLE ADD：添加字段

```text
ALTER TABLE 表名
ADD 字段名 数据类型 [(长度)] [COMMENT '注释'] [约束];
```

方括号中的长度、注释和约束可以省略，实际输入时不写方括号。数据类型是否需要长度，要根据具体类型决定。

例如，给员工表添加年龄字段：

```sql
ALTER TABLE employee
ADD age TINYINT UNSIGNED COMMENT '年龄';
```

### 6.6 ALTER TABLE MODIFY：修改字段类型

```text
ALTER TABLE 表名
MODIFY 字段名 新数据类型 [(长度)];
```

例如，把员工姓名允许的最大长度改为100：

```sql
ALTER TABLE employee
MODIFY name VARCHAR(100);
```

`MODIFY`只修改字段定义，不修改字段名称。修改类型前要确认已有数据能够转换成新类型。

### 6.7 ALTER TABLE CHANGE：修改字段名和类型

```text
ALTER TABLE 表名
CHANGE 旧字段名 新字段名 数据类型 [(长度)] [COMMENT '注释'] [约束];
```

例如，把`job`字段改名为`position`，并重新声明其完整类型：

```sql
ALTER TABLE employee
CHANGE job position VARCHAR(50) COMMENT '职位';
```

使用`CHANGE`时，即使只想修改字段名，也需要重新写出字段的数据类型。

### 6.8 ALTER TABLE DROP COLUMN：删除字段

```text
ALTER TABLE 表名 DROP COLUMN 字段名;
```

例如：

```sql
ALTER TABLE employee DROP COLUMN age;
```

> [!warning]
> 删除字段会同时删除该字段中的全部数据，执行前需要确认字段名并备份重要数据。

### 6.9 ALTER TABLE RENAME TO：修改表名

```text
ALTER TABLE 旧表名 RENAME TO 新表名;
```

例如：

```sql
ALTER TABLE employee RENAME TO employees;
```

### 6.10 DROP TABLE：删除整张表

```text
DROP TABLE [IF EXISTS] 表名;
```

例如：

```sql
DROP TABLE IF EXISTS employees;
```

`IF EXISTS`可以省略。`DROP TABLE`会同时删除表结构和表中的全部数据。

### 6.11 TRUNCATE TABLE：清空表中全部数据

```text
TRUNCATE TABLE 表名;
```

例如：

```sql
TRUNCATE TABLE employee;
```

`TRUNCATE TABLE`会快速清空表中的全部数据，但执行后同名的空表仍然存在，可以继续插入数据。在MySQL中，它在逻辑上接近删除并重新创建该表，因此属于DDL，并且通常不能回滚。

| 命令 | 删除数据 | 删除表结构 | 当前阶段的理解 |
| --- | --- | --- | --- |
| `DROP TABLE 表名;` | 是 | 是 | 整张表都不要了 |
| `TRUNCATE TABLE 表名;` | 是，删除全部数据 | 否 | 保留空表，重新开始使用 |

> [!warning]
> `DROP TABLE`和`TRUNCATE TABLE`都会造成数据丢失，练习时也要先确认当前数据库和表名。

## 7. DML数据操作

### 7.1 INSERT：新增数据

```sql
INSERT INTO employee (name, job, dept_id)
VALUES ('小明', '开发', 1);
```

DML主要负责向表中新增、修改和删除数据。

## 8. DQL数据查询

### 8.1 SELECT：查询数据

```sql
SELECT id, name
FROM employee;
```

`SELECT`是实际开发中使用非常频繁的SQL语句，后续需要重点学习查询条件、排序、分组和多表查询。

## 9. 当前需要掌握什么

我需要掌握SQL语句的分号、缩进、大小写习惯和三种注释写法，能够区分DDL、DML和DQL，并做到：

- 会查询、创建、选择和谨慎删除数据库。
- 会创建和查询数据表。
- 会使用`ALTER TABLE`添加、修改、重命名和删除字段。
- 能区分`DROP TABLE`删除整张表与`TRUNCATE TABLE`清空全部数据。

DCL和TCL现阶段知道用途即可，等学习用户权限和事务时再深入。

返回：[[数据库学习导航]]。
