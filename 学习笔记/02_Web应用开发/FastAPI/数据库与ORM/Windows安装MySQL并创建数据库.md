# Windows安装MySQL并创建数据库

这篇笔记是数据库环境准备的支线参考，只在首次安装或重新配置MySQL时查看，不需要反复背诵安装向导。

## 1. 下载MySQL

打开[MySQL Community Server 8.4官方下载页](https://dev.mysql.com/downloads/mysql/8.4.html)，选择：

```text
Operating System：Microsoft Windows
安装包：Windows (x86, 64-bit), MSI Installer
```

当前安装包文件名类似：

```text
mysql-8.4.11-winx64.msi
```

版本号以后可能变化，应以官网当前提供的MySQL 8.4 LTS版本为准。如果下载页面要求登录，可以选择“No thanks, just start my download”。

## 2. 安装和配置

运行MSI安装包，需要修改安装目录时选择`Custom`。安装完成后，运行：

```text
D:\MySQL\Server8.4\bin\mysql_configurator.exe
```

配置过程按下面的设置完成即可，不需要记住每一页按钮的位置：

| 配置项 | 设置 |
| --- | --- |
| 程序目录 | `D:\MySQL\Server8.4\` |
| 数据目录 | `D:\MySQL\Data8.4\` |
| Config Type | `Development Computer` |
| TCP/IP | 勾选，端口使用`3306` |
| X Protocol Port | `33060` |
| Named Pipe、Shared Memory | 不勾选 |
| Windows防火墙 | 仅供本机学习时，不开放MySQL端口 |
| root账户 | 设置并记住密码，不需要在笔记中记录 |
| Windows服务 | 服务名`MySQL84`，随系统自动启动，使用标准系统账户 |
| 文件权限 | 使用默认的完整访问权限配置 |
| 示例数据库 | 不创建Sakila和World数据库 |

最后执行配置，确认所有项目完成并出现`Configuration Complete`。

## 3. 登录MySQL

在PyCharm的PowerShell终端中运行：

```bash
& "D:\MySQL\Server8.4\bin\mysql.exe" -u root -p
```

- `&`：让PowerShell运行指定路径中的程序。
- `mysql.exe`：MySQL命令行客户端。
- `-u root`：使用root账户登录。
- `-p`：登录时提示输入密码。

看到`Enter password:`后输入安装时设置的密码。输入过程中不显示字符属于正常现象，登录成功后会出现`mysql>`。

## 4. 创建并检查学习数据库

登录后依次执行：

```sql
SHOW DATABASES;

CREATE DATABASE fastapi_test
CHARACTER SET utf8mb4
COLLATE utf8mb4_unicode_ci;

USE fastapi_test;

SHOW TABLES;

exit;
```

| 命令 | 作用 |
| --- | --- |
| `SHOW DATABASES;` | 查看当前账户能够看到的数据库 |
| `CREATE DATABASE ...;` | 创建名为`fastapi_test`的学习数据库 |
| `USE fastapi_test;` | 选择后续需要操作的数据库 |
| `SHOW TABLES;` | 查看当前数据库中的数据表 |
| `exit;` | 退出MySQL命令行客户端 |

新安装的MySQL通常包含`information_schema`、`mysql`、`performance_schema`和`sys`等系统数据库，不要随意修改或删除。

`utf8mb4`支持中文、英文和Emoji。这里使用的`utf8mb4_unicode_ci`是有效的排序规则；MySQL 8.4默认排序规则是`utf8mb4_0900_ai_ci`，两者不要混淆。

刚创建的`fastapi_test`还没有数据表，因此`SHOW TABLES;`显示`Empty set`是正常现象，后续可以使用SQLAlchemy ORM创建表。

## 5. 与FastAPI ORM学习的关系

```text
安装并启动MySQL Server
        ↓
创建fastapi_test数据库
        ↓
使用aiomysql连接MySQL
        ↓
使用SQLAlchemy定义并创建数据表
```

完成本篇操作后，继续学习[[FastAPI数据库与ORM]]。

## 6. 安全提示

> [!warning]
> 本机学习阶段可以暂时使用root账户。正式项目应创建权限受限的应用账户，不要把root密码或其他数据库密码直接写入代码、笔记或Git仓库。

## 7. 官方资料

- [MySQL Community Server 8.4下载](https://dev.mysql.com/downloads/mysql/8.4.html)
- [MySQL Configurator配置说明](https://dev.mysql.com/doc/refman/8.4/en/mysql-configurator-workflow-server.html)
- [MySQL字符集和排序规则](https://dev.mysql.com/doc/refman/8.4/en/charset.html)

