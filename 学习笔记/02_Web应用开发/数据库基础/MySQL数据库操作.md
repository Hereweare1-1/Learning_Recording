# MySQL数据库操作

## 1. 如何阅读SQL语法格式

SQL资料通常使用下面的方式表示完整语法：

```text
CREATE DATABASE [IF NOT EXISTS] 数据库名
    [DEFAULT CHARACTER SET 字符集]
    [COLLATE 排序规则];
```

其中：

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

## 2. 查询数据库

### 2.1 查看所有可见数据库

```sql
SHOW DATABASES;
```

该命令显示当前账户有权查看的数据库，不一定能看到MySQL服务器上的所有数据库。

### 2.2 查看当前使用的数据库

```sql
SELECT DATABASE();
```

如果还没有使用`USE`选择数据库，查询结果通常是`NULL`。

## 3. 创建数据库

通用写法：

```text
CREATE DATABASE [IF NOT EXISTS] 数据库名
    [DEFAULT CHARACTER SET 字符集]
    [COLLATE 排序规则];
```

- `IF NOT EXISTS`：数据库不存在时才创建，避免数据库已经存在时报错。
- `DEFAULT CHARACTER SET`：指定数据库默认字符集。
- `COLLATE`：指定默认排序和比较规则。

学习示例：

```sql
CREATE DATABASE IF NOT EXISTS fastapi_test
DEFAULT CHARACTER SET utf8mb4
COLLATE utf8mb4_unicode_ci;
```

## 4. 选择数据库

```text
USE 数据库名;
```

例如：

```sql
USE fastapi_test;
```

选择数据库后，后续没有明确写出数据库名的建表和查询操作，会默认在当前数据库中执行。重新连接MySQL后，通常需要再次使用`USE`选择数据库。

## 5. 删除数据库

通用写法：

```text
DROP DATABASE [IF EXISTS] 数据库名;
```

其中`IF EXISTS`可以省略。它表示数据库存在时才删除，避免数据库不存在时报错。

```sql
DROP DATABASE IF EXISTS demo;
```

> [!warning]
> `DROP DATABASE`会删除整个数据库及其中的数据表和数据。执行前必须确认数据库名称，并确保重要数据已经备份。

## 6. 这些命令属于哪一类

- `CREATE DATABASE`、`DROP DATABASE`用于定义或删除数据库对象，属于DDL。
- `SHOW DATABASES`、`SELECT DATABASE()`用于查看信息。
- `USE`用于切换当前数据库。

因此，“数据库操作”中会同时出现DDL和辅助管理命令，不需要为了分类而强行把所有命令都算作DDL。

## 7. 当前需要掌握什么

我需要熟练使用`SHOW DATABASES`、`SELECT DATABASE()`、`CREATE DATABASE`和`USE`。`DROP DATABASE`需要知道写法，但实际操作时必须谨慎。

返回：[[数据库学习导航]]。
