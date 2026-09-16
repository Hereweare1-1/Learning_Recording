# Windows下载、安装与登录MySQL

这篇笔记是Windows上的MySQL环境准备参考，只在首次下载、安装、重新配置或登录MySQL时查看，不需要反复背诵安装向导。

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

## 3. 登录和退出MySQL

如果MySQL的`bin`目录已经加入系统环境变量`PATH`，可以在PyCharm终端或PowerShell中直接运行：

```bash
mysql -u root -p
```

如果终端提示无法识别`mysql`命令，说明当前没有配置对应的`PATH`，可以改用完整路径：

```bash
& "D:\MySQL\Server8.4\bin\mysql.exe" -u root -p
```

- `&`：让PowerShell运行指定路径中的程序。
- `mysql`或`mysql.exe`：启动MySQL命令行客户端。
- `-u root`：使用root账户登录。
- `-p`：登录时提示输入密码。

看到`Enter password:`后输入安装时设置的密码。输入过程中不显示字符属于正常现象，登录成功后会出现`mysql>`。

需要退出MySQL命令行客户端时运行：

```sql
exit;
```

> [!warning]
> 本机学习阶段可以暂时使用root账户。正式项目应创建权限受限的应用账户，不要把root密码或其他数据库密码直接写入代码、笔记或Git仓库。

完成登录后，回到[[数据库学习导航]]继续学习SQL。

## 4. 官方资料

- [MySQL Community Server 8.4下载](https://dev.mysql.com/downloads/mysql/8.4.html)
- [MySQL Configurator配置说明](https://dev.mysql.com/doc/refman/8.4/en/mysql-configurator-workflow-server.html)
