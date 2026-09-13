# FastAPI基础

## 1. 什么是FastAPI

FastAPI是一个现代、快速、高性能的Python Web框架，可以用于开发服务端API接口。

建议先了解[[Web网络基础]]和[[RESTful API设计]]。

## 2. 使用FastAPI开发服务端接口

主要步骤：

- 导入FastAPI。
- 创建FastAPI实例对象。
- 创建路径操作函数，定义访问路径。
- 运行FastAPI服务。

```python
from fastapi import FastAPI

# 创建FastAPI实例
app = FastAPI()


# 定义API接口
@app.get("/")
def root():
    return {"message": "Hello World"}


# 定义API接口
@app.get("/users")
def get_users():
    return [
        {"id": 1, "name": "张三"},
        {"id": 2, "name": "李四"},
        {"id": 3, "name": "王五"},
    ]


# 启动服务
if __name__ == "__main__":
    import uvicorn

    uvicorn.run(app, host="0.0.0.0", port=8000)
```

### 2.1 FastAPI路由

**路由**用于规定：收到某种请求时，应该交给哪个函数处理。

```python
@app.get("/")
def root():
    return {"message": "Hello World"}
```

| 代码 | 含义 |
| --- | --- |
| `app` | 前面创建的FastAPI实例 |
| `get` | 接收HTTP的`GET`请求 |
| `"/"` | 请求路径，表示网站根路径 |
| `@app.get("/")` | 路径操作装饰器，把下面的函数登记为该路由的处理函数 |
| `root()` | 路径操作函数，请求匹配时由FastAPI调用 |
| `return {...}` | 返回响应数据，Python字典会被转换为JSON响应 |

执行过程：

```text
客户端发送GET /
       ↓
FastAPI匹配@app.get("/")
       ↓
调用root()函数
       ↓
把返回值转换为HTTP响应
```

路径操作函数既可以使用普通的`def`，也可以使用`async def`。异步写法已经整理在[[#4.1 异步支持应该怎样理解]]中，不在这里重复。

`@app.get()`使用了Python装饰器语法，装饰器基础可以查看[[闭包与装饰器#六、FastAPI中的装饰器]]。

### 2.2 FastAPI请求参数

参数是客户端发送请求时携带的额外信息。同一个接口可以根据不同的参数，查询或处理不同的数据。

例如，同样访问图书接口，参数不同，返回的图书也不同：

```text
GET /books/1 → 返回编号为1的图书
GET /books/2 → 返回编号为2的图书
```

FastAPI中常见的参数分为三类：

| 参数类型 | 常见位置 | 主要作用 |
| --- | --- | --- |
| 路径参数 | URL路径中，例如`/books/2` | 指向某个具体资源 |
| 查询参数 | URL的`?`后面 | 查询、筛选、排序或分页 |
| 请求体 | HTTP请求的Body中 | 提交结构化数据，通常是JSON |

#### 路径参数

路径参数是URL路径的一部分，使用`{参数名}`声明：

```python
@app.get("/books/{book_id}")
async def get_book(book_id: int):
    return {
        "id": book_id,
        "title": f"这是第{book_id}本书",
    }
```

访问：

```text
http://127.0.0.1:8000/books/2
```

FastAPI会把路径中的`2`传给`book_id`，然后得到下面的响应：

```json
{
  "id": 2,
  "title": "这是第2本书"
}
```

这里的`book_id: int`已经可以完成两件事：

- 把URL中的参数转换为整数。
- 检查参数能否转换为整数，无法转换时自动返回参数校验错误。

路由中的参数名和函数中的参数名必须对应：

```python
@app.get("/books/{book_id}")
async def get_book(book_id: int):
    ...
```

##### 使用Path增加校验规则

如果只需要类型转换，写`book_id: int`就够了。需要增加数值范围、描述等规则时，可以使用`Path()`。

```python
from typing import Annotated

from fastapi import FastAPI, Path

app = FastAPI()


@app.get("/books/{book_id}")
async def get_book(
    book_id: Annotated[int, Path(gt=0, description="图书ID")],
):
    return {"id": book_id}
```

这个例子规定`book_id`必须是大于`0`的整数。

| `Path()`参数 | 含义 | 适合的数据 |
| --- | --- | --- |
| `gt` | 大于 | 数字 |
| `ge` | 大于或等于 | 数字 |
| `lt` | 小于 | 数字 |
| `le` | 小于或等于 | 数字 |
| `min_length` | 最小长度 | 字符串 |
| `max_length` | 最大长度 | 字符串 |
| `description` | 参数说明 | 各种类型 |

> [!tip] 现阶段怎样写
> 路径参数始终是必填的。我优先使用官方推荐的`Annotated[int, Path(...)]`写法；在旧代码中也可能看到`book_id: int = Path(...)`。

#### 查询参数

查询参数位于URL的`?`后面，多个参数使用`&`连接，常用于筛选、排序和分页。

```python
@app.get("/books")
async def get_books(keyword: str | None = None, page: int = 1):
    return {
        "keyword": keyword,
        "page": page,
    }
```

访问：

```text
http://127.0.0.1:8000/books?keyword=Python&page=2
```

- `keyword`的默认值是`None`，因此可以不传。
- `page`的默认值是`1`，不传时会自动使用`1`。
- 没有写在路由`/books`中的普通类型参数，会被FastAPI识别为查询参数。

#### 请求体

请求体用于携带要提交给服务器的数据，通常使用JSON格式。FastAPI一般使用Pydantic模型描述请求体的数据结构。

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class BookCreate(BaseModel):
    title: str
    price: float


@app.post("/books")
async def create_book(book: BookCreate):
    return book
```

客户端可以发送下面的JSON请求体：

```json
{
  "title": "Python入门",
  "price": 39.9
}
```

FastAPI会自动读取JSON、转换数据类型并进行校验。请求体常用于`POST`、`PUT`和`PATCH`请求；虽然FastAPI支持为`GET`请求声明请求体，但这种做法不推荐。

#### FastAPI怎样判断参数来自哪里

```text
参数名出现在路由的{ }中
        ↓
路径参数

参数是int、str、float、bool等普通类型，且不在路由中
        ↓
查询参数

参数类型是Pydantic模型
        ↓
请求体
```

> [!summary] 简单记忆
> 路径参数负责“找到谁”，查询参数负责“怎么筛选”，请求体负责“提交什么数据”。

## 3. 运行FastAPI服务

方式一：

```bash
fastapi dev xxxx.py
```

方式二：

```bash
uvicorn xxxx:app --reload
```

方式三：在代码中启动

```python
if __name__ == "__main__":
    import uvicorn

    uvicorn.run(app, host="0.0.0.0", port=8000)
```

## 4. FastAPI、Flask和Django的区别

这三个都是Python Web框架，但它们的设计重点不同，不能只用“谁快、谁慢”来判断哪个更好。

| 对比维度 | FastAPI | Flask | Django |
| --- | --- | --- | --- |
| 主要定位 | 以API开发为核心的现代Web框架 | 轻量、灵活的Web框架 | 功能完整的Web框架 |
| 异步支持 | 基于ASGI设计，支持`async def`和`await` | 支持异步视图，但整体仍以WSGI设计为主 | 支持异步视图，使用ASGI时可以运行异步请求栈 |
| 数据校验 | 结合Python类型注解和Pydantic自动校验请求数据 | 核心框架通常需要手动处理或使用扩展 | 表单和模型有校验能力；开发API时通常还会配合其他工具 |
| API文档 | 根据OpenAPI自动生成Swagger UI和ReDoc文档 | 核心框架不自动生成，需要自行实现或使用扩展 | 核心框架不直接提供完整的REST API文档，通常需要配合其他工具 |
| 内置功能 | 偏向API所需功能 | 核心较小，按需选择扩展 | 内置ORM、后台管理、认证等功能 |
| 常见场景 | API、微服务、AI模型服务 | 小型Web项目、简单API、需要灵活组合的项目 | 数据库驱动的网站、内容系统和管理后台 |

### 4.1 异步支持应该怎样理解

异步适合需要等待数据库、外部API或文件等I/O操作的场景，但使用`async`并不代表程序一定更快。

- **FastAPI**：属于异步优先的ASGI框架，也可以混合使用普通的`def`和异步的`async def`。
- **Flask**：可以编写`async def`视图，但一次请求仍会占用一个工作进程，因此不是异步优先框架。
- **Django**：原生支持异步视图；在ASGI环境下可以使用异步请求栈，但部分代码或第三方组件仍可能是同步的。

FastAPI中的异步接口示例：

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/items")
async def get_items():
    return {"items": []}
```

### 4.2 性能应该怎样比较

框架性能会受到业务代码、数据库、网络请求、部署方式和服务器配置等因素影响，不能简单地记成“FastAPI一定最快、Django一定最慢”。

我现阶段只需要记住：

- FastAPI针对API开发和并发I/O场景进行了良好设计。
- Flask轻量，但很多功能需要我自己选择和组合。
- Django内置功能多，适合需要ORM、认证和后台管理的完整网站。

### 4.3 我现阶段为什么优先学习FastAPI

我的求职方向是智能应用开发，经常需要把大模型、算法或业务能力封装成API，因此FastAPI与当前学习目标比较匹配。

```text
AI模型或业务功能
       ↓
使用FastAPI封装成API
       ↓
前端、智能体或其他程序调用
```

这并不表示Flask和Django不好，而是三个框架适合解决的问题不同。现阶段我应先掌握FastAPI，之后再根据项目需要学习其他框架。

### 4.4 官方资料

- [FastAPI官方文档](https://fastapi.tiangolo.com/)
- [FastAPI异步说明](https://fastapi.tiangolo.com/async/)
- [Flask异步说明](https://flask.palletsprojects.com/en/stable/async-await/)
- [Django异步说明](https://docs.djangoproject.com/en/stable/topics/async/)
- [FastAPI路径参数与数值校验](https://fastapi.tiangolo.com/tutorial/path-params-numeric-validations/)
- [FastAPI查询参数](https://fastapi.tiangolo.com/tutorial/query-params/)
- [FastAPI请求体](https://fastapi.tiangolo.com/tutorial/body/)
