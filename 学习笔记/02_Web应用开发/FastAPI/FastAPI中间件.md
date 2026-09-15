# FastAPI中间件

## 1. 中间件是什么

**中间件（Middleware）**是在请求到达路由函数之前，以及响应返回客户端之前，统一执行一段处理逻辑的功能。

可以把它理解为所有请求和响应都要经过的“公共检查站”。适合处理很多接口都需要执行的公共操作，例如：

- 记录请求日志。
- 进行身份认证。
- 处理跨域请求（CORS）。
- 给响应添加统一的响应头。
- 统计接口处理时间。

## 2. 中间件的执行流程

```text
客户端发送请求
      ↓
执行中间件中call_next之前的代码
      ↓
call_next(request)把请求交给对应的路由函数
      ↓
路由函数生成响应
      ↓
执行中间件中call_next之后的代码
      ↓
把响应返回客户端
```

## 3. 定义HTTP中间件

在函数上方使用`@app.middleware("http")`装饰器，可以定义处理HTTP请求的中间件。

```python
from fastapi import FastAPI, Request

app = FastAPI()


@app.middleware("http")
async def log_middleware(request: Request, call_next):
    print("中间件开始处理")

    response = await call_next(request)

    print("中间件处理完成")
    return response
```

需要先理解三个部分：

- `request`：客户端发送过来的请求对象。
- `call_next(request)`：把请求继续交给对应的路由函数处理，并得到路由函数生成的响应。
- `response`：准备返回给客户端的响应对象，最后必须使用`return response`返回。

> [!tip] 简单记忆
> `call_next()`之前处理请求，`call_next()`之后处理响应。

## 4. 现阶段需要掌握什么

我需要理解中间件会统一处理请求和响应，能够看懂并写出上面的基本结构。多个中间件的执行顺序、自定义ASGI中间件等高级内容，现阶段了解即可，不需要展开。

## 5. 官方资料

- [FastAPI中间件](https://fastapi.tiangolo.com/tutorial/middleware/)

