# FastAPI请求参数

参数是客户端发送请求时携带的额外信息。同一个接口可以根据不同的参数，查询或处理不同的数据。

例如，同样访问图书接口，参数不同，返回的图书也不同：

```text
GET /books/1 → 返回编号为1的图书
GET /books/2 → 返回编号为2的图书
```

## 1. 参数分类

FastAPI中常见的参数分为三类：

| 参数类型 | 常见位置 | 主要作用 |
| --- | --- | --- |
| 路径参数 | URL路径中，例如`/books/2` | 指向某个具体资源 |
| 查询参数 | URL的`?`后面 | 查询、筛选、排序或分页 |
| 请求体 | HTTP请求的Body中 | 提交结构化数据，通常是JSON |

### 1.1 Python类型注解和Pydantic怎样完成自动校验

Python类型注解负责说明数据应该是什么类型，例如`age: int`表示`age`应该是整数。Pydantic模型可以把多个带有类型注解的字段组织成一个完整的数据结构。

FastAPI收到请求后，会自动读取数据、尝试转换类型并进行校验：

```text
客户端发送请求数据
        ↓
FastAPI读取数据
        ↓
根据Python类型注解和Pydantic模型转换、校验
        ↓
校验成功：调用接口函数
校验失败：不调用接口函数，直接返回校验错误
```

以[[#4. 请求体参数|请求体参数]]中的`BookCreate`模型为例，下面的数据符合要求：

```json
{
  "title": "Python入门",
  "price": 39.9
}
```

如果`price`无法转换成数字，例如：

```json
{
  "title": "Python入门",
  "price": "不是数字"
}
```

FastAPI会自动返回校验错误，不需要我自己编写大量`if`语句检查每个字段。

## 2. 路径参数

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

### 2.1 使用Path增加校验规则

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

## 3. 查询参数

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

### 3.1 使用Query增加校验规则

普通类型注解已经可以声明查询参数。只有需要增加范围、长度、描述等规则时，才需要使用`Query()`。

```python
from typing import Annotated

from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/books")
async def get_books(
    keyword: Annotated[str | None, Query(max_length=50)] = None,
    skip: Annotated[int, Query(ge=0)] = 0,
    limit: Annotated[int, Query(gt=0, le=100)] = 10,
):
    return {
        "keyword": keyword,
        "skip": skip,
        "limit": limit,
    }
```

在这个例子中：

- `keyword`最多包含`50`个字符，可以不传。
- `skip`必须大于或等于`0`，默认值是`0`。
- `limit`必须大于`0`且小于或等于`100`，默认值是`10`。

```text
http://127.0.0.1:8000/books?keyword=Python&skip=0&limit=10
```

查询参数是否必填主要由默认值决定：

| 写法 | 是否必填 |
| --- | --- |
| `user_id: int` | 必填，没有默认值 |
| `page: int = 1` | 非必填，不传时使用`1` |
| `keyword: str | None = None` | 非必填，不传时使用`None` |

> [!tip] 现阶段怎样写
> 简单查询参数直接使用Python类型注解；需要额外校验时，优先使用官方推荐的`Annotated[类型, Query(...)]`写法。

## 4. 请求体参数

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

### 4.1 使用Field增加模型字段校验

模型中的`title: str`和`price: float`是Python类型注解，负责说明字段的数据类型。需要增加长度、数值范围或描述等规则时，可以使用Pydantic提供的`Field()`。

```python
from pydantic import BaseModel, Field


class BookCreate(BaseModel):
    title: str = Field(min_length=1, max_length=50, description="图书名称")
    price: float = Field(gt=0, description="图书价格")
```

这个例子规定：

- `title`必须是字符串，长度为`1～50`个字符。
- `price`必须是浮点数，并且大于`0`。
- `description`会成为自动生成的接口文档中的字段说明。

常用规则：

| `Field()`参数               | 含义                  |
| ------------------------- | ------------------- |
| `...`                     | 表示必填，例如`Field(...)` |
| `default`                 | 设置默认值               |
| `gt`、`ge`                 | 大于、大于或等于            |
| `lt`、`le`                 | 小于、小于或等于            |
| `min_length`、`max_length` | 限制字符串长度             |
| `description`             | 添加字段说明              |

> [!important] 注意来源
> `Path()`和`Query()`来自FastAPI，`Field()`来自Pydantic。

## 5. FastAPI怎样判断参数来自哪里

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

## 6. 三种请求参数的注解总结

### 6.1 Python类型注解与框架工具的分工

```text
Python类型注解：说明数据是什么类型
        +
Path、Query、Field：补充数据来源、校验规则和说明
```

例如，`book_id: int`中的`int`是Python类型注解；`Path(gt=0)`则进一步规定它来自路径，并且必须大于`0`。

`Annotated`来自Python标准库中的`typing`模块，用于把类型和额外规则组合在一起，它本身也不是FastAPI提供的。

### 6.2 三种参数对照

| 参数类型 | 只使用Python类型注解                         | 增加校验规则                                                         | 工具来源              |
| ---- | ------------------------------------- | -------------------------------------------------------------- | ----------------- |
| 路径参数 | `book_id: int`                        | `book_id: Annotated[int, Path(gt=0)]`                          | `Path`来自FastAPI   |
| 查询参数 | `keyword: str \| None = None`         | `keyword: Annotated[str \| None, Query(max_length=50)] = None` | `Query`来自FastAPI  |
| 请求体  | `book: BookCreate`，模型字段使用`title: str` | 模型字段使用`title: str = Field(min_length=1)`                       | `Field`来自Pydantic |

> [!summary] 我的记忆方法
> 先用Python类型注解写清楚“它是什么”，再按需要使用`Path()`、`Query()`或`Field()`补充“它从哪里来、有什么限制”。

## 7. 官方资料

- [FastAPI路径参数与数值校验](https://fastapi.tiangolo.com/tutorial/path-params-numeric-validations/)
- [FastAPI查询参数](https://fastapi.tiangolo.com/tutorial/query-params/)
- [FastAPI请求体](https://fastapi.tiangolo.com/tutorial/body/)
- [FastAPI请求体字段](https://fastapi.tiangolo.com/tutorial/body-fields/)
