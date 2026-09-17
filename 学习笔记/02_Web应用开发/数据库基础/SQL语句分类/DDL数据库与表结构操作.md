# DDL数据库与表结构操作

DDL（Data Definition Language，数据定义语言）主要用于定义和修改数据库、数据表及字段等结构。

## 1. 语句汇总

### 1.1 数据库操作

| 作用 | 语句 |
| --- | --- |
| 查看所有数据库 | `SHOW DATABASES;` |
| 查看当前数据库 | `SELECT DATABASE();` |
| 创建数据库 | `CREATE DATABASE 数据库名;` |
| 选择数据库 | `USE 数据库名;` |
| 删除数据库 | `DROP DATABASE 数据库名;` |

### 1.2 数据表操作

| 作用 | 语句 |
| --- | --- |
| 创建数据表 | `CREATE TABLE 表名 (...);` |
| 查看所有表 | `SHOW TABLES;` |
| 查看表结构 | `DESC 表名;` |
| 查看建表语句 | `SHOW CREATE TABLE 表名;` |
| 添加字段 | `ALTER TABLE 表名 ADD 字段名 数据类型;` |
| 修改字段类型 | `ALTER TABLE 表名 MODIFY 字段名 新数据类型;` |
| 修改字段名和类型 | `ALTER TABLE 表名 CHANGE 旧字段名 新字段名 数据类型;` |
| 删除字段 | `ALTER TABLE 表名 DROP COLUMN 字段名;` |
| 修改表名 | `ALTER TABLE 旧表名 RENAME TO 新表名;` |
| 删除整张表 | `DROP TABLE 表名;` |
| 清空表中全部数据 | `TRUNCATE TABLE 表名;` |

## 2. 语法说明

DDL操作的是数据库和数据表的结构，而不是表中的某一条具体数据。

语法格式中的方括号`[]`表示可以省略的可选部分，实际输入SQL时不写方括号。`数据库名`等中文说明是占位内容，需要替换成实际值。

## 3. 数据库操作

### 3.1 SHOW DATABASES：查看所有数据库

```sql
SHOW DATABASES;
```

该命令显示当前账户有权查看的数据库。

新安装的MySQL通常包含`information_schema`、`mysql`、`performance_schema`和`sys`等系统数据库，不要随意修改或删除。

### 3.2 SELECT DATABASE：查看当前数据库

```sql
SELECT DATABASE();
```

如果还没有使用`USE`选择数据库，查询结果通常是`NULL`。

### 3.3 CREATE DATABASE：创建数据库

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

`utf8mb4`支持中文、英文和Emoji。这里使用的`utf8mb4_unicode_ci`是有效的排序规则；MySQL 8.4的默认排序规则是`utf8mb4_0900_ai_ci`，两者不要混淆。

### 3.4 USE：选择数据库

```text
USE 数据库名;
```

例如：

```sql
USE fastapi_test;
```

选择数据库后，后续没有明确写出数据库名的建表和查询操作，会默认在当前数据库中执行。重新连接MySQL后，通常需要再次使用`USE`选择数据库。

### 3.5 DROP DATABASE：删除数据库

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

## 4. 数据表操作

### 4.1 CREATE TABLE：创建数据表

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

这个例子演示表名、字段名、字段类型和注释，不包含主键、非空和默认值等字段约束。

常用的MySQL字段类型见：[[MySQL数据类型]]。

### 4.2 SHOW TABLES：查看所有表

查询当前数据库中的所有表：

```sql
SHOW TABLES;
```

刚创建的数据库还没有数据表时，结果显示`Empty set`属于正常现象。

### 4.3 DESC：查看表结构

```text
DESC 表名;
```

例如：

```sql
DESC employee;
```

`DESC`是`DESCRIBE`的简写，可以查看字段名、字段类型、是否允许为空和键信息等表结构。

### 4.4 SHOW CREATE TABLE：查看建表语句

```text
SHOW CREATE TABLE 表名;
```

例如：

```sql
SHOW CREATE TABLE employee;
```

这个命令可以查看MySQL实际保存的建表语句，包括自动补充的默认配置。

### 4.5 ALTER TABLE ADD：添加字段

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

### 4.6 ALTER TABLE MODIFY：修改字段类型

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

### 4.7 ALTER TABLE CHANGE：修改字段名和类型

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

### 4.8 ALTER TABLE DROP COLUMN：删除字段

```text
ALTER TABLE 表名 DROP COLUMN 字段名;
```

例如：

```sql
ALTER TABLE employee DROP COLUMN age;
```

> [!warning]
> 删除字段会同时删除该字段中的全部数据，执行前需要确认字段名并备份重要数据。

### 4.9 ALTER TABLE RENAME TO：修改表名

```text
ALTER TABLE 旧表名 RENAME TO 新表名;
```

例如：

```sql
ALTER TABLE employee RENAME TO employees;
```

### 4.10 DROP TABLE：删除整张表

```text
DROP TABLE [IF EXISTS] 表名;
```

例如：

```sql
DROP TABLE IF EXISTS employees;
```

`IF EXISTS`可以省略。`DROP TABLE`会同时删除表结构和表中的全部数据。

### 4.11 TRUNCATE TABLE：清空表中全部数据

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

## 5. 相关笔记

- [[SQL基础语法]]
- [[MySQL数据类型]]
- [[数据库学习导航]]

## 6. 官方资料

- [MySQL字符集和排序规则](https://dev.mysql.com/doc/refman/8.4/en/charset.html)
