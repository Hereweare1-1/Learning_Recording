# FastAPI数据库与ORM

这篇笔记整理ORM概念，以及FastAPI通过异步引擎、模型类和数据库会话操作数据库的整体流程。

## 1. ORM是什么

**ORM（Object-Relational Mapping，对象关系映射）**是在面向对象编程语言和关系型数据库之间建立映射的技术。

使用ORM后，可以把Python类和数据库表对应起来，把Python对象和表中的一行数据对应起来，并通过操作对象的方式完成常见数据库操作。

```text
Python类      ↔ 数据库表
Python对象    ↔ 表中的一行数据
对象的属性    ↔ 表中的字段
```

ORM不会让SQL完全消失。遇到复杂查询、性能优化和排查问题时，我仍然需要理解基本SQL。

## 2. ORM的主要作用

- 减少重复编写简单SQL的工作。
- 让数据库操作代码更接近Python的面向对象写法。
- 帮助管理数据库连接、会话和事务，但仍然需要正确配置和提交、回滚事务。
- ORM通常使用参数化查询，可以降低SQL注入风险；如果错误拼接原始SQL，仍然可能出现安全问题。

## 3. 常见ORM工具

这里不进行主观排名，只根据框架关系和我的学习方向选择重点。

| ORM工具 | 特点和适用场景 |
| --- | --- |
| SQLAlchemy ORM | 独立、功能完整，同时支持同步和异步用法，常与FastAPI配合 |
| Django ORM | Django内置的ORM，与Django项目结合紧密 |
| Tortoise ORM | 采用异步风格，API相对直观 |

FastAPI本身没有内置ORM，可以根据项目需求选择数据库工具。SQLAlchemy功能完整并支持异步用法，是FastAPI项目中的常见选择。

## 4. FastAPI操作数据库的整体流程

```text
安装并启动MySQL Server
        ↓
创建项目数据库
        ↓
安装SQLAlchemy和aiomysql
        ↓
配置数据库连接和会话
        ↓
定义模型，建立类与表的映射
        ↓
创建或迁移数据库表
        ↓
通过依赖注入为路由提供数据库会话
        ↓
查询、新增、修改和删除数据
        ↓
提交或回滚事务，并释放会话资源
```

### 4.1 安装工具

```bash
pip install "sqlalchemy[asyncio]" aiomysql
```

- `sqlalchemy[asyncio]`：安装SQLAlchemy及其异步功能所需的依赖，用于定义模型和操作数据库。
- `aiomysql`：MySQL的异步数据库驱动，负责让Python程序通过异步方式连接MySQL。

命令中的引号用于保护`[asyncio]`，避免某些终端把方括号当作特殊字符；它和`pip install sqlalchemy[asyncio] aiomysql`安装的是相同内容。

这里安装的是Python操作数据库所需的工具。如果电脑还没有安装和登录MySQL Server，可以查看：[[Windows下载、安装与登录MySQL]]。

### 4.2 建库和建表

学习数据库`fastapi_test`的创建步骤见：[[DDL数据库与表结构操作#3.3 CREATE DATABASE：创建数据库]]。

使用ORM创建表的顺序是：

```text
创建异步数据库引擎 → 定义模型类 → FastAPI启动时创建表
```

**第一步：创建异步数据库引擎**

```python
import os

from sqlalchemy.ext.asyncio import create_async_engine

DATABASE_URL = os.environ["DATABASE_URL"]

async_engine = create_async_engine(
    DATABASE_URL,
    echo=True,
    pool_size=10,
    max_overflow=20,
)
```

数据库连接地址的格式如下：

```text
mysql+aiomysql://用户名:密码@localhost:3306/fastapi_test?charset=utf8mb4
```

- `mysql+aiomysql`：使用MySQL和异步驱动`aiomysql`。
- `localhost:3306`：MySQL运行在本机的`3306`端口。
- `fastapi_test`：需要连接的数据库。
- `echo=True`：开发时在终端输出SQL日志，方便观察和排错。
- `pool_size`：连接池中长期保留的连接数量。
- `max_overflow`：连接池繁忙时允许临时增加的连接数量。

小型项目可以省略`pool_size`和`max_overflow`并使用默认值。数据库地址通过环境变量读取，避免把真实密码直接写进代码和Git仓库。

**第二步：定义模型类**

```python
from datetime import datetime

from sqlalchemy import DateTime, String, func
from sqlalchemy.orm import DeclarativeBase, Mapped, mapped_column


class Base(DeclarativeBase):
    pass


class TimeMixin:
    create_time: Mapped[datetime] = mapped_column(
        DateTime,
        server_default=func.now(),
    )
    update_time: Mapped[datetime] = mapped_column(
        DateTime,
        server_default=func.now(),
        onupdate=func.now(),
    )


class Book(TimeMixin, Base):
    __tablename__ = "book"

    id: Mapped[int] = mapped_column(primary_key=True)
    bookname: Mapped[str] = mapped_column(String(255))
    author: Mapped[str] = mapped_column(String(255))
```

- `Base`：所有ORM模型类共同继承的基类。
- `TimeMixin`：集中定义多个表可以复用的创建时间和修改时间字段。
- `Book`：对应数据库中的`book`表。
- `Mapped[...]`：使用Python类型注解描述字段在Python中的类型。
- `mapped_column()`：设置字段长度、主键和默认值等数据库规则。
- `__tablename__`：指定模型对应的数据表名称。

**第三步：应用启动时创建表**

```python
from contextlib import asynccontextmanager

from fastapi import FastAPI


async def create_tables():
    async with async_engine.begin() as conn:
        await conn.run_sync(Base.metadata.create_all)


@asynccontextmanager
async def lifespan(app: FastAPI):
    await create_tables()
    yield
    await async_engine.dispose()


app = FastAPI(lifespan=lifespan)
```

- `async_engine.begin()`：从连接池取得异步连接并开启事务。
- `Base.metadata.create_all`：根据所有继承`Base`的模型创建尚不存在的数据表。
- `run_sync()`：让同步形式的`create_all()`通过异步连接执行。
- `lifespan`中`yield`之前的代码在应用启动时执行，之后的代码在应用关闭时执行。
- `async_engine.dispose()`：应用关闭时释放连接池资源。

`create_all()`适合学习阶段首次建表，但不会自动修改已经存在的表结构。正式项目通常使用数据库迁移工具管理表结构变化，这部分后续再学习。

图片中的`@app.on_event("startup")`属于旧的事件写法。当前FastAPI推荐使用`lifespan`统一处理启动和关闭逻辑。

### 4.3 操作数据

数据库最常见的四类操作是CRUD：

| 英文 | 中文 | 作用 |
| --- | --- | --- |
| Create | 新增 | 添加数据 |
| Read | 查询 | 读取数据 |
| Update | 修改 | 更新数据 |
| Delete | 删除 | 移除数据 |

## 5. ORM与依赖注入的关系

数据库会话需要在请求开始时创建，并在使用结束后释放。FastAPI通常使用依赖注入把数据库会话提供给需要访问数据库的路由函数。

依赖注入的基础内容见：[[FastAPI依赖注入]]。

## 6. 官方资料

- [SQLAlchemy ORM快速开始](https://docs.sqlalchemy.org/en/20/orm/quickstart.html)
- [SQLAlchemy声明式模型与数据表](https://docs.sqlalchemy.org/en/20/orm/declarative_tables.html)
- [SQLAlchemy asyncio支持](https://docs.sqlalchemy.org/en/20/orm/extensions/asyncio.html)
- [FastAPI Lifespan事件](https://fastapi.tiangolo.com/advanced/events/)
- [Django模型与数据库](https://docs.djangoproject.com/en/5.2/topics/db/)
- [Tortoise ORM入门](https://tortoise.github.io/getting_started.html)
