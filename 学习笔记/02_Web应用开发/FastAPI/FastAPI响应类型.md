# FastAPI响应类型

FastAPI接口处理完请求后，需要把结果返回给客户端。现阶段我重点掌握三种常用响应：**JSON、HTML和文件**。

| 响应内容 | 常用方式 | 适合场景 |
| --- | --- | --- |
| JSON | 直接返回字典、列表或Pydantic模型 | 普通API接口 |
| HTML | `HTMLResponse` | 返回一段HTML网页内容 |
| 文件 | `FileResponse` | 下载图片、文档等文件 |

## 1. JSON响应

JSON是FastAPI默认使用的响应格式。路径操作函数返回字典、列表或Pydantic模型时，FastAPI会自动把Python对象转换成JSON响应，一般不需要手动创建`JSONResponse`。

转换过程中会用到`jsonable_encoder`。现阶段可以把它简单理解为“把Python对象整理成能够写入JSON的数据”的转换工具，不需要手动调用它。

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/books")
async def get_books():
    return {
        "name": "Python入门",
        "price": 39.9,
    }
```

客户端收到的JSON数据：

```json
{
  "name": "Python入门",
  "price": 39.9
}
```

简单理解：我返回Python对象，FastAPI负责把它转换为JSON并发送给客户端。

## 2. HTML响应

返回HTML内容时，需要从`fastapi.responses`中导入`HTMLResponse`。

### 2.1 在装饰器中指定响应类

当接口固定返回HTML时，可以在装饰器中设置`response_class=HTMLResponse`：

```python
from fastapi import FastAPI
from fastapi.responses import HTMLResponse

app = FastAPI()


@app.get("/html", response_class=HTMLResponse)
async def get_html():
    return "<h1>这是一级标题</h1>"
```

FastAPI会把返回的字符串作为HTML内容，并把响应的`Content-Type`设置为`text/html`。

### 2.2 直接返回HTMLResponse对象

也可以在函数中创建并返回一个`HTMLResponse`对象：

```python
from fastapi import FastAPI
from fastapi.responses import HTMLResponse

app = FastAPI()


@app.get("/html")
async def get_html():
    html_content = "<h2>这是二级标题</h2>"
    return HTMLResponse(content=html_content)
```

两种写法都能返回HTML。现阶段可以优先使用第一种，因为装饰器直接说明了该接口的响应类型，看起来更直观。

## 3. 文件响应

返回图片、PDF或其他文件时，可以使用`FileResponse`。

```python
from pathlib import Path

from fastapi import FastAPI
from fastapi.responses import FileResponse

app = FastAPI()


@app.get("/file")
async def get_file():
    file_path = Path("files") / "example.pdf"
    return FileResponse(path=file_path, filename="example.pdf")
```

这个例子中：

- `path`表示服务器中要返回的文件路径。
- `filename`表示客户端下载时看到的文件名。
- 文件必须真实存在，否则接口会报错。
- 项目中优先使用相对路径，避免把某台电脑独有的绝对路径写死在代码中。

## 4. 其他响应类型（了解）

除了JSON、HTML和文件响应，FastAPI还提供下面这些响应类型。现阶段知道它们是做什么的即可，不需要展开学习。

| 响应类型 | 作用 |
| --- | --- |
| `PlainTextResponse` | 返回不包含HTML标签的纯文本 |
| `StreamingResponse` | 分批、持续地返回数据，适合较大的内容或实时数据 |
| `RedirectResponse` | 告诉客户端跳转到另一个URL |

## 5. 两种响应类型设置方式

### 5.1 在装饰器中设置response_class

当一个接口的响应类型固定时，可以在装饰器中设置`response_class`，然后让函数返回普通内容。

```python
@app.get("/html", response_class=HTMLResponse)
async def get_html():
    return "<h1>这是标题</h1>"
```

这种方式的优点是：只看装饰器就能知道接口返回HTML，而且FastAPI可以在接口文档中记录正确的响应类型。

### 5.2 在函数中直接返回响应对象

当函数需要提供文件路径、下载文件名、状态码或响应头等具体信息时，可以直接创建并返回响应对象。

```python
@app.get("/file")
async def get_file():
    return FileResponse(path="files/example.pdf", filename="example.pdf")
```

这时返回的`FileResponse`已经是一个完整响应，FastAPI会直接把它发送给客户端。仅直接返回响应对象时，自动生成的接口文档不一定能知道实际的响应类型。

### 5.3 应该怎样选择

| 情况 | 适合的方式 |
| --- | --- |
| 接口固定返回HTML等某种类型 | 在装饰器中设置`response_class` |
| 需要传入文件路径、下载名称等具体参数 | 直接返回`FileResponse`等响应对象 |
| 需要动态设置状态码或响应头 | 直接返回响应对象 |
| 既想让接口文档记录响应类型，又要自己构造响应对象 | 设置`response_class`，同时返回响应对象 |

> [!summary] 简单记忆
> 类型固定、内容简单时，在装饰器中声明；需要自己控制具体响应细节时，直接返回响应对象。

现阶段我只需要重点掌握：普通API直接返回JSON数据，网页内容使用`HTMLResponse`，文件下载使用`FileResponse`。

## 6. 官方资料

- [FastAPI自定义响应](https://fastapi.tiangolo.com/advanced/custom-response/)
