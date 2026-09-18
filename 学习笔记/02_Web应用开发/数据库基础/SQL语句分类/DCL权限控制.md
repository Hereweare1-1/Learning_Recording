# DCL权限控制

DCL（Data Control Language，数据控制语言）用于管理数据库用户和访问权限。

## 1. 语句汇总

| 作用 | 基本语句 |
| --- | --- |
| 查询用户 | `SELECT User, Host FROM mysql.user;` |
| 创建用户 | `CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';` |
| 修改密码 | `ALTER USER '用户名'@'主机名' IDENTIFIED BY '新密码';` |
| 删除用户 | `DROP USER '用户名'@'主机名';` |
| 查询权限 | `SHOW GRANTS FOR '用户名'@'主机名';` |
| 授予权限 | `GRANT 权限列表 ON 数据库名.表名 TO '用户名'@'主机名';` |
| 撤销权限 | `REVOKE 权限列表 ON 数据库名.表名 FROM '用户名'@'主机名';` |

## 2. 管理用户

MySQL账户由用户名和允许连接的主机共同确定，完整格式为`'用户名'@'主机名'`。

### 2.1 查询用户

MySQL用户信息保存在`mysql`系统数据库的`user`表中：

```sql
USE mysql;

SELECT User, Host
FROM user;
```

这里只查询`User`和`Host`字段，通常不需要查询系统表中的全部字段。

### 2.2 创建用户

```text
CREATE USER [IF NOT EXISTS] '用户名'@'主机名'
IDENTIFIED BY '密码';
```

方括号表示`IF NOT EXISTS`可以省略，实际SQL中不写方括号。

例如，创建一个只能从本机连接的用户：

```sql
CREATE USER 'app_user'@'localhost'
IDENTIFIED BY '替换为安全密码';
```

### 2.3 修改用户密码

```text
ALTER USER '用户名'@'主机名'
IDENTIFIED BY '新密码';
```

例如：

```sql
ALTER USER 'app_user'@'localhost'
IDENTIFIED BY '替换为新的安全密码';
```

> [!warning]
> MySQL 8.4默认禁用了已经弃用的`mysql_native_password`插件，因此不要直接照搬`IDENTIFIED WITH mysql_native_password BY ...`这种旧写法。使用`IDENTIFIED BY`可以采用服务器当前的默认认证方式。

### 2.4 删除用户

```text
DROP USER [IF EXISTS] '用户名'@'主机名';
```

例如：

```sql
DROP USER IF EXISTS 'app_user'@'localhost';
```

### 2.5 主机名

- `'localhost'`：只允许用户从MySQL服务器所在的计算机连接。
- `'%'`：匹配任意主机地址，允许远程连接。

使用`'%'`只表示账户允许从任意主机尝试连接，能否实际连接还会受到MySQL监听地址、防火墙和网络配置等因素影响。应根据实际需要限制来源地址，不要为了方便直接开放给所有主机。

## 3. 权限控制

### 3.1 常见权限

| 权限 | 作用 |
| --- | --- |
| `ALL`或`ALL PRIVILEGES` | 当前授权范围内的全部权限 |
| `SELECT` | 查询数据 |
| `INSERT` | 插入数据 |
| `UPDATE` | 修改数据 |
| `DELETE` | 删除数据 |
| `ALTER` | 修改表结构 |
| `DROP` | 删除数据库、表或视图 |
| `CREATE` | 创建数据库或表 |

### 3.2 查询权限

```text
SHOW GRANTS FOR '用户名'@'主机名';
```

例如：

```sql
SHOW GRANTS FOR 'app_user'@'localhost';
```

查询当前登录用户的权限时，可以直接使用：

```sql
SHOW GRANTS;
```

### 3.3 授予权限

```text
GRANT 权限列表
ON 数据库名.表名
TO '用户名'@'主机名';
```

多个权限之间使用逗号分隔。例如，允许用户查询、新增和修改`company`数据库中的所有表：

```sql
GRANT SELECT, INSERT, UPDATE
ON company.*
TO 'app_user'@'localhost';
```

### 3.4 撤销权限

```text
REVOKE 权限列表
ON 数据库名.表名
FROM '用户名'@'主机名';
```

例如，撤销用户对`company`数据库中所有表的修改权限：

```sql
REVOKE UPDATE
ON company.*
FROM 'app_user'@'localhost';
```

### 3.5 授权范围中的通配符

| 写法 | 授权范围 |
| --- | --- |
| `company.employee` | `company`数据库中的`employee`表 |
| `company.*` | `company`数据库中的所有表 |
| `*.*` | 所有数据库中的所有对象 |

> [!warning]
> 权限应遵循最小权限原则，只授予程序完成工作所需的权限。`ALL PRIVILEGES ON *.*`权限范围很大，不应随意授予普通应用账户。

用户管理和权限分配通常由数据库管理员负责，但开发程序时也需要为应用创建权限受限的账户，不要让应用长期使用`root`账户。

## 4. 相关笔记

- [[SQL基础语法]]
- [[数据库学习导航]]

## 5. 官方资料

- [MySQL 8.4 CREATE USER语句](https://dev.mysql.com/doc/refman/8.4/en/create-user.html)
- [MySQL 8.4修改账户密码](https://dev.mysql.com/doc/refman/8.4/en/assigning-passwords.html)
- [MySQL 8.4 SHOW GRANTS语句](https://dev.mysql.com/doc/refman/8.4/en/show-grants.html)
- [MySQL 8.4 GRANT语句](https://dev.mysql.com/doc/refman/8.4/en/grant.html)
- [MySQL 8.4 REVOKE语句](https://dev.mysql.com/doc/refman/8.4/en/revoke.html)
